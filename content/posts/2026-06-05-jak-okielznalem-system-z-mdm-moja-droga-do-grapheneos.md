+++
title = "Jak okiełznałem system z MDM. Moja droga do GrapheneOS"
date = 2026-06-05
description = "Historia o tym, jak ukryty profil MDM w telefonie z drugiej ręki zmusił mnie do natychmiastowej reakcji i ewakuacji cyfrowej tożsamości, a GrapheneOS stał się moim nowym, bezpiecznym domem."
+++

W świecie cyberbezpieczeństwa często rozmawiamy o „modelach zagrożeń” w sposób czysto teoretyczny. Sam byłem w tym miejscu, budując swój poprzedni setup na iPhonie 17 Pro. Wszystko zmieniło się w ułamku sekundy, gdy teoria zderzyła się z brutalną rzeczywistością.

Gdy moja żona nagle trafiła do szpitala, priorytety zmieniły się w mgnieniu oka. Aby podreperować budżet, sprzedałem iPhone'a, a za ułamek zdobytej kwoty kupiłem z drugiej ręki Pixela 8. Od dawna interesowałem się rozwiązaniami *privacy-focused* oraz ruchem FOSS, więc ten model był naturalnym kierunkiem — docelowo miał na nim wylądować GrapheneOS. Na tamten moment nie posiadaliśmy jednak w domu komputera, ale jak się później okazało... to i tak nic by nie zmieniło. Ukryty profil MDM, który znajdował się na telefonie, skutecznie zablokowałby każdą próbę odblokowania bootloadera i flashowania systemu. Musiałem więc uruchomić go na oprogramowaniu fabrycznym.

Chcę Wam opowiedzieć o incydencie, który zmusił mnie do natychmiastowej ewakuacji cyfrowej tożsamości i o tym, jak techniki obronne pozwoliły mi przetrwać najgorszy moment.

## Kiedy Twój telefon zaczyna żyć własnym życiem

To był potwornie ciężki okres. Moja żona leżała w szpitalu, a telefon był moim jedynym, krytycznym oknem na świat i jedynym źródłem informacji o jej stanie zdrowia. Połowa moich myśli była na oddziale szpitalnym, gdy nagle zauważyłem, że mój nowo kupiony Pixel 8 zachowuje się nietypowo.

Zaczęło się od drobnych anomalii, które szybko przerodziły się w techniczny koszmar:

- **Aktywny podsłuch:** Na ekranie pojawiała się zielona ikona aktywnego mikrofonu, mimo że nie prowadziłem żadnej rozmowy ani nie używałem aplikacji dyktafonu.
- **Zdalne sterowanie:** Telefon losowo wykonywał akcje, jakby ktoś sterował nim w czasie rzeczywistym.
- **Manipulacja danymi:** Z systemowej galerii Google Photos zaczęły znikać pojedyncze zdjęcia i zrzuty ekranu.
- **Intercepcja połączeń:** Zauważyłem bezpodstawne przekierowania moich połączeń przychodzących na obcy, nieznany numer.

Najgorszy był efekt "migawki" — ekran zachowywał się tak, jakby ktoś robił zrzuty ekranu w locie. W warunkach skrajnego stresu rodzinnego, świadomość, że ktoś obcy patrzy na mój ekran i słucha mojego otoczenia, była paraliżująca.

## Śledztwo w trybie naprawy: Syndrom MDM

Wiedziałem, że standardowy restart nic nie da. Wyłączyłem urządzenie i uruchomiłem je w trybie naprawy (*Recovery/Repair Mode*). To tam klocki układanki wreszcie wpadły na swoje miejsce.

> **Urządzenie jest kontrolowane przez administratora...**
> Telefon posiadał ukryty, nadrzędny profil **MDM (Mobile Device Management)**. Ktoś sprzedał mi urządzenie korporacyjne jako czysty sprzęt konsumencki. Profil MDM dawał zdalnemu napastnikowi uprawnienia administracyjne wyższe niż moje własne. Mógł w locie podglądać ekran, aktywować sensory i manipulować routingiem połączeń.

