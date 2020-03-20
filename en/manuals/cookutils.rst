.. _cookutils manual:

Cookutils Documentation
=======================


SliTaz Cook & Cooker
--------------------

The SliTaz Cookutils provide tools and utils to help build SliTaz packages.
They are easy to use and learn, fast and light.
You will be able to create SliTaz packages in a few commands.
The cookutils provide the :program:`cook` utility and :ref:`cookutils cooker`.

:program:`Cook` lets you compile and create a package, provide a log file and check the receipt/package quality.
The :program:`Cooker` is a build bot with more automation and can be used as a frontend to :program:`cook` since it provides a CGI/web interface which lets you view cook logs in a nice and colored way.
:program:`Cook` and the :program:`Cooker` use the same DB files and *wok*, they both share :ref:`blocked <cookutils blocked>` and broken packages as well as any activity.

For technical information, for example the coding style, etc, please refer to the :file:`README` found in the source tree or in :file:`/usr/share/doc/cookutils`.


Cook usage
^^^^^^^^^^

:program:`Cook` provides a small built-in help usage that you can display with the command :command:`usage`.
It also has some options to perform special tasks on a package before *cooking* it or afterwards.
To get help and usage:

.. code-block:: console

   # cook usage


Howto
^^^^^

The first thing you will have to do before building packages is setup your environment.
The 2 recommended ways of working: *cook* directly on host or *cook* in chroot to protect your host.
In the case you want to work in a chroot you can install and use :program:`Tazdev` to create one and chroot into it:

.. code-block:: console

   # tazdev gen-chroot && tazdev chroot

By default :program:`Tazdev` creates a chroot in :file:`/home/slitaz/cooking/chroot` but you can specify a custom path in the argument.
The chroot location is not important, when you will be in the chroot you will use standard SliTaz paths such as :file:`/home/slitaz/wok` for the *wok* directory or :file:`/home/slitaz/log` for all the cook logs.
As usual you can display :program:`tazdev` help usage with: :command:`tazdev usage`.

When you use a chroot there are 2 special directories mounted with the ``bind`` option: :file:`src` and :file:`packages`.
The sources for all packages are stored by default in :file:`/home/slitaz/src`, this directory is mounted into the chroot so the utils can use them.
This method lets you share sources between many chroots such as one for *cooking* and one for *stable*.
The packages directory default location is: :file:`/home/slitaz/{version}/packages` so they are not in the chroot and are safe in case the chroot is removed by error.


Getting started
^^^^^^^^^^^^^^^

So you have decided the way you want to work, so lets prepare the cook environment.
:program:`Cook` uses the :file:`cook.conf` configuration file, if you want to use custom paths for SliTaz directories and files, you'll have to modify it.
The setup will create some directories and files to keep trace of activity and errors, all files are pure plain text files that you can open in a text editor.
To prepare your environment:

.. code-block:: console

   # cook setup

The setup command has a ``--wok`` option which lets you clone a SliTaz wok while setting up your cook environment.
Even if you are not yet an official developer you can clone it and use existing packages as an example to create your own.
To setup and clone the default *cooking* *wok* or the *undigest* *wok*:

.. code-block:: console

   # cook setup --wok
   # cook setup --undigest


Test your environment
^^^^^^^^^^^^^^^^^^^^^

:program:`Cook` provides a :command:`test` command which will create a package and *cook* it.
This lets you see if your environment is working and it provides an example package with a receipt.
The dummy package is named 'cooktest' and can be removed after testing.
To *cook* the test package:

.. code-block:: console

   # cook test


Create and cook
^^^^^^^^^^^^^^^

If your environment is setup correctly you can start creating and compiling SliTaz packages from your *wok*.
To create a new package with an empty receipt (you can also create a receipt interactively):

.. code-block:: console

   # cook new pkgname
   # cook new pkgname --interactive

If you have just created a new package, you'll have to edit the receipt with your favorite text editor.
When the receipt is ready or if you have an existing package, you can *cook* it:

.. code-block:: console

   # cook pkgname

If all went well you will find your package in the :file:`$SLITAZ/packages` directory and any produced files in :file:`$SLITAZ/wok/{pkgname}`.


Cook and install
^^^^^^^^^^^^^^^^

If you want to *cook* and install the package in one command:

.. code-block:: console

   # cook pkgname --install


Get sources
^^^^^^^^^^^

