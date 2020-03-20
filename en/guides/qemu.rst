.. http://doc.slitaz.org/en:guides:qemu
.. en/guides/qemu.txt · Last modified: 2010/08/16 20:51 by linea

.. _qemu:

QEMU
====


Introduction
------------

QEMU provides a virtual machine that will run within XP.
Refer to the QEMU documentation for a detailed description of the virtual hardware.

It allows to test an operating system environment without actually installing it and without rebooting out of XP; e.g. you can test Slitaz this way, before deciding if you want to install it on your machine.

Compared with Virtual Box (which I am also using regularly), QEMU is initially much faster and easier to set up.
You will also have less problems if you are somewhat memory constrained (which you are, if you have less than 1.0 or 1.5 GB RAM).
Performance wise, both virtualization platforms looked similar to me (although way slower than a native install). 

If you just want to try SliTaz within your XP environment, prefer the QEMU solution described here (unless VirtualBox is already installed and set up on your machine).


Getting started
---------------


Requirements
^^^^^^^^^^^^

This works on XP with SliTaz 3.0 (should work with all versions of SliTaz).


Install QEMU
^^^^^^^^^^^^

Go to Pendrivelinux.com and download the QEMU PC hardware emulator installer (:file:`QPU804.exe`).
This executable is actually a self-extracting archive.
When you run it you just need to tell the installer where to unzip the contents (which may be on your hard disk or on a flash drive).
The result will be a folder named :file:`QPU804`.

The QEMU setup can execute any ISO that you put in that folder.
Copy the SliTaz ISO to that folder.
You now have a virtualized SliTaz installation that can run alongside Windows.


Start QEMU
^^^^^^^^^^

Execute :file:`QPU804.bat` (double click on it from an explorer window) to start your virtual machine (it will advertise it is running Ubuntu, but really it will run the ISO you copied to that folder, :file:`SliTaz.iso` in this case).

You will see your virtual machine booting from the ISO file, as if it would be booting from a SliTaz LiveCD.
Refer to other parts of the SliTaz documentation for a detailed presentation of what you can do from there.


Getting in and out of the QEMU Window
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To move the mouse out of the QEMU screen window press :kbd:`Ctrl`, keep it pressed and press the :kbd:`Alt` key simultaneously.
To go back, just left click in the window.


Shared Folder
^^^^^^^^^^^^^

Open a :program:`PCManFM` window: the SliTaz partition labeled "QEMU VVFAT" refers to the same physical (shared) location as the XP "QPU804\shared folder" folder.


Screen resolution
^^^^^^^^^^^^^^^^^

My screen resolution was 800×600.
You may want to read the QEMU documentation and play around with the available options from both QEMU and SliTaz to get a higher resolution.

To get a full screen SliTaz, try adding the ``-full-screen`` option to the qemu command line of the :file:`QPU804.BAT` file.


I want to keep the changes I made in SliTaz
-------------------------------------------


Private data
^^^^^^^^^^^^

Copy any private data created in SliTaz to the shared directory: it will survive a virtual machine reboot.


Keeping system changes
^^^^^^^^^^^^^^^^^^^^^^

Two ways to do this:

* create a new ISO with your changes, or
* install SliTaz in a qemu virtual drive


.. rubric:: Create an ISO file with your system changes

In the SliTaz menu select :menuselection:`System Tools --> Create a LiveCD` (refer to the LiveCD section of the documentation).

The writeISO panel will create a bootable image of SliTaz with any changes you made, and will store this image file as :file:`/home/slitaz/distro/slitaz.iso`.

In the SliTaz environment, copy-paste that :file:`slitaz.iso` file to the shared folder.
Then from XP you will be able to copy the :file:`slitaz.iso` file from the :file:`QPU804\\shared` folder to the :file:`QPU804` folder, replacing the existing ISO file (delete the old :file:`.iso` file if it has a different name).
Next time you start QEMU, it will boot from your new ISO which brings you back to the same environment.


.. rubric:: Install Slitaz in a qemu virtual drive

Alternatively, refer to the QEMU documentation to start QEMU with a "virtual disk" file, where you will be able to install SliTaz by using the menu item :menuselection:`System Tools --> SliTaz Installer`.

(Not tested with SliTaz but tested with other systems in the past)
