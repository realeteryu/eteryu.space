+++
title = "Mój zero-trust setup w 2026 roku: Google Pixel 8 + GrapheneOS"
date = 2026-06-22
description = "Szczegółowy zapis mojej konfiguracji GrapheneOS na Pixelu 8. Architektura profili, izolacja WireGuard, higiena uprawnień oraz kompletny przegląd codziennego stacku aplikacji."

[extra]
toc = true
+++

Proces de-big-techowania mojego cyfrowego życia to maraton, a nie sprint. Po przygodach z `MDM` i wymianie urządzenia wiedziałem, że kolejny krok musi być bezkompromisowy. Gdy w końcu wziąłem do ręki `Google Pixel 8` z zainstalowanym `GrapheneOS`, cel był jeden: zbudowanie środowiska opartego na filozofii *zero trust*. Oto szczegółowy zapis mojej obecnej konfiguracji.

Wzorując się na świetnym wpisie z bloga davd.io (*Back on GrapheneOS in 2026*), postanowiłem spisać swoją obecną konfigurację. Nowoczesny degoogle nie musi być całkowitą ucieczką w cyfrowy niebyt. W `GrapheneOS` to my decydujemy, komu i ile o sobie mówimy.

## Architektura profili: Separacja i pragmatyzm

Fundamentem mojego setupu jest ścisła izolacja danych. Zamiast mieszać wszystko w jednym miejscu, podzieliłem system na dwie sfery:

- **Profil główny (`Main Profile`):** Całkowicie pozbawiony usług Google. Czyste środowisko `FOSS` (Free and Open Source Software). To tutaj toczy się 90% mojego cyfrowego życia.
- **Obszar prywatny (`Private Space`):** Odizolowana przestrzeń, w której żyją zapiaskowniczone (`sandboxed`) `Google Services` oraz `Google Play Store`.

Konto używane w `Play Store` to zwykła „wydmuszka" bez powiązania z moją tożsamością. Samego sklepu używam wyłącznie do pobierania i aktualizowania aplikacji bankowych. Uważam, że oficjalne repozytorium to wciąż najbezpieczniejsze miejsce, z którego można pobrać aplikacje finansowe, ponieważ minimalizuje to ryzyko podrzucenia zmodyfikowanego pliku `APK`.

## Ruch sieciowy: Izolacja WireGuard i specyfika Private Space

Prywatność na urządzeniu nie ma sensu bez zabezpieczenia sieci. Na obu profilach używam oficjalnego klienta `WireGuard`. Całość opiera się na plikach konfiguracyjnych `.conf`, które można w prosty sposób wygenerować bezpośrednio na stronie internetowej `Mullvad VPN`.

Podczas generowania profilu na ich platformie możemy zintegrować tunel z `DNS content blockerem`, który wycina reklamy, trackery i malware. Tak przygotowany plik pobieramy, a następnie importujemy do aplikacji `WireGuard`.

Warto pamiętać, że oba środowiska są od siebie odizolowane i nie współdzielą działających w tle połączeń VPN. `WireGuard` musi być zainstalowany i skonfigurowany na każdym profilu osobno. Aby ruch się nie mieszał, zastosowałem dywersyfikację geograficzną: profil główny łączy się przez lokalizację w Warszawie (PL), natomiast `Private Space` tuneluje ruch przez Amsterdam (NL).

Kluczowa jest tu jednak specyfika działania samego systemu. Gdy opuszczam `Private Space` i blokuję tę przestrzeń, wszystkie znajdujące się tam aplikacje – w tym również tamtejszy klient `WireGuard` – są całkowicie zamrażane. Ruch sieciowy dla profilu prywatnego zostaje odcięty, a aktywny pozostaje wyłącznie VPN i procesy w profilu głównym. Na obu profilach mam włączone systemowe opcje `Always On` oraz `Block Connections without VPN`. Dzięki temu, nawet podczas przełączania przestrzeni czy ewentualnego rozłączenia tunelu, telefon całkowicie blokuje niezaszyfrowany ruch, zapobiegając wyciekom danych (`leakom`).

## Wygoda i Hardware: Bezpieczeństwo bez kompromisów w UX

