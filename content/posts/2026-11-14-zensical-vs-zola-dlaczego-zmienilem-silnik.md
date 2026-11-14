+++
title = "Ewolucja bloga: Dlaczego zamieniłem Zensical na Zolę"
date = 2026-11-14
description = "Analiza decyzji o migracji z Zensical na Zolę. O kompromisach, kontroli nad kodem, braku zależności w Pythonie i tym, dlaczego KISS wygrywa w projektach osobistych."

[extra]
toc = true
image = "og/posts/zensical-vs-zola-og.png"
+++

Wybór silnika do bloga to decyzja inżynierska. Każdy z generatorów stron statycznych (SSG) ma swoje mocne i słabe strony. Przez długi czas mój blog działał na **[Zensical](https://zensical.org/)**, który jest świetnym i nowoczesnym narzędziem. Ostatnio jednak podjąłem decyzję o migracji na **[Zolę](https://www.getzola.org/)**. 

Nie stało się to z powodu kaprysu. Poniżej przedstawiam chłodną analizę przyczyn, kompromisów i tego, co ostatecznie przeważyło szalę.

## Punkt wyjścia: Zensical jako potężne narzędzie

Zensical to świetny wybór, zwłaszcza jeśli ktoś potrzebuje rozbudowanej dokumentacji. Został stworzony przez twórców Material for MkDocs i jest rozwijany jako jego następca. Oferuje wspaniały ekosystem wtyczek, gotowe komponenty i świetnie wygląda "z pudełka".

Warto oddać sprawiedliwość: Zensical również dąży do uproszczenia i potrafi budować istniejące projekty MkDocs bez konieczności zmian w konfiguracji. Wykorzystuje nowoczesny rdzeń, co przekłada się na szybkie przyrostowe przebudowy. Jednak z czasem zauważyłem, że w moim prywatnym, minimalistycznym projekcie ta "bogata" architektura zaczyna mi ciążyć. Zensical, mimo nowoczesnego rdzenia, jest dystrybuowany jako pakiet Pythona i wymaga środowiska Python oraz menedżera pakietów do instalacji i działania. Przy wdrażaniu na GitHub Actions oznacza to zarządzanie zależnościami i wirtualnymi środowiskami (`venv`), co bywa uciążliwe.

## Dlaczego Zola? (Powody migracji)

Zola to generator napisany w języku Rust. Skupia się na jednej rzeczy: **kompilacji Markdowna do HTML**. I to robi bezbłędnie. Oto dlaczego wygrała.

### 1. Szybkość i lekkość (Single Binary)
To jest czynnik, który w społeczności deweloperów jest najczęściej chwalony. Zola jest niezwykle szybka, ponieważ jej rdzeń napisano w Rust. Rzeczywista różnica w czasie budowania zależy oczywiście od rozmiaru projektu, ale prawdziwa przewaga leży w architekturze. Zola to pojedynczy plik binarny, który nie wymaga środowiska Node.js, Ruby'ego ani Pythona. Wdrażanie na GitHub Actions jest błyskawiczne, a sama kompilacja strony zajmuje ułamek sekundy. Nie musisz martwić się o "dependency hell" ani o to, że za pół roku build przestanie działać z powodu aktualizacji jakiejś biblioteki w tle.

### 2. Pełna kontrola nad szablonami (Tera)
W Zensical wiele rzeczy działa "magicznie" i jest narzuconych przez motyw. Chcąc zrobić coś niestandardowego (np. mój własny **Katalog Oprogramowania**), musiałem czasem walczyć z tym, jak framework widzi świat.

Zola używa silnika szablonów **Tera** i czystego HTML. Jeśli chcę zmienić wygląd czegokolwiek, po prostu edytuję plik `.html`. Jeśli chcę dodać nową sekcję z własną logiką (jak katalog oparty na czystym gridzie CSS), po prostu to robię. Nie ma tutaj żadnej "czarnej magii" – to czysta inżynieria.

### 3. Wbudowane funkcje bez wtyczek
Zola ma wbudowane wsparcie dla kompilacji Sass, podświetlania składni kodu, generowania kanałów RSS, przetwarzania obrazów oraz automatycznego generowania spisu treści. Nie musisz instalować dziesiątek zewnętrznych wtyczek, aby uzyskać podstawową funkcjonalność. Wszystko działa od razu po wyjęciu z pudełka.

### 4. Zasada KISS (Keep It Simple, Stupid)
Dla prywatnego bloga z kilkoma podstronami, Zensical był miejscami "over-engineered". Zola wymusza pewną dyscyplinę: każda podstrona to plik `.md` z nagłówkiem TOML, a jej wygląd zależy od przypisanego szablonu. To sprawia, że struktura projektu jest czytelna i logiczna.

## Co poświęciłem? (Kompromisy)

Żadna migracja nie jest darmowa. Przechodząc na Zolę, zrezygnowałem z:
*   Gotowego ekosystemu wtyczek (w Zoli wiele rzeczy trzeba napisać samemu, np. obsługę przypisów czy generowanie specyficznych tagów).
*   Opartego na gotowych komponentach interfejsu Material Design (Zola wymaga zdefiniowania własnego CSS, co z jednej strony daje wolność, a z drugiej wymaga pracy).
*   Wbudowanych mechanizmów nawigacji, które w Zensical są dostępne od razu.

## Podsumowanie

Migracja z Zensical na Zolę to powrót do korzeni statycznych stron. Zola wymaga odrobiny więcej pracy na starcie przy budowie szablonów, ale w zamian daje absolutną, bezdyskusyjną kontrolę nad tym, co i jak jest renderowane. 

Jeśli budujesz rozległą bazę wiedzy (jak np. PrivacyGuides), Zensical będzie świetnym wyborem. Jeśli jednak tworzysz osobisty, minimalistyczny blog i chcesz, aby silnik był tylko przezroczystym narzędziem w Twoich rękach – Zola jest bezkompromisowym zwycięzcą. W społeczności widać wyraźny trend migracji z Jekylla i innych cięższych narzędzi właśnie w stronę Zoli, ze względu na jej szybkość, brak zależności i prostotę.
