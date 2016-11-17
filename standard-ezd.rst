Szkic standardu EZD — wersja robocza
====================================

Wstęp
-----

Niniejszy dokument proponuje standard przechowywania w systemie EZD mających znaczenie prawne danych dotyczących dokumentów i spraw, który pozwoli na:

1. Zachowanie dowodliwości powstania dokumentów w konkretnym czasie.
2. Zawarcie informacji dowodzących autentyczności wytworzonych dokumentów.
3. Ustandaryzowanie procesu migracji w/w danych między różnymi systemami EZD.
4. Wydzielanie kompletów danych wybranych spraw w sposób zachowujący ich dowodliwość.

Wykonanie
---------

Elementy metryki sprawy uznajemy za obiekty istotne prawnie (i tym samym podpisywane). Każdy element metryki zawiera następujące pola:

* URI elementu poprzedniego.
* URI wykonawcy czynnośći.
* Specyfikacja czynności (polimorficzna).
* Powiązania (lista URI).

Pytania
~~~~~~~

#. Czy zmiany metadanych sprawy mają wchodzić w skład dokumentów sprawy? Raczej nie, bo metadane mają wartość pomocniczą, a nie prawną. (Choć oczywiście metadane powinny być ujęte w procesie import-eksport).
#. Czy urzędnik powinien za pomocą oddzielnej czynności potwierdzać przyjęcie dekretacji do wiadomości? Bez tego nie będzie wiarygodnego dowodu, że urzędnik faktycznie otrzymał sprawę, która została mu zlecona. Być może jest to regulowane przez istniejące przepisy bądź też pozostawione do decyzji w ramach autonomii poszczególnych urzędów. Tak czy siak, pytanie jest bardziej proceduralne i wydaje się nie naruszać struktury danych formatu EZD.
#. Na ile istotne jest zwalczenie transaction malleability? [#ciagliwosc-dokumentow]_

Uwagi
-----

Daty zawarte w dokumentach nie mają (nie powinny mieć) większego znaczenia prawnego — znaczenie mają obiektywne daty wynikające z KSI bądź z innej usługi oznaczającej dokumenty czasem.

Nie da się efektywnie (w sposób dowodliwy) kontrolować dostępu do danych, w związku z czym rdzeń systemu nie zawiera informacji o tym, kto kiedy jaki dokument otrzymał. Można takie informacje przechowywać niezależnie, w celach poglądowych lub jeśli taka potrzeba wynika z obowiązujących przepisów. Można też w ramach systemu wytwarzać pokwitowania dostępu do dokumentów (być może jako wydzielona czynność w aktach sprawy).

Można wyobrazić sobie pracę w systemie EZD nad sprawami pobranymi już na komputer urzędnika w trybie offline, podobnie jak można pracować z Gitem w trybie offline. Można też rozważać częściową synchronizację prac z pominięciem centralnego serwera, w przypadku awarii tegoż.

.. _ciągliwość transakcji: https://en.bitcoin.it/wiki/Transaction_Malleability
.. _CAdES: https://tools.ietf.org/html/rfc5126
.. _XAdES: https://www.w3.org/TR/XAdES/
.. _RFC 6920: https://tools.ietf.org/html/rfc6920

.. [#ciagliwosc-dokumentow]
   Por. `ciągliwość transakcji`_ w Bitcoinie.
