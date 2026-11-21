+++
title = "AI i cyberbezpieczeństwo. Jak zmieniają się zagrożenia i jak się chronić?"
date = 2026-11-21
description = "Jak sztuczna inteligencja zmienia cyberprzestępczość, co potrafią już agenci AI i jak chronić swoje konta, smartfon oraz dane?"
[taxonomies]
tags = ["AI", "cyberbezpieczeństwo", "bezpieczeństwo", "prywatność"]
+++

Sztuczna inteligencja coraz częściej pojawia się w kontekście cyberbezpieczeństwa. Pomaga tworzyć wiarygodne wiadomości phishingowe, generować obrazy i głosy, analizować podatności oprogramowania, a nawet automatyzować kolejne etapy działań ofensywnych. Nie oznacza to jednak, że cyberprzestępczość zaczęła się od AI albo że każdy atak jest dziś przeprowadzany przez autonomicznego agenta.

Zmienia się przede wszystkim dostępność narzędzi, szybkość wykonywania niektórych zadań i możliwość ich skalowania. Czynności, które wcześniej wymagały określonych umiejętności lub dużej ilości czasu, mogą być teraz częściowo zautomatyzowane.

Dla użytkownika najważniejsze jest jednak coś innego: coraz trudniej ocenić wiarygodność wiadomości, rozmówcy czy materiału, który widzimy na ekranie. Smartfon, poczta elektroniczna, bankowość i usługi chmurowe tworzą połączony ekosystem, w którym przejęcie jednego konta może mieć konsekwencje dla wielu pozostałych.

## AI jako narzędzie cyberprzestępców

Jednym z najbardziej oczywistych zastosowań AI jest przygotowywanie treści wykorzystywanych w oszustwach. Modele językowe potrafią generować wiadomości w poprawnej polszczyźnie, dopasowywać ich ton do odbiorcy i tworzyć wiele wariantów podobnego komunikatu.

To ważne, ponieważ tradycyjny phishing często zdradzał się błędami językowymi, nienaturalnym stylem albo nieudolnym tłumaczeniem. Nie oznacza to, że takie sygnały zniknęły, ale przestały być wystarczającym sposobem oceny wiarygodności wiadomości.

AI może również pomagać w przygotowaniu fałszywych stron, grafik i materiałów wideo. W przypadku oszustw wykorzystujących wizerunek znanych osób lub przedstawicieli instytucji obraz może wyglądać przekonująco, mimo że przedstawiona sytuacja nigdy nie miała miejsca.

FBI opisywało przypadki wykorzystania filmów generowanych przez AI, w których osoby podszywające się pod funkcjonariuszy FBI kierowały ofiary do fałszywej strony amerykańskiego centrum zgłaszania przestępstw internetowych IC3. Celem było wyłudzanie danych od osób, które wcześniej padły ofiarą oszustwa.[^1]

Podobny mechanizm może dotyczyć rozmów telefonicznych. Klonowanie głosu pozwala stworzyć nagranie przypominające głos konkretnej osoby. Oszust może podszyć się pod członka rodziny, pracownika firmy albo osobę zaufaną i wykorzystać presję czasu, aby skłonić ofiarę do szybkiego przekazania pieniędzy.

Amerykańska Federalna Komisja Handlu (FTC) ostrzega przed oszustwami rodzinnymi, w których przestępcy mogą wykorzystywać narzędzia cyfrowe do tworzenia fałszywych nagrań wideo lub klonowania głosu bliskiej osoby. Typowy scenariusz opiera się na pilnej prośbie o pomoc finansową i żądaniu zachowania sprawy w tajemnicy.[^2]

**Najważniejsza zmiana polega na tym, że sam głos, obraz czy poprawna forma wiadomości przestają być wiarygodnym potwierdzeniem tożsamości.**

## Czy AI potrafi już samodzielnie przeprowadzić cyberatak?

To pytanie wymaga rozróżnienia kilku różnych sposobów wykorzystania sztucznej inteligencji.

Najprostszy scenariusz to AI jako narzędzie wspomagające człowieka. Przestępca może poprosić model o przygotowanie wiadomości phishingowej, wyjaśnienie działania podatności albo wygenerowanie fragmentu kodu. Człowiek nadal podejmuje decyzje i wykonuje kolejne działania.

