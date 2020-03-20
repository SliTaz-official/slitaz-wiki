.. _tazpkg manual:

TazPkg Manual
=============


Name
----

TazPkg — Tiny autonomous zone package manager.


Syntax
------

.. code-block:: shell

   tazpkg [command] [options...]


Description
-----------

:program:`TazPkg` is a lightweight package manager to install, list, download, update or remove precompiled packages on a GNU/Linux system.
:program:`TazPkg` offers commands for searching and creating packages and was created independently for the project.
The format of the packages using the :file:`*.tazpkg` extension is a cpio archive containing a filesystem compressed with lzma, a receipt and an optional description.
:program:`TazPkg` also manages dependencies based on package receipts.
Each receipt contains all the information about a package and can also include pre- and post-installation functions.
The same receipt is used by :ref:`Cookutils <cookutils manual>` to compile sources and generate a :file:`.tazpkg` package.

:program:`TazPkg` is entirely built from scratch using Shell script, compatible with Bash; it runs under Ash — part of the Busybox project.
:program:`TazPkg` is distributed under the free GNU license GPL V3.


Environment
-----------

:program:`TazPkg` uses some environment variables:

:``LANGUAGE``:
  defines the language of output and user confirmations.

  Note, ``LANG`` and ``LC_ALL`` environment variables also affect output language

:``LC_TIME``:
  defines the date format in the :ref:`tazpkg activity` command

:``root``:
  if defined, points to the root of a file system where :program:`TazPkg` should work.
  Note, a value defined using ``--root=`` option has precedence over this environment variable


Files
-----


Configuration files
^^^^^^^^^^^^^^^^^^^

* :file:`/etc/slitaz/slitaz.conf`
* :file:`/etc/slitaz/tazpkg.conf`


Package database files
^^^^^^^^^^^^^^^^^^^^^^

Default placement of the package database is :file:`/var/lib/tazpkg`.

:file:`ID` (deprecated):
  identifier of the current SliTaz repository state.

  The value changes when new or updated packages appear in the repository.

:file:`IDs`:
  identifier of the current SliTaz repository state and the UNIX time stamp.

  The ID value changes when new or updated packages appear in the repository.
  Time stamp allows you to track how long a change is made in the repository and to track the freshness of repository mirrors.
  (To convert UNIX time stamp to the date: :command:`date -d@timestamp`)

:file:`mirror`:
  URL of the current repository mirror in use.

  URL points to the remote folder containing packages and database files.

:file:`mirrors`:
  list of URLs of available repository mirrors.

  Note the difference between URLs from these two files; you should append URL from this file by :file:`packages/{cooking}/` (for cooking-based SliTaz version).

:file:`packages.list` (deprecated):
  list of package names with version numbers available in the repository.

:file:`packages.desc` (deprecated):
  list contained package name, version, short description, category and upstream URL.

:file:`packages.txt` (deprecated):
  list containing package name, version, short description and two package sizes
  (first — traffic to download package, second — HDD size for installed package).

:file:`packages.md5` (deprecated):
  list containing MD5 checksum with package file name.

:file:`packages.info`:
  list was built to replace and extend above lists.

  For every package available in the repository it contains: package name, version, category, short description, upstream URL, tags, package sizes, depends and MD5 checksum.
  Development continues, and the list can be extended by other fields, if necessary.

:file:`packages.equiv`:
  list of equivalent packages available in the repository.

  Format of the list item: ``package = rule rule…``.

  Format of the rule: ``alternative:newname`` — if ``alternative`` is installed then ``newname`` will be installed instead of the ``package``.

  In the second format of the rule ``alternative:`` part is omitted — in this case ``newname`` *can be* installed instead of the ``package`` (in the user choice).

:file:`packages.diff`:
  list created when you run :ref:`recharge <tazpkg recharge>` command.

  List contains package names with versions of the new packages available in the repository with the last recharge.

