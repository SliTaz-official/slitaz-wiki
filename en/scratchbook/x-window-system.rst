.. http://doc.slitaz.org/en:scratchbook:x-window-system
.. en/scratchbook/x-window-system.txt · Last modified: 2011/04/23 23:13 by domcox

.. _scratchbook x window system:

X window system
===============

:author: domcox

Installation and basic configuration of the X window system.


About
-----

This chapter describes the installation and configuration of the X window system on SliTaz.
We will install libraries for expat, XML, fonts, a graphical server (Xvesa), a terminal emulator (xterm), various small tools and a window manager (JWM).
We'll also install the JPEG libraries and Links web browser.


.. rubric:: Environmental variable (``$fs``)

If you do not specify any path to the rootfs directory, export the environmental variable:

.. code-block:: console

   # export fs=$PWD/rootfs

To check:

.. code-block:: console

   # echo $fs


expat-2.0.0 — XML parser library
--------------------------------

Expat (http://expat.sourceforge.net/) contains the XML parsing libraries:

.. code-block:: console

   # cd ..
   # wget http://switch.dl.sourceforge.net/sourceforge/expat/expat-2.0.0.tar.gz
   # tar xzf expat-2.0.0.tar.gz
   # cd expat-2.0.0
   # ./configure --sysconfdir=/etc --prefix=/usr \
     --mandir=/usr/share/man
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/lib/*
   # strip -v _pkg/usr/bin/*

Thereafter, we will install 'xterm' which needs libexpat.so.0, simply create a symbolic link and voilà.
Then you can install the 'xmlwf' application and libraries in the rootfs:

.. code-block:: console

   # cd _pkg/usr/lib
   # ln -s libexpat.so.1.5.0 libexpat.so.0
   # cp -a *.so* $fs/usr/lib
   # cd ..
   # cp -a bin/* $fs/usr/bin
   # cd ../..


.. rubric:: libs

Libraries used by xmlwf::

  libexpat.so.1 => /usr/lib/libexpat.so.1 (0x40021000)
  libc.so.6 => /lib/tls/libc.so.6 (0x40041000)
  /lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000)


freetype-2.3.1 — System font libraries
--------------------------------------

The package freetype (http://www.freetype.org/) contains libraries used by X for configuring the system fonts:

.. code-block:: console

   # cd ..
   # wget http://download.savannah.gnu.org/releases/freetype/freetype-2.3.1.tar.bz2
   # tar xjf freetype-2.3.1.tar.bz2
   # cd freetype-2.3.1
   # ./configure --sysconfdir=/etc --prefix=/usr \
     --mandir=/usr/share/man
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -vs _pkg/usr/lib/*
   # cp -a _pkg/usr/bin/* $fs/usr/bin
   # cp -a _pkg/usr/lib/*.so* $fs/usr/lib


fontconfig-2.4.2 — Manage system fonts
--------------------------------------

The fontconfig package (`www.fontconfig.org/wiki/ <http://www.fontconfig.org/wiki/>`_) provides the libfontconfig library used by many programs under X.
Note XFree86 also provides these utilities.
We chose the original package because it works better with JWM:

.. code-block:: console

   # cd ..
   # wget http://fontconfig.org/release/fontconfig-2.4.2.tar.gz
   # tar xzf fontconfig-2.4.2.tar.gz
   # cd fontconfig-2.4.2
   # ./configure --sysconfdir=/etc --prefix=/usr \
     --mandir=/usr/share/man --localstatedir=/var
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/bin/*
   # strip -v _pkg/usr/lib/*
   # cp -a _pkg/usr/bin/* $fs/usr/bin
   # cp -a _pkg/usr/lib/*.so* $fs/usr/lib
   # cp -a _pkg/etc $fs
   # cp -a _pkg/var $fs


.. rubric:: libs

A 'ldd' on fc-cache gives the libraries below.
You can also use libfreetype of XFree86::

  libfreetype.so.6 => /usr/lib/libfreetype.so.6 (0xb7f12000)
  libz.so.1 => /usr/lib/libz.so.1 (0xb7eff000)
  libexpat.so.1 => /usr/lib/libexpat.so.1 (0xb7edf000)
  libfontconfig.so.1 => /usr/lib/libfontconfig.so.1 (0xb7eb0000)
  libc.so.6 => /lib/tls/libc.so.6 (0xb7d7b000)
  /lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0xb7f8c000)


Xserver — Graphical Xvesa server of Xfree86
-------------------------------------------

We will use the binary versions of Xvesa server (`www.xfree86.org/ <http://www.xfree86.org/>`_) and fonts distributed by Xfree86.org.
We could also copy Xorg libraries from the host system that would be used for compiling X applications.
Xvesa works well like this and simplifies things.
You can also rebuild :ref:`Xorg packages <scratchbook xorg>` on your development system.


.. rubric:: Xtinyx server — Xvesa

The Xvesa server is very light and uses tiny libraries; it is contained in the :file:`Xtinyx.tgz` archive.
Download and install in :file:`/usr/bin` of SliTaz rootfs:

.. code-block:: console

   # cd ..
   # mkdir -p XFree86-4.6.0 && cd XFree86-4.6.0
   # wget http://ftp.xfree86.org/pub/XFree86/4.6.0/binaries/Linux-ix86-glibc23/Xtinyx.tgz
   # tar xzf Xtinyx.tgz
   # cp bin/Xvesa $fs/usr/bin
   # strip $fs/usr/bin/Xvesa
   # chmod 4711 $fs/usr/bin/Xvesa


.. rubric:: libs for Xvesa

::

  libz.so.1 => /usr/lib/libz.so.1 (0xb7ed6000)
  libm.so.6 => /lib/tls/libm.so.6 (0xb7eb1000)
  libc.so.6 => /lib/tls/libc.so.6 (0xb7d7e000)
  /lib/ld-linux.so.2 (0xb7ef8000)


.. rubric:: rgb.txt — RGB colors in X

The colors configuration file used by the X server is called: :file:`rgb.txt`; we suggest that you copy it to the host system.
The library :file:`libX11.so` will seek the configuration files in :file:`/usr/share/X11`, and the Xvesa server in :file:`/usr/X11R6/lib/X11`; we create a link in :file:`/usr/share/X11` to enable this:

.. code-block:: console

   # mkdir -p $fs/usr/share/X11
   # cp /usr/share/X11/rgb.txt $fs/usr/share/X11
   # chroot $fs /bin/ash
   # mkdir -p /usr/X11R6/lib/X11/
   # ln -s /usr/share/X11/rgb.txt /usr/X11R6/lib/X11/rgb.txt
   # exit


.. rubric:: Xfnts — Fonts

To operate the server, we need the basic fonts; you can download them from Xfree86.org and then compile packages from Xorg, or copy them from your host system.
The system fonts can be put into different folders and the cache updated with :command:`lc-cache`.
Attention, fonts take pride of place and you can only install the minimum.
:file:`/usr/share/fonts` contains the TrueType fonts such as bitstream-vera:

.. code-block:: console

   # wget http://ftp.xfree86.org/pub/XFree86/4.6.0/binaries/Linux-ix86-glibc23/Xfnts.tgz
   # tar xzf Xfnts.tgz
   # mkdir -p $fs/usr/X11R6/lib/X11/fonts
   # mkdir -p $fs/usr/share/fonts/truetype

Copy the fonts…

.. code-block:: console

   # cp -a lib/X11/fonts/* $fs/usr/X11R6/lib/X11/fonts)
   # cp -a /usr/share/fonts/truetype/* $fs/usr/share/fonts/truetype

Then regenerate the :file:`fonts.dir` file, you must run :command:`mkfontdir` on the directory in question:

.. code-block:: console

   # mkfontdir $fs/usr/X11R6/lib/X11/fonts/75dpi

Fontconfig configuration files can be found in :file:`/etc/fonts` provided by the :program:`fontconfig` package.
Now you can run :command:`fc-cache` to update the cache, and :command:`fc-list` for a list of fonts.
You do this by chrooting into the rootfs:

.. code-block:: console

   # chroot $fs /bin/ash
   # fc-cache -v
   # fc-list
   # exit


.. rubric:: Xlib locale — Localization files

On SliTaz, we installed 4 locales: ``C``, ``iso8859-1``, ``iso8859-15`` and ``iso8859-2`` from the :ref:`compilation of Xorg <scratchbook xorg>`.
You can copy these files from the host system or use the files distributed by XFree86.
Sample copy of all the locales from the host system:

.. code-block:: console

   # mkdir -p $fs/usr/share/X11/locale
   # cp -a /usr/share/X11/locale/* $fs/usr/share/X11/locale


.. rubric:: Using X

Note that you can already use Xvesa as a X terminal if you have a machine on the network accepting XDMCP connections.
For this, you can start the server with the ``-query`` option.
For example:

.. code-block:: console

   # Xvesa -ac -shadow -screen 1024x768x24 -query 192.168.0.2


xterm — Terminal Emulator
-------------------------

The :program:`xterm` package (`invisible-island.net/xterm/ <http://invisible-island.net/xterm/>`_) provides a terminal emulator for X:

.. code-block:: console

   # wget ftp://invisible-island.net/xterm/xterm-223.tgz
   # tar xzf xterm-223.tgz
   # cd xterm-223
   # ./configure --prefix=/usr --sysconfdir=/etc \
     --mandir=/usr/share/man --localstatedir=/var \
     --with-app-defaults=/usr/share/X11/app-defaults \
     --build=i486-pc-linux-gnu --host=i486-pc-linux-gnu
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip _pkg/usr/bin/*
   # cp _pkg/usr/bin/* $fs/usr/bin
   # cp -a _pkg/usr/share/X11/* $fs/usr/share/X11


.. rubric:: libs

A :command:`ldd` on XTerm, we copy (and :command:`strip`) the missing libraries from the host system::

  libXft.so.2 => /usr/lib/libXft.so.2 (0xb7f09000)
  libXrender.so.1 => /usr/lib/libXrender.so.1 (0xb7f00000)
  libfontconfig.so.1 => /usr/lib/libfontconfig.so.1 (0xb7ed5000)
  libfreetype.so.6 => /usr/lib/libfreetype.so.6 (0xb7e68000)
  libz.so.1 => /usr/lib/libz.so.1 (0xb7e54000)
  libX11.so.6 => /usr/lib/libX11.so.6 (0xb7d68000)
  libXaw.so.7 => /usr/lib/libXaw.so.7 (0xb7d0f000)
  libXmu.so.6 => /usr/lib/libXmu.so.6 (0xb7cfa000)
  libXext.so.6 => /usr/lib/libXext.so.6 (0xb7cec000)
  libXt.so.6 => /usr/lib/libXt.so.6 (0xb7c9e000)
  libSM.so.6 => /usr/lib/libSM.so.6 (0xb7c96000)
  libICE.so.6 => /usr/lib/libICE.so.6 (0xb7c7f000)
  libncurses.so.5 => /lib/libncurses.so.5 (0xb7c3c000)
  libc.so.6 => /lib/libc.so.6 (0xb7b2c000)
  libexpat.so.1 => /usr/lib/libexpat.so.1 (0xb7b0b000)
  libXau.so.6 => /usr/lib/libXau.so.6 (0xb7b08000)
  libXdmcp.so.6 => /usr/lib/libXdmcp.so.6 (0xb7b03000)
  libdl.so.2 => /lib/libdl.so.2 (0xb7aff000)
  libXpm.so.4 => /usr/lib/libXpm.so.4 (0xb7aee000)


libpng-1.2.18 — PNG Libraries
-----------------------------

PNG libraries (http://libpng.org/pub/png/libpng.html) are used to manipulate and format PNG images:

.. code-block:: console

   # wget http://puzzle.dl.sourceforge.net/sourceforge/libpng/libpng-1.2.18.tar.bz2
   # tar xjf libpng-1.2.18.tar.bz2
   # cd libpng-1.2.18
   # ./configure --enable-shared --prefix=/usr \
     --mandir=/usr/share/man
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip _pkg/usr/lib/*.so*
   # cp -a _pkg/usr/lib/libpng12.so* $fs/usr/lib
   # cp -a _pkg/usr/bin/libpng12* $fs/usr/bin


jwm-2.0 — Window manager
------------------------

Joe's Window Manager (http://www.joewing.net/programs/jwm/) is an ultra light and friendly window manager.
This is the default SliTaz window manager.
The main configuration file :file:`/etc/jwm/system.jwmrc` includes the style and config menu:

.. code-block:: console

   # cd ..
   # wget http://www.joewing.net/programs/jwm/releases/jwm-2.0.tar.bz2
   # tar xjf jwm-2.0.tar.bz2
   # cd jwm-2.0
   # ./configure --prefix=/usr --mandir=/usr/share/man \
     --sysconfdir=/etc/jwm --disable-xinerama
   # make
   # strip src/jwm
   # cp src/jwm $fs/usr/bin
   # mkdir $fs/etc/jwm
   # cp example.jwmrc $fs/etc/jwm/system.jwmrc


.. rubric:: libs

:command:`ldd` libraries that we have provided::

  libX11.so.6 => /usr/lib/libX11.so.6 (0xb7e35000)
  libpng12.so.0 => /usr/lib/libpng12.so.0 (0xb7e12000)
  libXft.so.2 => /usr/lib/libXft.so.2 (0xb7e00000)
  libXrender.so.1 => /usr/lib/libXrender.so.1 (0xb7df7000)
  libfontconfig.so.1 => /usr/lib/libfontconfig.so.1 (0xb7dcc000)
  libfreetype.so.6 => /usr/lib/libfreetype.so.6 (0xb7d5f000)
  libz.so.1 => /usr/lib/libz.so.1 (0xb7d4a000)
  libXpm.so.4 => /usr/lib/libXpm.so.4 (0xb7d3a000)
  libXext.so.6 => /usr/lib/libXext.so.6 (0xb7d2c000)
  libc.so.6 => /lib/libc.so.6 (0xb7c1c000)
  libXau.so.6 => /usr/lib/libXau.so.6 (0xb7c19000)
  libXdmcp.so.6 => /usr/lib/libXdmcp.so.6 (0xb7c14000)
  libdl.so.2 => /lib/libdl.so.2 (0xb7c0f000)
  libm.so.6 => /lib/libm.so.6 (0xb7bea000)
  libexpat.so.1 => /usr/lib/libexpat.so.1 (0xb7bc9000)

You can start the X server and JWM with the command below or create a script in :file:`/usr/bin/startx` with the content:

.. code-block:: shell

   Xvesa -ac -shadow -screen 1024x768x24 & exec jwm


.. rubric:: On SliTaz

SliTaz uses the :file:`~/.Xsession` file to start a graphical session.
The :command:`startx` command checks whether the file exists or it runs :command:`tazx` to configure the X system.
The user guide on X window is located in :file:`/usr/share/doc/slitaz/user-guide/x-window.html` or is on the website:

We chose to use the Tango icons theme http://tango.freedesktop.org/, that isn't compiled.
We only use the minimum: images in 16×16 format that we put in :file:`/usr/share/icons`.

To test JWM with a cooking ISO:

.. code-block:: console

   # Xvesa -ac -shadow -screen 800x600x24 & exec jwm


jpeg-6b — JPEG Libraries
------------------------

Libraries handling JPEG images, and some small utilities:

.. code-block:: console

   # wget http://www.ijg.org/files/jpegsrc.v6b.tar.gz
   # tar xzf jpegsrc.v6b.tar.gz
   # cd jpeg-6b
   # ./configure --enable-shared --prefix=/usr \
     --mandir=/usr/share/man
   # make
   # strip .libs/*
   # cp -a .libs/*.so* $fs/usr/lib
   # cp .libs/{cjpeg,djpeg,jpegtran} $fs/usr/bin


tiff-3.8.2 — TIFF Libraries and Utilities
-----------------------------------------

Libraries handling TIFF images and some small optional utilities:

.. code-block:: console

   # wget ftp://ftp.remotesensing.org/pub/libtiff/tiff-3.8.2.tar.gz
   # tar xzf tiff-3.8.2.tar.gz
   # cd tiff-3.8.2
   # ./configure  --prefix=/usr --mandir=/usr/share/man
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip _pkg/usr/bin/*
   # strip _pkg/usr/lib/*.so*
   # cp -a _pkg/usr/lib/*.so* $fs/usr/lib

You can install the utilities you want.


links-2.1pre29 — Graphical and text mode web browser
----------------------------------------------------

Links (`links.twibright.com <http://links.twibright.com/>`_) is a web browser offering graphical and text modes.
It is translated into multiple languages, including French:

.. code-block:: console

   # cd ..
   # wget http://links.twibright.com/download/links-2.1pre28.tar.gz
   # tar xzf links-2.1pre28.tar.gz
   # cd links-2.1pre28
   # ./configure --prefix=/usr --sysconfdir=/etc --mandir=/usr/share/man \
     --without-directfb --without-ssl --enable-graphics --enable-javascript
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/bin/*
   # cp -v _pkg/usr/bin/* $fs/usr/bin


.. rubric:: libs

::

  libtiff.so.3 => /usr/lib/libtiff.so.3
  libjpeg.so.62 => /usr/lib/libjpeg.so.62 (0xb7ede000)
  libpng12.so.0 => /usr/lib/libpng12.so.0 (0xb7eba000)
  libz.so.1 => /usr/lib/libz.so.1 (0xb7ea7000)
  libX11.so.6 => /usr/lib/libX11.so.6 (0xb7dbb000)
  libdl.so.2 => /lib/tls/libdl.so.2 (0xb7db7000)
  libpcre.so.0 => /usr/lib/libpcre.so.0 (0xb7d96000)
  libm.so.6 => /lib/tls/libm.so.6 (0xb7d70000)
  libc.so.6 => /lib/tls/libc.so.6 (0xb7c3e000)
  libXau.so.6 => /usr/lib/libXau.so.6 (0xb7c3b000)
  libXdmcp.so.6 => /usr/lib/libXdmcp.so.6 (0xb7c36000)
  /lib/ld-linux.so.2 (0xb7f5d000)


Generate the initramfs and an ISO image
---------------------------------------

To create a new ISO image, you can use :command:`mktaziso` in ref:`cookbook slitaztools`.
Or you can create a new initramfs image, copy it to :file:`/boot` in the root of the CD-ROM (rootcd) and finally generate an ISO image with :command:`genisoimage`:

.. code-block:: console

   # cd $fs
   # find . -print | cpio -o -H newc | gzip -9 > ../rootfs.gz
   # cd ..
   # cp rootfs.gz rootcd/boot
   # genisoimage -R -o slitaz-cooking.iso -b boot/isolinux/isolinux.bin \
     -c boot/isolinux/boot.cat -no-emul-boot -boot-load-size 4 \
     -V "SliTaz" -boot-info-table rootcd


.. rubric:: Following chapter

The next chapter :ref:`scratchbook gtk libs` describes the installation of GTK libraries.