Bardziej zaawansowanym rozwiązaniem jest agent AI. W odróżnieniu od zwykłego chatbota agent może korzystać z narzędzi, analizować wyniki ich działania i na tej podstawie podejmować kolejne kroki. Można mu powierzyć określony cel, a system będzie realizował serię zadań bez konieczności wydawania osobnego polecenia na każdym etapie.

W praktyce oznacza to możliwość automatyzacji rozpoznania infrastruktury, wyszukiwania podatności, analizowania wyników skanowania czy przygotowywania kodu wykorzystującego znalezione błędy.

Nie jest to już wyłącznie teoretyczna możliwość. W raporcie opublikowanym 16 listopada 2026 roku Google Threat Intelligence Group opisało framework `exploit_agent`, który wykorzystywał wiele modeli AI do automatycznej analizy zmian w kodzie oprogramowania. System porównywał wersje przed i po opublikowaniu poprawek, szukał w różnicach potencjalnych podatności, przygotowywał exploity i automatycznie podejmował próby ich uruchomienia — bez człowieka w pętli.[^3]

To ważny dowód na automatyzację procesu poszukiwania i prób wykorzystania podatności. Trzeba jednak zachować rozróżnienie: Google nie podało, że exploity przygotowane przez ten konkretny framework skutecznie przełamały zabezpieczenia określonych systemów. Framework znaleziono na wystawionym serwerze stagingowym. Na tym samym serwerze znajdowały się również dane wyeksfiltrowane od ofiar i skrypty do działań po uzyskaniu dostępu, ale raport nie przypisuje wszystkich tych czynności samemu `exploit_agent`.[^3]

Innym przykładem jest `LAMEHUG`, złośliwe oprogramowanie opisane przez ukraiński zespół CERT-UA. Google śledzi tę rodzinę pod nazwą `PROMPTSTEAL`. Program korzystał z modelu `Qwen2.5-Coder-32B-Instruct` przez API Hugging Face, aby generować polecenia systemu Windows. Wygenerowane instrukcje były następnie wykonywane na zainfekowanym komputerze. Służyły między innymi do zbierania informacji o systemie i wyszukiwania dokumentów.[^4]

W tym przypadku AI nie podejmowała samodzielnie decyzji o rozpoczęciu całej kampanii. Była elementem mechanizmu złośliwego oprogramowania, który wykorzystywał wygenerowane polecenia w ramach wcześniej przygotowanego procesu.

**Automatyzacja nie jest tym samym co pełna autonomia.** Agent może wykonać wiele czynności bez bieżącego nadzoru, ale nadal działa w środowisku, z dostępem do narzędzi i w granicach wyznaczonych przez jego twórców lub operatorów.

Google zwraca uwagę, że takie rozwiązania mają również ograniczenia. Automatyczne działanie może pozostawiać charakterystyczne ślady, generować nadmiarowy ruch sieciowy i prowadzić do błędów wynikających z nieprawidłowych odpowiedzi modeli. Nie oznacza to jednak, że można je lekceważyć. Szybkość i możliwość łączenia wielu etapów w jeden proces stanowią realne wyzwanie dla obrony.[^3]

## Smartfon jako centrum cyfrowego życia

Smartfon jest dziś czymś więcej niż urządzeniem do komunikacji. Przechowuje dostęp do poczty, bankowości, komunikatorów, zdjęć, dokumentów i usług publicznych. Często służy również jako drugi składnik uwierzytelniania lub narzędzie odzyskiwania dostępu do innych kont.

Dlatego przejęcie telefonu, konta pocztowego albo aplikacji bankowej może mieć konsekwencje wykraczające poza pojedyncze urządzenie.

Warto przy tym rozdzielić zagrożenia wykorzystujące AI od klasycznego złośliwego oprogramowania. Nie każde oszustwo mobilne jest związane ze sztuczną inteligencją.

Przykładem jest kampania `NGate`, którą CERT Polska uznał za najgroźniejszą zaobserwowaną w 2025 roku kampanię mobilnego złośliwego oprogramowania wymierzoną w polskich użytkowników. Jej celem było umożliwienie nieuprawnionych wypłat z bankomatów bez fizycznego przejęcia karty.[^5]

Atak rozpoczynał się od wiadomości o rzekomym problemie z kontem lub bezpieczeństwem bankowym. Po instalacji aplikacji na Androidzie do ofiary dzwonił fałszywy pracownik banku i instruował ją, aby przyłożyła kartę do telefonu oraz wpisała PIN. Aplikacja przechwytywała komunikację `NFC` i przekazywała ją przez internet do urządzenia przestępcy znajdującego się przy bankomacie. Dzięki temu możliwe było odtworzenie komunikacji karty i wypłacenie gotówki.[^6]