:file:`packages.up`:
  list created when you run :ref:`upgrade <tazpkg upgrade>` command.

  List contains package names you can upgrade with the last recharge.

:file:`descriptions.txt`:
  list of the “long” descriptions of the all packages available on the repository.

:file:`packages-desc.{language}`:
  optional list of the packages translated short descriptions
  (see the package tazpkg-desc-ru).

:file:`descriptions.{language}.txt`:
  optional list of the packages translated “long” descriptions
  (see the package tazpkg-desc-ru).

:file:`packages.icons`:
  optional list of the packages icons for TazPanel.

:file:`blocked-packages.list`:
  optional list of the packages blocked for update.

:file:`extra.list`:
  list of the extra packages (non-free packages; free packages but not compiled from sources).
  List contains package name, short description, upstream URL, category, version, license.

:file:`files.list.lzma`:
  very large compressed list which contains all the files of all the packages available in the repository
  (nearly 5,000 packages and nearly 0.7 million files now).

:file:`files-list.md5`:
  MD5 checksum of the :file:`files.list.lzma`.

:file:`installed.info`:
  list of the installed packages with the exact format of the :file:`packages.info` list.

  Idea is just to copy list item from :file:`packages.info` to the :file:`installed.info` during package installation.

:file:`installed.md5` (deprecated):
  list containing MD5 checksum with file names of all installed packages.

:file:`priority`:
  optional list of the repositories priority.

  One repository name per line.
  Undigest repositories are called by their names and main repository by “main”.
  If priority list absent, then default priority is: main repository and all existing undigest repositories in alphabetical order.

All the deprecated files will be deleted after we verify that the programs do not use them (and modify the programs as necessary).

In addition to the common package database files there is also individual folders for all installed packages placed by default in the :file:`/var/lib/tazpkg/installed/{package}`.
Every folder here may contain the following files:

* :file:`receipt` (mandatory) — the package recipe
* :file:`files.list` (mandatory) — list of package files
* :file:`md5sum` (mandatory) — checksums of package files (other checksum files can be specified in the settings: ``cksum`` (CRC32), ``md5sum`` (MD5), ``sha1sum`` (SHA1), ``sha256sum`` (SHA256), ``sha512sum`` (SHA512), ``sha3sum`` (SHA3-512))
* :file:`description.txt` (optional) — “long” description
* :file:`modifiers` (optional) — list of packages that have replaced some of the files of this package
* :file:`volatile.cpio.gz` (optional) — archive of “official” configuration files


Cache
^^^^^

Default placement of the packages cache is :file:`/var/cache/tazpkg` with sub-folders for the different repositories.

It is exactly :file:`/var/cache/tazpkg/{cooking}/packages` for the cooking-based SliTaz version.


Misc files
^^^^^^^^^^

File with default placement :file:`/var/log/slitaz/tazpkg.log` stores the TazPkg activity log.

.. tip::
   Log stores five types of actions: installing, uninstalling, blocking, unblocking, reconfiguring packages.

Shared MIME information which allows to “guess” SliTaz package files, package receipts and SliTaz flavor files.

.. tip::
   File placed here: :file:`/usr/share/mime/packages/tazpkg.xml`.

Plug-in for the TazPanel (SliTaz administration and configuration panel) :file:`/var/www/tazpanel/pkgs.cgi` allows you to manage SliTaz packages in the `web application <http://127.0.0.1:82/pkgs.cgi>`_.

TazPkg documentation is placed in the :file:`/usr/share/doc/tazpkg` folder.


Commands
--------

The first :command:`tazpkg` parameter is a command followed by other mandatory and optional parameters as will be described hereinafter.
Options begin with double dashes, you can arrange them in any order and in any place, even before the command.
Unknown and inappropriate options are ignored.
The following commands are equivalent:

.. code-block:: console

   $ tazpkg info nano --root=/mnt/sda6
   $ tazpkg --root=/mnt/sda6 info nano
   $ tazpkg info --root=/mnt/sda6 nano --color

