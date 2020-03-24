.. http://doc.slitaz.org/relnotes
.. relnotes.txt · Last modified: 2014/05/14 20:54 by linea

.. _relnotes:

5.0 SliTaz Release Notes (draft)
================================

:author: pankso, domcox, bellard, trixar_za, linea, godane, paranor, mojo

.. note::
   Can the developers and coders also add to these notes as they are probably the only ones fully aware of all the changes since 4.0.
   20140511 — Feel free to edit, modify and add new sections.
   Thanks

Welcome to SliTaz 5.0!
Modern, secure, fast and flexible, the new SliTaz stable version is now out for Desktops and servers in production use.

* Over ??? commits in the repos
* 4400 packages in the packages database
* Slitaz ported to the ARM architecture
* Official Raspberry Pi flavors
* New Slitaz-configs dialog tool 
* Frugal install tools added
* Many tools updated and improved (tazbox, tazweb, ???)


Overview
--------

SliTaz GNU/Linux is a free, open source community project.
Version 5.0 was released on ??? after two years of hard work.
SliTaz comprises of 4400 software packages easily installed via the package manager "Tazpkg".
The LiveCD can be fully configured to taste, to easily create a custom distribution specifically for tasks such as multimedia, graphics or development.

SliTaz can also be installed to your hard drive, or used with USB media — with "TazUSB" you are only a few simple commands away from a fully formatted and configured USB device, ready to boot and the Tazlito tool allows you to fully remaster your own your Live CD.
SliTaz provides also a Cookutils suite of tools to build your own packages.

Technical support is provided to users via the mailing list and the official forum.
The "Slitaz Handbook" is an instructive manual on how to use and finely configure the system.
SliTaz can be updated easily via the graphic installer or by using the simple fast text installer.


Hardware
--------

As well as most i486 or x86 architectures, SliTaz also supports ARM v6 and work is on the stove for armv6hf (hard float) and armv7.
A minimum of ??? is required to use the LiveCD and our Russians dolls system will automatically boot one of 4 systems, (currently core, gtk only, X windows and base) depending on the amount of memory available.
The kernel supports a wide variety of wireless, network, ???, sound drivers and firmware.


Kernel & Toolchain
------------------

The kernel currently stands at 3.2.53 and uses Binutils 2.23.1, GCC 4.6.3, Glibc 2.14.1.
Cookutils now boasts two new tools: 'cross' which gives you the ability to cross compile i?86, arm and x86_64 packages and 'cooklinux' which generates custom kernels from scratch.


LiveCD core flavors
-------------------

SliTaz GNU/Linux is distributed as a bootable LiveCD allowing you to graphically install to the hard drive and retain the use of your previous system including all settings, applications, documents, etc.
The core LiveCD contains 4 different operating systems and will automatically select the one that your hardware will support.
You can also select images from the menu and hit :kbd:`Tab` to edit any options.
The menu also provides a command line, help options, languages and the ability to webboot.
The ISO image uses a 'hybrid' system which can also be directly copied (:command:`dd`) onto a USB stick without formatting.
SliTaz also provides the tools to remaster your LiveCD both graphically and from a command line.
The SliTaz community provides many custom and preset flavors which can be downloaded from our mirrors.


Installation
------------

Installation can be fully automated with the Tazinst tool found in the SliTaz panel, this allows you to install SliTaz from a Live-CD, a LiveUSB key, a downloaded ISO image, or directly from the web.
Frugal tools have been added to 5.0 to assist in a minimal HDD install and Tazinst also supports command line installation.


Raspberry Pi
------------

The SliTaz port to the ARM processing architecture consists of a text mode base system and a lightweight desktop powered by JWM and the Fox toolkit.
Both can be installed to a SD card by using the instructions found in the download directory.
SliTaz RPi comes with its own tools including 'spk' — a tiny package manager, 'slitaz-config' — a system configuration menu and the 'tazberry' tool for RPi specific configurations.
Boot scripts have been optimized for SliTaz and you will be prompted for a root password, keymap and default user on first power on.
SliTaz RPi has nearly 900 packages in its database and can also keep up to date with system and kernel updates.
The SliTaz `Pi Book <http://arm.slitaz.org/codex/pibook.html>`_ contains detailed instructions to help you install and get started on ARM and the Raspberry Pi.
Official SliTaz Raspberry Pi `website <http://arm.slitaz.org/rpi/>`_


Packages
--------

Among the 4400 packages available in SliTaz 5.0 you will find anything you need to transform your machine to an
office suite with LibreOffice, graphics studio with The Gimp or Inkscape, a video editor with Kino or a complete graphical desktop (razor-qt).
You can experience the world wide web with instant messaging, VOIP, email and of course through a web browser.
Packages can be found through the search function of Tazpkg, Tazpanel or via the website: http://pkgs.slitaz.org/

Iptables functions as the firewall and Rsync can be installed for incremental backup.
SliTaz can of course also provide a complete development environment with the GCC 4.6.3 compiler, Geany IDE, Mercurial Repostitories and all development libraries.

SliTaz is designed to function as a powerful web server, using the stable LightTPD/PHP package (not installed by default), supporting CGI, Perl and Python.
Apache, Squid and Privoxy are also available.
The ability to browse the web securely using the Tor network is also supported.
Packages are also checked by Cookutils to ensure the FHS is followed and packages are now built automatically by the SliTaz Build Bot: http://cook.slitaz.org/


Administration
--------------

The tool to configure the system on SliTaz 5.0 is TazPanel.
It is a CGI/web interface with a themable user interface — which was updated for 5.0 — from where you can control your entire system such as networking, package management, adding or removing users, managing hardware, creating Live systems and much more.
Each page provides a small description to help you manage your SliTaz system.
To access the panel you can use the menu entry in “System Tools” or this url: http://tazpanel:82


Core Desktop
------------

By default, the SliTaz LiveCD uses the very light and stable Openbox window manager.
Openbox is widely themeable and configurable using the ObConf utility.
The integration of the taskbar “LXpanel” makes it possible to dynamically provide a menu based on the Freedesktop standards.
The principle is to have a small menu accessible via a screen click with the favorites, windows effects, LiveCD and LiveUSB tools, Openbox configuration and system actions made available.
Applications can also be accessed through the menu supplied by LXpanel.
The managment of the Desktop and icons are entrusted to the file manager PCManFM.

Through the support of a LiveCD flavor or an installed system you can install the Enlightenment (e17) desktop environment or the window managers XFCE, Pekwm, JWM, DWM and Razor-qt.
The different sessions can be selected via the :kbd:`F1` key when using the “Slim” login window.
To change the default session you can use tazx; or manually edit the :file:`~/.Xinitrc` file.


From 4.0 to 5.0
---------------

A SliTaz GNU/Linux installer offers an update function allowing you to upgrade from a 4.0 to 5.0 version.
To upgrade the system you first need to boot the Stable LiveCD, launch the installer from the command line or Tazpanel, select upgrade and then specify the partition or configuration file containing the system that you want to update.
The installer will then clean out the system and reinstall all the packages not present on the CD from the mirror.
When this has finished you can reboot with your new version of SliTaz.

To upgrade a 4.0 to 5.0 it is also possible to use the package manager Tazpkg via the set-release function, but beware this is not yet proven and may require some manual intervention.


People of the Project
---------------------

SliTaz is proud to be an international community project.
The people of the project are the ones who develop the distribution, correct the website, develop the HG repositories and write the official documentation.
Passing through Switzerland, France, Brazil, Quebec, China, India, Russia, Ukraine, England, and the U.S.
