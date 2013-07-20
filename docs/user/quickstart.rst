.. _quickstart:

Quickstart
==========

.. module:: requests.models

Chcesz zacząć? Ta strona to dobre wprowadzenie jak zacząć pracować z Requests.
To wprowadzenie zakład, że już zainstalowałeś Requests.  Jeśli tego jeszcze nie
zrobiłeś, sprawdź sekcję :ref:`Instalacja <install>`.

Po pierwsze, upewnij się, że:

* Requests są :ref:`zainstalowane <install>`
* Requests są :ref:`aktualne <updates>`

Zacznijmy od kilku prostych przykładów.


Wykonaj Żądanie
---------------

Wykonywanie żądania z Requests jest bardzo proste.

Zacznij od zaimportowania modułu Requests::

    >>> import requests

Teraz spróbujemy pobrać stronę. W tym przykładzie, spróbujmy pobrać publiczną
oś czasu na GitHubie::

    >>> r = requests.get('https://github.com/timeline.json')

Teraz mamy obiekt klasy :class:`Response` zwany ``r``. Możemy uzyskać wszelkie
potrzebne informacje z tego obiektu.

Proste API Requests oznacza, że wszystkie formy żądań HTTP są równie oczywiste.
Na przykład, tak wykonuje się żądanie POST::

    >>> r = requests.post("http://httpbin.org/post")

Nieźle, prawda? A jak wykonuje się inne żądania HTTP: PUT, DELETE, HEAD i
OPTIONS? Równie prosto::

    >>> r = requests.put("http://httpbin.org/put")
    >>> r = requests.delete("http://httpbin.org/delete")
    >>> r = requests.head("http://httpbin.org/get")
    >>> r = requests.options("http://httpbin.org/get")

To jest dobre, ale to dopiero początek tego, co może zrobić Requests.


Podawanie parametrów w URL-ach
------------------------------

Bardzo często chcesz wysłać jakieś dane w ciągu zapytania URL-a. Jeśli
konstruowałbyś URL ręcznie, byłyby to pary klucz/wartość w URL-u po znaku
zapytania, np. ``httpbin.org/get?key=val``.
Requests pozwala podawać te argumenty jako słownik (``dict``), używając keyword
argumentu ``params``. Na przykład, jeśli chciałbyś przekazać
``key1=value1`` i ``key2=value2`` do ``httpbin.org/get``, użyłbyś poniższego
kodu::

    >>> payload = {'key1': 'value1', 'key2': 'value2'}
    >>> r = requests.get("http://httpbin.org/get", params=payload)

Możesz zobaczyć, że URL został poprawnie zakodowany przez wydrukowanie URL-a::

    >>> print r.url
    http://httpbin.org/get?key2=value2&key1=value1

Zauważ, że klucz o wartości ``None`` nie zostanie dodany do URL-a.


Zawartość odpowiedzi
--------------------

Możemy przeczytać zawartość odpowiedzi serwera. Skorzystamy ponownie z osi
czasu z GitHuba::

    >>> import requests
    >>> r = requests.get('https://github.com/timeline.json')
    >>> r.text
    u'[{"repository":{"open_issues":0,"url":"https://github.com/...

Requests automatycznie zdekoduje treść z serwera. Większość charsetów Unicode
jest poprawnie i bezszwowo dekodowana.

Kiedy wykonujesz żądanie, Requests inteligentnie zgaduje kodowanie na podstawie
nagłówków HTTP. To kodowanie jest używane przez ``r.text``. Możesz dowiedzieć
się, jakiego kodowanie Requests używa, i je zmienić, używając własności
``r.encoding``::

    >>> r.encoding
    'utf-8'
    >>> r.encoding = 'ISO-8859-1'

Jeśli zmienisz kodowanie, Requests użyje nowej wartości ``r.encoding`` przy
whenever you call ``r.text``.

Requests może też używać twoich własnych kodowań jeśli będziesz ich
potrzebował. Jeśli stworzyłeś swoje własne kodowanie i zarejestrowałeś je w
module ``codecs``, możesz po prostu użyć nazwy kodeka jako wartość
``r.encoding`` i Requests zajmie się dekodowaniem za ciebie.

Binarna zawartość odpowiedzi
----------------------------

Możesz też uzyskać dostęp do body odpowiedzi jako bajty, dla żądań nietekstowych::

    >>> r.content
    b'[{"repository":{"open_issues":0,"url":"https://github.com/...

Transfer-encodings: ``gzip`` i ``deflate`` są automatycznie dekodowane.

Na przykład, jeśli chcesz stworzyć obrazek z danych binarnych, możesz użyć
poniższego kodu::

    >>> from PIL import Image
    >>> from StringIO import StringIO
    >>> i = Image.open(StringIO(r.content))


