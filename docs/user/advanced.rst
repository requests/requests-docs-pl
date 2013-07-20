.. _advanced:

Użycie zaawansowane
===================

Ten dokument opisuje niektóre z najważniejszych zaawansowanych funkcji
Requests.


Obiekty Session
---------------

Obiekty Seesion pozwalają na zachowywanie niektórych parametrów pomiędzy
żądaniami. Zachowują one również ciasteczka pomiędzy wszystkimi żądaniami
wykonanymi z instancji Session.

Obiekt Session ma wszystkie metody głównego API Requests.

Zachowajmy trochę ciasteczek pomiędzy żadaniami::

    s = requests.Session()

    s.get('http://httpbin.org/cookies/set/sessioncookie/123456789')
    r = s.get("http://httpbin.org/cookies")

    print r.text
    # '{"cookies": {"sessioncookie": "123456789"}}'


Sesje mogą też być używane do dostarczania domyślnych danych do metod żądań. Robi się to przekazując dane do właściwości obiektu Session::

    s = requests.Session()
    s.auth = ('user', 'pass')
    s.headers.update({'x-test': 'true'})

    # both 'x-test' and 'x-test2' are sent
    s.get('http://httpbin.org/headers', headers={'x-test2': 'true'})


Jakiekolwiek słowniki przekazane do metody żądania będą złączone z ustawionymi wartościami sesyjnymi. Parametry metod nadpisują parametry sesji.

.. admonition:: Usuń wartość ze słownika parametru

    Czasami chcesz pominąć klucze sesyjne w słowniku parametru. Aby to
    zrobić, wystarczy ustawić wartość klucza na ``None`` w parametrze
    metody. Zostanie on automatycznie pominięty.

Wszystkie wartości w sesji są dostępne bezpośrednio. Sprawdź
:ref:`dokumentację API sesji <sessionapi>` aby dowiedzieć się więcej.

Obiekty Request i Response
--------------------------

Kiedy wywołujesz requests.*() robisz dwie ważne rzeczy: po pierwsze,
konstruujesz obiekt ``Request`` który zostanie wysłany do serwera aby
zażądać albo wykonać zapytanie dotyczące jakiegoś zasobu. Po drugie,
obiekt ``Response`` jest generowany kiedy ``requests`` otrzymuje
odpowiedź od serwera. Obiekt Response zawiera wszystkie informacje
zwrócone przez serwer oraz oryginalny obiekt ``Request``.  Oto proste
żądanie aby otrzymać parę ważnych informacji z serwerów Wikipedii::


    >>> r = requests.get('http://en.wikipedia.org/wiki/Monty_Python')

