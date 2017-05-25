<h2>
 <p align="center">Zapis propozycji nowej budowy metadanych</p>
 <p align="center">Niezbędnych Elementów Struktury (NES)</p>
 <p align="center">dokumentów elektronicznych,</p>
 <p align="center">w Notacji Backusa-Naura (BNF)</p>
 <p align="center">oraz</p>
 <p align="center">plik XSD do automatycznej walidacji</p>
 <p align="center">paczek administracyjnych</p>
</h2>

<!-- TODO: link do „klikalnej” wersji dokumentu powinien wskazywać to samo repozytorium;
           ze względu na ustawienia naszego repozytorium, dokument HTML „klikalny” nie jest.
           Zmienić, gdy to będzie możliwe. -->
<p align="right"><em>Na skróty: dokument HTML jest tu: <a href="https://stas53.github.io/tests/nes_bnf.html"> nes_bnf.html</a>
</em></p>
<p align="right"><em>dokument XSD jest tu: <a href="nes_20.xsd"> nes_20.xsd</a>
</em></p>

### Środowisko

[Rozporządzenie Ministra Spraw Wewnętrznych i Administracji
z dnia 30 października 2006 r.](http://isap.sejm.gov.pl/DetailsServlet?id=WDU20062061517) określa niezbędne elementy struktury
(*NES*) dokumentów elektronicznych powstałych i gromadzonych w organach państwowych, państwowych jednostkach organizacyjnych,
w organach jednostek samorządu terytorialnego i samorządowych jednostkach organizacyjnych.  
Stanowi ono, że każdy dokument elektroniczny powinien być opatrzony pewnymi metadanymi takimi jak identyfikator, twórca, tytuł,
data, format, zasady dostępu itd.

Z kolei, [Rozporządzenie Ministra Spraw Wewnętrnych i Administracji
z dnia 2 listopada 2006 r.](http://isap.sejm.gov.pl/DetailsServlet?id=WDU20062061519) w sprawie wymagań technicznych formatów zapisu
i informatycznych nośników danych, na których utrwalono materiały archiwalne przekazywane do archiwów państwowych, określło format
całej paczki danych (tzw. „paczki archiwalnej”), który powinien być stosowany przy przekazywaniu danych z urzędów do Archiwów Państwowych.
W szczególności, w Załączniku do Rozporządzenia został określony konkretny format plików XML, który należy zastosować przy eksporcie
metadanych. Format ten zdefiniowano formalnie przy pomocy tzw. XSD, czyli pliku definicji formatów XML.

### Propozycja zmian NES

Grupa robocza powołana w końcu 2016 roku przez porozumienie MC - NSA - PUW, przy współpracy osób z MKiDN, NDAP i Laboratorium EE,
przygotowała opis zmodyfikowanej wersji NES, dla potrzeb „paczki administracyjnej” — formatu danych przewidzianego do przekazywania
danych o sprawach z systemów EZD urzędów państwowych, do Sądów Administracyjnych.  
Wydaje się, że po pewnych rozszerzeniach, ten sam format będzie mógł być wykorzystany do tworzenia „paczek migracyjnych” — do
przekazywania danych pomiędzy systemami EZD.

[Dokument źródłowy przygotowany przez Grupę Roboczą](http://epuap.gov.pl/wps/PA_E2_PI/zalacznik_oi/zalacznik_oi_1460_1)
jest obszernym materiałem tekstowym (.docx), zawierającym opisy poszczególnych metadanych oraz przykłady ich użycia.

### Zapis w notacji BNF

Dla umożliwienia szybkiego i wygodnego przeglądania proponowanej nowej budowy NES, zaproponowaliśmy zapisanie struktury metadanych
w postaci schematów produkcji, uzywanych do definiowania
[języków bezkontekstowych](http://edu.pjwstk.edu.pl/wyklady/jfa/scb/jfa-main-node11.html), w zapisie znanym jako
Notacja Backusa-Naura — BNF.

Dokument HTML [nes_bnf.html](https://stas53.github.io/tests/nes_bnf.html)
można przeglądać wykorzystując to, że symbole nieterminalne są w nim aktywnymi linkami do definicji.
Pokazuje on zarówno strukturę danych, jak i ich zapis w tekstowym formacie XML.
Na stronie znajduje się przycisk, aktywujący wyświetlenie objaśnień.  
Sam [kod źródłowy nes_bnf.html](nes_bnf.html) jest dostepny w niniejszym repozytorium.

### Plik XSD do automatycznej walidacji paczek administracyjnych

Paczki danych eksportowane z systemów EZD, powinny być zgodne z wymaganiami sformułowanymi dla
Niezbędnych Elementów Struktury dokumentów elektronicznych i z formatem XML dla takiej paczki.

Przygotowano [plik definicyjny XSD](nes_20.xsd), który może zostać użyty do walidacji
formalnej poprawności paczek – plików XML.

 Plik XSD można użyć do kontroli poprawniści paczki, posługując się w tym celu generalnym programem walidacyjnym, np.
[dostępnym w  sieci](http://www.freeformatter.com/xml-validator-xsd.html). Docelowo, należy przygotować specjalizowany
 program do walidacji paczek administracyjnych, oparty na tym konkretnym XSD.

Rozważenia wymaga też pomysł napisania „walidatora semantycznego” — kontrolującego znaczenie danych, których to cech
nie można skontrolować przy pomocy badania samej składni XML. Nadmienić należy, że dla paczek archiwalnych zdefiniowanych
w rozporządzeniach z 2006 r., takiego walidatora semantycznego nie skonstruowano.
