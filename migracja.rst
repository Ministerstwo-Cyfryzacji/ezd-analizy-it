Model migracji z systemów EZD PUW i eDok
========================================

Wstęp
-----

Operacja migracji danych z instalacji istniejącego systemu EZD do innego system EZD, powinna być
realizowana za pośrednictwem pliku pośredniego (lub pewnej liczby plików).

Dane pośrednie będą zapisane w formacie, który jest wspólny dla obu systemów.

Migracja a eksport wybranych danych
-----------------------------------

Przekazywanie danych z systemu EZD może mieć miejsce w kilku sytuacjach. Każdy system EZD musi mieć
zaimplementowane operacje eksportu (przekazywania) danych do innych systemów. Przynajmniej:


- przesyłanie dokumentów do innych systemów teleinformatycznych, w szczególności przez eksport
  dokumentów i ich metadanych z zachowaniem powiązań pomiędzy tymi dokumentami i metadanymi
  (zob. §6 pkt 14 `Rozporządzenia Ministra Spraw Wewnętrznych i Administracji z dnia 30 października 2006 r. w sprawie szczegółowego sposobu postępowania z dokumentami elektronicznymi`_)

.. _Rozporządzenia Ministra Spraw Wewnętrznych i Administracji z dnia 30 października 2006 r. w sprawie szczegółowego sposobu postępowania z dokumentami elektronicznymi: http://isap.sejm.gov.pl/DetailsServlet?id=WDU20062061518

- wytworzenie „paczki archiwalnej” z dokumentami, do przekazania do Archiwum Państwowego oraz spisu
  zdawczo-odbiorczego odpowiadającego określonej paczce archiwalnej (zob. §6 pkt 13 – 14
  `Rozporządzenia MSWiA z dnia 30 października 2006`_ wspomnianego wyżej)

.. _Rozporządzenia MSWiA z dnia 30 października 2006: http://isap.sejm.gov.pl/DetailsServlet?id=WDU20062061518

Do marca 2017r. jest też realizowany projekt we współpracy MC - COI - NSA - WSA - PUW, mający na celu
przygotowanie formatu danych, który będzie używany do przekazywania danych z systemów EZD
do systemu do obsługi postępowania sądowoadministracyjnego realizowanego w ramach projektu OPSAD [1]_.

Wydaje się rozsądnym, by do wszystkich operacji masowego produkowania danych do wymiany używany
był syntaktycznie ten sam format danych — a tworzone pliki różniły się tylko zakresem danych,
w zależności od zamierzonego celu eksportu.

Model danych
------------

Dane przechowywane w systemie
+++++++++++++++++++++++++++++
  
Rysunek poniżej ilustruje fakt, iż przeniesienie danych z jednego systemu do drugiego jest możliwe
tylko dla danych takich, które są modelowane w obydwu systemach i są dla systemu docelowego
"interesujące" (użytkownicy systemu potrzebują takich danych w swej działalności).

.. image:: images/dane_dwoch_systemow.png

To, że w jednym systemie nie ma pewnych danych, które są obecne w innym systemie może wynikać
z braku odpowiednich pól albo z ograniczeń zastosowanego modelu danych.

Można też rozróżnić dane które mogą być w jakimś systemie gromadzone i przechowywane, od danych
których potrzebują użytkownicy do realizacji swoich zadań. (Znane są przykłady systemów, gdzie
niektórym kategoriom użytkowników ograniczano dostęp do pewnych danych nie dlatego iż dane te nie
powinny być im udostępniane, ale dlatego by nie rozpraszać uwagi pracownika nieistotnymi na jego
stanowisku szczegółami.)


Format wymiany danych
---------------------

Do wymiany danych między nie-współpracującymi on-line systemami, stosowane bywają formaty:

- tekstowe, zwykle oparte na składni XML
- binarne, specyficzne dla rozwiązania; ostatnio popularność zyskuje stworzony przez Google format
  "Protocol Buffers".

Zaletą formatów opartych na XML jest możliwość łatwego czytania pliku tekstowego przez człowieka.
Zaletą binarnych formatów jest ich mniejszy rozmiar w stosunku do formatów tekstowych. 

