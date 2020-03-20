.. http://doc.slitaz.org/en:devnotes:xorgserver-user
.. en/devnotes/xorgserver-user.txt · Last modified: 2010/08/23 00:16 by linea

.. _xorgserver user:

xorg-server-1.8
===============


Upgrade Xorg configuration
--------------------------

Upgrade Slitaz:

.. code-block:: console

   # echo 'y' | tazpkg upgrade

Backup :file:`xorg.conf`:

.. code-block:: console

   # mv /etc/X11/xorg.conf /etc/X11/xorg.conf-backup

Re-configure :program:`Xorg` with :command:`tazx`:

.. code-block:: console

   # tazx config-xorg


User tweaks
^^^^^^^^^^^

If you have tweaked the :file:`xorg.conf` file, then you have two possibilities.
In both cases the tweaks will be read before the default configuration and will not be erased by an upgrade:

* Put your modifications (examples) in the file :file:`/etc/X11/xorg.conf`.
  This file is now reserved for users, it's the first read by :program:`Xorg` when X boots.
* You put them in the correct file in the directory :file:`/etc/X11/xorg.conf.d`.
  SliTaz use two files for each Xorg section:

  * :file:`n0-Section.conf` for the default configuration and
  * :file:`(n-1)5-SectionTweaks.conf` for the configurations which overwrite the default one.

Example:

:file:`40-evdev.conf` contains the default configuration for all devices.

:file:`35-synaptics.conf` is installed with the package :program:`xorg-input-xf86-synaptics` and contains the configuration file for touchpad.
It overwrites the default one (:program:`evdev`).

If you wish to modify this section manually, take care to rename the file with a lower number like :file:`31-MyDevices.conf`.
And don't use a file named x5 or x0 to ensure that it will not be overwritten by an upgrade.
