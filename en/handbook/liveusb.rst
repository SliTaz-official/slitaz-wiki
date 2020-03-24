.. http://doc.slitaz.org/en:handbook:liveusb
.. en/handbook/liveusb.txt · Last modified: 2014/11/16 10:35 by brianperry

.. _handbook liveusb:

LiveUSB media
=============

:author: jozee, linea, totoetsasoeur, ceel, mojo, brianperry


Tazusb — Live USB Tool
----------------------

:program:`Tazusb` is a tool that can create bootable USB media with a few simple commands.
It can also compress and backup the entire current filesystem to the media, thus preserving all modifications for future use.
Type :command:`tazusb usage` for a full list of commands or read the `TazUSB manual <http://hg.slitaz.org/tazusb/raw-file/tip/doc/tazusb.en.html>`_.


.. rubric:: Generate your own LiveUSB media

To generate your own LiveUSB media, first you need to login as *root* with :command:`su` and locate your USB storage device using the command:

.. code-block:: console

   # fdisk -l /dev/sdxx

Then format and label your device (Warning: this will erase all data, make sure you specify the right device):

.. code-block:: console

   # tazusb format /dev/sdxx

And finally generate your LiveUSB media device with either:

.. code-block:: console

   # tazusb gen-liveusb /dev/sdxx

(if you're working with a LiveCD as the install source)

or

.. code-block:: console

   # tazusb gen-iso2usb slitaz.iso /dev/sdxx

(if you're working with an ISO file as the install source)

Note that the :file:`/dev/sd{xx}` part of the command above specifies the location to where you need to write the ISO's files to; it is not the source media where the ISO is on!
With the ISO file as install source option, you should hence specify the exact location where the slitaz ISO source file is located.
If it is located at :file:`/home/tux` (while running SliTaz from a non-live version), write

.. code-block:: console

   # tazusb gen-iso2usb /home/tux/slitaz.iso /dev/sdxx

If the source file is found on an external hard disk, mention the folder in which the media's files are outputted to — check this with :program:`PCManFM` (in practice the folder can be something like :file:`/media/{disk}`).
So, the command you'd need to write with the latter would be something like

.. code-block:: console

   # tazusb gen-iso2usb /media/disk/slitaz.iso /dev/sdxx

And that's all you need to do, you can now boot SliTaz from USB media (providing your computer BIOS supports this method).


.. rubric:: Tazusb Manual

The official :program:`Tazusb` manual can be found in the SliTaz `Web site documentation <http://hg.slitaz.org/tazusb/raw-file/tip/doc/tazusb.en.html>`_; this contains a lot more useful information.
The manual is also available through the documentation menu on the LiveCD.


Install SliTaz on USB from Windows
----------------------------------

SliTaz can be installed on USB media from windows.
The following description has been tested on Windows XP and Vista.
Before starting you need to download:

#. the latest SliTaz iso from `Get SliTaz <http://www.slitaz.org/en/get/#stable>`_.
#. the syslinux tool for the SliTaz version you install:

   * `syslinux-4.05.zip <https://www.kernel.org/pub/linux/utils/boot/syslinux/4.xx/syslinux-4.05.zip>`_ for SliTaz 4.0
   * `syslinux-3.82.zip <https://www.kernel.org/pub/linux/utils/boot/syslinux/3.xx/syslinux-3.82.zip>`_ for SliTaz 3.0
   * `syslinux-3.73.zip <https://www.kernel.org/pub/linux/utils/boot/syslinux/3.xx/syslinux-3.73.zip>`_ for SliTaz 2.0
   * `syslinux-3.61.zip <https://www.kernel.org/pub/linux/utils/boot/syslinux/3.xx/syslinux-3.61.zip>`_ for SliTaz 1.0

1. Extract the SliTaz ISO file to the USB stick.
   For this, use a tool like `7-Zip <http://www.7-zip.org/>`_ or Winimage (shareware).
   Once extracted, you should get the following::

     \boot
     \boot\isolinux
     \boot\grub

   .. note::
      Depending on the version of SliTaz you install, you can find on root other directories and files but they are not used by the LiveUSB.

2. On your USB stick, rename the :file:`\\boot\\isolinux` directory to :file:`\\boot\\syslinux`.

3. In the new directory :file:`\\boot\\syslinux`, rename :file:`isolinux.cfg` file to :file:`syslinux.cfg` and :file:`isolinux.msg` file to :file:`syslinux.msg`.
   This is not mandatory but makes more sense.

4. Now extract the syslinux archive previously downloaded and copy the :file:`syslinux.exe` file from its :file:`win32` directory into the :file:`\\boot\\syslinux` directory of the USB stick.

5. Modify the :file:`\\boot\\syslinux\\syslinux.cfg`: replace all :file:`isolinux.*` appellations with :file:`syslinux.*`.


  * example for SliTaz 4.0::

      ...
      kernel /boot/isolinux/ifmem.c32
      ...

    to ::

      ...
      kernel /boot/syslinux/ifmem.c32
      ...


  * example for SliTaz 3.0 and previous versions::

      display syslinux.msg
      label slitaz
      	kernel /boot/bzImage
      	append initrd=/boot/rootfs.gz rw root=/dev/null lang=C kmap=uk vga=normal autologin

      include common.cfg

  .. note::
     In the example above, compared to the original file, the line ``display isolinux.msg`` has been replaced by ``display syslinux.cfg`` and the parameters ``lang=C kmap=uk`` have been added to the line ``append`` so that, lang and keyboard are automatically selected at boot.

  In file :file:`\\boot\\syslinux\\common.cfg`, modify ::

    ...
    F3 isolinux.msg
    ...

  to ::

    ...
    F3 syslinux.msg
    ...

6. Now, from the terminal in Windows, run :command:`syslinux.exe` to install the *bootloader*.
   For example, here the USB stick is shown as letter K, type:

  .. code-block:: doscon

     k:
     cd \boot\syslinux
     syslinux -ma -d \boot\syslinux k:

7. You can now restart your computer.
   Change option in BIOS to boot first from USB.


See also
--------

:ref:`liveusb`
