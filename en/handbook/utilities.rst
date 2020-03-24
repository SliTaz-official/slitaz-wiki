.. http://doc.slitaz.org/en:handbook:utilities
.. en/handbook/utilities.txt · Last modified: 2010/09/13 19:23 by linea

.. _handbook utilities:

Utilities
=========

:author: jozee, linea


Galculator
----------

:program:`Galculator` is a simple GTK 2 based calculator that features basic, paper (cmdline) and scientific modes.
You can easily switch number bases between decimal, hexadecimal, binary, etc and it supports ordinary notation/reverse polish notation.
To locate from the menu, select :menuselection:`Utilities --> Scientific Calculator`.


Cdrkit — Burn and manipulate CD/DVD-R or RW
-------------------------------------------

To burn and handle CD/DVD-R or RW, SliTaz provides the :program:`cdrkit` utility and a graphical interface :program:`burnbox` which you will find in the menu.
The tools in :command:`wodim` make it possible to burn CD/DVD and erase CD/DVD-RW.
When used with :command:`genisoimage`, it can also create images in the ISO 9660 format.
Burning on the command line requires us to know the device/writer (``dev``) name and :command:`wodim` provides several possibilities to know which drive to use and specify it when burning to optical media.
If you run :command:`wodim` with the ``-devices`` option, it will automatically search for a good device and display it, the ``-checkdrive`` option allows you to check the recognized device and ``-scanbus`` will display in relation to the system bus.
Examples (as root):

.. code-block:: console

   # wodim -devices

Or:

.. code-block:: console

   # wodim -checkdrive

For the bus:

.. code-block:: console

   # wodim -scanbus


.. rubric:: Create an ISO 9660 image

To burn data on to a CD/DVD, you must first have an ISO image.
To begin we must create a directory to contain all the files to be burned.
You can copy your files on the command line with :command:`cp`, the file manager :program:`Clex` or graphically with :program:`PCManFM2`.
To create a directory named :file:`iso/` in the root of user space and copy all the files contained in :file:`Documents/`:

.. code-block:: console

   $ mkdir ~/iso
   $ cp -a Documents/* ~/iso

Create an ISO image named :file:`image.iso`, using the :command:`genisoimage` tool and specify the root directory containing the files to be included in the ISO:

.. code-block:: console

   $ genisoimage -o image.iso ~/iso

Note that there are many options that you can use to create ISOs, one of the most widely used is the extension ``-R``, signifying *Rock ridge*, this allows the use of names of up to 255 characters (with a few exceptions), it also supports symlinks and file permissions.
To see all the available options, simply type ``-help``.
Example of creating an ISO image using the ``-R`` option:

.. code-block:: console

   $ genisoimage -o image.iso -R ~/iso


.. rubric:: Burn an ISO image

To burn an ISO image in the 9660 format either created by yourself or downloaded from the web, you must use :command:`wodim` specifying the proper device (``dev``) to burn the CD/DVD.
Example using the verbose mode (``-v``), with the device :file:`hdc` and data contained in an ISO image named :file:`image.iso`:

.. code-block:: console

   # wodim -v dev=/dev/hdc image.iso

Another example setting the (``speed``) to burn as 8\ :sup:`×`:

.. code-block:: console

   # wodim -v speed=8 dev=/dev/hdc image.iso


.. rubric:: Erase CD-RW/DVD-RW

You can quickly erase a CD-RW/DVD-RW using :command:`wodim` with the ``blank=fast`` option.
Example using the device :file:`hdc` in verbose mode:

.. code-block:: console

   # wodim -v blank=fast dev=/dev/hdc

Or you can have a full erase using the ``blank=all`` option (this will take a bit longer):

.. code-block:: console

   # wodim -v blank=all dev=/dev/hdc


Nano
----

The default text editor in SliTaz is :program:`nano`.
Once launched you can use :kbd:`Ctrl`\ +\ :kbd:`G` for the help menu.
To start :program:`nano`, you can type :command:`nano` from a console, a :program:`xterm` terminal, or from the :menuselection:`menu --> Editors --> Nano`.

The initialization file :file:`/etc/nanorc` includes the files of colored syntax found in :file:`/usr/share/nano`.
The user configuration file is :file:`~/nanorc`.
To edit a file directly, just launch :command:`nano` proceeded by the name of the file.
Example (:kbd:`Ctrl`\ +\ :kbd:`X` to save & quit):

.. code-block:: console

   $ nano Templates/script-shell.sh


LeafPad
-------

:program:`Leafpad` is an ultra light and quick graphical text editor, handy for taking notes or editing configuration files.
You will find it in the menu or you can run it directly on a file via a terminal:

.. code-block:: console

   $ leafpad Templates/script-shell.sh


ISO Master — Create and edit ISOs
---------------------------------

:program:`ISO Master` is a graphical tool allowing you to edit, manipulate and create ISO images which you can later store and burn.
It's simple and intuitive and lets you create ISO images the size you want.

Website: http://littlesvr.ca/isomaster/


Xpad — Mini note-taking application
-----------------------------------

:program:`Xpad` is a small application that can take quick notes via a 'sticky note' displayed on the desktop.
Each consists of a note pad that you can hide and customise via a right click on the window in question.
Once launched you can close :program:`Xpad` via the dock located on the window manager taskbar.
The notes are stored in your local directory and are available at each session (if you use USB media with the LiveCD or on an installed system).
To install :program:`Xpad`:

.. code-block:: console

   # tazpkg get-install xpad
