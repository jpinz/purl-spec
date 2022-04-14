User URL Type definitions
============================

Each website, service, platform, type, or ecosystem has its own conventions and
protocols to identify, locate, and provision user accounts.

The user **type** is the component of a user URL that is used to capture
this information with a short string such as ``facebook``, ``twitter``, ``github``, ``linkedin``,
``npm``, etc.


These are known ``uurl`` user type definitions.

Known ``uurl`` type definitions are formalized here independent of the core
User URL specification. See also a candidate list further down.

Definitions can also include types reserved for future use.

See also https://github.com/package-url/purl-spec and
<UURL-SPECIFICATION.rst>`_ for the User URL specification.


Known ``uurl`` types
~~~~~~~~~~~~~~~~~~~~

bitbucket
---------
``bitbucket`` for Bitbucket-based users:

- The default site is ``https://bitbucket.org``
- The ``name`` is the user name. It is not case sensitive and must be
  lowercased.
- Examples::

      usr:bitbucket/atlassian

docker
------
``docker`` for Docker users

- The default site is ``https://hub.docker.com``
- Examples::

      usr:docker/jpinz
      usr:docker/grafana

facebook
------
``facebook`` for Facebook users

- The default site is ``https://facebook.com``
- Examples::

      usr:facebook/zuck?fullname=Mark%20Zuckerberg

github
------
``github`` for Github-based users:

- The default repository is ``https://github.com``
- Examples::

      usr:github/octocat
      usr:github/jpinz?fullname=Julian%20Pinzer


npm
---
``npm`` for Node NPM users:

- The default site is ``https://npmjs.com``
- Examples::

      usr:npm/yyx990803
      usr:npm/jpinzer
      usr:npm/microsoft1es

nuget
-----
``nuget`` for NuGet .NET users:

- The default repository is ``https://www.nuget.org``
- Examples::

      usr:nuget/jamesnk
      usr:nuget/newtonsoft
      usr:nuget/kzu

pypi
----
``pypi`` for Python users:

- The default repository is ``https://pypi.org``
- Examples::

      usr:pypi/microsoft
      usr:pypi/jupinzer

twitter
----
``twitter`` for Twitter users:

- The default site is ``https://twitter.com``
- Examples::

      usr:twitter/jpinzer?location=Boston,%20MA&site=jpinzer.me
      usr:twitter/nasa


Other candidate types to define:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``conda`` for anaconda users:
- ``gem`` for rubygems users:
- ``gitlab`` for Gitlab-based users:
- ``golang`` for Go users:
- ``gravatar`` for gravatar users:
- ``maven`` for maven users:
- ``pub`` for Dart users:
- ``sourceforge`` for Sourceforge-based users:
- ``wordpress`` for Wordpress users:


License
~~~~~~~

This document is licensed under the MIT license
