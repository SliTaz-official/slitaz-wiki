.. http://doc.slitaz.org/en:scratchbook:base-ncurses
.. en/scratchbook/base-ncurses.txt · Last modified: 2011/04/23 23:05 by domcox

.. _scratchbook base ncurses:

Ncurses libraries and applications
==================================

Installation and configuration of ncurses libraries and applications.


About
-----

This chapter describes the construction and installation of some ncurses applications and libraries in SliTaz.
The procedure consists of moving into the :file:`/src` directory, downloading the sources for the application in question, unpacking, reading the README or INSTALL file(s), compiling and installing the binary in SliTaz.
Once the applications are installed, we can create a new initramfs, copy it to the root of the CD-ROM and generate a new ISO image.
For this you can also use :program:`mktaziso` in :ref:`cookbook slitaztools`.


Assign an environment variable (``$fs``)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

An environmental variable can't specify the path to the directory, just the name of the directory.
We will affect a variable ``$fs`` to indicate the path to the root filesystem (rootfs).
To do this, we venture into the working directory :file:`SliTaz/`, and type:

.. code-block:: console

   # export fs=$PWD/rootfs

To check:

.. code-block:: console

   # echo $fs


ncurses-5.6 — Terminal utilities and libraries
----------------------------------------------

:program:`ncurses` (`dickey.his.com/ncurses/ <http://dickey.his.com/ncurses/>`_) contains functions to display text in different ways on the screen of a Linux terminal and also provides the terminfo file.
Ncurses libraries are used among others by :program:`retawq`, :program:`nano` and some games.
We install the libraries in :file:`/lib` and the rest in :file:`/usr/bin` with a small :command:`strip` to clean the executables:

