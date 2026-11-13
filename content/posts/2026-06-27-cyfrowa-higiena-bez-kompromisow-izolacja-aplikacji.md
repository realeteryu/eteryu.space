+++
title = "Cyfrowa higiena bez kompromisów. Jak łatwo odizolować toksyczne aplikacje?"
date = 2026-06-27
description = "Jak zbudować potrójną zaporę sieciową z RethinkDNS i odizolować toksyczne aplikacje za pomocą Private Space lub profili użytkowników, zachowując pełną kontrolę."

[extra]
toc = true
+++

W poprzednim artykule omówiliśmy podstawowe nawyki i ustawienia systemowe. Czas pójść o krok dalej i wytoczyć ciężkie działa. Przejmiemy pełną kontrolę nad siecią oraz systemem, zmuszając najbardziej bezczelne aplikacje do gry na naszych warunkach. Zaczynamy od totalnej blokady komunikacji i systemowej izolacji.

## 1. RethinkDNS jako sieciowy szwajcarski scyzoryk (3w1)

Sam używałem tego rozwiązania jako głównej tarczy podczas walki z inwigilacją przez korporacyjne MDM. Całą konfigurację wyklikałem w zaledwie dziesięć minut na tylnym siedzeniu Bolta, jadąc do żony do szpitala. W czasie jednego krótkiego kursu odciąłem sieć wszystkim aplikacjom, tworząc restrykcyjną białą listę i spinając całość z bezpiecznym DNS od Quad9.

Jak powtórzyć ten wyczyn na własnym telefonie?

### Problem: Androidowy konflikt interesów
Fabryczny Android ma jedną dużą wadę architektoniczną, ponieważ pozwala na uruchomienie tylko **jednego tunelu VPN na raz**. Chcesz zablokować aplikacji X dostęp do sieci za pomocą firewallu? System stawia lokalny VPN. Chcesz zaszyfrować ruch i zmienić IP na przykład przez `Mullvada`? Musisz włączyć kolejny VPN, który natychmiastowym ruchem wyłączy ten pierwszy.

Efekt jest taki, że albo chronisz swoją prywatność w drodze do serwera, albo kontrolujesz, co ucieka z telefonu. Nie dało się mieć ciasteczka i zjeść ciasteczka. Do teraz.

### Rozwiązanie: Jeden proces, by wszystkimi rządzić
Aplikacja `RethinkDNS` rozwiązuje ten problem genialnie. Działa jak centralny hub, który bierze na siebie całą komunikację sieciową telefonu, ale wewnątrz pozwala Ci skonfigurować trzy niezależne warstwy. Dopiero gdy wszystko poustawiasz, aplikacja odpala jeden lekki tunel systemowy.

Oto jak skonfigurować te trzy pieczenie na jednym ogniu:

* **Warstwa pierwsza, czyli DNS (Zanim wyjdzie zapytanie):** W zakładce `DNS` nie musisz polegać na domyślnych serwerach swojego operatora komórkowego. Możesz wybrać wbudowane w Rethinka listy blokujące syf albo podpiąć zewnętrzny adres, na przykład bezpieczny `9.9.9.9` od `Quad9` czy `NextDNS`. Każde zapytanie o domenę reklamową zostanie ubite na starcie.
* **Warstwa druga, czyli Firewall (Kto ma prawo mówić):** Przechodzisz do zakładki `Apps`. Tutaj zaczyna się absolutna władza. Możesz włączyć tryb `Whitelisting`, który blokuje wszystko domyślnie oprócz wybranych aplikacji, dokładnie tak jak zrobiłem to w taksówce. Możesz też po prostu przejrzeć listę i odciąć internet kalkulatorowi, czytnikowi PDF czy gierce offline.
* **Warstwa trzecia, czyli Proxy i VPN (Jak bezpiecznie lecieć w świat):** To tutaj dzieje się magia. Nie musisz instalować osobnej aplikacji od dostawcy VPN. Wchodzisz w sekcję `Proxy`, wybierasz `WireGuard` i importujesz zwykły plik konfiguracyjny `.conf`, który pobierasz ze swojej strony profilowej na przykład w `Mullvadzie` czy `Protonie`.

### Finał: Klikasz Start i zapominasz
Kiedy przejdziesz przez te trzy zakładki, wracasz na ekran główny Rethinka i klikasz wielki przycisk uruchomienia. System tworzy jeden tunel.

Od tej sekundy proces wygląda tak: toksyczna aplikacja próbuje wysłać paczkę danych, firewall w locie sprawdza, czy ma na to zgodę, a jeśli tak, DNS filtruje czy adres nie jest trackerem. Na koniec czysty ruch zostaje zapakowany w bezpieczny tunel `WireGuard` i wysłany w świat.

Wszystko dzieje się w ułamku sekundy, w jednym procesie, bez kłótni systemowych i bez katowania baterii kilkoma programami działającymi w tle.

## 2. Cyfrowa kwarantanna na toksyczne aplikacje

