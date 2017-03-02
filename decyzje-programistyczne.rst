Poniższy dokument odzwierciedla stan nieaktualny. Architektura platformy została zasadniczo zmieniona, a co za tym idzie, także decyzje programistyczne. Dokument jest pozostawiony w repozytorium w celu dokumentacji przebiegu procesu badawczego.

Decyzje programistyczne — platforma aplikacje.gov.pl
====================================================

W dokumencie opisane zostały decyzje programistyczne i decyzje dotyczące architektury IT podjęte w trakcie pracy nad platformą aplikacje.gov.pl.

Spis treści
-----------

* `Decyzje`_

  * `Język programowania`_
  * `Modułowość`_
  * `Wybór bibliotek`_

    * `Framework webowy`_

  * `Wstrzykiwanie zależności`_
  * `Uwierzytelnianie`_
  * `Dziennik zdarzeń`_

* `Rozważania`_

  * `Modułowość w Javie`_

    * `Modułowość w Javie w wersjach <= 8`_
    * `Modułowość w Javie 9`_
    * `Konkluzja i rozwiązanie problemu`_

Decyzje
-------

Język programowania
~~~~~~~~~~~~~~~~~~~

Jako język w którym powstanie backendowa część platformy aplikacje.gov.pl wybrano Javę. Oprócz argumentów technologicznych (dostępność bibliotek, statyczne typowanie itp.) ważnym argumentem była dostępność programistów tego języka w Centralnym Ośrodku Informatyki, we współpracy z którym tworzymy projekt. Istotnym powodem wziętym pod uwagę przy wyborze języka programowania był też fakt, że kod systemu eDok4, który może zostać użyty w Platformie, jest również napisany w Javie.

Modułowość
~~~~~~~~~~

Podjęto decyzję, że poszczególne moduły (być może napisane przez zewnętrznych dostawców) będą miały postać plików JAR. Załadowanie poszczególnych modułów będzie opcjonalne (pomijając potencjalne zależności między modułami). Wybór które moduły zostaną załadowane będzie się odbywać w czasie wykonania. W przypadku modułów, które nie potrzebują konfiguracji ich instalacja będzie polegała na umieszczeniu plików JAR w odpowiednim katalogu, bądź dodaniu ścieżek do plików JAR w wywołaniu JVM. Opis problemów, jakie mogą wyniknąć, przedstawiono w części `Modułowość w Javie`_.

Wybór bibliotek
~~~~~~~~~~~~~~~

Framework webowy
````````````````

Jako framework wybrano Spring, ponieważ budujemy serwer HTTP, w którym wykorzystamy wiele mechanizmów oferowanych przez bibliotekę Spring (moduł autentykacji, wstrzykiwanie zależności, rejestrowanie końcówek API w różnych miejscach/modułach itp.).

Wstrzykiwanie zależności
~~~~~~~~~~~~~~~~~~~~~~~~

Kiedy odpowiednie klasy zostaną już załadowane z odpowiednich modułów, z odpowiednimi bibliotekami od których zależą, ich instancje muszą mieć możliwość komunikacji z resztą systemu.

Zdecydowaliśmy się na łączenie ze sobą komponentów za pomocą wzorca wstrzykiwania zależności. Takie podejście ułatwia tworzenie modularnego kodu, słabo powiązanego między modułami (loose-coupling). Pociąga to za sobą korzyści wynikające z modularności kodu.

Funkcja komponentów będzie określana poprzez zestaw interfejsów, które są implementowane przez klasy składające się na dany komponent. Interfejsy muszą być znane w czasie kompilacji i nie podlegają mechanizmowi wstrzykiwania zależności.

Gdy mamy dostępnych wiele implementacji pojawia się pytanie, którą z nich wstrzykiwać do później ładowanych modułów - do zarządzania wstrzykiwaniem zależności używamy narzędzia Spring Boot, a informacje o tym, których implementacji używać umieszczamy w pliku konfiguracyjnym. Jeśli nie skonfigurujemy jednoznacznie, której implementacji należy użyć, zostanie zgłoszony błąd. Niektóre moduły mogą poprosić o wszystkie załadowane implementacje danego interfejsu (np. poproszę o wszystkie metody uwierzytelnienia zdefiniowane w systemie).

