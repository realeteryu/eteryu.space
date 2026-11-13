+++
title = "Świadomy kompromis: Dynamic Code Loading vs Piaskownica GrapheneOS"
date = 2026-06-21
description = "Analiza inżynierska kompromisu z GrapheneOS: jak bezpiecznie zezwolić aplikacjom Ente Photos i Filen na Dynamic Code Loading (DCL), łącząc architekturę Zero-Knowledge z modelem zagrożeń STRIDE."

[extra]
toc = true
+++

Czy aplikacja szanująca prywatność może wymagać od nas wyłączenia kluczowych zabezpieczeń systemu? W dzisiejszym wpisie bierzemy na warsztat inżynieryjny kompromis z GrapheneOS: jak bezpiecznie zezwolić aplikacjom Ente Photos i Filen na korzystanie z Dynamic Code Loading (DCL), łącząc architekturę Zero-Knowledge z rygorystycznym modelem zagrożeń STRIDE.

Oto szczegółowa analiza techniczna tego kompromisu, przeprowadzona za pomocą frameworku modelowania zagrożeń STRIDE.

## Czym jest Dynamic Code Loading (DCL) i dlaczego system go nienawidzi?

W klasycznym modelu Androida aplikacja instalowana jest jako kompletny pakiet APK. System operacyjny podczas instalacji zna strukturę kodu i może narzucić mu odpowiednie ramy. **Dynamic Code Loading (DCL)** to mechanizm, w którym aplikacja już po uruchomieniu pobiera i wykonuje dodatkowy kod (pliki `.dex` lub natywne biblioteki współdzielone `.so`), który nie był spakowany w pierwotnym instalatorze.

Architektonicznie DCL dzieli się na dwa podstawowe typy:

1. **DCL via Storage:** Kod jest pobierany, zapisywany w pamięci masowej urządzenia, a następnie stamtąd mapowany i uruchamiany.
2. **DCL via Memory:** Kod trafia do pamięci RAM jako surowy strumień bajtów bezpośrednio z sieci i tam jest wykonywany, całkowicie pomijając zapis plików na dysku.

### Dlaczego GrapheneOS domyślnie to blokuje?

W standardowym Androidzie DCL to gigantyczny wektor ataku dla bezplikowego złośliwego oprogramowania (*fileless malware*). Z tego powodu GrapheneOS stosuje bezwzględną politykę jądra i SELinux, blokując operacje typu `execmem` (zasada *Write XOR Execute* – obszar pamięci nie może być jednocześnie zapisywalny i wykonywalny).

W naszym przypadku aplikacje potrzebują tego mechanizmu do legalnych celów:

- **Ente Photos** wykorzystuje **DCL via Memory** do inicjalizacji zaawansowanych modeli lokalnego uczenia maszynowego (np. rozpoznawanie twarzy w galerii bezpośrednio na urządzeniu).
- **Filen** wykorzystuje **hybrydę DCL via Memory oraz Storage** do obsługi swojego natywnego, wysoce wydajnego silnika kryptograficznego.

Bez ręcznego przyznania uprawnienia DCL w ustawieniach systemu, obie aplikacje natychmiast wykrzywią się pod barierą SELinux i odmówią współpracy.

## Modelowanie zagrożeń (Framework STRIDE)

Zgoda na DCL to otwarcie furtki. Aby ocenić ryzyko, musimy zmapować potencjalne zagrożenia za pomocą metodologii STRIDE:

- **Spoofing (Podszywanie się):** Ryzyko, że serwer dewelopera zostanie przejęty lub ruch sieciowy zostanie przekierowany (ataki DNS Spoofing / MitM), w wyniku czego aplikacja pobierze złośliwy kod udający legalny moduł.
- **Tampering (Manipulacja danymi):** Ryzyko modyfikacji dociąganego kodu w locie (RAM) lub w lokalnych katalogach zapisu na dysku (Storage) przez inny złośliwy proces. Zagrożenie to dotyczy szczególnie aplikacji Filen.
- **Information Disclosure (Wyciek danych):** Jeśli dynamicznie załadowany kod okazałby się złośliwy (np. w wyniku ataku na łańcuch dostaw dewelopera), mógłby podjąć próbę eksfiltracji kluczy szyfrujących lub bazy danych aplikacji.

## Wielowarstwowa architektura zabezpieczeń (Środki zaradcze)

Zgodnie z zasadą *Defense-in-Depth* (obrona w głąb), ryzyko związane z przyznaniem DCL zostało zredukowane do poziomu akceptowalnego (*Residual Risk*) za pomocą trzech niezależnych warstw ochronnych.

