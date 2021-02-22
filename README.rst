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

* GNU coreutils (for ``readlink`` and ``realpath``)

* GNU findutils (for ``find``)


Installation
------------

.. code-block:: bash

   $ git clone https://github.com/pyenv/pyenv-users.git "$(pyenv root)/plugins/pyenv-users"


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

For scripting, use the ``--raw`` option to output a list of ``:`` separated
items:

.. code-block:: bash

   $ pyenv users --raw ~
   3.7.9:/home/peter/.cache/pypoetry/virtualenvs/my_project-KM_3YcvM-py3.7
   3.7.9:/home/peter/work/venvs/long name with spaces
   3.8.6:/home/peter/.cache/pypoetry/virtualenvs/my_project-KM_3YcvM-py3.8
   pypy3.6-7.3.1:/home/peter/work/venvs/example1

For example, to get a list of all versions linked to a virtual environment:

.. code-block:: bash

   $ pyenv users --raw ~ | cut -d: -f1 | uniq
   3.7.9
   3.8.6
   pypy3.6-7.3.1

The list of linked versions can be combined with the list of all installed
versions to show which installations are not linked to any environment in the
specified directory:

.. code-block:: bash

   $ pyenv versions --bare
   3.6.10
   3.7.5
   3.7.9
   3.8.6
   3.9.1
   pypy3.6-7.3.0
   pypy3.6-7.3.1

   $ comm -3 <(pyenv users --raw ~ | cut -d: -f1 | uniq) <(pyenv versions --bare) | tr -d "[:blank:]"
   3.6.10
   3.7.5
   3.9.1
   pypy3.6-7.3.0


Disclaimer
----------

The plugin doesn't maintain a list of environments; it generates the list each
run by performing a brute force scan of the directory for symlinks named
`python`. This does mean it's not blazingly fast, but its simplicity provides
state-free reliability, and in practice ``find`` only takes a few seconds.

Also, it could really use more testing; so far I've been using it on Fedora 33
with no issues. Tested with ``bash v5.0.17(1)`` and ``find v4.7.0``.
