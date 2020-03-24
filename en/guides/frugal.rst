.. http://doc.slitaz.org/en:guides:frugal
.. en/guides/frugal.txt · Last modified: 2013/03/22 10:02 by bellard

.. _frugal:

Winboot
=======

:author: jozee, linea, bellard, mojo


Frugal install to Microsoft Windows partition
---------------------------------------------

Install Grub4dos_.

This article will assume that your :file:`C:` drive is ``(hd0,0)``.
You need at least 160MB to run SliTaz cooking (128MB for Slitaz 1.0).

Tested with Windows XP on FAT32 and on NTFS.

For Windows XP, 2000 or NT, copy grldr_ into :file:`C:\\` and append to :file:`C:\\boot.ini`::

  C:\grldr="Grub4DOS"


.. _traditional frugal install:

Traditional frugal install
--------------------------

Copy bzImage_ and rootfs.gz_ (both <30MB) into :file:`C:\\boot` and append to :file:`C:\\menu.lst` the following text::

  title SliTaz cooking
    kernel (hd0,0)/boot/bzImage rw root=/dev/null vga=normal
    initrd (hd0,0)/boot/rootfs.gz

Works fine with Windows XP on FAT32 and on NTFS.


.. _iso image install:

ISO image install
-----------------

Copy slitaz-cooking.iso_ (<30MB) into :file:`C:\\boot`.
Run defragmentation on :file:`C:`.
And append to :file:`C:\\menu.lst` the following text::

  title SliTaz cooking
    map (hd0,0)/boot/slitaz-cooking.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage rw root=/dev/null vga=normal
    initrd (hd1)/boot/rootfs.gz

Works fine with Windows XP on FAT32 and on NTFS.
Using version 0.4.3.
The file :file:`C:\\boot\\slitaz-cooking.iso` MUST be defragmented.

.. tip::
   Anyway, you can always boot from an ISO image on a fragmented filesystem with::

     title SliTaz cooking
       map --mem --heads=0 --sectors-per-track=0 (hd0,0)/boot/slitaz-cooking.iso (hd1)
       map --hook
       kernel (hd1)/boot/bzImage rw root=/dev/null vga=normal
       initrd (hd1)/boot/rootfs.gz

You need 160MB or 256MB of RAM to run the :file:`slitaz-cooking.iso`.
If you have less RAM try: slitaz-cooking-base.iso_ or slitaz-cooking-justX.iso_ or slitaz-loram.iso_ instead::

  title SliTaz cooking base
    map (hd0,0)/boot/slitaz-cooking-base.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage rw root=/dev/null vga=normal
    initrd (hd1)/boot/rootfs.gz

See http://community.slitaz.org/wiki/Loram


Web boot install
----------------

Copy gpxe_ (<200KB) into :file:`C:\\boot` and append to :file:`C:\\menu.lst` the following text::

  title SliTaz web
    kernel (hd0,0)/boot/gpxe

Gpxe is provided by the SliTaz Live CD too::

  title SliTaz web
    map (hd0,0)/boot/slitaz-cooking.iso (hd1)
    map --hook
    kernel (hd1)/boot/gpxe

This will boot the latest available version of SliTaz.
See http://boot.slitaz.org/.
Works fine with Windows XP on FAT32 and on NTFS.


.. _tuning the boot process:

Tuning the boot process
-----------------------

Additional boot parameters may be appended.
For example::

  title SliTaz cooking
    map (hd0,0)/boot/slitaz-cooking.iso (hd1)
    map --hook
    kernel (hd1)/boot/bzImage rw root=/dev/null vga=extended lang=fr_FR kmap=fr-latin1 laptop autologin config=/dev/hda1,boot/slitaz.sh
    initrd (hd1)/boot/rootfs.gz

The file :file:`/boot/slitaz.sh` in :file:`/dev/hda1` will be executed at the end of the boot script::

  initrd (hd1)/boot/rootfs.gz (hd0,0)/boot/extra-softwares.gz (hd0,0)/boot/my-config-files.gz

The compressed cpio archives :file:`/boot/extra-softwares.gz` and :file:`/boot/my-config-files.gz` will be loaded after the official initramfs :file:`rootfs.gz`.
This is faster than using the previous :file:`/boot/slitaz.sh` script.
Example:

.. code-block:: console

   # find /etc/rcS.conf /etc/daemons.conf /etc/dropbear /home/tux/.ssh | cpio -o -H newc | gzip -9 > /boot/my-config-files.gz


Automated Graphical Approach using UNetbootin
---------------------------------------------

A SliTaz Frugal Install or a Live USB installation can be performed using UNetbootin_.

.. warning::
   **Unetbootin will not work with the main 4.0 release slitaz-4.0.iso or slitaz-rolling.iso** which are core 4-in-1 ISO's with multi-rootfs.gz without manually editing the boot menu or combining the multi-rootfs.gz into one.

   Please use slitaz-4.0-core.iso_ or another single rootfs.gz flavor found here_.

Either the standard version (Windows_ | Linux_) can be used (select SliTaz from the Distribution list), or a custom SliTaz version (Windows_ | Linux_) may also be used.

To perform a Frugal Install, select :guilabel:`Hard Disk` under installation type; to create a Live USB, select :guilabel:`USB Drive` under installation type.


.. _Grub4dos:                 http://grub4dos.sourceforge.net/wiki/index.php/Grub4dos_tutorial#Installation
.. _grldr:                    http://mirror.slitaz.org/boot/grldr
.. _bzImage:                  http://download.tuxfamily.org/slitaz/boot/cooking/bzImage
.. _rootfs.gz:                http://download.tuxfamily.org/slitaz/boot/cooking/rootfs.gz
.. _slitaz-cooking.iso:       http://download.tuxfamily.org/slitaz/iso/cooking/slitaz-cooking.iso
.. _slitaz-cooking-base.iso:  http://mirror.slitaz.org/iso/cooking/flavors/slitaz-cooking-base.iso
.. _slitaz-cooking-justX.iso: http://mirror.slitaz.org/iso/cooking/flavors/slitaz-cooking-justX.iso
.. _slitaz-loram.iso:         http://mirror.slitaz.org/iso/cooking/flavors/slitaz-loram.iso
.. _gpxe:                     http://download.tuxfamily.org/slitaz/boot/gpxe
.. _UNetbootin:               http://unetbootin.sourceforge.net/
.. _slitaz-4.0-core.iso:      http://mirror.slitaz.org/iso/4.0/flavors/slitaz-4.0-core.iso
.. _here:                     http://mirror.slitaz.org/iso/4.0/flavors/
.. _Windows:                  http://unetbootin.sourceforge.net/unetbootin-windows-latest.exe
.. _Linux:                    http://unetbootin.sourceforge.net/unetbootin-linux-latest
