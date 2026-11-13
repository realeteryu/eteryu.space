+++
title = "Jak zainstalować Pixel Camera na GrapheneOS bez usług Google"
date = 2026-08-15
description = "Krótki przewodnik, jak zastąpić systemowy aparat pełnoprawną aplikacją Pixel Camera na GrapheneOS. Zyskasz rewelacyjny tryb nocny, zachowując pełną prywatność i odcinając aplikację od sieci."

[extra]
toc = true
+++

Czy na GrapheneOS da się mieć doskonały aparat bez wpuszczania usług Google? Oczywiście. Oto krótki przewodnik, jak zastąpić systemowy aparat pełnoprawną aplikacją Pixel Camera. Zyskasz rewelacyjny tryb nocny (Night Sight) i pełne wykorzystanie procesora, zachowując przy tym całkowitą prywatność i odcinając aplikację od sieci.

Ostatnio na moim profilu na Mastodonie zostałem zapytany o to, jak dokładnie używam aparatu w moim setupie na GrapheneOS. Postanowiłem napisać krótki wpis, aby w jednym miejscu wyjaśnić, jak to zrobiłem.

Zależało mi na zastąpieniu domyślnego aparatu systemowego, ponieważ aplikacja od Google w pełni wykorzystuje procesor i robi po prostu o wiele lepsze zdjęcia, co doskonale widać zwłaszcza w trybie nocnym. Wszystko to chciałem osiągnąć, zachowując pełną prywatność i bez wpuszczania usług Google do głównego profilu w telefonie.

Oto krótkie podsumowanie, dlaczego w ogóle warto to zrobić:

| Cecha | Aparat GrapheneOS (Secure Camera) | Pixel Camera (Offline) |
| :--- | :--- | :--- |
| **Jakość zdjęć i tryb nocny** | Przeciętna. Nie wykorzystuje w pełni układu Tensor i zaawansowanych algorytmów Google. | **Rewelacyjna.** Pełne wsparcie procesora, fenomenalny tryb nocny (Night Sight) i HDR+. |
| **Prywatność (Out-of-the-box)** | **Absolutna.** Z założenia pozbawiona dostępu do sieci (`Network`) i jakiejkolwiek telemetrii. | Wymaga ręcznego odcięcia od sieci (**`Network Permission`**), by powstrzymać telemetrię. |
| **Integracja z galerią** | Bezproblemowa, korzysta z domyślnej galerii systemowej. | Wymaga instalacji mostka (`Gcam Services Provider`), by płynnie połączyć się z zewnętrzną galerią (np. Ente). |

A tak dokładnie wygląda proces konfiguracji.

## 1. Pixel Camera w odpowiedniej wersji

Kluczowa sprawa to aplikacja działająca bez usług Google Play w głównym profilu. Nie znajdziemy w internecie gotowego pliku instalacyjnego z zaufanego źródła. Zdecydowałem się na użycie `Aurora Store`. Nie polecam korzystania z tego sklepu na co dzień, ale w tym konkretnym przypadku był to świadomy kompromis i najlepsze wyjście z sytuacji.

Co ważne, **nie musimy już pobierać starszych wersji z tzw. ręcznego pobierania**. Jeszcze do niedawna, od wersji 10 aplikacja wymuszała obecność usług Google Play z powodu braku dostępu do systemowej czcionki, co powodowało jej natychmiastowe zamykanie się.

**Dlaczego najnowsze wersje znów działają?** 
Zespół GrapheneOS zaimplementował niedawno na poziomie systemu specjalną "zaślepkę" (ang. *shim*), która emuluje zachowanie usługi ładowania czcionek od Google. Dzięki temu rozwiązaniu, system skutecznie "oszukuje" aplikację, dostarczając jej to, czego wymaga, bez konieczności faktycznej instalacji usług Google Play. W efekcie najnowsze wersje Pixel Camera znów działają stabilnie prosto z pudełka.

