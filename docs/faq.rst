FAQ
===

Frequently Asked Questions about :code:`pyhf` and its use.

Questions
---------

Where can I ask questions about ``pyhf`` use?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If you have a question about the use of ``pyhf`` not covered in the `documentation <https://pyhf.readthedocs.io/>`__, please ask a question on the `GitHub Discussions <https://github.com/scikit-hep/pyhf/discussions>`__.

If you believe you have found a bug in ``pyhf``, please report it in the `GitHub Issues <https://github.com/scikit-hep/pyhf/issues/new?template=Bug-Report.md&labels=bug&title=Bug+Report+:+Title+Here>`__.

How can I get updates on ``pyhf``?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If you're interested in getting updates from the ``pyhf`` dev team and release
announcements you can join the |pyhf-announcements mailing list|_.

.. |pyhf-announcements mailing list| replace:: ``pyhf-announcements`` mailing list
.. _pyhf-announcements mailing list: https://groups.google.com/group/pyhf-announcements/subscribe

Is it possible to set the backend from the CLI?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Yes.
Use the :code:`--backend` option for :code:`pyhf cls` to specify a tensor backend.
The default backend is NumPy.
For more information see :code:`pyhf cls --help`.

I installed ``pyhf`` from PyPI, why am I getting an error from a dependency?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You might need to manually constrain the **upper bound** on ``pyhf``'s core
dependencies.

We work hard to make sure that ``pyhf`` is well maintained so that it installs
correctly "out of the box" and have tested all of ``pyhf``'s core dependencies
to determine hard lower bounds for compatible dependency releases.
However, as ``pyhf`` `is a Python library
<https://caremad.io/posts/2013/07/setup-vs-requirement/>`__ we can only define
lower bounds for its core dependencies, as defining upper bounds would make
decisions for users on what versions of libraries they can use in Python
applications they build around ``pyhf`` --- `that would be bad
<https://hynek.me/articles/semver-will-not-save-you/>`__.
If ``pyhf`` were to define upper bounds we could create situations in which
``pyhf`` and other libraries defined in an environment file (i.e.,
``requirements.txt``) could have directly conflicting dependencies that would
result in ``pip`` failing to be able to install ``pyhf``.

To give an explicit example, changes in ``click``'s behavior between its
``v7.X`` releases and ``v8.X`` resulted in a runtime ``TypeError`` if ``click``
``v8.X`` was used with ``pyhf`` ``v0.6.2`` (c.f. Issue :issue:`1506`).
The Issue was fixed in the next release of ``pyhf``, but the intermediate
solution for users was to simply install an older version of ``click`` that was
still compatible with ``pyhf``

    .. code-block::

        # requirements.txt
        click<8.0.0
        pyhf==0.6.2

Does ``pyhf`` support Python 2?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
No.
Like the rest of the Python community, as of January 2020 the latest releases of ``pyhf`` no longer support Python 2.
The last release of ``pyhf`` that was compatible with Python 2.7 is `v0.3.4 <https://pypi.org/project/pyhf/0.3.4/>`__, which can be installed with

    .. code-block:: console

        python -m pip install pyhf~=0.3

I only have access to Python 2. How can I use ``pyhf``?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is recommended that ``pyhf`` is used as a standalone step in any analysis, and its environment need not be the same as the rest of the analysis.
As Python 2 is not supported it is suggested that you setup a Python 3 runtime on whatever machine you're using.
If you're using a cluster, talk with your system administrators to get their help in doing so.
If you are unable to get a Python 3 runtime, versioned Docker images of ``pyhf`` are distributed through `Docker Hub <https://hub.docker.com/r/pyhf/pyhf>`__.

Once you have Python 3 installed, see the :ref:`installation` page to get started.

I validated my workspace by comparing ``pyhf`` and ``HistFactory``, and while the expected CLs matches, the observed CLs is different. Why is this?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Make sure you're using the right test statistic (:math:`q` or :math:`\tilde{q}`) in both situations.
In ``HistFactory``, the asymptotics calculator, for example, will do something more involved for the observed CLs if you choose a different test statistic.

I ran validation to compare ``HistFitter`` and ``pyhf``, but they don't match exactly. Why not?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``pyhf`` is validated against ``HistFactory``.
``HistFitter`` makes some particular implementation choices that ``pyhf`` doesn't reproduce.
Instead of trying to compare ``pyhf`` and ``HistFitter`` you should try to validate them both against ``HistFactory``.

How is ``pyhf`` typeset?
~~~~~~~~~~~~~~~~~~~~~~~~

As you may have guessed from this page, ``pyhf`` is typeset in all lowercase.
This is largely historical, as the core developers had just always typed it that way and it seemed a bit too short of a library name to write as ``PyHF``.
When typesetting in LaTeX the developers recommend introducing the command

    .. code-block:: latex

        \newcommand{\pyhf}{\texttt{pyhf}}

If the journal you are publishing in requires you to use ``textsc`` for software names it is okay to instead use

    .. code-block:: latex

        \newcommand{\pyhf}{\textsc{pyhf}}

Troubleshooting
---------------

- :code:`import torch` or :code:`import pyhf` causes a :code:`Segmentation fault (core dumped)`

    This is may be the result of a conflict with the NVIDIA drivers that you
    have installed on your machine.  Try uninstalling and completely removing
    all of them from your machine

    .. code-block:: console

        # On Ubuntu/Debian
        sudo apt-get purge nvidia*

    and then installing the latest versions.
