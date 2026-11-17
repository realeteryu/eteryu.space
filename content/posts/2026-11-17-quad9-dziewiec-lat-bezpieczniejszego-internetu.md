+++
title = "Quad9 – dziewięć lat walki o bezpieczniejszy i bardziej prywatny internet"
date = 2026-11-17
description = "Dziewięć lat Quad9: historia projektu, DNS Threat Intelligence, prywatność, warianty resolverów, DNSSEC i spór o blokowanie domen we Francji."
[taxonomies]
tags = ["DNS", "bezpieczeństwo", "prywatność", "Quad9"]
+++

Dziewiątka jest dla Quad9 czymś więcej niż częścią nazwy. To również adres, pod którym działa najbardziej rozpoznawalny wariant usługi: `9.9.9.9`. W listopadzie 2026 roku mija dziewięć lat od uruchomienia publicznego resolvera DNS, którego celem było połączenie ochrony przed zagrożeniami internetowymi z poszanowaniem prywatności użytkowników.

Quad9 to publiczna usługa rekurencyjnego DNS, która wykorzystuje informacje o zagrożeniach do blokowania domen powiązanych między innymi z phishingiem, złośliwym oprogramowaniem i infrastrukturą wykorzystywaną w cyberatakach.[^1]

Przez lata Quad9 stał się jedną z rozpoznawalnych alternatyw dla resolverów dostarczanych przez operatorów internetowych i dużych dostawców usług. Wcześniej sam korzystałem z tej usługi. Obecnie używam własnego resolvera, ale Quad9 nadal uważam za sprawdzone rozwiązanie, które warto znać i rozważyć.

## Początki Quad9

Quad9 został uruchomiony w listopadzie 2017 roku jako wspólna inicjatywa IBM, Packet Clearing House (PCH) i Global Cyber Alliance (GCA). Projekt miał udostępnić użytkownikom bezpłatny resolver DNS, który wykorzystuje informacje o zagrożeniach pochodzące od wyspecjalizowanych dostawców Threat Intelligence.[^2]

Każda z organizacji wniosła do projektu inne zasoby:

- **IBM** zapewniło wsparcie organizacyjne i technologiczne, informacje o zagrożeniach pochodzące z IBM X-Force oraz charakterystyczny adres IPv4 `9.9.9.9`.
- **Packet Clearing House** dostarczyło infrastrukturę sieciową i doświadczenie w obsłudze globalnych usług DNS.
- **Global Cyber Alliance** uczestniczyło w inicjowaniu projektu, łączeniu go z dostawcami informacji o zagrożeniach oraz w pracach nad rozwojem usługi.

Adres `9.9.9.9` stał się głównym i najbardziej rozpoznawalnym adresem publicznego resolvera Quad9. Usługa udostępnia jednak również inne adresy z przestrzeni `9.9.9.0/24`, między innymi `9.9.9.10`, `9.9.9.11` i `9.9.9.12`. Obsługują one alternatywne warianty usługi, różniące się funkcjami filtrowania zagrożeń i obsługą ECS.

Warto odróżnić przekazanie własności konkretnego adresu od kwestii routingu i wykorzystania całego prefiksu. Oficjalne materiały Quad9 opisują przekazanie przez IBM adresu `9.9.9.9`. Starszy wpis Quad9 dotyczący historii projektu wspomina natomiast o bloku `9.9.9.0/24`. Nie należy na tej podstawie automatycznie wnioskować, że IBM przekazało fundacji własność całego bloku. To dwie odrębne kwestie: własność adresacji oraz możliwość wykorzystywania i ogłaszania prefiksu w globalnym routingu.[^3][^4]

Quad9 działa jako niezależna fundacja. Nie jest usługą IBM ani projektem należącym do jednego z organizacyjnych sponsorów.[^5]

## DNS Threat Intelligence – skąd bierze się ochrona?

