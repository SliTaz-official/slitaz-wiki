.. http://doc.slitaz.org/en:handbook:systemtools
.. en/handbook/systemtools.txt · Last modified: 2015/12/28 17:58 by genesis

.. _handbook systemtools:

System Tools
============


Clex — Command line File Manager
--------------------------------

To navigate through your folders and directories you can use :command:`cd` on the command line or install the :program:`Clex` File Manager:

.. code-block:: console

   # tazpkg get-install clex

Using :program:`ncurses`, :program:`clex` is fast and easy to use and can be configured through the files :file:`~/.clexrc` and :file:`~/.clexbm` (bookmarks) or via the panel (:kbd:`Ctrl`\ +\ :kbd:`G`).
To start :program:`clex` from a terminal or console:

.. code-block:: console

   $ clex


PCManFM — File Manager
----------------------

:program:`PCManFM` is a file manager providing many useful functions for daily tasks such as managing devices, opening terminals in the current directory, tabbed browsing, drag and drop, creating directories or managing file permissions.
It contains bookmarks to allow you to browse faster, search functions and much more.
PCManFM can be launched with some command line options — you can set the wallpaper to display or open folders in new tabs, etc.
For a full list of options:

.. code-block:: console

   $ pcmanfm --help-all

PCManFM supports hotkeys (firefox) and the context menu (right click on file/directory) makes it easy to unpack .taz .gz archives, compress and create archives.


Htop — View system processes
----------------------------

:program:`Htop` is a system process viewer that displays CPU load, memory state (RAM) and swap used.
It can also display the number of tasks, uptime and PIDs of active processes.
:program:`Htop` can be used with the keyboard in console mode, the mouse with a X terminal (:program:`xterm`) and provides configuration options (:kbd:`F2`).
:program:`Htop` can also kill processes — you can select items with the up/down arrows or a mouse click.
Note :program:`htop` also functions via SSH and can be used to monitor a remote server:

.. code-block:: console

   # tazpkg get-install htop


LXTask — Graphical system process viewer
----------------------------------------

:program:`LXTask` is the default process viewer in SliTaz.
It offers the same functionality as :program:`Htop`, except for the ability to control it remotely.
You can start it from the :menuselection:`System Tools Menu --> Task Manager`.


Mountbox — Mount devices
------------------------

:program:`Mountbox` is a small GTK+ application to quickly mount media such as a USB drive, hard drive or CD-ROM.
:program:`Mountbox` can be launched from a terminal or via the :menuselection:`Tools` menu (:menuselection:`System Tools`).
Simply specify the peripheral (:guilabel:`Device`) and the mount point, ie. the directory where you want to access the media in question.
Typically a CD is mounted on :file:`/media/cdrom`, a USB key on :file:`/media/flash` and disk drives on the local machine on :file:`/mnt`.
Note the Handbook also contains more information.


Gparted — Partition a hard drive
--------------------------------

:program:`Gparted` is a graphical application making it possible to manage the partitions of a local hard drive or USB media.
It allows you to reformat, resize or check a partition on a hard drive and is the tool of choice if you need to prepare a partition to install SliTaz.
:program:`Gparted` supports proper GNU/Linux filesystems (ext2, ext3 and ext4) via :program:`mkfs`, and :program:`Parted` automatically handles dependencies.


.. rubric:: Support FAT and NTFS filesystems

To have the support of FAT16 or Windows FAT32 filesystems, you must install the package :program:`dosfstools`.
To enable read/write support for NTFS partitions: :program:`fuse`, :program:`ntfs-3g` and :program:`ntfsprogs`.
