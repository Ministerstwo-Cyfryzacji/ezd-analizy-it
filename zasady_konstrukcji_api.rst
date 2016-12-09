Zasady konstrukcji API
======================
do wewnętrznego użycia w EZD oraz dla zewnętrznych aplikacji — rekomendacje
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>


*Dobre praktyki i zalecenia dotyczące konstrukcji API.*


API dla Java
------------

Projektuj przewidując rozwój
++++++++++++++++++++++++++++

Jeśli API jest cokolwiek warte – będzie ewoluować.   Z drugiej strony, jeśli coś w API już jest, to zapewne będzie musiało zostać.

2 postulaty wstecznej zgodności:

- skompilowany kod nadal działa (zgodność binarna)
- istniejący kod źródłowy można nadal skompilować (zgodność źródłowa).

Nie da się przewidzieć, w jaki sposób użytkownicy skorzystają z API.
Rozważając zmianę – należy rozumować konserwatywnie: *tylko* jeśli można stwierdzić z prawdopodobieństwem graniczącym z pewnością, że zmiana nie spowoduje załamania jakiejkolwiek istniejącej aplikacji – *można* zmianę wprowadzić.

Jeśli potrzebne jest wprowadzenie większych zmian – należy zbudować nowe API, z inną nazwą.

Dobrym pomysłem jest wprowadzenie na początek jednej lub kilku wersji o numerach 0.xx przed wersją 1.0. Użytkownicy rozumieją, że takie wersje mogą być zmieniane.

API – cele projektowe
+++++++++++++++++++++

Musi być:

- absolutnie poprawny
- łatwy do użycia
- trudny do błędnego użycia
- łatwy do nauczenia
- wystarczająco szybki
- wystarczająco mały.

Minimalizm
++++++++++

Znacznie łatwiej dodawać nowe elementy niż je usuwać.

Im więcej jest elementów w API, tym trudniej jest się nauczyć.

Im większy API, tym więcej może być błędów.

Nadmiarowe metody niepotrzebnie zajmują miejsce.

Ostrożnie z pakietami
+++++++++++++++++++++

Całe API powinno być w jednym pakiecie.

Różne wskazówki
+++++++++++++++

Dobrze jest użyć klas niezmiennych.

Jedyne widoczne pola powinny być ‘static’ i ‘final’.

Unikaj ekscentryczności: pisz tak jak zwykle inni piszą. Jest wiele zwyczajów dotyczących nazw, getterów & setterów, klas obsługujących wyjątki etc.

Nie implementuj Cloneable: tworzenie kopii obiektu jest zwykle mniej przydatne niż może się wydawać.

Wyjątki (Exceptions) zwykle powinny nie być przesłaniane.

Przewidź stosowanie dziedziczenia - lub nie pozwalaj na to.


API dla REST
------------

Używaj rzeczowników, nie czasowników
++++++++++++++++++++++++++++++++++++

Korzystaj z poniższego schematu dla wszelkich zasobów:

+-----------+--------------+--------------+----------------------+---------------------------------+-----------+
|           | GET          | POST         | PUT zmiana według    | PATCH zmiana według danych      |DELETE     |
|           | pobieranie   | tworzenie    | danych dostarczonych | dostarczonych i/lub wyliczanych |usuwanie   |
+-----------+--------------+--------------+----------------------+---------------------------------+-----------+
| /cars     | Zwraca listę | Tworzy       | Masowa                                                 | Usuwa     |
|           | samochodów   | nowy         | aktualizacja                                           | wszystkie |
+-----------+--------------+--------------+----------------------+---------------------------------+-----------+
| /cars/711 | Zwraca       | Metoda       | Aktualizuje                                            | Usuwa     |
|           | wskazany     | niedozwolona | wskazany                                               | wskazany  |
|           |              | (405)        |                                                        |           |
+-----------+--------------+--------------+----------------------+---------------------------------+-----------+

Nie używaj czasowników, np.

::

  /getAllCars
  /createNewCar
  /deleteAllRedCars

Metoda GET i parametry zapytania nie powinny modyfikować stanu
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Nie używaj GET dla poleceń zmieniających stan.

Używaj liczby mnogiej
+++++++++++++++++++++

Nie mieszaj rzeczowników w liczbie pojedynczej i mnogiej.

Niech będzie prosto. Używaj tylko liczby mnogiej.

Używaj hierarchii dla wskazania relacji między obiektami
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