Jeśli chcemy uzyskać dostęp do nagłówków, robimy to::

    >>> r.headers
    {'content-length': '56170', 'x-content-type-options': 'nosniff', 'x-cache':
    'HIT from cp1006.eqiad.wmnet, MISS from cp1010.eqiad.wmnet', 'content-encoding':
    'gzip', 'age': '3080', 'content-language': 'en', 'vary': 'Accept-Encoding,Cookie',
    'server': 'Apache', 'last-modified': 'Wed, 13 Jun 2012 01:33:50 GMT',
    'connection': 'close', 'cache-control': 'private, s-maxage=0, max-age=0,
    must-revalidate', 'date': 'Thu, 14 Jun 2012 12:59:39 GMT', 'content-type':
    'text/html; charset=UTF-8', 'x-cache-lookup': 'HIT from cp1006.eqiad.wmnet:3128,
    MISS from cp1010.eqiad.wmnet:80'}

Natomiast, jeśli chcemy otrzymać nagłówki które wysłaliśmy do serwera, po prostu uzyskujemy dostęp do żądania, a potem do jego nagłówków::

    >>> r.request.headers
    {'Accept-Encoding': 'identity, deflate, compress, gzip',
    'Accept': '*/*', 'User-Agent': 'python-requests/1.2.0'}

Przygotowane żądania
--------------------

Kiedy otrzymasz obiekt :class:`Response <requests.models.Response>`
z wywołania API lub Session, atrybut ``request`` jest tak naprawdę
``PreparedRequest``, które było użyte.  Czasami chciałbyś coś jeszcze
zrobić z body lub nagłówkami (lub czymkolwiek innym) przed wysłaniem
żądania. Prostym przepisem na to jest poniższy kod::

    from requests import Request, Session

    s = Session()
    prepped = Request('GET',  # or any other method, 'POST', 'PUT', etc.
                      url,
                      data=data
                      headers=headers
                      # ...
                      ).prepare()
    # do something with prepped.body
    # do something with prepped.headers
    resp = s.send(prepped,
                  stream=stream,
                  verify=verify,
                  proxies=proxies,
                  cert=cert,
                  timeout=timeout,
                  # etc.
                  )
    print(resp.status_code)

Ponieważ nie robisz nic specjalnego z obiektem ``Request``,
przygotowujesz go natychmiastowo i zmodyfikowałeś obiekt ``PreparedRequest``. Potem możesz wysłać go z innymi parametrami, które przekazałbyś do ``requests.*`` lub ``Sesssion.*``.

Weryfikacja certyfikatów SSL
----------------------------

Requests może weryfikować certyfikaty SSL dla żądań HTTPS, tak jak
przeglądarka. Aby sprawdzić certyfikat SSL hosta, możesz użyć argumentu
``verify``::

    >>> requests.get('https://kennethreitz.com', verify=True)
    requests.exceptions.SSLError: hostname 'kennethreitz.com' doesn't match either of '*.herokuapp.com', 'herokuapp.com'

Nie mam SSL ustawionego na tej domenie, a więc żądanie nie powodzi się.
Świetnie. GitHub natomiast ma certyfikat::

    >>> requests.get('https://github.com', verify=True)
    <Response [200]>

Możesz też przekazać ścieżkę do pliku CA_BUNDLE dla prywatnych certyfikatów parametrowi ``verify``.  Możesz też ustawić zmienną środowiskową ``REQUESTS_CA_BUNDLE``.

Requests może też ignorować weryfikację certyfikatu SSL jeśli ustawisz ``verify`` na False.

::

    >>> requests.get('https://kennethreitz.com', verify=False)
    <Response [200]>

Domyślnie, ``verify`` jest ustawiony na True. Opcja ``verify`` dotyczy tylko certyfikatów hostów.

Możesz też podać lokalny certyfikat do użycia jako certyfikat po stronie
klienta, jako pojedynczy plik (zawierając klucz prywatny i certyfikat)
lub jako krotkę (tuple) zawierającą ścieżki obu plików::

    >>> requests.get('https://kennethreitz.com', cert=('/path/server.crt', '/path/key'))
    <Response [200]>

Jeśli podasz złą ścieżkę lub niewłaściwy certyfikat::

    >>> requests.get('https://kennethreitz.com', cert='/wrong_path/server.pem')
    SSLError: [Errno 336265225] _ssl.c:347: error:140B0009:SSL routines:SSL_CTX_use_PrivateKey_file:PEM lib

Workflow zawartości body
------------------------

Domyślnie, kiedy wykonujesz żądanie, body odpowiedzi jest pobierane od
razu. Możesz nadpisać to działanie i opóźnić pobieranie do czasu, kiedy
wywołasz atrybut :class:`Response.content` przy użyciu parametru
``stream``::

    tarball_url = 'https://github.com/kennethreitz/requests/tarball/master'
    r = requests.get(tarball_url, stream=True)

W tej chwili tylko nagłówki zostały pobrane, a połączenie jest wciąż
otwarte, co pozwala nam na pobieranie zawartości pod pewnymi warunkami::

    if int(r.headers['content-length']) < TOO_LONG:
      content = r.content
      ...

Możesz dalej kontrolować workflow używając metod :class:`Response.iter_content` i :class:`Response.iter_lines`, albo czytając z klasy urllib3 :class:`urllib3.HTTPResponse` w :class:`Response.raw`.


Keep-Alive
----------

Dobre wieści — dzięki urllib3, keep-alive jest w 100% automatyczne w
sesji! Jakiekolwiek żądanie które wykonasz w sesji automatycznie
wykorzysta odpowiednie połączenie!

Zauważ, że połączenia są zwracane do pool do ponownego użycia kiedy wszystkie dane body zostaną przeczytane; upewnij się, że albo ustawiłeś ``stream`` na ``False`` albo przeczytałeś własność ``content`` obiektu ``Response``.


Strumieniowanie Uploadów
------------------------

Requests wspiera strumieniowanie uplaodów, co pozwala na wysyłanie
dużych strumieni lub plików bez wczytywania ich do pamięci.  Aby
strumieniować i uploadować, po prostu podaj obiekt plikopodobny jako
twoje body::

    with open('massive-body') as f:
        requests.post('http://some.url/streamed', data=f)


Żądania Chunk-Encoded
---------------------

Requests wspiera również kodowanie transferu Chunked dla żądań
przychodzących i wychodzących. Aby wysłać żądanie Chunk-encoded, po
prostu podaj generator (albo jakikolwiek iterator bez określonej
długości) jako twoje body::


    def gen():
        yield 'hi'
        yield 'there'

    requests.post('http://some.url/chunked', data=gen())


Hooki zdarzeń
-------------

Requests ma system hooków który możesz użyć do manipulowania częściami procesu żądania lub procesowania sygnałów zdarzeń.

Dostępne hooki:

``response``:
    Odpowiedź wygenerowana z Request.


Możesz przypisać funkcję hooka do każdego żądania osobno przez
przekazanie słownika ``{hook_name: callback_function}`` do parametru
``hooks`` żądania::

    hooks=dict(response=print_url)

Ta ``callback_function`` otrzyma kawałek danych jako swój pierwszy
argument.

::

    def print_url(r):
        print(r.url)

Jeśli nastąpi błąd podczas wykonywania callbacku, nastąpi ostrzeżenie.

Jeśli funkcja callbacku zwraca wartość, przyjmuje się, że ta wartość ma
zastąpić dane podane dla funkcji. Jeśli funkcja nic nie zwraca, nic się
nie dzieje.

Wydrukujmy niektóre argumenty metody żądania podczas działania
(*at runtime*)::

    >>> requests.get('http://httpbin.org', hooks=dict(response=print_url))
    http://httpbin.org
    <Response [200]>


Własne uwierzytelnienie
-----------------------

Requests pozwala na użycie własnego mechanizmu uwierzytelnienia.

Jakiekolwiek callable przekazane jako argument ``auth`` dla metody
żądania będzie miał możliwość zmodyfikowania żądania zanim zostanie
wysłane.

Implementacje uwierzytelnienia są subklasami ``requests.auth.AuthBase``,
i można je bardzo prosto zdefiniować.  Requests oferuje dwa popularne
schematy uwierzytelnienia w ``requests.auth``: ``HTTPBasicAuth`` i
``HTTPDigestAuth``.

Załóżmy że mamy usługę sieciową która odpowie tylko jeśli nagłówek
``X-Pizza`` jest ustawiony na wartość hasła. Mało prawdopodobne, ale po
prostu zignoruj to.

::

    from requests.auth import AuthBase

    class PizzaAuth(AuthBase):
        """Attaches HTTP Pizza Authentication to the given Request object."""
        def __init__(self, username):
            # setup any auth-related data here
            self.username = username

        def __call__(self, r):
            # modify and return the request
            r.headers['X-Pizza'] = self.username
            return r

Później, możemy wykonać żądanie używając naszego Pizza Auth::

    >>> requests.get('http://pizzabin.org/admin', auth=PizzaAuth('kenneth'))
    <Response [200]>

.. _streaming-requests

Żądania Strumieniowe
--------------------

Z ``requests.Response.iter_lines()`` możesz łatwo iterować na
strumieniowych API takich jak `Twitter Streaming API
<https://dev.twitter.com/docs/streaming-api>`_.

Aby użyć Twitter Streaming API do śledzenia słowa kluczowego
„requests”::

    import json
    import requests

    r = requests.get('http://httpbin.org/stream/20', stream=True)

    for line in r.iter_lines():

        # filter out keep-alive new lines
        if line:
            print json.loads(line)


Proxies
-------

Jeśli musisz użyć proxy, możesz skonfigurować indywidualne żądania przy użyciu argumentu ``proxies`` do każdej metody żądania::

    import requests

    proxies = {
      "http": "http://10.10.1.10:3128",
      "https": "http://10.10.1.10:1080",
    }

    requests.get("http://example.org", proxies=proxies)

Możesz też skonfigurować proxy przy użyciu zmiennych środowiskowych
``HTTP_PROXY`` i ``HTTPS_PROXY``.

::

    $ export HTTP_PROXY="http://10.10.1.10:3128"
    $ export HTTPS_PROXY="http://10.10.1.10:1080"
    $ python
    >>> import requests
    >>> requests.get("http://example.org")

Aby użyć HTTP Basic Auth z twoim proxy, użyj składni `http://user:password@host/`::

    proxies = {
        "http": "http://user:pass@10.10.1.10:3128/",
    }

Zgodność
--------

Requests w zamiarze ma być zgodny ze wszystkimi specyfikacjami i RFC
odpowiednimi dla Requests jeśli ta zgodność nie będzie sprawiała
problemów użytkownikom. Ta uwaga na specyfikację może doprowadzić do
niektórych zachowań, które osoby nie znające specyfikacji mogą uznać za
dziwne.

Kodowania
^^^^^^^^^

When you receive a response, Requests makes a guess at the encoding to
use for decoding the response when you call the ``Response.text``
method. Requests will first check for an encoding in the HTTP header,
and if none is present, will use `charade
<http://pypi.python.org/pypi/charade>`_ to attempt to guess the
encoding.

The only time Requests will not do this is if no explicit charset is present
in the HTTP headers **and** the ``Content-Type`` header contains ``text``. In
this situation,
`RFC 2616 <http://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.7.1>`_
specifies that the default charset must be ``ISO-8859-1``. Requests follows
the specification in this case. If you require a different encoding, you can
manually set the ``Response.encoding`` property, or use the raw
``Response.content``.

HTTP Verbs
----------

Requests provides access to almost the full range of HTTP verbs: GET, OPTIONS,
HEAD, POST, PUT, PATCH and DELETE. The following provides detailed examples of
using these various verbs in Requests, using the GitHub API.

We will begin with the verb most commonly used: GET. HTTP GET is an idempotent
method that returns a resource from a given URL. As a result, it is the verb
you ought to use when attempting to retrieve data from a web location. An
example usage would be attempting to get information about a specific commit
from GitHub. Suppose we wanted commit ``a050faf`` on Requests. We would get it
like so::

    >>> import requests
    >>> r = requests.get('https://api.github.com/repos/kennethreitz/requests/git/commits/a050faf084662f3a352dd1a941f2c7c9f886d4ad')

We should confirm that GitHub responded correctly. If it has, we want to work
out what type of content it is. Do this like so::

    >>> if (r.status_code == requests.codes.ok):
    ...     print r.headers['content-type']
    ...
    application/json; charset=utf-8

So, GitHub returns JSON. That's great, we can use the ``r.json`` method to
parse it into Python objects.

::

    >>> commit_data = r.json()
    >>> print commit_data.keys()
    [u'committer', u'author', u'url', u'tree', u'sha', u'parents', u'message']
    >>> print commit_data[u'committer']
    {u'date': u'2012-05-10T11:10:50-07:00', u'email': u'me@kennethreitz.com', u'name': u'Kenneth Reitz'}
    >>> print commit_data[u'message']
    makin' history

So far, so simple. Well, let's investigate the GitHub API a little bit. Now,
we could look at the documentation, but we might have a little more fun if we
use Requests instead. We can take advantage of the Requests OPTIONS verb to
see what kinds of HTTP methods are supported on the url we just used.

::

    >>> verbs = requests.options(r.url)
    >>> verbs.status_code
    500

Uh, what? That's unhelpful! Turns out GitHub, like many API providers, don't
actually implement the OPTIONS method. This is an annoying oversight, but it's
OK, we can just use the boring documentation. If GitHub had correctly
implemented OPTIONS, however, they should return the allowed methods in the
headers, e.g.

::

    >>> verbs = requests.options('http://a-good-website.com/api/cats')
    >>> print verbs.headers['allow']
    GET,HEAD,POST,OPTIONS

Turning to the documentation, we see that the only other method allowed for
commits is POST, which creates a new commit. As we're using the Requests repo,
we should probably avoid making ham-handed POSTS to it. Instead, let's play
with the Issues feature of GitHub.

This documentation was added in response to Issue #482. Given that this issue
already exists, we will use it as an example. Let's start by getting it.

::

    >>> r = requests.get('https://api.github.com/repos/kennethreitz/requests/issues/482')
    >>> r.status_code
    200
    >>> issue = json.loads(r.text)
    >>> print issue[u'title']
    Feature any http verb in docs
    >>> print issue[u'comments']
    3

Cool, we have three comments. Let's take a look at the last of them.

::

    >>> r = requests.get(r.url + u'/comments')
    >>> r.status_code
    200
    >>> comments = r.json()
    >>> print comments[0].keys()
    [u'body', u'url', u'created_at', u'updated_at', u'user', u'id']
    >>> print comments[2][u'body']
    Probably in the "advanced" section

Well, that seems like a silly place. Let's post a comment telling the poster
that he's silly. Who is the poster, anyway?

::

    >>> print comments[2][u'user'][u'login']
    kennethreitz

OK, so let's tell this Kenneth guy that we think this example should go in the
quickstart guide instead. According to the GitHub API doc, the way to do this
is to POST to the thread. Let's do it.

::

    >>> body = json.dumps({u"body": u"Sounds great! I'll get right on it!"})
    >>> url = u"https://api.github.com/repos/kennethreitz/requests/issues/482/comments"
    >>> r = requests.post(url=url, data=body)
    >>> r.status_code
    404

Huh, that's weird. We probably need to authenticate. That'll be a pain, right?
Wrong. Requests makes it easy to use many forms of authentication, including
the very common Basic Auth.

::

    >>> from requests.auth import HTTPBasicAuth
    >>> auth = HTTPBasicAuth('fake@example.com', 'not_a_real_password')
    >>> r = requests.post(url=url, data=body, auth=auth)
    >>> r.status_code
    201
    >>> content = r.json()
    >>> print content[u'body']
    Sounds great! I'll get right on it.

Brilliant. Oh, wait, no! I meant to add that it would take me a while, because
I had to go feed my cat. If only I could edit this comment! Happily, GitHub
allows us to use another HTTP verb, PATCH, to edit this comment. Let's do
that.

::

    >>> print content[u"id"]
    5804413
    >>> body = json.dumps({u"body": u"Sounds great! I'll get right on it once I feed my cat."})
    >>> url = u"https://api.github.com/repos/kennethreitz/requests/issues/comments/5804413"
    >>> r = requests.patch(url=url, data=body, auth=auth)
    >>> r.status_code
    200

Excellent. Now, just to torture this Kenneth guy, I've decided to let him
sweat and not tell him that I'm working on this. That means I want to delete
this comment. GitHub lets us delete comments using the incredibly aptly named
DELETE method. Let's get rid of it.

::

    >>> r = requests.delete(url=url, auth=auth)
    >>> r.status_code
    204
    >>> r.headers['status']
    '204 No Content'

Excellent. All gone. The last thing I want to know is how much of my ratelimit
I've used. Let's find out. GitHub sends that information in the headers, so
rather than download the whole page I'll send a HEAD request to get the
headers.

::

    >>> r = requests.head(url=url, auth=auth)
    >>> print r.headers
    ...
    'x-ratelimit-remaining': '4995'
    'x-ratelimit-limit': '5000'
    ...

Excellent. Time to write a Python program that abuses the GitHub API in all
kinds of exciting ways, 4995 more times.

Link Headers
------------

Many HTTP APIs feature Link headers. They make APIs more self describing and discoverable.

GitHub uses these for `pagination <http://developer.github.com/v3/#pagination>`_ in their API, for example::

    >>> url = 'https://api.github.com/users/kennethreitz/repos?page=1&per_page=10'
    >>> r = requests.head(url=url)
    >>> r.headers['link']
    '<https://api.github.com/users/kennethreitz/repos?page=2&per_page=10>; rel="next", <https://api.github.com/users/kennethreitz/repos?page=6&per_page=10>; rel="last"'

Requests will automatically parse these link headers and make them easily consumable::

    >>> r.links["next"]
    {'url': 'https://api.github.com/users/kennethreitz/repos?page=2&per_page=10', 'rel': 'next'}

    >>> r.links["last"]
    {'url': 'https://api.github.com/users/kennethreitz/repos?page=7&per_page=10', 'rel': 'last'}

Transport Adapters
------------------

As of v1.0.0, Requests has moved to a modular internal design. Part of the
reason this was done was to implement Transport Adapters, originally
`described here`_. Transport Adapters provide a mechanism to define interaction
methods for an HTTP service. In particular, they allow you to apply per-service
configuration.

Requests ships with a single Transport Adapter, the
:class:`HTTPAdapter <requests.adapters.HTTPAdapter>`. This adapter provides the
default Requests interaction with HTTP and HTTPS using the powerful `urllib3`_
library. Whenever a Requests :class:`Session <Session>` is initialized, one of
these is attached to the :class:`Session <Session>` object for HTTP, and one
for HTTPS.

Requests enables users to create and use their own Transport Adapters that
provide specific functionality. Once created, a Transport Adapter can be
mounted to a Session object, along with an indication of which web services
it should apply to.

::

    >>> s = requests.Session()
    >>> s.mount('http://www.github.com', MyAdapter())

The mount call registers a specific instance of a Transport Adapter to a
prefix. Once mounted, any HTTP request made using that session whose URL starts
with the given prefix will use the given Transport Adapter.

Implementing a Transport Adapter is beyond the scope of this documentation, but
a good start would be to subclass the ``requests.adapters.BaseAdapter`` class.

.. _`described here`: http://kennethreitz.org/exposures/the-future-of-python-http
.. _`urllib3`: https://github.com/shazow/urllib3

Blocking Or Non-Blocking?
-------------------------

With the default Transport Adapter in place, Requests does not provide any kind
of non-blocking IO. The ``Response.content`` property will block until the
entire response has been downloaded. If you require more granularity, the
streaming features of the library (see :ref:`streaming-requests`) allow you to
retrieve smaller quantities of the response at a time. However, these calls
will still block.

If you are concerned about the use of blocking IO, there are lots of projects
out there that combine Requests with one of Python's asynchronicity frameworks.
Two excellent examples are `grequests`_ and `requests-futures`_.

.. _`grequests`: https://github.com/kennethreitz/grequests
.. _`requests-futures`: https://github.com/ross/requests-futures
