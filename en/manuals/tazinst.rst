.. _tazinst manual:

Tazinst Manual
==============


NAME
----

:program:`Tazinst` — Tiny autonomous zone installer manager


SYNTAX
------

.. code-block:: shell

   tazinst [command] <setting> <value> <file>


DESCRIPTION
-----------

:program:`Tazinst` is a lightweight SliTaz HDD installer.
It installs SliTaz to a hard drive from any local media such as a Live-CD, a LiveUSB key, an ISO image located on one of your disks, or from the web by automatically downloading a SliTaz image.

:program:`Tazinst` can format the target partition to ext2, ext3 or ext4.
The home partition can be installed on another partition and if need be, formatted before installation into any one of the available formats.
:program:`Tazinst` may upon request install a bootloader on the target disk.
A dual-boot with an existing Windows™ partition is possible — finding the Windows™ partition can either be done automatically or manually specified.

:program:`Tazinst` can also update SliTaz installed on a hard disk partition which is handy in case of version changes.
In this case, SliTaz is updated, any data in :file:`/home` is preserved and additional packages are reinstalled on to the new version.

:program:`Tazinst` was created independently for the needs of the SliTaz GNU/Linux mini distribution.

:program:`Tazinst` is written from scratch in shell script and is compatible with Busybox Ash and Bash.
:program:`Tazinst` is licensed under the GNU Free GPL v3.


Known limitations
^^^^^^^^^^^^^^^^^

:program:`Tazinst` doesn't allow SliTaz to boot on (U)EFI systems (mostly Windows™ 8 systems), except in BIOS compatibility mode.


SETTINGS
--------

:program:`Tazinst` installer is able to perform an installation automatically based on a few settings.


:mode:
  Installation mode that will be performed by :program:`tazinst`.
  Type :command:`tazinst help mode` in order to have a list of supported modes.

:media:
  The media containing the SliTaz source files, either **cdrom** (SliTaz LiveCD), **usb** (SliTaz LiveUSB), **iso** (ISO image of SliTaz), or **web** (ISO image on the Web).

:source:
  The name of the source file containing SliTaz.
  It depends on the type of **media**:

  .. list-table::
     :widths: 1 8

     * - cdrom:
       - *unused*
     * - usb:
       - name of the partition on the host USB device.
         Type :command:`tazinst list usb` to list USB partitions.
     * - iso:
       - name of the ISO file, example: :file:`~/slitaz-rolling.iso`.
         Type :command:`tazinst list iso` to list ISO files on your disks.
     * - web:
       - name of the image on the web, example: ``stable cooking rolling base core gtkonly justx``, for a full list type :command:`tazinst list web`, or enter the full URL of the image, example: ``http://mirror.slitaz.org/iso/cooking/slitaz-cooking.iso``.

:root_uuid:
  The name of the target partition SliTaz will install to.
  Type :command:`tazinst list uuid` to list partitions on your disks.

:root_format:
  Optional.
  If this setting is used, the target partition will be formatted in the file system specified, otherwise the partition will be cleaned and :file:`/home` will be preserved.
  Type :command:`tazinst help format` to get the list of all supported filesystems, and :command:`tazinst list root_format` to see filesystems already installed on your system.

:home_uuid:
  Optional.
  A separate home partition may be created if needed.
  This setting indicates if need be, the name of the partition to receive the :file:`/home` directory.

:home_format:
  Optional.
  If this setting is used, a separate :file:`/home` partition will be created, this partition will be formatted in the file system specified.

:hostname:
  Optional.
  Hostname of the system, ``slitaz`` by default.

:root_pwd:
  Optional.
  Superuser [root] password, ``root``, by default.

:user_login:
  Optional.
  First user name, ``tux`` by default.

:user_pwd:
  Optional.
  First user password, ``tux`` by default.

:bootloader:
  Optional.
  Install a bootloader.
  Usually you should set it to ``auto`` unless you want to use an already installed bootloader on your system, install a bootloader by hand yourself or install a specific bootloader.
  In this case type :command:`tazinst help bootloader` to list supported bootloaders.

:winboot:
  Optional.
  If a bootloader is installed, this setting indicates the partition containing Windows™ to implement a dual-boot.
  It can also be set to ``auto``, in this case the dual-boot will be on the first Windows™ partition.
  Type :command:`tazinst list winboot` to see values :program:`tazinst` automatically detects.


COMMANDS
--------

:new:
  Generates a new self-documenting install file containing settings which, when set up as required by the user, will allow :program:`tazinst` to execute an unattended installation.
  The default file is :file:`./tazinst.rc`, but an optional file name may be given as a parameter.

  Examples::

    tazinst new
    tazinst new /var/lib/tazinst.conf

:set:
  Assign a new value to a given setting.

  Examples::

    tazinst set mode install
    tazinst set mode install /var/lib/tazinst.conf

:unset:
  Unset, clears a setting.

  Examples::

    tazinst unset mode
    tazinst unset mode /var/lib/tazinst.conf

:get:
  Get the value of a setting.
  Without a parameter, gets the values of all settings.

  Examples::

    tazinst get
    tazinst get mode
    tazinst get mode /var/lib/tazinst.conf

:check:
  Check a setting for errors.
  Without a parameter, checks all settings.

  Examples::

    tazinst check
    tazinst check mode
    tazinst check mode /var/lib/tazinst.conf

