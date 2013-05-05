Filozofia rozwoju
=================

Requests jest otwartą ale zadufaną w sobie biblioteką, tworzoną przez otwartego ale zadufanego w sobie developera.


Dobrotliwy Dyktator (BDFL)
~~~~~~~~~~~~~~~~~~~~~~~~~~

`Kenneth Reitz <http://kennethreitz.org>`_ jest BDFL-em. Ma ostateczny głos w każdej decyzji dotyczącej Requests.

Walory
~~~~~~

- Prostota jest zawsze lepsza niż funkcjonalność.
- Wysłuchaj wszystkich, a potem to zignoruj.
- Tylko API jest ważne. Wszystko inne jest drugorzędne.
- Mieść się w use-case 90%. Zignoruj krytykantów.

Semantic Versioning
~~~~~~~~~~~~~~~~~~~

Przez wiele lat społeczność open-source była nękana przez dystonię numerów wersji. Numery różnią się tak bardzo między projektami, że są praktycznie bez znaczenia.

Requests używa `Semantic Versioning <http://semver.org>`_. Specyfikacja próbuje skończyć to szaleństwo przy użyciu małego zestawu praktycznych zasad, które ty i twoi koledzy powinniście zastosować w następnym projekcie.

Biblioteka Standardowa?
~~~~~~~~~~~~~~~~~~~~~~~

Requests nie ma *aktywnych* planów bycia dołączonym w bibliotece standardowej. Było to przedyskutowane z Guido i wieloma developerami.

Istotnie, biblioteka standardowa to miejsce gdzie biblioteka przychodzi umrzeć. Jest dobra dla modułu którego ciągły rozwój nie jest potrzebny.

Requests ostatnio osiągnęło v1.0.0. Ten wielki kamień milowy oznacza duży krok w dobrą stronę.

Pakiety dystrybucji Linuksa
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Istnieją dystrybucje dla wielu repozytoriów Linuksowych, w tym: Ubuntu, Debian, RHEL i Arch.

Te dystrybucje są czasami rozbieżnymi forkami, albo są w inny sposób nieaktualne w stosunku do ostatniego kodu i poprawek. PyPI (i jego mirrory) oraz GitHub są oficjalnymi źródłami dystrybucji; alternatywy nie są wspierane przez projekt Requests.