Istniejące  w kraju rozwiązania
+++++++++++++++++++++++++++++++

W załączniku do `Rozporządzenia Ministra Spraw Wewnętrznych i Administracji z dnia 2 listopada 2006 r. w sprawie wymagań technicznych formatów zapisu i informatycznych nośników danych, na których utrwalono materiały archiwalne przekazywane do archiwów państwowych <http://isap.sejm.gov.pl/DetailsServlet?id=WDU20062061519>`_
Dz.U. 2006 nr 206 poz. 1519 (dalej zwane rozporządzeniem 1519 [2]_), podano techniczne informacje
dotyczące sposobu zapisu przekazywanych danych, w szczególności zdefiniowano format danych.

Format ten powstał w wyniku prac interdyscyplinarnej grupy specjalistów z Ministerstwa Spraw Wewnętrznych
i Administracji, Archiwów Państwowych oraz Naukowej i Akademickiej Sieci Komputerowej (NASK) i Uniwersytetu
Warszawskiego. Systemy EZD PUW i eDok posiadają funkcjonalności generujące paczki danych zgodne z tym
formatem. Format ten jest nazywany „formatem paczki archiwalnej”, zgodnie z definicją zawartą w §6 ust. 4
rozp. 1519. Ze względu na rozmiar, nie cytujemy go w tym miejscu; w całości można go znaleźć w załączniku
do rozporządzenia 1519, jak również w zasobach portalu
`Archiwów Państwowych <https://ade.ap.gov.pl/ndap-walidator/downloadmeta.do>`_
(tamże jako Metadane-1.6.xsd)

Modyfikacje obowiązującego formatu wymiany danych
+++++++++++++++++++++++++++++++++++++++++++++++++

Format ustanowiony w załączniku rozporządzenia 1519 został określony zgodnie z zasadami schematu XML.
Schemat XML jest strukturą techniczną ustalającą nazewnictwo i układ wyodrębnionych elementów
metadanych, w tym ich kolejność występowania i powiązania między nimi oraz wymagalność danych.
Taki schemat nie wyjaśnia jednak sposobu rozumienia poszczególnych elementów metadanych, na przykład
nie jest jednoznacznie jasne jakie metadane powinny być przekazane w ramach elementów oznaczanych
jako <Tworca>, <Dostep>, <Grupowanie> itd.

Nie wyjaśnia tego także rozporządzenie o którym mowa w §3 ust. 2 i 3  rozp. 1519, czyli
`Rozporządzenie Ministra Spraw Wewnętrznych i Administracji z dnia 30 października 2006 r. w sprawie niezbędnych elementów struktury dokumentów elektronicznych <http://isap.sejm.gov.pl/DetailsServlet?id=WDU20062061517>`_, Dz.U. 2006 nr 206 poz. 1517 (zwane rozporządzeniem 1517 [3]_).

Celem ustalenia dobrej praktyki, powstała propozycja objaśnień dla metadanych stanowiących niezbędne
elementów struktury dokumentów elektronicznych, stanowiąca jednocześnie uzupełnienie dla dokumentu
schematu XML [4]_ (nie stała się ona jednak dokumentem obowiązującym).
Planowane jest opublikowanie nowej propozycji objaśnień oraz ustanowienie niewielkich zmian formatu
paczki archiwalnej, mając na uwadze kilkuletnią praktykę stosowania systemów EZD w administracji.
Propozycja zostanie przedstawiona jako materiał roboczy, pośród zasobów Ministerstwa Cyfryzacji
na GitHub, do dyskusji z zainteresowanymi.

Ustalenie objaśnień jest ważne, gdyż oczywiste w schemacie XML ograniczenia dotyczące wymagalności
poszczególnych elementów metadanych, mogą przekładać się na bardzo ograniczony zestaw eksportowanych
danych w stosunku do danych zgromadzonych w systemie EZD. Na przykład, nie każdy dokument powstający
w systemie EZD ma określonego adresata i co za tym idzie nie może to być element wymagany schematem XSD.
W efekcie metadane pozbawione informacji o adresatach (odbiorcach) dokumentów będą automatycznie
uznawane jako zgodne z rozporządzeniem 1519, gdyż paczka archiwalna będzie technicznie zgodna
ze schematem XM. Mimo że takie dane nie będą zgodne z ogólnymi wytycznymi wynikającymi w art. 6
ust. 1 i 1a ustawy o narodowym zasobie archiwalnym i archiwach (Dz.U. 2016, poz.1506) gdyż paczka
nie będzie zawierała kompletu danych odzwierciedlających sposób załatwiania sprawy, to nie da się
tego zweryfikować automatycznie.