Zwartość odpowiedzi JSON
------------------------

Istnieje też wbudowany dekoder JSON, jeśli zajmujesz się danymi JSON::

    >>> import requests
    >>> r = requests.get('https://github.com/timeline.json')
    >>> r.json()
    [{u'repository': {u'open_issues': 0, u'url': 'https://github.com/...

Jeśli dekodowanie JSON nie powiedzie się, ``r.json`` podnosi wyjątek.  Na
przykład, jeśli odpowiedź wyniesie 401 (Unauthorized), próba użycia ``r.json``
podnosi ``ValueError: No JSON object could be decoded``


Surowa zawratość odpowiedzi
---------------------------

Jeśli chcesz otrzymać surową odpowiedź socket od serwera (a zazwyczaj nie
chcesz), możesz użyć ``r.raw``. Jeśli chcesz to zrobić, upewnij się, że
ustawiłeś ``stream=True`` w twoim oryginalnym żądaniu. Jeśli to uczynisz,
możesz zrobić tak::

    >>> r = requests.get('https://github.com/timeline.json', stream=True)
    >>> r.raw
    <requests.packages.urllib3.response.HTTPResponse object at 0x101194810>
    >>> r.raw.read(10)
    '\x1f\x8b\x08\x00\x00\x00\x00\x00\x00\x03'


Własne nagłówki
---------------

Jeśli chciałbyś dodać własne nagłówki HTTP do żądania, po prostu użyj parametru ``headers`` i umieść nagłówki w słowniku (``dict``).

Na przykład, nie podaliśmy content-type w poprzednim przykładzie::

    >>> import json
    >>> url = 'https://api.github.com/some/endpoint'
    >>> payload = {'some': 'data'}
    >>> headers = {'content-type': 'application/json'}

    >>> r = requests.post(url, data=json.dumps(payload), headers=headers)


Bardziej skompliowane żądania POST
----------------------------------

Zazwyczaj, chcesz wysłać dane form-encoded — na przykład z formularza w HTML.
Aby to zrobić, po prostu przekaż słownik do argumentu ``data``. Twój słownik
danych będzie automatycznie zakodowany w formacie formularzy kiedy żądanie
zostanie wykonane::

    >>> payload = {'key1': 'value1', 'key2': 'value2'}
    >>> r = requests.post("http://httpbin.org/post", data=payload)
    >>> print r.text
    {
      ...
      "form": {
        "key2": "value2",
        "key1": "value1"
      },
      ...
    }

Ale czasami chcesz wysłać dane które nie są form-encoded. Jeśli przekażesz ``string`` zamiast ``dict``, dane będą wysłane prosto to serwera.

Na przykład, GitHub API v3 akceptuje dane POST/PATCH zakodowane w JSON::

    >>> import json
    >>> url = 'https://api.github.com/some/endpoint'
    >>> payload = {'some': 'data'}

    >>> r = requests.post(url, data=json.dumps(payload))


POST — plik zakodowany Multipart
--------------------------------

Requests sprawia, że dodawanie plików zakodowanych Multipart jest proste::

    >>> url = 'http://httpbin.org/post'
    >>> files = {'file': open('report.xls', 'rb')}

    >>> r = requests.post(url, files=files)
    >>> r.text
    {
      ...
      "files": {
        "file": "<censored...binary...data>"
      },
      ...
    }

Możesz jawnie ustawić nazwę pliku::

    >>> url = 'http://httpbin.org/post'
    >>> files = {'file': ('report.xls', open('report.xls', 'rb'))}

    >>> r = requests.post(url, files=files)
    >>> r.text
    {
      ...
      "files": {
        "file": "<censored...binary...data>"
      },
      ...
    }

Jeśli chcesz, możesz wysłać ciągi znaków, które będą otrzymane jako pliki::

    >>> url = 'http://httpbin.org/post'
    >>> files = {'file': ('report.csv', 'some,data,to,send\nanother,row,to,send\n')}

    >>> r = requests.post(url, files=files)
    >>> r.text
    {
      ...
      "files": {
        "file": "some,data,to,send\\nanother,row,to,send\\n"
      },
      ...
    }


Kody odpowiedzi
---------------

Możemy sprawdzić kod statusu odpowiedzi::

    >>> r = requests.get('http://httpbin.org/get')
    >>> r.status_code
    200

Requests ma też wbudowany obiekt sprawdzania kodów dla łatwej referencji::

    >>> r.status_code == requests.codes.ok
    True

Jeśli wykonaliśmy złe żądanie (odpowiedź 4XX błąd klienta lub 5XX błąd
serwera), możemy podnieść wyjątek używając :class:`Response.raise_for_status()`::

    >>> bad_r = requests.get('http://httpbin.org/status/404')
    >>> bad_r.status_code
    404

    >>> bad_r.raise_for_status()
    Traceback (most recent call last):
      File "requests/models.py", line 832, in raise_for_status
        raise http_error
    requests.exceptions.HTTPError: 404 Client Error

Ale, ponieważ ``status_code`` dla ``r`` wynosił ``200``, ``raise_for_status()``
wykonuje::

    >>> r.raise_for_status()
    None

Wszystko jest dobrze.


Nagłówki odpowiedzi
-------------------

Możemy przejrzeć nagłówki odpowiedzi serwera przy użyciu słownika Pythona::

    >>> r.headers
    {
        'content-encoding': 'gzip',
        'transfer-encoding': 'chunked',
        'connection': 'close',
        'server': 'nginx/1.0.4',
        'x-runtime': '148ms',
        'etag': '"e1ca502697e5c9317743dc078f67693f"',
        'content-type': 'application/json'
    }

Ten słownik jest specjalny: jest on stworzony tylko dla nagłówków HTTP. Zgodnie z
`RFC 2616 <http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html>`_, wielkość
liter nie ma znaczenia w nagłówkach HTTP.

Więc możemy uzyskać dostęp do nagłówków używając dowolnej wielkości liter::

    >>> r.headers['Content-Type']
    'application/json'

    >>> r.headers.get('content-type')
    'application/json'


Ciasteczka (cookies)
--------------------

Jeśli odpowiedź zawiera jakieś ciasteczka, możesz szybko uzyskać dostęp do
nich::

    >>> url = 'http://example.com/some/cookie/setting/url'
    >>> r = requests.get(url)

    >>> r.cookies['example_cookie_name']
    'example_cookie_value'

Aby wysłać własne ciasteczka do serwera, możemy użyć parametru ``cookies``::

    >>> url = 'http://httpbin.org/cookies'
    >>> cookies = dict(cookies_are='working')

    >>> r = requests.get(url, cookies=cookies)
    >>> r.text
    '{"cookies": {"cookies_are": "working"}}'


Przekierowania i historia
-------------------------

Requests automatycznie przekieruje żądania przy użyciu GET i OPTIONS.

GitHub przekierowuje wszystkie żądania HTTP na HTTPS. Możemy użyć metody
``history`` obiektu Response do śledzenia przekierowań. Zobaczmy, co robi
GitHub:

    >>> r = requests.get('http://github.com')
    >>> r.url
    'https://github.com/'
    >>> r.status_code
    200
    >>> r.history
    [<Response [301]>]

Lista :class:`Response.history` zawiera obiekty :class:`Request` stworzone w
celu zakończenia żądania. Lista jest posortowana od najstarzego do najnowszego
żądania.

Jeśli używasz GET lub OPTIONS, możesz zablokować obsługę przekierowań przy
użyciu parametru ``allow_redirects``::

    >>> r = requests.get('http://github.com', allow_redirects=False)
    >>> r.status_code
    301
    >>> r.history
    []

Jeśli używasz POST, PUT, PATCH, DELETE lub HEAD, możesz też włączyć
automatyczne przekierowania::

    >>> r = requests.post('http://github.com', allow_redirects=True)
    >>> r.url
    'https://github.com/'
    >>> r.history
    [<Response [301]>]


Timeouty (przekroczenia limitu czasu żądania)
---------------------------------------------

Możesz przerwać czekanie na odpowiedź przez Requests po danej liczbie sekund
przy użyciu parametru ``timeout``::

    >>> requests.get('http://github.com', timeout=0.001)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    requests.exceptions.Timeout: HTTPConnectionPool(host='github.com', port=80): Request timed out. (timeout=0.001)


.. admonition:: Note:

    ``timeout`` wpływa tylko na połączenie, nie na pobieranie odpowiedzi.


Błędy i wyjątki
---------------

W razie problemu z siecią (np. nieudane żądanie do DNS, odmowa połączenia
itd.), Requests podniesie wyjątek :class:`~requests.exceptions.ConnectionError`.

W razie rzadkiej nieprawidłowej odpowiedzi HTTP, Requests podniesie wyjątek
:class:`~requests.exceptions.HTTPError`.

Jeśli żądanie osiągnie timeout, wyjątek :class:`~requests.exceptions.Timeout` jest podnoszony.

Jeśli żądanie przekroczy skonfigurowany limit maksymalnych przekierowań, wyjątek
:class:`~requests.exceptions.TooManyRedirects` jest podnoszony.

Wszystkie wyjątki podnoszone przez Requests dziedziczą z
:class:`requests.exceptions.RequestException`.

-----------------------

Gotowy na więcej?  Sprawdź sekcję :ref:`zaawansowaną <advanced>`.
