+++
title = "AsterMail: obiecująca prywatna poczta z kryptografią post-kwantową"
date = 2026-10-19
description = "Testuję nowy projekt AsterMail. Sprawdzam szyfrowanie end-to-end, ochronę przed trackerami, obsługę własnej domeny i oceniam, czy warto się tam przenosić."

[extra]
toc = true
+++

Ostatnio na moim radarze pojawiła się nowa usługa poczty elektronicznej – AsterMail. Postanowiłem sprawdzić, jak wypada w praktyce pod kątem prywatności, szyfrowania end-to-end i codziennego użytkowania. Poniżej dzielę się wrażeniami z kilkudniowych testów.

## Pierwsze kroki i darmowy plan

Aplikację na Androida pobrałem bezpośrednio z oficjalnego repozytorium GitHub projektu. Proces rejestracji okazał się prosty. Konto można założyć bez podawania numeru telefonu, a imię i nazwisko są opcjonalne. Co ciekawe, podczas tworzenia skrzynki od razu zarezerwowany zostaje alias w domenie alternatywnej. W moim przypadku oprócz głównego adresu w domenie **astermail.org** otrzymałem również alias w domenie **aster.cx**.

Darmowy plan oferuje:

- **10 GB** pamięci
- **5 aliasów**
- **1 własną domenę**
- Szyfrowanie end-to-end

To wszystko bez karty kredytowej i bez okresu próbnego. Co ważne, szyfrowanie nie jest okrojone w darmowym planie. Działa dokładnie tak samo jak w płatnych subskrypcjach.

> **Warto wiedzieć**
> 
> AsterMail to projekt w pełni otwartoźródłowy. Serwery znajdują się w Niemczech, a darmowy plan jest w pełni funkcjonalny od pierwszego dnia.

## Aliasy i własna domena

System aliasów działa sprawnie. Bez problemu udało się utworzyć losowy alias, a także tak zwany Ghost Alias, czyli adres generowany automatycznie na potrzeby jednorazowych rejestracji. Wysyłanie i odbieranie wiadomości na wszystkie trzy typy adresów działa poprawnie.

Co ważne, opcja podpięcia własnej domeny także jest dostępna w darmowym planie. Po skonfigurowaniu rekordów DNS wysyłanie oraz odbieranie poczty z własnej domeny działa bez zarzutu. To duży plus dla osób, które chcą korzystać z własnej tożsamości w sieci bez ponoszenia dodatkowych kosztów.

## Szyfrowanie i komunikacja między skrzynkami

Gdy wysłałem wiadomość z głównego adresu na jeden z aliasów w obrębie tej samej usługi, w interfejsie od razu pojawiła się informacja o szyfrowaniu end-to-end. W szczegółach wiadomości nie ma surowych nagłówków, co potwierdza, że treść jest szyfrowana po stronie klienta. Co istotne, szyfrowane są nie tylko treści wiadomości, ale także tematy i załączniki.

Warto dodać, że AsterMail nie poprzestaje na klasycznym szyfrowaniu. Projekt wykorzystuje nowoczesną kryptografię post-kwantową. W praktyce oznacza to ochronę przed atakami typu *store now, decrypt later*, czyli zapisywaniem zaszyfrowanego ruchu dzisiaj z nadzieją na złamanie go w przyszłości przez komputery kwantowe. Architektura zero-access gwarantuje z kolei, że dostawca nie ma technicznej możliwości odczytania korespondencji.

## Ochrona przed trackerami i fingerprintingiem

Jednym z elementów, które pozytywnie mnie zaskoczyły, jest blokowanie mechanizmów śledzących. AsterMail nie tylko deklaruje ochronę, ale realnie ją egzekwuje.

Aplikacja pokazuje liczbę zablokowanych pikseli śledzących oraz domeny, z których pochodzą. Co istotne, blokowane są również zdalne czcionki. Wiele klientów poczty ogranicza się do blokowania obrazków, tymczasem zestaw fontów może służyć do fingerprintingu, czyli tworzenia unikalnego profilu urządzenia. AsterMail wychwytuje ten mechanizm, co świadczy o dojrzałym podejściu do prywatności. Warto podkreślić, że w aplikacji nie ma reklam ani analityki śledzącej.

## Wsparcie techniczne

Na wysłane pytania techniczne support odpowiedział tego samego dnia. To pozytywny sygnał, zwłaszcza w przypadku młodego projektu, gdzie komunikacja z twórcami bywa utrudniona.

## Podsumowanie

AsterMail to projekt otwartoźródłowy z obiecującym zestawem funkcji prywatnościowych już w darmowym planie. Aplikacja działa stabilnie, a mechanizmy ochrony są realne, a nie wyłącznie marketingowe. Kryptografia post-kwantowa i blokowanie zdalnych fontów to cechy, których na próżno szukać u wielu konkurentów.

> **Ostrzeżenie**
> 
> Projekt jest młody i wciąż znajduje się we wczesnej fazie rozwoju. Na obecnym etapie **nie polecam przenoszenia całej swojej poczty do tej usługi**. Warto obserwować jego rozwój i testować rozwiązanie równolegle, bez rezygnacji z dotychczasowego dostawcy.
