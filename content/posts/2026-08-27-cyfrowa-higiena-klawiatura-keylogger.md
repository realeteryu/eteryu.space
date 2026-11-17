+++
title = "Cyfrowa higiena bez złudzeń. Dlaczego Twoja klawiatura to legalny keylogger?"
date = 2026-08-27
description = "Klawiatura ekranowa i schowek to najczęściej ignorowane elementy systemu. Sprawdź, jak ograniczyć telemetrię, zmniejszyć ryzyko keyloggingu i zabezpieczyć dane przed atakami Clipper."

[taxonomies]
tags = ["prywatnosc", "android", "bezpieczenstwo"]

[extra]
toc = true
+++

Kiedy myślimy o prywatności w smartfonie, instynktownie zaklejamy kamerę, blokujemy mikrofon i wyłączamy lokalizację. Tymczasem często dobrowolnie oddajemy nasze najskrytsze myśli, wpisywane hasła i intymne wiadomości jednemu niepozornemu elementowi systemu. Tym elementem jest klawiatura ekranowa. Czas rozprawić się z tym największym koszmarem telemetrycznym.

W dopełnieniu koncepcji `Zero Trust` czas zająć się dwoma najbardziej newralgicznymi i najczęściej ignorowanymi komponentami mobilnymi, czyli klawiaturą oraz schowkiem systemowym.

## Klawiatura: legalny keylogger w Twojej kieszeni

Domyślne klawiatury dostarczane ze współczesnymi systemami, z `Google Gboard` na czele, mogą stanowić istotne źródło danych telemetrycznych. Pod przykrywką wygody w postaci autokorekty, podpowiedzi słów i naklejek, analizują sposób, w jaki piszesz, oraz Twoje nawyki językowe.

Narzędzia te mogą gromadzić informacje o używanych słowach i budować profile behawioralne w oparciu o analizę dynamiki pisania. Mierzą tempo wprowadzania tekstu, długość przerw i specyfikę popełnianych błędów. Co gorsza, lokalne słowniki automatyczne potrafią zapamiętywać i po czasie podpowiadać poufne dane, takie jak numery `PIN`, frazy z menedżera haseł czy dolegliwości wpisywane w oknie wyszukiwarki.

    [ ZAGROŻENIE: Telemetryczna klawiatura domyślna ]

     Uderzenie w klawisz ──► [ KLAWIATURA (np. Gboard) ] ──► [ Słownik lokalny / AI ]
                                     │
                                     ▼ (Telemetria i statystyki)
                         [ SERWERY GOOGLE / CLOUD ]


    [ ROZWIĄZANIE: Odizolowana maszyna do pisania ]

     Uderzenie w klawisz ──► [ HELIBOARD / FUTO ] ──► System Android (AOSP)
                                     │
                                  ( X ) ──► BRAK INTERNETU / ZABLOKOWANY FIREWALL

### Jak okiełznać klawiaturę? Strategia dla iOS oraz Androida

Problem ten można rozwiązać na dwa sposoby, w zależności od używanego ekosystemu.

#### 1. iOS i metoda prymitywnej maszyny do pisania

W zamkniętym ekosystemie Apple instalowanie zewnętrznych klawiatur bywa problematyczne pod kątem uprawnień. Producent chwali się wprawdzie prywatnością różnicową (`Differential Privacy`) i przetwarzaniem danych głównie na samym urządzeniu, jednak w myśl zasady ograniczonego zaufania nie chcemy, aby system tworzył jakiekolwiek słowniki naszych zachowań. Najskuteczniejszą metodą ograniczenia tego rodzaju przetwarzania jest wyłączenie funkcji predykcyjnych klawiatury systemowej.

Wchodzisz w ustawienia ogólne, następnie w zakładkę klawiatury i wyłączasz funkcje, których nie potrzebujesz. Zaznacz do wyłączenia autokorektę, sprawdzanie pisowni, podpowiedzi tekstowe oraz automatyczne wielkie litery. Klawiatura zostaje wtedy sprowadzona do roli prymitywnej maszyny do pisania. Jej zadaniem jest po prostu wprowadzanie znaków bez dodatkowych podpowiedzi i automatycznej analizy tekstu.

#### 2. Android, bezpieczne alternatywy i fizyczne odcięcie sieci

Środowisko `Androida` daje większą swobodę i pozwala na zastąpienie klawiatury producenta. Warte uwagi alternatywy zorientowane na prywatność to `HeliBoard` oraz `FUTO Keyboard`.

Co sprawia, że są one interesującym wyborem?

- **Przejrzystość kodu:** `HeliBoard` to projekt w pełni darmowy i otwartoźródłowy na licencji `FOSS`. Z kolei `FUTO Keyboard` nie posiada ścisłej licencji `FOSS`, ale udostępnia swój kod do wglądu i deklaruje działanie z naciskiem na przetwarzanie lokalne. Obie aplikacje są projektowane z myślą o ograniczeniu śledzenia.
- **Brak uprawnienia do sieci:** `HeliBoard` na poziomie manifestu aplikacji nie deklaruje dostępu do internetu (`android.permission.INTERNET`). Nie może więc samodzielnie nawiązywać połączeń sieciowych. W przypadku innych klawiatur warto sprawdzić ich uprawnienia i sposób działania.
- **Awaryjna zapora sieciowa:** Jeśli z jakiegoś powodu musisz korzystać ze standardowej klawiatury systemowej, użyj lokalnego firewalla takiego jak `RethinkDNS`. Zablokuj w nim ruch sieciowy dla aplikacji klawiatury.
- **Wyłączone uczenie i lokalne słowniki:** Nawet jeśli klawiatura jest fizycznie odcięta od sieci, w jej ustawieniach wyłącz opcje uczenia się wpisywanego tekstu, jeśli nie są Ci potrzebne. Lokalny słownik może być objęty systemową kopią zapasową (`Cloud Backup`). Ponadto nadgorliwe podpowiedzi mogą zdradzić Twoje hasło osobom zerkającym Ci przez ramię (tzw. `shoulder surfing`), a złośliwe aplikacje nadużywające uprawnień ułatwień dostępu (`Accessibility Services`) mogą odczytywać informacje wyświetlane na ekranie.