Oprócz ustaleń dotyczących wymaganej kompletności danych, objaśnienia mogą być przydatne dla ustalenia
przeznaczenia elementów zdefiniowanych schematem XML oraz dobrej praktyki ich wykorzystania
do przekazania informacji w sposób odzwierciedlający przebieg załatwiania i rozstrzygania spraw,
np. w celu wskazania w jaki sposób przekazywać ujednoliconą informację o dekretacjach i akceptacjach
w systemach EZD, informacje o potwierdzeniu doręczenia itd. Brak takich informacji stanowiłby
zubożenie w stosunku do papierowych akt spraw, w których dekretacje nanoszone wprost na dokumenty
papierowe stanowią naturalną część dokumentacji.


Wymagania interoperacyjne
+++++++++++++++++++++++++

Formaty danych oparte na składni XML są bardzo rozpowszechnione.
Ze względu na fakt obowiązywania rozporządzenia 1519, które wprowadza format oparty na XML, oraz
na fakt, że format proponowany do eksportu danych z EZD do sądów administracyjnych będzie
prawdopodobnie oparty na składni XML i biorąc pod uwagę postulat podany na początku, by do wszystkich
operacji masowego produkowania danych do wymiany, używany był syntaktycznie ten sam format danych — należy
przyjąć, że format używany do migracji danych powinien używać składni XML i powinien mieć podobną budowę.

Należy podkreślić, że bardzo ważnym jest, by formaty przeznaczone do

- przekazywania paczki danych do innego EZD
- przekazywania paczki danych do archiwum państwowego
- przekazywania paczki danych do sądu
- migracji danych

były podzbiorami tego samego, spójnego, generalnego formatu wymiany danych.
Spójność tych formatów znacznie ułatwi ich implementację, a zwłaszcza spowoduje duże ułatwienie
w przyszłości, w sytuacjach gdy pojawi się potrzeba rozszerzenia zakresu przekazywanych danych.

.. image:: images/zakres_danych_formatu_wymiany.png

Niestety, realizacja tego postulatu może być zagrożona przez fakt, iż prace nad formatem przekazywania
danych dla sądów administracyjnych mają się zakończyć w marcu 2017 i mogą doprowadzić do powstania
formatu który nie będzie opierał się o schemat XML ustalony w rozporządzeniu 1519 i, co za tym idzie,
może być niezgodny technicznie z paczką archiwalną lub paczką danych do migracji.


Dane, których może nie być w paczce archiwalnej
+++++++++++++++++++++++++++++++++++++++++++++++
                   
Paczka archiwalna została zdefiniowana w celu określenia uporządkowanego sposobu przekazywania
do archiwów państwowych, dokumentacji stanowiącej materiały archiwalne w postaci elektronicznej.
W systemach EZD gromadzone są jednak także takie dane, które odnoszą się do materiałów archiwalnych
ale nie muszą być eksportowane do paczki archiwalnej, a powinny być ujęte w paczce migracji.
O tym, czy dane są umieszczane w paczce archiwalnej, decyduje kwalifikacja dokumentacji do określonej
kategorii archiwalnej. Przy migracji, należy przekazać wszystko potrzebne do odtworzenia danych
w innym środowisku.

