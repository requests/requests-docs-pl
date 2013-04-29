.. _faq:

Często zadawane pytania
=======================

Ta część dokumentacji odpowiada na częste pytania o Requests.

Kodowane dane?
--------------

Requests automatycznie dekoduje odpowiedzi gzip-encoded, i czyni co w jego
mocy, aby zdekodować odpowiedż do Unicode.

Możesz bezpośrednio dostać się do surowej odpowiedzi (a nawet socketu) jeśli
zaistnieje taka potrzeba.

Własny User-Agent?
------------------

Requests pozwala na łatwe zmienianie ciągów User-Agent, wraz z wszystkimi
innymi nagłówkami HTTP.

Dlaczego nie Httplib2?
----------------------

Chris Adams podsumował to świetnie na
`Hacker News <http://news.ycombinator.com/item?id=2884406>`_:

    httplib2 jest częścią “dlaczego powinieneś używać requests”: jest bardziej
    respektowalny jako klient ale nie jest dobrze udokumentowany i potrzeba za
    dużo kodu dla podstawowych operacji. Doceniam to, co httplib2 próbuje
    zrobić, że jest wiele niskopoziomowych kłopotóæ w budowaniu nowoczesnego
    klienta HTTP, ale naprawdę, po prostu użyj requests. Kenneth Reitz jest
    bardzo zmotywowany i rozumie do jakiego stopnia proste rzeczy powinny być
    proste, tymczasem httplib2 jest bardziej akademickim ćwiczeniem niż czymś
    co powinno być używane do budowania systemów w produkcji[1].

    Uwaga: jestem w pliku AUTHORS dla request ale jestem odpowiedzialny tylko
    za około 0.0001% wspaniałości.

    1. http://code.google.com/p/httplib2/issues/detail?id=96 jest świetnym
    przykładem: dokuczliwy bug który dotyczy wielu ludzi, przez miesiące
    istniała poprawka, która działała świetnie na forku przetestowanym paroma
    TB danych, ale ponad rok zajęło dostanie się tego do trunk i jeszcze dłużej
    do PyPI gdzie każdy inny projekt wymagający "httplib2" dostałby działającą
    wersję.


Wsparcie dla Python 3
---------------------

Tak! Oto lista oficjalnie wspieranych platform Pythona:

* Python 2.6
* Python 2.7
* Python 3.1
* Python 3.2
* Python 3.3
* PyPy 1.9