Uwierzytelnianie
~~~~~~~~~~~~~~~~

Zdecydowaliśmy, że nie możemy trwale zapisać w programie sposobu uwierzytelniania - będzie ono więc realizowane przez konfigurowalny komponent uwierzytelniający. Dzięki temu instancje systemu będą miały możliwość udostępniania wielu sposobów uwierzytelnienia, wyłączenia ich, wybrania dowolnej ich kombinacji. Gdy powstanie potrzeba uwierzytelnienia w jeszcze inny sposób, nie będzie przeszkód aby napisać nową implementację komponentu uwierzytelniającego. Nie użyliśmy więc żadnej gotowej biblioteki uwierzytelniającej - połączyliśmy wbudowaną w framework Spring strukturę uwierzytelniania z własnymi modułami (które można wyłączyć lub włączyć) obsługującymi metody, takie jak uwierzytelnianie loginem i hasłem.

Dziennik zdarzeń
~~~~~~~~~~~~~~~~

Podjęliśmy decyzję, aby nie korzystać z frameworku logującego i w zamian zbudować własne narzędzie, w którym logujemy nie wiadomości, lecz instancje klas implementujących pewien określony interfejs Event. Wszystkie obiekty implementujące ten interfejs mogą być tłumaczone na JSON i zapisywane w celu późniejszego użycia. Umożliwi nam to wygodniejsze przetwarzanie zdarzeń i budowanie aplikacji, które je interpretują (w tym aplikacji obsługujących analizy i notyfikacje związane z bezpieczeństwem).

Rozważania
----------

Modułowość w Javie
~~~~~~~~~~~~~~~~~~

Modułowość w Javie w wersjach <= 8
``````````````````````````````````

Aktualnie w Javie dla wersji do 8 włącznie występuje następujący problem:

* uruchamiamy aplikację składającą się z wielu modułów,
* dwa moduły korzystają z tej samej biblioteki (mają ją w zależnościach), ale w różnych wersjach (np. wersja X i Y),
* w przypadku wystartowania tych modułów razem w classPath maszyna wirtualna Javy będzie miała bibliotekę w 2 wersjach,
* załóżmy, że podczas działania aplikacji dochodzimy do fragmentu kodu z modułu, który w zależnościach miał bibliotekę w wersji Y, wtedy classloader szuka w classpath biblioteki mającej taką klasę. Jeżeli najpierw znajdzie bibliotekę w wersji X, skorzysta z klasy w niej zdefiniowanej.

Java do wersji 8 włącznie podczas startowania aplikacji ładuje wszystkie wykorzystywane biblioteki do „jednego worka” i w przypadku złożonych z wielu modułów aplikacji może dochodzić do konfliktu bibliotek (opisanego powyżej). W przypadku korzystania w swoim projekcie z jakiejś zewnętrznej biblioteki mamy jednocześnie dostęp do wszystkich bibliotek, z których ona także korzysta. Nie mamy możliwości „przykrycia” ich widoczności. (Akapit Classpath/JAR hell:  http://www.javaworld.com/article/2878952/java-platform/modularity-in-java-9.html)

Modułowość w Javie 9
````````````````````

W tej wersji Javy zastosowano podejście do modularności bardzo podobne do tego w OSGi [#osgi]_. Tutaj podczas tworzenia biblioteki będziemy mogli zdefiniować, które klasy/pakiety z naszej biblioteki będą widoczne na zewnątrz (dla kodu, który będzie korzystał z naszej biblioteki), tzn. że będziemy mogli ukryć dla osób korzystających z naszej biblioteki np. jej zależności lub typowo wewnętrzne klasy.

Konkluzja i rozwiązanie problemu
````````````````````````````````

W związku z opisywanymi problemami zamierzamy, na jak najwcześniejszym etapie prac, korzystać z Javy 9 oraz Springa 5 (w trakcie prac nad systemem zintegrujemy wersje testowe tych narzędzi).

.. [#osgi]
   Standard OSGi opisuje sposób ładowania modułów, pozwalając m. in. na ukrywanie prywatnych komponentów, specyfikowanie zależności pomiędzy modułami, ładowanie i odładowywanie modułów w czasie działania aplikacji.

   Szczegółowy znajduje się na stronie https://www.osgi.org/developer/architecture/.
