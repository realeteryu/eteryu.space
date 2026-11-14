+++
title = "SMS of Death: dlaczego aktualizacje to nie opcja, a higiena"
date = 2026-11-12
description = "Analiza CVE-2026-58882. Jak 8 bajtów wysłanych w wiadomości WAP Push kładzie telefonię w Androidzie i dlaczego starsze wersje systemu zostają bez ochrony."

[extra]
toc = true
+++

Wiadomość SMS, która nigdy nie trafia do skrzynki odbiorczej. Zero interakcji ze strony użytkownika. Telefon traci zasięg, aktywne połączenie zostaje zerwane, a w najgorszym scenariuszu urządzenie wpada w pętlę restartów. To nie scenariusz z filmu science fiction, a realna podatność oznaczona jako CVE-2026-58882, nazwana przez odkrywcę „SMS of Death”[^1].

## Czym jest WAP Push i dlaczego to działa?

Większość użytkowników kojarzy SMS-y wyłącznie z tekstem, który pojawia się w aplikacji Wiadomości. Tymczasem infrastruktura telefonii komórkowej obsługuje również wiadomości typu **WAP Push** – czyli binarne wiadomości danych (data SMS), które operatorzy wykorzystują do zdalnej konfiguracji urządzeń, np. do ustawień MMS lub parametrów sieci.

Kluczowy problem polega na tym, że te wiadomości są przetwarzane przez system operacyjny **na poziomie frameworka telekomunikacyjnego**, zanim jeszcze trafią do jakiejkolwiek aplikacji użytkownika. Oznacza to, że atakujący nie musi czekać, aż ofiara otworzy wiadomość – wystarczy, że telefon ją odbierze.

## Anatomia błędu: od 8 bajtów do reboot loopa

Podatność tkwi w funkcji `WapPushOverSms.decodeWapPdu()` w Androidzie. Mechanizm jest klasycznym błędem obcięcia liczby całkowitej (numeric truncation error, **CWE-197**)[^2]:

1.  Nagłówek wiadomości WAP Push zawiera 5-bajtową wartość określającą jego długość.
2.  Android odczytuje tę wartość jako liczbę typu `long` (np. 2 147 483 648).
3.  Następnie konwertuje ją do mniejszego typu `int` (32-bit), **bez sprawdzenia zakresu**.
4.  Wartość ulega obcięciu i staje się liczbą ujemną (-2 147 483 648).
5.  System próbuje zaalokować tablicę o ujemnym rozmiarze, co wywołuje wyjątek `NegativeArraySizeException`.
6.  Wyjątek nie jest przechwytywany, co powoduje natychmiastowe ubicie procesu `com.android.phone` – czyli usługi odpowiedzialnej za obsługę połączeń.

Pojedyncza wiadomość powoduje utratę zasięgu na 1-2 sekundy, dopóki proces nie zostanie zrestartowany. Problem w tym, że operatorzy często ponawiają wysyłkę niepotwierdzonych wiadomości, a Android zapisuje „zatrutą” wiadomość w kolejce oczekujących, którą próbuje przetworzyć przy każdym rozruchu. Według badacza Marka Blaszczyka, około 20 takich wiadomości może trwale unieruchomić urządzenie, zmuszając użytkownika do przywrócenia ustawień fabrycznych[^3].

## Dlaczego Android 12 i 13 zostają bez łatki?

To jest punkt, w którym filozofia **distrust** staje się niezwykle aktualna. Google naprawiło błąd w ramach aktualizacji bezpieczeństwa z października 2026 roku. Poprawka trafiła jednak **wyłącznie do Androida 14, 15, 16 i 17**.

> **Brak ochrony dla starszych urządzeń**
> 
> Urządzenia pracujące na Androidzie 12 i 13 – mimo że podatność została na nich z powodzeniem zreprodukowana – **nie otrzymały i najprawdopodobniej nie otrzymają oficjalnej łatki**. Wynika to z cyklu wsparcia: Google oficjalnie zakończyło wsparcie bezpieczeństwa dla tych wersji.

Warto dodać niuans: badacz zgłosił błąd 5 maja 2026 roku, a Google wewnętrznie opracowało poprawkę już 15 czerwca. Trafiła ona jednak do publicznej wiadomości dopiero w październiku, co mogło sprawiać wrażenie, że zgłoszenie zostało zignorowane[^4].