Sprawę natychmiast zgłosiłem na Policję, a urządzenie zostało zakwalifikowane do zwrotu. Pojawił się jednak kluczowy problem: jak bezpiecznie przeczekać kilka dni, mając na telefonie krytyczne konta bankowe, mailowe i komunikatory, w dodatku bez możliwości odcięcia się od sieci ze względu na sytuację w szpitalu?

## Incident Response w warunkach bojowych

Musiałem działać jak inżynier w strefie konfliktu. Zastosowałem podręcznikową taktykę izolacji skażonego hosta za pomocą trzech kroków:

### 1. Bezpieczny bastion dla dowodów (Ente Photos)
Zanim zacząłem głębszą konfigurację, zabezpieczyłem zrzuty ekranu dokumentujące aktywność aplikacji oraz sam komunikat o MDM. Ponieważ napastnik bezczelnie usuwał pliki z Google Photos, zainstalowałem **Ente Photos** — w pełni szyfrowaną (E2EE) alternatywę dla chmury giganta. Napastnik stracił możliwość manipulowania tymi plikami, co uratowało moją dokumentację dla Policji.

### 2. Zapora ogniowa w trybie Default-Deny
Zainstalowałem lokalny firewall **RethinkDNS** i natychmiast przełączyłem go w tryb całkowitej blokady ruchu wychodzącego. Analizując logi w locie, zauważyłem, że ataki i próby eksfiltracji danych szły głównie przez systemowe procesy `Shell` oraz `SystemUI`. Odcięcie im dostępu do sieci sparaliżowało architekturę Command & Control (C2) napastnika.

### 3. Szyfrowany i filtrowany DNS od Quad9
W RethinkDNS zmieniłem domyślny routing zapytań na **Quad9 DNS**. Poza szyfrowaniem ruchu, Quad9 automatycznie dropuje zapytania do znanych złośliwych domen i serwerów botnetowych na poziomie infrastruktury.

Dzięki tej sterylnej kwarantannie zyskałem cenny czas. Wszystkie dowody przesłałem prowadzącej sprawę policjantce za pomocą bezpiecznej, szyfrowanej skrzynki **Tuta Mail**. W tym sztucznie utrzymywanym środowisku spokojnie zmieniłem hasła i klucze sesyjne we wszystkich krytycznych usługach.

## Nowy początek: GrapheneOS jako cyfrowa forteca

Po odesłaniu wadliwego urządzenia skontaktowałem się ze sprzedawcą. Sklep wykazał się niesamowitą postawą — w ramach wymiany nie tylko dostarczyli mi pewny egzemplarz Pixela 8, ale ich serwisant **samodzielnie wgrał GrapheneOS na nowy telefon przed wysyłką**. Chciał mieć absolutną pewność, że sprzęt trafi do mnie w 100% czysty i bezpieczny.

Wgranie GrapheneOS całkowicie eliminuje ryzyko przetrwania ukrytych profilów korporacyjnych (dzięki mechanizmowi Verified Boot). Na tym systemie buduję swój obecny Threat Model, oparty na braku ślepego zaufania (Zero Trust):

- **Całkowita de-googlizacja:** Profil główny jest całkowicie wolny od kodu Google (FOSS). Przestarzałego F-Droida zastąpiłem nowoczesnymi, bezpiecznymi menedżerami **Obtainium** i **Accrescent**.
- **Ścisła izolacja (Multiple Profiles):** Wszystkie usługi komercyjne zamknąłem w odizolowanym, osobnym profilu użytkownika.
- **Separacja sieciowa:** Ruch sieciowy obu profili jest całkowicie odseparowany przez niezależne instancje WireGuard w trybie Always-on.

## Wnioski na przyszłość

Prywatność i cyberbezpieczeństwo to nie są hobbystyczne fanaberie. Skompromitowane urządzenie mobilne to bezpośrednie zagrożenie dla Twojego życia osobistego, finansów i spokoju psychicznego.

> **Nie czekaj na anomalię**
> Jeśli podejrzewasz, że Twój system zachowuje się nienaturalnie — nie czekaj. Odetnij procesy systemowe od sieci, zabezpiecz tożsamość za pomocą szyfrowanych narzędzi i szukaj rozwiązań, które dają Ci pełną kontrolę nad krzemem, za który zapłaciłeś. Dla mnie tą jedyną, bezpieczną odpowiedzią stał się duet czystego sprzętu i GrapheneOS.
