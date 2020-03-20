.. http://doc.slitaz.org/en:guides:dvd
.. en/guides/dvd.txt · Last modified: 2010/07/08 17:14 (external edit)

.. _dvd:

All SliTaz resources on one DVD
===============================

A bootable DVD image of all available packages version is generated every day.
See http://www.slitaz.org/en/get/.
Both *cooking* or *stable* versions are built; they contain:

* a Slitaz boot
* a copy of the website
* a snapshot of the wok
* all official packages

It is designed to be used without an internet connection.


.. _dvd hybrid iso:

Hybrid ISO
----------

The ISO image should be burned onto a DVD using:

.. code-block:: console

   # wodim -v dev=/dev/dvd packages-cooking.iso

It can also be written onto a USB key (say the key is :file:`/dev/usbkey`).
USE WITH CAUTION this will erase all your data.

.. code-block:: console

   # dd if=packages-cooking.iso of=/dev/usbkey

After writing the USB key, you can create partitions to get back the remaining storage.

.. code-block:: console

   # fdisk /dev/usbkey

The USB key is bootable.
If your BIOS doesn't support USB boot, you can boot :program:`plop` from http://www.slitaz.org/en/get/#floppy floppy.


Burnable on CD-ROM
------------------

The files on the ISO image are ordered in 5 groups:

* the boot files
* the loram boot files
* the website
* the wok
* the packages

The four first groups fit in the first 100-200Mb of the ISO image.
If you don't need the packages, you can burn it onto a mini CD-ROM (8cm/210Mb)

.. code-block:: console

   # wodim -v dev=/dev/cdrom packages-cooking.iso

The packages are stored in alphabetical order.
:program:`915resolution` should likely be present but :program:`zsh` surely not.
The directories are stored at the beginning of the image and are always present.
If you try to load a non-present package (:program:`zsh`) you will get an I/O error.


Dual boot
---------

Both *core* and *loram* (*loram-cdrom* to be exact) boot files are stored in the ISO image.
The default boot entry (slitaz) will select which to start according to your RAM size.
You can force the choice with *core* or *loram* keyword::

  boot: loram screen=1280x1024

With core boot you can:

* unmount the DVD (:command:`umount /cdrom`)
* test (install) packages in RAM

You can't do that with loram boot because it has :file:`/usr` mounted read-only from the DVD.
You can install SliTaz on a hard disk with both boot files.


Auto install
------------

During boot the following is performed:

* the DVD is mounted in :file:`/cdrom`, the wok is onto :file:`/home/slitaz/wok`
* a pseudo tazpkg recharge links packages from the DVD
* the SliTaz web site is installed in file:///cdrom/website/index.html

Note: USB key users should read USB key instead of DVD