You can add global option ``--root=…`` to any :program:`TazPkg` command.
This option allows you to work with other SliTaz installations and can point to the root of a mounted file system from another SliTaz installation.

.. tip::
   By the way, using this option allows to install SliTaz to the other file system “from scratch” and upgrade SliTaz packages remotely.


* Service commands

  * :ref:`tazpkg usage`: print short usage
  * :ref:`tazpkg help`: show help on the TazPkg commands
  * :ref:`tazpkg activity`: show TazPkg activity log
  * :ref:`tazpkg clean-cache`: clean all packages downloaded in cache directory
  * :ref:`tazpkg list-cache`: list all packages downloaded in cache directory
  * :ref:`tazpkg shell`: run interactive TazPkg shell

* Working with lists

  * :ref:`tazpkg list`: list installed packages on the system
  * :ref:`tazpkg list-mirror`: list all available packages on the mirror
  * :ref:`tazpkg list-config`: list the configuration files

* Search

  * :ref:`tazpkg search`: search for a package by pattern or name
  * :ref:`tazpkg search-pkgname`: search on mirror for package having a particular file
  * :ref:`tazpkg search-file`: search for file in all installed packages files

* Installing and removing packages

  * :ref:`tazpkg get`: download a package into the current directory
  * :ref:`tazpkg install`: install a local package
  * :ref:`tazpkg get-install`: download and install a package from the mirror
  * :ref:`get-list <tazpkg get-list>`: download a list of packages from the mirror
  * :ref:`install-list <tazpkg get-list>`: install all packages from a list of packages
  * :ref:`get-install-list <tazpkg get-list>`: download and install a list of packages from the mirror
  * :ref:`tazpkg remove`: remove the specified package and all installed files
  * :ref:`tazpkg reconfigure`: replay post install script from package
  * :ref:`tazpkg link`: link a package from another SliTaz installation
  * :ref:`tazpkg set-release`: change release and update packages
  * :ref:`add-flavor <tazpkg add-flavor>`: install the flavor list of packages
  * :ref:`install-flavor <tazpkg add-flavor>`: install the flavor list of packages and remove other ones

* Working with packages

  * :ref:`tazpkg info`: print information about a package
  * :ref:`tazpkg desc`: print description of a package
  * :ref:`tazpkg list-files`: list the files installed with a package
  * :ref:`block, unblock <tazpkg block>`: block an installed package version or unblock it for upgrade
  * :ref:`tazpkg check`: verify consistency of installed packages
  * :ref:`tazpkg bugs`: show known bugs in packages
  * :ref:`depends <tazpkg depends>`: display dependencies tree
  * :ref:`rdepends <tazpkg depends>`: display reverse dependencies tree
  * :ref:`tazpkg extract`: extract a (:file:`*.tazpkg`) package into a directory
  * :ref:`tazpkg pack`: pack an unpacked or prepared package tree
  * :ref:`tazpkg repack`: create a package archive from an installed package
  * :ref:`tazpkg repack-config`: create a package archive with configuration files
  * :ref:`tazpkg recompress`: rebuild a package with a better compression ratio
  * :ref:`tazpkg convert`: convert alien package to tazpkg
  * :ref:`tazpkg list-suggested`: print list of suggested packages

* Working with repositories

  * :ref:`tazpkg recharge`: recharge your packages database from the mirror
  * :ref:`tazpkg upgrade`: check packages, list and install latest upgrades
  * :ref:`tazpkg setup-mirror`: change the mirror URL configuration
  * :ref:`setup-undigest <tazpkg add-undigest>`: update an undigest mirror
  * :ref:`tazpkg list-undigest`: list undigest mirrors
  * :ref:`add-undigest <tazpkg add-undigest>`: add an undigest mirror
  * :ref:`tazpkg remove-undigest`: remove an undigest mirror
  * :ref:`tazpkg mkdb`: make a TazPkg database for a folder with :file:`*.tazpkg` packages


