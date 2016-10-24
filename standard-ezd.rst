Szkic standardu EZD — wersja robocza
====================================

Założenia
---------

1. Rdzeń bazodanowy zawiera wszystkie dane mające znaczenie prawne i tylko te dane.
2. Import/eksport zachowuje dowodliwość powstania dokumentów w konkretnym czasie.
3. Wszystkie akcje mające znaczenie prawne są opatrywane podpisem cyfrowym.
4. Format danych niepowiązany z konkretnym silnikiem bazodanowym.

Wykonanie
---------

Wszystkie dokumenty w systemie identyfikujemy poprzez ich sumy kontrolne (funkcje skrótu).

Sprawy to ciągi specyficznych typów dokumentów, których rodzicem jest poprzedni dokument w sprawie (nie dotyczy dokumentu zakładającego sprawę, gdzie zamiast przypisania do rodzica mamy nadanie znaku sprawy).

Surowe (nieosadzone bezpośrednio w żadnej sprawie) dokumenty mogą w systemie istnieć bez podpisu cyfrowego, ale wszystkie nowo tworzone dokumenty w sprawach muszą taki podpis cyfrowy zawierać.

Dokumenty w sprawach mogą zawierać odniesienia do innych dokumentów, np. dokument zakładający sprawę może odnosić się do dokumentu, który jest powodem wszczęcia tej sprawy.

Rejestry to z technicznego punktu widzenia szczególne przypadki spraw (choć nieposiadające nadanego znaku sprawy).

Stany spraw są na bieżąco oznaczane czasem za pomocą KSI. Informacje pozwalające na zweryfikowania istnienia dokumentów są przypisywane do dokumentów w bazie danych (ale oczywiście nie wchodzą w skład samych dokumentów). Pierwsza warstwa agregacji haszy jest wykonywana na poziomie urzędu, dzięki czemu można opublikować jeden hasz odzwierciedlający stan wytworzonych w danym urzędzie w danym czasie.

Nie istnieje metoda kasowania dokumentów. Jeśli do jakiejś sprawy dokument został dodany omyłkowo, to tworzona jest nowa gałąź sprawy odchodząca od ostatniego poprawnego dokumentu, a do starej gałęzi zostaje dodany dokument oznaczający ją jako omyłkową. (Omyłkowa gałąź co do zasady przestaje być widoczna dla obywateli i urzędników mających dostęp do sprawy).

System uprawnień jest egzekwowany niezależnie, poza rdzeniem struktury danych. Urzędnik teoretycznie mógłby wytworzyć dokument w sprawie, do której nie jest uprawniony (gdyby w jakiś sposób uzyskał z niej dane), ale serwer powinien taki dokument odrzucić i nie włączyć go do KSI dzięki istnieniu systemu uprawnień.

Format dokumentów
~~~~~~~~~~~~~~~~~

W ramach standardu potrzebne będzie wybranie formatu dokumentów wchodzących w skład akt sprawy (w razie potrzeby format ten będzie można zmienić w przyszłości).