To nie był atak oparty na AI. Jest jednak dobrym przykładem tego, jak wiele funkcji telefonu może zostać wykorzystanych przeciwko użytkownikowi, jeśli ten zostanie nakłoniony do zainstalowania złośliwej aplikacji i wykonania określonych czynności.

CERT Polska opisał również inne zagrożenia mobilne, między innymi aplikacje podszywające się pod znane usługi oraz oprogramowanie próbujące pozyskiwać dane z urządzenia. Wśród analizowanych przypadków znalazł się `SparkCat`, który wykorzystywał mechanizm `OCR`, czyli optycznego rozpoznawania tekstu na obrazach. Złośliwy kod analizował obrazy zapisane na urządzeniu i wyszukiwał w nich potencjalnie wrażliwe informacje, na przykład hasła lub frazy odzyskiwania portfeli kryptowalutowych.[^5]

Samo zastosowanie `OCR` nie oznacza jednak, że `SparkCat` wykorzystywał generatywną AI. Narzędzia OCR mogą być dziś łączone z rozwiązaniami opartymi na AI, ale raport CERT opisuje w tym przypadku mechanizm rozpoznawania tekstu na obrazach i wyszukiwania w nim wrażliwych informacji — nie autonomicznego agenta ani modelu językowego. To pokazuje, że zagrożenia mobilne nie potrzebują zaawansowanych modeli generatywnych, aby skutecznie wykorzystywać dane przechowywane na urządzeniu.

Wspólnym elementem wielu takich ataków jest wykorzystanie zaufania użytkownika: do marki, banku, komunikatu, aplikacji albo osoby, która kontaktuje się z nim przez telefon.

## Zagrożenie nie kończy się na telefonie

Perspektywa mobilna jest ważna, ale nie powinna przesłaniać szerszego obrazu. Dane użytkownika znajdują się również na serwerach usługodawców, w systemach firm, poczcie elektronicznej i usługach chmurowych.

Nawet dobrze zabezpieczony telefon nie ochroni przed każdym incydentem po stronie dostawcy usługi. Wyciek danych, przejęcie konta administratora albo wykorzystanie podatności serwera może dotknąć wielu użytkowników jednocześnie.

Raport ABW o stanie bezpieczeństwa cyberprzestrzeni RP w 2025 roku wskazuje na wykorzystanie AI między innymi do przygotowywania wiarygodniejszych materiałów phishingowych, automatyzacji rozpoznania podatności i tworzenia materiałów dezinformacyjnych. Jednocześnie raport zwraca uwagę na utrzymującą się rolę znanych problemów, takich jak niewdrażanie aktualizacji i podatność użytkowników na socjotechnikę.[^7]

Warto więc pamiętać, że bezpieczeństwo cyfrowe nie zależy wyłącznie od ustawień smartfona. Obejmuje również sposób działania usług, z których korzystamy, oraz to, jak chronią one nasze dane.

## Jak zadbać o siebie w cyfrowym świecie?

Nie istnieje jedna aplikacja ani ustawienie, które rozwiąże wszystkie problemy. Najwięcej daje połączenie kilku rozsądnych nawyków.

### Nie podejmuj decyzji pod presją

Wiadomości o zablokowanym koncie, problemie z płatnością, przesyłce lub pilnej potrzebie pomocy powinny skłaniać do sprawdzenia sytuacji, a nie do natychmiastowego działania.

Nie korzystaj z linków przesłanych w podejrzanej wiadomości. Otwórz oficjalną aplikację albo samodzielnie wpisz adres usługi. Jeśli ktoś prosi o pieniądze, kody lub dane logowania, potwierdź prośbę innym kanałem komunikacji.

W przypadku rozmowy z członkiem rodziny nie polegaj wyłącznie na głosie. Rozłącz się i zadzwoń na znany wcześniej numer. Można też ustalić rodzinne hasło lub inne pytanie, którego odpowiedź nie jest publicznie dostępna.

### Zabezpiecz najważniejsze konta

Zacznij od poczty elektronicznej. To często najważniejsze konto w całym ekosystemie, ponieważ umożliwia resetowanie haseł i odzyskiwanie dostępu do innych usług.

