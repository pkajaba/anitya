Anitya
======

Anitya is a project version monitoring system.

Every-day Anitya checks if there is a new version available and broadcast the
new versions found via a message bus: `fedmsg <http://fedmsg.com/>`_
(`More information on fedmsg for anitya <fedmsg>`_).

Anyone with an OpenID account can register a new application on Anitya. To
do so, all you need is the project name and its home page, the combination
of both must be unique. In order to retrieve the new version, you can specify
a backend for the project hosting. More information below.


Backends
--------

The backend of a project tells Anitya how to retrieve the versions of the
project.

The backends available are:

* ``cpan.py`` for perl projects hosted on `CPAN <http://www.cpan.org/>`_
* ``crates.py`` for crates hosted on `crates.io <https://crates.io/>`_
* ``debian.py`` for projects hosted on the
  `Debian ftp <http://ftp.debian.org/debian/pool/main/>`_
* ``drupal6.py`` for Drupal6 modules hosted on
  `drupal.org <http://drupal.org/project/>`_
* ``drupal7.py`` for Drupal7 modules hosted on
  `drupal.org <http://drupal.org/project/>`_
* ``folder.py`` for projects whose release archives are provided in a folder
  basic apache folder or modified one.
* ``freshmeat.py`` for projects hosted on
  `freshmeat.net <http://freshmeat.net/>`_ / `freecode.com <http://freecode.com/>`_
* ``github.py`` for projects hosted on `github.com <http://github.com/>`_
* ``gnome.py`` for projects hosted on
  `download.gnome.org <https://download.gnome.org/sources/>`_
* ``gnu.py`` for projects hosted on `gnu.org <https://www.gnu.org/software/>`_
* ``google.py`` for projects hosted on
  `code.google.com <https://code.google.com/>`_
* ``hackage.py`` for projects hosted on
  `hackage.haskell.org <http://hackage.haskell.org/>`_
* ``launchpad.py`` for projects hosted on
  `launchpad.net <https://launchpad.net/>`_
* ``npmjs.py`` for projects hosted on `npmjs.org <https://www.npmjs.org/>`_
* ``pear.py`` for projects hosted on
  `pear.php.net <http://pear.php.net/>`_
* ``pecl.py`` for projects hosted on
  `pecl.php.net <http://pecl.php.net/>`_
* ``pypi.py`` for projects hosted on
  `pypi.python.org <https://pypi.python.org/pypi>`_
* ``rubygems.py`` for projects hosted on
  `rubygems.org <http://rubygems.org/>`_
* ``sourceforge.py`` for projects hosted on
  `sourceforge.net <http://sourceforge.net/>`_
* ``stackage.py`` for projects hosted on
  `www.stackage.org <https://www.stackage.org/>`_

If your project cannot be used with any of these backend you can always try
the ``custom`` backend.

* ``custom.py`` for projects who require a more flexible way of finding their
  version.


The custom backend requires two arguments:

* ``version_url`` the url of the page where the versions information can be
  found, for example for `banshee <http://banshee.fm/>`_
  that would be `their download page <http://banshee.fm/download/>`_

* ``regex`` a regular expression to find the releases on the ``version_url``
  page.

.. note:: In most cases, you can set the ``regex`` to `DEFAULT` which will
          make anitya use its default regular expression:

          ::

            <package name>(?:[-_]?(?:minsrc|src|source))?[-_]([^-/_\s]+?)(?i)(?:[-_](?:minsrc|src|source))?\.(?:tar|t[bglx]z|tbz2|zip)


Test your regex
---------------

Sometimes one needs to use a custom regular expression to find the version
on a page.

The simplest way to check your regular expression is to open a python shell
and test it.

Below is an example on how it can be done::

  >>> url = 'http://www.opendx.org/download.html'
  >>>
  >>> import requests
  >>> import re
  >>> text = requests.get(url).text
  >>> re.findall('version.is ([\d]*)\.', text)
  [u'4']
  >>> re.findall('version.is ([\d\.-]*)\.', text)
  [u'4.4.4']

The regular expression ``version.is ([\d\.]*)\.`` can then be provided to
anitya and used to find the new releases.


Version prefix
--------------

The version prefix can be used to retrieve the exact version number when the
upstream maintainer prefixes its versions.

For example, if the project's version are: ``foo-1.2``, you can set the
version prefix to ``foo-`` to tell Anitya how to get the version ``1.2``.

More concrete examples:
  * `junit <https://github.com/junit-team/junit/tags>`_ tags are of the form:
    ``r<version>``, to retrieve the version, one can set the version prefix
    to ``r``.
  * `carbon-c-relay <https://github.com/grobian/carbon-c-relay/tags>`_ tags
    are of the form ``v<version>``, for this project, the version prefix
    can be set to ``v``.
  * `fdupes <https://github.com/adrianlopezroche/fdupes/tags>`_ tags are of
    the form ``fdupes-<version>``, for this project, the version prefix can
    be set to ``fdupes-``.


Dénouement
----------

You can report `issues
<https://github.com/fedora-infra/anitya/issues>`_ and find the
`sources <https://github.com/fedora-infra/anitya/>`_ on github.
The development team hangs out in ``#fedora-apps`` on the freenode network.
Please do stop by and say hello.
