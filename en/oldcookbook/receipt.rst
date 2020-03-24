.. http://doc.slitaz.org/en:oldcookbook:receipt
.. en/oldcookbook/receipt.txt · Last modified: 2012/04/06 12:16 by linea

.. _oldcookbook receipt:

Recipes
=======

:author: linea

This document describes the opportunities offered by the recipes used by :program:`Tazwok` to compile and generate packages for SliTaz and :program:`Tazpkg` through The wok and tools.
The recipe for a package is also used by :program:`Tazpkg` to install/uninstall and provide information about a :file:`.tazpkg` package.
Each recipe begins with a comment in English:

.. code-block:: shell

   # SliTaz package receipt


Variables
---------

The first 5 variables should always be present and defined.
They respectively configure the package (``$PACKAGE``), its version, its category, provide a short description and the name of the maintainer.
Example for the package, file manager :program:`Clex`:

.. code-block:: shell

   PACKAGE="clex"
   VERSION="3.16"
   CATEGORY="base-apps"
   SHORT_DESC="Text mode file manager."
   MAINTAINER="pankso@slitaz.org"


Variables (optional)
--------------------

:program:`Tazwok` also knows how to use various optional variables.
It can, for example, use the name of another source package.
There are also variables that are used by :program:`Tazpkg` to manage dependencies or provide information about the package.

``$DEPENDS``:
  Set dependencies, there may be several dependencies separated by a space or on several lines.
  This variable is used mainly by :program:`Tazpkg` when installing the package and :program:`Tazwok` to build large packages such as :program:`Xorg`.
  Example for :program:`Clex` which depends on :program:`ncurses`:

  .. code-block:: shell

     DEPENDS="ncurses"

``$BUILD_DEPENDS``:
  Set compilation dependencies, again separated by a space or several lines.
  This variable is used by :program:`Tazwok` during the cooking of a package.
  Example:

  .. code-block:: shell

     BUILD_DEPENDS="ncurses-dev"

``$TARBALL``:
  The archive is a source with the extension (:file:`tar.gz`, :file:`tgz` or :file:`tar.bz2`).
  In general, the variables ``$PACKAGE`` and ``$VERSION`` are used to just change the extension, it helps to upgrade the package without changing the ``$VERSION`` variable.
  Generic example (see also ``$SOURCE`` example):

  .. code-block:: shell

     TARBALL="$PACKAGE-$VERSION.tar.gz"

``$WEB_SITE``:
  The official website of the package.
  It may be that some libraries have no website, in this case, there is no need to specify a URL.
  Note :program:`Tazwok` and :program:`Tazpkg` both expect to find a URL with the complete HTTP:

  .. code-block:: shell

     WEB_SITE="http://www.clex.sk/"

``$WGET_URL``:
  URL to download the source file.
  In general the variable ``$TARBALL`` should be used to facilitate the updating of the package without changing the ``$VERSION``.
  Using a configuration file, :program:`Tazwok` also configures by default 3 mirrors: ``$GNU_MIRROR`` for the GNU mirror, ``$SF_MIRROR`` for SourceForge and ``XORG_MIRROR`` for mirroring the graphical server :program:`Xorg`.
  Example for :program:`Clex`:

  .. code-block:: shell

     WGET_URL="http://www.clex.sk/download/$TARBALL"

``$CONFIG_FILES``:
  Some packages provide customized configuration files.
  The ``$CONFIG_FILES`` variable provides a list of these files that can be saved by the :command:`tazpkg repack-config` command.
  These files are not overwritten when reinstalling the package if they already exist and the package can be successfully recreated with :command:`tazpkg repack`, (even if they have been modified since).
  :program:`Netatalk` for example:

  .. code-block:: shell

     CONFIG_FILES="/etc/netatalk/AppleVolumes.* /etc/netatalk/*.conf"

``$SUGGESTED``:
  Lists useful packages without being essential.
  Also used to activate optional features.