Używaj unikalnych haseł przechowywanych w menedżerze haseł. Włącz uwierzytelnianie wieloskładnikowe (`MFA`), a jeśli usługa obsługuje `passkeys`, czyli klucze dostępu, rozważ ich wykorzystanie. Pozwalają logować się bez wpisywania tradycyjnego hasła, zwykle przez potwierdzenie tożsamości odciskiem palca, rozpoznaniem twarzy lub kodem urządzenia. Klucz dostępu opiera się na mechanizmie kryptograficznym powiązanym z konkretną usługą i jej domeną, dlatego fałszywa strona logowania nie może po prostu przechwycić go tak jak hasła.

Warto wiedzieć, że nie wszystkie metody MFA zapewniają taki sam poziom ochrony. Kody jednorazowe wpisywane ręcznie, w tym kody z SMS lub aplikacji uwierzytelniającej, mogą zostać przechwycone przez fałszywą stronę logowania. Standard NIST wskazuje, że takie metody nie są odporne na phishing. Mechanizmy oparte na `FIDO2` i `WebAuthn` wiążą uwierzytelnienie z właściwą domeną, co utrudnia wykorzystanie ich na stronie podszywającej się pod prawdziwą usługę.[^8]

### Dbaj o telefon i aplikacje

Aktualizuj system operacyjny oraz aplikacje. Instaluj oprogramowanie z zaufanych źródeł i zwracaj uwagę na uprawnienia, których wymaga.

Aplikacja do prostego zadania nie powinna bez wyraźnego powodu potrzebować dostępu do SMS-ów, kontaktów, mikrofonu czy funkcji dostępności. Jeżeli coś budzi wątpliwości, lepiej zrezygnować z instalacji.

Nie ignoruj również ostrzeżeń systemowych. Aktualizacje często usuwają podatności, które mogą być wykorzystywane do atakowania urządzeń.

### Pamiętaj o kopiach zapasowych

Synchronizacja danych z chmurą nie zawsze jest tym samym co niezależna kopia zapasowa.

Przykładowo w `iCloud Photos` usunięcie zdjęcia na jednym urządzeniu powoduje jego usunięcie również na pozostałych urządzeniach korzystających z tej samej biblioteki. Zdjęcia synchronizowane w ten sposób nie są dodatkowo kopiowane do standardowej kopii zapasowej iCloud.[^9]

W Androidzie zakres kopii zapasowej zależy od systemu i aplikacji. Google zaznacza, że nie wszystkie aplikacje potrafią zapisać lub odtworzyć wszystkie swoje dane.[^10]

Warto więc sprawdzić, co faktycznie jest objęte kopią, i przechowywać dodatkową kopię najważniejszych plików w niezależnej lokalizacji. Przydatną zasadą jest `3-2-1`: trzy kopie danych, na dwóch różnych rodzajach nośników, z jedną kopią przechowywaną poza głównym miejscem pracy.[^11]

### Ogranicz skutki wycieku danych

Nie udostępniaj więcej informacji, niż jest to konieczne. Publicznie dostępne dane o rodzinie, pracy, podróżach czy miejscu zamieszkania mogą pomóc w przygotowaniu bardziej przekonującego oszustwa.

W Polsce warto również rozważyć zastrzeżenie numeru PESEL. Od 1 czerwca 2024 roku instytucje finansowe mają obowiązek sprawdzać jego status przy określonych czynnościach, między innymi przy zawieraniu umów kredytu i pożyczki. Zastrzeżenie nie blokuje zwykłych przelewów ani wypłat z bankomatu.[^12]

### Reaguj szybko, jeśli coś się wydarzy

Jeżeli podejrzewasz przejęcie konta lub wyłudzenie danych, nie czekaj. Skontaktuj się z bankiem, zablokuj dostęp lub kartę, jeśli istnieje takie ryzyko, zmień hasła i sprawdź aktywne sesje oraz podłączone urządzenia.

Podejrzane wiadomości SMS zawierające linki można przekazać do CERT Polska na numer `8080`. Podejrzane wiadomości e-mail można zgłosić przez formularz CERT Polska; jeśli to możliwe, warto przekazać je jako załącznik w formacie EML lub MSG, ponieważ zachowuje on nagłówki techniczne potrzebne do analizy.[^13]

## Podsumowanie

Sztuczna inteligencja zmienia cyberbezpieczeństwo, ale nie unieważnia dotychczasowych zasad ochrony. Przestępcy nadal wykorzystują phishing, kradzież danych, podatności i socjotechnikę. AI może sprawić, że część tych działań będzie szybsza, bardziej przekonująca i łatwiejsza do zautomatyzowania.