Przykładowo, w systemie EZD mogą być zbierane wszelkie robocze tymczasowe wersje pism jakie są
przygotowywane w trakcie załatwiania spraw, które nie zostały jeszcze przekazane do akceptacji,
tylko zostały tymczasowo zapisane w celu zabezpieczenia pracy w danym dniu. Dotyczy to zwłaszcza
rozbudowanych dokumentów analitycznych wymagających dłuższej pracy. Takie tymczasowe wersje zapisywane
automatycznie przez system EZD, mogłyby stanowić niepotrzebne i nadmiarowe obciążenie paczki archiwalnej,
gdyż dla celów archiwalnych mogłyby wystarczyć tylko wersje przekazane do akceptacji lub zapisane
jako ostatecznie dokończona wersja. Kwestie te wymagają wyjaśnienia z archiwami państwowymi.
Być może nawet, w niektórych przypadkach, właśnie dla celów archiwalnych powinno się przekazywać
całkowicie kompletne dane, ze wszystkim wersjami roboczymi gdyż tylko takie pozwolą po wielu latach
prześledzić faktyczne zaangażowanie pracowników w załatwianie sprawy. Może być także odwrotnie — być
może migrując / przenosząc dane z systemu EZD do innego systemu EZD nie będzie potrzeby przekazywania
wcześniejszych wersji roboczych (wystarczy ostatnia zapisana + wszystkie zaakceptowane, jeżeli są).

Z drugiej strony systemy EZD zapewniają niekiedy funkcjonalności, które co do zasady prowadzą
do tworzenia dokumentacji niestanowiącej akt spraw i niebędącej materiałami archiwalnymi jak
np. obsługa wniosków urlopowych, zamawianie sal konferencyjnych, zgłaszanie usterek sprzętu
i oprogramowania itd. W przypadku migracji takich danych między systemami EZD zastosowanie
formatu paczki archiwalnej może niepotrzebnie komplikować tę migrację.


Dane o ograniczonej dostępności (niejawne)
++++++++++++++++++++++++++++++++++++++++++

Dane niejawne są przechowywane odrębnie i nie będą przedmiotem procedury automatycznej migracji
danych EZD. Ich przeniesienie musi być zrealizowane odrębną procedurą.

Wnioski
-------

- podstawą do zaprojektowania formatu danych używanych przy migracji, powinien być (zmodyfikowany)
  format używany obecnie do eksportu danych z systemów EZD do archiwów państwowych, rozszerzony o

  - dane które są przechowywane w EZD ale nie są gromadzone przez archiwa państwowe (np. dane
    o archiwalnych kategoriach B, Bc, BE)
  - dane, które nie wchodzą w skład spraw i z tego powodu nie są umieszczane w paczkach archiwalnych
    (jakie ? — do wyjaśnienia)

- format migracji powinien też umożliwić przekazywanie spraw niezakończonych, które w zwykłej paczce
  archiwalnej nie występują
- ponieważ istniejące instalacje EZD PUW i eDok mają już zaimplementowane operacje tworzenia paczki
  archiwalnej, należy — w porozumieniu z Autorami tamtych systemów — uzgodnić jedną z dwóch metod
  tworzenia plików z danymi, których w paczkach archiwalnych nie ma: albo poprzez rozszerzenie
  zakresu eksportowanych danych w operacji tworzenia paczki archiwalnej (co uczyniłoby ją
  „paczką migracyjną”) albo poprzez utworzenie odrębnej funkcji eksportowania danych nie dotyczących
  spraw. Nowo tworzony system EZD RP powinien umieć czytać dane zarówno

  - paczki archiwalnej (tak jak jest w tej chwili zdefiniowana)
  - paczki migracyjnej
  - paczki z danymi nie umieszczanych w paczkach archiwalnych. Te funkcje będą zapewne przydatne
    także przy migracji danych z innych systemów EZD.

- należy próbować uczestniczyć w pracach dotyczących zaproponowania formatu do przekazywania
  EZD --> sądowy system OPSAD, i dbać o to by utworzony format mógł być zgodny z formatem generalnym.

=========

.. [1] http://krmc.mc.gov.pl/krm/posiedzenia/posiedzenia-krmc-2016-r/posiedzenie-w-dniu-0711/materialy-na-posiedzeni/3289,Naczelny-Sad-Administracyjny.html.

.. [2] http://isap.sejm.gov.pl/DetailsServlet?id=WDU20062061519

.. [3] http://isap.sejm.gov.pl/DetailsServlet?id=WDU20062061517

.. [4] https://www.archiwa.gov.pl/pl/zarzadzanie-dokumentacja/dokument-elektroniczny/projekt-obja%C5%9Bnie%C5%84-do-element%C3%B3w-struktury

