.. http://doc.slitaz.org/en:handbook:chroot
.. en/handbook/chroot.txt · Last modified: 2011/05/03 22:47 by domcox

.. _handbook chroot:

Chroot environment
==================

This document describes the necessary steps to create a chrooted environment, in order to change the root of the system so that you can work.
This makes it possible to compile, test and develop SliTaz without any risk to the host system you're working on.
The host system can be SliTaz installed to a hard drive or any other GNU/Linux system such as Debian, Fedora, PCLinuxOS and so on.
You can also create a chrooted environment in LiveCD mode associated with USB media.
The only prerequisite is to have a SliTaz ISO image available and a little time.
Note that all commands are carried out as system administrator (root).


Prepare the environment
-----------------------

To begin, we must extract the contents of the ISO image into the directory that will serve as our chroot.
The directory can be created any place you choose, we'll use a directory :file:`/home/slitaz/chroot-env`.
To extract the contents of an ISO image, we must mount it in a loop directory and then copy the compressed root filesystem (:file:`rootfs.gz`) into the chroot directory.
Assuming the ISO is in the current directory:

.. code-block:: console

   # mkdir /tmp/loop
   # mount -o loop slitaz-cooking.iso /tmp/loop
   # mkdir -p /home/slitaz/chroot-env
   # cp /tmp/loop/boot/rootfs.gz /home/slitaz/chroot-env
   # umount /tmp/loop

Now we have a copy of the compressed filesystem, we must extract and unpack it (this is a :program:`cpio` archive compressed with either :program:`gzip` or :program:`lzma`).
To complete this stage, we can remove the rootfs which is no longer required:

.. code-block:: console

   # cd /home/slitaz/chroot-env
   # (zcat rootfs.gz 2>/dev/null || lzma d rootfs.gz -so) | cpio -id
   # rm rootfs rootfs.gz

If the unpacking of the rootfs compressed with :program:`lzma` fails; you can use the following method:

.. code-block:: console

   # unlzma rootfs.gz -S .gz 
   # cat rootfs | cpio -id


Using the environment
---------------------

To begin using the chrooted environment, you just need to mount some virtual filesystems and use the :command:`chroot` command.
To simplify things, we can write a small script automating the process.
Example using the chroot directory :file:`/home/slitaz/chroot-env` and creating a script :file:`chroot_in_env.sh` in :file:`/home/slitaz`.
On any systems other than SliTaz you can uncomment the lines about :file:`/dev` and :file:`/tmp` — Note to save typing you can copy and paste:

.. code-block:: console

   # cat > /home/slitaz/chroot_in_env.sh << "EOF"

.. code-block:: shell

   #!/bin/sh
   # Chroot in SliTaz to hack.
   #
   ROOTFS="/home/slitaz/chroot-env"
   
   # Mount virtual Kernel file systems and chroot.
   #
   #mount --bind /dev $ROOTFS/dev
   #mount --bind /tmp $ROOTFS/tmp
   mount -t proc proc $ROOTFS/proc
   mount -t sysfs sysfs $ROOTFS/sys
   mount -t devpts devpts $ROOTFS/dev/pts
   mount -t tmpfs shm $ROOTFS/dev/shm
   
   echo "Chrooting into $ROOTFS... "
   chroot $ROOTFS /bin/sh --login
   
   # Unmount virtual Kernel file systems on exit.
   #
   umount $ROOTFS/dev/shm
   umount $ROOTFS/dev/pts
   umount $ROOTFS/sys
   umount $ROOTFS/proc
   #umount $ROOTFS/tmp
   #umount $ROOTFS/dev
   
   echo "Exiting $ROOTFS chroot environment... "
   
   EOF

To finish and test the environment, you just make the script executable and run:

.. code-block:: console

   # chmod +x /home/slitaz/chroot_in_env.sh
   # sh /home/slitaz/chroot_in_env.sh


.. rubric:: To activate the network

In order to have the network up to download and install some development packages, just start the DHCP client on the correct interface.
Example using ``eth1``:

.. code-block:: console

   # udhcpc -i eth1


.. rubric:: Installing packages

If the network is functional, just reload the list of packages and use :command:`tazpkg get-install` to install them.
If a connection is not possible, you can download the packages from another system, copy them to the chrooted environment and install them with the :command:`tazpkg install` command.
To install the basic compilation tools:

.. code-block:: console

   # tazpkg recharge
   # tazpkg get-install slitaz-toolchain

Once the environment is configured, you can compile applications from source to create packages, test scripts, etc.
The Cookbook should help you out here:


.. rubric:: Exit the environment

To exit the chrooted environment, just type :command:`exit`, the :file:`chroot_in_env.sh` script will then end by unmounting the virtual filesystems from the Linux Kernel:

.. code-block:: console

   # exit
   #
