.. _api:

.. TODO 152

Interfejs dewelopera
====================

.. module:: requests

Ta część dokumentacji obejmuje wszystkie interfejsy Requests.  Tam, gdzie
Requests zależy od zewnętrznych bibliotek, dokumentujemy najważniejsze tutaj i
oferujemy linki do oryginalnej dokumentacji.

Główny interfejs
----------------

Cała funkcjonalność Requests jest dostępna w poniższych 7 metodach.  Wszystkie
zwracają instancję obiektu :class:`Response <Response>`.

.. autofunction:: request

.. autofunction:: head
.. autofunction:: get
.. autofunction:: post
.. autofunction:: put
.. autofunction:: patch
.. autofunction:: delete


Klasy niższego poziomu
~~~~~~~~~~~~~~~~~~~~~~

.. autoclass:: requests.Request
   :inherited-members:

.. autoclass:: Response
   :inherited-members:

Sesje żądań
-----------

.. autoclass:: Session
   :inherited-members:

.. autoclass:: requests.adapters.HTTPAdapter
   :inherited-members:


Wyjątki
~~~~~~~

.. module:: requests

.. autoexception:: RequestException
.. autoexception:: ConnectionError
.. autoexception:: HTTPError
.. autoexception:: URLRequired
.. autoexception:: TooManyRedirects


Sprawdzanie kodów odpowiedzi
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. autofunction:: requests.codes

::

    >>> requests.codes['temporary_redirect']
    307

    >>> requests.codes.teapot
    418

    >>> requests.codes['\o/']
    200

Ciasteczka (cookies)
~~~~~~~~~~~~~~~~~~~~

.. autofunction:: dict_from_cookiejar
.. autofunction:: cookiejar_from_dict
.. autofunction:: add_dict_to_cookiejar


Kodowania
~~~~~~~~~

.. autofunction:: get_encodings_from_content
.. autofunction:: get_encoding_from_headers
.. autofunction:: get_unicode_from_response
.. autofunction:: decode_gzip


Klasy
~~~~~

.. autoclass:: requests.Response
   :inherited-members:

.. autoclass:: requests.Request
   :inherited-members:

.. autoclass:: requests.PreparedRequest
   :inherited-members:

.. _sessionapi:

.. autoclass:: requests.Session
   :inherited-members:

.. autoclass:: requests.adapters.HTTPAdapter
   :inherited-members:


Migracja do wersji 1.x
----------------------

Ta sekcja opisuje najważniejsze różnice pomiędzy 0.x a 1.x i ma na celu
ułatwienie aktualizacji.


Zmiany w API
~~~~~~~~~~~~

* ``Response.json`` jest teraz wywoływalne (callable), a nie właściwością (property) odpowiedzi.

  ::

      import requests
      r = requests.get('https://github.com/timeline.json')
      r.json()   # *wywołanie* podnosi wyjątek jeśli dekodowanie JSON nie uda się

* API dla ``Session`` zmieniło się.  Obiekty sesji nie przyjmują już
  parametrów.  ``Session`` jest teraz pisane wielką literą, ale wciąż można
  używać ``session`` dla kompatybilności wstecznej.

  ::

      s = requests.Session()    # wcześniej sesja przyjmowała parametry
      s.auth = auth
      s.headers.update(headers)
      r = s.get('http://httpbin.org/headers')

* Wszystkie request hooki zostały usunięte, za wyjątkiem ``response``.

* Pomocnicy uwierzytelniania są teraz w oddzielnych modułach.  Patrz
  requests-oauthlib_ i requests-kerberos_.

.. _requests-oauthlib: https://github.com/requests/requests-oauthlib
.. _requests-kerberos: https://github.com/requests/requests-kerberos

* Parametr dla żądań streamingowych został zmieniony z ``prefetch`` na
  ``stream`` i logika została odwróćona. Dodatkowo, ``stream`` jest teraz
  wymagany do odczytu surowej odpowiedzi.

  ::

      # w 0.x, prefetch=False miałoby taki sam efekt
      r = requests.get('https://github.com/timeline.json', stream=True)
      r.raw.read(10)

* Parametr ``config`` metody requests został usunięty. Niektóre opcje są teraz konfigurowalne w ``Session``, np. keep-alive i maksymalna ilość przekierowań. Opcja gadatliwości powinna być skonfigurowana przez logging.

  ::

      import requests
      import logging

      # te dwie linie włączają debugowanie na poziomie httlib (requests->urllib3->httplib)
      # zobaczysz REQUEST, wraz z HEADERS i DATA, oraz RESPONSE z HEADERS i bez DATA.
      # jedyną brakującą rzeczą będzie response.body, które nie jest logowane.
      import httplib
      httplib.HTTPConnection.debuglevel = 1
      
      logging.basicConfig() # musisz zainicjować logging, w przeciwnym wypadku nie zobaczysz nic z requests
      logging.getLogger().setLevel(logging.DEBUG)
      requests_log = logging.getLogger("requests.packages.urllib3")
      requests_log.setLevel(logging.DEBUG)
      requests_log.propagate = True
      
      requests.get('http://httpbin.org/headers')



Licencja
~~~~~~~~

Jedną z kluczowych zmian która nie dotyczy API jest zmiana licencji z licencji
ISC_ na licencję `Apache 2.0`_. Licensja Apache 2.0 zapewnia licencjonowanie
kontrybucji na tejże licencji.

.. _ISC: http://opensource.org/licenses/ISC
.. _Apache 2.0: http://opensource.org/licenses/Apache-2.0