Service commands
^^^^^^^^^^^^^^^^

.. _tazpkg usage:

usage
~~~~~

Show the full list of the TazPkg commands with a brief description.

.. code-block:: console

   $ tazpkg usage


.. _tazpkg help:

help
~~~~

Display help for the selected command (``help`` or ``-h``).
You can enter a short name of the command, the full name or a part of the full name.
You can ignore hyphens at the beginning of the short name.
If the requested part of the full name match the several commands, you will be asked to clarify the request.

.. code-block:: console

   $ tazpkg help -gi
   $ tazpkg -h us


.. _tazpkg activity:

activity
~~~~~~~~

Display TazPkg activity log (``activity`` or ``log`` or ``-a``).
Optional parameter ``--nb=`` lets you set number of displayed lines.

.. code-block:: console

   $ tazpkg activity
   $ tazpkg -a --nb=20


.. _tazpkg clean-cache:

clean-cache
~~~~~~~~~~~

Remove :file:`*.tazpkg` packages downloaded to the cache (``clean-cache`` or ``-cc``).
During installation, TazPkg keeps a copy of packages downloaded from the Web.
This is done to save bandwidth in case of reinstallation, but you may want to free up space on the hard drive or re-download the packages.

.. code-block:: console

   # tazpkg clean-cache
   # tazpkg -cc


.. _tazpkg list-cache:

list-cache
~~~~~~~~~~

List :file:`*.tazpkg` packages downloaded to the cache.
Displays a list of file names and their sizes, as well as the total amount and size.

.. code-block:: console

   # tazpkg list-cache


.. _tazpkg shell:

shell
~~~~~

Run interactive TazPkg shell.
Here you can enter all the TazPkg commands listed above.

.. code-block:: console

   $ tazpkg shell
   # tazpkg shell


Working with lists
^^^^^^^^^^^^^^^^^^


.. _tazpkg list:

list
~~~~

List packages installed on the system (``list`` or ``-l``).
This command displays a column list of all installed packages.
It also allows you to list the categories (``c`` or ``cat`` or ``categories``), packages based on category and packages placed on hold (``b`` or ``blocked``).
You can also use the :ref:`tazpkg search` command for a list based on a term or package name.

.. code-block:: console

   $ tazpkg list
   $ tazpkg list cat
   $ tazpkg list games
   $ tazpkg list blocked


.. _tazpkg list-mirror:

list-mirror
~~~~~~~~~~~

List packages available on the mirror (``list-mirror`` or ``-lm``).
This command will display the packages list recharged from the mirror.
If it doesn't exist, you will be asked to launch ``tazpkg`` :ref:`tazpkg recharge` as administrator (root) for a list of available packages.
The ``--diff`` option is used to display the differences between the last and current list of packages.

.. code-block:: console

   $ tazpkg list-mirror
   $ tazpkg -lm --diff


.. _tazpkg list-config:

list-config
~~~~~~~~~~~

Lists the system configuration files.
The ``--box`` option displays in table form.
You can specify package name to display configuration files only for this package.

.. code-block:: console

   $ tazpkg list-config
   $ tazpkg list-config --box
   $ tazpkg list-config slim
   $ tazpkg list-config slim --box


Search
^^^^^^


.. _tazpkg search:

search
~~~~~~

Search for packages by owner or package name (``search`` or ``-s``).
This command will search for the term wanted in the installed packages (``-i`` or ``--installed``) and the list of available packages on the mirror (``-l`` or ``--list``).

To obtain the latest list of installable packages on the mirror, just run ``tazpkg recharge`` before conducting a search.

.. code-block:: console

   $ tazpkg search gcc
   $ tazpkg search mt -i
   $ tazpkg search bit -l


.. _tazpkg search-pkgname:

search-pkgname
~~~~~~~~~~~~~~