Częstą obawą przy przejściu na `GrapheneOS` jest utrata wygody, jaką dają systemy komercyjne. W moim przypadku `Google Pixel 8` pod kontrolą alternatywnego ROM-u w żaden sposób nie odstaje od zwykłego telefonu z `iOS` czy stockowym `Androidem`. Co więcej, uważam, że jest od nich lepszy, bo to ja mam nad wszystkim pełną kontrolę.

Jak wygląda codzienne użytkowanie od strony technicznej?

- **Karta SIM:** Korzystam z fizycznej karty SIM. Z perspektywy prywatności nie różni się ona niczym od `eSIM`, a zapewnia pełną niezależność sprzętową.
- **Uwierzytelnianie:** Do odblokowywania urządzenia używam `PIN-u` z włączoną opcją `scrambled PIN`. To losowy układ cyfr na ekranie przy każdej próbie odblokowania, co uniemożliwia odczytanie kodu na podstawie śladów palców lub podglądania.
- **Biometria:** Używam również odcisku palca. Skaner działa bezbłędnie w każdym z profili. Warto pamiętać, że ze względów bezpieczeństwa w `GrapheneOS` zarówno `PIN`, jak i odciski palców trzeba skonfigurować osobno dla każdego profilu użytkownika.
- **Higiena powiadomień:** Moje podejście do powiadomień push jest bardzo restrykcyjne. Jedyne aplikacje w całym telefonie, które mają uprawnienia do wysyłania powiadomień, to `Molly`, `Telefon`, `Zegar` (na potrzeby budzika), `Wiadomości` oraz `ntfy`. Całkowite odcięcie się od usług Google na profilu głównym rozwiązałem za pomocą wdrożenia pary `Molly` i `ntfy` działających w oparciu o protokół `UnifiedPush`. Dzięki temu wiadomości przychodzą natychmiast, a brak ciągłego odpytywania serwerów w tle przekłada się na znacznie lepsze utrzymywanie baterii.
- **Zapiaskowane usługi Google:** W `Private Space` usługi Google są całkowicie „ślepe i głuche". Mają odebrane uprawnienia do lokalizacji czy danych urządzenia. Działają wyłącznie jako odizolowane procesy tła niezbędne do tego, by aplikacje bankowe nie zgłaszały błędów.

## Komunikacja, tożsamość i przeglądanie sieci

Moja poczta to `Tuta Mail` w połączeniu z `Addy.io` do tworzenia aliasów, gdzie posługuję się wygodnym panelem webowym. Dla każdej usługi tworzę osobny alias. Jeśli baza danych jakiegoś serwisu wycieknie, po prostu usuwam alias, a mój główny adres pozostaje bezpieczny.

W kwestii przeglądania internetu stosuję zasadę absolutnego minimalizmu i pełnego zaufania do inżynierii `GrapheneOS`. Pozbyłem się z systemu wszelkich zewnętrznych forków (takich jak `Cromite`), aby maksymalnie zmniejszyć powierzchnię ataku i nie polegać na zewnętrznych deweloperach.

Cały ruch webowy obsługuje u mnie jedna przeglądarka, ale podzielona na dwie instancje odizolowane zarówno na poziomie systemu, jak i sieci:

1. **`Vanadium` w profilu głównym (Codzienny driver):** Służy mi do codziennego przeglądania sieci. Za blokowanie reklam, skryptów śledzących i malware odpowiada tutaj tunel `WireGuard` zintegrowany z filtrami DNS od `Mullvad VPN`. Dzięki temu ochrona działa na poziomie sieciowym, a przeglądarka nie jest obciążona dodatkowym, zewnętrznym kodem.
2. **`Vanadium` w Private Space (Dane wrażliwe):** Druga, całkowicie czysta instancja przeglądarki, działająca w odizolowanym obszarze systemu. Używam jej wyłącznie do logowania się do bankowości czy usług urzędowych, gdzie i tak posługuję się realnymi danymi osobowymi. Ta przestrzeń posiada swój własny, osobny tunel VPN z filtrowaniem DNS, a gdy ją opuszczam, całe środowisko (wraz z przeglądarką, sesjami i procesami sieciowymi) zostaje natychmiast zamrożone.

