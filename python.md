#Krótkie wprowadzenie do GeoPythona – Małgorzata Papież

Od kilku lat, w Polsce coraz większą popularność zdobywają aplikacje oparte na wykorzystaniu *danych przestrzennych*. 
To dzięki nim przestaliśmy być zależni od papierowych map i bez wychodzenia z domu możemy zobaczyć każdy zakątek Ziemi. 
Co jeśli dostępne aplikacje takie jak Google Earth, Google Maps nam nie wystarczają i chcemy czegoś więcej ? 
Zobaczmy, więc jak wykorzystując Pythona możemy zaprojektować aplikacje pozwalające nam na tworzenie własnych map czy przeglądanie najnowszych zdjęć satelitarnych.

## Dane przestrzenne 
Rozwój technologii komputerowych w ostatnich latach sprawił, że przetwarzanie bardzo dużej ilości danych w czasie rzeczywistym stało się możliwe. Przyspieszenie analityki dużych zbiorów danych przyczyniło się wzrostu zainteresowania danymi przestrzennymi. Dane przestrzenne przechowują informacje o obiekcie świata rzeczywistego biorąc pod uwagę jego położenie przestrzenne. Oprócz informacji przestrzennej zazwyczaj podanej w postaci współrzędnych mogą zawierać również inne istotne informacje takie jak data utworzenia, związki przestrzenne obiektów ze sobą tzw. topologię czy właściwości danego obiektu czyli jego atrybuty. Dane przestrzenne dzieli się na wektorowe i rastrowe. Pierwsze z nich reprezentowane są przez elementy geometryczne takie jak punkty, linie, powierzchnie, których kształt i położenie definiowany jest przez współrzędne. Taki sposób reprezentacji używany jest z racji swojej dużej dokładności oraz możliwości wyodrębniania poszczególnych obiektów. Do obiektów wektorowych w rzeczywistości należą granice działek, ulice, budynki, sieci: elektryczne, gazowe, wodne, telefoniczne itd. W modelu rastrowym dane są przechowywane w postaci pojedynczych pikseli, które mają przypisane współrzędne terenowe. O ile model wektorowy przeznaczony jest do przechowywania danych o charakterze dyskretnym, tak model rastrowy jest przypisany do danych o charakterze ciągłym. Dzięki temu znacznie wydajniej radzi sobie z analizą i modelowaniem zjawisk zachodzących w przestrzeni np. stanu zanieczyszczenia, opadów atmosferycznych.
Źródła danych przestrzennych:

* mapy i plany
* digitalizacja i wektoryzacja papierowych map
* odbiorniki GPS
* zdjęcia satelitarne i lotnicze
* pomiary geodezyjne, stacje pomiarowe i wywiady terenowe


## Python i dane przestrzenne
Znaczna część aplikacji korzystająca z danych przestrzennych napisana została w języku Python. Taki wybór podyktowany został możliwościami jakie daje ten język programowania. Po pierwsze pozwala korzystać za darmo praktycznie ze wszystkich bibliotek związanych z danymi przestrzennymi w szczególności z *GDAL/OGR*. Dodatkowo ułatwia prace na danych rastrowych dzięki bibliotece *numpy*. Graficzna biblioteka *PyQt* pozwala budować interfejsy graficzne, które mogą być dołączane jako nakładki do istniejących już aplikacji przestrzennych i przenoszone między systemami operacyjnymi bez potrzeby modyfikacji ich kodu. 

## GDAL/OGR
GDAL (*Geospatial Data Abstraction Library*) jest biblioteką rozwijaną przez fundację *OSGEO* jako wolne oprogramowanie służącą do operacji na danych rastrowych. Zawiera w sobie również bibliotekę OGR (*Simple Features Library*) do danych wektorowych. Obecnie obsługuje około 140 formatów danych rastrowych oraz 80 wektorowych. Obie biblioteki napisane zostały w języku C++, ale posiadają bindingi dla innych języków w tym dla Pythona. Istotną rzeczą jest, że GDAL jest tak naprawdę zbiorem odrębnych programów tzw. *utility programs*, które możemy wywołać z linii komend. Wszystkie dostępne operacje znajdują się na stronie biblioteki GDAL [1]. 

