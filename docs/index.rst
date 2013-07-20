.. Requests documentation master file, created by
   sphinx-quickstart on Sun Feb 13 23:54:25 2011.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Requests: HTTP dla ludzi
========================

Wydanie v\ |version|. (:ref:`Instalacja <install>`)

Requests jest biblioteką HTTP na :ref:`licencji Apache2 <apache2>`, w języku
Python, dla istot ludzkich.

Wbudowany w Pythona moduł **urllib2** oferuje
większość potrzebnych możliwości HTTP, ale API jest całkowicie **zepsute**.
Zostało zbudowane dla innych czasów — i dla innej sieci. Wymaga *olbrzymiej*
ilości pracy (nawet nadpisywania metod) żeby wykonać najprostsze zadania.

Tak nie powinno to wyglądać. Nie w Pythonie.

::

    >>> r = requests.get('https://api.github.com/user', auth=('user', 'pass'))
    >>> r.status_code
    200
    >>> r.headers['content-type']
    'application/json; charset=utf8'
    >>> r.encoding
    'utf-8'
    >>> r.text
    u'{"type":"User"...'
    >>> r.json()
    {u'private_gists': 419, u'total_private_repos': 77, ...}

Zobacz `podobny kod, bez Requests <https://gist.github.com/973705>`_.

Requests bierze na siebie całą trudną pracę z HTTP/1.1 w Pythonie — czyniąc twoją integrację z usługami sieciowymi bezszwową.  Nie ma potrzeby ręcznie dodawać ciągów zapytań do URL–i, albo poddawać twoje dane POST metodzie form-encode. Keep-alive i conection pooling są automatycznew 100% za sprawą `urllib3 <https://github.com/shazow/urllib3>`_, wbudowanego w Requests.


Testimonials
------------

Rząd Jej Królewskiej Mości, Amazon, Google, Twilio, Mozilla, Heroku, PayPal, NPR, Obama for America, Transifex, Native Instruments, The Washington Post, Twitter, SoundCloud, Kippt, Readability, i Instytucje Federalne Stanów Zjednoczonych używają Requests wewnętrznie.  Requests zostało pobrane ponad 3 000 000 razy z PyPI.

**Armin Ronacher**
    Requests to perfekcyjny przykład, jak piękne może być API z prawidłowym
    poziomem abstrakcji.

**Matt DeBoard**
    Wytatuuję sobie jakoś moduł Pythona requests autorstwa @kennethreitz’a na
    moim ciele.  W całości.

**Daniel Greenfeld**
    Zastąpiłem kod spaghetti o długości 1200 LOC z 10 liniami kodu dzięki
    bibliotece Requests autorstwa @kennethreitz’a. Dziesiejszy dzień był
    NIESAMOWITY.

**Kenny Meyers**
    HTTP w Pythonie: W razie wątpliwości, albo w razie ich braku, użyj Requests.  Piękne, proste, Pythoniczne.

Wspierane funkcje
-----------------

Requests jest gotowy na dziesiejszą sieć.

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
- ``.netrc`` support
- Python 2.6—3.3
- Wątkowo-bezpieczny


Instrukcja użytkownika
----------------------

Ta część dokumentacji, w większości proza, zaczyna się od podstawowych
informacji o Requests, a potem skupia się na instrukcjach krok po kroku
uzyskiwania jak najwięcej z Requests.

.. toctree::
   :maxdepth: 2

   user/intro
   user/install
   user/quickstart
   user/advanced
   user/authentication


Informacje o społeczności
--------------------------

Ta część dokumentacji, w większości proza, opisuje ekosystem i społeczność
Requests.

.. toctree::
   :maxdepth: 1

   community/faq
   community/out-there.rst
   community/support
   community/updates

Dokumentacja API
----------------

Jeśli poszukujesz informacji o specyficznej funkcji, klasie lub metodzie, ta
część dokumentacji jest dla ciebie.

.. toctree::
   :maxdepth: 2

   api


Instrukcja współpracownika
--------------------------

Jeśli chcesz wnieść wkład do projektu, ta część dokumentacji jest dla ciebie.

.. toctree::
   :maxdepth: 1

   dev/philosophy
   dev/internals
   dev/todo
   dev/authors
