+++
title = "Lekcja prywatności z Poznania. Czego uczy nas \"afera\" wokół Pyrkonu?"
date = 2026-06-22
description = "Analiza techniczna i prawna tzw. afery wokół Pyrkonu. Jak działają brokerzy danych, dlaczego RODO nie wystarcza i jak samodzielnie zadbać o cyfrową higienę."

[extra]
toc = true
+++

Afera wokół rzekomej inwigilacji uczestników festiwalu Pyrkon wywołała burzę w sieci. Zamiast poddawać się emocjom i politycznym przepychankom opartym na wyrwanych z kontekstu słowach urzędników, spójrzmy na ten problem z chłodną, inżynierską głową. Okazuje się, że prawdziwe zagrożenie dla naszej prywatności leży zupełnie gdzie indziej.

Choć w sieci dostępny jest [pełny, dwugodzinny zapis tego posiedzenia](https://video.strona-czynna.pl/w/6Xo1ftLDrfTFFcvQ46REq5), większość komentujących oparła swoje oburzenie wyłącznie na wyrwanym z kontekstu fragmencie. W internetowych debatach natychmiast wybuchło oburzenie, podsycone bezpośrednio przez samych autorów medialnego zamieszania. Profile, które nagłośniły sprawę, zaczęły wybiórczo podbijać wyłącznie te komentarze, które wpisują się w sensacyjną narrację o nielegalnym szpiegowaniu obywateli i rażącym łamaniu przepisów `RODO`. Głosy merytoryczne, precyzyjnie wyjaśniające techniczną naturę przetwarzania danych czy mechanizmy komercyjnych zgód u operatorów są w tej dyskusji systemowo ignorowane i pomijane, byle tylko utrzymać wysoki poziom emocji. Kiedy jednak te emocje opadną i spojrzymy na mechanizmy rynkowe z chłodną, techniczną głową, okazuje się, że problem leży zupełnie gdzie indziej. I jest on o wiele poważniejszy niż lokalne, polityczne przepychanki oparte na słowach urzędników.

## 1. Jak powstają takie raporty, czyli makro vs mikro

W oficjalnym oświadczeniu wydanym przez `PLOT` organizacja wprost odniosła się do burzy medialnej wywołanej wyciętym fragmentem nagrania. Prezes zarządu, Jan Mazurczak, jednoznacznie potwierdził w nim techniczną naturę badania: miasto nie kupuje bilingów, numerów PESEL ani nazwisk. Samorządy i lokalne organizacje turystyczne kupują po prostu gotowe, w pełni zagregowane i zanonimizowane produkty statystyczne od firm zewnętrznych – w tym przypadku od wyspecjalizowanej firmy analitycznej `Selectivv`.

Warto wyraźnie rozróżnić dwie sytuacje:

1. Dla firmy analitycznej identyfikatory urządzeń mobilnych to dane, które przetwarza ona na podstawie zgód pobieranych w tysiącach darmowych aplikacji. Brokerzy danych muszą pilnować procedur i dbać o własne zaplecze prawne.
2. Dla `Poznańskiego Urzędu Miasta` czy `PLOT` sytuacja wygląda zupełnie inaczej. Żadne unikatowe identyfikatory czy dane osobowe do urzędników nie trafiają. Miasto kupuje wyłącznie suche, zbiorcze wykresy procentowe, tabele i trendy.

Żaden analityk nie łączy prywatnych wiadomości z telefonu konkretnego uczestnika z jego osobistym kontem bankowym, bo takie działanie byłoby przestępstwem. W publicznych debatach urzędnicy często używają skrótów myślowych, mówiąc ogólnie o „zliczaniu telefonów" czy „danych od operatorów" (co mogłoby sugerować namierzanie kart SIM przez maszty `BTS`). W rzeczywistości – jak potwierdza oświadczenie `PLOT` – głównym motorem tych analiz jest ekosystem reklamy mobilnej.

Analizy opierają się na anonimowych identyfikatorach reklamowych urządzeń (`MAID`). Dla systemów analitycznych Twój smartfon to po prostu anonimowa kropka przemieszczająca się po mapie. Jeśli algorytm widzi, że tysiące takich kropek rano znajdowały się na terenie `Międzynarodowych Targów Poznańskich`, a po południu przemieściły się w rejon konkretnych hoteli czy restauracji, po prostu łączy te punkty, zliczając masowe trendy.

Podobnie wygląda kwestia szacowania wydatków – to średnia `estymacja ekonometryczna`. Ogólny, odnotowany przez partnerów finansowych wzrost obrotów w handlu i gastronomii w danym rejonie dzieli się przez szacowaną liczbę nadmiarowych turystów. Z perspektywy prawa wszystko jest czyste. `PLOT` wskazuje, że model przetwarzania danych przez `Selectivv` posiada audyt niezależnej firmy `Deloitte Consulting S.A.`, który potwierdza pełną zgodność z `RODO` oraz brak możliwości identyfikacji konkretnych osób fizycznych.

## 2. Gdzie leży prawdziwy problem? Historia zatacza koło

Skoro od strony prawa wszystko jest w porządku, to dlaczego sprawa wciąż budzi tak duży niepokój? Ponieważ cała ta sytuacja obnaża brutalną prawdę o współczesnym internecie: prawdziwym źródłem danych dla brokerów informacji nie są urzędy miast, ale smartfony, które codziennie nosimy w kieszeniach, a dokładniej oprogramowanie, które sami na nich instalujemy.

Co ciekawe, dokładnie z tą samą sytuacją mieliśmy do czynienia już wiele lat temu. Jak przypominał serwis **`Niebezpiecznik.pl`** w swoim głośnym artykule *„Za darmowe aplikacje mobilne płacisz informacjami o sobie"*, ta sama firma – `Selectivv` – wywołała potężny skandal po festiwalu `Open'er 2017`. Analizując smartfony uczestników tamtego wydarzenia, firma opublikowała infografiki pokazujące między innymi, ile procent festiwalowiczów „stara się o dziecko" (wywnioskowane przez analityków na podstawie profilu zachowań oraz używania specyficznych aplikacji zdrowotnych czy ciążowych) albo w jakich dyskontach najczęściej robią zakupy.

Fakt, że po niemal dekadzie, wdrożeniu restrykcyjnego unijnego prawa `RODO` i setkach debat o prywatności dokładnie te same mechanizmy i ta sama firma nadal z powodzeniem analizują nasze zachowania, pokazuje jedno: systemowe i prawne próby ochrony naszej prywatności, w oderwaniu od świadomości użytkownika, to fikcja.

Większość darmowych programów, prostych gier, aplikacji pogodowych czy latarek utrzymuje się z tego, że w ich kodzie zaszyte są pakiety reklamowe i analityczne (tak zwane `SDK`). To właśnie te komponenty bez przerwy odczytują unikalny identyfikator reklamowy Twojego telefonu i potrafią wysyłać go w świat razem z dokładną lokalizacją `GPS`. Choć duże platformy zakupowe (jak niezwykle popularne `Temu`) zbierają potężne ilości danych głównie na własny użytek, to właśnie setki mniejszych, darmowych aplikacji stanowią główny silnik zasilający bazy zewnętrznych hurtowni danych.

Użytkownicy w sieci często podnoszą argument: „Przecież to zgody wymuszone! Jeśli chcę mieć telefon czy konto w banku, muszę podpisać umowę, więc nie ma tu mowy o uczciwym wyborze". Niestety, od strony formalnej i biznesowej rzeczywistość bywa jeszcze bardziej cyniczna. Operatorzy telekomunikacyjni, budując własne narzędzia analityczne (na przykład `Orange Insights`), wykorzystują logowania do stacji bazowych (`BTS`). Po pełnej anonimizacji danych (gdy algorytm zlicza tylko suche statystyki typu „w tym kwadrancie zalogowało się 500 kart SIM"), `RODO` przestaje mieć zastosowanie, bo z wykresu zbiorczego nie da się zidentyfikować żywego człowieka.

