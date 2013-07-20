.. _authentication:

Uwierzytelnienie
================

Ten dokument opisuje różne formy uwierzytelniania z Requests.

Wiele usług sieciowych wymaga uwierzytelnienia, istnieje też wiele różnych
typów. Poniżej opisujemy różne formy uwierzytelniania dostępne w Requests, od
tych prostszych do tych bardziej kompleksowych.


Basic Authentication
--------------------

Wiele usług sieciowych wymagających uwierzytelnienia akceptuje HTTP Basic Auth. Jest to najprostszy rodzaj uwierzytelnienia, i Requests wspiera go od razu *po wyjęciu z pudełka* (out of the box).

Wykonywanie żądań z HTTP Basic Auth jest bardzo proste::

    >>> from requests.auth import HTTPBasicAuth
    >>> requests.get('https://api.github.com/user', auth=HTTPBasicAuth('user', 'pass'))
    <Response [200]>

W rzeczywistości HTTP Basic Auth jest tak pospolity, że Requests oferuje skrót do używania go::

    >>> requests.get('https://api.github.com/user', auth=('user', 'pass'))
    <Response [200]>

Podawanie danych logowania w krotce (``tuple``) w ten sposób jest identyczne z
przykładem ``HTTPBasicAuth`` wyżej.


Uwierzytelnienie netrc
~~~~~~~~~~~~~~~~~~~~~~

Jeśli metoda uwierzytelnienia nie jest podana z argumentem ``auth``, Requests spróbuje zdobyć dane do logowania dla nazwy hosta URL-a z pliku netrc użytkownika.

Jeśli dane do logowania zostaną znalezione, żądanie jest wysyłane z HTTP Basic
Auth.


Digest Authentication
---------------------

Inną popularną formą uwierzytelnienia HTTP jest Digest Authentication,
a Requests wspiera ją również *po wyjęciu z pudełka*::

    >>> from requests.auth import HTTPDigestAuth
    >>> url = 'http://httpbin.org/digest-auth/auth/user/pass'
    >>> requests.get(url, auth=HTTPDigestAuth('user', 'pass'))
    <Response [200]>


Uwierzytelnienie OAuth 1
------------------------

Popularną formą uwierzytelnienia w niektórych API sieciowych jest OAuth.
Biblioteka ``requests-oauthlib`` pozwala użytkownikom Requests prosto wykonywać
uwierzytelnione żądania OAuth::

    >>> import requests
    >>> from requests_oauthlib import OAuth1

    >>> url = 'https://api.twitter.com/1.1/account/verify_credentials.json'
    >>> auth = OAuth1('YOUR_APP_KEY', 'YOUR_APP_SECRET',
                      'USER_OAUTH_TOKEN', 'USER_OAUTH_TOKEN_SECRET')

    >>> requests.get(url, auth=auth)
    <Response [200]>

Aby uzyskać więcej informacji o tym, jak działa OAuth, zobacz oficjalną stronę `OAuth`_.
Aby uzyskać przykłady i dokumentację do requests-oauthlib, zobacz repozytorium
`requests_oauthlib`_ na GitHubie.


Inne uwierzytelnienie
---------------------

Requests jest stworzony aby umożliwić łatwe i szybkie dodawanie innych form
uwierzytelnienia. Członkowie społeczności open-source często tworzą dodatki do
obsługi innych, bardziej skomplikowanych lub rzadziej
używanych metod uwierzytelnienia. Najlepsze są częścią `organizacji Requests`_, w tym:

- Kerberos_
- NTLM_

Jeśli chcesz używać którejś z tych form uwierzytelnienia, idź do ich strony na
GitHubie i podążaj za instrukcjami.

Nowe formy uwierzytelnienia
---------------------------

Jeśli nie możesz znaleźć dobrej implementacji formy uwierzytelnienia którą
chcesz użyć, możesz sam ją zaimplementować. Requests ułatwia dodawanie własnych
form uwierzytelnienia.

Aby to zrobić, stwórz subklasę :class:`requests.auth.AuthBase` i zaimplementuj metodę
``__call__()``. Kiedy handler uwierzytelnienia jest dołączony do żądania, jest on wywoływany (*call*) podczas przygotowywania żądania.  Metoda ``__call__`` musi więc zrobić wszystko co trzeba, aby uwierzytelnienie działało. Niektóre formy uwierzytelnienia dodają hooki do oferowania dodatkowej funkcjonalności.

Przykłady można znaleźć w `organizacji Requests`_ i w pliku ``auth.py``.

.. _OAuth: http://oauth.net/
.. _requests_oauthlib: https://github.com/requests/requests-oauthlib
.. _Kerberos: https://github.com/requests/requests-kerberos
.. _NTLM: https://github.com/requests/requests-ntlm
.. _organizacji Requests: https://github.com/requests

