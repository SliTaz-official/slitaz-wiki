.. http://doc.slitaz.org/en:guides:virtualbox
.. en/guides/virtualbox.txt · Last modified: 2015/04/01 04:05 by mojo

.. _virtualbox:

Virtualbox
==========


SliTaz GNU/Linux with VirtualBox
--------------------------------

You can now play with SliTaz GNU/Linux on Windows with VirtualBox.
Required:

* `SliTaz GNU/Linux Image <http://virtualbox.wordpress.com/2008/06/27/slitaz-gnulinux-is-here/>`_, the virtual machine.
* `Oracle VirtualBox <http://www.virtualbox.org/wiki/Downloads>`_ software, binaries for Windows.
* `7-Zip <http://www.7-zip.org/fr/download.html>`_, needed to decompress the file.


Step 1 — Downloads and installation.
------------------------------------

* Download and install 7-Zip — necessary to decompress the files.
* Download and install the Oracle VitualBox software.
* Download and decompress the virtual machine somewhere on your local hard drive.


Step 2 — Play the virtual machine.
----------------------------------

Before running SliTaz GNU/Linux virtual machine you need to configure a new virtual machine in the VirtualBox software.

#. Run virtualbox :menuselection:`Start --> Programms --> Oracle xVM VirtualBox --> VirtualBox`.
#. Add a new Machine: Click on :guilabel:`New` toolbar button.
#. Click :guilabel:`Next` in the “Create New Virtual Machine” dialog box.
#. Give a name to your new virtual machine (i.e: ``SliTaz GNU/Linux``), and select ``Linux 2.6`` in :guilabel:`OS Type` list and click :guilabel:`Next`.
#. Select the amount of memory for the virtual machine.
   256MB default should be sufficient.
#. In the “:guilabel:`Virtual Hard Disk`” dialog box click on the :guilabel:`Existing` button to use your SliTaz virtual disk.
#. The Virtual Disk Manager start.
   Click the :guilabel:`Add` toolbar button, locate and select the VDI file (i.e: :file:`slitaz-1.9-x86.vdi`).
#. Click on :guilabel:`Next` and :guilabel:`Finish`.

If you wish to set up a network using the Host-only adapter, follow these instructions:

#. Click on the :guilabel:`Settings` toolbar button
#. Click on :guilabel:`Network` in the side menu
#. Select the :guilabel:`Network Adapter` (usually :guilabel:`Adapter 1`) and set it for Host-only networking
#. Click on the :guilabel:`Advanced` arrow
#. Change the :guilabel:`Adapter Type` to :guilabel:`PCnet-FAST III`
#. Click on :guilabel:`OK`

The default Intel adapter seems to work fine for NAT, but not for Host-only networking.

You can now play with your new SliTaz GNU/Linux virtual machine.

.. note::
   VirtualBox can use VMware virtual disks as well.

.. note::
   If you experience resolution problems (no larger than 800×600) check the following `forum post <http://forum.slitaz.org/topic/screen-resolution/page/2>`_


Virtualbox-OSE
--------------

You can install the open source edition of virtualbox (free software) in SliTaz with the package :program:`virtualbox-ose`:

.. code-block:: console

   # tazpkg get-install virtualbox-ose

This is a restricted version without USB support.


SliTaz get-virtualbox
---------------------

You can install virtualbox (full, but non-free version) in SliTaz with the :program:`get-virtualbox` package:

.. code-block:: console

   # tazpkg get-install get-virtualbox ; get-virtualbox

Other virtualization (free software) is also available such as lguest or qemu.


Installing Virtualbox Guest Additions
-------------------------------------

.. tip::
   This procedure is intended for Slitaz 5.0.

   This procedure was tested with:

   * VirtualBox 4.3.20 installed on Host, including the Extensions Pack;
   * slitaz-5.0-rc2.iso dated 20140519;
   * slitaz-rolling.iso 32-bit dated 20150201.

   See guest additions for SliTaz 4.0 `in the forum <http://forum.slitaz.org/topic/installing-virtualbox-guest-additions-in-slitaz-40-target#post-10791>`_ also.

Virtualbox Guest Additions needs to be manually installed by Slitaz.

Steps:

#. In the SliTaz virtual machine make sure you have a working internet connection.
   Sometimes you need to stop/restart eth0 using TazPanel (Slitaz Panel) the first time to get a connection.
#. Open Package Manager and recharge the package list.
#. Install the following packages:

   * linux-module-headers (3.2.53)
   * mesa-dri
   * bzip2

#. Mount the VBoxAdditions ISO using the VirtualBox menu option :menuselection:`Devices --> Insert Guest Additions CD image…`
#. In a terminal, change directory to the mount point for the VBoxAdditions iso, e.g.

   .. code-block:: console

      $ cd /media/cdrom/

#. Run as root the following command:

   .. code-block:: console

      # sh ./VBoxLinuxAdditions.run

   Ignore text about scripts added to :file:`/etc/init.d` (installer does not recognize your Linux Distribution etc).
   Ignore text about kernel headers for current running version not found.
#. Open a terminal as root and issue the following commands:

   .. code-block:: console

      # adduser -h /var/run/vboxadd -G daemon -S -s /bin/false vboxadd >/dev/null 2>&1
      # addgroup -S vboxsf >/dev/null 2>&1

#. Open as root :file:`/etc/rcS.conf` and add "vboxguest vboxsf vboxvideo" to the ``LOAD_MODULES`` line, e.g.

   .. code-block:: shell

     LOAD_MODULES=" vboxguest vboxsf vboxvideo"

#. Open as root :file:`/etc/slim.conf` and change the ``login_cmd`` line to start VBoxClient-all before it executes :file:`~./xinitrc`, e.g.

   .. code-block:: shell

     login_cmd VBoxClient-all & exec /bin/sh -l ~/.xinitrc %session</file>

   .. tip::
      Instead of editing :file:`/etc/slim.conf`, you could add "VBoxClient-all &" without the quotes to a new line above the ``CASE 1$`` line in every users :file:`~/.xinitrc` file.

#. Reboot.

----

To check that the vbox modules are loaded, open a terminal and type:

.. code-block:: console

   $ lsmod | grep vbox

… that should show::

  vboxguest
  vboxsf
  vboxvideo

(as well as ``drm`` using ``vboxvideo``)

Also, you can install the :program:`mesa-demos` package to get the :command:`glxgears` and :command:`glxinfo` utilities.
After you install :program:`mesa-demos`, open a terminal and type:

.. code-block:: console

   $ glxinfo | grep render

… that should show::

  direct rendering: Yes
  OpenGL renderer string: Chromium

:command:`glxinfo` and :command:`glxgears` will show a false error::

  libGL error: failed to load driver: vboxvideo

… which according to the VirtualBox bugtracker is due to the way vboxvideo hooks itself into the Mesa library instead of being loaded in the normal way by Mesa.
So ignore it…
If you have ``OpenGL render = Chromium`` and ``Direct rendering = Yes`` then accelerated 3D is supported.

----

Clipboard sharing Host<->Guest works if you enable it in the VirtualBox menus or in your virtual machine settings.

Shared folders work (The author manually mounts the shared folders as and when he needs them).
For instance, if you wanted to mount the (hypothetical) share called :file:`pubdoc` and access it as a desktop folder called :file:`Docs` (make sure the folder exists first) then you issue the following command in a terminal as root:

.. code-block:: console

   # mount -t vboxsf pubdoc /home/tux/Desktop/Docs

References: `Slitaz Forum topic <http://forum.slitaz.org/topic/installing-virtualbox-guest-additions-in-slitaz-50#post-34521>`_