Są takie aplikacje, których po prostu potrzebujesz do życia lub pracy, chociaż doskonale wiesz, że ich kod pisali specjaliści od wysysania danych. `Microsoft Teams`, `Facebook`, `Messenger`, aplikacje linii lotniczych czy popularne dyskonty spożywcze. Wszystkie one potrafią bezwstydnie żądać dostępu do Twoich kontaktów czy całej galerii zdjęć, a jeśli im odmówisz, potrafią być niesamowicie upierdliwe lub wręcz odmawiają posłuszeństwa. Zamiast bawić się w hakowanie uprawnień przez komputer, możesz użyć genialnego mechanizmu izolacji, który Twój telefon ma już wbudowany pod maską. Do wyboru masz dwie metody, w zależności od tego, jak bardzo chcesz odciąć grubą kreską toksyczne programy.

### Ścieżka pierwsza, czyli najwygodniejsza metoda wewnątrz Twojego profilu
Jeśli masz nowszego Androida z funkcją `Private Space` lub używasz telefonu Samsung z funkcją `Bezpieczny Folder`, konfiguracja jest banalnie prosta. Wchodzisz w ustawienia bezpieczeństwa systemu, aktywujesz tę osobną przestrzeń i zabezpieczasz ją osobnym kodem PIN lub odciskiem palca. Od tej chwili na dole listy aplikacji pojawia się specjalna, zablokowana sekcja.

Konfiguracja aplikacji polega po prostu na zainstalowaniu jej bezpośrednio wewnątrz tej ukrytej przestrzeni. Kiedy to zrobisz, system odpala dla niej kompletnie czyste środowisko. Taki program ma swoją osobną pamięć podręczną i nie widzi Twoich prywatnych zdjęć ani kontaktów z głównego profilu. Jeśli nienasytny komunikator wewnątrz prywatnej przestrzeni zażąda dostępu do książki adresowej, dajesz mu zgodę bez mrugnięcia okiem. Aplikacja przeszuka bazę danych, zobaczy cyfrowe nic, nie zgłosi błędu i pozwoli Ci działać. Myśli, że dostała to, czego chciała, a w rzeczywistości została całkowicie oszukana. Ty z kolei zyskujesz spokój. W przypadku Samsunga powiadomienia o nowych wiadomościach zobaczysz na głównym ekranie w dyskretnej, ukrytej formie. Z kolei na czystym Androidzie z funkcją Private Space aplikacje zostaną całkowicie zamrożone i nie będą zawracać Ci głowy ani zużywać baterii, dopóki sam nie zdecydujesz się odblokować swojej prywatnej przestrzeni.

### Ścieżka druga, czyli pełna separacja przez osobny profil użytkownika
Jeśli Twój telefon nie ma jeszcze funkcji `Private Space`, możesz użyć klasycznych profili użytkowników, które działają dokładnie tak jak konta dla różnych osób na domowym komputerze. Wchodzisz w ustawienia systemowe, wybierasz sekcję `Użytkownicy` i klikasz `Dodaj użytkownika`. Przełączanie między nimi zajmuje jedno kliknięcie w górnej belce powiadomień.

Konfiguracja tej metody wymaga odrobiny cierpliwości, ponieważ system tworzy wtedy drugi, fabrycznie nowy telefon wewnątrz starego. Przełączasz się na nowy profil i instalujesz tam aplikacje. Ta metoda daje najbardziej bezwzględną izolację ze wszystkich możliwych, ponieważ aplikacje z drugiego profilu są całkowicie zamrożone i nie mają żadnego kontaktu z Twoim głównym kontem. Haczyk polega na tym, że dopóki nie przełączysz się ręcznie na ten drugi profil, nie zobaczysz pochodzących z niego powiadomień. To genialne rozwiązanie na aplikacje służbowe, które chcesz kompletnie wyciszyć po godzinach pracy.

Niezależnie od wybranej ścieżki, efekt końcowy jest ten sam. Toksyczna aplikacja ląduje w kontrolowanej piaskownicy, a Ty odzyskujesz prywatność bez rezygnacji z narzędzi niezbędnych do codziennego funkcjonowania.

## Podsumowanie: Twoje własne zasady

Wprowadzenie tych dwóch mechanizmów w życie zajmie Ci łącznie kilkanaście minut, a diametralnie zmieni zasady gry. Łącząc `RethinkDNS` z systemową izolacją (`Private Space` lub `Profilami użytkowników`), tworzysz wokół swoich danych potężny mur o dwóch niezależnych warstwach. Pierwsza warstwa pilnuje, by z Twojego urządzenia nie wypłynął żaden nieautoryzowany pakiet informacji. Druga dba o to, by aplikacje, na które jesteś skazany, żyły w cyfrowej niewiedzy i były całkowicie odcięte od Twojej prywatności.

Smartfon ma być narzędziem, które ułatwia Ci codzienne funkcjonowanie, a nie szpiegiem pracującym na etacie u brokerów danych. Najlepsze jest to, że nie potrzebujesz do tego skomplikowanego hakowania systemu. Wszystkie te opcje masz już pod maską – wystarczy po prostu zacząć z nich korzystać na własnych warunkach.