Podstawową funkcją Quad9 jest rozpoznawanie i blokowanie domen, które zostały zidentyfikowane jako związane z aktywnością szkodliwą dla użytkowników.

Usługa korzysta z informacji dostarczanych przez zewnętrznych analityków i organizacje zajmujące się Threat Intelligence. Dane z różnych źródeł są łączone w zestawy domen, które z wysokim prawdopodobieństwem służą do prowadzenia działań takich jak:[^1]

- phishing i wyłudzanie danych,
- dystrybucja złośliwego oprogramowania,
- komunikacja z serwerami command and control,
- oszustwa internetowe,
- inne działania wykorzystujące domeny do atakowania użytkowników.

Gdy urządzenie wysyła zapytanie DNS o domenę znajdującą się na liście zagrożeń, resolver może zwrócić odpowiedź blokującą zamiast prawidłowego adresu IP. Dzięki temu połączenie z rozpoznaną szkodliwą domeną może zostać przerwane już na etapie rozwiązywania nazwy.

Nie oznacza to jednak, że Quad9 zastępuje program antywirusowy, firewall czy inne mechanizmy ochronne. Blokowanie na poziomie DNS nie analizuje zawartości stron, nie kontroluje wszystkich połączeń sieciowych i nie daje gwarancji wykrycia każdego zagrożenia.

Quad9 nie jest również typowym blokerem reklam. Jego głównym zadaniem jest blokowanie domen związanych z zagrożeniami, a nie usuwanie reklam czy elementów śledzących ze stron internetowych. Nie należy więc traktować go jako bezpośredniego zamiennika rozwiązań takich jak Pi-hole czy listy filtrujące reklamy.

Istotną cechą usługi jest możliwość sprawdzenia, dlaczego konkretna domena została zablokowana. Quad9 udostępnia narzędzie pozwalające zweryfikować status domeny i źródło informacji o zagrożeniu. To ważne, ponieważ filtrowanie oparte na zewnętrznych danych może prowadzić również do błędnych klasyfikacji.

## Prywatność i sposób przetwarzania zapytań

Prywatność jest jednym z głównych założeń Quad9. Fundacja deklaruje, że nie gromadzi i nie zapisuje adresów IP użytkowników korzystających z normalnie działającej usługi. Nie tworzy też kont użytkowników ani profili łączących ich aktywność z konkretną osobą.[^4][^6]

Adres IP jest jednak technicznie niezbędny do obsłużenia zapytania DNS. Zgodnie z polityką prywatności Quad9 adres źródłowy zapytania znajduje się w pamięci operacyjnej tylko przez czas potrzebny do obsłużenia żądania i wysłania odpowiedzi. Fundacja określa ten czas jako mikrosekundy lub milisekundy. Następnie adres jest usuwany i nie trafia do trwałego magazynu danych.[^6]

Nie oznacza to, że Quad9 nie zbiera żadnych informacji o działaniu usługi. Fundacja gromadzi między innymi zagregowane liczniki i statystyki dotyczące liczby zapytań, kategorii domen, lokalizacji infrastruktury, protokołów czy wydajności. Dane te mają służyć utrzymaniu i rozwijaniu usługi oraz analizie zagrożeń. Według deklaracji Quad9 nie są łączone z adresami IP użytkowników w sposób pozwalający budować ich indywidualne profile.[^6]

Warto uwzględnić wyjątek opisany w dokumentacji. Zwykła polityka prywatności dotyczy normalnego działania usługi. Quad9 publikuje osobne zasady na wypadek sytuacji nadzwyczajnych, na przykład gdy operator uzna, że ktoś prowadzi atak na jego infrastrukturę lub wykorzystuje ją do atakowania innych. W takich okolicznościach mogą być przetwarzane dodatkowe dane diagnostyczne, w tym adresy IP, zgodnie z odrębną polityką. To wyjątek związany z bezpieczeństwem infrastruktury, a nie standardowy sposób obsługi zapytań DNS.

