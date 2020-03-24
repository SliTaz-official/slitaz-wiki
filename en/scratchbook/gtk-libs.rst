.. http://doc.slitaz.org/en:scratchbook:gtk-libs
.. en/scratchbook/gtk-libs.txt · Last modified: 2011/04/23 23:19 by domcox

.. _scratchbook gtk libs:

GTK+ Libraries
==============

:author: domcox

Compilation and installation of GTK+ packages and libraries.


About
-----

This chapter describes the installation and configuration of GTK libraries on SliTaz used by lots of free software.
Note that you can simply compile and create a SliTaz package that you can install on demand with :program:`tazpkg`.

The compilation of GTK applications can take quite some time and you must meet many dependencies.
You will find a guide in English: `gtk-building.html <http://developer.gnome.org/gtk/2.22/gtk-building.html>`_ on developer.gnome.org.
This document sets out the need to compile things in order: Glib, Pango, ATK and GTK+, etc.
Before commencing you must verify that the dependencies are properly installed on your host system.
Glib, Pango, ATK and GTK+ form a group of packages and are distributed by the team of GTK developers.


.. rubric:: Environmental variable (``$fs``)

If you do not specify any path to the rootfs directory, export the environmental variable:

.. code-block:: console

   # export fs=$PWD/rootfs

To check:

.. code-block:: console

   # echo $fs


cairo-1.2.6 — 2D graphics library
---------------------------------