## Pierwsza aplikacja z użyciem GDAL/OGR

Aby rozpocząć przygodę z GDAL pierwszym krokiem jaki należy wykonać jest sprawdzenie czy nie posiadamy zainstalowanej wersji GDAL. Oczywiście nasuwa się nam pytanie – dlaczego mam sprawdzać, skoro nigdy go nie instalowałem.  Warto jednak wiedzieć, że GDAL z racji swojej dużej użyteczności i popularności jest często instalowany razem z innymi programami (Google Earth, QGis, ArcGIS). Dla tych którzy jednak nie posiadają GDAL odsyłam na stronę [2], na której krok po kroku wytłumaczone zostało jak zainstalować bibliotekę różnymi metodami. 

Zacznijmy od najprostszego przykładu prezentującego w jaki sposób następuje odczyt danych rastrowych.
_KOD_

Z modułu GDAL należy wywołać metodę *Open* ze ścieżką dostępu jako parametrem (najlepiej w postaci bezwzględnej). Metoda zwraca całego rastra (*Dataset*) w przypadku prawidłowego odczytania rastra. Mając wczytanego rastra pierwszą rzeczą, której możemy dokonać jest sprawdzenie takich wartości jak ilość kanałów (*RasterCount*), ilość wierszy (*RasterXSize*), ilość kolumn (*RasterYSize*), z których składa się raster. Możemy również sprawdzić georeferencję rastra czyli jego położenie w przestrzeni. 

_KOD_

Teraz spróbujmy otworzyć plik wektorowy. Przed otwarciem pliku ustawiamy sterownik (*Driver*), który jest obiektem odpowiadającym za poprawne wczytanie odpowiedniego typu danych. Ważne jest również by przy pierwszym otwarciu pliku ustawić prawa dla sterownika, w zależności od tego czy chcemy odczytywać czy zapisywać dane. Domyślnym prawem jest prawo do odczytu oznaczane zerem. Jedynka oznacza możliwość modyfikacji pliku i jego ponownego zapisu. Nie wszystkie wspierane przez OGR formaty posiadają opcję zapisu. Metoda Open zwraca obiekt zwany źródłem danych (*DataSource*).

_KOD_

Źródło danych składa się z warstw, które pobieramy za pomocą funkcji *GetLayer*. Najbardziej podstawowy format wektorowy Shapefile posiada tylko jedną warstwę, dlatego użycie indeksu jest opcjonalne. Przy pozostałych formatach ustawienie indeksu jest obowiązkowe. W celu sprawdzenia ilości warstw możemy wywołać funkcję *GetLayerCount*. 

_KOD_

Po pobraniu warstwy jesteśmy w stanie odczytać podstawowe informacje o obiektach w niej zawartej. Możemy sprawdzić z ilu obiektów składa się warstwa (*GetFeatureCount*) oraz sprawdzić zasięg warstwy podany we współrzędnych geograficznych (GetExtent). Najważniejsze jednak jest to, że możemy pobrać każdy obiekt po to by móc odczytać jego geometrię, nazwę oraz wartości jakie w sobie przechowuje. 
Jak widzimy odczyt danych wektorowych jest bardziej skomplikowany i dobranie się do struktury danych wymaga przejścia przez kilka poziomów co przy bardzo dużej ilości danych powoduje opóźnienia. 


## Ale skąd te dane ?

W podanych przykładach korzystaliśmy z danych rastrowych i wektorowych, które były dołączone jako przykładowe dane do testowania. Pisanie własnej aplikacji wymaga od nas jednak rzeczywistych danych dostosowanych do naszych potrzeb. Oto kilka źródeł z których możemy pobierać bezpłatnie potrzebne nam dane:

* Centralny Ośrodek Dokumentacji Geodezyjnej i Kartograficznej [3]
* Centralna Baza Danych Geologicznych [4]
* Geoportal 2 [5]
* OpenStreetMap [6]
* USGS [7]
* ESA/Sentinel [8]