Takie rozróżnienie jest istotne: deklaracja o nieprzechowywaniu adresów IP odnosi się do zwykłego działania resolvera, a nie do każdej możliwej sytuacji operacyjnej.

### Szyfrowanie DNS nie oznacza anonimowości

Quad9 obsługuje szyfrowane protokoły DNS:[^7]

- **DNS over TLS (DoT)** – szyfruje komunikację DNS z resolverem z wykorzystaniem TLS.
- **DNS over HTTPS (DoH)** – przesyła zapytania DNS przez HTTPS.

Szyfrowanie chroni zapytania przed prostym podsłuchem w sieci lokalnej i na trasie do resolvera. Nie oznacza jednak anonimowości. Wybrany operator DNS nadal otrzymuje zapytania i musi je przetworzyć. DoH i DoT nie zastępują VPN ani nie ukrywają całej aktywności sieciowej urządzenia.

Więcej o samym mechanizmie prywatnego DNS opisałem wcześniej we wpisie [Prywatny DNS – wyjaśnienie](@/posts/2026-08-28-prywatny-dns-wyjasnienie.md).

## Cztery warianty resolvera

Quad9 udostępnia kilka wariantów usługi. Różnią się one przede wszystkim włączeniem lub wyłączeniem blokowania zagrożeń oraz obsługą EDNS Client Subnet (ECS).[^7]

| Adres IPv4 | Blokowanie zagrożeń | ECS |
|---|---|---|
| `9.9.9.9` | Tak | Nie |
| `9.9.9.10` | Nie | Nie |
| `9.9.9.11` | Tak | Tak |
| `9.9.9.12` | Nie | Tak |

Wariant `9.9.9.9` jest podstawową usługą Quad9. Wykorzystuje Threat Intelligence do blokowania domen uznanych za szkodliwe.

`9.9.9.10` oferuje resolver bez tego rodzaju filtrowania. Może być przydatny dla osób, które chcą korzystać z infrastruktury i polityki prywatności Quad9, ale wolą samodzielnie kontrolować blokowanie domen.

Warianty `9.9.9.11` i `9.9.9.12` udostępniają odpowiednio te same funkcje co dwa poprzednie, ale z włączonym ECS.

### Czym jest ECS?

EDNS Client Subnet to rozszerzenie DNS, które pozwala przekazać część informacji o sieci klienta dalej, do serwera autorytatywnego. Może to pomóc w dobraniu odpowiedniego serwera CDN lub innego punktu obsługi treści, szczególnie gdy resolver działa w innej lokalizacji niż użytkownik.

Ma to jednak konsekwencje dla prywatności. Informacja o części adresu sieciowego może ujawniać więcej o lokalizacji lub sieci użytkownika niż zapytanie przesłane bez ECS. W podstawowych wariantach `9.9.9.9` i `9.9.9.10` ECS jest wyłączony. Warianty `9.9.9.11` i `9.9.9.12` przekazują tę dodatkową informację.

### DNSSEC na wszystkich wariantach

Ważną zmianą było włączenie walidacji DNSSEC na wszystkich endpointach Quad9 15 czerwca 2026 roku. Dotyczy to również wariantów `9.9.9.10` i `9.9.9.12`, które nie blokują zagrożeń.[^8]

Brak Threat Blocking nie oznacza więc rezygnacji z DNSSEC. To dwie różne funkcje: pierwsza odpowiada za filtrowanie domen na podstawie informacji o zagrożeniach, druga za weryfikację kryptograficznych podpisów danych DNS.

## Jak skonfigurować Quad9?

Najprostszy sposób korzystania z Quad9 to ustawienie adresów resolvera bezpośrednio w systemie operacyjnym, routerze lub urządzeniu sieciowym.

Dla podstawowego wariantu z blokowaniem zagrożeń można użyć:

