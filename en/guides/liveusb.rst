.. http://doc.slitaz.org/en:guides:liveusb
.. en/guides/liveusb.txt · Last modified: 2017/02/01 20:08 by linea

.. _liveusb:

Live USB
========

:author: jozee, linea, pankso, totoetsasoeur, mojo, fantomas, brianperry, bellard

This page describes how to make a SliTaz live usb stick.


From Windows
------------

A SliTaz Live USB installation can be performed using:

* `LinuxLive USB Creator`_
* `Universal USB Installer`_
* :program:`UnetBootin`
* :program:`TazUSB`

.. warning::
   SliTaz with xorg-server and persistent :file:`/home` requires a Linux filesystem (ext2,ext3) for regular users to login to an X-session.

   Fat32 or NTFS filesystems cause SLiM login manager error: ``Failed To Execute Login Command``

.. tip::
   Solution:

   SliTaz runs in RAM so after booting it no longer needs the flash drive.
   This enables you to reformat the flash drive to a Linux filesystem and reinstall SliTaz.

#. Install SliTaz with `LinuxLive USB Creator`_ or `Universal USB Installer`_
#. Copy the :file:`slitaz.iso` to the root of the flash drive.
#. Boot into SliTaz with the flash drive,login root, password: root
#. Mount_ the flash drive,copy the :file:`slitaz.iso` to :file:`/root`
#. Use **From SliTaz** instructions above to format the flash drive ext2, reinstall SliTaz using ISO file as install source.

.. warning::
   :program:`Tazusb.exe` and :program:`Unetbootin` do not work with any :file:`cooking.iso`, :file:`rolling.iso` or :file:`slitaz-4.0.iso` (core 4-in-1).

   They will work with `slitaz-4.0-core.iso`_ or another single :file:`rootfs.gz` flavor found `here <flavors_>`_.

.. warning::
   :program:`Tazusb.exe` does not work with any :file:`cooking.iso`, :file:`rolling.iso` or :file:`slitaz-5.0.iso`.

   The :file:`.iso` file must be renamed to an :file:`.exe` file, i.e.:

   #. boot into Windows,
   #. download the latest :file:`slitaz-rolling.iso`,
   #. go to the downloaded file directory,
   #. rename the file :file:`slitaz-rolling.iso` to :file:`slitaz-rolling.exe`,
   #. double-click on the file :file:`slitaz-rolling.exe` to run it,
   #. follow the instructions given by the application (2-3 steps):

      #. Do you want to create a boot key: => answer yes,
      #. Step 1: unplug the USB stick,
      #. Step 2: plug the USB stick in and wait for Windows to mount it,
      #. wait for the confirmation window,

   #. reboot,
   #. press :kbd:`Fn` key to choose alternative boot device, slitaz boot panel must be displayed,
   #. select the language,
   #. :kbd:`RETURN` to boot.


Remarks when using :program:`UNetbootin`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can find `UNetbootin for Windows`_ at this page.
Note that you need to select SliTaz ISO from the :guilabel:`Distribution list` in UNetbootin.
To create a Live USB, select :guilabel:`USB Drive` under installation type.
You can also choose to do a frugal install.
To perform a Frugal Install, select :guilabel:`Hard Disk` under installation type.


Remarks when using :program:`TazUSB`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The binary file :file:`TazUSB.exe` is a Windows executable that installs an ISO file on a USB drive.

* download :program:`TazUSB` executable for Windows: `tazusb.exe`_ (the sources are available here `tazusb.nsi`_)
* plug in your USB drive and format it in **FAT32** (formatting might not be required),
* launch :program:`TazUSB` application by double-clicking on :file:`TazUSB.exe` executable file,
* choose the language,
* select the destination USB drive (**double-check that the letter refers to the correct drive**),
* select the SliTaz **iso**


From Linux
----------

From any Linux distro
^^^^^^^^^^^^^^^^^^^^^

* You can use :program:`unetbootin`; download from `UNetbootin for Linux`_.


From Slitaz linux
^^^^^^^^^^^^^^^^^

Download > Burn > Boot a SliTaz ISO image

Verify the install target, format will delete everything.

.. code-block:: console

   $ su root
   # fdisk -l
   # tazusb format /dev/sdxx

LiveCD as install source

.. code-block:: console

   # tazusb gen-liveusb /dev/sdxx

ISO file as install source

.. code-block:: console

   # tazusb gen-iso2usb slitaz.iso /dev/sdxx

Note that the :file:`/dev/sd{xx}` part of the command above specifies the location to where you need to write the ISO's files to; it is not the source media where the ISO is on!
With the ISO file as install source option, you should hence specify the exact location where the SliTaz ISO source file is located.
If it is located at :file:`/home/tux` (while running SliTaz from a non-live version), write

.. code-block:: console

   # tazusb gen-iso2usb /home/tux/slitaz.iso /dev/sdxx

If the source file is found on an external hard disk, mention the folder in which the media's files are outputted to — check this with :program:`PCManFM` (in practice the folder can be something like :file:`/media/disk`).
So, the command you'd need to write with the latter would be something like

.. code-block:: console

   # tazusb gen-iso2usb /media/disk/slitaz.iso /dev/sdxx

Once done, wait… and then reboot!


SliTaz 4.0 kernel panic (not booting)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In Slitaz 4.0 the rootfs file is divided into 4 parts which is a problem with :program:`tazusb.exe` and :program:`UNetbootin`.
For SliTaz to work with :program:`tazusb.exe` and :program:`Unetbootin` you must download the `core flavor of SliTaz`_ consisting of a single file rootfs.


Sources
-------

* `how to install slitaz 4.0 on usb`_


See also
--------

* :ref:`liveusb`


.. _LinuxLive USB Creator:   http://www.linuxliveusb.com/
.. _Universal USB Installer: http://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/
.. _Mount:                   http://doc.slitaz.org/en:handbook:commands
.. _slitaz-4.0-core.iso:     http://mirror.slitaz.org/iso/4.0/flavors/slitaz-4.0-core.iso
.. _flavors:                 http://mirror.slitaz.org/iso/4.0/flavors/
.. _UNetbootin for Windows:  http://unetbootin.sourceforge.net/unetbootin-windows-latest.exe
.. _UNetbootin for Linux:    http://unetbootin.sourceforge.net/unetbootin-linux-latest
.. _tazusb.exe:              http://mirror.slitaz.org/packages/windows/tazusb.exe
.. _tazusb.nsi:              http://hg.slitaz.org/tazusb/raw-file/tip/win32/tazusb.nsi
.. _core flavor of SliTaz:   http://mirror.slitaz.org/iso/4.0/flavors/slitaz-4.0-core.iso
.. _how to install slitaz 4.0 on usb: http://oldpapyrus.wordpress.com/tag/how-to-install-slitaz-4-0-on-usb/