If you want or need to download only the source of a package without building it, you can use the option ``--getsrc`` as below:

.. code-block:: console

   # cook pkgname --getsrc


Clean packages
^^^^^^^^^^^^^^

After compilation and packaging there are several files in the *wok* that take up disk space.
To clean a single package:

.. code-block:: console

   # cook pkgname --clean

You can also clean the full *wok* at once or you can choose to keep SliTaz related files and just remove the source:

.. code-block:: console

   # cook clean-wok
   # cook clean-src


Search
^^^^^^

:program:`Cook` provides a simple search function to quickly find a package in the *wok*.
It uses :command:`grep` and so supports regular expressions:

.. code-block:: console

   # cook search busybox


Receipt functions
^^^^^^^^^^^^^^^^^

Many packages provide the same kind of files such as ``*-dev`` packages with static libs, *pkgconfig* files and include headers.
So cook provides a function to be used in the receipt::

  get_dev_files     : Install /usr/lib/{lib.*a,pkgconfig} /usr/include


Packages DB list
^^^^^^^^^^^^^^^^

:program:`Cook` can list packages in the *wok* and also create a suitable packages list for :program:`Tazpkg`.
This lets you create a local packages repository quite easily and is used to create the official SliTaz packages list found on the *mirrors*.
To list the current *wok* used by :program:`cook` (you don't need to be *root*):

.. code-block:: console

   $ cook list-wok

When creating the packages DB, :program:`cook` will check if you have a *flavors* repo in :file:`/home/slitaz/flavors`, if so, it will pack all *flavors* using the latest packages list available.
To create a packages list and the Live *flavors* files:

.. code-block:: console

   # cook pkgdb


.. _cookutils cooker:

The Cooker
^^^^^^^^^^

The :program:`Cooker` is a Build Bot, its first function is to check for commits in a *wok*, create an ordered cooklist and *cook* all modified packages.
It can also be used as a frontend to :program:`cook` since they both use the same files.
The :program:`Cooker` can also be used to *cook* a big list of packages at once such as all the packages in a *flavor*.
The :program:`Cooker` provides a nice CGI/Web interface that works by default on any SliTaz system since it provides CGI support via the :program:`Busybox` :program:`httpd` web server.

The :program:`Cooker` provides a small built-in help usage and short command switch.
For example to display usage you can use:

.. code-block:: console

   # cooker usage
   # cooker -u


Cooker setup
^^^^^^^^^^^^

Like :program:`cook`, the :program:`Cooker` needs a working environment before starting to use it.
The main difference with the :program:`cook` environment is that the :program:`Cooker` needs 2 *woks*.
One Hg and clean *wok* as a reference and one build *wok*.
In this way it is easy to compare both *woks* and get modifications.
If you already have a *cook* environment, you must move your *wok* before setting up the :program:`Cooker` or it will complain.
Setup will also install a set of development packages that can be configured in the :file:`cook.conf` configuration file and the variable ``SETUP_PKGS``.
To setup your :program:`cooker` environment:

.. code-block:: console

   # cooker setup

If all went well you now have 2 *woks*, base development packages installed and all needed files created.
The default behavior is to check for commits, you can run a test:

.. code-block:: console

   # cooker


Cooker cook
^^^^^^^^^^^

Again, 2 ways to work now: make changes in the clean Hg *wok* and launch the :program:`cooker` without any arguments or *cook* packages manually.
The :program:`cooker` lets you *cook* a single package or all packages of a category or a *flavor*.
You can also try to build all unbuilt packages, but be aware the :program:`Cooker` was not designed to handle thousands of packages.

To *cook* a single package which is the same as :command:`cook pkgname` but with more logs:

.. code-block:: console

   # cooker pkg pkgname

To *cook* more than one package at once you have different kind of choices.
You can use an existing package such as used for Live *flavors*, you can also use a custom list using the package names listed line by line.
Finally you can build all packages of a category.

.. code-block:: console

   # cooker flavor [name]
   # cooker list [/path/to/cooklist]
   # cooker cat [category]

The :program:`Cooker` lets you also *recook* a specific Hg revision.
It's useful in production so that if the Build Bot was interrupted while *cooking* commits, you can then *cook* packages manually:

.. code-block:: console

   # cooker rev 9496


.. _cookutils blocked:

Blocked packages
^^^^^^^^^^^^^^^^

:program:`Cook` and the :program:`Cooker` handle a file with a list of blocked package so they will not *cook* when commits happen or if a cooklist is used.
This is very useful for a :program:`Cooker` Build Bot in production.
When you block or unblock a package you can add a note to the cooknotes.
Blocking packages example:

.. code-block:: console

   # cook pkgname --block
   # cooker block pkgname
   # cooker -n "Blocked pkgname note"

The list of blocked packages are also displayed on the :program:`Cooker` web interface.
To unblock a package you have to use the :command:`unblock` command or cook ``--unblock`` option:

.. code-block:: console

   # cook pkgname --unblock
   # cooker unblock pkgname


Cooker CGI/Web
^^^^^^^^^^^^^^

To let you view log files in a nice way, keep trace of activity and help find errors, you can use the :program:`Cooker` Web interface located by default in the folder :file:`/var/www/cooker`.
If you don't use a chroot and the :program:`Busybox` :program:`httpd` web server is running, the web interface will work without configuration and should be reachable at: http://localhost/cooker/cooker.cgi

If you used a chroot environment, you should also install :program:`cookutils` on your host and modify the ``SLITAZ`` path variable.
A standard working way is to have a chroot in: :file:`/home/slitaz/cooking/chroot`

With :file:`/etc/slitaz/cook.conf` modified as below:

.. code-block:: shell

   SLITAZ="/home/slitaz/cooking/chroot/home/slitaz"

.. note::
   It's not obligatory to install the :program:`cookutils` on your host to use the web interface.
   If you use :program:`Lighttpd` you can also copy the :file:`cooker.cgi` and :file:`style.css` files for example into your :file:`~/Public` directory and use a custom :file:`cook.conf` with it.
   The advantage of installing :program:`cookutils` on the host is to get regular updates via the :program:`Tazpkg` packages manager.
   Say you have cloned or downloaded the :program:`cookutils`:

   .. code-block:: console

      $ cp -a cookutils/web ~/Public/cgi-bin/cooker
      $ cp -f cookutils/cook.conf ~/Public/cgi-bin/cooker

   Edit the configuration file: :file:`~/Public/cgi-bin/cooker/cook.conf` to set your ``SLITAZ`` path and you're all done!


Cooknotes
^^^^^^^^^

The cooknotes feature lets you write small personal notes about packaging and is useful for collaboration.
The cooknotes was coded to let the SliTaz :program:`Cooker` bot maintainers share notes between themselves and other contributors.
The :program:`Cooker` can block a package's build or *recook* packages manually, for example it's nice to make a note if a package is blocked so that the maintainer knows why admin did that.
Cooknotes are displayed on the web interface and can be checked from a cmdline:

.. code-block:: console

   # cooker note "Blocked pkgname due to heavy CPU load"
   # cooker notes


Cooker as a Build Bot
^^^^^^^^^^^^^^^^^^^^^

The :program:`Cooker` is designed to be a Built Bot for SliTaz, this means it monitors 2 *woks*, updates the Hg *wok*, gets the differences and *cooks* all packages that have been committed.
The safer and cleaner way to run the :program:`Cooker` as a Build Bot with :program:`cron` is to use a chroot environment, but it can run directly on the host if you want.

To run The :program:`Cooker` automatically you must use :program:`cron` from the chroot and add a single line to root :file:`crontabs` in :file:`/var/spool/cron/crontabs`.
Say you would like to run the :program:`Cooker` every 2 hours::

  * */2 * * * /usr/bin/cooker


Cooker BB started at boot
^^^^^^^^^^^^^^^^^^^^^^^^^

The :program:`Cooker` environment and :program:`cron` task can automatically be started at boot time.
You must have the :program:`cookutils-daemon` installed on the host and use a standard SliTaz installation to make it work properly (*cooking* goes in :file:`/home/slitaz/cooking`).
The daemon script will mount any virtual filesystems if needed as well as source and packages.
Source files are in :file:`/home/slitaz/src` and bound into the chroot so you can share package's sources between several versions (*stable*, *cooking*, *undigest*).
If the package is not yet installed:

.. code-block:: console

   # tazpkg get-install cookutils-daemon

To start the daemon you must have a :program:`cron` file definition for root in the chroot, the daemon script works like all other system daemons and can be handled with:

.. code-block:: console

   # /etc/init.d/cooker [start|stop|restart]
