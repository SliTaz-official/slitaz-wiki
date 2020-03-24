.. http://doc.slitaz.org/en:scratchbook:base-system
.. en/scratchbook/base-system.txt · Last modified: 2011/05/14 01:04 by lebardix

.. _scratchbook base system:

Base System
===========

:author: domcox, lebardix

Build a SliTaz GNU/Linux distro running in RAM and using BusyBox.


About
-----

This document describes the construction of the SliTaz base system and why we use a Linux Kernel, BusyBox and Syslinux to boot the system.
SliTaz uses an initramfs archive unpacked in RAM by the kernel at boot.
We will create a box to hold a root of 3 to 4MB and use strip on the libraries and binaries to save space.

The scripts and configuration files are created with GNU nano, using the keystroke :kbd:`Ctrl`\ +\ :kbd:`X` to save and exit.
But of course you are free to replace with your own text editor.

This document is based on a howto found in the archive of BusyBox, which is itself based on a paper presented by Erik Anderson in the Embedded Systems Conference in 2001.


Wget src
--------

Create a :file:`src` directory for downloading and compiling:

.. code-block:: console

   # mkdir -p src
   # cd src

* Linux Kernel 2.6.20 (http://www.kernel.org/).

  .. code-block:: console

     # wget ftp://ftp.kernel.org/pub/linux/kernel/v2.6/linux-2.6.20.tar.bz2

* Busybox 1.2.2 (http://www.busybox.net/).

  .. code-block:: console

     # wget http://www.busybox.net/downloads/busybox-1.2.2.tar.bz2

* Syslinux 3.35 (http://syslinux.zytor.com/).

  .. code-block:: console

     # wget ftp://ftp.kernel.org/pub/linux/boot/syslinux/3.xx/syslinux-3.35.tar.gz

* SliTaz tools 1.1.
  Download SliTaz tools, unpack, save the file in :file:`src/` and that's it:

  .. code-block:: console

     # wget http://download.tuxfamily.org/slitaz/sources/tools/slitaz-tools-1.1.tar.gz
     # tar xzf slitaz-tools-1.1.tar.gz


Unpack and prepare the Linux Kernel
-----------------------------------

We will begin by compiling a Linux kernel, which may take a little time.


Linux Kernel
^^^^^^^^^^^^

Your kernel must support the intramfs filesystem, otherwise the CD-ROM will not start.
You can also install the modules in a directory so as not to touch the host system.
The configuration of the Linux kernel sources is done by :command:`make menuconfig` using ncurses or graphically with :command:`make gconfig` or :command:`make xconfig` using GTK development packages and/or QT respectively.
You can find in :ref:`cookbook slitaztools`, Makefiles for the various 2.6.xx kernels.

A feature of the 2.6 kernels is that if we :command:`make menuconfig`, :command:`xconfig` or :command:`config` for the first time, the setup menu is displayed based on the configuration of our current kernel.

The options depend on your needs, you can install module-init-tools to support compressed modules or for a minimal install, you can select only the vital options.

We start by changing into the sources, :command:`make mrproper` to put things in order, then we start a configuration interface: gconfig, xconfig, menuconfig or oldconfig:

.. code-block:: console

   # tar xjf linux-2.6.20.tar.bz2
   # cd linux-2.6.20
   # make mrproper
   # cp ../slitaz-tools-1.1/Makefiles/linux-2.6.20-slitaz.config .config
   # make oldconfig
   (# make menuconfig)
   # make bzImage
   # make modules
   # make INSTALL_MOD_PATH=$PWD/_pkg modules_install
   # cd ..

If you want more info on compiling kernels, there are many textbooks.
Note that you can install the kernel and after rebooting, you can compile your own kernel following the same instructions.


Creation of the root system (rootfs)
------------------------------------

The next step will create a file named 'rootfs' — Root File System, in the working directory :file:`SliTaz/`:

.. code-block:: console

   # mkdir ../rootfs


Install BusyBox
^^^^^^^^^^^^^^^

BusyBox (`www.busybox.net <http://www.busybox.net/>`_) is a single executable offering versions of the main tools necessary to use a Linux kernel.
It is (mainly) intended to be used embedded and can do almost anything.
As well as proposing (coreutils) shell commands and a daemons system, it also provides a websever and client/server (DHCP, udhcpc).

.. code-block:: console

   # tar xjf busybox-1.2.2.tar.bz2

Configure and compile, remembering the dumpkmap options, init, etc — you can find help in the Makefile in SliTaz Busybox tools.
Make install creates a :file:`_install` directory in the current directory:

.. code-block:: console

   # cd busybox-1.2.2
   # cp ../slitaz-tools-1.1/Makefiles/busybox-1.2.2.config .config
   # make oldconfig
   (# make menuconfig)
   # make
   # make install
   # chmod 4755 _install/bin/busybox

Copy files compiled by BusyBox in the directory :file:`_install` to the root file system (rootfs):

.. code-block:: console

   # cp -a _install/* ../../rootfs

The :file:`linuxrc` link pointing to :file:`/bin/busybox`, folders :file:`/bin`, :file:`/lib` and :file:`/sbin` were added to the directory :file:`/rootfs` — you can check this.
It may be that the link isn't there if you didn't select the option initrd support in Busybox.
We'll delete the :file:`linuxrc` link and create a link for :file:`init` that points to :file:`/bin/busybox`:

.. code-block:: console

   # cd ../../rootfs
   # ls -CF
   bin/  linuxrc@  sbin/  usr/

   # rm linuxrc
   # ln -s bin/busybox init


:command:`ldd` on :program:`BusyBox`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :command:`ldd` command can show any libraries used by a program.
Libraries used by :program:`Busybox` may differ depending on the host system.
On Debian for example, copying the libraries in :file:`/lib/tls`.
The following commands are given using 'v' for verbose mode.
To eliminate the symbols of executable binaries and shared libraries we can utilize :command:`strip`.
Note you may also use the mklibs or uClibc libraries.

.. code-block:: console

   # mkdir lib

SliTaz or another:

.. code-block:: console

   # cp /lib/{libcrypt.so.1,libm.so.6,libc.so.6} lib
   # cp /lib/ld-linux.so.2 lib

Example on Debian Etch:

.. code-block:: console

   # cp /lib/tls/{libcrypt.so.1,libm.so.6,libc.so.6} lib
   # cp /lib/ld-linux.so.2 lib

Cleanup libraries with :command:`strip`:

.. code-block:: console

   # strip -v lib/*


Linux tree and configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Make some directories for a classic Linux branch SliTaz installation.
:file:`/dev` for devices, :file:`/etc`, :file:`/home`, :file:`/usr`, :file:`/proc`, :file:`/root` and co.
To learn more about the hierarchy of a file system and its contents, there is a File System Hierarchy Standard available in various formats at `www.pathname.com/fhs/ <http://www.pathname.com/fhs/>`_.

You are free to create your own directory tree.
In traditional Unix systems, :file:`/usr` usually contains files from the distribution, :file:`/dev` contains devices (devices), :file:`/etc` contains configuration files, :file:`/lib` libraries, :file:`/home` for home users and :file:`/var` for variable data.
Note that we do not create :file:`/lib`, :file:`/bin` or :file:`/sbin` — these are created when BusyBox is installed.

.. code-block:: console

   # mkdir -p dev etc root home proc media mnt sys tmp var
   # mkdir -p usr/{lib,local,games,share} \
     var/{cache,lib,lock,log,games,run,spool} \
     media/{cdrom,flash,usbdisk}

Change permissions on the :file:`/tmp` directory:

.. code-block:: console

   # chmod 1777 tmp

Setting up glibc — note :file:`/etc/ld.so.conf` and :file:`/etc/rpc` are not essential for a micro system:

.. code-block:: console

   # touch etc/ld.so.conf
   # cp /etc/rpc etc


Create the devices in :file:`/dev`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This can be done with the script :file:`mkdevs.sh` found in BusyBox, or with our script :file:`mktazdevs.sh` in SliTaz tools.
If you want more details, read the scripts.
If you used the BusyBox version, we must still create the :file:`pts` directory:

.. code-block:: console

   # cp ../src/slitaz-tools-1.1/utils/mktazdevs.sh bin
   # ./bin/mktazdevs.sh dev

or:

.. code-block:: console

   # cp ../src/busybox-1.2.2/examples/bootfloppy/mkdevs.sh bin
   # ./bin/mkdevs.sh dev
   # mkdir -p dev/{pts,input,shm,net,usb}

Note that we start mdev-s with the rcS script to create devices dynamically at boot.


Support for the resolution of hostnames (DNS)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Copy the libraries :file:`libnss_*` of the host system into our SliTaz system.
These libraries are used for name resolution and are cleaned with strip:

.. code-block:: console

   # cp /lib/{libnss_dns.so.2,libnss_files.so.2} lib
   # cp /lib/libresolv.so.2 lib
   # strip -v lib/*.so*


Configuration of your box
-------------------------

Create the necessary files in :file:`/etc`.
For more info, just look at the contents of the files.
We start by creating some files relevant to the core operating system.


Configure network
^^^^^^^^^^^^^^^^^

Create basic files used to configure the network:

.. code-block:: console

   # echo "127.0.0.1      localhost" > etc/hosts
   # echo "localnet    127.0.0.1" > etc/networks
   # echo "slitaz" > etc/hostname
   # echo "order hosts,bind" > etc/host.conf
   # echo "multi on" >> etc/host.conf


:file:`/etc/nsswitch.conf`
^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration files used to resolve names:

.. code-block:: console

   # nano etc/nsswitch.conf

.. code-block:: shell

   # /etc/nsswitch.conf: GNU Name Service Switch config.
   #
   
   passwd:     files
   group:      files
   shadow:     files
   
   hosts:      files dns
   networks:   files


:file:`/etc/securetty`
^^^^^^^^^^^^^^^^^^^^^^

:file:`/etc/securetty` lists terminals that can connect to root:

.. code-block:: console

   # nano etc/securetty

.. code-block:: shell

   # /etc/securetty: List of terminals on which root is allowed to login.
   #
   console
   
   # For people with serial port consoles
   ttyS0
   
   # Standard consoles
   tty1
   tty2
   tty3
   tty4
   tty5
   tty6
   tty7


:file:`/etc/shells`
^^^^^^^^^^^^^^^^^^^

:file:`/etc/shells`, a shells list of valid connections.
This file is used by the SSH server (:program:`Dropbear`):

.. code-block:: console

   # nano etc/shells

.. code-block:: shell

   # /etc/shells: valid login shells.
   /bin/sh
   /bin/ash
   /bin/hush


:file:`/etc/issue` and :file:`/etc/motd`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

:file:`/etc/issue` is displayed at the end of boot and the *message of the day* is displayed after logging in:

.. code-block:: console

   # echo "SliTaz GNU/Linux 1.0 Kernel \r \l" > etc/issue
   # echo "" >> etc/issue
   # nano etc/motd

.. code-block:: text

    (°-  { Get documentation in: /usr/share/doc.
    //\    Use: 'less' or 'more' to read files, 'su' to be root. }
    v_/_
   
   SliTaz is distributed in the hope that it will be useful, but
   with ABSOLUTELY NO WARRANTY.


:file:`/etc/busybox.conf`
^^^^^^^^^^^^^^^^^^^^^^^^^

The configuration file for BusyBox, it can set duties on BusyBox applications.
For more information, you can read the :ref:`handbook security` page in the Handbook.
:file:`BusyBox.conf` file:

.. code-block:: console

   # nano etc/busybox.conf

.. code-block:: ini

   # /etc/busybox.conf: SliTaz GNU/linux Busybox configuration.
   #
   
   [SUID]
   # Allow command to be run by anyone.
   su = ssx root.root
   passwd = ssx root.root
   loadkmap = ssx root.root
   mount = ssx root.root
   reboot = ssx root.root
   halt = ssx root.root

For added security, change the permissions on the file:

.. code-block:: console

   # chmod 600 etc/busybox.conf


:file:`/etc/inittab`
^^^^^^^^^^^^^^^^^^^^

Minimal configuration file for init.
It helps to have a root console without having to go through the login and a console on tty2.

.. code-block:: console

   # nano etc/inittab

.. code-block:: shell

   # /etc/inittab: init configuration for SliTaz GNU/Linux.
   
   ::sysinit:/etc/init.d/rcS
   ::respawn:-/bin/sh
   tty2::askfirst:-/bin/sh
   ::ctrlaltdel:/bin/umount -a -r
   ::ctrlaltdel:/sbin/reboot

You will also find a wider example of an :file:`inittab` file in the archive of BusyBox.


:file:`/etc/profile`
^^^^^^^^^^^^^^^^^^^^

This file is read at each login and affects all users.
We must use the :file:`~/.profile` config file for each individual user:

.. code-block:: console

   # nano etc/profile

.. code-block:: shell

   # /etc/profile: system-wide .profile file for the Bourne shells
   
   PATH="/usr/sbin:/usr/bin:/sbin:/bin:/usr/games"
   LD_LIBRARY_PATH="/usr/lib:/lib"
   
   if [ "`id -u`" -eq 0 ]; then
     PS1='\e[1m\u@\h:\w\#\e[m '
   else
     PS1='\e[1m\u@\h:\w\$\e[m '
   fi
   
   DISPLAY=:0.0
   
   export PATH LD_LIBRARY_PATH PS1 DISPLAY ignoreeof
   umask 022


Users, groups and passwords
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Create configuration files of users, groups and passwords in :file:`/etc/ {passwd, shadow, group, gshadow}`, and adjust permissions:

.. code-block:: console

   # echo "root:x:0:0:root:/root:/bin/sh" > etc/passwd
   # echo "root::13525:0:99999:7:::" > etc/shadow
   # echo "root:x:0:" > etc/group
   # echo "root:*::" > etc/gshadow
   # chmod 640 etc/shadow
   # chmod 640 etc/gshadow

You can add other users, like hacker is used by the LiveCD mode.
You can also configure a password for the root user with the :command:`passwd` command.
To add an existing user to an existing group, you must edit :file:`/etc/group` and :file:`/etc/gshadow` because the applet :program:`adduser` provided by BusyBox doesn't offer all of the options provided by the original program.


:file:`/etc/fstab` or :file:`/etc/mtab`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

List filesystems to be mounted:

.. code-block:: console

   # nano etc/fstab

.. code-block:: shell

   # /etc/fstab: information about static file system.
   #
   proc            /proc        proc    defaults          0       0
   sysfs           /sys         sysfs   defaults          0       0
   devpts          /dev/pts     devpts  defaults          0       0
   tmpfs           /dev/shm     tmpfs   defaults          0       0

:file:`/etc/mtab` is used by other :file:`mkfs*`, for listing the mounted partitions.
It needs :file:`/proc` because there is a link on :file:`/proc/mounts`:

.. code-block:: console

   # chroot . /bin/ash
   /# ln -s /proc/mounts /etc/mtab


Keyboard
^^^^^^^^

You can create a :file:`kmap` file specific to your keyboard with the :command:`dumpkmap` command provided by BusyBox.
You can find some :file:`kmap` files in SliTaz tools.
To create a fr_CH kmap file:

.. code-block:: console

   /# mkdir /usr/share/kmap
   /# /bin/busybox dumpkmap > /usr/share/kmap/fr_CH.kmap
   /# exit

Once this is done, you can automatically load your keyboard with loadkmap in a :file:`/etc/init.d/rcS` script:


:file:`/usr/share/doc`
^^^^^^^^^^^^^^^^^^^^^^

You can also add various documents, such as a SliTaz user manual, which you can download as a :file:`tar.gz` from the website:

.. code-block:: console

   # mkdir -p usr/share/doc


Installing the :file:`udhcpc` script
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Udhcpc DHCP client supplied by Busybox is fast and stable, but is developed independently.
Web site: http://udhcp.busybox.net/.
You can use the default script found in the archive of BusyBox.
This script goes into :file:`/usr/share/udhcpc/default.script`, but this can be changed via the command line.
On SliTaz, the client is started at boot by the script :file:`/etc/init.d/network.sh` via the configuration file :file:`/etc/network.conf`:

.. code-block:: console

   # mkdir usr/share/udhcpc
   # cp ../src/busybox-1.2.2/examples/udhcp/simple.script \
     usr/share/udhcpc/default.script
   # chmod +x usr/share/udhcpc/default.script


:file:`/etc/init.d/rcS`
^^^^^^^^^^^^^^^^^^^^^^^

To finish off this draft, you must create the init script :file:`/etc/init.d/rcS` to mount the filesystems and run some commands.
For more information, you can look at the :ref:`cookbook bootscripts` page.
You can change the value of the variable ``KMAP=`` for the keyboard:

.. code-block:: console

   # mkdir etc/init.d
   # nano etc/init.d/rcS

.. code-block:: shell

   #! /bin/sh
   # /etc/init.d/rcS: rcS initial script.
   #
   
   KMAP=fr_CH
   
   echo "Processing /etc/init.d/rcS... "
   
   /bin/mount proc
   /bin/mount -a
   /bin/hostname -F /etc/hostname
   /sbin/ifconfig lo 127.0.0.1 up
   /sbin/loadkmap < /usr/share/kmap/$KMAP.kmap

.. code-block:: console

   # chmod +x etc/init.d/rcS


Note
^^^^

Note that you can still install the Tazpkg package manager (10 kb) that we created, you will find information to install in the source tarball.
You can also install various files from SliTaz tools, such as the licence.


Build an initramfs cpio archive
-------------------------------

The initramfs is a "cpio" archive generated from the root of the system, it is decompressed in RAM by the Linux kernel at boot to create the filesystem (also in RAM).
To generate an initramfs archive, using the root directory of system files (rootfs), we facilitate a search with :command:`find` and add some pipes :command:`|`.
Then we create a cpio archive using :command:`gzip` which we put in the working directory.

The SliTaz initramfs :file:`rootfs.gz` is the root system, but with a :file:`.gz` extension.
If you want to change the name, you need to edit the configuration file for isolinux: :file:`isolinux.cfg` or the :file:`menu.lst` for GRUB.

Generation of the initramfs:

.. code-block:: console

   # find . -print | cpio -o -H newc | gzip -9 > ../rootfs.gz

You should now have a file :file:`rootfs.gz` about 1 to 2MB in the working directory :file:`SliTaz/`.

For a new image, when making changes in rootfs, simply copy the new :file:`rootfs.gz` archive to :file:`rootcd/boot` and create a new image with :command:`genisoimage` or :command:`mkisofs`.
For this you can also use :command:`mktaziso` within SliTaz tools.
This script will check if the directories are present, create a new compressed cpio archive and generate a new bootable ISO image.


Make rootcd files
-----------------

The following steps will help you create the root of the bootable CD-ROM.
We begin by creating the :file:`rootcd`, :file:`boot` and :file:`isolinux` directories for the CD-ROM files:

.. code-block:: console

   # cd ..
   # mkdir -p rootcd/boot/isolinux

Optionally, you can create some other directories in which to place various data, such as HTML documents or packages.


Copy the kernel
^^^^^^^^^^^^^^^

Just copy the kernel previously compiled to :file:`rootcd/boot`:

.. code-block:: console

   # cp src/linux-2.6.20/arch/i386/boot/bzImage rootcd/boot


Copy the initramfs into :file:`rootcd/boot`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Copy the :file:`rootfs.gz` to :file:`rootcd/boot`.
We must not forget to generate a new initramfs archive for any changes made to the rootfs (root file system):

.. code-block:: console

   # cp rootfs.gz rootcd/boot


Install the isolinux bootloader
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The bootloader isolinux — simply copy the :file:`isolinux.bin` from the source archive of Syslinux:

.. code-block:: console

   # cd src
   # tar xzf syslinux-3.35.tar.gz
   # cp syslinux-3.35/isolinux.bin ../rootcd/boot/isolinux
   # cd ..


:file:`isolinux.cfg` — Configure isolinux
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here is an example of an :file:`isolinux.cfg` file that should work well.
You can change it if you wish:

.. code-block:: console

   # nano rootcd/boot/isolinux/isolinux.cfg

.. code-block:: text

   display display.txt
   default slitaz
   label slitaz
       kernel /boot/bzImage
       append initrd=/boot/rootfs.gz rw root=/dev/null vga=788
   implicit 0
   prompt 1
   timeout 80

Here are some changes that you might like to make in :file:`isolinux.cfg`:

* The ``timeout`` value is the number of seconds to wait before booting.
  You can make it 0 or delete the line to start instantly, or choose to wait as long as 10s.
* ``prompt`` can be set to 0 to disable the ``boot:`` prompt.
* You can add more lines to view the contents of several text files when the user presses :kbd:`F1`, :kbd:`F2`, :kbd:`F3`, etc.


:file:`display.txt`
^^^^^^^^^^^^^^^^^^^

A small welcome note, powered by isolinux, you can modify this file if you wish:

.. code-block:: console

   # nano rootcd/boot/isolinux/display.txt

.. code-block:: text

   /*       _\|/_
            (o o)
    +----oOO-{_}-OOo---------------------------------------------------+
        ____  _ _ _____
       / ___|| (_)_   _|_ _ ____
       \___ \| | | | |/ _` |_  /
        ___) | | | | | (_| |/ /
       |____/|_|_| |_|\__,_/___|
   
    SliTaz GNU/Linux - Temporary Autonomous Zone
   
        <ENTER> to boot.
   
                                                                      */


Create an ISO image with :command:`genisoimage` or :command:`mkisofs`
---------------------------------------------------------------------

.. code-block:: console

   # genisoimage -R -o slitaz-cooking.iso -b boot/isolinux/isolinux.bin \
     -c boot/isolinux/boot.cat -no-emul-boot -boot-load-size 4 \
     -V "SliTaz" -input-charset iso8859-1 -boot-info-table rootcd

For each change in the root of the box, you must create a new ISO image.

You can create a small script that will generate a new compressed cpio archive and a new image, or use :command:`mktaziso` within SliTaz tools.
Note that you can also use GRUB to boot the box.


Burn or test ISO image with :program:`Qemu`
-------------------------------------------

You can burn the ISO image with :program:`Graveman`, :program:`k3b` or :command:`wodim` and boot it.
Simple burning command using :command:`wodim` (also valid for :command:`cdrecord`), with a 2.6.XX. kernel:

.. code-block:: console

   # wodim -v -speed=24 -data slitaz-cooking.iso


Qemu
^^^^

Note that you can test the ISO image with the software emulator :program:`Qemu` (On Debian :command:`# aptitude install qemu`).
To emulate the newly created ISO image, simply type:

.. code-block:: console

   # qemu -cdrom slitaz-cooking.iso


Following chapter
^^^^^^^^^^^^^^^^^

The next chapter :ref:`scratchbook base apps` provides all the instructions to install and configure the basic applications and libraries.