To doskonały przykład na to, dlaczego „domyślne zaufanie” do systemu operacyjnego jest złudzeniem. System może być technicznie szczelny w momencie zakupu, ale po zakończeniu cyklu wsparcia staje się cyfrowym reliktem – podatnym na ataki, na które nikt już nie szuka rozwiązań.

## Co to oznacza dla użytkownika?

Zgodnie z zasadą minimalizacji ryzyka, oto praktyczne wnioski:

*   **Aktualizuj.** Jeśli korzystasz z Androida 14 lub nowszego, upewnij się, że masz zainstalowaną aktualizację bezpieczeństwa z października lub listopada 2026. W wielu przypadkach wymaga to restartu urządzenia.
*   **Weryfikuj cykl wsparcia przed zakupem.** Kupując telefon, nie patrz tylko na specyfikację. Sprawdź, jak długo producent gwarantuje aktualizacje bezpieczeństwa. Urządzenie bez wsparcia to potencjalna luka, niezależnie od ceny.
*   **Nie licz na „spokój”.** Fakt, że podatność nie jest obecnie masowo wykorzystywana, nie oznacza, że jest bezpieczna. Kod proof-of-concept jest publicznie dostępny, a bariera wejścia dla atakującego jest zerowa – wystarczy znać numer telefonu ofiary.

> **Uwaga na fałszywe poczucie bezpieczeństwa**
> 
> Jeśli korzystasz z Androida 12 lub 13, wyłączenie WAP Push w ustawieniach aplikacji SMS **nie zapobiega atakowi**. Podatność jest wykorzystywana na niższym poziomie systemu (`com.android.phone`), zanim wiadomość trafi do aplikacji. Jedynym realnym rozwiązaniem jest aktualizacja systemu. W razie wystąpienia pętli restartów pozostaje ręczne usunięcie zablokowanej wiadomości z kolejki (np. przez `ADB`) lub użycie narzędzi pomocniczych, takich jak WapGuard[^5].

## Podsumowanie

CVE-2026-58882 nie jest wyrafinowanym atakiem. To błąd walidacji, który mógłby zostać wyeliminowany odpowiednio wczesną kontrolą typów. Prawdziwym problemem nie jest jednak sam błąd, a **model dystrybucji łat** i cykl życia systemu. „SMS of Death” przypomina, że w świecie, w którym urządzenie jest naszym głównym interfejsem do rzeczywistości, aktualizacje bezpieczeństwa nie są luksusem. Są higieną.

---

## Przypisy

[^1]: CVE-2026-58882: Android "SMS of Death" Vulnerability. ThaiCERT. [https://www.thaicert.or.th/en/2026/11/02/android-sms-of-death-vulnerability-could-crash-phone-services-through-crafted-messages/](https://www.thaicert.or.th/en/2026/11/02/android-sms-of-death-vulnerability-could-crash-phone-services-through-crafted-messages/)
[^2]: CWE-197: Numeric Truncation Error. Common Weakness Enumeration. [https://cwe.mitre.org/data/definitions/197.html](https://cwe.mitre.org/data/definitions/197.html)
[^3]: BleepingComputer. (2026, October 31). New Android SMS of Death exploit can remotely crash unpatched phones. [https://www.bleepingcomputer.com/news/security/new-android-sms-of-death-exploit-can-remotely-crash-unpatched-phones/](https://www.bleepingcomputer.com/news/security/new-android-sms-of-death-exploit-can-remotely-crash-unpatched-phones/)
[^4]: eSecurity Planet. (2026, October 30). Android 'SMS of Death' Flaw Can Drop Emergency Calls, Trigger Reboot Loops. [https://www.esecurityplanet.com/threats/news-android-sms-of-death-cve-2026-58882/](https://www.esecurityplanet.com/threats/news-android-sms-of-death-cve-2026-58882/)
[^5]: Android Authority. (2026, October 30). Android's SMS of Death bug can knock out your phone, but a fix is coming soon. [https://mobile.androidauthority.com/android-sms-of-death-google-fix-3729929/](https://mobile.androidauthority.com/android-sms-of-death-google-fix-3729929/)