``$WANTED``:
  SliTaz packages normally depend on the compilation of a source package.
  Sometimes the recipe of a package requires no compilation of rules, then ``$WANTED`` is used to copy files from the source of another package by using the variable ``$src``.

``$SOURCE``:
  It may be that the :program:`Tazpkg` package name differs from the name of the source package.
  Example for :program:`Xorg` packages, the name of :program:`Tazpkg` library :program:`X11` is ``xorg-libX11`` and the name of the package source is ``libX11``.
  ``$SOURCE`` allows you to use the variables ``$src`` and ``$_pkg`` during the cooking of a package.
  It should be noted that in the case of ``libX11``, the name of the source archive becomes ``$SOURCE-$VERSION.tar.gz``.

``$PROVIDE``:
  Some packages offer the same functionality, for instance the web server was at first :program:`lighttpd`; now :program:`apache` is available.
  All packages dependent on a web server refer to :program:`lighttpd`.
  The ``PROVIDE="lighttpd"`` variable in the :program:`apache` recipe states that packages dependent on :program:`lighttpd` do not need to install the :program:`lighttpd` package if :program:`apache` is already on the system.

  Some packages may vary according to the webserver you choose, ie. the :program:`php` package is dependent on :program:`lighttpd`, as is :program:`php-apache` on :program:`apache`.
  The ``PROVIDE="php:apache"`` in the :program:`apache` recipe says that you must install :program:`php-apache` instead of :program:`php`, if :program:`apache` is already on the system.
  Therefore each package dependent on :program:`php` will install either :program:`php-apache` or :program:`php` according to the webserver on the system.

  This variable also chooses packages compiled with different options.
  The ``PROVIDE="epdfview:cups"`` in the :program:`epdfview-cups` recipe allows you to install :program:`epdfview` with printer support via :program:`cups` if :program:`cups` is already on the system.

  You can also define virtual packages with this variable.
  The lines ``PROVIDE="libgl"`` in the :program:`mesa` package and ``PROVIDE="libgl:nvidia"`` in the :program:`nvidia-glx` package, define that :program:`libgl` is an optimized version when the :program:`nvidia` package is installed.

``$SELF_INSTALL`` (obsolete):
  Certain packages use commands provided by the package itself in the ``post_install`` function.
  To install this package into a directory other than root and still be able to use these commands, the package must have been installed in :file:`/` in earlier stages.
  The line: ``SELF_INSTALL=1`` alerts :program:`tazpkg` to this feature.
  This variable is depreciated.
  The command :command:`chroot "$1/" a_package_command` in ``post_install`` does the job.


Variables generated by :program:`Tazwok`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Certain factors are known only during the cooking of a package or after the package has been cooked.
:program:`Tazwok` will add them to the recipe automatically.

``$PACKED_SIZE``:
  :program:`Tazpkg` file size.

``$UNPACKED_SIZE``:
  Space taken up by the package after installation.

``$EXTRAVERSION``:
  Some packages have 2 different versions.
  This is in case of modules added to the Linux kernel, such as :program:`squashfs`, because the module depends on the version of the kernel with which it was compiled.
  In this case ``$EXTRAVERSION`` contains the kernel version and :program:`Tazwok` determines the module from the contents of :file:`/lib/modules`.


Variables used in functions
^^^^^^^^^^^^^^^^^^^^^^^^^^^

:program:`Cookutils` configures several variables that facilitate the compilation and construction of :program:`Tazpkg` packages.
These variables are controlled automatically by :program:`cookutils` using the information contained in the recipe; they can be used by the functions ``compile_rules`` and ``genpkg_rules`` described in the chapter Functions.

``$src``:
  Defines the path to the directory of unarchived sources.

``$_pkg``:
  Defines the path to the compiled binaries installed via :command:`make DESTDIR=$PWD/_pkg install`.
  This variable is used to copy the generated files and create :program:`Tazpkg` packages.

``$install``:
  Same as ``$_pkg``.

