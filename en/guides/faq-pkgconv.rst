.. http://doc.slitaz.org/en:guides:faq-pkgconv
.. en/guides/faq-pkgconv.txt · Last modified: 2010/07/08 17:14 (external edit)

.. _faq-pkgconv:

Packages from Other Distributions
=================================

:author: unknown


Symptoms
--------

* There is a useful package that is not yet available in the SliTaz repositories.
* Can I use packages from other distributions on my SliTaz installation?


Explanation
-----------

Packages in a general sense are simply a collection of files and meta-data, compiled from source code to suit a particular environment and installation.
Software binaries from other distributions can sometimes run if the libraries that power it exist for SliTaz.
It is an easy but not-always-accurate way of testing packages before writing SliTaz :ref:`Receipts <cookbook receipt>` for a SliTaz version of the package.

The SliTaz Package Manager, Tazpkg, can `convert <http://hg.slitaz.org/tazpkg/raw-file/tip/doc/tazpkg.en.html#convert>`_ packages from the Debian, RedHat, Slackware and Arch Linux formats, by unpacking them and using the meta-data information inside to create a Tazpkg.


Solution
--------

Simply run the following command in a Terminal::

  tazpkg convert {filename}

The converted Tazpkg will be created after auto-detection of the original format.

.. important::
   There may be occasions where the generated package does not function.
   This could be a result of mismatching libraries or missing dependant software.
   Similarly, the binaries may under-perform as they may have been compiled for a different environment and have to adapt.
   For these reasons, it is highly advisable to compile the software from source code and create a Tazpkg proper.
