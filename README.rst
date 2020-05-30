manylinux
=========

Email: wheel-builders@python.org

Archives: https://mail.python.org/mailman/listinfo/wheel-builders

Older archives: https://groups.google.com/forum/#!forum/manylinux-discuss

The goal of the manylinux project is to provide a convenient way to
distribute binary Python extensions as wheels on Linux. This effort
has produced `PEP 513 <https://www.python.org/dev/peps/pep-0513/>`_ which
is further enhanced by `PEP 571 <https://www.python.org/dev/peps/pep-0571/>`_
defining ``manylinux2010_x86_64`` and ``manylinux2010_i686`` platform tags.

PEP 513 defined ``manylinux1_x86_64`` and ``manylinux1_i686`` platform tags
and the wheels were built on Centos5. Centos5 reached End of Life (EOL) on
March 31st, 2017 and thus PEP 571 was proposed.

Code and details regarding ``manylinux1`` can be found here:
`manylinux1 <https://github.com/pypa/manylinux/tree/manylinux1>`_.

Wheel packages compliant with those tags can be uploaded to
`PyPI <https://pypi.python.org>`_ (for instance with `twine
<https://pypi.python.org/pypi/twine>`_) and can be installed with
pip:

+-------------------+----------------------------------+
| ``manylinux`` tag | Client-side pip version required |
+===================+==================================+
| ``manylinux2014`` | pip >= 19.3                      |
+-------------------+----------------------------------+
| ``manylinux2010`` | pip >= 19.0                      |
+-------------------+----------------------------------+
| ``manylinux1``    | pip >= 8.1.0                     |
+-------------------+----------------------------------+

The manylinux2010 tags allow projects to distribute wheels that are
automatically installed (and work!) on the vast majority of desktop
and server Linux distributions.

This repository hosts several manylinux-related things:


Docker images
-------------

.. image:: https://travis-ci.org/pypa/manylinux.svg?branch=master
   :target: https://travis-ci.org/organization/pypa


manylinux1
~~~~~~~~~~

x86-64 image: ``quay.io/pypa/manylinux1_x86_64``

.. image:: https://quay.io/repository/pypa/manylinux1_x86_64/status
   :target: https://quay.io/repository/pypa/manylinux1_x86_64

i686 image: ``quay.io/pypa/manylinux1_i686``

.. image:: https://quay.io/repository/pypa/manylinux1_i686/status
   :target: https://quay.io/repository/pypa/manylinux1_i686

manylinux2010
~~~~~~~~~~~~~

x86-64 image: ``quay.io/pypa/manylinux2010_x86_64``

.. image:: https://quay.io/repository/pypa/manylinux2010_x86_64/status
   :target: https://quay.io/repository/pypa/manylinux2010_x86_64

i686 image: ``quay.io/pypa/manylinux2010_i686``

.. image:: https://quay.io/repository/pypa/manylinux2010_i686/status
   :target: https://quay.io/repository/pypa/manylinux2010_i686

manylinux2014
~~~~~~~~~~~~~

x86_64 image: ``quay.io/pypa/manylinux2014_x86_64``

.. image:: https://quay.io/repository/pypa/manylinux2014_x86_64/status
   :target: https://quay.io/repository/pypa/manylinux2014_x86_64

i686 image: ``quay.io/pypa/manylinux2014_i686``

.. image:: https://quay.io/repository/pypa/manylinux2014_i686/status
   :target: https://quay.io/repository/pypa/manylinux2014_i686

aarch64 image: ``quay.io/pypa/manylinux2014_aarch64``

.. image:: https://quay.io/repository/pypa/manylinux2014_aarch64/status
   :target: https://quay.io/repository/pypa/manylinux2014_aarch64

ppc64le image: ``quay.io/pypa/manylinux2014_ppc64le``

.. image:: https://quay.io/repository/pypa/manylinux2014_ppc64le/status
   :target: https://quay.io/repository/pypa/manylinux2014_ppc64le

s390x image: ``quay.io/pypa/manylinux2014_s390x``

.. image:: https://quay.io/repository/pypa/manylinux2014_s390x/status
   :target: https://quay.io/repository/pypa/manylinux2014_s390x

These images are rebuilt using Travis-CI on every commit to this
repository; see the
`docker/ <https://github.com/pypa/manylinux/tree/master/docker>`_
directory for source code.

The images currently contain:

- CPython 2.7, 3.5, 3.6, 3.7 & 3.8, installed in
  ``/opt/python/<python tag>-<abi tag>``. The directories are named
  after the PEP 425 tags for each environment --
  e.g. ``/opt/python/cp27-cp27mu`` contains a wide-unicode CPython 2.7
  build, and can be used to produce wheels named like
  ``<pkg>-<version>-cp27-cp27mu-<arch>.whl``.

- Devel packages for all the libraries that PEP 571 allows you to
  assume are present on the host system

- The `auditwheel <https://pypi.python.org/pypi/auditwheel>`_ tool

Note that prior to CPython 3.3, there were two ABI-incompatible ways
of building CPython: ``--enable-unicode=ucs2`` and
``--enable-unicode=ucs4``. We provide both versions
(e.g. ``/opt/python/cp27-cp27m`` for narrow-unicode,
``/opt/python/cp27-cp27mu`` for wide-unicode). NB: essentially all
Linux distributions configure CPython in ``mu``
(``--enable-unicode=ucs4``) mode, but ``--enable-unicode=ucs2`` builds
are also encountered in the wild. Other less common or virtually
unheard of flag combinations (such as ``--with-pydebug`` (``d``) and
``--without-pymalloc`` (absence of ``m``)) are not provided.

Note that `starting with CPython 3.8 <https://docs.python.org/dev/whatsnew/3.8.html#build-and-c-api-changes>`_,
default ``sys.abiflags`` became an empty string: the ``m`` flag for pymalloc
became useless (builds with and without pymalloc are ABI compatible) and so has
been removed. (e.g. ``/opt/python/cp38-cp38``)