Co więcej, operatorzy stosują prosty mechanizm psychologiczny: oferują stałą zniżkę na fakturze (na przykład `5 zł miesięcznie`) w zamian za zaznaczenie zgód marketingowych i analitycznych przy podpisywaniu umowy lub w aplikacji. Klient kalkuluje szybko i wybiera tańszy abonament. W ten sposób ludzie dosłownie sprzedają historię swojej lokalizacji za parę złotych rabatu, dając telekomom pełne prawo do komercyjnej analizy tego ruchu, a potem w internetowej przestrzeni wybucha zdziwienie, że firmy z tych zgód korzystają. Oficjalnie można te zgody wycofać – ale wtedy traci się rabat.

## 3. Sprawdź swój telefon: Exodus Privacy

Zamiast teoretyzować, szukać spisków lub opierać się na emocjach wywołanych wypowiedziami polityków, warto zejść na poziom twardych faktów technicznych. Każdy może samodzielnie zweryfikować, co tak naprawdę dzieje się wewnątrz własnego smartfona.

Doskonale nadaje się do tego platforma **`Exodus Privacy`** ([exodus-privacy.eu.org](https://exodus-privacy.eu.org)). To w pełni darmowy i niezależny projekt prowadzony przez francuską organizację pożytku publicznego, który pozwala przeanalizować niemal każdą aplikację dostępną w sklepie `Google Play` pod kątem wbudowanych elementów śledzących oraz wymaganych uprawnień.

Wystarczy wpisać na stronie głównej projektu lub w bazie `reports.exodus-privacy.eu.org` nazwę dowolnego programu, z którego korzystasz na co dzień, aby czarno na białym zobaczyć listę wbudowanych trackerów reklamowych i analitycznych. To potężne źródło wiedzy, które potrafi otworzyć oczy i skutecznie wyleczyć z technologicznej naiwności. Ludzi trzeba uświadamiać i pokazywać im te mechanizmy merytorycznie, zamiast zamieniać temat prywatności w polityczną burzę.

## 4. Cyfrowa higiena zamiast skarg do radnych

Oficjalna odpowiedź `PLOT` dobitnie pokazuje jedno: nie ma sensu liczyć na odgórne zmiany systemowe, nowe ustawy czy rewolucje w prawie. Wszystko, co się wydarzyło, odbyło się legalnie, zgodnie z procedurami i pod osłoną unijnych przepisów. Prawo zawsze będzie o krok za technologią, a korporacje zawsze znajdą sposób, by napisać regulamin i warunki korzystania z usług (`TOS`) na pięćdziesiąt stron drobnym drukiem, którego nikt nie przeczyta.

Skoro oficjalne systemy zgód i prawo `RODO` chronią przede wszystkim urzędy i firmy od strony formalnej, jedyną skuteczną barierą ochronną przed masowym profilowaniem pozostaje nasza własna, codzienna `higiena cyfrowa`. Musimy zacząć działać u samego źródła. Narzędzia obrony mamy we własnych rękach.

Co możesz zrobić na swoim telefonie od zaraz?

- **`Kontroluj i cofaj uprawnienia`**: Wejdź w ustawienia systemowe swojego smartfona i bezwzględnie zablokuj dostęp do lokalizacji `GPS`, kontaktów, aparatu czy pamięci urządzenia wszystkim aplikacjom, które nie potrzebują tego do swojego podstawowego działania. Aplikacja z przepisami kulinarnymi czy latarka nie muszą wiedzieć, gdzie aktualnie przebywasz.
- **`Odcinaj dostęp do sieci aplikacjom offline`**: Programom, które działają całkowicie lokalnie (takim jak proste gry jednoosobowe, kalkulatory, dyktafony czy lokalne odtwarzacze muzyki), warto całkowicie odebrać uprawnienie do korzystania z transmisji danych i `Wi-Fi`. Gra nadal będzie działać idealnie, ale zaszyte w niej skrypty reklamowe stracą techniczną możliwość wysłania jakichkolwiek informacji na serwery zewnętrznych firm.
- **`Blokuj działanie w tle`**: Odbierz uprawnienia do pracy w tle aplikacjom, które nie muszą bez przerwy nasłuchiwać na zablokowanym ekranie. Jeśli program nie służy do komunikacji i nie musi dostarczać natychmiastowych powiadomień, powinien uruchamiać się tylko wtedy, gdy świadomie go otwierasz, a po wyjściu – natychmiast kończyć swoją aktywność.
- **`Zresetuj lub usuń identyfikator reklamowy`**: W ustawieniach prywatności systemu `Android` (sekcja *Prywatność* -> *Reklamy*) znajdziesz opcję odpowiedzialną za Twój identyfikator reklamowy (`GAID`) – możesz go w każdej chwili zresetować lub całkowicie usunąć. Z kolei w systemie `iOS` (`Apple`) warto wejść w sekcję *Prywatność i bezpieczeństwo* -> *Śledzenie* i całkowicie zabronić aplikacjom wysyłania żądań śledzenia, co odetnie im dostęp do Twojego profilu reklamowego (`IDFA`).
- **`Wybieraj otwarte oprogramowanie (FOSS)`**: Kiedy to tylko możliwe, zastępuj darmowe, własnościowe aplikacje zbierające dane ich otwartymi, bezpiecznymi alternatywami, które nie posiadają wbudowanych modułów śledzących.

Prawdziwa walka o prywatność nie rozgrywa się na sesji rady miasta, w komitetach politycznych ani w sekcji komentarzy na portalach społecznościowych. Ona rozgrywa się w menu ustawień Twojego własnego smartfona. Zamiast bezrefleksyjnie klikać „akceptuję wszystko" dla własnej wygody, dla kilku złotych zniżki lub w naiwnej wierze, że przed tym wszystkim obroni nas `RODO`, zacznij świadomie kontrolować to, co Twoje urządzenie wysyła w świat. Zamiast robić medialny wir w szklance wody i licytować się na oburzenie, o wiele lepiej zacząć rzetelnie edukować ludzi: pokazywać im, jakie konkretnie informacje, w jaki sposób i przez kogo są o nich zbierane, a przede wszystkim jak skutecznie i samodzielnie się przed tym bronić.
