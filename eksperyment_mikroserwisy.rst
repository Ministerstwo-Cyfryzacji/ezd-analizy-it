Testowanie mikrousług/Dockera
=============================

Streszczenie
------------

Cel
~~~

* Przetestowanie w praktyce możliwości budowania systemów w oparciu o
  mikrousługi.
* Przetestowanie konteneryzacji Dockera stosowanej do uruchamiania
  mikrousług.

Sposób
~~~~~~

* Zbudowanie kilku współpracujących mikrousług.
* Uruchomienie testowych mikrousług w kontenerach Dockera.
* Zebranie wniosków.

Rezultat
~~~~~~~~

* Kod mikrousług i konfiguracji umieszczono w repozytorium_.
* Szczegółowe wnioski zebrane są na końcu tego dokumentu.

Przebieg eksperymentu
---------------------

Mikrousługi
~~~~~~~~~~~

Stworzono trzy mikrousługi:

* Osobisty pulpit - serwuje stronę WWW, na której umieszczane są
  komponenty graficzne zgłaszane do wyświetlenia przez inne
  mikrousługi. Każdy komponent wyświetlany jest w osobnym
  iframe. Osobisty pulpit ma też REST API, aby inne programy mogły
  zgłosić chęć wyświetlenia interfejsu. Zgłoszenie zawiera adres
  strony, która powinna być wyświetlona wewnątrz ramki.
* Tablica ogłoszeń - serwuje stronę WWW, na której można dodać
  ogłoszenie (tekst). Ma przeglądarkę ogłoszeń. Korzystając z zarządcy
  zależności pobiera adres osobistego pulpitu, a następnie umieszcza
  na nim komponent, w którym wyświetlane jest najnowsze ogłoszenie.
* Zarządca zależności - usługa, która ma tylko REST API, z którego
  można pobrać informację, pod jakimi adresami znajdują się inne
  usługi. Na potrzeby eksperymentu implementacja jest trywialna:
  serwowany jest zawsze ten sam obiekt JSON. Adresy zapisane są na
  stałe.


Te trzy mikrousługi można uruchomić na jednym lub wielu komputerach,
modyfikując tylko adresy zwracane przez zarządcę zależności.

Konteneryzacja Docker
~~~~~~~~~~~~~~~~~~~~~

Dla każdej z trzech stworzonych usług stworzono konfigurację kontenera
Dockera (Dockerfile). Na podstawie tej konfiguracji oraz kodu
mikrousług można wygenerować obrazy kontenerów. Uruchamiając je
podłączone do wspólnej sieci wirtualnej i podając im odpowiednie
adresy zależności można uruchomić testowy system.

Docker Compose
~~~~~~~~~~~~~~

Uruchamianie wielu usług (nawet trzech - jak w przeprowadzonym teście)
jest uciążliwe. Aby ułatwić proces zastosowano narzędzie Docker
Compose. Skonfigurowano je tak, aby jedną komendą można było uruchomić
trzy serwisy podłączone do odpowiednich sieci, z odpowiednimi adresami
zależności.

Wnioski
-------

* Prosta funkcjonalność testowego systemu okazała się łatwa do
  zrealizowania w architekturze mikrousług.

* Frontend w modelu mikrousług można zrealizować następująco: każda
  usługa, która chce mieć interfejs graficzny WWW ma go niezależnie od
  innych usług (tzn. ma wbudowany serwer HTTP, który serwuje
  strony). Jeśli jakaś usługa wewnątrz swojego frontendu chce załączyć
  frontend innej mikrousługi, może zrobić to za pomocą iframe.

* Gotowe mikrousługi można uruchamiać bez narzędzi
  wirtualizacji/konteneryzacji. Można też uruchamiać je z użyciem
  takich narzędzi. Użycie takich narzędzi jest dla usług
  przezroczyste. To dobra cecha, ponieważ pozwala na zmianę środowiska
  bez zmiany kodu usług.

* Występują problemy z inicjalizacją trwałego stanu, co oznacza, że
  inicjalizacja stanu jest szczególnym przypadkiem działania usługi,
  który trzeba przewidzieć i obsłużyć (ten aspekt stosuje się do
  wszelkich programów, które mają trwały stan). Są dwie główne
  możliwości:

  * Inicjalizacja jest przeprowadzana automatycznie, gdy aplikacja
    wykryje konieczność inicjalizacji.
  * Inicjalizacja jest przeprowadzana przez inny kod niż zasadnicza
    aplikacja. Może to być inny program lub część aplikacji, która
    uruchamiana jest przez przekazanie do aplikacji odpowiedniego
    argumentu.

* Korzystanie z zarządcy zależności nie wygląda na dobry pomysł. Każda
  usługa powinna przyjmować w swoich argumentach lokalizację innych
  aplikacji, od których zależy i nie korzystać z zarządcy zależności.

* Dzięki konteneryzacji nie ma problemu z kolidującymi wersjami
  oprogramowania (np. A wymaga biblioteki foobar-3.3.6, B wymaga
  biblioteki foobar-3.4.1), nie ma problemu z kolizjami portów
  (wszystkie serwery HTTP, których jest n, można uruchomić na porcie
  80).

* Do zarządzania zależnościami pomiędzy usługami dobrze sprawdza się
  DNS serwowany przez Dockera wewnątrz sieci wirtualnych,
  np. uruchamiamy mikrousługę A i nazywamy ją "ms_a". Następnie,
  uruchamiając mikrousługę B, która korzysta z A, podajemy jej adres
  A: http://ms_a/api/. Nie musimy wiedzieć jaki adres ma
  usługa A. Dzięki temu adres może być przydzielany dynamicznie.

* Docker-compose znacznie ułatwia uruchamianie projektu poprzez
  automatyzację uruchamiania wielu powiązanych usług.

* Być może Docker nie powinien obsługiwać stanowych
  usług. Stwierdzenie nie jest wynikiem eksperymentu, lecz artykułów
  wskazujących zagrożenia związane z uruchamianiem stanowych usług w
  kontenerach (por. `artykuł 1`_, `artykuł 2`_). Cytaty:

  * "Docker MUST NOT run any databases in production, EVER."
  * "Services which store data cannot be dockerized. Docker is
    designed to NOT store data. Don’t go against it, it’s a recipe for
    disaster."

.. _repozytorium:
   https://github.com/Ministerstwo-Cyfryzacji/aplikacje.gov.pl-microservices/tree/dd8dd40ee8a518c5325ac2386301afd6f6b50659
.. _artykuł 1:
   https://thehftguy.com/2016/11/01/docker-in-production-an-history-of-failure/
.. _artykuł 2: https://thehftguy.com/2017/02/23/docker-in-production-an-update/