W przypadku zastosowania tekstowego/rozwlekłego formatu (np. opartego o JSON) albo mielibyśmy do czynienia ze zjawiskiem ciągliwości dokumentów[#ciagliwosc-dokumentow]_ (można by utworzyć wiele równoważnych dokumentów o różnej sumie kontrolnej), albo też zachodziłaby konieczność ścisłej normalizacji formatu dokumentu, co stanowiłoby obostrzenie wykraczające poza standard danego formatu.

Różne formaty dokumentów należało będzie oczywiście wersjonować (własne formaty binarne mają tu pewną przewagę, bo można jednoznacznie wstawić numer wersji w określonym miejscu, co ułatwia rozpoznanie).

Pytania
~~~~~~~

1. Czy zmiany metadanych sprawy mają wchodzić w skład dokumentów sprawy? Raczej nie, bo metadane mają wartość pomocniczą, a nie prawną. (Choć oczywiście metadane powinny być ujęte w procesie import-eksport).
2. Czy urzędnik powinien za pomocą oddzielnego dokumentu potwierdzać takie czynności, jak przyjęcie dekretacji do wiadomości? Bez tego nie będzie wiarygodnego dowodu, że urzędnik faktycznie otrzymał sprawę, która została mu zlecona. Być może jest to regulowane przez istniejące przepisy bądź też pozostawione do decyzji w ramach autonomii poszczególnych urzędów. Tak czy siak, pytanie jest bardziej proceduralne i wydaje się nie naruszać struktury danych formatu EZD.
3. Czy i w jaki sposób chcemy przechowyać typ dokumentu (“content type”)? Wydaje się, że analogicznie do metadanych.
4. Na ile istotne jest zwalczenie transaction malleability?
5. Czy dokument sprawy może mieć wielu rodziców (np. przy łączeniu spraw)?

Rozróżnianie formatów
---------------------

Do odczytania i zinterpretowania dowolnego pliku potrzebna jest znajomość jego formatu. Istnieje możliwość tworzenia plików, które wyświetlane jako pewien format ukazują zupełnie inną treść niż w wyświetlane jako inny format. Przykładowo, można łatwo stworzyć plik, który w przeglądarce HTML wyświetli treść „Wyrażam zgodę”, podczas gdy w przeglądarce PDF wyświetli treść „Nie wyrażam zgody”[#przyklad-pliku-wieloformatowego]_. Z punktu widzenia bezpieczeństwa wskazane jest zatem podpisywanie nie tylko surowych treści plików, ale też informacji o użytym formacie. Można to osiągnąć poprzez składowanie plików w opakowaniu z informacją o ich typie (podobnie jak składowane są obiekty w Gicie, choć niekoniecznie z wykorzystaniem formatu tekstowego). Typ mógłby być reprezentowany przez jedno z następujących (do ustalenia):

1. Rozszerzenie.
2. Mimetype.

Urzędnik wgrywający plik mógłby mieć możliwość zmiany typu dokumentu (odgadniętego na podstawie rozszerzenia i ew. zawartości dokumentu).

Potencjalnie można by w ten sam sposób składować również dokumenty wchodzące w skład akt sprawy (przydzielając im własny mimetype).

Uwagi
-----

Daty zawarte w dokumentach nie mają (nie powinny mieć) większego znaczenia prawnego — znaczenie mają obiektywne daty wynikające z KSI bądź z innej usługi oznaczającej dokumenty czasem.

Nie da się efektywnie (w sposób dowodliwy) kontrolować dostępu do danych, w związku z czym rdzeń systemu nie zawiera informacji o tym, kto kiedy jaki dokument otrzymał. Można takie informacje przechowywać niezależnie, w celach poglądowych lub jeśli taka potrzeba wynika z obowiązujących przepisów. Można też w ramach systemu wytwarzać pokwitowania dostępu do dokumentów (być może jako szczególny, wydzielony typ dokumentu w aktach spraw/wydzielonym rejestrze).

Można wyobrazić sobie pracę w systemie EZD nad sprawami pobranymi już na komputer urzędnika w trybie offline, podobnie jak można pracować z Gitem w trybie offline. Można też rozważać częściową synchronizację prac z pominięciem centralnego serwera, w przypadku awarii tegoż.

.. _ciągliwość transakcji: https://en.bitcoin.it/wiki/Transaction_Malleability

.. [#ciagliwosc-dokumentow]
   Por. `ciągliwość transakcji`_ w Bitcoinie.

.. [#przyklad-pliku-wieloformatowego]
   Korzystając z narzędzia Pandoc_ możemy tego dokonać następująco::

     echo '<div>Wyrażam zgodę.<div style="display: none;">' > akceptacja.html
     echo 'Nie wyrażam zgody.' | pandoc -t latex -o akceptacja.pdf
     cat akceptacja.html akceptacja.pdf > akceptacja