## Higiena uprawnień i bezpieczny łańcuch dostaw

Moje podejście do uprawnień nie znosi kompromisów. Wszystkie aplikacje, które nie wymagają połączenia z internetem do swojego podstawowego działania, mają całkowicie odebrany dostęp do sieci poprzez funkcję `Network Permission`. Podobnie te, które nie muszą działać w tle, nie mają pozwolenia na `Allow Background Usage`. `GrapheneOS` pozwala na taką blokadę nawet wobec domyślnych aplikacji systemowych, co na stockowym Androidzie jest niewykonalne.

Do pobierania i aktualizacji aplikacji na profilu głównym używam zamkniętego, bezpiecznego łańcucha dostaw składającego się z zestawu:

`Obtainium` -> `GitHub` -> `AppVerifier BG`

`Obtainium` pobiera czyste pliki bezpośrednio z wydań (`releases`) na repozytoriach autorów. Następnie `AppVerifier BG` (fork oryginału oferujący bazę tworzoną przez *Privacy Guides* dla ich projektu *Verified Apps* oraz opcję importu, eksportu i tworzenia lokalnej bazy) sprawdza, czy podpis kryptograficzny pobranej aplikacji jest w 100% zgodny z oryginałem. To świetny podwójny test (`double-check`) chroniący przed sytuacją, w której konto dewelopera na GitHubie zostało przejęte i podmieniono w nim pliki `APK`.

## Mój codzienny zestaw aplikacji (App Stack)

Na chwilę obecną zbieram się do stworzenia mojego małego homelabu i rozpoczęcia przygody z self-hostingiem. Póki co używam albo aplikacji lokalnych z możliwością eksportu danych, albo zaufanych chmur typu *zero-knowledge*.

Podstawowe narzędzia, takie jak `Telefon`, `Wiadomości`, `Zegar`, `Aparat` czy `Menedżer plików`, to bezpieczne aplikacje systemowe dostarczane przez `GrapheneOS`. Reszta mojego stacku wygląda następująco:

### Prywatność, Bezpieczeństwo i Komunikacja

- **`KeePassDX`:** Menedżer haseł. Działa w 100% lokalnie i pozwala na eksport bazy haseł do bezpiecznego pliku `.kdbx`.
- **`Aegis`:** Obsługa weryfikacji dwuskładnikowej (`2FA`). Bezpieczna, otwarta aplikacja z opcją szyfrowanego eksportu bazy do pliku.
- **`Molly`:** Utwardzony pod kątem bezpieczeństwa fork `Signala`, który służy mi jako główny komunikator.
- **`ntfy`:** Wspomniany wcześniej klient powiadomień `UnifiedPush`, który dostarcza wiadomości z `Molly`.
- **`HeliBoard`:** Klawiatura. To jedno z najbardziej wrażliwych miejsc w systemie, bo to właśnie klawiatury bywają największymi zbieraczami danych. Choć `HeliBoard` to rozwiązanie `FOSS` i privacy-friendly, profilaktycznie ma całkowicie odcięty dostęp do sieci (`Network Permission`) oraz wyłączoną opcję uczenia się na podstawie wpisywanego tekstu.

### Przechowywanie danych i Produktywność

- **`Filen`:** Dysk chmurowy działający w modelu zero knowledge, przeznaczony do bezpiecznego przechowywania plików i backupów.
- **`Ente Photos`:** Świetna alternatywa dla Google Photos. Szyfrowany (`E2EE`) cloud storage dla zdjęć oraz lokalna galeria działająca na urządzeniu.
- **`Quillpad`:** Otwartoźródłowa aplikacja, która przejmuje u mnie rolę edytora tekstu, notatnika i narzędzia do list todo. Zastąpiła dotychczasowe rozwiązania, idealnie wpisując się w mój minimalistyczny workflow oparty na filozofii FOSS.

### Fotografia i higiena metadanych (OpSec)

