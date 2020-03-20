.. http://doc.slitaz.org/en:guides:uncommoninst
.. en/guides/uncommoninst.txt · Last modified: 2015/06/21 14:28 by linea

.. _uncommoninst:

Unusual install methods
=======================

The :ref:`slitaz-installer <handbook installation>` installs SliTaz in a partition or a hard drive as most Linux distributions do.
However, there are many other ways to install SliTaz…

The following configurations are using the SliTaz 4.0 boot loader :program:`grub4dos-linux`.

Most kernel command line arguments are processed by `/init <http://hg.slitaz.org/slitaz-boot-scripts/file/tip/init>`_ during the :ref:`boot process <bootguide>`.


Frugal install
--------------

You don't need a special partition, the system runs in RAM like a Live CD…

See :ref:`traditional frugal install` and :ref:`iso image install`

…it can be tuned to your needs a little bit.

See :ref:`tuning the boot process`

You can also use a LORAM flavor created with :program:`tazlitobox` and :guilabel:`The filesystem is always in RAM`…

…or :guilabel:`The filesystem may be on a small CD-ROM`.
Install the CD-ROM files :file:`/boot/bzImage` and :file:`/boot/rootfs.gz` and copy the :file:`/rootfs.gz`.
Say into :file:`/this/directory`.
Now get the label of the partition.
Say mypartition:

.. code-block:: console

   # blkid

And append the param ``loram=`` to the cmdline::

  kernel (hd1)/boot/bzImage rw root=/dev/null vga=normal loram=LABEL=mypartition,this/directory

.. tip::
   You can also use a device name (``loram=/dev/hda2,this/directory``).
   The device name may vary with the kernel version (``hda`` or ``sda``) and USB keys.


USB key install
---------------


Tazusb
^^^^^^

This is a mix between a frugal and traditional install.
The system runs fully in RAM but the :file:`home` directory is always on the key.
You can modify the system (configure, install packages) and then save the new system on the key 8-).
See `tazusb manual <http://hg.slitaz.org/tazusb/raw-file/tip/doc/tazusb.en.html>`_

.. tip::
   You can create the USB key from the ISO image

   .. code-block:: console

      # tazusb gen-iso2usb slitaz-cooking.iso


Hybrid ISO
^^^^^^^^^^

This install method will **erase all of your key** and install a **unmodifiable** :-/ SliTaz.
You can create a custom system with :program:`tazlito` or :program:`tazlitobox`.
Each SliTaz ISO image is :ref:`hybrid <dvd hybrid iso>`.

.. tip::
   Use :program:`fdisk` to get back the remining free space on the USB keys.
   Simply add a partition(s).

   .. code-block:: console

      # dd if=slitaz-cooking.iso of=/dev/usbkey
      # fdisk /dev/usbkey


Floppy install
--------------


The last resort install
^^^^^^^^^^^^^^^^^^^^^^^

Imagine you have a very old PC with a floppy drive and a hard disk.
No CD-ROM, no network card, and no USB.
The hard disk works only with this machine.
You can't plug it into your friend's PC or into an USB disk box.


Prepare a floppy set
^^^^^^^^^^^^^^^^^^^^

Get a floppy set from http://mirror.slitaz.org/floppies/.
The base subset should be sufficient (6 floppies).

.. tip::
   Your may use one floppy only with `tiny slitaz <http://pizza.slitaz.org/tiny/>`_ for a manual install (without the slitaz-installer)

Build a data floppy set from the ISO image:

.. code-block:: console

   # echo "slitaz.iso" | cpio -o -H newc | split -b 1440k /dev/stdin iso
   # dd if=isoaa of=/dev/fd0
   # dd if=isoab of=/dev/fd0
   # ...

.. tip::
   Some `data floppy sets <http://mirror.slitaz.org/floppies/#fdiso>`_ can be generated on SliTaz mirrors


Transfer the ISO image onto hard disk
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Boot from the SliTaz floppy set, mount a hard disk partition in :file:`/mnt` and restore the data floppy set in :file:`/mnt`:

.. code-block:: console

   # cd /mnt
   # dd if=/dev/fd0 of=fdiso01
   # dd if=/dev/fd0 of=fdiso02
     ...
   # cat fdiso* | cpio -i
   # rm fdiso*

.. tip::
   If space is critical, replace the last 2 lines with:

   .. code-block:: shell

      for i in fdiso*; do cat $i; rm -f $i; done | cpio -i

Now you can use any install method from an ISO image.
Example:

.. code-block:: console

   # mount -o loop,ro slitaz.iso /media/cdrom
   # slitaz-installer

.. tip::
   .. compound::
      With `tiny slitaz <http://pizza.slitaz.org/tiny/>`_, you don't have the slitaz-installer.
      You can start a frugal install:

      .. code-block:: console

         # mkdir boot && cp /media/cdrom/boot/bzImage /media/cdrom/boot/rootfs.gz boot && rm slitaz.iso

      and optionally a standard install:

      .. code-block:: console

         # unlzma -c boot/rootfs.gz | cpio -id

      The problem: you have no boot loader!

   Boot the `SliTaz generic boot floppy <http://mirror.slitaz.org/boot/floppy-grub4dos>`_ with the :guilabel:`SliTaz frugal` menu entry.
   Now you can install the :program:`GRUB` bootloader on the hard disk.


.. _loop install:

Loop install
------------

If you want to install SliTaz on a disk (not a frugal install), and you don't want to create a partition for SliTaz, but you have enough room in a feature-poor filesystem (FAT32 or NTFS)…

… create a loop file and install SliTaz into it!

The problem is: what size?
200MB should be the minimum.
Imagine, you could like it and install many more packages!


Loopfile creation
^^^^^^^^^^^^^^^^^

You can create the loop file with :program:`mountbox` (click :guilabel:`loop`, enter the file name, then click :guilabel:`create`, enter the size, the units, click :guilabel:`create`) or with the command line:

.. code-block:: console

   # dd if=/dev/zero bs=1M count=200 of=slitaz.fs

You now need to create a filesystem in this loopfile:

.. code-block:: console

   # yes | mke2fs -j slitaz.fs

