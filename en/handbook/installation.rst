.. http://doc.slitaz.org/en:handbook:installation
.. en/handbook/installation.txt · Last modified: 2015/04/16 16:22 by bellard

.. _handbook installation:

Hard Disk Installation
======================

:author: jozee, linea, gokhlayeh, seawolf, bellard


Introduction
------------

This document gives information and necessary instructions on how to install SliTaz on a hard disk.
This should take about 10 minutes, SliTaz core LiveCD expands to 80 MB, so we suggest a minimum of 120 MB of free space.
This way you will be able to install a few more packages.
If you can use the LiveCD, you should be able to install SliTaz.
You may also do a :ref:`frugal <frugal>` or :ref:`unusual <uncommoninst>` install.


SliTaz Installer
----------------

SliTaz provides a simple to use Installer which can be launched from the menu on Tazpanel.
SliTaz Installer messages are in English and can be used with these complimentary instructions.


.. rubric:: Install Type

The first step lets you choose the type of installation: new install or system upgrade.
In most cases you will want a new and clean installation.
On confirming this the installer will mount the master CD-ROM device and search for the compressed file-system (:file:`rootfs.gz`).
If no file-system is found, then the installation will abort.

If you get into trouble because the compressed file-system is not found, please check that SliTaz is in the master CD/DVD device.
If the problem persists you can use a downloaded ISO image and mount it on :file:`/media/cdrom` where the installer expects to find it:

.. code-block:: console

   # mount -o loop slitaz-3.0.iso /media/cdrom


.. rubric:: Source media

Here you can choose the type of media to install SliTaz from.
Either from a LiveCD, LiveUSB, a downloaded ISO file or directly from the web.


.. rubric:: Target Partition

Next is the partition configuration.
You will need to have a partition ready; the installer does not set-up your disk for you.
If you already have a free partition you can use it; if not — you will have to create one graphically using :program:`GParted`, or from the command line using :command:`fdisk`.

For example, if you want to install SliTaz on the second partition of the first disk recognized as :file:`hda`::

  /dev/hda2


.. rubric:: Formatting

.. warning::
   Formatting a partition **permanently** removes all data from it.
   Be sure you choose your intended partition carefully as *this action is irreversible*.

The next step lets you format the target partition.
Ext3 is a robust, stable and journalled file-system.
If the partition is already formatted you can skip this step, if not just accept.


.. rubric:: Home partition

A separate home partition can be created and also formatted.


.. rubric:: Hostname

Hostname configuration lets you set the machine name.
The hostname is used internally and to identify the computer on a network.
This can be changed after the system is installed.
It cannot be longer than 64 characters and can only contain letters, numbers, and dashes.


.. rubric:: Root

The root password can be configured here.


.. rubric:: User

This allows you to configure a user name and password.


.. rubric:: Boot-Loader (GRUB)

With the next step you have the option to install the :program:`GRUB` boot-loader and enable a Windows dual-boot.
:program:`GRUB` is capable of booting almost any kind of operating system and can be configured through a human-readable text file; changes to this file are instant and do not require any additional commands to take effect.

If you want to use an existing :program:`GRUB` installation, skip this step and add the correct lines to your :program:`GRUB` configuration file (:file:`menu.lst`); :ref:`see below <grub boot-loader>` for more information.
Note that the SliTaz Installer creates a configuration file on the target which can be used as an example (:file:`/mnt/target/boot/grub/menu.lst`).


.. rubric:: Finishing the Installation

When the Installer has finally done its job you have the option to exit or directly reboot your new SliTaz GNU/Linux operating system.
First boot is like the LiveCD, you will be prompted for options.
Future reboots will not prompt you anymore for configuration details, but all the values can be changed either manually or with the project tools such as :program:`tazlocale` or :program:`tazx`.


Manual ('By Hand') Installation
-------------------------------

SliTaz can also be installed 'by hand' from the command line.
You can use a CD-ROM or an ISO image.
The following commands can be copied/pasted from your web browser to the Terminal.

Firstly, prepare a target partition and mount it.
For example, to use the second partition on the first disk drive (:file:`/dev/hda2`), one would type:

.. code-block:: console

   # mkdir /mnt/target
   # mount /dev/hda2 /mnt/target


.. rubric:: Mount CD-ROM or ISO image

Mount the CD-ROM…

.. code-block:: console

   # mount /dev/cdrom /media/cdrom

…or if you are using an ISO image:

.. code-block:: console

   # mount -o loop slitaz-3.0.iso /media/cdrom


.. rubric:: Install and Extract

