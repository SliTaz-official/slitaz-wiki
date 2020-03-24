.. http://doc.slitaz.org/en:handbook:livecd
.. en/handbook/livecd.txt · Last modified: 2015/12/11 21:15 by linea

.. _handbook livecd:

Using the LiveCD
================

:author: pankso, jozee, linea, seawolf, natty537, seacat, brianperry

SliTaz can be used straight from a CD-ROM or USB memory stick, without being installed to a hard drive.
There are a number of ways to use the Live media, with options to customise how it starts.

SliTaz runs entirely in memory (RAM), independently of the installed host system.
It will not damage your other operating system(s) in any way, so you are perfectly safe to try out SliTaz.


Quick Start
-----------

To start using SliTaz from a CD-ROM, just burn the ISO image onto a blank disc and reboot your computer, leaving the disc in your CD-ROM drive.
SliTaz will load automatically, detecting your hardware configuration to start.


.. rubric:: If SliTaz LiveCD Does Not Start

In most cases, your computer is already configured to boot from the CD-ROM.
If the SliTaz splash screen does not appear, you can change the boot order via the BIOS set-up interface.
This is different with each computer but can often be done by pressing a key such as the :kbd:`F11`, :kbd:`F12` or :kbd:`Esc` button directly after turning on, before your operating system starts.
With some BIOS's you can use the 'Boot Selection Popup' by pressing a :kbd:`F*` key (eg. :kbd:`F8`).
You can then change the boot sequence and settings so that the CD-ROM comes first.
Finally, save your changes before leaving the BIOS configuration interface.

When the SliTaz Live media starts, the splash image will be presented.
This is the :program:`isolinux` boot-loader, which affects options to start SliTaz.
You can just press :kbd:`Enter` to use the default settings, or enter options.

.. tip::
   Selecting :guilabel:`Help & Options` at the boot-splash will display help and information.

When the loading process has finished, you can log-in to the desktop as the *tux* user, without any password.
To use the administrator account, or **root**, you can start a :program:`Terminal` and type the command :command:`su`.
The default password is **root**.


Boot Options
------------

The SliTaz LiveCD accepts various boot options at the prompt.
There are two types of options: options handled by SliTaz software and those generally handled by the Linux kernel.

The options for SliTaz are used by various start-up scripts; the parameters such as the VGA mode are managed directly by the Kernel (kernel boot parameters).
To pass options at start-up, either press :kbd:`Tab` at the language selection screen or just precede your commands with *slitaz* when the splash screen and **boot:** prompt is displayed.
For example::

  slitaz modprobe=nvidia nomodeset

.. tip::
   The Linux kernel keeps options that were passed.
   These can be seen in the text file :file:`/proc/cmdline`.
   You can view this information by running the command:

   .. code-block:: shell

      cat /proc/cmdline


.. rubric:: Parameters of the Linux Kernel

On GNU/Linux systems, parameters specific to the Kernel vary greatly depending on the configuration used during the build.
The SliTaz-built kernel has few core modules, compensated by loading others on-demand.
This means few modifiable parameters are available at start-up.
However, you can disable the emulation of a math coprocessor via:

``no387``
  Disables the emulation of a math co-processor.

``irqpoll``
  Turn on in case of problems with interrupts, shown by problems with the CD-ROM.

``vga=XXX``
  Specifies the kernel graphics mode.
  The SliTaz kernel displays the Tux penguin logo and manages the display of the Linux terminal by providing a basic video output mode, called the VGA/VESA frame-buffer.
  The following table lists the codes used; select a resolution and colour combination:

  ========= ======= ======= ======== ========= =========
  ⇩ Colours 640x480 800x600 1024x768 1280x1024 1600x1200
  ========= ======= ======= ======== ========= =========
     256      769     771     773       775       796
    32768     784     787     790       793       797
    65536     785     788     791       794       798
    16,8M     786     789     792       795       799
  ========= ======= ======= ======== ========= =========

  .. tip::
     ``vga=normal`` lets the system set a working resolution automatically.

.. tip::
   Once the system has started you have access to six pseudo-terminals via the key combinations :kbd:`Ctrl`\ +\ :kbd:`Alt`\ +\ :kbd:`F1` through :kbd:`Ctrl`\ +\ :kbd:`Alt`\ +\ :kbd:`F6`.
   The key combination :kbd:`Ctrl`\ +\ :kbd:`Alt`\ +\ :kbd:`F7` and upwards are reserved for graphical output.