Later, if the loopfile is too small you can extend it (assuming you don't boot from the loopfile, but a Slitaz Live CD for example):

.. code-block:: console

   # dd if=/dev/zero bs=1M count=100 >> slitaz.fs
   # resize2fs slitaz.fs


Root filesystem files installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Copy files from the :file:`rootfs.gz` archive of a CD-ROM into the loopfile:

.. code-block:: console

   # mount /dev/cdrom /media/cdrom
   # mount -o loop,rw slitaz.fs /mnt
   # unlzma -c /media/cdrom/boot/rootfs.gz | ( cd /mnt; cpio -idmu )
   # umount -d /mnt
   # umount /media/cdrom


Boot setup
^^^^^^^^^^

Get a **preinit** ISO file with same version (the kernel version must match the modules version in the root filesystem).
The partition storing the loopfile (say :file:`/dev/hda1`) and its path into the partition (say :file:`/data/slitaz.fs`) is defined by the ``mount`` and ``loopfs`` arguments::

  title SliTaz cooking
    map (hd0,0)/boot/slitaz-preinit.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage mount=/dev/hda1 loopfs=data/slitaz.fs
    initrd (hd1)/boot/rootfs.gz

.. tip::
   The loop install does not use exotic packages from preinit.
   You can use any SliTaz flavor (except lorams).

.. tip::
   You can built an up-to-date **preinit** ISO anytime with

   .. code-block:: console

      # tazlito get-flavor preinit
      # tazlito gen-distro

::

  title SliTaz cooking in loop file
    map (hd0,0)/boot/slitaz-cooking.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage mount=/dev/hda1 loopfs=data/slitaz.fs
    initrd (hd1)/boot/rootfs.gz
  
  title SliTaz cooking in RAM (like the Live CD)
    map (hd0,0)/boot/slitaz-cooking.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage rw root=/dev/null autologin
    initrd (hd1)/boot/rootfs.gz

Or, you can replace the device name of the ``mount`` variable by the UUID or LABEL returned by :program:`blkid`::

  title SliTaz cooking
    map (hd0,0)/boot/slitaz-preinit.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage mount=a4b346ee-4c7b-46aa-9fd4-6bc39ab4fa96 loopfs=data/slitaz.fs
    initrd (hd1)/boot/rootfs.gz

.. tip::
   You can extract the :file:`bzImage` and :file:`rootfs.gz` from the ISO image to avoid :command:`map` commands and defragmentation.


.. _subdirectory install in a posix filesystem:

Subdirectory install in a Posix filesystem
------------------------------------------

If you want install SliTaz on a disk (not a frugal install), and you don't want to create a partition for SliTaz, but you have room in a filesystem for another Unix and you don't know how much space to reserve for SliTaz…

… create a subdirectory and install SliTaz into it!


Root filesystem files installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Simply install SliTaz file in a subdirectory (say :file:`/var/slitaz`) of another Linux partition:

.. code-block:: console

   # mkdir /mnt/var/slitaz
   # unlzma -c /media/cdrom/boot/rootfs.gz | ( cd /mnt/var/slitaz ; cpio -idmu )


Boot setup
^^^^^^^^^^

Like a loop install, you need a preinit ISO file with a matching version.
The partition (say :file:`/dev/hda1`) and the path into the partition are defined by the ``mount`` and ``subroot`` arguments::

  title SliTaz cooking
    map (hd0,0)/boot/slitaz-preinit.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage mount=/dev/hda1 subroot=var/slitaz
    initrd (hd1)/boot/rootfs.gz

Both notes in :ref:`loop install` section about :file:`bzImage` extraction and UUID/LABEL also apply here.

.. tip::
   The subdirectory install does not use exotic packages from preinit.
   You can use any SliTaz flavor (except lorams).

.. tip::
   The subdirectory install can be easily tested with a `Raspberry Pi`_ running Raspbian_ with the `tazbian <http://hg.slitaz.org/slitaz-arm/raw-file/tip/rpi/tazbian>`_ script.
   This script creates a :program:`raspbian` package from the latest SliTaz tarballs found on the `mirror <http://mirror.slitaz.org/arm/rpi/>`_.
   The installation of this package

   .. code-block:: console

      $ sudo dpkg -i slitaz-<VERSION>-1_armhf.deb

   will install SliTaz in :file:`/var/os/slitaz` and setup a multiboot.
   It does not remove rasbian or alter partitions.


Subdirectory install in a non-Posix filesystem
----------------------------------------------

You want to install SliTaz in a subdirectory but the filesystem (NTFS_ or VFAT_) does not fully support UNIX features.

Use `posixovl <http://mirror.slitaz.org/pkgs/?package=posixovl>`_!


Root filesystem files installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You need to mount the target subdirectory (say :file:`/slitaz`) with :program:`posixovl` **before** installing the files.

.. code-block:: console

   # mkdir /mnt/slitaz
   # mount.posixovl /mnt/slitaz
   # unlzma -c /media/cdrom/boot/rootfs.gz | ( cd /mnt/slitaz ; cpio -idmu )

.. tip::
   Windows users can extract the archive http://mirror.slitaz.org/iso/4.0/slitaz-4.0.zip and look at the file :file:`\\slitaz\\boot\\install.txt`


Boot setup
^^^^^^^^^^

Like a loop install, you need a *preinit* ISO file with a matching version.
The partition (say :file:`/dev/hda1`) and the path into the partition are defined by the ``mount``, ``subroot`` and **posixovl** arguments::

  title SliTaz cooking
    map --mem --heads=0 --sectors-per-track=0 (hd0,0)/boot/slitaz-preinit.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage mount=/dev/hda1 subroot=slitaz posixovl
    initrd (hd1)/boot/rootfs.gz

Both notes in :ref:`loop install` section about :file:`bzImage` extraction and UUID/LABEL also apply here.


Extra setup
^^^^^^^^^^^

You want to see the host partition while running SliTaz like UMSDOS_ does with :file:`/DOS`.

Create the mount point:

.. code-block:: console

   # mkdir /mnt/slitaz/Windows

And update the boot arguments::

  title SliTaz cooking
    map (hd0,0)/boot/slitaz-preinit.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage mount=/dev/hda1 subroot=slitaz posixovl bindfs=.,slitaz/Windows
    initrd (hd1)/boot/rootfs.gz


LVM install
-----------

The Logical Volume Manager can manage (add disks, replace disks…) and logically freeze any disks for backup (snapshots) without disrupting service.
See LVM_


LVM partition setup
^^^^^^^^^^^^^^^^^^^

A small amount of storage (depending on the disk activity, likely between 1% and 15%) is used by snapshots to hold frozen data during a backup.
Assuming we use the :file:`sda1` partition with 5% reserved for snapshots:

.. code-block:: console

   # tazpkg get-install lvm2
   # modprobe dm-mod
   # pvcreate /dev/sda1
   # vgcreate slitaz /dev/sda1
   # lvcreate -l 95%VG slitaz -n root 
   # mke2fs -j /dev/mapper/slitaz-root        
   # tune2fs -c 0 -i 0 /dev/mapper/slitaz-root
   # mount /dev/mapper/slitaz-root /mnt


Root filesystem files installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Similar to a loop install:

.. code-block:: console

   # unlzma -c /media/cdrom/boot/rootfs.gz | ( cd /mnt ; cpio -idmu )


Boot setup
^^^^^^^^^^

Like a loop install, you need a **preinit** ISO file with a matching version.
The argument **lvmroot** holds the volume name::

  title SliTaz cooking
    map (hd0,0)/boot/slitaz-preinit.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage lvmroot=slitaz-root
    initrd (hd1)/boot/rootfs.gz


RAID install
------------


Hardware RAID
^^^^^^^^^^^^^

Full hardware RAID_ is transparent for SliTaz.
The disk array is seen as a single disk and nothing special has to be done to install SliTaz.


Semi hardware RAID
^^^^^^^^^^^^^^^^^^


Creation & installation
~~~~~~~~~~~~~~~~~~~~~~~

The RAID_ array is built with the BIOS menus.
SliTaz needs the driver **dmraid** to see the array and not only each hard disk:

.. code-block:: console

   # tazpkg get-install lvm2
   # tazpkg get-install dmraid
   # dmraid -s            # shows raid infomation
   # modprobe raid1       # could be raid0, raid456 or raid10
   # dmraid -ay           # activates the array in /dev/mapper
   # mount /etc/mapper/myraid /media
   # unlzma -c /media/cdrom/boot/rootfs.gz | ( cd /mnt ; cpio -idmu )


Boot setup
~~~~~~~~~~

Like a loop install, you need a **preinit** ISO file with a matching version.
The argument **dmraid** holds the volume name::

  title SliTaz cooking
    map (hd0,0)/boot/slitaz-preinit.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage dmraid=myraid
    initrd (hd1)/boot/rootfs.gz


Software RAID
^^^^^^^^^^^^^

The array does not need the BIOS and can be fully administered remotely!

.. tip::
   You should tune the **preinit** flavor to your needs.
   Enable the :program:`dropbear` startup in :file:`/etc/rcS.conf` and maybe install a VPN.
   If the software RAID does not start on startup, you will be able to fix it remotely…


Creation & installation
~~~~~~~~~~~~~~~~~~~~~~~

Example for mirroring (raid1) devices :file:`/dev/sda3` and :file:`/dev/sdb3`:

.. code-block:: console

   # tazpkg get-install lvm2
   # tazpkg get-install mdadm
   # echo y | mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sda3 /dev/sdb3 --bitmap=internal --assume-clean
   # modprobe raid1
   # mdadm --assemble --scan
   # mount /dev/md0 /media
   # unlzma -c /media/cdrom/boot/rootfs.gz | ( cd /mnt ; cpio -idmu )


Boot setup
~~~~~~~~~~

Like a loop install, you need a preinit ISO file with a matching version.
The argument **softraid** holds the device name::

  title SliTaz cooking
    map (hd0,0)/boot/slitaz-preinit.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage softraid=/dev/md0
    initrd (hd1)/boot/rootfs.gz


Crypto install
--------------

.. note::
   Encrypts the whole `root filesystem <Disk encryption_>`_, not just the :file:`/home` partition.
   Important, because the files in :file:`/tmp`, :file:`/var/tmp` may betray your work.
   The swap does too unless you use a file instead of a partition (like :file:`/tmp/swapfile`; this will be encrypted too because this file is in the root filesystem)


LUKS
^^^^

LUKS_ replaces the Cryptoloop_ and Loop-AES formats now.


Creation & installation
~~~~~~~~~~~~~~~~~~~~~~~

Create the encrypted device with :program:`mountbox` (:guilabel:`crypto` button).
You may have to accept the missing packages installation.
Select the device (say :file:`/dev/sda3`) and click the :guilabel:`create` button.
Now you can start to format it:

.. code-block:: console

   # mke2fs -j /dev/mapper/crypto-sda3
   # tune2fs -c 0 -i 0 /dev/mapper/crypto-sda3
   # mount /dev/mapper/crypto-sda3 /media
   # unlzma -c /media/cdrom/boot/rootfs.gz | ( cd /mnt ; cpio -idmu )


Boot setup
~~~~~~~~~~

Like a loop install, you need a preinit ISO file with a matching version.
The argument ``cryptoroot`` holds the volume name::

  title SliTaz cooking
    map (hd0,0)/boot/slitaz-preinit.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage cryptoroot=sda3
    initrd (hd1)/boot/rootfs.gz


Loop-AES compatibility
^^^^^^^^^^^^^^^^^^^^^^

This deprecated format needs the same **boot setup** as LUKS.


Mixed install
-------------

You can mix several above methods using one device access and/or one filesystem access.

========================= =============================
Device access             Filesystem access
========================= =============================
``mount=``                ``subroot=``
``*raid=``                ``loopfs=``
``lvmroot=``              ``cryptoroot=``
``*raid=`` + ``lvmroot=`` ``loopfs=`` + ``cryptoroot=``
========================= =============================


Example 1 : RAID + LVM
^^^^^^^^^^^^^^^^^^^^^^


Example 2 : Loop + crypto
^^^^^^^^^^^^^^^^^^^^^^^^^


Possible improvements?
^^^^^^^^^^^^^^^^^^^^^^

Add network support: nbd/iscsi + RAID 1 net&local + crypto


PXE: No install!
----------------

You can :ref:`setup a PXE server <pxe server set up>` (well… you need to configure your server) or a :ref:`PXE forwarder <advanced web booting configuration>`, see the *Embedded Web Boot with PXE boot PROM* (the SliTaz team has `configured the server for you <http://boot.slitaz.org>`_ 8-))


.. _Raspberry Pi:    http://en.wikipedia.org/wiki/Raspberry_Pi
.. _Raspbian:        http://en.wikipedia.org/wiki/Raspbian
.. _NTFS:            http://en.wikipedia.org/wiki/NTFS
.. _VFAT:            http://en.wikipedia.org/wiki/VFAT
.. _UMSDOS:          http://en.wikipedia.org/wiki/UMSDOS
.. _LVM:             http://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)
.. _RAID:            http://en.wikipedia.org/wiki/RAID
.. _Disk encryption: http://en.wikipedia.org/wiki/Disk_encryption
.. _LUKS:            http://en.wikipedia.org/wiki/LUKS
.. _Cryptoloop:      http://en.wikipedia.org/wiki/Cryptoloop
