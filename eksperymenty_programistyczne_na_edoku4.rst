Eksperymenty programistyczne na eDoku4
======================================

Pierwszy eksperyment programistyczny
------------------------------------

Cel eksperymentu
~~~~~~~~~~~~~~~~

Celem eksperymentu jest wstępne zbadanie możliwości rozbudowy testowanego systemu. Jego wynik może pomóc w opracowaniu kolejnych, bardziej wyszukanych eksperymentów.

Założenia eksperymentu
~~~~~~~~~~~~~~~~~~~~~~

Instrukcja kancelaryjna w ramach minimalnego zestawu metadanych opisujących przesyłki wpływające oraz wychodzące wymienia (opcjonalny) adres e-mail odpowiednio nadawcy oraz adresata. Urząd mógłby chcieć wdrożyć narzędzie umożliwiające obywatelom zalogowanie się swoim adresem e-mail oraz wyświetlenie przesyłek powiązanych ze swoim kontem. Eksperyment ma za zadanie ustalić możliwość łatwego napisania takiej aplikacji.

Forma eksperymentu
~~~~~~~~~~~~~~~~~~

W ramach eksperymentu zbadana zostanie możliwość wydobycia z bazy danych poszczególnych systemów EZD metadanych przesyłek powiązanych z konkretnym adresem e-mail. Pierwszoplanową metodą osiągnięcia tego celu będzie skorzystanie z API; w dalszej kolejności rozpatrywane będą inne metody, o ile nie będą się one wiązały z nadmierną uciążliwością programistyczną. Jeżeli uzyskanie metadanych przesyłek okaże się możliwe, zbadana zostanie także możliwość uzyskania odwzorowań cyfrowych tych przesyłek.

Zrezygnowano ze stworzenia faktycznego prototypu aplikacji WWW umożliwiającej obywatelom wyświetlanie danych przesyłek z uwagi na to, że nie wniosłoby ono nic w kontekście celu eksperymentu.

Ograniczenia
~~~~~~~~~~~~

Eksperyment zakłada stworzenie aplikacji dla obywatela, który to nie jest bezpośrednim użytkownikiem systemu EZD. Z uwagi na to stopień integracji aplikacji z danym systemem EZD może być pobieżny i w celu głębszego zbadania możliwości jego rozbudowy wskazane mogą być dalsze eksperymenty.

Realizacja eksperymentu na eDoku4
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Po stronie eDoka4 potrzebna jest funkcja szukania klientów z przypisanym danym adresem e-mail, oraz funkcja szukania korespondencji powiązanej z danymi klientami.

Edok4, w formie w jakiej go dostaliśmy, nie pozwala na wyszukiwanie klientów za pomocą adresu e-mail. Aby dodać tą funkcjonalność dodano klasę filtra (z czym nie było problemów). Wyszukiwanie korespondencji klienta także jest niewspierane - napisany został nowy filtr.

Po stronie zewnętrznej aplikacji należało opanować API (format danych, sposób autoryzacji, adresy endpointów). Zewnętrzna aplikacja została napisana w języku programowania Python (narzędzie wywoływane z linii poleceń oraz serwer HTTP serwujący prosty interfejs graficzny). Użycie REST API eDoka4 nie sprawiło problemów. EDok4 nie posiada dokumentacji swojego API poza kodem.

Podczas tworzenia oprogramowania napotkano problemy:

* mieszanie polskiego i angielskiego nazewnictwa w kodzie - utrudnia to pracę programistom (np. trudno zapamiętać czy pole w rekordzie ma nazwę ``client`` czy ``klient``)
* skonfigurowanie wyszukiwania dokładnym dopasowaniem w silniku wyszukiwania (Elasticsearch) było uciążliwe i w rezultacie nie zostało wykonane. Takie wyszukiwanie jest bardzo potrzebne w przypadku szukania po adresie e-mail lub po identyfikatorze klienta.

Wnioski
~~~~~~~

System eDok4 ma możliwości rozbudowy. Można zintegrować z nim zewnętrzną aplikację, która korzysta z danych w nim zgromadzonych. Możliwość dostępu do danych nie jest ograniczona przez zakres API - można go poszerzyć dodając końcówki i filtry w kodzie aplikacji.

Drugi eksperyment programistyczny
---------------------------------

Cel technologiczny
~~~~~~~~~~~~~~~~~~

Lepiej zrozumieć, jak zewnętrzna firma może rozwijać system eDOK. Między innymi:

* dodawać pola na metadane,
* modyfikować widoki interfejsu WWW,
* dodawać rodzaje raportów.

Opis eksperymentu
~~~~~~~~~~~~~~~~~

Eksperyment zakłada dobudowanie narzędzia obliczającego stopień zrozumiałości pism wytwarzanych przez urzędników i adresowanych do obywateli, a także uwzględnienie obliczonej wartości w narzędziach do raportowania — pozwalając na określenie zrozumiałości m.in. ze względu na komórkę organizacyjną, klasyfikację JRWA i osobę urzędnika. Dodatkową funkcjonalnością będzie wyświetlanie stopnia zrozumiałości pisma przed jego dodaniem do akt sprawy, aby urzędnik je tworzący miał możliwość dokonania ewentualnej korekty.

Aplikacje oceniające zrozumiałość tekstu już istnieją, np. `Jasnopis
<http://jasnopis.pl/>`_ (którego kod nie jest publicznie dostępny). W eksperymencie nie będzie jednak przeprowadzona integracja z rzeczywistą aplikacją, tylko zastosowana atrapa takowej (w formie funkcji zwracającej bądź to losową wartość, bądź uproszczoną metrykę opartą o dane takie, jak liczba poszczególnych znaków interpunkcyjnych, liczba słów i liczba znaków).

Realizacja eksperymentu na eDoku4
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Podczas realizacji eksperymentu powstał kod, który wprowadza:

* zmiany w eDoku:

  * nowe pole "clarity score" w metadanych dokumentu,
  * wykonywanie zapytań (REST API) do serwera zewnętrznego oceniającego zrozumiałość,
  * wyświetlanie ocen w interfejsie użytkownika,
  * generowanie rankingów zrozumiałości jako raportów,

* atrapę serwera oceniającego zrozumiałość napisana w języku programowania Python.

Wnioski
~~~~~~~

Firma zewnętrzna, dysponująca kodem eDoka, może m. in.:

* łatwo dodawać pola na metadane w bazie danych,
* dodawać wywołania REST API innych aplikacji, aby pobrać z nich dane,
* modyfikować widoki w interfejsie graficznym,
* dodawać nowe rodzaje raportów.
