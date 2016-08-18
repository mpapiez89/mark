#Krótkie wprowadzenie do GeoPythona – Małgorzata Papież

Od kilku lat, w Polsce coraz większą popularność zdobywają aplikacje oparte na wykorzystaniu *danych przestrzennych*. 
To dzięki nim przestaliśmy być zależni od papierowych map i bez wychodzenia z domu możemy zobaczyć każdy zakątek Ziemi. 
Co jeśli dostępne aplikacje takie jak Google Earth, Google Maps nam nie wystarczają i chcemy czegoś więcej ? 
Zobaczmy więc jak wykorzystując Pythona możemy zaprojektować aplikacje pozwalające nam na tworzenie własnych map czy przeglądanie najnowszych zdjęć satelitarnych.

## Dane przestrzenne 
Rozwój technologii komputerowych w ostatnich latach sprawił, że przetwarzanie i analiza bardzo dużej ilości danych stała się możliwa. 
Do takich danych niewątpliwie należą dane przestrzenne. Przechowują one informacje o obiekcie świata rzeczywistego biorąc pod uwagę jego położenie przestrzenne. Oprócz informacji przestrzennej zazwyczaj podanej w postaci współrzędnych mogą zawierać również inne istotne informacje takie jak data utworzenia, związki przestrzenne obiektów ze sobą (tzw. topologia) czy właściwości danego obiektu (atrybuty). Dane przestrzenne dzieli się na wektorowe i rastrowe. Pierwsze z nich reprezentowane są przez elementy geometryczne takie jak punkty, linie, poligony, których kształt i położenie definiowany jest przez współrzędne. W modelu rastrowym dane są przechowywane w postaci pojedynczych pikseli które mają przypisane współrzędne terenowe. 
Źródła danych przestrzennych:

* mapy i plany
* digitalizacja i wektoryzacja papierowych map
* odbiorniki GPS
* zdjęcia satelitarne i lotnicze
* pomiary geodezyjne, stacje pomiarowe i wywiady terenowe


## Python i dane przestrzenne
Znaczna część aplikacji wykorzystujących dane przestrzenne napisana została w języku Python. Taki wybór podyktowany został możliwościami jakie daje ten język programowania. Po pierwsze pozwala korzystać za darmo praktycznie ze wszystkich bibliotek związanych z danymi przestrzennymi w szczególności z *GDAL/OGR*. Dodatkowo ułatwia prace na danych rastrowych dzięki bibliotece *numpy*. Graficzna biblioteka *PyQt* pozwala budować interfejsy graficzne, które mogą być dołączane jako nakładki do istniejących już aplikacji przestrzennych (np. QGis) i przenoszone między systemami operacyjnymi bez potrzeby modyfikacji ich kodu. 
