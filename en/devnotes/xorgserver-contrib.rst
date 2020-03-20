.. http://doc.slitaz.org/en:devnotes:xorgserver-contrib
.. en/devnotes/xorgserver-contrib.txt · Last modified: 2010/09/10 23:01 by linea

.. _xorgserver contrib:

xorg-server-1.8
===============


Introduction
------------

With xorg-server-1.8, the file :file:`/etc/X11/xorg.conf` is deprecated.
Though we can actually reserve it for user modifications, it's no longer edited automatically.

The configuration files are now in :file:`/etc/X11/xorg.conf.d`.
To ensure that the users configuration will not be erased by a system upgrade, here's a suggestion of numbers and content.
The configuration files are read in alphanumerical order and their names must end with :file:`.conf`:

* 00 — ServerFlag
* 05 — ServerLayout Tweaks
* 10 — ServerLayout
* 15 — Files Tweaks
* 20 — Files
* 25 — Modules Tweaks
* 30 — Modules
* 35 — InputClass/InputDevice Tweaks
* 40 — InputClass/InputDevice
* 45 — Monitor Tweaks
* 50 — Monitor
* 55 — Device Tweaks
* 60 — Device
* 65 — Screen Tweaks
* 70 — Screen
* 75 — Modes Tweaks
* 80 — Modes
* 85 — DRI Tweaks
* 90 — DRI
* 95 — Extension Tweaks
* 100 — Extension

Some of the x0 sections are configured by :command:`tazx xorg-config` using a template :file:`xorg.conf` file (generated with :command:`Xorg -configure`).
You can find more information about this in the function `xorg_conf_d <http://hg.slitaz.org/slitaz-tools/file/ca6804d9b56b/tinyutils/tazx#l27>`_ of :program:`tazx`.

If you want add a configuration file to a package, please take care to use a name different from those specified by :program:`tazx` or this file will be erased when booting in live mode.
You can use the same number (x0) with a different name if this configuration doesn't conflict with those used by default.
Otherwise, use (x-1)5 for that configuration overwriting the default one.

Examples:

* `40-evedev.conf <http://hg.slitaz.org/wok/file/c29991cef110/xorg-xf86-input-evdev/stuff/40-evdev.conf>`_ — Default configuration for all devices
* `35-synaptics.conf <http://hg.slitaz.org/wok/file/c29991cef110/xorg-xf86-input-synaptics/stuff/35-synaptics.conf>`_ — Advanced configuration for touchpad devices, overwriting the default configuration for these devices
* hwsetup, section `nvidia <http://hg.slitaz.org/slitaz-tools/file/ca6804d9b56b/tinyutils/hwsetup#l685>`_ and `ati <http://hg.slitaz.org/slitaz-tools/file/ca6804d9b56b/tinyutils/hwsetup#l753>`_ — Add a file :file:`55-DeviceTweaks.conf` configuring a graphic driver other than :program:`vesa`


Configuration files list
------------------------

This avoid naming two files identically, you can use these in the :file:`/etc/X11/xorg.conf.d/readme` for users with the 4.0 release.

:program:`tazx`:

  * :file:`10-ServerLayout.conf`
  * :file:`20-Files.conf`
  * :file:`30-Module.conf`
  * :file:`40-Keyboard.conf`
  * :file:`50-Monitor.conf`
  * :file:`60-Device.conf`
  * :file:`70-Screen.conf`

:program:`tazhw`:

  * :file:`55-DeviceTweaks.conf`

:program:`xorg-xf86-input-evdev`:

  * :file:`40-evdev.conf`

:program:`xorg-xf86-input-synaptics`:

  * :file:`35-synaptics.conf`


TODO
----

* Improve the auto-configuration tool in :program:`tazx`: some fonts and modules don't need to be loaded by default or don't exist.
  Some of these elements can be configured by their original package.
* Pre-configure some drivers to make them work out-of-the-box.