::

  GET /cars/711/drivers/              Lista kierowców samochodu 711
  GET /cars/711/drivers/4             Kierowca nr 4 samochodu 711

Używaj nagłówków HTTP
+++++++++++++++++++++

Zarówno klient jak i serwer powinny znać format używany w komunikacji. Format powinien być wyspecyfikowany w nagłówku HTTP.

*Content-Type*    określa format żądania

*Accept*              określa listę akceptowalnych formatów odpowiedzi.

Używaj HATEOAS (Hypermedia as the Engine of Application State)
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Klient kontaktuje się z aplikacją wyłącznie przez interfejs dostarczany przez serwer aplikacyjny.
Nie potrzebuje żadnej wstępnej wiedzy o tym, jak komunikować się, poza generalną wiedzą o hipermediach:

- rekordy są adresowane przez URL
- można w ten sposób otrzymać wszystkie rekordy
- ścieżka do korzenia udostępnia index API.

W odróżnieniu, niektóre aplikacje o architekturze SOA (service-oriented architecture) komunikują się sztywnym interfejsem albo
językiem opisu interfejsu (IDL).

Zasada HATEOAS rozdziela klienta i serwer w sposób, który pozwala im ewoluować niezależnie.

Umożliwiaj filtrowanie, sortowanie, wybór pól, stronicowanie przy pobieraniu kolekcji
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Elastyczność filtrowania rozluźnia związek API z modelem.

Przykłady

::

  GET /cars?color=red            Lista czerwonych samochodów

  GET /cars?seats<=2             Lista samochodów o max 2 siedzeniach

  GET /cars?sort=-manufacturer,+model

  GET /cars?fields=manufacturer,model,id,color

  GET /cars?offset=10&limit=5

Udzielaj kompletnych odpowiedzi
+++++++++++++++++++++++++++++++

Odpowiedzi powinny zawierać wszystkie dane potrzebne do utworzenia kompletnego obiektu modelu.

::

  // GET /users/12
  {
    user: {
      id: 12,                    // powtórzone z pytania, ale ułatwia pracę klienta
      name: 'Bob Barker'
    }
  }

  // GET /users/12
  {
    user: {
      id: 12,
      name: 'Bob Barker',
      todo_list_ids: [ 4, 5 ]    // struktury będą w osobnych rekordach; dyskusyjne
    }
    todo_lists: [
      { id: 4, name: 'shopping' },
      { id: 5, name: 'work' }
    ]
  }

Używaj warstwy prezentacji
++++++++++++++++++++++++++

Separuj dane API od warstwy danych aplikacji:

- pozwala obliczać wtórne wartości, które mogą być potrzebne po stronie klienckiej, np. obliczenie płci na podstawie numeru PESEL
- tworzy to dobrą warstwę do testów
- chroni kod kliencki od zmian po stronie serwera.

Daj przykłady odpowiedzi na GET
+++++++++++++++++++++++++++++++

Pokaż, jakich danych można oczekiwać w odpowiedzi na wszystkie poprawne rodzaje żądań GET.
Przykłady powinne być proste i łatwe do zrozumienia
(czyli w czasie < 5 sek.).

Umożliw ograniczenie listy zwracanych pól
+++++++++++++++++++++++++++++++++++++++++

Parametr taki jak 'pola', podający rozdzieloną przecinkami listę,  pozwala na ograniczenie zwracanych pól do tych,
których użytkownik potrzebuje. Na przykład

::

  GET /tickets?fields=id,subject,customer_name,updated_at

Tworzenie i aktualizacje powinny zwracać nowe dane
++++++++++++++++++++++++++++++++++++++++++++++++++

Operacje PUT, POST i PATCH mogą dokonywać zmian pól, które nie były dostarczone na liście parametrów (np. czas).

Niektórzy autorzy postulują, że
aby uniknąć konieczności ponownego pytania o zaktualizowane wartości, operacja aktualizująca powinna od razu
je zwracać. (To jest dyskusyjne, np. w przypadku masowych zmian — których efektu program kliencki może nie potrzebować.)

Zwracaj JSON
++++++++++++

XML jest zbyt dosłowny i trudniejszy do czytania i parsowania.

W sieci obserwuje się odejście od XML na rzecz formatu JSON.

Nowym mechanizmem, który też można użyć, zwłaszcza gdy potrzeba
otrzymywać wiele struktur danych w odpowiedzi na jedno żądanie
jest `GraphQL <http://graphql.org/>`_.

