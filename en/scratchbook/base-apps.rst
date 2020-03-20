.. http://doc.slitaz.org/en:scratchbook:base-apps
.. en/scratchbook/base-apps.txt · Last modified: 2011/04/23 23:02 by domcox

.. _scratchbook base apps:

Base Applications
=================

Install and configure libraries and basic applications.


About
-----

This chapter describes the facilities libraries and basic text mode applications supplied with SliTaz.


.. rubric:: Assign an environment variable (``$fs``)

An environmental variable can't specify the path to the directory, just the name of the directory.
We will affect a variable ``$fs`` to indicate the path to the root filesystem (rootfs).
To do this, we venture into the working directory :file:`SliTaz/`, and type:

.. code-block:: console

   # export fs=$PWD/rootfs

To check:

.. code-block:: console

   # echo $fs


bc-1.06 — Text mode calculator
------------------------------

The application :program:`bc` (`www.gnu.org/software/bc/ <http://www.gnu.org/software/bc/>`_) provides a small calculator.
When compiling the utility, :program:`dc` is also built, but not installed by SliTaz.
Note that :program:`dc` is also available with :program:`BusyBox`.
If you decide to copy :program:`dc`, you need to delete the link to :program:`BusyBox` (if it exists).
We use a directory :file:`_pkg` (package) for installation, use :program:`strip` to clean the executables and copy the utilities:

