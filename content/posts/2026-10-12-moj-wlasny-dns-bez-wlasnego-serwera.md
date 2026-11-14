+++
title = "Mój własny DNS. Bez własnego serwera, bez logów."
date = 2026-10-12
description = "Zbudowałem własny resolver DNS-over-HTTPS na Cloudflare Workers, który blokuje reklamy, trackery, malware i phishing. Bez panelu, bez historii zapytań, z własnym pipeline’em czyszczenia list."

[extra]
toc = true
+++

Kiedyś używałem publicznego DNS z filtrowaniem, ale brakowało mi kontroli. Chciałem sam decydować o źródłach, łączeniu list i czyszczeniu ich z duplikatów. Dlatego zbudowałem własny resolver na Cloudflare Workers, który stoi w chmurze, blokuje reklamy, trackery i malware, a przy tym nie zapisuje historii moich zapytań.

W publicznych resolverach, takich jak NextDNS czy AdGuard DNS, wybierasz listy z gotowego katalogu. Ja chciałem sam decydować, z których źródeł korzystam, jak je łączę i co z nich wyciągam. Pi-hole też na to pozwala, ale wymaga serwera w domu i domyślnie działa tylko w sieci lokalnej. Żeby filtrował poza domem, trzeba stawiać VPN albo wystawiać go jako publiczny endpoint. Ja nie chciałem ani jednego, ani drugiego.

To mnie uwierało. Nie z powodu paranoi, ale z poczucia, że oddaję kontrolę nad czymś fundamentalnym. Nad tym, jak moje urządzenia łączą się ze światem.

## Co to właściwie jest?

To **prywatny resolver DNS-over-HTTPS (DoH)** na Cloudflare Workers. Nie klasyczny DNS na porcie 53 — tylko DoH, czyli DNS szyfrowany i transportowany po HTTPS. Dzięki temu działa na urządzeniach i w aplikacjach, które obsługują DoH: w przeglądarkach, w systemowych ustawieniach „Private DNS” (jeśli wybrana opcja to DoH, a nie DoT) albo w dedykowanych klientach DoH, takich jak AdGuard, Intra czy Nebulo.

Cały proces wygląda prosto:

1. Urządzenie wysyła zapytanie DoH do mojego Workera.
2. Worker sprawdza, czy domena (lub jej domena nadrzędna) jest na liście blokad.
3. Jeśli jest — zwraca `NXDOMAIN` („nie znam takiej strony”).
4. Jeśli nie ma — przekazuje zapytanie do upstreamu, usuwając wcześniej nagłówki identyfikujące klienta.

Blokuję reklamy, trackery, telemetrię, domeny malware i phishing. Korzystam z kilku starannie wybranych źródeł. Część skupia się na reklamach i trackerach (OISD, HaGeZi, 1Hosts, Easyprivacy), a inne dostarczają wiedzy o zagrożeniach oraz polskim internecie (phishing-filter, Dandelion Sprout, urlhaus, KADhosts, MajkiIT).

> **Pierwsza warstwa**
> To działa na poziomie DNS. Nie blokuje reklam w YouTube ani elementów wewnątrz strony. Do tego potrzebny jest osobny content blocker w przeglądarce. Resolver jest tylko (i aż) pierwszą warstwą ochrony.

## Jak łączę listy i usuwam duplikaty

Resolver nie działa w próżni — potrzebuje listy, na której może pracować. Przygotowanie tej listy to osobna część całego rozwiązania. Pipeline na GitHub Actions pobiera źródła z `sources.txt`, normalizuje je do czystych domen, odrzuca adresy IP i komentarze, a następnie **aglomeruje**: usuwa `sub.domena.com`, jeśli `domena.com` już jest na liście, bo domena nadrzędna i tak zablokuje całą subdomenę.

Pipeline obsługuje kilka formatów, ale świadomie preferuje najlżejsze:

| Format | Przykład zapisu | Status |
| :--- | :--- | :--- |
| **`domains`** | `domena.com` | Preferowany (najlżejszy) |
| **`wildcards`** | `*.domena.com` | Wspierany |
| **`hosts`** | `0.0.0.0 domena.com` | Wspierany |
| **`dnsmasq`** | `address=/domena.com/0.0.0.0` | Preferowany |
| **`adblock`** | `\|\|domena.com^` | Wspierany awaryjnie (tylko bezpieczny podzbiór reguł; modyfikatory `$` nie są bezwarunkowo ignorowane) |

Po aglomeracji i przejściu przez `allowlist.txt` powstają dwa pliki wyjściowe: tekstowa lista domen oraz **`doh_combined.msgpack`** — format binarny zoptymalizowany pod niskie zużycie CPU w Workerze. Worker dekoduje MsgPack bezpośrednio do struktury `Set`, co minimalizuje koszt zimnego startu.

Efekt ostatniego przebiegu:

| Etap | Liczba wpisów |
| :--- | :--- |
| Surowych linii z wszystkich źródeł | **493 754** |
| Po deduplikacji | **355 972** |
| **Po aglomeracji (gotowa lista)** | **307 314** |
| *Usunięto redundantnych subdomen* | *48 658* |

> **Automatyzacja i wydajność**
> Ten proces dzieje się raz dziennie. Lista sama się aktualizuje, sama czyści i sama trafia do Cloudflare KV. Worker jest maksymalnie odchudzony — nie traci milisekund CPU na ponowne przetwarzanie tekstu, skoro Python zrobił to już za niego. Gdy KV zawiedzie, Worker pobiera listę awaryjnie z GitHuba. Jeśli oba źródła zawiodą, przepuszcza ruch bez filtracji, ale nadal utrzymuje szyfrowany tunel DoH.

## Dlaczego to nie ma panelu ani statystyk?

Mój resolver nie ma panelu administracyjnego. Nie ma wykresów, nie ma listy ostatnich zapytań, nie ma licznika zablokowanych domen.

To celowa decyzja. Panel ze statystykami oznacza, że ktoś — nawet tylko ja — gdzieś te dane przechowuje. Lista ostatnich zapytań to nic innego jak historia tego, co robię w internecie. A ja nie chcę, żeby taka historia istniała w jakiejkolwiek formie.

> **Zero-Logs po stronie aplikacji**
> Worker przetwarza zapytanie, zwraca odpowiedź i zapomina. Nie zapisuje niczego i nie analizuje. Nie ma czego nie logować, bo nie ma gdzie logować. To nie znaczy, że „nikt nic nie widzi” — Cloudflare jako platforma może widzieć metadane na poziomie edge/konta, a mój upstream widzi zapytania, których nie zablokowałem. Różnica polega na tym, że **ja nie buduję historii moich zapytań**.

## Dla kogo to jest

To rozwiązanie dla kogoś, kto ma kilka urządzeń i chce, żeby wszystkie filtrowały tak samo. Telefon, laptop, tablet — pod warunkiem, że obsługują DoH lub mają klienta/profil DoH. Bez instalowania osobnych aplikacji na każdym z nich. Dla kogoś, kto ceni prywatność i chce sam decydować, co blokuje, zamiast czekać, aż ktoś inny zrobi to za niego.

Jeśli zależy Ci na panelu i statystykach, inne rozwiązania zrobią to lepiej. Ale jeśli cenisz kontrolę i prywatność bardziej niż wygodę, to jest dokładnie to.