Search for a file on mirror and output only the packages names (``search-pkgname`` or ``-sp``).

.. code-block:: console

   $ tazpkg search-pkgname libnss
   $ tazpkg -sp /usr/share/fonts


.. _tazpkg search-file:

search-file
~~~~~~~~~~~

Search for a file among the files installed by the packages (``search-file`` or ``-sf``).
This command is very useful to find the full path to a file and determine if a file is present on the system.
Option ``--mirror`` allows to search for a file among all the files available on the mirror.

.. code-block:: console

   $ tazpkg search-file libnss
   $ tazpkg -sf /usr/share/fonts --mirror


Installing and removing packages
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. _tazpkg get:

get
~~~

Get a package from the mirror (``get`` or ``-g``).
The downloaded package is stored in the current directory.
You will get regular package, or get-package, or extra-package (in that order).
You can specify ``--extra`` option to get an extra-package only.

You can specify multiple packages on the command line or give tazpkg a list of the packages you want to download or use :ref:`get-list <tazpkg get-list>` command.

.. code-block:: console

   # tazpkg get grub
   # tazpkg get nano --root=/mnt/sda6
   # tazpkg get palemoon --extra
   # tazpkg -g nano mc
   # tazpkg -g --list=/tmp/office


.. _tazpkg install:

install
~~~~~~~

This command allows the installation of a local package with the :file:`.tazpkg` extension (``install`` or ``-i``).

Option ``--forced`` allows you to update an already installed package.
Option ``--newconf`` allows you to rewrite all user configuration files using the new files from a package.
Option ``--nodeps`` allows you to install only a specified package without its dependencies.

When TazPkg installs package dependencies, it prefers local packages (i.e. dependent packages located in the same folder as installed packages) over mirrored/cached packages with the ``--local`` option.
It is useful when you want to install a lot of already downloaded packages and their dependencies without the need of a network connection.

You can specify multiple packages on the command line or give tazpkg a list of the packages you want to install or use :ref:`install-list <tazpkg get-list>` command.

See :ref:`tazpkg get-install` to install a package from the internet.

.. code-block:: console

   # tazpkg install package-1.0.tazpkg
   # tazpkg -i path/to/package-1.0.tazpkg --forced
   # tazpkg -i path/to/package-1.0.tazpkg --root=/mnt/rootfs
   # tazpkg -i nano-2.4.0.tazpkg mc-4.8.14.tazpkg
   # tazpkg -i --list=/tmp/development
   # cd /home/boot/packages; tazpkg -i nano-2.4.0.tazpkg --local


.. _tazpkg get-install:

get-install
~~~~~~~~~~~

Get and install a package from a mirror on the internet (``get-install`` or ``-gi``).
Command begins by checking whether the package exists on the mirror and if it has been already downloaded.

Option ``--forced`` allows you to update an already installed package.
Option ``--newconf`` allows you to rewrite all user configuration files using the new files from a package.
Option ``--nodeps`` allows you to install only a specified package without its dependencies.

You can specify multiple packages on the command line or give tazpkg a list of the packages you want to get and install or use :ref:`get-install-list <tazpkg get-list>` command.

For a list of packages on the mirror, you must use the :ref:`tazpkg list-mirror` command.

.. code-block:: console

   # tazpkg get-install grub
   # tazpkg -gi grub --forced
   # tazpkg -gi nano --root=/mnt/sda6
   # tazpkg -gi nano mc
   # tazpkg -gi --list=/tmp/multimedia


.. _tazpkg get-list:

get-list, install-list, get-install-list
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Get and/or install a set of packages listed in a file.
This command allows you to work with the several packages with a single command.

All options are the same as for respective simple commands: :ref:`tazpkg get`, :ref:`tazpkg install` and :ref:`tazpkg get-install`.

.. code-block:: console

   # tazpkg install-list my-packages.list
   # tazpkg get-install-list my-packages.list --forced


.. _tazpkg remove:

