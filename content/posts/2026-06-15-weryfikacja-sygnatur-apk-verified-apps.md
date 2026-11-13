+++
title = "Weryfikacja sygnatur APK. Poznaj Verified Apps, oficjalny projekt od Privacy Guides"
description = "Porównanie trzech narzędzi do weryfikacji sygnatur APK: AppVerifier, Verified Apps od Privacy Guides i AppVerifierBG. Które wybrać i dlaczego."
date = 2026-06-15

[extra]
toc = true
+++

Pobieranie aplikacji bezpośrednio z GitHuba daje pełną kontrolę, ale stanowi spore wyzwanie dla bezpieczeństwa. Skąd mamy wiedzieć, czy w repozytorium nikt nie podmienił pliku? Odpowiedzią jest weryfikacja sygnatur cyfrowych. Oto starcie trzech narzędzi: AppVerifier, Verified Apps od Privacy Guides oraz hybrydowego AppVerifierBG.

Przez długi czas podstawową linią obrony dla wielu użytkowników był `AppVerifier`, czyli świetne, lokalne narzędzie od dewelopera soupslurpr. Ostatnio jednak na scenie pojawił się nowy gracz. Mowa o `Verified Apps`, czyli oficjalnym projekcie bazującym na kodzie AppVerifiera, ale sygnowanym przez ekipę z Privacy Guides.

Każda aplikacja, która pozwala na realną weryfikację kodu w ramach modelu zero trust, jest warta uwagi, a ten konkretny projekt rozwija się obecnie naprawdę prężnie. Ma ambicję mocno uprościć proces kontroli cyfrowych sygnatur.

## Dwie filozofie, jeden cel

Choć pod maską oba programy robią dokładnie to samo, czyli kalkulują hashe SHA256 przed właściwą instalacją, reprezentują zupełnie inne podejście do wygody użytkownika.

- **AppVerifier** stawia na twardą decentralizację. Aplikacja bazuje na czystej matematyce. Jeśli nie zna danego hasha, po prostu informuje, że nie ma go w bazie. Wtedy cała odpowiedzialność spada na użytkownika. Trzeba ręcznie przekopać dedykowany wątek na forum GrapheneOS albo stronę dewelopera, skopiować skrót certyfikatu i zweryfikować go za pomocą wbudowanej funkcji schowka. Bywa to męczące na dłuższą metę.
- **Verified Apps** idzie w stronę automatyzacji i crowdsourcingu, ale w bardzo bezpiecznym, lokalnym wydaniu. Zamiast pobierać bazę dynamicznie z sieci przy każdym uruchomieniu, projekt od Privacy Guides zaszywa całe crowdsourcingowe repozytorium bezpośrednio wewnątrz aplikacji. Baza jest sprawdzana całkowicie lokalnie, a jej aktualizacja następuje wraz z nowym wydaniem programu. Zielony ekran oznacza, że podpis znajduje się w tym oficjalnym spisie, a czerwony alarmuje o niezgodności lub braku rekordu.

## Automatyzacja, która nie odbiera kontroli

W tym miejscu warto na chwilę się zatrzymać, żeby nie wpaść w pułapkę własnych paradygmatów. Krytyka korporacyjnego modelu technologii często opiera się na tym, że rozleniwił nas funkcjami ułatwiającymi bezrefleksyjne korzystanie ze sprzętu, odbierając nam w zamian kontrolę nad własnymi danymi. Czy zachwyt nad automatyzacją Verified Apps nie jest więc przejawem niekonsekwencji?

Nie, ponieważ istnieje fundamentalna różnica w architekturze obu tych rozwiązań.

> Automatyzacja Big Techu to czarna skrzynka. Ma być bezobsługowo po to, aby nikt nie pytał, co dzieje się w tle i jakie dane telemetryczne opuszczają telefon.

Automatyzacja w wydaniu Verified Apps jest w pełni transparentna. Opiera się na otwartym kodzie i jawnej bazie danych na GitHubie, którą każdy z nas może zweryfikować, a nawet współtworzyć. Narzędzie od Privacy Guides nie podejmuje decyzji za użytkownika i nie ukrywa procesów. Ono po prostu zdejmuje z niego powtarzalną, mechaniczną pracę polegającą na ręcznym sprawdzaniu ciągów znaków. Ostateczna kontrola i rygor modelu zero trust nadal zostają po stronie użytkownika.

## Pułapka jednego wzorca

Ta automatyzacja ma jednak na tym etapie rozwoju swoje wady, co widać chociażby przy próbach instalacji i aktualizacji programów takich jak `Obsidian`.

Wersja Obsidianu dystrybuowana przez Google Play posiada inny podpis cyfrowy niż oficjalne wydanie Standalone APK, które twórcy wrzucają bezpośrednio na GitHuba. Choć baza Verified Apps docelowo agreguje podpisy z różnych źródeł, w tym z GitHuba, F-Droida czy Play Store, jako młody projekt nie ma jeszcze w swoich plikach absolutnie wszystkiego. Przy próbie aktualizacji Obsidianu z poziomu zewnętrznego instalatora system potrafi przywitać użytkownika twardym komunikatem o nieznanej sygnaturze.

W starym AppVerifierze można w takiej sytuacji po prostu wkleić zaufany hash ze schowka, żeby aplikacja dokonała matematycznego porównania na miejscu. Verified Apps to projekt świadomie odchudzony, z którego całkowicie usunięto funkcję porównywania poprzez schowek. Owszem, program pozwala łatwo skopiować lub udostępnić wyliczony hash na zewnątrz, ale samej weryfikacji ze schowkiem wewnątrz aplikacji już nie przeprowadzimy. Jeśli trafimy na prawidłową aplikację, której brakuje w bazie, jedyną opcją automatyczną jest użycie wbudowanej funkcji i wygenerowanie gotowego zgłoszenia na GitHubie projektu, aby dołożyć swoją cegiełkę do rozwoju tego narzędzia.

## Trzecia droga: AppVerifierBG

Dla osób, które nie chcą rezygnować z wygody automatycznej bazy danych, ale jednocześnie potrzebują pełnej funkcjonalności manualnej, na horyzoncie pojawia się ciekawa alternatywa w postaci `AppVerifierBG` (forka tworzonego przez użytkownika RoundSalmon4).

Projekt ten skutecznie łączy oba światy. Z jednej strony zachowuje tradycyjne funkcje oryginału, w tym niezwykle przydatne sprawdzanie i porównywanie podpisów bezpośrednio ze schowka. Z drugiej strony pozwala na zaciągnięcie i zintegrowanie zewnętrznych baz danych (w tym tej stworzonej przez Verified Apps) oraz budowanie własnych, w pełni lokalnych zestawów zaufanych sygnatur przez samego użytkownika.

## Podsumowanie

Automatyzacja od Privacy Guides nie jest jeszcze idealna, ale potencjał społeczności jest ogromny. Rynek narzędzi do weryfikacji sygnatur APK dynamicznie się rozwija, a możliwość wyboru między surową matematyką, centralną bazą a hybrydowymi rozwiązaniami pokroju AppVerifierBG to świetna wiadomość dla każdego, kto poważnie podchodzi do bezpieczeństwa swojego systemu. Jeśli używasz Androida, niezależnie od tego, czy to GrapheneOS, LineageOS czy zwykły system, warto zainstalować `Verified Apps` z GitHub Releases i sprawdzić, czy Twoje kluczowe programy są w bazie. To najprostszy sposób, żeby dołożyć cegiełkę do ekosystemu.
