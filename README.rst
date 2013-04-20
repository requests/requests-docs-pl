Requests: HTTP dla ludzi
========================


.. image:: https://travis-ci.org/kennethreitz/requests.png?branch=master
        :target: https://travis-ci.org/kennethreitz/requests

Requests jest biblioteką HTTP na licencji Apache2, w języku Python, dla istot ludzkich.

Większość istniejących modułów Pythona używanych do wysyłania żądań HTTP jest
niezwykle rozwlekła i niewygodna. Wbudowany w Pythona moduł urllib2 oferuje
większość potrzebnych możliwości HTTP, ale API jest całkowicie zepsute. Wymaga
olbrzymiej ilości pracy (nawet nadpisywania metod) żeby wykonać najprostsze
zadania.

Tak nie powinno to wyglądać. Nie w Pythonie.

.. code-block:: pycon

    >>> r = requests.get('https://api.github.com', auth=('user', 'pass'))
    >>> r.status_code
    204
    >>> r.headers['content-type']
    'application/json'
    >>> r.text
    ...

Zobacz `ten sam kod, bez Requests <https://gist.github.com/973705>`_.

Requests pozwala na wysyłanie żądań HTTP/1.1. Możesz dodawać nagłówki, dane
formularzy, pliki multipart i parametry z prostymi słownikami Pythona (typ
``dict``), a dostęp do odpowiedzi jest równie prosty. Requests jest napędzane
przez httplib i  `urllib3 <https://github.com/shazow/urllib3>`_, ale wykonuje
całą ciężką pracę i robi zwariowane hacki za ciebie.

Funkcje
-------

- Międzynarodowe domeny i URL-e
- Keep-Alive i Connection Pooling
- Sesje z zachowywaniem Cookies (ciasteczek)
- Weryfikacja SSL w stylu przeglądarek
- Basic/Digest Authentication
- Eleganckie Cookies (klucz/wartość)
- Automatyczna dekompresja
- Odpowiedzi Unicode
- Przesyłanie plików multipart
- Timeout połączeń
- Wątkowo-bezpieczny


Instalacja
----------

Aby zainstalować requests, wystarczy:

.. code-block:: bash

    $ pip install requests

Albo, jeśli musisz:

.. code-block:: bash

    $ easy_install requests

Ale naprawdę nie powinieneś tego robić.

Wnieś swój wkład!
-----------------

#. Sprawdź listę otwartych problemów albo otwórz nowy aby rozpocząć dyskusję
   nad nową funkcją albo błędem. Istnieje tag Contributor Friendly dla
   problemów które powinny być idealne dla ludzi, którzy nie są jeszcze zbyt
   obeznani z kodem.
#. Zforkuj repozytorium_ na GitHubie aby zacząć dokonywać zmian na gałęzi
   **master** (albo na gałęzi stworzonej z niej).
#. Napisz test, który pokazuje, że bug został naprawiony albo że funkcja działa
   jak oczekiwano.
#. Wyślij pull request i wkurzaj opiekuna, aż nie zostanie włączony do
   oficjalnego repozytorium i opublikowany. :) Upewnij się, że dodałeś siebie
   do pliku AUTHORS_.

.. _repozytorium: http://github.com/kennethreitz/requests
.. _AUTHORS: https://github.com/kennethreitz/requests/blob/master/AUTHORS.rst
