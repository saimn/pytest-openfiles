================
pytest-openfiles
================

This package provides a plugin for the `pytest`_ framework that allows
developers to detect whether any file handles or other file-like objects were
inadvertently left open at the end of a unit test. It has been moved from the
core `astropy`_ project since it is of use more generally.

.. _pytest: https://pytest.org/en/latest/
.. _astropy: https://astropy.org/en/latest/

Motivation
----------

The `pytest-openfiles`_ plugin allows for the detection of open I/O resources
at the end of unit tests.  This is particularly useful for testing code that
manipulates file handles or other I/O resources. It allows developers to ensure
that this kind of code properly cleans up I/O resources when they are no longer
needed.

Installation
------------

The ``pytest-openfiles`` plugin can be installed using ``pip``::

    $ pip install pytest-openfiles

It is also possible to install the latest development version from the source
repository::

    $ git clone https://github.com/astropy/pytest-openfiles
    $ cd pytest-openfiles
    $ python ./setup.py install

In either case, the plugin will automatically be registered for use with
``pytest``.

Usage
-----

This plugin adds the ``--open-files`` option to the ``pytest`` command.  When
running tests with ``--open-files``, if a file is opened during the course of a
unit test but that file is not closed before the test finishes, the test will
fail.

In some cases certain files are expected to remain open between tests. In order
to prevent these files from causing tests to fail, they can be ignored using
the configuration file variable ``open_files_ignore``. This variable is added
to the ``[tool:pytest]`` section of a package's top-level ``setup.cfg`` file.

Files can be ignored using a fully specified filename::

    [tool:pytest]
    open_files_ignore = "/home/user/monty/output.log"

It is also possible to ignore files with a particular name regardless of where
they reside in the file system::

    [tool:pytest]
    open_files_ignore = "output.log"

In this example, all files named ``output.log`` will be ignored if they are
found to remain open after a test completes. Paths and filenames can include
``*`` and ``?`` as wildcards::

    [tool:pytest]
    open_files_ignore = "*.ttf"

It is also possible to ignore open files for particular test cases by
decorating them with the ``openfiles_ignore`` decorator:

.. code::

    import pytest

    @pytest.mark.openfiles_ignore
    def test_open_file_and_ignore():
        """We want to ignore this test when checking for open file handles."""


The test function will not be skipped, but any files that are left open by the
test will be ignored by this plugin.


Development Status
------------------

.. image:: https://travis-ci.com/astropy/pytest-openfile.svg
    :target: https://travis-ci.com/astropy/pytest-openfiles
    :alt: Travis CI Status

Questions, bug reports, and feature requests can be submitted on `github`_.

.. _github: https://github.com/astropy/pytest-openfiles

License
-------
This plugin is licensed under a 3-clause BSD style license - see the
``LICENSE.rst`` file.