remove
~~~~~~

Remove a package (``remove`` or ``-r``).
You will be asked for confirmation (y/N) of removing the package, as well as for removing packages depending on this package, and for reinstalling packages modified by this package.
This command will delete all files installed with the package.

Option ``--auto`` removes and reinstalls packages without your confirmation.

.. code-block:: console

   # tazpkg remove bc
   # tazpkg -r gtk+-3 --root=/mnt/sda6
   # tazpkg -r nano --auto


.. _tazpkg reconfigure:

reconfigure
~~~~~~~~~~~

Replays the post-install script from the package.

.. code-block:: console

   # tazpkg reconfigure gcc
   # tazpkg reconfigure gcc --root=/mnt/sda6


.. _tazpkg link:

link
~~~~

This command allows the installation of a package from another media device.
The set up is done through symbolic links and consumes very little memory.
It is generally used within the system RAM to install add-ons from a USB key.

.. code-block:: console

   # tazpkg link openoffice /media/usbdisk


.. _tazpkg set-release:

set-release
~~~~~~~~~~~

This command changes the current version and upgrades all of the packages to the latest release.

.. code-block:: console

   # tazpkg set-release cooking


.. _tazpkg add-flavor:

add-flavor, install-flavor
~~~~~~~~~~~~~~~~~~~~~~~~~~

Install a set of packages from a flavor.
In addition, ``install-flavor`` purges other installed packages.

.. code-block:: console

   # tazpkg add-flavor gtkonly
   # tazpkg install-flavor justx


Working with packages
^^^^^^^^^^^^^^^^^^^^^


.. _tazpkg info:

info
~~~~

Show all the available information related to your package.
You can specify the name of the installed package or a package that is not yet installed, but available in the repository.
You can also get information about a local file package by entering an absolute or relative path to the file :file:`.tazpkg`.
The information contained in the TazPkg database and in the package recipe — its version, category, maintainer, Web site and all the dependencies (see also :ref:`cookutils manual` for more information on recipes).

.. code-block:: console

   $ tazpkg info busybox
   $ tazpkg info minitube
   $ tazpkg info packages/comix-4.0.4.tazpkg


.. _tazpkg desc:

desc
~~~~

Description of the package (``desc`` or ``-d``).

.. code-block:: console

   $ tazpkg desc busybox


.. _tazpkg list-files:

list-files
~~~~~~~~~~

List all files installed with a package (``list-files`` or ``-lf``).
This command will simply read and display the :file:`files.list` of each package which is automatically generated when the package is created and is also used to remove files when uninstalling a package.

.. code-block:: console

   $ tazpkg list-files bc


.. _tazpkg block:

block, unblock, chblock
~~~~~~~~~~~~~~~~~~~~~~~

The ``block`` (or ``-b``) and ``unblock`` (or ``-u``) commands permit you to block installed package versions so that they are not maintained by an :ref:`tazpkg upgrade`.
Command ``chblock`` changes the blocking state of the package.
The list of packages on hold are contained in the :file:`/var/lib/tazpkg/blocked-packages.list`.
This file can also be edited by hand.

.. code-block:: console

   # tazpkg block grub
   # tazpkg unblock grub
   # tazpkg chblock grub


.. _tazpkg check:

check
~~~~~

Check dependencies on installed packages and determine whether all the files needed for the repacking of packages are present.
You can specify package name to check or check all installed packages.
Option ``--full`` makes few more checks and need more time.

.. code-block:: console

   $ tazpkg check
   $ tazpkg check --full
   $ tazpkg check nano
   $ tazpkg check sakura --full


.. _tazpkg bugs:

bugs
~~~~

Generates a list of known bugs in the packages.
You can specify a single package to show bugs.

.. code-block:: console

   $ tazpkg bugs
   $ tazpkg bugs nano


.. _tazpkg depends:

depends, rdepends
~~~~~~~~~~~~~~~~~

