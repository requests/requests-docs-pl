Jak pomóc
=========

Requests są aktywnie rozwijane, a kontrybucje są więcej niż mile widziane!

#. Poszukaj otwartych problemów albo otwórz nowy aby rozpocząć dyskusję nad
   bugiem.  Istnieje tag *Contributor Friendly* dla problemów idealnych dla
   osób nieobeznanych z kodem.
#. Sforkuj `repozytorium <https://github.com/kennethreitz/requests>`_ na
   GitHubie i zacznij dokonywać zmian na nowej gałęzi (branch).
#. Napisz test pokazujący, że bug został naprawiony.
#. Wyślij pull request i wkurzaj maintainera dopóki nie zostanie zmerge’owany i opublikowany.
   Upewnij się, że dodałes się do pliku `AUTHORS <https://github.com/kennethreitz/requests/blob/master/AUTHORS.rst>`_.

Zamrożenie feature’ów
---------------------

Od v1.0.0, Requests wszedł w status zamrożenia feature’ów. Prośby o nowe funkcje i Pull Requesty je implementujące nie będą akceptowane.

Zależności do developmentu
--------------------------

Będziesz musiał zainstalować py.test aby uruchomić testy Requests::

    $ pip install -r requirements.txt
    $ invoke test
    py.test
    platform darwin -- Python 2.7.3 -- pytest-2.3.4
    collected 25 items

    test_requests.py .........................
    25 passed in 3.50 seconds

Środowiska runtime
------------------

Requests obecnie wspiera następujące wersje Pythona:

- Python 2.6
- Python 2.7
- Python 3.1
- Python 3.2
- Python 3.3
- PyPy 1.9

Wsparcie dla Pythona 3.1 i 3.2 może zostać usunięte w każdej chwili.

Google App Engine nigdy nie będzie oficjanie wspierane. Pull requesty dot.
kompatybilności będą akceptowane, o ile nie skomplikują kodu.


Jesteś szalony?
---------------

- wsparcie dla SPDY byłoby świetne. Bez rozszerzeń w C.

Paczkowanie w Downstreamie
--------------------------

Jeśli paczkujesz Requests, zauważ że musisz też redystrybuować plik
``cacerts.pem`` aby uzyskać poprawne funkcjonowanie SSL.
