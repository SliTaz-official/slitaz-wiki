.. http://doc.slitaz.org/en:oldcookbook:wok
.. en/oldcookbook/wok.txt · Last modified: 2012/04/06 12:21 by linea

.. _oldcookbook wok:

Wok & Tools
===========


Overview
--------

`Tazwok <http://hg.slitaz.org/tazwok/raw-file/tip/doc/tazwok.en.html>`_ is used to compile and generate code (cooking) via instructions found in a receipt.
It places compiled files in to a directory and calls upon :program:`Tazpkg` to package said directory.
The receipt found in a wok has a different "bottom half" to that of a :program:`Tazpkg`; :program:`Tazwok` has rules for compilation *and* packaging (which it forwards to :program:`Tazpkg`), whereas :program:`Tazpkg` is only concerned with packaging.

:program:`Tazwok` is one of many small utilities the SliTaz project uses to automatically rebuild the distribution from source.
The project also offers an archive of :ref:`tools <cookbook slitaztools>` containing various small utilities, examples and configuration files.
The distribution generator `Tazlito <http://hg.slitaz.org/tazwok/raw-file/tip/doc/tazwok.en.html>`_ is designed for users and developers; it can retrieve and reconstruct a LiveCD ISO image and generate a distribution flavor from a list of packages, a configuration file and a description.
The utilities are all distributed as a source archive and are installed by default on SliTaz.

.. tip::
   Developers and future contributors can refer to the development page that provides information on SliTaz project management.


.. rubric:: Wok Structure and Organisation

The :file:`wok` is a directory structure that houses all the available packages.
Each directory contains at least one :file:`receipt` to download, unpack, compile and generate a package.
:program:`Tazwok` also needs to create a directory to store downloaded sources (``$SOURCES_REPOSITORY``, usually :file:`/home/slitaz/src`) and a repository of generated packages (``$PACKAGES_REPOSITORY``, usually :file:`/home/slitaz/packages`); these values can be configured in the :file:`/etc/tazwok.conf` file.

There is more than one Wok on the `Mercurial repositories <http://hg.slitaz.org>`_:

wok-undigest:
  contributions awaiting testing/bug-fixing for inclusion in the unstable Wok

wok:
  packages for the unstable, Cooking release

wok-stable:
  packages for the stable SliTaz release

Initially, any contributions will be committed to the *undigest* repository.
When the package has seen sufficient testing with regards to automatic generation, it can be moved to the Wok.


Preparation
-----------

The Developer's Corner provides invaluable background information.
Please ensure you have read and understood it before continuing.

To begin using the Wok, `Tazwok <http://hg.slitaz.org/tazwok/raw-file/tip/doc/tazwok.en.html>`_ must already be installed on the system along with the main development tools (:program:`binutils`, compiler, libraries-dev, :command:`make`).
This requires you to install the meta-package :program:`slitaz-toolchain`:

.. code-block:: console

   # tazpkg recharge
   # tazpkg get-install slitaz-toolchain

To access the SliTaz repositories, you will need to install the :program:`mercurial` package:

.. code-block:: console

   # tazpkg get-install mercurial

More information on the use of the :program:`Mercurial` VCS is available from its `website <http://mercurial.selenic.com/>`_ and the "`Hg Book <http://hgbook.red-bean.com/>`_".


.. rubric:: Cloning the Wok

If you are to generate a package for inclusion in the SliTaz repositories, it is necessary to first obtain the current wok by using :program:`Mercurial`.
This is called *cloning* the Wok, a procedure that downloads the entire Wok and all its history to a working directory.
**If you wish to only use Tazwok to build packages for personal use, this is not necessary.**
See the :ref:`old creating a personal wok` section below instead.

The usual destination for a Wok clone is :file:`/home/slitaz/wok`:

.. code-block:: console

   $ hg clone http://hg.slitaz.org/wok/ /home/slitaz/wok 

This download may take some time; you will have a complete directory structure of the `Cooking wok <http://hg.slitaz.org/wok/>`_ as a working directory.

.. important::
   The Wok is one of many projects hosted in the `Mercurial repositories <http://hg.slitaz.org>`_.
   Individual packages are grouped as a large project (the Wok, Wok-Stable or Wok-Undigest) and is not its own sub-project but merely a sub-directory; Mercurial cannot (yet) clone specific parts of a project thus you cannot clone an individual package.

.. _old creating a personal wok:

.. rubric:: Creating a Personal Wok

If your packages are only for personal use and are not intended for inclusion in the SliTaz repositories, a wok can be created from scratch.

.. code-block:: console

   # tazwok gen-clean-wok


Compiling and Generating Packages
---------------------------------

Before compiling your first package, :program:`Tazwok` must know where your working directory is.
By default the path is :file:`/home/slitaz/wok` but you can change this or rename the wok that you want to download.
To view and check :program:`Tazwok` paths that will be used, and the number of packages in the wok, you can ask :program:`Tazwok` for statistics:

.. code-block:: console

   # tazwok stats

The process for generating a SliTaz package from source can be summarised thus: `configure <http://www.tuxfiles.org/linuxhelp/softinstall.html#s2>`_, `compile <http://www.tuxfiles.org/linuxhelp/softinstall.html#s3>`_ & `strip <http://linux.die.net/man/1/strip>`_.

