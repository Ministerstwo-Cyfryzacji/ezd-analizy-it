Analiza mikroserwisów
=====================

Streszczenie
------------

Cel
~~~

Podjęcie decyzji, czy platforma aplikacje.gov.pl będzie budowana w
oparciu o wzorzec mikroserwisów
(`http://microservices.io/ <http://microservices.io/>`__).

Sposób realizacji
~~~~~~~~~~~~~~~~~

Analiza korzyści i zagrożeń wynikających z podjęcia decyzji o zbudowaniu
platformy zgodnie ze wzorcem mikroserwisów. Analiza wagi poszczególnych
argumentów. Na podstawie tej analizy podjęcie decyzji w sprawie użycia
wzorca mikroserwisów.

Konkluzja
~~~~~~~~~

Wzorzec mikroserwisów bardziej niż architektura monolityczna odpowiada
potrzebom i założeniom aplikacji.gov.pl. Aby podjąć decyzję
wykorzystania tego wzorca konieczne są konsultacje z zespołem
odpowiedzialnym za infrastrukturę chmury w sprawie jej wydajności oraz
konsultacje z zespołem odpowiedzialnym za certyfikację aplikacji w
sprawie kosztów i przebiegu procesu certyfikacji.

Analiza
-------

Korzyści wykorzystania mikroserwisów
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Różne części systemu mogą być tworzone w różnych technologiach.
-  Zarządzanie zmianą i deploymentem jest łatwiejsze, niż w przypadku
   architektury monolitycznej.
-  Łatwa wymiana konkretnej usługi na inną, w innej technologii,
   implementującą to samo API.
-  Zarządzanie separacją aplikacji jest łatwiejsze, niż w przypadku
   architektury monolitycznej, co może wpływać pozytywnie na poziom
   bezpieczeństwo rozwiązania.
-  Rozpraszanie aplikacji pomiędzy wiele maszyn jest łatwiejsze, niż w
   przypadku architektury monolitycznej.

Ryzyka
~~~~~~

W dyskusjach środowiskowych o stosowaniu mikroserwisów pojawiają się
argumenty przeciwko temu rozwiązaniu [1]_. Przygotowując analizę
zebraliśmy je w jednym miejscu punktując zasadność krytyki.
Podzieliliśmy ryzyka na takie, które zgodnie z przytoczonymi przez nas
argumentami są niezasadne lub nie wpływają w istotny sposób na poziom
trudności systemu i takie, które istotnie występują i muszą być wzięte
pod uwagę w podejmowaniu decyzji o wykorzystaniu mikroserwisów.