With a target partition prepared and the installation media made accessible, we need to copy the files from the media into the target partition and then extract the compressed file-system (:file:`rootfs.gz`).

* Create a boot directory and install the Linux Kernel file:

  .. code-block:: console

     # mkdir /mnt/target/boot
     # cp -a /media/cdrom/boot/vmlinuz-* /mnt/target/boot

* Copy the root file-system:

  .. code-block:: console

     # cp /media/cdrom/boot/rootfs.gz /mnt/target

  .. note::
     Since SliTaz 4.0, multiple rootfs should be copied.

     .. code-block:: console

        # cp /media/cdrom/boot/rootfs* /mnt/target

     You can also copy :file:`rootfs4.gz` only to get minimum the text mode installation

Now the necessary files are present, change (:command:`cd`) to the target directory and decompress the file-system.
This is done with the :command:`lzma` and :command:`cpio` utilities:

.. code-block:: console

   # cd /mnt/target
   # unlzma < rootfs.gz | cpio -id 
   # rm rootfs.gz init

.. note::
   For SliTaz 4.0 and newer:

   .. code-block:: console

      # cd /mnt/target
      # unlzma < rootfs4.gz | cpio -id 
      # unlzma < rootfs3.gz | cpio -id 
      # unlzma < rootfs2.gz | cpio -id 
      # unlzma < rootfs1.gz | cpio -id 
      # rm rootfs* init

That's it; SliTaz is installed!
Before rebooting to start your new SliTaz GNU/Linux installation, please check that you have a boot-loader (:program:`GRUB` or :program:`Lilo`) installed and add the necessary lines (see below) to boot SliTaz.


.. _grub boot-loader:

:program:`GRUB` Boot-Loader
---------------------------

:program:`GRUB` is an universal boot-loader capable of booting almost any operating system, including Linux, \*BSD and Windows.
:program:`GRUB` uses a single configuration file named :file:`menu.lst`.

If you used the SliTaz Installer and installed :program:`GRUB`, you don't need to manually install :program:`GRUB` — just reboot.

Otherwise, to install :program:`GRUB` onto the MBR (Master Boot Record) using a root directory of :file:`/mnt/target` (the target mounted partition) and the disk named :file:`hda`, use the following command and note the lack of a partition number:

.. code-block:: console

   # grub-install --root-directory=/mnt/target /dev/hda

You can now create a :program:`GRUB` configuration file and add the lines which will boot SliTaz.
The :file:`menu.lst` file can be edited with your favourite text editor such as :program:`Nano` or :program:`Leafpad`:

.. code-block:: console

   # leafpad /mnt/target/boot/grub/menu.lst


.. rubric:: Example :file:`/boot/grub/menu.lst`

::

  title  SliTaz GNU/Linux 3.0 (Kernel 2.6.34-slitaz)
         root(hd0,0)
         kernel /boot/vmlinuz-2.6.34-slitaz root=/dev/hda1 vga=normal

Verify again that everything is in place before rebooting with the :command:`reboot` command:

.. code-block:: console

   # reboot

You should see :program:`GRUB` with a SliTaz item in its menu.


Dual-Booting with Windows
-------------------------

A common query asked on the `Community Forum <http://forum.slitaz.org/>`_ is how to dual-boot SliTaz and Windows.
This is a straight-forward task that just needs the following lines appended to the :file:`/boot/grub/menu.lst` file::

  title   Microsoft Windows
          rootnoverify (hd0,0)
          chainloader +1

In this example, the Windows installation resides on the first hard disk (:file:`hd0`) and the first partition (the second ``0``) within it.
This may need modification to reflect individual cases.
If it were the other way around and SliTaz proceeded Windows for instance, the line would read::

  rootnoverify (hd0,1)

Most operating systems will either contain a boot-loader of their own (in the case of Windows and \*BSD) or can be booted directly with :program:`GRUB`.


Installing SliTaz on an USB Device
----------------------------------

If you want to install SliTaz on an USB device, you must give a little ``rootdelay`` to allow time for the Linux kernel to detect it.

To include this option, edit your :file:`menu.lst` to include the argument::

  title  SliTaz GNU/Linux 3.0 (Kernel 2.6.34-slitaz)
         root(hd0,0)
         kernel /boot/vmlinuz-2.6.34-slitaz root=/dev/sda1 vga=normal rootdelay=10


Sharing a partition
-------------------

It is not always necessary to format a partition.
You can share a partition with another OS and install SliTaz into a :ref:`loop file <loop install>` or in a :ref:`subdirectory <subdirectory install in a posix filesystem>`.