``$fs``:
  Defines the path to the pseudo filesystem (fs) in each package.
  The 'fs' of the package corresponds to the root of the system, a bit like :program:`Clex` will for example be in :file:`$fs/usr/bin/clex`.
  Note the need to create the necessary directories via function ``genpkg_rules()`` before copying the files.

``$CONFIGURE_ARGS``:
  This variable is defined in the :program:`cookutils` configuration file (:file:`cook.conf`).
  It allows you to specify generic optimization arguments during construction of a package.
  Default is the i486 architecture. 

``$DESTDIR``:
  Defines the path to install compiled binaries after the build via :command:`make DESTDIR=$DESTDIR install`.


Functions
---------

A recipe may contain 4 functions.
:program:`Tazwok` knows how to deal with functions containing compilation rules (``compile_rules``) and rules used to generate a package (``genpkg_rules``).
These functions may contain all sorts of GNU/Linux standard commands, such as :command:`sed`, :command:`awk`, :command:`patch` and variables automatically configured.


.. rubric:: ``compile_rules()``

To compile a package you can use the variable ``$src`` to move (:command:`cd`) in the directory of sources and use ``$CONFIGURE_ARGS`` to include arguments from the Tazwok configuration file.
To build the package you usually launch :command:`make` without any arguments, and to install the package into the directory :file:`_pkg`: it's necessary to use the command :command:`make DESTDIR=$PWD/_pkg install`.
Generic example:

.. code-block:: shell

   # Rules to configure and make the package.
   compile_rules()
   {
   	cd $src
   	./configure --prefix=/usr --infodir=/usr/share/info \
   	--mandir=/usr/share/man $CONFIGURE_ARGS
   	make
   	make DESTDIR=$PWD/_pkg install
   }


.. rubric:: ``genpkg_rules()``

To generate a :program:`tazkg` package we must specify commands in the function ``genpkg_rules``.
In this example we create a psuedo directory :file:`/usr` in the filesystem of the package, copy the whole binary(s) and finally use :command:`strip` to clean the files:

.. code-block:: shell

   # Rules to gen a SliTaz package suitable for Tazpkg.
   genpkg_rules()
   {
   	mkdir -p $fs/usr
   	cp -a $_pkg/usr/bin $fs/usr
   	strip -s $fs/usr/bin/*
   }


.. rubric:: ``pre_install()`` and ``post_install()``

These functions are initiated by :program:`Tazpkg` when installing the package.
They must be defined before generating the :file:`.tazpkg` package with :program:`Tazwok`.
If no rules are given for these functions, they have no raison d'etre and can be removed.
Example using :command:`echo` to display some text (no function should be empty):

.. code-block:: shell

   # Pre and post install commands for Tazpkg.
   pre_install()
   {
   	echo "Processing pre-install commands..."
   }
   post_install()
   {
   	echo "Processing post-install commands..."
   }


.. rubric:: ``pre_remove()`` and ``post_remove()``

These functions are initiated by :program:`Tazpkg` when removing the package.
They must be defined before generating the :file:`.tazpkg` package with :program:`Tazwok`.
If no rules are given for these functions, they have no raison d'etre and can be removed.
Example using :command:`echo` to display some text (no function should be empty):

.. code-block:: shell

   # Pre and post remove commands for Tazpkg.
   pre_remove()
   {
   	echo "Processing pre-remove commands..."
   }
   post_remove()
   {
   	echo "Processing post-remove commands..."
   }


.. rubric:: ``clean_wok()`` (deprecated)

This is useless with latest :program:`cookutils`, source are all uncompressed in :file:`wok/{pkg}/source` to keep build wok clean and structured.

This function helps to define additional commands to be run when cleaning the wok, it is useful to delete files or directories that are not supported by :program:`Tazwok`:

.. code-block:: shell

   # clean commands for Tazwok.
   clean_wok()
   {
   	rm -rf $WOK/$PACKAGE/vim71
   }
