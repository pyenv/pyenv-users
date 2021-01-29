pyenv-users
===========

Search a directory for virtual environments that use pyenv-managed versions of
Python.

I find it helpful for verifying that I can safely uninstall old Python
versions. Unlike the `pyenv-virtualenv
<https://github.com/pyenv/pyenv-virtualenv>`_ plugin, this will list all
virtual environments, regardless of how they were created (``python -m venv
<venv>``, ``poetry install``, etc).


Prerequisites
-------------

Requires ``find`` from GNU ``findutils`` to collect a list of candidates.

Uses ``column`` from the ``util-linux`` package (if available) for
pretty-printing the output. If ``column`` is not available, the output is the
simple form ``<version>:<venv path>``.


Installation
------------

Verify that ``$PYENV_ROOT`` has been configured:

.. code-block:: bash

   $ echo $PYENV_ROOT

Install the plugin:

.. code-block:: bash

   $ git clone https://github.com/pyenv/pyenv-users.git "$PYENV_ROOT"/plugins/pyenv-users


Usage
-----

Run ``pyenv users`` to search the current directory for virtual environments,
or ``pyenv users <directory>`` to search a specific directory. For example, to
search your home directory:

.. code-block:: bash

   $ pyenv users ~
   3.7.9          /home/peter/.cache/pypoetry/virtualenvs/my_project-KM_3YcvM-py3.7
   3.7.9          /home/peter/work/venvs/long name with spaces
   3.8.6          /home/peter/.cache/pypoetry/virtualenvs/my_project-KM_3YcvM-py3.8
   pypy3.6-7.3.1  /home/peter/work/venvs/example1


Disclaimer
----------

I'm not a script writer so it's probably a bit crude. It's not blazingly fast,
since it uses a brute force scan, but on my system it only takes 3 seconds,
and I like the simplicity.

I've been using it on Fedora 33 with no issues, but it could use more testing.
In particular, users on MacOS and Windows will probably prefer the addition of
a pretty-printing solution that doesn't rely on ``column``.

Also, I seem to recall that not all versions of ``column`` support the ``-s``
parameter for setting the separator. I haven't had time to research that.

Tested with ``bash v5.0.17(1)``, ``find v4.7.0``, and ``column v2.36.1`` on
Fedora 33.
