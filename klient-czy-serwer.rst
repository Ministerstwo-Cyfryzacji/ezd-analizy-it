Rozdział kompetencji między klienta a serwer w systemie EZD
===========================================================

Wstęp
-----

Współczesne aplikacje internetowe tworzone są częstokroć w architekturze klient-serwer. Część operacji wykonywana jest na stacji roboczej użytkownika za pomocą oprogramowania, które komunikuje się z serwerem wykonującym inne (zazwyczaj newralgiczne) operacje. W niniejszym dokumencie rozpatrujemy podział klient-serwer w kontekście tego, kto ma kontrolę nad wykonywanym kodem, w związku z czym zdefiniujemy ten podział w zaburzony sposób:

1. Za kod kliencki uznajemy kod uruchamiany przez użytkownika z uprzednią faktyczną możliwością ustalenia jego dokładnej wersji (obliczenia funkcji skrótu).
2. Za kod serwerowy uznajemy kod uruchamiany bądź to bezpośrednio na serwerze, bądź kod wstrzykiwany przez serwer na urządzenie klienta i uruchamiany bez jego explicite wyrażonej akceptacji.

Oznacza to, że w typowej aplikacji WWW uruchamianej w typowej przeglądarce internetowej do strony serwerowej zaliczamy m.in. kod HTML i JavaScript; rolę klienta pełni w tym układzie sama przeglądarka.

Cechą kodu klienckiego jest to, że kontrolę nad nim ma w ogólności użytkownik; nad kodem serwerowym kontrolę ma natomiast administrator serwera. Oznacza to, że podział obowiązków między klienta a serwer powinien odzwierciedlać faktyczny (prawny) podział obowiązków między poszczególne podmioty[#kto-stoi-za-serwerem]_. W przeciwnym wypadku doprowadzi to do sytuacji, w której kontrolę nad czynnościami, za który odpowiedzialny jest jeden podmiot, będzie miał inny podmiot — wprowadzając do systemu niebezpieczny element zaufania.

Podział kompetencji w urzędzie
------------------------------

Urzędnik jest osobiście odpowiedzialny za składane przez siebie podpisy, skąd wynika, że po jego stronie (po stronie klienta) powinno się odbywać zarówno samo złożenie podpisu, jak i weryfikacja dokumentu, pod którym podpis jest składany[#podpis-pod-lancuchem-dokumentow]_. W szczególności podgląd dokumentu przed podpisaniem powinien odbywać się po stronie klienta, aby wykluczyć możliwość podmiany.

Z kolei po stronie serwera zazwyczaj leży zarządzanie dostępem do poszczególnych obiektów, centralna synchronizacja bazy danych, nadawanie kolejnych numerów nowo tworzonym obiektom, rozsyłanie powiadomień emailowych i tego typu pomocniczo-administracyjne czynności.

Wytwarzanie dokumentów może odbywać się zarówno po stronie klienta, jak i serwera, aczkolwiek jeśli urzędnik wytwarza dokument osobiście, to skorzystanie z aplikacji klienckiej pozwala mu uniknąć dodatkowej weryfikacji/podglądu dokumentu przed podpisaniem[#weryfikacja-dokumentow-z-serwera].

Sposoby na przeniesienie części kontroli na stronę klienta
----------------------------------------------------------

1. Stworzenie aplikacji biurkowej.
2. Stworzenie aplikacji przeglądarkowej, której kod jest serwowany w całości z lokalnego komputera urzędnika.

Istotne jest też `odtwarzalne budowanie paczek`_ z kodem klienckim.

Podpisywanie kodu w aplikacjach przeglądarkowych
------------------------------------------------

Zamiast przenoszenia bezpośredniej kontroli nad kodem na stronę klienta można by rozważyć wprowadzenie wymogu podpisywania przesyłanego kodu przez deweloperów aplikacji przeglądarkowych, aby znieść wymóg ufania administratorowi serwera. Wydaje się jednak, że współcześnie nie istnieją odpowiednie narzędzia techniczne ku temu, a także nie widać wyraźnych przewag takiego rozwiązania nad serwowaniem kodu z lokalnego komputera (przy odpowiednio skonfigurowanym mechanizmie aktualizacji).

Wnioski
-------

Stworzenie systemu EZD, w którym użytkownik będzie miał faktyczną kontrolę nad tym, co robi, może wymagać wykonywania części operacji po stronie klienta.

.. [#kto-stoi-za-serwerem]
   Pytaniem otwartym jest to, który podmiot jest reprezentowany przez serwer w kontekście pracy urzędu.

.. [#podpis-pod-lancuchem-dokumentow]
   Jeżeli podpisywany jest nie wyizolowany dokument, tylko dokument umiejscowiony w kontekście pewnego łańcucha, to potrzebna jest możliwość weryfikacji całego łańcucha.

.. [#weryfikacja-dokumentow-z-serwera]
   W praktyce wielu użytkowników korzystających z aplikacji serwerowej pominie ten etap, mając zaufanie do administratora serwera.

.. _odtwarzalne budowanie paczek: https://reproducible-builds.org/
