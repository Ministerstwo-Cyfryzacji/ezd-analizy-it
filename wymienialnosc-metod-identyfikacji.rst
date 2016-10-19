Wymienialność metod identyfikacji
=================================

Wstęp
-----

Dokument przedstawia kwestie związane z bezpieczeństwem i wygodą różnych metod identyfikacji, krytykując identyfikację za pomocą hasła i postulując zaprojektowanie systemu EZD w sposób na tyle elastyczny, aby pozwalał on na zastosowanie innych metod.

Identyfikacja za pomocą hasła
-----------------------------

We współczesnych systemach informatycznych nadal najbardziej popularną metodą identyfikacji jest zastosowanie hasła. W dobrze zabezpieczonych systemach hasło jest zapisywane z bazie serwisu w postaci praktycznie uniemożliwiającej jego odszyfrowanie. Często stosuje się również dodatkową warstwę zabezpieczeń w postaci weryfikacji dwuetapowej. Tym samym zabezpieczenie procesu identyfikacji opiera się na dwóch filarach: tym, co użytkownik wie (hasło) i tym, co posiada (klucz TOTP_). Taki sposób identyfikacji posiada szereg wad:

1. Choć hasło w dobrze zabezpieczonym serwisie jest przesyłane przez sieć w postaci zaszyfrowanej, a także trafia do bazy danych po jednostronnym zaszyfrowaniu, to pomiędzy tymi dwoma czynnościami znajduje się ono w pamięci serwera w postaci odszyfrowanej.
2. Użytkownikowi nie można udowodnić, że jego hasło jest faktycznie przechowywane w bazie danych w sposób niemożliwy do odszyfrowania.
3. Użytkownik wpisujący hasło w miejscu publicznym jest podatny na kradzież hasła przez osoby podglądające przez ramię oraz za pomocą kamer monitoringu.
4. Nawet jeśli hasło jest przechowywane na serwerze w postaci zaszyfrowanej, to użytkownik może mieć w innym, niewłaściwie zabezpieczonym serwisie, ustawione to samo hasło (choć jest to odradzaną praktyką). W przypadku włamania do bazy danych tamtego serwisu wystawione na ryzyko stają się oba konta.
5. Jeśli użytkownik stosuje politykę ustawiania różnych haseł w każdym serwisie, z którego korzysta, to stwarza to trudności w zapamiętaniu ich wszystkich.
6. `Rozporządzenie Ministra Spraw Wewnętrznych i Administracji w sprawie dokumentacji przetwarzania danych osobowych oraz warunków technicznych i organizacyjnych,jakim powinny odpowiadać urządzenia i systemy informatyczne służące do przetwarzania danych osobowych`_ nakłada obostrzenia na politykę zarządzania hasłami w niektórych systemach, co stanowi dodatkowe niedogodności.
7. Jeśli użytkownik, rezygnując z zapamiętywania wszystkich haseł, zaczyna korzystać z menedżera haseł, to wystawia się na inne zagrożenia w przypadku, gdy menedżer haseł nie jest zabezpieczony właściwie[#bezpieczenstwo-menedzerow-hasel]_.
8. Poszczególne serwisy internetowe mogą wyłączać autouzupełnianie haseł[#atrybut-autocomplete]_ (takie rozwiązanie stosuje m.in. ePUAP), co w przypadku poszczególnych przeglądarek niekiedy utrudnia wygodne stosowanie menedżerów haseł[#autocomplete-hasla]_.
9. Menedżery haseł wbudowane w przeglądarki częstokroć nie aktywują domyślnie szyfrowania haseł za pomocą hasła głównego, co sprowadza zabezpieczenie identyfikacji do jednego filaru (to, co użytkownik posiada).
10. Identyfikacja za pomocą hasła wymaga zazwyczaj, aby przed wysłaniem do serwera znajdowało się ono na komputerze użytkownika w postaci niezaszyfrowanej, co ułatwia jego wykradzenie (np. za pomocą programów typu keylogger).
11. Jeżeli użytkownik nieopatrznie zaloguje się po HTTP na oszukańczą stronę udającą prawdziwy serwis, to oznacza to złamanie części zabezpieczenia oferowanej w tym przypadku przez hasło.

Identyfikacja za pomocą kryptografii asymetrycznej
--------------------------------------------------

Metody identyfikacji wykorzystujące kryptografię asymetryczną pozwalają na uniknięcie wielu problemów związanych z hasłami:

1. Informacja wrażliwa (prywatny klucz kryptograficzny) nigdy nie jest przesyłana przez sieć, a zatem jedyną możliwością dla atakującego jest włamanie na urządzenie użytkownika. Biorąc pod uwagę to, że w skrajnych przypadkach może on wykorzystywać oddzielne urządzenie wyłącznie na potrzeby identyfikacji, czyni to potencjalne ataki dużo trudniejszymi.
2. Prywatne klucze kryptograficzne można dodatkowo zaszyfrować za pomocą hasła, osiągając w ten sposób poziom zabezpieczeń oparty o dwa wymienione wcześniej filary.
3. Identyfikacja za pomocą kryptografii asymetrycznej jest wygodniejsza (nie wymaga ręcznego przepisywania kodów TOTP).

Wśród tych metod można wymienić.

* Bitid_.
* BitAuth_. Identyfikacja następuje bez wykorzystania mechanizmu sesyj, dla każdego zapytania oddzielnie. Jest to zatem rozwiązanie bezstanowe.
* SQRL_.
* Certyfikaty klienckie SSL (wykorzystywane m.in. w `estońskiej e-rezydencji`_).
* Polski podpis kwalifikowany.

Identyfikacja w systemach EZD
-----------------------------

W kontekście systemów EZD można w naturalny sposób wskazać następujące potencjalne metody identyfikacji jako zastępniki identyfikacji za pomocą hasła:

1. Podpis kwalifikowany.
2. Cyfrowa tożsamość — w przypadku, gdyby taka usługa, oferująca funkcjonalność podpisu, została wprowadzona przez rząd.
3. Profil zaufany ePUAP — w przypadku, gdyby zyskał on adekwatny poziom zabezpieczeń[#bezpieczenstwo-epuap]_.
4. Certyfikaty klienckie SSL.

Z powyższych metod wyróżnić należy podpis kwalifikowany, jako że i tak prawdopodobnie będzie on wykorzystywany w systemie.

Wnioski
-------

Identyfikacja za pomocą hasła jest metodą dość prymitywną i problematyczną. W projektowaniu systemów EZD warto uwzględniać możliwość identyfikacji za pomocą bardziej zaawansowanych technicznie i prostych w użyciu metod, m.in. tych, które wykorzystują kryptografię asymetryczną. Architektura systemu EZD powinna umożliwiać łatwe dodawanie nowych metod identyfikacji.

.. [#bezpieczenstwo-menedzerow-hasel]
   Na ten temat zob. m.in.:

   1. `“Password Managers: Attacks and Defenses”`_. Dokument zdaje się przypisywać menedżerom haseł wiele win źle zabezpieczonych (głównie poprzez brak szyfrowania) stron internetowych, ale podaje też faktyczne luki (autouzupełnianie przy korzystaniu z HTTP haseł zapisanych podczas korzystania z HTTPS).
   2. Przechowywanie haseł niezaszyfrowanych (1 filar).

.. [#atrybut-autocomplete]
   Atrybut “autocomplete” znacznika ``<input>`` w HTML 5.

.. [#bezpieczenstwo-epuap]
   W temacie bezpieczeństwa ePUAP zobacz `„Nie używam profilu zaufanego na ePUAP”`_.

.. [#autocomplete-hasla]
   Na ten temat zob. m. in.:`“<form autocomplete="off"> no longer prevents passwords from being saved”`_. Historia zmian w przeglądarce Firefox wskazuje na trend ignorowania atrybutu ```autocomplete``` w przypadku pól haseł.

.. _TOTP: https://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm
.. _`“Password Managers: Attacks and Defenses”`: http://crypto.stanford.edu/~dabo/pubs/abstracts/pwdmgrBrowser.html
.. _`Rozporządzenie Ministra Spraw Wewnętrznych i Administracji w sprawie dokumentacji przetwarzania danych osobowych oraz warunków technicznych i organizacyjnych,jakim powinny odpowiadać urządzenia i systemy informatyczne służące do przetwarzania danych osobowych`: http://isap.sejm.gov.pl/DetailsServlet?id=WDU20041001024
.. _Bitid: https://github.com/bitid/bitid
.. _BitAuth: https://github.com/bitpay/bitauth
.. _SQRL: https://www.grc.com/sqrl/sqrl.htm
.. _estońskiej e-rezydencji: https://e-estonia.com/e-residents/about/
.. _„Nie używam profilu zaufanego na ePUAP”: http://www.computerworld.pl/news/382785/Nie.uzywam.profilu.zaufanego.na.ePUAP.html
.. _`“<form autocomplete="off"> no longer prevents passwords from being saved”`: https://www.fxsitecompat.com/en-CA/docs/2014/form-autocomplete-off-no-longer-prevents-passwords-from-being-saved/