.. code-block:: console

   # cd src
   # wget http://ftp.gnu.org/pub/gnu/bc/bc-1.06.tar.gz
   # tar xzfv bc-1.06.tar.gz
   # cd bc-1.06
   # ./configure --prefix=/usr --infodir=/usr/share/info \
     --mandir=/usr/share/man
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -vs _pkg/usr/bin/*
   # cp -avi _pkg/usr/bin/bc $fs/usr/bin


.. rubric:: libs

A small :command:`ldd` on :file:`bc` should produce::

  libc.so.6 => /lib/libc.so.6 (0x40029000)
  /lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000)


zlib-1.2.3 — Compression libraries
----------------------------------

The :program:`zlib` (http://www.zlib.net/) package provides compression and decompression functions used by among others, the SSH server :program:`Dropbear` and the X server:

.. code-block:: console

   # cd ..
   # wget http://www.gzip.org/zlib/zlib-1.2.3.tar.bz2
   # tar xjfv zlib-1.2.3.tar.bz2
   # cd zlib-1.2.3
   # ./configure --shared --prefix=/usr
   # make
   # strip -vs libz.so*
   # cp -av libz.so* $fs/usr/lib


pcre-7.4 — Perl-compatible regular expressions
----------------------------------------------

The package :program:`pcre` (http://www.pcre.org/) provides libraries of functions for Perl compatible regular expressions used by among others, the web server :program:`Lighttpd`:

.. code-block:: console

   # cd ..
   # wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-7.4.tar.gz
   # tar xzfv pcre-7.4.tar.gz
   # cd pcre-7.4
   # ./configure --prefix=/usr
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -vs _pkg/usr/bin/*
   # strip -vs _pkg/usr/lib/*
   # cp -av _pkg/usr/bin/* $fs/usr/bin
   # cp -av _pkg/usr/lib/*.so* $fs/usr/lib


e2fsprogs-1.39 — Filesystem management utilities
------------------------------------------------

The :program:`e3fsprogs` (http://e2fsprogs.sourceforge.net/) provides utilities for handling ext2 and ext3 filesystems.
We will not take all of them because we need the space.
It should be noted that we use :program:`fsck` of :program:`BusyBox`:

.. code-block:: console

   # cd ..
   # wget http://puzzle.dl.sourceforge.net/sourceforge/e2fsprogs/e2fsprogs-1.39.tar.gz
   # tar xzf e2fsprogs-1.39.tar.gz
   # cd e2fsprogs-1.39
   # ./configure --prefix=/usr --with-root-prefix="" \
     --enable-elf-shlibs --disable-evms --sysconfdir=/etc \
     --infodir=/usr/share/info --mandir=/usr/share/man
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -vs _pkg/sbin/*
   # strip -vs _pkg/lib/*
   # strip -vs _pkg/usr/bin/*
   # strip -vs _pkg/usr/sbin/*
   # strip -vs _pkg/usr/lib/*

Install the utilities, configuration files and libraries in the rootfs of SliTaz.
Be careful if you used :program:`fsck`, that you didn't destroy the link to :program:`BusyBox`:

.. code-block:: console

   # cp -i _pkg/sbin/{badblocks,blkid,dumpe2fs,e2fsck,e2image} $fs/sbin
   # cp -i _pkg/sbin/{e2label,findfs,logsave,mke2fs,mkfs.*} $fs/sbin
   # cp -i _pkg/sbin/{resize2fs,tune2fs} $fs/sbin
   # cp -a _pkg/lib/* $fs/lib
   # rm -rf $fs/lib/libss*
   # cp -a _pkg/etc/* $fs/etc
   # cp -a _pkg/usr/bin/* $fs/usr/bin
   # cp -a _pkg/usr/sbin/* $fs/usr/sbin
   # cp -ad _pkg/usr/lib/*.so $fs/usr/lib
   # rm -rf $fs/usr/lib/libss*

You can also copy files from the French locale:

.. code-block:: console

   # mkdir $fs/usr/share/locale
   # cp -a _pkg/usr/share/locale/fr $fs/usr/share/locale


Dropbear-0.50 — Lightweight SSH client and server
-------------------------------------------------

:program:`Dropbear` (http://matt.ucc.asn.au/dropbear/dropbear.html) is a small secure client/server supporting SSH 2.
:program:`Dropbear` is compatible with :program:`OpenSSH` and uses :file:`~/.ssh/authorized_keys` for the management of public keys.
:program:`Dropbear` also provides a version of :program:`scp`, which must be compiled with :command:`make scp`:

.. code-block:: console

   # cd ..
   # wget http://matt.ucc.asn.au/dropbear/releases/dropbear-0.50.tar.gz
   # tar xzf dropbear-0.50.tar.gz
   # cd dropbear-0.50
   # ./configure --prefix=/usr
   # make
   # make scp
   # make DESTDIR=$PWD/_pkg install
   # strip -v scp
   # strip -v _pkg/usr/bin/*
   # strip -v _pkg/usr/sbin/*

Install the client and tools in :file:`/usr/bin`, and the server in :file:`/usr/sbin`:

.. code-block:: console

   # cp scp $fs/usr/bin
   # cp -a _pkg/usr/bin/* $fs/usr/bin
   # cp -a _pkg/usr/sbin/* $fs/usr/sbin


.. rubric:: libs

::

  libutil.so.1 => /lib/libutil.so.1 (0x40025000)
  libz.so.1 => /usr/lib/libz.so.1 (0x40028000)
  libcrypt.so.1 => /lib/libcrypt.so.1 (0x4003b000)
  libc.so.6 => /lib/libc.so.6 (0x40068000)
  /lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x40000000)

Copy the library :file:`libutil.so.1` to :file:`$fs/lib`, if this is not already the case.
Other libraries should be present following the construction of the base system:

.. code-block:: console

   # cp -a /lib/libutil* $fs/lib


.. rubric:: Configure Dropbear

The user configuration files :file:`authorized_keys` and :file:`known_hosts` are in :file:`~/.ssh`.
This directory and the file :file:`known_hosts` are created automatically the first time the user launches :program:`dbclient`.
The system configuration files for the :program:`Dropbear` server are in :file:`/etc/dropbear`:

.. code-block:: console

   # mkdir $fs/etc/dropbear

You must generate the secure keys before starting the :program:`Dropbear` server on SliTaz.
You can use :program:`dropbearkey` with the following commands:

.. code-block:: console

   # dropbearkey -t rsa -f /etc/dropbear/dropbear_rsa_host_key
   # dropbearkey -t dss -f /etc/dropbear/dropbear_dss_host_key

On SliTaz, you can start the SSH server with the command:

.. code-block:: console

   # /etc/init.d/dropbear start


lighttpd-1.4.18 — HTTP Web server
---------------------------------

:program:`Lighttpd` (`www.lighttpd.net <http://www.lighttpd.net/>`_) is a light, secure and powerful web server.
The project is very active and the server's configuration simple.
It supports virtual hosts, CGI scripts, and allows intelligent management of the CPU:

.. code-block:: console

   # cd ..
   # wget http://www.lighttpd.net/download/lighttpd-1.4.18.tar.gz
   # tar xzf lighttpd-1.4.18.tar.gz
   # cd lighttpd-1.4.18
   # ./configure -enable-shared --disable-ipv6 --prefix=/usr \
     --libdir=/usr/lib/lighttpd --mandir=/usr/share/man
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -vs _pkg/usr/bin/*
   # strip -vs _pkg/usr/sbin/*
   # strip -vs _pkg/usr/lib/lighttpd/*

Install the server and generated libraries.
We will then copy some of the modules (9):

.. code-block:: console

   # cp _pkg/usr/bin/* $fs/usr/bin
   # cp _pkg/usr/sbin/* $fs/usr/sbin
   # mkdir $fs/usr/lib/lighttpd
   Modules :
   # cp _pkg/usr/lib/lighttpd/mod_access.so $fs/usr/lib/lighttpd
   # cp _pkg/usr/lib/lighttpd/mod_accesslog.so $fs/usr/lib/lighttpd
   # cp _pkg/usr/lib/lighttpd/mod_alias.so $fs/usr/lib/lighttpd
   # cp _pkg/usr/lib/lighttpd/mod_auth.so $fs/usr/lib/lighttpd
   # cp _pkg/usr/lib/lighttpd/mod_cgi.so $fs/usr/lib/lighttpd
   # cp _pkg/usr/lib/lighttpd/mod_compress.so $fs/usr/lib/lighttpd
   # cp _pkg/usr/lib/lighttpd/mod_rewrite.so $fs/usr/lib/lighttpd
   # cp _pkg/usr/lib/lighttpd/mod_status.so $fs/usr/lib/lighttpd
   # cp _pkg/usr/lib/lighttpd/mod_userdir.so $fs/usr/lib/lighttpd


.. rubric:: libs

There should be a :file:`libdl.so.2` library; if missing, we can copy:

.. code-block:: console

   # cp -a /lib/libdl* $fs/lib


.. rubric:: :file:`/var/www` — root of documents served

:file:`/var/www` is the root directory of documents served by default.
You can access this via the URL http://localhost/.
This directory contains an :file:`index.html` automatically displayed by a query.
We will create the directory :file:`/var/www`, to see what's placed inside:

.. code-block:: console

   # mkdir -p $fs/var/www


.. rubric:: :file:`lighttpd.conf` — Lighttpd configuration file

The :program:`Lighttpd` main configuration file is located at :file:`/etc/lighttpd` and is called :file:`lighttpd.conf`.
The configuration file SliTaz provides is self-explanatary, just browse.
You can find other examples on the :program:`Lighttpd` website and as well as an example configuration in :file:`/doc` in the :program:`Lighttpd` archive:

.. code-block:: console

   # cp -a ../slitaz-tools-1.1/etc/lighttpd $fs/etc

Creating the directory containing the log files:

.. code-block:: console

   # mkdir $fs/var/log/lighttpd


.. rubric:: User and group www

We will add a user and a group for the web server, it adds security and there is no reason for it to be run a root.
The default user on SliTaz is 'www', but you can change this in the configuration file :file:`lighttpd.conf`.
The :program:`BusyBox` application :program:`adduser` has some limitations, so we add user 'www' manually.
We also change permissions on the directory of web server logs:

.. code-block:: console

   # echo "www:x:80:80:www:/var/www:/bin/sh" >> $fs/etc/passwd
   # echo "www:*:13509:0:99999:7:::" >> $fs/etc/shadow
   # echo "www:*:13509:0:99999:7:::" >> $fs/etc/shadow-
   # chroot $fs /bin/ash
   /# addgroup -g 80 www
   /# chown www.www /var/log/lighttpd
   # exit

To start the web server, you can use script :file:`/etc/init.d/lighttpd` provided by SliTaz tools, by typing: :command:`/etc/init.d/lighttpd start`.
You can also automate its launch at boot with a link :file:`/etc/init.d/lighttpd pointing` to :file:`/etc/rc.d/60lighttpd`.


iptables-1.3.7 — Netfilter, Linux firewall
------------------------------------------

:program:`Netfilter` (`www.netfilter.org <http://www.netfilter.org/>`_) is the module which provides the Linux kernel firewall functions, shared internet connections (NAT) and the archiving of network traffic.
The :command:`iptables` command allows you to configure :program:`Netfilter` using :command:`iptables-restore` and :command:`iptable-save`, to save and restore the :program:`Netfilter` configuration:

.. code-block:: console

   # cd ..
   # wget http://www.netfilter.org/projects/iptables/files/iptables-1.3.7.tar.bz2
   # tar xjf iptables-1.3.7.tar.bz2
   # cd iptables-1.3.7
   # make KERNEL_DIR=../linux-2.6.20 BINDIR=/sbin \
     LIBDIR=/lib MANDIR=/usr/share/man
   # make KERNEL_DIR=../linux-2.6.20 BINDIR=/sbin \
     LIBDIR=/lib MANDIR=/usr/share/man \
     DESTDIR=$PWD/_pkg install
   # strip  _pkg/sbin/*
   # strip  _pkg/lib/iptables/*

Installing the :command:`iptables*` applications and libraries sufficient for a basic firewall:

.. code-block:: console

   # cp -a _pkg/sbin/iptables* $fs/sbin
   # mkdir $fs/lib/iptables
   # cp -a _pkg/lib/iptables/{libipt_standard.so,libipt_conntrack.so} \
     $fs/lib/iptables
   # cp -a _pkg/lib/iptables/{libipt_tcp.so,libipt_udp.so} $fs/lib/iptables

To satisfy the :command:`iptables` dependencies, you must copy the :file:`libnsl*` library:

.. code-block:: console

   # cp -va /lib/libnsl* $fs/lib/tls
   # strip $fs/lib/libnsl*


sqlite-3.5.1 — Small SQL database engine
----------------------------------------

This package provides :file:`sqlite3` (`www.sqlite.org <http://www.sqlite.org/>`_) and :file:`sqlite3.so*` libraries.
:program:`SQLite` is fast and efficient and integrates directly to programs using database files:

.. code-block:: console

   # cd ..
   # wget http://www.sqlite.org/sqlite-3.5.1.tar.gz
   # tar xzf sqlite-3.5.1.tar.gz
   # cd sqlite-3.5.1
   # ./configure --prefix=/usr --disable-tcl
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip _pkg/usr/lib/*.so*
   # strip _pkg/usr/bin/*

Installing the :file:`sqlite3` utility and libraries in the rootfs of SliTaz:

.. code-block:: console

   # cp -a _pkg/usr/lib/*.so* $fs/usr/lib
   # cp -a _pkg/usr/bin/* $fs/usr/bin


cdrkit-1.1.5 — Tools for manipulating cdrom and ISO images
----------------------------------------------------------

:program:`cdrkit` (`www.cdrkit.org <http://www.cdrkit.org/>`_) provides tools for manipulating CD-ROMs.
SliTaz installs by default :command:`wodim` for burning and :command:`genisoimage` to create an ISO image.
The compilation is a bit different (:command:`cmake`), but shouldn't pose any problems:

.. code-block:: console

   # cd ..
   # wget http://cdrkit.org/releases/cdrkit-1.1.5.tar.gz
   # tar xzf cdrkit-1.1.5.tar.gz
   # cd cdrkit-1.1.5
   # make
   # make install PREFIX=$PWD/_pkg/usr
   # strip -v _pkg/usr/bin/*
   # strip -v _pkg/usr/sbin/*
   # cp _pkg/usr/bin/genisoimage $fs/usr/bin
   # cp _pkg/usr/bin/wodim $fs/usr/bin

Copy the library :file:`libcap.so.1` required by :file:`wodim`:

.. code-block:: console

   # cp -a /lib/libcap.so* $fs/lib


cpio-2.8 — Archiver
-------------------

"cpio" (http://www.gnu.org/software/cpio/) provides tools for manipulating cpio archives.
The archive format is used for packages and the SliTaz initramfs image of the CD-ROM.
Note that :program:`BusyBox` provides a version of cpio that only unpacks archives:

.. code-block:: console

   # cd ..
   # wget ftp://sunsite.cnlab-switch.ch/mirror/gnu/cpio/cpio-2.8.tar.gz
   # tar xzf cpio-2.8.tar.gz
   # cd cpio-2.8
   # ./configure --prefix=/usr --bindir=/bin \
     --libexecdir=/usr/bin --mandir=/usr/share/man \
     --infodir=/usr/share/info
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/bin/*
   # strip -v _pkg/usr/bin/*

Installing :file:`cpio` in :file:`/bin` and :file:`rmt` in :file:`/usr/bin`.
You can also install the French locale files:

.. code-block:: console

   # cp -a _pkg/bin/* $fs/bin
   # cp -a _pkg/usr/bin/* $fs/usr/bin
   # cp -a _pkg/usr/share/locale/fr $fs/usr/share/locale


microperl-5.8.8 — A tiny Perl
-----------------------------

Microperl is a tiny implementation of Perl using the most basic functions of the language.
You can find more info in the source archive and the file :file:`README.micro`.
We use a small :command:`sed` on the configuration file that searches for microperl modules in :file:`/usr/lib/perl5`.
We also create a link to the ``#! /usr/bin/perl`` script:

.. code-block:: console

   # wget http://ftp.funet.fi/pub/CPAN/src/perl-5.8.8.tar.gz
   # tar xzf perl-5.8.8.tar.gz
   # cd perl-5.8.8
   # sed -i s/'usr\/local'/'usr'/ uconfig.sh
   # sed -i s/'perl5\/5.9'/'perl5'/ uconfig.sh
   # sed -i s/'unknown'/'i486-pc-linux-gnu'/ uconfig.sh
   # make -f Makefile.micro regen_uconfig
   # make -f Makefile.micro
   # strip microperl
   # cp microperl $fs/usr/bin
   # chroot $fs /bin/ash
   /# cd /usr/bin
   /# ln -s microperl perl
   /# exit


module-init-tools-3.2 — Utilities for manipulating kernel modules
-----------------------------------------------------------------

The `module-init-tools <http://ftp.kernel.org/pub/linux/utils/kernel/module-init-tools/>`_ from kernel.org: :command:`modprobe`, :command:`insmod`, :command:`rmmod` and :command:`lsmod`.
We have chosen to use these because we can compile modutils/modprobe to support compressed (.gz) modules to save space.
To do this we use the option ``--enable-zlib``, we then clean and copy the binaries.
We do not take everything that has been created, only what we need: :file:`depmod`, :file:`insmod`, :file:`modinfo`, :file:`modprobe` and :file:`rmmod` in :file:`/sbin` and :file:`lsmod` in :file:`/bin`:

.. code-block:: console

   # cd ..
   # wget http://ftp.kernel.org/pub/linux/utils/kernel/module-init-tools/module-init-tools-3.2.tar.bz2
   # tar xjf module-init-tools-3.2.tar.bz2
   # cd module-init-tools-3.2
   # ./configure --enable-zlib --prefix=/usr --sbindir=/sbin --bindir=/bin \
     --sysconfdir=/etc --infodir=/usr/share/info --mandir=/usr/share/man
   # make
   # make DESTDIR=$PWD/_pkg install
   # strip -v _pkg/sbin/{depmod,insmod,modinfo,modprobe,rmmod}
   # strip -v _pkg/bin/lsmod
   # cp -i _pkg/sbin/{depmod,insmod,modinfo,modprobe,rmmod} $fs/sbin
   # cp -i _pkg/bin/lsmod $fs/bin
   # cd ..


Copy kernel modules
-------------------

Copy files from :file:`linux-2.6.20/_pkg`:

.. code-block:: console

   # cp -a linux-2.6.20/_pkg/lib/* $fs/lib


.. rubric:: Compress kernel modules

Compress modules, this step will gain us back around 50% of available space.
We begin by moving into the rootfs, then we search for all files with the ".ko" extension, and compress them.
You can also do this with the :file:`gzmodtaz.sh` script found in SliTaz tools:

.. code-block:: console

   # cd $fs

With :file:`gztazmod.sh`:

.. code-block:: console

   # cp -v ../src/slitaz-tools-1.1/utils/gztazmod.sh sbin
   # ./sbin/gztazmod.sh lib/modules/2.6.20-slitaz

Or by hand:

.. code-block:: console

   # cd lib/modules/2.6.20-slitaz
   # find . -name "*.ko" -exec gzip '{}' \;
   # sed 's/\.ko/.ko.gz/g' modules.dep > tmp.dep
   # rm modules.dep
   # mv tmp.dep modules.dep


Generate the initramfs and an ISO image
---------------------------------------

To create a new ISO image, you can use :command:`mktaziso` in :ref:`SliTaz tools <cookbook slitaztools>`.
Or you can create a new initramfs image, copy it to :file:`/boot` in the root of the CD-ROM (rootcd) and finally generate an ISO image with :command:`genisoimage`:

.. code-block:: console

   # cd $fs
   # find . -print | cpio -o -H newc | gzip -9 > ../rootfs.gz
   # cd ..
   # cp rootfs.gz rootcd/boot
   # genisoimage -R -o slitaz-test.iso -b boot/isolinux/isolinux.bin \
     -c boot/isolinux/boot.cat -no-emul-boot -boot-load-size 4 \
     -V "SliTaz" -input-charset iso8859-1 -boot-info-table rootcd

Test ISO image:

.. code-block:: console

   # qemu -cdrom slitaz-test.iso


.. rubric:: Following chapter

The next chapter is called :ref:`scratchbook base ncurses`.
It covers the installation and configuration of the ncurses libraries and applications.