Displays a dependency tree or reverse dependency tree for a package.

For ``depends`` command: option ``--mark`` marks installed packages with plus sign and not installed with a minus/dash.
Option ``--total`` calculates the number of displayed packages and their size.
Also, with both options at a time you'll get the number and size of packages to be installed.

For ``rdepends`` command: *without* the ``--all`` option you'll get the list of only installed reverse dependency packages, *with* this option — a list of all available packages.
Option ``--mark`` marks packages as in the ``depends`` command.

.. code-block:: console

   $ tazpkg depends mpd
   $ tazpkg rdepends mpd


.. _tazpkg extract:

extract
~~~~~~~

Extract a package into a directory (``extract`` or ``-e``).
If you do not specify the destination directory, the package will be extracted in the current directory using the name :file:`{package}-{version}`.

.. code-block:: console

   $ tazpkg extract package.tazpkg
   $ tazpkg extract package.tazpkg target/dir


.. _tazpkg pack:

pack
~~~~

Create a package from a directory prepared in advance or from an unpacked package.
It can also manually create a :file:`.tazpkg` package (see the :ref:`Cookutils <cookutils manual>` documentation for the automatic creation of packages).

.. code-block:: console

   # tazpkg pack package-version


.. _tazpkg repack:

repack
~~~~~~

Recreate a package from the files on a system where it was previously installed.

.. code-block:: console

   # tazpkg repack <em>package</em>


.. _tazpkg repack-config:

repack-config
~~~~~~~~~~~~~

Recreate a package of the system configuration files (see :ref:`tazpkg list-config`).
It is enough to install the package to find the current configuration.

.. code-block:: console

   # tazpkg repack-config


.. _tazpkg recompress:

recompress
~~~~~~~~~~

Recompress :file:`.tazpkg` cpio archive with lzma.

.. code-block:: console

   # tazpkg recompress package.tazpkg


.. _tazpkg convert:

convert
~~~~~~~

Converts an “alien” package into a SliTaz package (:file:`.tazpkg`) (``convert`` or ``-c``).

Supported packages formats:

* `Debian packages <https://packages.debian.org/search>`_
  (:file:`*.deb`, :file:`*.udeb`)
* `RPM packages <http://rpmfind.net/linux/rpm2html/search.php>`_
  (:file:`*.rpm`)
* `Slax packages <http://www.slax.org/>`_
  (:file:`*.sb`)
* `Puppy packages <http://puppylinux.org/>`_
  (:file:`*.sfs`, :file:`*.pet`)
* `Slackware packages <http://www.slackware.com/packages/>`_
  (:file:`*.tgz`)
* `NuTyX packages <http://www.nutyx.org/>`_
  (:file:`*.cards.tar.xz`)
* `Arch Linux <https://www.archlinux.org/packages/>`_ /
  `Alpine Linux packages <http://pkgs.alpinelinux.org/packages>`_
  (:file:`*.apk`, :file:`*.pkg.tar.gz`, :file:`*.pkg.tar.xz`)
* `OpenWrt packages <http://wiki.openwrt.org/doc/packages>`_
  (:file:`*.ipk`, :file:`*.opk`)
* `0Linux packages <http://0.tuxfamily.org/doku.php/paquets/start>`_
  (:file:`*.spack`)
* `paldo packages <http://www.paldo.org/index-section-packages.html>`_
  (:file:`*.tar.bz2`)
* `Void packages <http://www.voidlinux.eu/packages/>`_
  (:file:`*.xbps`)
* `Tinycore packages <http://tinycorelinux.net/>`_
  (:file:`*.tce`, :file:`*.tcel`, :file:`*.tcem`, :file:`*.tcz`)

.. code-block:: console

   # tazpkg convert <em>alien-package-file</em>


.. _tazpkg list-suggested:

list-suggested
~~~~~~~~~~~~~~

List suggested packages for each of your installed packages.
With option ``--all`` it shows all suggested packages, and without option it shows only non installed suggested packages.