:list:
  List the system resources.
  Resources are:

  .. list-table::
     :widths: 1 4

     * - mode
       - Available modes of install
     * - media
       - Available media to install from.
         Example: cdrom is not listed on systems with no cdrom drive
     * - usb
       - Partitions of USB disks
     * - iso
       - ISO images located on local drives, in :file:`/root`, and in all user's home and first subdirectory
     * - web
       - Predefined names of ISO images to download automatically from the Internet
     * - format
       - Installed filesystems
     * - bootloader
       - Available bootloaders
     * - partition_table
       - Partition table schemes of local disks
     * - winboot
       - Bootable Windows™ partitions

  Examples::

    tazinst list
    tazinst list media

:execute:
  Performs a SliTaz install on a HDD based on data in the install file.
  If you selected to format your HDD, all data will be lost.
  If you do not, all data except for any existing :file:`/home` directory will be removed, (the home directory will be kept as is).

  Examples::

    tazinst execute
    tazinst execute /var/lib/tazinst.conf

:clean:
  Remove installation and log files.

  Examples::

    tazinst clean
    tazinst clean /var/lib/tazinst.conf

:log:
  Display the last log file contents and exit.

  Example::

    tazinst log

:version:
  Print the version information and exit.

  Example::

    tazinst version

:usage:
  Print a short help and exit.

  Example::

    tazinst usage

:help:
  Print a short help for a given setting and exit.
  Without an argument, print a short help for all settings.

  Example::

    tazinst help mode


EXAMPLES
--------


Install
^^^^^^^

How to Install SliTaz on a partition of your hard disk drive.
The root partition is not formatted, all data except for any existing :file:`/home` directory will be removed, (the :file:`home` directory will be kept as is).

#. Create an install file:

   .. code-block:: console

      # tazinst new

#. Set the mode as install:

   .. code-block:: console

      # tazinst set mode install

#. Use a CD-ROM as source:

   .. code-block:: console

      # tazinst set media cdrom

#. Select the partition to install SliTaz on:

   .. code-block:: console

      # tazinst set root_uuid /dev/hda1

#. Install a bootloader:

   .. code-block:: console

      # tazinst set bootloader auto

#. Execute an installation:

   .. code-block:: console

      # tazinst execute


Complex Install
^^^^^^^^^^^^^^^

How to Install SliTaz on your hard disk drive with a separate home partition and a Windows™ dual-boot.
The :file:`/home` and root partitions are both formatted, (all existing data will be lost).

#. Create an install file:

   .. code-block:: console

      # tazinst new

#. Set the mode as install:

   .. code-block:: console

      # tazinst set mode install

#. Use a Live USB as source:

   .. code-block:: console

      # tazinst set media usb

#. Select a partition on the Live USB:

   .. code-block:: console

      # tazinst set source /dev/sda1

#. Select the partition to install SliTaz on:

   .. code-block:: console

      # tazinst set root_uuid /dev/hda1

#. Format :file:`/` as ext4:

   .. code-block:: console

      # tazinst set root_format ext4

#. Use a separate :file:`/home` partition:

   .. code-block:: console

      # tazinst set home_uuid /dev/hda2

#. Format :file:`/home` as ext2:

   .. code-block:: console

      # tazinst set home_format ext2

#. Install a bootloader:

   .. code-block:: console

      # tazinst set bootloader auto

#. Set up a Windows™ dual-boot:

   .. code-block:: console

      # tazinst set winboot auto

#. Execute an installation:

   .. code-block:: console

      # tazinst execute

#. Remove any traces behind:

   .. code-block:: console

      # tazinst clean


Upgrade
^^^^^^^

How to upgrade an already installed SliTaz system on your hard disk drive.
Your :file:`/home` :file:`/etc` :file:`/var/www` directories will be kept, all other directories will be removed.
Any additional packages added to your old SliTaz system will be updated as long you have an active internet connection.

#. Create an install file:

   .. code-block:: console

      # tazinst new

#. Set the mode as upgrade:

   .. code-block:: console

      # tazinst set mode upgrade

#. Use web as source:

   .. code-block:: console

      # tazinst set media web

#. Select the stable image:

   .. code-block:: console

      # tazinst set source stable

#. Select the partition containing SliTaz to upgrade:

   .. code-block:: console

      # tazinst set root_uuid /dev/hda1

#. Install a bootloader:

   .. code-block:: console

      # tazinst set bootloader auto

#. Execute an installation:

   .. code-block:: console

      # tazinst execute


Tips
^^^^

#. Not all settings are used depending on the mode of install.
   List all settings to see which you need to edit:

   .. code-block:: console

      # tazinst get

#. Check your settings before executing install:

   .. code-block:: console

      # tazinst check


FILES
-----


INSTALL FILE
^^^^^^^^^^^^

Settings are saved in the install file, then used by :program:`tazinst` to execute an unattended installation.
The default file is :file:`./tazinst.rc`, but an optional file name may be given as a parameter.
The install file is self-documented.
The ``clean`` command erases this file.


SYSTEM FILE
^^^^^^^^^^^

The :file:`/etc/slitaz/tazinst.conf` configuration file allows you to change the default settings of :program:`tazinst` in case you want to use default custom values.

All settings are customisable, if a particular setting is missing, just add the name of the setting in caps and enter the new value.

Example:

If you intend to always install SliTaz from the same ISO on the web, you just have to modify or add the following values:

.. code-block:: shell

   MEDIA="web"
   SOURCE="stable"


LOG FILE
^^^^^^^^

The file :file:`/var/log/tazinst.log` contains a log of the install process.
The ``clean`` command erases this file.


MAINTAINERS
-----------

* Christophe Lincoln <pankso@slitaz.org>
* Dominique Corbex <domcox@slitaz.org>