## Wizualizacja danych

Odczyt danych i ich analiza to jednak nie wszystko. Aby nasza aplikacja miała możliwość podglądu danych potrzebujemy bibliotekę graficzną. O ile z wyświetleniem rastrów nie mamy problemów – możemy do ich wyświetlenia użyć dowolnej graficznej biblioteki Pythona np. *PyQt*. O tyle z wektorami jest już problem. Podobnie jak w przypadku rastrów możemy skorzystać z gotowych komponentów graficznych wbudowanych w bibliotekę PyQt np. QPainter do rysowania obiektów. Niestety rozwiązanie to ma jedna wadę – nie jest efektywne dla obiektów mających powyżej tysiąca wierzchołków. Wyświetlenie konturu jednego województwa zajmuje ułamki sekund, niestety wyświetlenie całej mapy Polski z konturami województw zajmuje już kilkanaście sekund. Z pomocą przychodzą biblioteki dedykowane do wizualizacji danych wektorowych. Jedną z nich jest *matplotlib* wraz z rozszerzeniem *Basemap*. Jest on odpowiednikiem znanego z Matlaba narzędzia zwanego Mapping Toolbox. Jego główną zaletą jest to, że sam dokonuje odwzorowania kartograficznego czyli transformacji współrzędnych geograficznych na współrzędne rysunku. Po drugie zawiera sporo funkcji ułatwiających rysowanie danych przestrzennych m.in. rysowanie równoleżników, południków, rysowane i wygładzanie granic i wybrzeży. Minusem jest jednak słabo opisana dokumentacja.

## Odwzorowanie kartograficzne a co to takiego ?

## Pozostałe biblioteki 

Wspomniane *GDAL/OGR* nie jest jedyną biblioteką wspomagającą przetwarzanie danych przestrzennych. Obecnie dostępnych darmowo mamy kilkanaście bibliotek, szczególnie do manipulacji danymi wektorowymi. Z nich najbardziej znane to Fiona i Shapely. Obie są typowymi bibliotekami Pythona, które pozwalają na odczyt danych wektorowych, tworzenie nowych geometrii, sprawdzanie poprawności geometrii oraz wszelkiego rodzaju operacje geometryczne. Wybór odpowiedniej biblioteki jest w znacznym stopniu zależny od stopnia zaawansowania operacji, które będą wykonywane w pisanej aplikacji. Pod tym względem niewątpliwie liderem jest GDAL/OGR, który dostarcza najwięcej gotowych funkcjonalności. Niemniej GDAL/OGR może być problematyczny ze względu na fakt, że jego struktura oparta jest na C++. Fiona i Shapely opierają się na standardach Pythona m.in. korzystając z plików, słowników czy iteratorów co jest znacznie wygodniejsze poprzez zmniejszenie ilości pisanego kodu.

_KOD_



## Źródła
* [1] Strona GDAL z listą wszystkich operacji http://www.gdal.org/gdal_utilities.html
* [2] Instalacja GDAL/OGR http://www.gis.usu.edu/~chrisg/python/2009/install.html
* [3] Centralny Ośrodek Dokumentacji Geodezyjnej i Kartograficznej http://www.codgik.gov.pl/index.php/darmowe-dane.html
* [4] Centralna Baza Danych Geologicznych http://baza.pgi.gov.pl/
* [5] Geoportal http://www.geoportal.gov.pl/web/guest/DOCHK
* [6] OpenStreetMap http://download.geofabrik.de/
* [7] USGS http://earthexplorer.usgs.gov/
* [8] ESA/Sentinel https://scihub.copernicus.eu/dhus/#/home
* [9] Systemy GIS http://wazniak.mimuw.edu.pl/images/9/9a/Systemy_mobilne_wyklad_8.pdf
* [10] Dane przestrzenne http://www.igik.edu.pl/pl/a/Dane-przestrzenne-def
* [11] Wykorzystanie języka Python w GIS http://gis-support.pl/wykorzystanie-jezyka-programowania-python-w-quantum-gis/