+-----------------------------+----------------------------------------+
|Ryzyko                       |Kontrargumenty                          |
+=============================+========================================+
|Różne części systemu mogą być|Zrozumienie całego kodu na szczegółowym |
|tworzone w różnych           |poziomie nie jest potrzebne. Aby nie    |
|technologiach, co utrudnia   |musieć tego robić w systemach           |
|zrozumienie całego kodu na   |informatycznych stosuje się różnego     |
|szczegółowym poziomie.       |rodzaju abstrakcje (system operacyjny,  |
|                             |język programowania,                    |
|                             |protokoły). Konieczność rozumienia      |
|                             |całego kodu oznacza źle                 |
|                             |zaprojektowany/napisany system.         |
+-----------------------------+----------------------------------------+
|Różne części systemu mogą być|Automaty pomagające w procesie          |
|tworzone w różnych           |wytwórczym mogą być niezależne dla      |
|technologiach, co może       |każdego serwisu z osobna.               |
|utrudnić stosowanie automatów|                                        |
|pomagających w procesie      |                                        |
|wytwórczym (np continuous    |                                        |
|integration).                |                                        |
+-----------------------------+----------------------------------------+
|Automaty pomagające w        |Testy dla całości nie są wykluczone. Nie|
|procesie wytwórczym mogą być |są przypisane do jakiegokolwiek serwisu,|
|niezależne dla każdego       |tylko do ich zestawu tworzącego         |
|serwisu z osobna. Co budzi   |platformę. Aplikacja może także zawierać|
|niepokój, że nie ma testów   |testy integracyjne uruchamiające inne   |
|dla całości.                 |aplikacje i sprawdzające poprawność ich |
|                             |współpracy.                             |
+-----------------------------+----------------------------------------+
|Pozostawienie dowolności     |Zła struktura kodu i źle dobrany język  |
|technologicznej wykonawcy    |programowania jako cechy same w sobie   |
|usługi może skutkować        |nie są szkodliwe. Szkodliwe są dopiero  |
|powstaniem kodu o złej       |skutki: złe funkcjonowanie, luki        |
|strukturze lub nietrafnym    |bezpieczeństwa, trudności w             |
|wyborem języka programowania.|modyfikowaniu danej mikrousługi.        |
+-----------------------------+----------------------------------------+
|Możliwość tworzenia w różnych|Odpowiednie testy integracyjne          |
|technologiach może spowodować|(niezależne od implementacji) powinny   |
|używanie przestarzałych lub  |wykryć złe funkcjonowanie.              |
|źle zaprojektowanych         |                                        |
|narzędzi, co może prowdzić do|                                        |
|niepoprawnego funkcjonowania |                                        |
|serwisów.                    |                                        |
+-----------------------------+----------------------------------------+
|Możliwość tworzenie w różnych|Testy bezpieczeństwa mikroserwisu nie są|
|technologiach może spowodować|istotnie trudniejsze od testów          |
|używanie używanie            |bezpieczeństwa aplikacji                |
|przestarzałych lub źle       |monolitycznej. Problemem jest jednak    |
|zaprojektowanych narzędzi,   |m.in. fakt, że należy takich testów     |
|które wprowadzą dziury       |wykonać wiele, po jednym na każdy       |
|bezpieczeństwa.              |mikroserwis.                            |
+-----------------------------+----------------------------------------+
|Możliwość tworzenia w różnych|Zapewnienie kodu o dobrej jakości nie   |
|technologiach może spowodować|powinno być wymuszane przez strukturę   |
|używanie niewłaściwie        |techniczną platformy. Powinno być       |
|dobranych narzędzi i         |wymuszane przez proces certyfikacji lub |
|powstawanie kodu o           |umowy z wykonawcami poszczególnych      |
|niewłaściwej strukturze, co  |aplikacji.                              |
|utrudni rozwój istniejących  |                                        |
|serwisów.                    |Można też stwierdzić, że jakość kodu    |
|                             |pojedynczej mikrousługi nie jest ważna w|
|                             |kontekście możliwości jej rozwijania.   |
|                             |Jako, że mikrousługa jest mała pod      |
|                             |względem pełnionych funkcji, napisanie  |
|                             |jej od nowa nie powinno być bardzo      |
|                             |kosztowne.                              |
+-----------------------------+----------------------------------------+
|Niezależność mikroserwisów   |Łatwość modyfikacji nie wpływa          |
|polegająca na łatwym         |negatywnie na możliwość wykonywania     |
|przebiegu ich modyfikacji    |większych zmian. Wygląda na to, że      |
|może być w niektórych        |problem tkwi w śledzeniu przez jakie    |
|sytuacjach postrzegana jako  |mikroserwisy używana jest część Y       |
|problem, na przykład wtedy,  |mikroserwisu X. Ten problem będzie      |
|gdy potrzebna będzie zmiana  |rozwiązany przez dokładne specyfikacje  |
|większej części systemu.     |API. W mikroserwisie Z nie korzystamy z |
|                             |mikroserwisu X, lecz z określonego      |
|                             |API. Zmiana API będzie stanowić problem,|
|                             |jednak nie jest to przypadłość tylko    |
|                             |mikroserwisów.                          |
+-----------------------------+----------------------------------------+
|W przypadku mikroserwisów    |Więcej standardowych, opisanych         |
|API, dla których wprowadzanie|protokołów jest pozytywnym aspektem.    |
|zmian jest trudnym procesem, |                                        |
|będzie znacznie więcej niż w |                                        |
|przypadku monolitu.          |                                        |
+-----------------------------+----------------------------------------+
|Trudniejsze zarządzanie      |Problem tkwi w zapewnieniu atomowości   |
|transakcjami.                |operacji, które angażują wiele          |
|                             |mikroserwisów. Kontrargumentem jest, że |
|                             |takich serwisów nie powinno być. Atomowe|
|                             |operacje powinny być realizowane        |
|                             |wewnątrz jednego serwisu. Z tego powodu |
|                             |operacje atomowe powinny być małe.      |
+-----------------------------+----------------------------------------+
|Wraz ze wzrostem każdej      |Niezależne aplikacje nie muszą być      |
|niezależnej aplikacji,       |tworzone przez jeden zespół. W momencie |
|wzrasta też zespół ją        |gdy osiągają stosunkową stabilność      |
|tworzący – może przyczynić   |(wprowadzane zmiany nie zmieniają API w |
|się to do problemów w        |poważny sposób) zespoły mogą pracować   |
|koordynacji. Łatwiej kierować|oddzielnie.                             |
|jednym, sporym zespołem,     |                                        |
|tworzącym spójną całość i    |                                        |
|pracującym nad jednym kodem, |                                        |
|niż nad wieloma zespołami    |                                        |
|pracującymi nad niezależnymi |                                        |
|aplikacjami, które powinny   |                                        |
|tworzyć spójną całość.       |                                        |
+-----------------------------+----------------------------------------+
|Mikroserwisy są stosunkowo   |Pisanie pojedynczego mikroserwisu to    |
|nowym stylem programowania,  |pisanie (w zamierzeniu małego - pod     |
|który wymaga bardziej        |względem funkcji) programu, który ma    |
|skomplikowanych narzędzi niż |API. Nie jest to nic                    |
|monolityczny                 |nowego. Uruchamianie wielu takich       |
|odpowiednik. Jest też mniej  |programów w celu stworzenia             |
|znany, dlatego na i tak już  |funkcjonującego produktu może nastręczać|
|małym rynku, znajduje się    |trudności (z powodu swojej żmudności i  |
|mniej specjalistów wdrożonych|konieczności odpowiedniej               |
|w system. Może więc generować|konfiguracji). Zadaniem zespołu         |
|większe koszty podczas       |tworzącego środowisko deweloperskie jest|
|tworzenia aplikacji,         |zapewnienie narzędzia, które w łatwy    |
|zwłaszcza na początku.       |sposób uruchamia wiele mikroserwisów.   |
+-----------------------------+----------------------------------------+