.. code-block:: console

   $ tazpkg list-suggested
   $ tazpkg list-suggested --all


Working with repositories
^^^^^^^^^^^^^^^^^^^^^^^^^


.. _tazpkg recharge:

recharge
~~~~~~~~

Recharge the list of available packages on the mirror.
This command will download the most recent packages database of installable packages on the mirror and before starting will save the old database.
Once the DB is updated, you can then use the :ref:`tazpkg list` and :ref:`tazpkg search` commands.
To view and list the differences, you can use :command:`list-mirror --diff`; and to view and update packages, you can simply :ref:`tazpkg upgrade`.

Command without options will recharge databases of all your repositories.
You can specify the repository to be recharged: “main” for main repo, or undigest repository name.

.. code-block:: console

   # tazpkg recharge
   # tazpkg recharge main
   # tazpkg recharge My_Undigest
   # tazpkg recharge --root=/mnt/sda6


.. _tazpkg upgrade:

upgrade
~~~~~~~

Upgrade allows you to update all installed packages available on the current mirror (``upgrade`` or ``up``).
Upgrading packages is an important part of system security, it helps to keep you secure with the latest updates and fixes.
The SliTaz project, although tiny, provides regular updates on security and generally offers the latest versions of software.
Note that this function is aimed at people with SliTaz installed on a hard drive.
Updated packages in Live CD mode will be lost on system shutdown.

At the beginning the packages database is updated automatically (:ref:`tazpkg recharge`) in order to provide you with the current list of packages that you can update.

Without options it runs in interactive mode and asks before install.
You can specify one of the next options: ``-c`` or ``--check`` to check only for available upgrades; ``-i`` or ``--install`` to check for upgrades and install them all.

.. code-block:: console

   # tazpkg upgrade
   # tazpkg up --check
   # tazpkg up -i


.. _tazpkg setup-mirror:

setup-mirror
~~~~~~~~~~~~

Setup the URL for the mirror (``setup-mirror`` or ``-sm``).
Command will ask for the URL of the new mirror.
Note that you can also modify the main :file:`/var/lib/tazpkg/mirror` file.
The URL must point to the directory containing the :file:`packages.info` and packages.

.. code-block:: console

   # tazpkg setup-mirror


.. _tazpkg add-undigest:

add-undigest, setup-undigest
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set the URL of an additional unofficial mirror to test packages that are not yet present on the official mirrors.
Note, you can also manually edit the file in :file:`/var/lib/tazpkg/undigest/{repository}`.
The URL must point to the directory containing the packages and :file:`packages.info`.

.. code-block:: console

   # tazpkg add-undigest public-repository http://my.home.org/slitaz
   # tazpkg setup-undigest local-repository /home/slitaz/packages


.. _tazpkg list-undigest:

list-undigest
~~~~~~~~~~~~~

Lists additional undigest mirrors.
Option ``--box`` will output list in the table form.

.. code-block:: console

   $ tazpkg list-undigest
   $ tazpkg list-undigest --box


.. _tazpkg remove-undigest:

remove-undigest
~~~~~~~~~~~~~~~

Removes the URL of an undigest mirror.
You will be asked for confirmation.

.. code-block:: console

   # tazpkg remove-undigest my-repository


.. _tazpkg mkdb:

mkdb
~~~~

Make a TazPkg database for a selected folder with :file:`*.tazpkg` packages.

The following files describing packages will be created inside the selected folder: :file:`packages.info`, :file:`packages.equiv`, :file:`descriptions.txt`, :file:`files.list.lzma`, :file:`IDs`.
Do nothing if the database already exists; you can also force database files rebuilding with the ``--forced`` option.

.. code-block:: console

   # tazpkg mkdb /home/boot/packages
   # tazpkg mkdb /home/boot/packages --forced


Maintainer
----------

Christophe Lincoln <pankso@slitaz.org>