## Schowek: otwarta księga dla aplikacji

Drugim najsłabszym ogniwem systemu jest schowek. To tymczasowy bufor pamięci, do którego trafia wszystko, co skopiujesz. Mogą to być kody jednorazowe z wiadomości SMS, hasła i tokeny dostępowe z menedżera haseł, numery kont bankowych czy adresy portfeli kryptowalutowych.

Przez lata schowek był słabo chronionym elementem systemu, a aplikacje mogły odczytywać jego zawartość bez wyraźnej informacji dla użytkownika. Dopiero nowsze wersje systemów operacyjnych (m.in. `iOS 14` i `Android 12`) zaczęły wyraźniej informować o dostępie aplikacji do schowka. Ujawniło to, że wiele popularnych aplikacji, od sieci społecznościowych po darmowe gry, odczytywało jego zawartość w różnych momentach działania.

### Cyberzagrożenie w postaci ataków typu Clipper

`Clipper` to rodzaj złośliwego oprogramowania działającego w tle. Monitoruje ono zawartość schowka w poszukiwaniu wzorców przypominających numery kont bankowych lub adresy kryptowalutowe. Kiedy wykryje skopiowany ciąg, podmienia go w pamięci na adres należący do atakującego. Jeśli bezrefleksyjnie wkleisz adres i klikniesz „Wyślij”, Twoje środki mogą zniknąć bezpowrotnie.

    [ ATAK TYPU CLIPPER: Podmiana bufora ]

    Skopiowano:  PL 12 3456 7890 ... (Twój bank)
                        │
                        ▼
              [ ZŁOŚLIWY CLIPPER ] ──► Podmienia bufor w RAM
                        │
    Wklejono:    PL 99 8765 4321 ... (Konto Hakera)


    [ BEZPIECZNY SCHOWEK: Czyszczenie i izolacja ]

    Menedżer haseł ──► [ SCHOWEK RAM ] ──► ( Wklejenie hasła )
                            │
                      [ TIMER 20s ] ──► ( BUFOR AUTOMATYCZNIE WYCZYSZCZONY! )

### Cztery filary bezpiecznego schowka

**1. Wyłącz historię schowka w klawiaturze**

Funkcje takie jak historia schowka w `Gboardzie` tworzą dodatkowy rejestr skopiowanych elementów. Jeśli trafiają tam hasła lub inne poufne dane, zwiększa to ryzyko ich przypadkowego ujawnienia. W ustawieniach klawiatury wyłącz przechowywanie historii skopiowanych elementów, jeśli nie korzystasz z tej funkcji.

**2. Ustaw automatyczne czyszczenie bufora**

Wprawdzie system `Android` (od wersji `13`) automatycznie czyści schowek systemowy po określonym czasie, jednak dla haseł to wciąż może być zbyt długie okno ekspozycji. Bezpieczne menedżery haseł, takie jak `Bitwarden` czy `KeePassDX`, posiadają funkcję automatycznego czyszczenia schowka po określonym czasie od skopiowania ciągu. Upewnij się, że opcja ta jest aktywna. Hasło po wklejeniu nie powinno pozostawać w schowku dłużej, niż jest to konieczne.

**3. Restrykcyjna kontrola uprawnień w systemie**

W nowszych wersjach systemów, takich jak `iOS` i `Android`, oraz w wariantach ukierunkowanych na utwardzenie, takich jak `GrapheneOS`, zwracaj uwagę na powiadomienia o dostępie aplikacji do schowka. Jeśli usługa lub gra nie ma uzasadnionego powodu, aby odczytywać bufor, ogranicz jej dostęp lub zrezygnuj z korzystania z niej.

**4. Weryfikacja kontekstowa i zasada ograniczonego zaufania**

Nawet na utwardzonym telefonie wypracuj nawyk precyzyjnego sprawdzania danych. Przy przelewach bankowych lub przesyłaniu krytycznych danych zawsze weryfikuj cały wklejony numer rachunku lub adres portfela, najlepiej porównując go z oryginalnym źródłem. To najprostsza i najskuteczniejsza metoda ograniczenia ryzyka ataków typu `Clipper`.

## Podsumowanie

Wygoda może być wrogiem bezpieczeństwa. Inteligentne autokorekty, uczące się słowniki i zapamiętywanie skopiowanych haseł brzmią jak ułatwienie życia, ale mogą generować dodatkową powierzchnię ataku.

Ograniczając funkcje klawiatury, wybierając rozwiązanie, które nie potrzebuje dostępu do sieci, oraz wymuszając automatyczne czyszczenie schowka, odzyskujesz większą kontrolę nad najważniejszymi punktami styku z Twoim urządzeniem. Cyfrowy sejf na nic się nie zda, jeśli nie zadbasz o to, co dzieje się z danymi, zanim do niego trafią.