- **`Pixel Camera`:** Oficjalna aplikacja aparatu od Google dla serii Pixel zapewnia pełną jakość przetwarzania obrazu i zaawansowane algorytmy trybu nocnego. Działa u mnie całkowicie na moich warunkach, ponieważ ma permanentnie odebrane uprawnienie dostępu do sieci (`Network Permission`).
- **`GCam Services Provider` (`photosonly`):** To lekki, otwartoźródłowy mostek (`stub`) autorstwa *lukaspieper*, który imituje obecność usług Google Play wyłącznie w zakresie wymaganym do podglądu zdjęć. Dzięki niemu aparat działa stabilnie offline i płynnie przekazuje pliki do zewnętrznej galerii (`Ente Photos`).
- **`Image Toolbox`:** Kluczowe narzędzie w moim codziennym OpSec. Zanim jakiekolwiek zdjęcie z galerii trafi do sieci, na bloga czy do komunikatora, przepuszczam je przez `Image Toolbox` za pomocą szybkiego menu *Udostępnij*. Aplikacja w sekundę sterylizuje plik z metadanych `EXIF`, wycinając lokalizację GPS, znacznik czasu oraz model urządzenia.

### Rozrywka i Codzienność

- **`NewPipe`:** Pełni u mnie funkcję awaryjnego klienta YouTube. Zastąpił aplikację PipePipe, ponieważ nie potrzebuję aż tylu źródeł wideo i korzystam tylko z YT. Każda linijka niewykorzystywanego kodu to zbędne ryzyko w moim threat modelu, podejściu zero trust i pod kątem attack surface. Ponadto NewPipe jest częściej aktualizowany i bardziej stabilny. Dodatkowo mocno ograniczam korzystanie z YouTube na telefonie, więc sięgam po niego tylko, gdy naprawdę muszę.
- **`Auxio`:** Odtwarzacz przejmujący rolę słuchania muzyki (wcześniej realizowaną przez PipePipe). Używam go, aby intencjonalnie i świadomie budować własną playlistę. Zanim na dobre rozpocznę przygodę z selfhostingiem i własnym homelabem, jest to dla mnie świetne rozwiązanie, które przy okazji omija problemy z frontendami YouTube i ich API.
- **`Breezy Weather`:** Przejrzysta, estetyczna i otwarta aplikacja pogodowa.
- **`Calculator You`:** Otwarta, nowoczesna alternatywa dla domyślnego kalkulatora systemowego.

## A co z bankowością i bezpieczeństwem niskopoziomowym?

Wielu użytkowników obawia się, czy aplikacje bankowe w ogóle ruszą na alternatywnym systemie operacyjnym. W Polsce sytuacja wygląda zaskakująco dobrze, ale zamiast opisywać każdy bank z osobna, odsyłam Was do gotowych, stale aktualizowanych zestawień kompatybilności:

- [Zestawienie PrivSec.dev (Wersja webowa)](https://privsec.dev/posts/android/banking-applications-compatibility-with-grapheneos/#poland)
- [Plik źródłowy na GitHubie PrivSec](https://github.com/PrivSec-dev/privsec.dev/blob/main/content/posts/android/Banking%20Applications%20compatibility%20with%20GrapheneOS.md#poland)

Warto pamiętać, że `GrapheneOS` chroni nas znacznie głębiej niż na poziomie samych uprawnień w interfejsie. System natywnie blokuje dostęp do mechanizmów Dynamic Code Loading (`DCL`) poprzez pamięć operacyjną czy pamięć masową oraz skutecznie odpiera dziesiątki innych exploitów. O tym procesie, jak i o analizie zagrożeń za pomocą frameworku `STRIDE`.

## Podsumowanie

Wdrożenie modelu *zero trust* na Pixelu 8 wymaga zmiany pewnych przyzwyczajeń, ale daje niesamowite poczucie kontroli. Świadomość, że mój telefon nie wysyła telemetrycznego raportu o każdym moim kroku do korporacyjnych serwerów, jest warta każdej minuty spędzonej na konfiguracji.

Nowoczesny degoogle to nie asceza. To stan, w którym smartfon w końcu staje się moim narzędziem, a nie ja jego produktem.

A jak wyglądają Wasze doświadczenia z `GrapheneOS`? Idziecie w pełną izolację czy szukacie kompromisów? Dajcie znać w komentarzach na Mastodonie!