Absolutne adresy danych dodatkowych
+++++++++++++++++++++++++++++++++++

Jeśli pojawiają się odnośniki do dodatkowych zasobów,
to odnośniki te powinny być absolutne.

Podaj połączenia do sąsiednich stron
++++++++++++++++++++++++++++++++++++

Jeśli wynik zwraca fragment zasobów, dołącz w odpowiedzi gotowe linki do sąsiednich porcji danych. Na przykład

::

 Link: </TheBook/chapter2>;
       rel="previous"; title="Giewont",
       </TheBook/chapter4>;
       rel="next"; title="Mont Blanc"

Autoryzacja
+++++++++++

Używaj tokenów.

Ogranicz użycie sesji i cookies.

Używaj SSL
++++++++++

Zawsze używaj SSL. Bez wyjątków.

Przy próbie dostępu bez SSL, nie przekierowuj do odpowiedniego SSL. Odrzuć żądanie.

Wersjonuj swoje API
+++++++++++++++++++

::

 /blog/api/v1

Obsługuj błędy zwracane jako status HTTP
++++++++++++++++++++++++++++++++++++++++

Przynajmniej te:

-  200 – OK
-  201 – OK                — Utworzono nowy obiekt
-  204 – OK                — Udało się usunąć
-  304 – Not Modified      — Klient nie może używać buforowanych danych
-  400 – Bad Request       — Niepoprawne żądanie
-  401 – Unauthorized      — Niepoprawna autentykacja
-  403 – Forbidden         — Niedozwolone żądanie lub brak dostępu do zasobów
-  404 – Not found         — Nie odnaleziono zasobu URI
-  500 – Internal Server Error — Twórcy API powinni unikać tego błędu. Jeśli wystąpi błąd w aplikacji – powinien być zapisywany do logu; należy starać się zwrócić przyjazny tekst komunikatu.

Dostarcz osobne komunikaty dla

- dewelopera
- użytkownika końcowego

oraz

- wewnętrzny kod błędu
- (ewentualnie) URL gdzie deweloper może znależć więcej informacji.

Narzędzia dokumentowania API
++++++++++++++++++++++++++++

Niektórzy Autorzy zalecają do wykonania opisu twojego API, narzędzia

- `OpenAPI Specification (fka Swagger RESTful API Documentation Specification) <http://swagger.io/specification/>`_

- `API Blueprint <https://apiblueprint.org/>`_

- `Interface description language <https://en.wikipedia.org/wiki/Interface_description_language>`_


API dla JavaScript
------------------

Używaj HTML5
++++++++++++

Dwie składnie wywołań
+++++++++++++++++++++

Dostarcz dwie różne składnie wywołań funkcji:

- proceduralną
- obiektową.

Nazwy
+++++

Powinny rozpoczynać się i kończyć znakami “a-z”.

Powinny zawierać jedynie znaki “a-z”, 0-9” i znak łącznika “-”.

Używaj raczej konwencji "Wielbłądziej"

::

  nazwaZlozonegoElementu

niż konwencji "Wężowej"

::

  nazwa_zlozonego_elementu

Wybrane dokumenty w Internecie
------------------------------

`Eamonn McManus. Java API Design Guidelines
<http://www.artima.com/weblogs/viewpost.jsp?thread=142428>`_

`JSON API. Recommendations. <http://jsonapi.org/recommendations/>`_

`Matthew Beale. Suggested REST API Practices
<https://madhatted.com/2013/3/19/suggested-rest-api-practices>`_

`Team Stormpath. The Fundamentals of REST API Design
<https://stormpath.com/blog/fundamentals-rest-api-design>`_

`m-way. 10 Best Practices for Better RESTful API
<http://blog.mwaysolutions.com/2014/06/05/10-best-practices-for-better-restful-api/>`_

`Keshav Vasudevan. Good Practices in API Design
<http://blog.swaggerhub.com/api-design/api-design-best-practices/>`_

`Vinay Sahni. Best Practices for Designing a Pragmatic RESTful API
<http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api>`_

`A query language for your API <http://graphql.org/>`_.

`Phil Sturgeon. Understanding REST And RPC For HTTP APIs
<https://www.smashingmagazine.com/2016/09/understanding-rest-and-rpc-for-http-apis/>`_

`iTunes Search API
<https://affiliate.itunes.apple.com/resources/documentation/itunes-store-web-service-search-api/>`_

`White House Web API Standards
<https://github.com/WhiteHouse/api-standards>`_