Ryzyka, które należy zestawić z korzyściami:

- Trudniej jest dbać o bezpieczeństwo małych, rozproszonych systemów,
  niż o jeden spory system. Problem ten może też generować dodatkowe
  koszta.
- Testowanie integracyjne jest trudniejsze, niż w przypadku architektury
  monolitycznej.
- Większe zużycie pamięci operacyjnej, niż w przypadku rozwiązań
  monolitycznych.
- Komunikacja między serwisami będzie obciążać sieć. Obciążenie będzie
  większe, niż w przypadku rozwiązań monolitycznych, których komponenty
  mogą komunikować się za pomocą wspólnej pamięci.

Podsumowanie
------------

Argumenty za i przeciw wykorzystaniu wzorca mikroserwisów są
porównywalnej siły.

Uwzględniając, że aplikacje uruchamiane na platformie będą wytwarzane
przez niezależnych producentów zapewnienie swobody technologicznej jest
ważnym aspektem. Bez takiej swobody zbiór potencjalnych producentów
aplikacji zostałby ograniczony.

Oprócz EZD i innych planowanych aplikacji, będzie potrzeba uruchamiania
na platformie innych aplikacji. Postaci tych aplikacji nie sposób
przewidzieć. Ograniczanie jej może ograniczyć możliwe do zrealizowania
funkcje. Architektura mikroserwisów, przez zapewnienie swobody
technologicznej, zapewnia małe ograniczenia.

Jednym z założeń Platformy jest zmiana, możliwość wymiany komponentów
bez zmiany całego systemu oraz możliwość dodawania dodatkowych
aplikacji. Architektura mikroserwisów dobrze wpisuje się w to założenie,
ponieważ ułatwia niezależne zmiany poszczególnych serwisów.

Poważnym argumentem przeciw mikroserwisom jest trudność w zapewnieniu
bezpieczeństwa. Eliminacja luk bezpieczeństwa wynikających ze stosowania
różnych technologii, z których niektóre mogą być wadliwe, wymaga nakładu
pracy. Każdy serwis musi być sprawdzony, czy nie zawiera luk. Ten sam
problem występowałby jednak (choć w mniejszej skali) także w sytuacji,
gdyby moduły wielu producentów kooperowały w ramach jednego systemu -
oba rozwiązania wymagają certyfikacji aplikacji pod kątem
bezpieczeństwa.

Rekomenduje się wykorzystanie wzorca mikroserwisów w budowie platformy
aplikacje.gov.pl. Aby wybrać to rozwiązanie konieczne jest
przeprowadzenie dodatkowych konsultacji z zespołem odpowiedzialnym za
architekturę chmurową i bezpieczeństwo docelowej platformy.

.. [1]
   Niektóre z nch zostały wyrażone we wpisie na blogu:
   `http://www.ictshop.pl/czym-sa-mikrouslugi/ <http://www.ictshop.pl/czym-sa-mikrouslugi/>`__