We begin with libcairo (http://www.cairographics.org/) used to compile pango:

.. code-block:: console

   # cd src
   # wget http://cairographics.org/releases/cairo-1.2.6.tar.gz
   # tar xzf cairo-1.2.6.tar.gz
   # cd cairo-1.2.6
   # ./configure --prefix=/usr --mandir=/usr/share/man \
     --with-html-dir=/usr/share/doc
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/lib/*.so*


.. rubric:: Install in rootfs

.. code-block:: console

   # cp -av _pkg/usr/lib/*.so* $fs/usr/lib


glib-2.12.4 — C routines
------------------------

.. code-block:: console

   # cd ..
   # wget ftp://ftp.gtk.org/pub/glib/2.12/glib-2.12.4.tar.bz2
   # tar xjf glib-2.12.4.tar.bz2
   # cd glib-2.12.4
   # ./configure --prefix=/usr --sysconfdir=/etc \
     --mandir=/usr/share/man --with-html-dir=/usr/share/doc
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/bin/*
   # strip -v _pkg/usr/lib/*.so*


.. rubric:: Install in rootfs

Option: the utilities glib-genmarshal and gobject-query need the :file:`/lib/tls/librt.so.1`:

.. code-block:: console

   # cp -a _pkg/usr/lib/*.so* $fs/usr/lib
   # cp -a _pkg/usr/share/locale/fr $fs/usr/share/locale

The binaries and options:

.. code-block:: console

   # cp -a _pkg/usr/bin/* $fs/usr/bin


pango-1.14.8 — Library for layout and rendering of text
-------------------------------------------------------

.. code-block:: console

   # cd ..
   # wget ftp://ftp.gtk.org/pub/pango/1.14/pango-1.14.8.tar.bz2
   # tar xjf pango-1.14.8.tar.bz2
   # cd pango-1.14.8
   # ./configure --prefix=/usr --sysconfdir=/etc \
     --mandir=/usr/share/man --with-html-dir=/usr/share/doc
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/bin/*
   # strip -v _pkg/usr/lib/*.so*
   # strip -v _pkg/usr/lib/pango/1.5.0/modules/*


.. rubric:: Install in rootfs

.. code-block:: console

   # cp -a _pkg/usr/bin/* $fs/usr/bin
   # cp -a _pkg/usr/lib/*.so* $fs/usr/lib
   # cp -a _pkg/usr/lib/pango $fs/usr/lib
   # rm -rf $fs/usr/lib/pango/1.5.0/modules/*.la
   # cp -a _pkg/etc $fs

Create :file:`/etc/pango.modules` via chroot in the rootfs (pango-querymodules uses :file:`librt.so.1`):

.. code-block:: console

   # chroot $fs /bin/ash
   /# pango-querymodules > /etc/pango/pango.modules
   # exit


atk-1.12.4 — Accessibility toolkit
----------------------------------

.. code-block:: console

   # cd ..
   # wget http://ftp.gnome.org/pub/gnome/sources/atk/1.12/atk-1.12.4.tar.bz2
   # tar xjf atk-1.12.4.tar.bz2
   # cd atk-1.12.4
   # ./configure --prefix=/usr --mandir=/usr/share/man \
     --with-html-dir=/usr/share/doc
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/lib/*.so*


.. rubric:: Install in rootfs

.. code-block:: console

   # cp -a _pkg/usr/lib/*.so* $fs/usr/lib
   # cp -a _pkg/usr/share/locale/fr $fs/usr/share/locale


gtk+-2.8.20 — The GIMP Toolkit
------------------------------

.. code-block:: console

   # cd ..
   # wget ftp://ftp.gtk.org/pub/gtk/v2.8/gtk+-2.8.20.tar.bz2
   # tar xjf gtk+-2.8.20.tar.bz2
   # cd gtk+-2.8.20
   # ./configure --prefix=/usr --sysconfdir=/etc \
     --mandir=/usr/share/man --with-html-dir=/usr/share/doc
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/usr/bin/*
   # strip -v _pkg/usr/lib/*.so*
   # strip -v --strip-unneeded \
     _pkg/usr/lib/gtk-2.0/2.4.0/*/*


.. rubric:: Install in rootfs

.. code-block:: console

   # cp -a _pkg/usr/lib/*.so* $fs/usr/lib
   # mkdir $fs/usr/lib/gtk-2.0
   # cp -a _pkg/usr/lib/gtk-2.0/2.4.0 $fs/usr/lib/gtk-2.0
   # rm -rf $fs/usr/lib/gtk-2.0/2.4.0/*/*.la

Locale and themes:

.. code-block:: console

   # cp -a _pkg/usr/share/locale/fr $fs/usr/share/locale
   # cp -a _pkg/usr/share/themes $fs/usr/share

The applications:

.. code-block:: console

   # cp  _pkg/usr/bin/gtk-query-immodules-2.0 $fs/usr/bin
   # cp  _pkg/usr/bin/gtk-update-icon-cache $fs/usr/bin
   # cp  _pkg/usr/bin/gdk-pixbuf-csource $fs/usr/bin
   # cp  _pkg/usr/bin/gdk-pixbuf-query-loaders $fs/usr/bin
   ...

For the gtk-demo application:

.. code-block:: console

   # cp -a _pkg/usr/bin/gtk-demo $fs/usr/bin
   # cp -a _pkg/usr/share/gtk-2.0 $fs/usr/share

Create files :file:`/etc/gtk-2.0/gtk.immodules` and :file:`gdk-pixbuf.loaders` via a chroot in the rootfs:

.. code-block:: console

   # chroot $fs /bin/ash
   /# mkdir /etc/gtk-2.0
   /# gtk-query-immodules-2.0 > /etc/gtk-2.0/gtk.immodules
   /# gdk-pixbuf-query-loaders > /etc/gtk-2.0/gdk-pixbuf.loaders
   # exit

At this stage you can test GTK+ with the :program:`gtk-demo` application by creating an ISO and using :program:`qemu`.
You can also compile a small GTK application such as :program:`LeafPad` and test!
The compiliation and installation of GTK+ applications distributed by default with SliTaz are described in the next chapter :ref:`scratchbook gtk apps`.


Generate the initramfs and an ISO image
---------------------------------------

To create a new ISO image, you can use :program:`mktaziso` in :ref:`cookbook slitaztools`.
Or you can create a new initramfs image, copy it to :file:`/boot` in the root of the CD-ROM (rootcd) and finally generate an ISO image with :program:`genisoimage`:

.. code-block:: console

   # cd $fs
   # find . -print | cpio -o -H newc | gzip -9 > ../rootfs.gz
   # cd ..
   # cp rootfs.gz rootcd/boot
   # genisoimage -R -o slitaz-cooking.iso -b boot/isolinux/isolinux.bin \
     -c boot/isolinux/boot.cat -no-emul-boot -boot-load-size 4 \
     -V "SliTaz" -boot-info-table rootcd


.. rubric:: Following chapter

After the libraries, the :ref:`scratchbook gtk apps`.