.. code-block:: console

   # cd src
   # wget ftp://invisible-island.net/ncurses/ncurses-5.6.tar.gz
   # tar xzf ncurses-5.6.tar.gz
   # cd ncurses-5.6
   # ./configure --prefix=/usr \
     --libdir=/lib --sysconfdir=/etc \
     --infodir=/usr/share/info --mandir=/usr/share/man \
     --with-shared --without-debug --without-ada
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/lib/*
   # strip -v _pkg/usr/bin/*

Copy the :file:`libncurses` library and some applications in SliTaz.
The :file:`reset` utility is used at the end of the rcS initialization script to remove the Linux logo.
If you wish, you can copy the associated utilities (:file:`tic`, :file:`tack`, :file:`toe`, etc), being careful not to delete the link to :program:`BusyBox`:

.. code-block:: console

   # cp -a _pkg/lib/libncurses.so* $fs/lib
   # cp -ia _pkg/usr/bin/{clear,ncurses5-config,tset,reset} \
     $fs/usr/bin

Copy :file:`terminfo` files, we only use a few files.
If you want more, you can copy:

.. code-block:: console

   # mkdir -v $fs/usr/share/terminfo
   # mkdir -v $fs/usr/share/terminfo/{a,l,r,v,x}
   # cp _pkg/usr/share/terminfo/a/ansi \
     $fs/usr/share/terminfo/a
   # cp _pkg/usr/share/terminfo/l/linux \
     $fs/usr/share/terminfo/l
   # cp _pkg/usr/share/terminfo/r/rxvt \
     $fs/usr/share/terminfo/r
   # cp _pkg/usr/share/terminfo/x/{xterm,xterm-color,xterm-new,xterm-vt220} \
     $fs/usr/share/terminfo/x
   # cp _pkg/usr/share/terminfo/v/{vt100,vt102*} \
     $fs/usr/share/terminfo/v

Copy the :file:`tabset` files:

.. code-block:: console

   # cp -a _pkg/usr/share/tabset $fs/usr/share


clex-3.16 — File Manager
------------------------

:program:`CLEX` (http://www.clex.sk/) is a small ncurses file manager (160 KB).
The configuration file (rc) is :file:`~/clexrc`; :file:`~/.clexbm` is used for bookmarks:

.. code-block:: console

   # cd ..
   # wget http://www.clex.sk/download/clex-3.16.tar.gz
   # tar xzf clex-3.16.tar.gz
   # cd clex-3.16
   # ./configure --prefix=/usr --infodir=/usr/share/info \
     --mandir=/usr/share/man
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/bin/clex

Install the :file:`clex` binary in the rootfs of SliTaz:

.. code-block:: console

   # cp _pkg/usr/bin/clex $fs/usr/bin


.. rubric:: libs

If we execute the :command:`ldd` command on :file:`clex`, the following dependancies should be displayed::

  libncurses.so.5 => /lib/libncurses.so.5 (0x40025000)
  libc.so.6 => /lib/libc.so.6 (0x40064000)
  /lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000)


nano-2.0.6 — Advanced Text Editor
---------------------------------

GNU :program:`nano` (`www.nano-editor.org <http://www.nano-editor.org/>`_) is a well known, fast, effective GNU/Linux text editor that supports colored syntax.
This is the default text editor in SliTaz:

.. code-block:: console

   # cd ..
   # wget http://www.nano-editor.org/dist/v2.0/nano-2.0.6.tar.gz
   # tar xzf nano-2.0.6.tar.gz
   # cd nano-2.0.6
   # ./configure --enable-all --enable-extra --prefix=/usr \
     --infodir=/usr/share/info --mandir=/usr/share/man \
     --sysconfdir=/etc
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/bin/nano

Copy the :file:`nano` binary and the :file:`rnano` link in SliTaz:

.. code-block:: console

   # cp -a _pkg/usr/bin/* $fs/usr/bin

Copy the configuration files in :file:`_pkg/usr/share/nano` to our rootfs:

.. code-block:: console

   # cp -a _pkg/usr/share/nano $fs/usr/share


.. rubric:: libs

If we execute the :command:`ldd` command on :file:`nano`, the following dependancies should be displayed::

  libncurses.so.5 => /lib/libncurses.so.5 (0x40025000)
  libc.so.6 => /lib/libc.so.6 (0x40064000)
  /lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000)


.. rubric:: locale

About the language, you can copy the :file:`.mo` files created when installing :program:`nano` from :file:`/usr/share/locale/(fr,en,de,es,etc)/LC_MESSAGES` to the rootfs.
Example for the French language:

.. code-block:: console

   # cp -a _pkg/usr/share/locale/fr/LC_MESSAGES $fs/usr/share/locale/fr


.. rubric:: Customize nano

You can customize :program:`nano` via :file:`/etc/nanorc` or :file:`~/.nanorc` for each user of the system.
It's in this file that you can define the colors used by :program:`nano` through the files in :file:`/usr/share/nano`.
You will find a broad example of this file in the archive of :program:`nano` and :ref:`cookbook slitaztools`.

For a system configuration file, you can copy the file in SliTaz tools to :file:`/etc` in the rootfs:

.. code-block:: console

   # cd ..
   # cp -a slitaz-tools-1.1/etc/nanorc $fs/etc


retawq-0.2.6c — Text mode Web browser
-------------------------------------

:program:`retawq` (`retawq.sourceforge.net <http://retawq.sourceforge.net/>`_) is a small text-only web browser.
We only flag a few useful options when configuring, :program:`retawq` needs terminfo files, libncurses libraries and libthread:

.. code-block:: console

   # wget http://switch.dl.sourceforge.net/sourceforge/retawq/retawq-0.2.6c.tar.gz
   # tar xzf retawq-0.2.6c.tar.gz
   # cd retawq-0.2.6c
   # ./configure --enable-i18n --enable-local-cgi --path-prefix=/usr \
     --path-doc=/usr/share/doc/retawq --path-man=/usr/share/man
   # make
   # strip -v retawq

Copy the :file:`retawq` binary in SliTaz:

.. code-block:: console

   # cp retawq $fs/usr/bin


.. rubric:: libs

::

  libncurses.so.5 => /lib/libncurses.so.5 (0x40025000)
  libpthread.so.0 => /lib/libpthread.so.0 (0x40064000)
  libc.so.6 => /lib/libc.so.6 (0x40074000)
  /lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000)

You can copy the lipthread library from your host system or use the minimum package glibc-2.3.6 distributed by SliTaz:

.. code-block:: console

   # cp -a /lib/libpthread* $fs/lib
   # strip --strip-unneeded $fs/lib/*


.. rubric:: locale

For language, you can copy the :file:`.mo` files in :file:`/i18n` of the :program:`retawq` archive to :file:`/usr/share/locale/(fr,en,es,etc)/LC_MESSAGES`.
Example for the French language, renaming the file to :file:`retawq.mo`:

.. code-block:: console

   # cp -v i18n/fr.mo $fs/usr/share/locale/fr/LC_MESSAGES/retawq.mo


.. rubric:: Customize retawq

To personalize :program:`retawq`, you can use a :file:`~/.retawq` directory containing a config file.
You can also save bookmarks (html) in the root directory of the user.
You will find an :file:`examples/` in the archive of :program:`retawq` (or SliTaz tools) containing a :file:`bookmarks.html` page with a list of favorite web sites.
You can also copy the docs (:file:`/documents`) from :program:`retawq` to :file:`/usr/share/doc/retawq`.


htop-6.0.5 — System process viewer
----------------------------------

:program:`htop` (`htop.sourceforge.net/ <http://htop.sourceforge.net/>`_) is software that displays system processes using ncurses.

Returning to the :file:`/src` directory, download, unpack, configure, compile and clean (with :command:`strip`):

.. code-block:: console

   # cd ..
   # wget http://switch.dl.sourceforge.net/sourceforge/htop/htop-0.6.5.tar.gz
   # tar xzf htop-0.6.5.tar.gz
   # cd htop-0.6.5
   # ./configure --prefix=/usr --mandir=/usr/share/man
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/bin/htop

Copy the :file:`htop` binary in SliTaz:

.. code-block:: console

   # cp _pkg/usr/bin/htop $fs/usr/bin

You can still copy the :program:`htop` icon found in: :file:`_pkg/usr/share/pixmaps`.


.. rubric:: libs

::

  libm.so.6 => /lib/libm.so.6 (0xb7f97000)
  libncurses.so.5 => /lib/libncurses.so.5 (0xb7f55000)
  libc.so.6 => /lib/libc.so.6 (0xb7e20000)
  /lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0xb7fc9000)


dialog-1.1.20070409 — GUI shell scripts
---------------------------------------

:program:`dialog` (`invisible-island.net/dialog/dialog.html <http://invisible-island.net/dialog/dialog.html>`_), is a utility to build GUI-based consoles:

.. code-block:: console

   # cd ..
   # wget ftp://invisible-island.net/dialog/dialog.tar.gz
   # tar xzf dialog.tar.gz
   # cd dialog-1.1-20070409
   # ./configure --enable-nls --with-ncurses --prefix=/usr \
     --sysconfdir=/etc --mandir=/usr/share/man
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/bin/dialog

Copy :file:`dialog` binary in SliTaz:

.. code-block:: console

   # cp _pkg/usr/bin/dialog $fs/usr/bin


.. rubric:: libs

::

  libncurses.so.5 => /lib/libncurses.so.5 (0x40027000)
  libm.so.6 => /lib/libm.so.6 (0x40066000)
  libc.so.6 => /lib/libc.so.6 (0x40089000)
  /lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000)


.. rubric:: locale

You can install locale files if you wish:

.. code-block:: console

   # cp -a _pkg/usr/share/locale/fr $fs/usr/share/locale

The :program:`dialog` configuration file is :file:`/etc/dialogrc` and/or :file:`~/.dialogrc` for each user.
There are also full examples of scripts in the :file:`/sample` directory in the sources of :program:`dialog`.


Ninvaders-0.1.1 — Space Invaders clone
--------------------------------------

:program:`ninvaders` (http://ninvaders.sourceforge.net/) is a clone of the popular :program:`Space Invaders` game (46 KB).
We begin by placing ourselves in the :file:`/src` directory, then we download, untar, compile, clean using :command:`strip` and copy the :file:`nInvaders` binary in :file:`/usr/games` of SliTaz:

.. code-block:: console

   # cd ..
   # wget http://ovh.dl.sourceforge.net/sourceforge/ninvaders/ninvaders-0.1.1.tar.gz
   # tar xzf ninvaders-0.1.1.tar.gz
   # cd ninvaders-0.1.1
   # make
   # strip -v nInvaders
   # cp nInvaders $fs/usr/games


bastet-0.41 — Bastard Tetris clone
----------------------------------

A game of Tetris (17 KB):

.. code-block:: console

   # wget http://fph.altervista.org/prog/bastet-0.41.tgz
   # tar xzf bastet-0.41.tgz
   # cd bastet-0.41
   # make
   # strip bastet
   # cp bastet $fs/usr/games
   # mkdir -p $fs/var/games
   # touch $fs/var/games/bastet.scores
   # chmod 666 $fs/var/games/bastet.scores


rhapsody-0.28b — IRC chat client
--------------------------------

:program:`Rhapsody` (http://rhapsody.sourceforge.net/) is a fast and lightweight chat client supporting the IRC protocol.
It provides a menu for managing servers, channels and configuration.
It is therefore easy to use:

.. code-block:: console

   # cd ..
   # wget http://switch.dl.sourceforge.net/sourceforge/rhapsody/rhapsody_0.28b.tgz
   # tar xzf rhapsody_0.28b.tgz
   # cd rhapsody-0.28b
   # ./configure -i /usr/bin -d /usr/share/doc/rhapsody
   # make
   # strip -v rhapsody

Install the binary and help files in SliTaz.
We must adjust permissions on these files so that everyone can read:

.. code-block:: console

   # cp rhapsody $fs/usr/bin
   # mkdir $fs/usr/share/doc/rhapsody
   # cp -a help $fs/usr/share/doc/rhapsody/help
   # chmod 644 $fs/usr/share/doc/rhapsody/help/*


.. rubric:: libs

:program:`Rhapsody` uses the following libraries::

  libncurses.so.5 => /lib/libncurses.so.5 (0x40026000)
  libc.so.6 => /lib/libc.so.6 (0x40066000)
  /lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000))


.. rubric:: Rhapsody use the following libraries:

You can customize :program:`rhapsody` via :file:`~/.rhapsodyrc` or use :kbd:`Ctrl`\ +\ :kbd:`T` for options:


Generate the initramfs and an ISO image
---------------------------------------

To create a new ISO image, you can use :command:`mktaziso` in SliTaz tools.
Or you can create a new initramfs image, copy it to :file:`/boot` in the root of the CD-ROM (rootcd) and finally generate an ISO image with :command:`genisoimage`:

.. code-block:: console

   # cd $fs
   # find . -print | cpio -o -H newc | gzip -9 > ../rootfs.gz
   # cd ..
   # cp rootfs.gz rootcd/boot
   # genisoimage -R -o slitaz-cooking.iso -b boot/isolinux/isolinux.bin \
     -c boot/isolinux/boot.cat -no-emul-boot -boot-load-size 4 \
     -V "SliTaz" -input-charset iso8859-1 -boot-info-table rootcd


.. rubric:: Following chapter

The next chapter describes the installation of the :ref:`locales <scratchbook locale>` and i18n.