.. note::
   We do not carry out the ':command:`make install`'-style step ourselves; the built files are not to be installed in the system but left in the output directory (``_pkg``), ready for packaging.

When generating your first package, it is advisable to `keep it simple <http://doc.slitaz.org/en:cookbook:devcorner#kiss-comply-to-standards>`_ and build your package without changing its receipt or seeking dependencies.
:program:`M4` is an ideal candidate for your first *cook*:

.. code-block:: console

   # tazwok cook m4

When :program:`Tazwok` has finished building :program:`M4`, its package is placed in the directory specified by the configuration file (:file:`/home/slitaz/packages` by default).
If all went well, you can install the package on the host system or use it to generate a LiveCD distribution via :program:`Tazlito`!

When you are familiar with :ref:`receipts <oldcookbook receipt>` and the compilation process, you can use the following command to create a new package (and a wok, if you don't have one) before interactively writing its receipt:

.. code-block:: console

   # tazwok new-tree <packageName> --interactive

Be sure to read the documentation on the options provided by the :ref:`receipt <oldcookbook receipt>` and the :ref:`Tazwok Tips <tazwoktips>` to avoid frustration!


.. rubric:: Cooking Multiple Packages with cook-list

:program:`Tazwok` can compile several packages with a single command.
This is achieved with a *cooking list*, a text file of one package per line.
:program:`Tazwok` can accept a cook-list with the command of the same name; for example, to cook the *mypkgs* cook-list:

.. code-block:: console

   # tazwok cook-list mypkgs.cooklist

.. tip::
   There are example lists in :file:`/usr/share/examples/tazwok/cooklists`.


Package Compilation Options
---------------------------

While you are free to use any options you want, it is necessary to respect the FSH, the documentation in :file:`/usr/share/doc` and follow the FreeDesktop standards (.desktop).


.. rubric:: Package-Specific

Package-specific options are your choice; for example, you can disable support for XML, have smaller binaries for :program:`PHP` and get rid of :program:`libxml2`, but in the case of :program:`PHP`, it's not worth the cost in terms of loss of functionality.
If you have any doubts, look at the receipts and compiler options in ``compile_rules``.


.. rubric:: Optimization

The official SliTaz packages are optimized for **i486**, the optimization arguments used to configure are specified in :file:`/etc/tazwok.conf` and can be called via the variable ``$CONFIGURE_ARGS``.
If you want to compile a package with different arguments, you can modify the :program:`Tazwok` configuration file:

.. code-block:: shell

   CONFIGURE_ARGS="--build=i486-pc-linux-gnu --host=i486-pc-linux-gnu"


.. rubric:: Files to Include/Exclude

Generally, the base packages contain no :file:`man`, :file:`info` or :file:`doc` files, nor static libraries; we have to create them via a package-doc or a package-dev.
Note that SliTaz does not intend to use the :command:`man` or :command:`info` command so there's no manual or GNU info file.
The creation of packages containing docs is really optional.
By contrast, writing documentation in the Handbook is more appreciated as it is widely-available and can be updated and improved easily.

In terms of configuration, the aim is to offer basic configuration files to run the package directly.
Special cases exist such as the web server :program:`LightTPD`, for example, where SliTaz supplies configuration files and start-up scripts in :file:`/etc/init.d` (documented in the Handbook).
For a new package, you are free to choose its default configuration depending on what you think is best for the end-user.
The :file:`/usr/share/examples` directory has example configurations and other kinds of useful information.


Package Categories
------------------

The categories of packages exist only for informational purposes and are not fixed.
The idea is to classify packages so that a web page that recovers data in the package receipt, can be generated each night.
For the short term, place development packages in 'devel', Xorg in 'x-window' and the variety of new packages in 'extra'.


Structure of a Wok Package
--------------------------

The structure of the packages in the wok should always be respected so that :program:`Tazwok` can find the correct files and directories.
Possible contents of a package (note the directory :file:`taz/` is created at time of cooking):

:file:`stuff/`:
  The material used to configure, compile and generate the package (patch(es), Makefile, pseudo fs, etc);

:file:`receipt`:
  The ever-present :ref:`receipt <oldcookbook receipt>`;

:file:`description.txt` (optional):
  The description of the package is included in the final package, copied to its root.
  Once installed, :program:`Tazpkg` identifies this file as the description and can display it via :command:`tazpkg desc pkgname`.

:file:`taz/`:
  Directory tree containing the package :program:`Tazpkg` generated, the compressed package is stored in the directory specified by ``$PACKAGES_REPOSITORY`` in the :program:`Tazwok` configuration file.

:program:`Tazwok` will automatically call upon :program:`Tazpkg` to package the :file:`taz` directory.
It also forwards any packaging instructions found in the receipt.


Structure of a Tazpkg
---------------------

The SliTaz packages are cpio archives containing files and a file-system compressed with gzip:

:file:`fs/`:
  Pseudo-file-system containing all the files to install.

:file:`receipt`:
  The :ref:`receipt <oldcookbook receipt>`.

:file:`files.list`:
  A list of files in the package.

:file:`description.txt`:
  The description of the package (optional).
