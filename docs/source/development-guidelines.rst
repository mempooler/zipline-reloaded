Development Guidelines
======================
This page is intended for developers of Zipline, people who want to contribute to the Zipline codebase or documentation, or people who want to install from source and make local changes to their copy of Zipline.

All contributions, bug reports, bug fixes, documentation improvements, enhancements and ideas are welcome. We `track issues`__ on `GitHub`__ and also have a `mailing list`__ where you can ask questions.

__ https://github.com/stefan-jansen/zipline-reloaded/issues
__ https://github.com/
__ https://exchange.ml4trading.io/

Creating a Development Environment
----------------------------------

First, you'll need to clone Zipline by running:

.. code-block:: bash

   $ git clone git@github.com:stefan-jansen/zipline-reloaded.git

Then check out to a new branch where you can make your changes:

.. code-block:: bash
   $ cd zipline-reloaded
   $ git checkout -b some-short-descriptive-name

If you don't already have them, you'll need some C library dependencies. You can follow the `install guide`__ to get the appropriate dependencies.

__ install.rst

Once you've created and activated a `virtual environment`__

__ https://docs.python.org/3/library/venv.html

.. code-block:: bash

   $ python3 -m venv venv
   $ source venv/bin/activate

Or, using `virtualenvwrapper`__:

__ https://virtualenvwrapper.readthedocs.io/en/latest/

.. code-block:: bash

   $ mkvirtualenv zipline

run the ``pip install -e .[test]`` to install :

After installation, you should be able to use the ``zipline`` command line interface from your virtualenv:

.. code-block:: bash

   $ zipline --help

To finish, make sure `tests`__ pass.

__ #style-guide-running-tests

During development, you can rebuild the C extensions by running:

.. code-block:: bash

   $ ./rebuid-cython.sh


Style Guide & Running Tests
---------------------------

We use `flake8`__ for checking style requirements, `black`__ for code formatting and `pytest`__ to run Zipline tests. Our `continuous integration`__ tools will run these commands.

__ https://flake8.pycqa.org/en/latest/
__ https://black.readthedocs.io/en/stable/
__ https://docs.pytest.org/en/latest/
__ https://en.wikipedia.org/wiki/Continuous_integration

Before submitting patches or pull requests, please ensure that your changes pass when running:

.. code-block:: bash

   $ flake8 src/zipline tests

In order to run tests locally, you'll need `TA-lib`__, which you can install on Linux by running:

__ https://mrjbq7.github.io/ta-lib/install.html

.. code-block:: bash

   $ wget http://prdownloads.sourceforge.net/ta-lib/ta-lib-0.4.0-src.tar.gz
   $ tar -xvzf ta-lib-0.4.0-src.tar.gz
   $ cd ta-lib/
   $ ./configure --prefix=/usr
   $ make
   $ sudo make install

And for ``TA-lib`` on OS X you can just run:

.. code-block:: bash

   $ brew install ta-lib

Then run ``pip install`` TA-lib:

You should now be free to run tests:

.. code-block:: bash

   $ pytest tests


Continuous Integration
----------------------
[TODO]

Packaging
---------

[TODO]


Contributing to the Docs
------------------------

If you'd like to contribute to the documentation on zipline.io, you can navigate to ``docs/source/`` where each `reStructuredText`__ (``.rst``) file is a separate section there. To add a section, create a new file called ``some-descriptive-name.rst`` and add ``some-descriptive-name`` to ``appendix.rst``. To edit a section, simply open up one of the existing files, make your changes, and save them.

__ https://en.wikipedia.org/wiki/ReStructuredText

We use `Sphinx`__ to generate documentation for Zipline, which you will need to install by running:

__ https://www.sphinx-doc.org/en/master/


If you would like to use Anaconda, please follow :ref:`the installation guide<managing-conda-environments>` to create and activate an environment, and then run the command above.

To build and view the docs locally, run:

.. code-block:: bash

   # assuming you're in the Zipline root directory
   $ cd docs
   $ make html
   $ {BROWSER} build/html/index.html


Commit messages
---------------

Standard prefixes to start a commit message:

.. code-block:: text

   BLD: change related to building Zipline
   BUG: bug fix
   DEP: deprecate something, or remove a deprecated object
   DEV: development tool or utility
   DOC: documentation
   ENH: enhancement
   MAINT: maintenance commit (refactoring, typos, etc)
   REV: revert an earlier commit
   STY: style fix (whitespace, PEP8, flake8, etc)
   TST: addition or modification of tests
   REL: related to releasing Zipline
   PERF: performance enhancements


Some commit style guidelines:

Commit lines should be no longer than `72 characters`__. The first line of the commit should include one of the above prefixes. There should be an empty line between the commit subject and the body of the commit. In general, the message should be in the imperative tense. Best practice is to include not only what the change is, but why the change was made.

__ https://git-scm.com/book/en/v2/Distributed-Git-Contributing-to-a-Project

**Example:**

.. code-block:: text

   MAINT: Remove unused calculations of max_leverage, et al.

   In the performance period the max_leverage, max_capital_used,
   cumulative_capital_used were calculated but not used.

   At least one of those calculations, max_leverage, was causing a
   divide by zero error.

   Instead of papering over that error, the entire calculation was
   a bit suspect so removing, with possibility of adding it back in
   later with handling the case (or raising appropriate errors) when
   the algorithm has little cash on hand.


Formatting Docstrings
---------------------

When adding or editing docstrings for classes, functions, etc, we use `numpy`__ as the canonical reference.

__ https://github.com/numpy/numpy/blob/master/doc/HOWTO_DOCUMENT.rst.txt


Updating the Whatsnew
---------------------

We have a set of `whatsnew <https://github.com/stefan-jansen/zipline-reloaded/tree/main/docs/source/whatsnew>`__ files that are used for documenting changes that have occurred between different versions of Zipline.
Once you've made a change to Zipline, in your Pull Request, please update the most recent ``whatsnew`` file with a comment about what you changed. You can find examples in previous ``whatsnew`` files.