Pojawiają się już udokumentowane przypadki agentów realizujących kolejne etapy działań ofensywnych bez bieżącego udziału człowieka. Nie oznacza to jednak, że każdy cyberatak jest autonomiczny albo że użytkownik nie ma wpływu na własne bezpieczeństwo.

W praktyce nadal warto dbać o aktualizacje, unikalne hasła, odporne na phishing metody logowania, kopie zapasowe i ostrożność wobec niespodziewanych próśb. Szczególnie ważne jest, aby nie podejmować decyzji pod presją i weryfikować tożsamość rozmówców niezależnie od tego, jak wiarygodnie brzmią lub wyglądają.

Nie chodzi o to, żeby bać się każdej wiadomości i każdego nowego narzędzia. Chodzi o świadome korzystanie z technologii i ograniczanie skutków błędów, które mogą się zdarzyć każdemu.

## Przypisy

[^1]: FBI Internet Crime Complaint Center. (2026, July 20). *FBI Warns of Scammers Impersonating the IC3*. [Link](https://www.ic3.gov/PSA/2026/PSA260720)

[^2]: Federal Trade Commission. (2026, November 18). *“Grandpa, I need your help!” How to spot and avoid grandkid scams*. [Link](https://consumer.ftc.gov/consumer-alerts/2026/11/grandpa-i-need-your-help-how-spot-and-avoid-grandkid-scams)

[^3]: Google Threat Intelligence Group. (2026, November 16). *GTIG AI Threat Tracker: Agentic Threat Activity Crosses a Significant Threshold*. [Link](https://cloud.google.com/blog/topics/threat-intelligence/agentic-threat-activity-crosses-a-significant-threshold)

[^4]: Google Threat Intelligence Group. (2025, November 5). *GTIG AI Threat Tracker: Advances in Threat Actor Usage of AI Tools*. [Link](https://cloud.google.com/blog/topics/threat-intelligence/threat-actor-usage-of-ai-tools)

[^5]: CERT Polska. (2026, April 8). *Raport roczny z działalności CERT Polska w 2025 roku*. [Link](https://cert.pl/uploads/docs/Raport_CP_2025.pdf)

[^6]: Ratajczak, K. (2025, November 3). *Analiza kampanii złośliwego oprogramowania NGate (NFC relay)*. CERT Polska. [Link](https://cert.pl/posts/2025/11/analiza-ngate/)

[^7]: Agencja Bezpieczeństwa Wewnętrznego, CSIRT GOV. (2026, September 18). *Raport o stanie bezpieczeństwa cyberprzestrzeni RP w 2025 roku*. [Link](https://www.abw.gov.pl/pl/aktualnosci/2841,Raport-o-stanie-bezpieczenstwa-cyberprzestrzeni-RP-w-2025-roku.html)

[^8]: National Institute of Standards and Technology. (2025, July 31). *Digital Identity Guidelines: Authentication and Authenticator Management (SP 800-63B-4)*. [Link](https://pages.nist.gov/800-63-4/sp800-63b.html)

[^9]: Apple. (n.d.). *Set up and use iCloud Photos*. [Link](https://support.apple.com/en-us/108782)

[^10]: Google. (n.d.). *Back up or restore data on your Android device*. [Link](https://support.google.com/android/answer/2819582)

[^11]: European Union Agency for Cybersecurity (ENISA). (2025, June). *Technical Implementation Guidance on Commission Implementing Regulation (EU) 2024/2690*, version 1.0. [Link](https://www.enisa.europa.eu/sites/default/files/2025-06/ENISA_Technical_implementation_guidance_on_cybersecurity_risk_management_measures_version_1.0.pdf)

[^12]: Ministerstwo Cyfryzacji. (n.d.). *Zastrzeż swój numer PESEL lub cofnij zastrzeżenie*. [Link](https://www.gov.pl/web/gov/zastrzez-swoj-numer-pesel-lub-cofnij-zastrzezenie)

[^13]: CERT Polska. (n.d.). *Zgłaszanie incydentów przez formularz na stronie incydent.cert.pl*. [Link](https://wiedza.cert.pl/obsluga-incydentow/zgloszenie/incydent-cert-pl/). Informacje o przekazywaniu podejrzanych wiadomości SMS na numer 8080: [gov.pl](https://www.gov.pl/web/baza-wiedzy/dostales-niepokojacy-sms-albo-email-zglos-go-do-cert-polska-csirt-nask)
