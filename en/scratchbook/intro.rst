.. http://doc.slitaz.org/en:scratchbook:start
.. en/scratchbook/start.txt · Last modified: 2011/04/21 23:17 by domcox


Introduction
------------

The scratchbook allows you to track the creation of the first public release of SliTaz and make a trip to the heart of GNU/Linux.
You'll be able to customize your new system or create your own autonomous distro running in system memory (RAM) that's fully installable on a hard drive or USB key.
Once started you'll be able to remove the CD-ROM and still have SliTaz working.
SliTaz can also be used as an environment in which we can chroot or use the CD-ROM for multitasking.
The only prerequisite is a host distribution in which you can store libraries, use a compiler and development tools, etc.
The host system can be a chrooted development environment, a minimal distro, SliTaz installed on a hard drive or a 'general' distro such as Debian, Slackware, Fedora, Gentoo, Mandriva, Arch, etc.
Note that nothing is installed in the host system by our commands.

SliTaz uses the 'Swiss Army Knife' BusyBox as the basis of the system and the Linux Kernel, it runs embedded using a small memory footprint and provides many files.
BusyBox is our main source of information and it's a utility of the Debian project which we use and cherish.

SliTaz uses the Syslinux bootloader and an archived initramfs compressed with cpio.
This archive is then decompressed in memory at boot by the kernel into a system of no fixed size, retaining control over init.
At the time of compilation or copying of applications, we use strip to clean the repositoiries.
The system commands :command:`genisoimage` or :command:`mkisofs` are used to create the ISO images.
To finish, you can test the ISO image with :program:`Qemu` or engrave the generated ISO on to a rewritable CD-RW.


Organize a working directory
----------------------------

To create SliTaz, we need a working directory and several subdirectories whether you have a chrooted environment for developing or a host system, we advise to use a directory named :file:`distro/` in which to work.
The :file:`distro/` directory can be a simple folder or a partition, but you are obviously free to put all of this elsewhere.


.. rubric:: :file:`distro/`

Contents of a working directory:

:file:`rootfs/`:
  The root filesystem — this is the root system, designed to operate in RAM, it is used to generate the initramfs image.

:file:`rootfs.gz`:
  The initramfs image of our system — a cpio archive compressed with gzip.

:file:`rootcd/`:
  The rootcd.
  This is the root of the CD-ROM files.

:file:`src/`:
  The sources, Kernel, Syslinux, Busybox, Dropbear, etc (it can also be a symbolic link).

Thereafter, the initramfs and bootable ISO image (:file:`slitaz-cooking.iso`) will be created in the root directory of our work named :file:`SliTaz/`.


.. rubric:: Option: rootfs.ext2 — using a virtual hard drive

Option: rootfs.ext2 (root filesystem in ext2) is a virtual hard disk formatted with ext2 and mounted on a (rootfs) loop.
A device loop allows a file to be used as a standard device (hard drive, floppy, etc) to build a filesystem inside.
This file can be any number of megabytes, we propose 20,480, which corresponds to 20MB:

.. code-block:: console

   # dd if=/dev/zero of=rootfs.ext2 bs=1k count=20480

Create a *ext2* filesystem named :file:`rootfs.ext2`, the option ``-F`` formats the file.
Note that the ``-m 0`` option doesn't allocate any space for the user root — by default it occupies approximately 5% and the ``-t`` option defines the type of filesystem to be used, such as ext2 or ext3:

.. code-block:: console

   # mkfs -t ext2 -F -m 0 rootfs.ext2

We can now assemble :file:`rootfs.ext2` with a loop, thanks to the ``-o loop option`` provided by the mount utility in the :file:`rootfs/` directory.
You can check if the assembly went well with the :command:`df-h` command:

.. code-block:: console

   # mkdir rootfs
   # mount -o loop rootfs.ext2 rootfs
   # df -h

At the end of the session, you can dismount the volume with umount:

.. code-block:: console

   # umount rootfs

Now we can proceed to the construction of the :ref:`scratchbook base system`.
