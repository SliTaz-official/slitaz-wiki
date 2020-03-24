.. http://doc.slitaz.org/en:handbook:xwindow
.. en/handbook/xwindow.txt · Last modified: 2012/04/09 22:13 by linea

.. _handbook xwindow:

X Window System
===============

:author: jozee, linea, jpeg


X11 — X Window System
---------------------

The X Window System or X11 provides a window manager running on top of a X server.

SliTaz 1.0 and 2.0 by default use the lightweight X server called :program:`Xvesa` from the Xorg project (www.x.org).

Slitaz 3.0 by default uses the :program:`Xorg` server, there is however a :program:`Xvesa` flavor.

The X server can be started with the SLiM login manager or directly from a Linux console with the command :command:`startx`, but for this you must first disable the Login Manager.
To reconfigure your X session you can use :program:`tazx` as root or as the current user if you start X from the command line.


:program:`Tazx` — SliTaz X configuration tool
---------------------------------------------

:program:`Tazx` is the configuration tool to manage your X window sessions on a SliTaz box.
Simply select a resolution and press :guilabel:`OK`.
You can also select a (Xorg) session by selecting a video driver best suited to your hardware.
After you first run :command:`startx`, the configuration is saved in the executable files :file:`~/.xsession` and :file:`~/.xinitrc`.
These files are then used to start a X session with either :command:`startx` or via the SLiM login manager and can be easily altered with a text editor.
:program:`Tazx` can also be used to change your default window manager.
Example:

.. code-block:: console

   # tazx jwm


SLiM — Simple Login Manager
---------------------------

:program:`SLiM` is a lightweight session manager that is very easy to configure and is customizable using system themes.
The configuration file is found in :file:`/etc/slim.conf`.
It defines window managers available via the :kbd:`F1` key, the default user or theme, and the X window system parameters.
SLiM offers special user commands like ``console`` to help manage the session.

In LiveCD mode you can disable SLiM with the boot option ``screen=text``.
On an installed system you can remove the package or delete ``slim`` from the ``RUN_DAEMONS`` variable in :file:`/etc/rcS.conf`.

More details and themes can be found on the website


.. rubric:: Default user

SLiM offers a way to pre-load a user login name, by default ``tux`` is configured for convenience.
You can change this by editing the SLiM configuration file :file:`/etc/slim.conf` and modifying the line ``default_user`` or just leave the line blank to avoid pre-loading a user name.
Example::

  default_user        tux


Xorg
----

:program:`Xorg` is the default server on SliTaz and designed to work out of the box on most systems.
It should detect and configure most devices such as keyboards, mice, displays, etc.
Once installed, running :program:`Tazx` allows you to reconfigure/reinstall the :program:`xorg-server` package and select the correct driver for your card.
Example:

First stop the :program:`Xorg` server using :kbd:`Alt`\ +\ :kbd:`Ctrl`\ +\ :kbd:`Backspace`, you should now be in console mode.
Then run :command:`tazx` as root:

.. code-block:: console

   # tazx

Then select Xorg and select your video driver, this reconfigures :program:`Xorg`.
Then restart the :program:`SLiM` login manager:

.. code-block:: console

   # /etc/init.d/slim start

You can also do this by searching for and installing a video driver and reconfiguring :program:`Xorg` manually (after stopping the server):

.. code-block:: console

   # tazpkg search xorg-xf86-video
   # tazpkg get-install xorg-xf86-video-nv
   # Xorg -configure 

Then copy the newly generated file to :file:`/etc/X11`:

.. code-block:: console

   # cp /root/xorg.conf.new /etc/X11/xorg.conf

And restart the login manager:

.. code-block:: console

   # /etc/init.d/slim start


.. rubric:: :file:`xorg.conf.d` — Configuration files

:program:`Xorg` uses the configuration files found in the :file:`xorg.conf.d` directory which are automatically setup when you first boot and can be easily edited with your favorite text editor.
The files are configured separately into sections such as modules to be loaded, default screen, mouse, keyboard, etc.
This document provides a few examples:

:file:`10-ServerLayout.conf`:

.. code-block:: xorg.conf

   Section "ServerLayout"
   	Identifier     "X.org Configured"
   	Screen      0  "Screen0" 0 0
   EndSection

:file:`30-Module.conf`:

.. code-block:: xorg.conf

   Section "Module"
   	Load  "dbe"
   	Load  "dri2"
   	Load  "extmod"
   	Load  "dri"
   	Load  "record"
   	Load  "glx"
   EndSection

Note that a :file:`xorg.conf` file can also be found in :file:`/etc/X11` as another way to configure :program:`Xorg`.
This file is read before all files in :file:`/etc/X11/xorg.conf.d` and will NOT be erased by any updates.


Use :program:`Xvesa` as X terminal (Deprecated)
-----------------------------------------------

You can use :program:`Xvesa` as X terminal, if you have a machine on the network that accepts Xdmcp connections.
To enable this, you can start the server with the option ``-query`` followed by the machine name or IP address.
Example of machine 192.168.0.2 on a local network:

.. code-block:: console

   $ Xvesa -ac -shadow -screen 1024x768x24 -query 192.168.0.2

The use of a graphical remote server can be of great use, although response times of applications depend greatly on Internet speed and the remote machine's power.
This technique works very well within a local area network (LAN) and allows you to control applications installed on the remote machine directly from the screen of the local machine from which you work.
Note that the distant remote machine may have multiple accounts in use simultaneously and/or direct access.


Fonts
-----

The management of Fonts (fonts) is powered by the package :program:`fontconfig`.
This package provides tools to add, list and manipulate fonts.
The fonts can be installed in user space or at the system level, this means that each user can use his/her own fonts or the system administrator (root) can install fonts available to all users of the system.
If you use USB media associated with the SliTaz LiveCD, you can easily install fonts and retain them for the next time you use the CD-ROM.


.. rubric:: Installing fonts

At the system level fonts are installed in the directory :file:`/usr/share/fonts`, core SliTaz provides TTF Vera fonts, they take up little space and are rendered correctly.
At the root of user space :file:`~/`, fonts are found in the hidden directory :file:`.fonts`.
To create a home directory to accommodate new fonts, you can use the graphical window manager :program:`emelFM2`, :program:`Clex` or the command line:

.. code-block:: console

   $ mkdir ~/.fonts

Once you have installed the fonts you need to run the :command:`fc-cache` tool to generate configuration files, this ensures that your fonts are available for use in applications:

.. code-block:: console

   $ fc-cache