- IPv4: `9.9.9.9` i `149.112.112.112`,
- DoT: `dns.quad9.net`,
- DoH: `https://dns.quad9.net/dns-query`.

Wariant bez blokowania zagrożeń korzysta z adresów IPv4 `9.9.9.10` i `149.112.112.10`, nazwy DoT `dns10.quad9.net` oraz adresu DoH `https://dns10.quad9.net/dns-query`.

Dla pozostałych wariantów obowiązują analogiczne nazwy:

- `dns11.quad9.net` – Threat Blocking i ECS,
- `dns12.quad9.net` – bez Threat Blocking, z ECS.

Samo wpisanie adresów IPv4 w ustawieniach systemu nie oznacza automatycznie korzystania z szyfrowanego DNS. Jeżeli urządzenie lub system obsługuje DoH albo DoT, trzeba skonfigurować odpowiedni protokół i nazwę serwera. Wtedy zapytania są szyfrowane na odcinku między urządzeniem a resolverem.

Aktualne adresy IPv4, IPv6 oraz parametry poszczególnych wariantów są dostępne w [oficjalnej dokumentacji Quad9](https://docs.quad9.net/services/).

## Quad9 jako upstream dla własnego resolvera

Quad9 może pełnić rolę upstreamu dla własnego serwera DNS. W takim układzie urządzenia w sieci lokalnej kierują zapytania do własnego resolvera, który obsługuje cache, lokalne reguły i listy blokad. Dopiero zapytania wymagające dalszego rozwiązania przekazuje do Quad9.

Schemat przepływu:

```text
Urządzenia w sieci lokalnej
         |
         v
  Własny resolver DNS
         |
         +-- Cache i lokalne reguły
         |
         v
       Quad9
         |
         v
  Serwery autorytatywne
```

Można w ten sposób połączyć lokalne filtrowanie z dodatkową ochroną Quad9. Wybór `9.9.9.9` zapewnia dodatkowe Threat Blocking, a `9.9.9.10` pozwala korzystać z upstreamu bez tego filtrowania.

Warto pamiętać o trzech kwestiach:

- **Źródło zapytań:** Quad9 otrzymuje zapytania od własnego resolvera, a nie bezpośrednio od poszczególnych urządzeń. Nie oznacza to jednak anonimowości.
- **Logowanie:** lokalny resolver nadal może przechowywać historię zapytań. Trzeba samodzielnie określić, czy i jak długo takie dane są zapisywane.
- **Szyfrowanie:** DoT lub DoH chroni połączenie między własnym resolverem a Quad9. Nie zmienia to zasad działania lokalnego DNS ani sposobu przechowywania danych po stronie własnego serwera.

Quad9 publikuje dokumentację dotyczącą konfiguracji forwarderów i współpracy z popularnymi rozwiązaniami DNS, między innymi Unbound, BIND, dnsdist i PowerDNS. Szczegóły zależą od używanego oprogramowania i jego konfiguracji.[^9]

## Spór we Francji – gdzie kończy się neutralność resolvera?

W październiku 2026 roku Quad9 poinformował o niekorzystnym dla fundacji rozstrzygnięciu francuskiego sądu w sprawie dotyczącej domen powiązanych z nielegalnymi transmisjami wydarzeń sportowych. Sprawa dotyczyła żądania blokowania zapytań DNS do wskazanych domen, zgłoszonego przez beIN.[^10][^11]

Według komunikatu Quad9 sąd nakazał blokowanie rozwiązywania nazw dla użytkowników znajdujących się we Francji. Fundacja poinformowała również, że kara finansowa została znacząco obniżona – z 580 000 euro do 1000 euro dziennie.[^10][^11]

Dla Quad9 problem nie sprowadza się jednak do wysokości kary. Jak opisano w sekcji dotyczącej prywatności, fundacja nie prowadzi standardowego rejestru użytkowników ani nie przechowuje ich adresów IP w normalnym trybie działania usługi. Argumentuje więc, że wymuszenie blokowania wyłącznie dla użytkowników z Francji wymagałoby wprowadzenia mechanizmów geolokalizacji lub identyfikacji, które stałyby w sprzeczności z przyjętym modelem prywatności.[^10][^11]

Alternatywą byłoby blokowanie wskazanych domen dla wszystkich użytkowników, niezależnie od ich lokalizacji. Quad9 nie chce jednak przenosić skutków francuskiego nakazu na użytkowników w innych krajach.[^10][^11]

W komunikacie opublikowanym 22 października 2026 roku Quad9 zapowiedział zakończenie świadczenia bezpłatnej usługi ochrony DNS dla użytkowników znajdujących się we Francji. Jako termin tej zmiany wskazano 24 listopada 2026 roku.[^10][^11]

Decyzja fundacji wynika z konfliktu między wymaganiami dotyczącymi blokowania domen a przyjętym modelem ochrony prywatności. Quad9 nie chce wprowadzać mechanizmów geolokalizacji użytkowników ani rozszerzać blokad na osoby korzystające z resolvera poza Francją. Fundacja przedstawiła tę decyzję jako konsekwencję braku możliwości pogodzenia wymagań prawnych z zasadami, na których opiera działanie usługi.

### To nie pierwszy taki przypadek

Spór Quad9 wpisuje się w szerszy problem nakładania obowiązków blokowania domen na publicznych dostawców DNS. Francuskie postępowania dotyczące pirackich transmisji sportowych obejmowały wcześniej również Google Public DNS, Cisco OpenDNS i Cloudflare.

W 2024 roku Cisco zawiesiło dostępność OpenDNS we Francji (a także w Portugalii) w następstwie nakazu blokowania domen związanego ze sprawą Canal+. Usługa przestała być dostępna dla użytkowników w tych krajach od 28 czerwca 2024 roku.[^12]

To pokazuje, że problem nie dotyczy wyłącznie jednej fundacji ani jednego sporu z nadawcą. Jest częścią szerszej dyskusji o tym, jak daleko mogą sięgać obowiązki pośredników infrastruktury internetowej i czy publiczny resolver DNS powinien być traktowany jako narzędzie egzekwowania blokad treści.

Z perspektywy technicznej DNS jest usługą infrastrukturalną. Z perspektywy prawnej coraz częściej staje się jednym z punktów, w których próbuje się ograniczać dostęp do określonych zasobów. To napięcie będzie prawdopodobnie powracać również w innych krajach.

## Czy warto korzystać z Quad9?

Quad9 pozostaje interesującą propozycją dla osób, które chcą korzystać z publicznego resolvera DNS z dodatkową ochroną przed znanymi zagrożeniami i jasno opisaną polityką prywatności.

Najważniejsze cechy usługi to:

- bezpłatny dostęp do publicznego resolvera, bez konieczności zakładania konta,
- blokowanie domen powiązanych z phishingiem, malware i innymi zagrożeniami,
- walidacja DNSSEC na wszystkich wariantach,
- obsługa szyfrowanych protokołów DoH i DoT,
- możliwość wyboru wariantu z Threat Blocking lub bez niego,
- możliwość wyboru wariantu z ECS lub bez ECS.

Nie jest to rozwiązanie pozbawione ograniczeń. Skuteczność blokowania zależy od jakości i aktualności źródeł Threat Intelligence. Quad9 nie zastępuje lokalnego filtrowania, ochrony endpointów ani świadomego korzystania z internetu.

Wybór wariantu zależy od potrzeb. Dla większości użytkowników sensownym punktem wyjścia będzie `9.9.9.9`. Osoby, które nie chcą filtrowania zagrożeń po stronie resolvera, mogą wybrać `9.9.9.10`. Warianty z ECS warto rozważyć wtedy, gdy ważniejsza jest optymalizacja wyboru serwerów CDN niż ograniczenie przekazywanych informacji o sieci klienta.

Sam obecnie korzystam z własnego resolvera, ale nie zmienia to mojej oceny Quad9 jako sprawdzonej i wartej rozważenia usługi. Dla osób, które nie chcą utrzymywać własnej infrastruktury DNS, może być rozsądnym sposobem na uzyskanie dodatkowej warstwy ochrony bez konieczności samodzielnego zarządzania listami zagrożeń.

## Podsumowanie

Dziewięć lat po uruchomieniu Quad9 nadal realizuje założenie, z którym powstał: udostępnia publiczny resolver DNS łączący ochronę przed zagrożeniami z naciskiem na prywatność użytkowników.

DNS jest tylko jednym z elementów bezpieczeństwa. Dobrze skonfigurowany resolver może ograniczyć ryzyko połączenia ze znanymi szkodliwymi domenami, ale nie zastąpi pozostałych mechanizmów ochronnych.

Sprawa francuska pokazuje natomiast, że publiczna infrastruktura DNS znajduje się coraz częściej w centrum sporów o blokowanie treści. To istotny temat nie tylko dla operatorów resolverów, ale również dla użytkowników, którzy korzystają z alternatywnych usług DNS ze względu na prywatność, bezpieczeństwo lub niezależność od ustawień dostawcy internetu.

Quad9 pozostaje przykładem usługi, która próbuje połączyć te cele w jednym, ogólnodostępnym rozwiązaniu.

Więcej informacji o projekcie można znaleźć na [oficjalnej stronie Quad9](https://quad9.net/) oraz na [profilu fundacji w Mastodon](https://mastodon.social/@quad9dns).

## Przypisy

[^1]: Quad9. (n.d.). *Threat Blocking*. [Link](https://quad9.net/service/threat-blocking/)
[^2]: IBM. (2017, November 17). *IBM, Packet Clearing House and Global Cyber Alliance Collaborate to Protect Businesses and Consumers from Internet Threats*. [Link](https://uk.newsroom.ibm.com/2017-11-17-IBM-Packet-Clearing-House-and-Global-Cyber-Alliance-Collaborate-to-Protect-Businesses-and-Consumers-from-Internet-Threats)
[^3]: Quad9. (n.d.). *Sponsors*. [Link](https://quad9.net/about/sponsors/)
[^4]: Quad9. (n.d.). *Quad9 and Your Data*. [Link](https://quad9.net/news/blog/quad9-and-your-data/)
[^5]: Quad9. (n.d.). *About*. [Link](https://quad9.net/about/)
[^6]: Quad9. (n.d.). *Data and Privacy Policy*. [Link](https://quad9.net/privacy/policy/)
[^7]: Quad9. (n.d.). *Services documentation*. [Link](https://docs.quad9.net/services/)
[^8]: Quad9. (2026, June 15). *Quad9 Enables DNSSEC on All Service Endpoints*. [Link](https://quad9.net/news/blog/quad9-enables-dnssec-on-all-service-endpoints/)
[^9]: Quad9. (n.d.). *DNS Forwarder Best Practices*. [Link](https://docs.quad9.net/Quad9_For_Organizations/DNS_Forwarder_Best_Practices/)
[^10]: Quad9. (2026, October 22). *Farewell, France*. [Link](https://quad9.net/news/blog/au-revoir-france/)
[^11]: Van der Sar, E. (2026, October 22). *DNS Resolver Quad9 Will Leave France After Court Orders Piracy Blocking Penalty*. TorrentFreak. [Link](https://torrentfreak.com/dns-resolver-quad9-will-leave-france-after-court-orders-piracy-blocking-penalty/)
[^12]: FlashStart. (2024, July 4). *OpenDNS not available in France and Portugal: why not and how to resolve this*. [Link](https://flashstart.com/opendns-not-available-in-france-and-portugal-why-not-and-how-to-resolve-this/)
