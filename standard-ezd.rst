Szkic standardu EZD — wersja robocza
====================================

Wstęp
-----

Niniejszy dokument proponuje standard przechowywania w systemie EZD mających znaczenie prawne danych dotyczących dokumentów i spraw, który pozwoli na:

1. Zachowanie dowodliwości powstania dokumentów w konkretnym czasie.
2. Zawarcie informacji dowodzących autentyczności wytworzonych dokumentów.
3. Ustandaryzowanie procesu migracji w/w danych między różnymi systemami EZD.
4. Wydzielanie kompletów danych wybranych spraw w sposób zachowujący ich dowodliwość.

Założenia
---------

1. Rdzeń bazodanowy zawiera wszystkie dane mające znaczenie prawne i tylko te dane.
2. Import/eksport zachowuje dowodliwość powstania dokumentów w konkretnym czasie.
3. Wszystkie akcje mające znaczenie prawne są opatrywane podpisem cyfrowym.
4. Format danych niepowiązany z konkretnym silnikiem bazodanowym.

Wykonanie
---------

Wszystkie dokumenty w systemie identyfikujemy poprzez ich sumy kontrolne (funkcje skrótu).

Sprawy to ciągi specyficznych typów dokumentów, których rodzicem jest poprzedni dokument w sprawie (nie dotyczy dokumentu zakładającego sprawę).

Surowe (nieosadzone bezpośrednio w żadnej sprawie) dokumenty mogą w systemie istnieć bez podpisu cyfrowego, ale wszystkie nowo tworzone dokumenty w sprawach muszą taki podpis cyfrowy zawierać.

Dokumenty w sprawach mogą zawierać odniesienia do innych dokumentów, np. dokument zakładający sprawę może odnosić się do dokumentu, który jest powodem wszczęcia tej sprawy.

Stany spraw (rozumiane jako najnowsze elementy akt spraw) są na bieżąco oznaczane czasem za pomocą KSI.

Nie istnieje metoda kasowania dokumentów. Jeśli do jakiejś sprawy dokument został dodany omyłkowo, to tworzona jest nowa gałąź sprawy odchodząca od ostatniego poprawnego dokumentu, a do starej gałęzi zostaje dodany dokument oznaczający ją jako omyłkową. (Omyłkowa gałąź co do zasady przestaje być widoczna dla obywateli i urzędników mających dostęp do sprawy).

System uprawnień jest egzekwowany niezależnie, poza rdzeniem struktury danych. Urzędnik teoretycznie mógłby wytworzyć dokument w sprawie, do której nie jest uprawniony (gdyby w jakiś sposób uzyskał z niej dane), ale serwer powinien taki dokument odrzucić i nie włączyć go do KSI dzięki istnieniu systemu uprawnień.

Format dokumentów
~~~~~~~~~~~~~~~~~

Dokumenty są opakowane w kontener `XAdES`_ [#dlaczego-nie-CAdES]_. Atrybuty ``URI`` elementów ``Reference`` są tworzone zgodnie z `RFC 6920`_, z obowiązkowo podanym parametrem ``ct`` oraz bez sekcji ``authority``.

Pytania
~~~~~~~

#. Czy zmiany metadanych sprawy mają wchodzić w skład dokumentów sprawy? Raczej nie, bo metadane mają wartość pomocniczą, a nie prawną. (Choć oczywiście metadane powinny być ujęte w procesie import-eksport).
#. Czy urzędnik powinien za pomocą oddzielnego dokumentu potwierdzać takie czynności, jak przyjęcie dekretacji do wiadomości? Bez tego nie będzie wiarygodnego dowodu, że urzędnik faktycznie otrzymał sprawę, która została mu zlecona. Być może jest to regulowane przez istniejące przepisy bądź też pozostawione do decyzji w ramach autonomii poszczególnych urzędów. Tak czy siak, pytanie jest bardziej proceduralne i wydaje się nie naruszać struktury danych formatu EZD.
#. Na ile istotne jest zwalczenie transaction malleability? [#ciagliwosc-dokumentow]_
#. Czy dokument sprawy może mieć wielu rodziców (np. przy łączeniu spraw)?

Uwagi
-----

Daty zawarte w dokumentach nie mają (nie powinny mieć) większego znaczenia prawnego — znaczenie mają obiektywne daty wynikające z KSI bądź z innej usługi oznaczającej dokumenty czasem.

Nie da się efektywnie (w sposób dowodliwy) kontrolować dostępu do danych, w związku z czym rdzeń systemu nie zawiera informacji o tym, kto kiedy jaki dokument otrzymał. Można takie informacje przechowywać niezależnie, w celach poglądowych lub jeśli taka potrzeba wynika z obowiązujących przepisów. Można też w ramach systemu wytwarzać pokwitowania dostępu do dokumentów (być może jako szczególny, wydzielony typ dokumentu w aktach spraw/wydzielonym rejestrze).

Można wyobrazić sobie pracę w systemie EZD nad sprawami pobranymi już na komputer urzędnika w trybie offline, podobnie jak można pracować z Gitem w trybie offline. Można też rozważać częściową synchronizację prac z pominięciem centralnego serwera, w przypadku awarii tegoż.

.. _ciągliwość transakcji: https://en.bitcoin.it/wiki/Transaction_Malleability
.. _CAdES: https://tools.ietf.org/html/rfc5126
.. _XAdES: https://www.w3.org/TR/XAdES/
.. _RFC 6920: https://tools.ietf.org/html/rfc6920

.. [#ciagliwosc-dokumentow]
   Por. `ciągliwość transakcji`_ w Bitcoinie.

.. [#dlaczego-nie-CAdES]
   Konkurencyjnym formatem jest CAdES_, który jednak nie został wybrany z uwagi na brak wyraźnie określonej możliwości odwołania się do obiektów z uwzględnieniem ich sumy kontrolnej.