### 1. Zabezpieczenie łańcucha dostaw (Supply Chain Security)

Tradycyjne sklepy z aplikacjami zostały całkowicie pominięte. Do instalacji i aktualizacji służy menedżer `Obtainium`, pobierający pakiety bezpośrednio z sekcji *Releases* oficjalnych, otwartych repozytoriów GitHub deweloperów.

Integralność pobranego APK jest weryfikowana w tle przez narzędzie `AppVerifier BG`. Agreguje ono zewnętrzną bazę sygnatur kryptograficznych `Verified Apps` (od Privacy Guides) oraz lokalną bazę zaufanych certyfikatów, budowaną ręcznie po uprzednim, manualnym sprawdzeniu klucza dewelopera. Taka potrójna walidacja całkowicie eliminuje ryzyko ataku typu *Spoofing* lub *Tampering* na etapie wdrażania kodu.

### 2. Piaskownica pamięci masowej (Storage Scopes & SAF)

Nawet jeśli załadowany dynamicznie kod okazałby się złośliwy, system operacyjny nie pozwala mu na swobodne skanowanie urządzenia:

- **Ente Photos** operuje w ramach `Storage Scopes` – system emuluje dla niej puste środowisko, dając jej wgląd wyłącznie w dwa jawnie wskazane katalogi (`DCIM/Camera` oraz `Pictures/Screenshots`).
- **Filen** idzie krok dalej i wykorzystuje systemowy mechanizm `Storage Access Framework (SAF)`. Aplikacja ma zerowy, stały dostęp do pamięci urządzenia. Wybór plików do synchronizacji odbywa się przez systemowy komponent `Files`. Filen otrzymuje od systemu jedynie tymczasowy, wyizolowany token dostępu (URI) do jednego, konkretnego pliku wskazanego przez użytkownika.

Napastnik odbija się od barier piaskownicy systemu i nie ma technicznej możliwości wyciągnięcia nieautoryzowanych struktur danych.

### 3. Bezwzględne zamrażanie procesów i optymalizacja sieciowa

Zarówno Ente, jak i Filen mają systemowy profil baterii ustawiony na "Ograniczone" (*Restricted*) oraz całkowicie wyłączoną możliwość działania w tle (*Allow background usage: Disabled*).

Ponieważ backup realizuję wyłącznie manualnie, scenariusz wygląda następująco: otwieram aplikację, wykonuję transfer, po czym **zamykam program**. W tym dokładnie momencie jądro systemu natychmiastowo i bezwarunkowo wysyła sygnał terminacji procesu (**`SIGKILL`**). Proces aplikacji oraz wszelkie struktury dynamicznie załadowanego kodu w RAM-ie przestają istnieć. Wyjątek DCL nie ma fizycznej możliwości wykonania jakiejkolwiek ukrytej aktywności w tle.

**Ważna uwaga dotycząca Security Fatigue (Zmęczenia bezpieczeństwem):** GrapheneOS umożliwia permanentne odcięcie sieci na poziomie SELinux dla każdej aplikacji. W tym modelu zdecydowałem o pozostawieniu sieci jako aktywnej. Skoro jądro systemu natychmiast zabija proces po wyjściu z aplikacji, ryzyko sieciowe w tle spada do zera. Wprowadzanie manualnego, ciągłego ryglowania sieci w ustawieniach przed i po każdym backupie generowałoby wyłącznie *security fatigue* – frustrację użytkownika, która w dłuższej perspektywie prowadzi do błędów ludzkich.

## Podsumowanie, czyli dojrzałe zarządzanie ryzykiem

Przyznanie uprawnienia DCL dla aplikacji *Open Source*, takich jak Ente czy Filen, w połączeniu z rygorystyczną konfiguracją GrapheneOS, to podręcznikowy przykład świadomego zarządzania ryzykiem szczątkowym.

Rezygnacja z nadmiarowej blokady sieciowej na rzecz automatyzacji czyszczenia pamięci przez sygnał `SIGKILL` to świadoma decyzja inżynieryjna. Zabezpieczenia zostały zoptymalizowane tak, aby skutecznie neutralizować realne zagrożenia (aktywność DCL w tle), jednocześnie chroniąc użytkownika przed proceduralnym zmęczeniem. Weryfikowalność otwartego kodu kryptograficznego tych narzędzi jest znacznie cenniejsza niż ślepe trzymanie się blokad systemowych za cenę korzystania z zamkniętych, komercyjnych "czarnych skrzynek".
