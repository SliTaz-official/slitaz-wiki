.. http://doc.slitaz.org/en:guides:tazwoktips
.. en/guides/tazwoktips.txt · Last modified: 2014/11/17 10:47 by brianperry

.. _tazwoktips:

Tazwok Tips
===========

:author: jozee, linea, brianperry

You must read the :ref:`cookbook` first — it's a great collection of documents.
This guide supplements the cookbook with some tips that you are most likely to face while compiling packages for SliTaz.
Note that it should be mentioned that Tazwok has been deprecated since SliTaz v4 (switched in favor for Tazlito, the ISO hacking method and :ref:`cookbook toolchain`).


Tips
----

#. Add the :program:`slitaz-dev-pkgs` package — it's a meta package that adds some of the useful :program:`*-dev` packages
#. Compilation Errors like 

   * checking for $PKG... configure: error: Couldn't find $PKG >= 1.0.0,
   * configure: error: Couldn't find $FILENAME
   * use: :command:`tazpkg search-pkgname $FILENAME` to find the package with the missing file; Add this package to the ``BUILD_DEPENDS`` in the receipt
#. Read receipts of the other packages for standardization.
   The problems you face may already have a solution in one of these receipts.
   Some example receipts include:

   * :program:`xorg-libSM` (or any :program:`xorg-*`): to use a ``$SOURCE`` variable
   * :program:`cups`: to strip files other than :file:`.so` in :file:`usr/lib`, and to compress some driver files
   * :program:`ndiswrapper-driver` or :program:`broadcom-wl`: to cook Linux modules
   * :program:`mplayer-svn`: to create packages from a SVN/Git repository
   * :program:`915resolution` or :program:`busybox`: to add patches
   * :program:`nvidia`: to create non-free packages that need the :program:`linux` package for compiling modules
   * :program:`get-*`: to create non-free packages
   * :program:`*-dev`: to see how to create a dev receipt

#. Create lighter packages.
   Always remember to disable GNOME dependencies or any other dependencies that are not so useful.
   Always use :command:`configure --help` to see the compile options.
   Many packages have auto-dependency tracking, i.e., if it finds a corresponding :program:`*-dev` package installed on the system, it will auto-enable an option.
   So, if you don't disable an unwanted dependency, then when a package is cooked on the SliTaz tank server, it is likely that the package won't run properly because of the missing dependency.
   The missing dependency problems that are often reported on the forum are caused due to this problem.
#. Missing :file:`.pc` files: Sometimes a dev package created upstream misses :file:`.pc` files, so :command:`pkg-config` cannot locate the corresponding dev files correctly.
   This can play havoc sometimes as you expect the upstream packages to be properly distributed.
   :program:`firefox-dev` is one such example.
#. gcc4.4 patches: The new stricter format of gcc4.4 breaks many packages.
   This is quite a common problem.
   You need to either create a simple patch or see if a new upgrade is available upstream.
   See :program:`mplayerplug-svn`.
#. Use a chroot environment and keep it clean so that you can submit the receipt with the correct ``BUILD_DEPENDS``.
