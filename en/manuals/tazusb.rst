.. _tazusb manual:

TazUSB Manual
=============


NAME
----

:program:`TazUSB` — SliTaz LiveUSB utility


SYNTAX
------

.. code-block:: shell

   tazusb [command] [compression|device|file]


DESCRIPTION
-----------

:program:`TazUSB` is a utility designed for installing SliTaz to a USB drive.
Unlike a hard drive install, the filesystem is kept in a compressed :file:`rootfs.gz` file.
The filesystem is loaded entirely into memory upon boot.
This should increase responsiveness, protect the filesystem against accidental corruption and reduce read/writes to the USB drive.
Once setup, this utility can also rewrite the root filesystem with any changes you have made since booting up, giving the effective benefits of a hard drive install.

:program:`TazUSB` supports FAT32/EXT3/EXT2 formatted drives using SYSLINUX and EXTLINUX respectively.
:file:`/home` is mounted on boot using the UUID of your particular flash drive.
Unlike a device name, the UUID has the benefit of never changing from machine to machine.


COMMANDS
--------

:usage:
  The command 'usage' will display a short summary of all available commands.

  .. code-block:: console

     # tazusb usage

:writefs:
  The command 'writefs' will take the current memory resident filesystem and create a :file:`rootfs.gz` file.
  If your flash drive is mounted as :file:`/home` (as it should be), the new filesystem will be copied to the drive for you, otherwise it is left on the root of the drive.
  Your previous filesystem will be renamed to :file:`previous.gz` and can be accessed on bootup by typing "previous" at the "boot:" prompt.
  All previous filesystems are renamed to :file:`rootfs.gz.{unixtimestamp}`.
  These are not removed automatically, so you should periodically delete these to keep disk usage down.

  Filesystem compression is supported in the form of lzma, gzip or none.
  Using no compression is very quick (under 5 seconds) and useful if you are experimenting with a lot of changes.
  By comparison, using lzma or gzip takes a few minutes but will dramatically reduce file size.
  This is recommended when committing permanent changes to the filesystem.

  .. code-block:: console

     # tazusb writefs compression

  Example:

  .. code-block:: console

     # tazusb writefs lzma

:format:
  The command 'format' is used for formatting a device for use as a LiveUSB device.
  Currently, it supports formatting as EXT2, EXT3 and FAT32.

  .. code-block:: console

     # tazusb format /dev/name

  Example:

  .. code-block:: console

     # tazusb format /dev/sda1

:gen-liveusb:
  "gen-liveusb" will install a fresh MBR, set your partition as bootable and install syslinux/extlinux depending on the detected filesystem.
  It will then copy the kernel and filesystem from the CD-ROM drive, and place this on the target USB drive.
  This will leave you with a bootable USB copy of SliTaz.

  .. code-block:: console

     # tazusb gen-liveusb /dev/name

  Example:

  .. code-block:: console

     # tazusb gen-liveusb /dev/sda1


:gen-swap:
  The 'gen-swap' command (re)creates a virtual swap file and places it in the :file:`/home` directory to be activated on each boot.
  This is useful for old systems with low memory.

  .. code-block:: console

     # tazusb gen-swap

:gen-iso2usb:
  This command performs the same task as gen-liveusb, only copying the Kernel and filesystem from a downloaded ISO image instead of the CD-ROM drive.

  .. code-block:: console

     # tazusb gen-iso2usb /path/to/iso

  Example:

  .. code-block:: console

     # tazusb gen-iso2usb /home/tux/slitaz.iso

:clean:
  "clean" removes old :file:`rootfs.gz.{unixtimestamp}` filesystems (see writefs) to keep disk usage down.

  .. code-block:: console

     # tazusb clean


MAINTAINER
----------

Eric Joseph-Alexandre <erjo@slitaz.org>