> **Jak to zrobić**
> * Pobieramy sklep `Aurora Store` (podczas instalacji wybieramy standardową paczkę, zazwyczaj pierwszą z brzegu, bez dopisków "hw" czy "preload").
> * Wyszukujemy standardowo aplikację `Pixel Camera` i po prostu ją instalujemy. Aurora Store sama dobierze najnowszą, kompatybilną z Twoim modelem telefonu wersję.
> * Aplikację `Aurora Store` od razu odinstalowujemy, jeśli nie korzystamy z niej na co dzień.

> **Ważne dla osób aktualizujących aparat**
> Jeśli miałeś zainstalowaną polecaną przeze mnie wcześniej, starszą wersję 9.x (build 68469120), **całkowicie ją odinstaluj** przed instalacją nowej. Przeskok na wersje z linii 10+ to spora zmiana i pozostawienie starych plików w pamięci podręcznej może powodować błędy. Najlepiej zainstalować nową wersję na czysto!

> **Kluczowe ustawienia po instalacji**
> Ostatnim etapem, w którym domykamy kwestie bezpieczeństwa i optymalizacji, jest nałożenie na aparat trzech restrykcji. Dostęp do sieci możesz zablokować już w samym oknie instalatora, natomiast po resztę musisz wejść w ustawienia nowo zainstalowanej aplikacji (pamiętaj, by ustawić to ponownie, jeśli instalowałeś aparat na czysto):
> 
> * **`Network Permission`** (Dostęp do sieci) – w systemie GrapheneOS przełącznik ten pojawia się już podczas instalacji i warto go od razu odznaczyć. Jeśli tego nie zrobiłeś, permanentnie odbierz to uprawnienie w ustawieniach. Aplikacja będzie całkowicie odcięta od internetu, a telemetria Google nie ma jak opuścić urządzenia.
> * **`Storage Scopes`** (Zakresy pamięci) – zamiast dawać aplikacji Google pełny dostęp do wszystkich Twoich plików, uruchamiamy tę potężną funkcję GrapheneOS. Dzięki niej aparat uważa, że ma dostęp do pamięci (więc normalnie i bez błędów zapisuje zrobione zdjęcia), ale w rzeczywistości widzi tylko i wyłącznie własny folder. Jest całkowicie "ślepy" na resztę zawartości Twojego dysku.
> * **`Allow background usage`** (Działanie w tle) – wyłączamy tę opcję. Aparat nie ma żadnego powodu, by pracować w pamięci RAM, gdy z niego nie korzystasz. To prosty nawyk, który znakomicie oszczędza baterię i zasoby telefonu.

## 2. Gcam Services Provider

Oryginalne wersje Pixel Camera domyślnie do podglądu zdjęć używają usługi Google Photos. Ponieważ moją główną galerią oraz szyfrowaną chmurą end to end jest `Ente Photos`, potrzebowałem sposobu na ich połączenie.

Z rozwiązaniem przychodzi tutaj `Gcam Services Provider` w wariancie `photosonly`. Jest to open source'owy mostek autorstwa lukaspieper, dzięki któremu aparat działa stabilnie offline i płynnie współpracuje z zewnętrzną galerią.

* Wchodzimy na platformę GitHub do sekcji wydań (releases) oficjalnego repozytorium: [lukaspieper / Gcam-Services-Provider](https://github.com/lukaspieper/Gcam-Services-Provider).
* Wybieramy opcję `photos only`. Ja instaluję i aktualizuję ją wygodnie przez aplikację `Obtainium`.
* Bezpośredni link do pliku: [app-photosonly-release.apk](https://github.com/lukaspieper/Gcam-Services-Provider/releases/download/v1.6.1/app-photosonly-release.apk)

I to tyle! Ten prosty zestaw pozwala cieszyć się świetną jakością zdjęć oraz bezproblemową współpracą z wybraną galerią. To idealne rozwiązanie na systemie GrapheneOS – nie tylko dla Pixela 8, ale dla każdego, kto nie korzysta z usług Google wcale lub zrezygnował z nich w swoim głównym profilu.
