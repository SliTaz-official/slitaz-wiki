.. http://doc.slitaz.org/en:guides:kernel
.. en/guides/kernel.txt · Last modified: 2011/01/06 21:40 by woodt

.. _kernel:

Build your own custom Linux Kernel for SliTaz
=============================================

:author: linea, fandesandro, jozee, seawolf, ernia, woodt


Prepare your system
-------------------

* Install the :program:`slitaz-toolchain` meta package.
  This package contains the required base files.

  * :program:`binutils`
  * :program:`linux-headers`
  * :program:`glibc-dev`
  * :program:`gcc`
  * :program:`make`

  .. code-block:: console

     # tazpkg get-install slitaz-toolchain

* Install the packages required to configure and compile the kernel sources.

  * :program:`ncurses-dev`
  * :program:`perl`

  .. code-block:: console

     # tazpkg get-install ncurses-dev
     # tazpkg get-install perl

* SliTaz provides a :program:`linux-source` package.
  The kernel source tree will be downloaded from the mirror, installed to the :file:`/usr/src/linux-{VERSION}` directory, and patched for SliTaz.

  .. code-block:: console

     # tazpkg get-install linux-source
     # /usr/bin/get-linux-source
     # ls -l /usr/src
     lrwxrwxrwx  1 root root       21 Jul 21 21:27 linux -> linux-2.6.25.5-slitaz
     drwxrwxr-x 23 root root     4096 Jul 21 22:41 linux-2.6.25.5-slitaz
     -rw-r--r--  1 root root 48589640 Jul 21 21:28 linux-2.6.25.5.tar.bz2


Configure and Compile
---------------------

The Linux kernel source tree is now ready to configure and compile.

* Go to the directory that contains the kernel source tree.

  .. code-block:: console

     # cd /usr/src/linux

* Prepare the build from the default configuration.

  .. code-block:: console

     # make oldconfig && make prepare

* Modify the kernel configuration to your needs, and compile.
* The *menuconfig* allows you to customise the kernel, which is built into the :file:`bzImage` file.
  Any *modules* are built and then *install*\ ed in to the :file:`/usr/include` directory.
  You should then copy the kernel itself to the :file:`/boot` directory.

  .. code-block:: console

     # make menuconfig
     # make
     # make modules
     # make modules_install
     # cp arch/x86/boot/bzImage /boot

* Configure the boot-loader (optional)

  .. code-block:: console

     # leafpad /boot/grub/menu.lst

* Add the new kernel to the list ::

    # My kernel:
    title 	SliTaz GNU/Linux (cooking) (Kernel <VERSION>)
    		root (hd0,1)
    		kernel /boot/bzImage root=/dev/sda2