.. _livecd slitaz parameters:

Slitaz Parameters
-----------------

``home=usb``
  Specifies a :file:`/home` directory to use within the Live environment.
  This will include your bookmarks, downloads and desktop customisations.
  To store your data permanently, you need USB media with a partition formatted in ext3; see the :ref:`persistence <persistence splash>` page for more information.
  In most cases ``home=usb`` can be used for :file:`sdb1`, or ``home={devname}`` where the equivalent :file:`/dev` node can be specified.
  Note that you can also specify the device using the partition UUID or label by using ``home=*``.
  Example::

    slitaz home=sdb1

Prepare USB media
  All USB media can formatted in the native Linux ext3 filesystem.
  Ext3 is a journalised, stable filesystem, it allows you to keep permissions on all files and is much more secure than the default Windows FAT32 filesystem.
  To format USB media you have a few options: the command line with :program:`mkfs.ext3`, the :program:`tazusb` utility or graphically with :program:`Gparted`.
  To get a full list of available partitions including the USB drive you can use the command :command:`fdisk -l` and then format.
  Example:

  .. code-block:: console

     # fdisk -l
     # tazusb format /dev/sda1

``lang=XX``, ``kmap=XX``
  Sets the system language & keyboard mapping.
  Each are codes such as ``en``, ``de`` or ``fr_CH``.
  Alternatively, set your country code and press :kbd:`Enter` to sets the locale and refreshes the boot-loader; other options can then be entered as normal.
  To skip the language and keyboard configuration you can simply type options on the command line, for English/UK::

    slitaz lang=en kmap=en

``config=<device>,<path>``
  Executes a script at SliTaz boot time.
  The script can be located on external media or a HD partition, specified with the ``device`` & ``path`` variables.
  For example, the script can mount an ISO image on :file:`/usr` to save memory and boot the LiveCD on computers with only 32 Mb of RAM.
  An example with a script named :file:`slitaz.sh` located on the first disk and partition::

    slitaz config=/dev/hda1,slitaz.sh

``screen=<height>x<width>x<colours>``
  Specifies the desktop screen resolution.
  These follow the standard pattern, e.g. ``1024x768x24`` ::

    slitaz screen=1024x768x24

  .. tip::
     The ``screen=text`` option disables the graphical desktop & :program:`SLiM` login manager.

``sound=no``
  Disables sound completely.
  This loads no sound-related kernel modules. ::

    slitaz sound=no

``sound=noconf``
  Skips automatic configuration of your sound card; you must configure it manually later.

``modprobe=<modules>``
  Loads specific kernel modules.
  Many can be loaded by separating with commas. ::

    slitaz modprobe=module1,module2

``laptop``
  Loads ``ac`` and ``battery`` Kernel modules — useful for laptop computers.

``previous``
  Used by the :program:`TazUSB` utility to roll-back to a previous filesystem. ::

    slitaz previous


The Desktop
-----------

When the system has finished its initialization, the screen is cleared and the login prompt (:program:`SLiM`) is displayed.
You can choose here to login as the regular *tux* account (without a password) or as the administrative *root* account (with the **root** password).

The desktop is powered by :program:`Openbox`.
You can start applications from the menu at the lower-left of the screen.
Applications are classified by category and are available in English.
Menu, theme and wallpaper can all easily be changed to your needs/preferences, and personal settings and data can be stored on various USB media (Flash key, SD card, etc).


Text Mode
---------

.. important::
   If you are new to SliTaz or Linux in general, a graphical desktop is highly recommended.

The above information applies also to the text-mode log-in prompt.
Once logged in, you can use the many text-mode applications available in SliTaz, such as the basic BusyBox operation, the GNU text editor :program:`Nano`, or the :program:`Clex` file manager.
Just type the name of the application you wish to start.
There is a :ref:`Command Line Reference <handbook commands>` page to get you started.

.. tip::
   To launch a graphical desktop session from the text-mode prompt (if you have passed the ``screen=text`` option, or if :program:`SLiM` is not configured to run at startup, for example) just type :command:`startx`.
