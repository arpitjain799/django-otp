.. vim: tw=80 lbr

django-otp
==========

.. image:: https://img.shields.io/pypi/v/django-otp?color=blue
   :target: https://pypi.org/project/django-otp/
   :alt: PyPI
.. image:: https://img.shields.io/readthedocs/django-otp-official
   :target: https://django-otp-official.readthedocs.io/
   :alt: Documentation
.. image:: https://img.shields.io/badge/github-django--otp-green
   :target: https://github.com/django-otp/django-otp
   :alt: Source

This project makes it easy to add support for `one-time passwords
<http://en.wikipedia.org/wiki/One-time_password>`_ (OTPs) to Django. It can be
integrated at various levels, depending on how much customization is required.
It integrates with ``django.contrib.auth``, although it is not a Django
authentication backend. The primary target is developers wishing to incorporate
OTPs into their Django projects as a form of `two-factor authentication
<http://en.wikipedia.org/wiki/Two-factor_authentication>`_.

Several simple OTP plugins are included and more are available separately. This
package also includes an implementation of OATH `HOTP
<http://tools.ietf.org/html/rfc4226>`_ and `TOTP
<http://tools.ietf.org/html/rfc6238>`_ for convenience, as these are standard
OTP algorithms used by multiple plugins.

If you're looking for a higher-level or more opinionated solution, you might be
interested in `django-two-factor-auth
<https://github.com/Bouke/django-two-factor-auth>`_.

Status
------

This project is stable and maintained, but is no longer actively used by the
author and is not seeing much ongoing investment.

Well-formed issues and pull requests are welcome, but please see the
Contributing section of the README first.

.. end-of-doc-intro


Development
-----------

This project is built and managed with `hatch`_. If you don't have hatch, I
recommend installing it with `pipx`_: ``pipx install hatch``.

``pyproject.toml`` defines several useful scripts for development and testing.
The default environment includes all dev and test dependencies for quickly
running tests. The ``test`` environment defines the test matrix for running the
full validation suite. Everything is executed in the context of the Django
project in test/test\_project.

As a quick primer, hatch scripts can be run with ``hatch run [<env>:]<script>``.
To run linters and tests in the default environment, just run
``hatch run check``. This should run tests with your default Python version and
the latest Django. Other scripts include:

* **manage**: Run a management command via the test project. This can be used to
  generate migrations.
* **lint**: Run all linters.
* **test**: Run all tests.
* **check**: Run linters and tests.
* **warn**: Run tests with all warnings enabled. This is especially useful for
  seeing deprecation warnings in new versions of Django.
* **cov**: Run tests and print a code coverage report.

To run the full test matrix, run ``hatch run test:run``. You will need multiple
specific Python versions installed for this.

By default, the test project uses SQLite. Because SQLite doesn't support row
locking, some concurrency tests will be skipped. To test against PostgreSQL in a
wide-open local install (username postgres, no password), run
``hatch run postgres:test``.

You can clean up the hatch environments with ``hatch env prune``, for example to
force dependency updates.


Contributing
------------

As mentioned above, this project is stable and mature. Issues and pull requests
are welcome for important bugs and improvements. For non-trivial changes, it's
often a good idea to start by opening an issue to track the need for a change
and then optionally open a pull request with a proposed resolution. Issues and
pull requests should also be focused on a single thing. Pull requests that
bundle together a bunch of loosely related commits are unlikely to go anywhere.

Another good rule of thumb—for any project, but especially a mature one—is to
keep changes as simple as possible. In particular, there should be a high bar
for adding new dependencies. Although it can't be ruled out, it seems highly
unlikely that a new runtime dependency will ever be added. New testing
dependencies are more likely, but only if there's no other way to address an
important need.

If there's a development tool that you'd like to use with this project, the
first step is to try to update config files (setup.cfg or similar) to integrate
the tool with the existing code. A bit of configuration glue for popular tools
should always be safe. If that's not possible, we can consider modifying the
code to be compatible with a broader range of tools (without breaking any
existing compatibilities). Only as a last resort would a new testing or
development tool be incorporated into the project as a dependency.

It's also good to remember that writing the code is typically the least part of
the work. This is true for software development in general, but especially a
small stable project like this. The bulk of the work is in `understanding the
problem <http://www.youtube.com/watch?v=f84n5oFoZBc>`_, determining the desired
attributes of a solution, researching and evaluating alternatives, writing
documentation, designing a testing strategy, etc. Writing the code itself tends
to be a minor matter that emerges from that process.


The Future
----------

Once upon a time, everything was usernames and passwords. Or even in the case of
other authentication mechanisms, a user was either authenticated or not
(anonymous in Django's terminology). Then there was two-factor authentication,
which could simply be an implementation detail in a binary authentication state,
but could also imply levels or degrees of authentication.

These days, it's increasingly common to see sites with more nuanced
authentication state. A site might remember who you are forever—so you're not
anonymous—but if you try to do anything private, you have to re-authenticate.
You may be able to choose from among all of the authentication mechanisms you
have configured, or only from some of them. Specific mechanisms may be required
for specific actions, such as using your U2F device to access your U2F settings.

In short, the world seems to be moving beyond the assumptions that originally
informed Django's essential authentication design. If I were still investing in
Django generally, I would probably start a new multi-factor authentication
project that would reflect these changes. It would incorporate the idea that a
user may be authenticated by various combinations of mechanisms at any time and
that different combinations may be required to satisfy diverse authorization
requirements across the site. It would most likely try to disentangle
authentication persistence from sessions, at least to some extent. Many sites
would not require all of this flexibility, but it would open up possibilities
for better experiences by not asking users for more than we require at any
point.

If anyone has a mind to take on a project like this, I'd be happy to offer
whatever advice or lessons learned that I can.


.. _hatch: https://hatch.pypa.io/
.. _pipx: https://pypa.github.io/pipx/
