.. _install:

Instalacja
==========

Ta część dokumentacji pokrywa instalację Requests.
Pierwszym krokiem używania jakiegokolwiek pakietu oprogramowania jest
prawidłowa instalacja.


Distribute & Pip
----------------

Instalowanie requests jest proste z `pip <http://www.pip-installer.org/>`_::

    $ pip install requests

albo z `easy_install <http://pypi.python.org/pypi/setuptools>`_::

    $ easy_install requests

Ale naprawdę `nie powinieneś tego robić <http://www.pip-installer.org/en/latest/other-tools.html#pip-compared-to-easy-install>`_.



Mirror dla Cheeseshop (PyPI)
----------------------------

Jeśli Cheeseshop (znany też jako PyPI) nie działa, możesz również zainstalować
Requests z jednego z mirrorów. `Crate.io <http://crate.io>`_ jest jednym z
nich::

    $ pip install -i http://simple.crate.io/ requests


Zdobądź kod
-----------

Requests jest aktywnie rozwijany na GitHubie, gdzie kod jest
`zawsze dostępny <https://github.com/kennethreitz/requests>`_.

Możesz sklonować publiczne repozytorium::

    git clone git://github.com/kennethreitz/requests.git

Pobrać `tarball <https://github.com/kennethreitz/requests/tarball/master>`_::

    $ curl -OL https://github.com/kennethreitz/requests/tarball/master

Lub pobrać `zipball <https://github.com/kennethreitz/requests/zipball/master>`_::

    $ curl -OL https://github.com/kennethreitz/requests/zipball/master


Kiedy już masz kopię źródeł, możesz wbudować je w swój pakiet lub łatwo
zainstalować je do site-packages::

    $ python setup.py install
