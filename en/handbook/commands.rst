.. http://doc.slitaz.org/en:handbook:commands
.. en/handbook/commands.txt · Last modified: 2016/10/01 07:30 by hgt

.. _handbook commands:

Command Line Reference
======================

:author: jozee, linea, seawolf, naitsirhc, genesis, hgt


Introduction to the commands
----------------------------

This document is intended as a quick reference for using commands on SliTaz via a Linux terminal or a graphical terminal (:program:`xterm`).
There are many GNU/Linux commands for file handling, system maintenance or network management.
You can also browse the web, chat on IRC, download files, edit scripts or even play games in text mode.
Note it is necessary to operate as root to assemble the hard drive or CD-ROM.
You can use the command :command:`su` to become system administrator.


Help and list available commands
--------------------------------

Most GNU/Linux system commands have an option for providing information on their use.
For support on the use of a command, it is necessary to type the command followed by the ``--help`` option.
Example using the :command:`cp` command to copy files:

.. code-block:: console

   $ cp --help

To list all the commands available on the system, you can simply press the :kbd:`Tab` button on the left of the keyboard twice.
For commands provided by the Busybox utility you can type:

.. code-block:: console

   $ busybox --help


List the files in a directory
-----------------------------

To list the files and folders contained in a directory, you can use the :command:`ls` command.
For all options remember to use the ``--help`` flag.
To simply list the files in the current directory:

.. code-block:: console

   $ ls

List all the files using the ``-al`` option:

.. code-block:: console

   $ ls -al

List a directory:

.. code-block:: console

   $ ls /home/slitaz


Moving around directories
-------------------------

To browse to the files, you can use the :command:`cd` command:

.. code-block:: console

   $ cd /usr/share/doc

Back to the parent directory:

.. code-block:: console

   $ cd ..

To go into the directory of the user (*root* = :file:`/root`):

.. code-block:: console

   $ cd

Or:

.. code-block:: console

   $ cd ~

Or:

.. code-block:: console

   $ cd $HOME


Copy files
----------

The :command:`cp` command copies files or folders.
The example copies the :file:`info.txt` file in the current directory into the :file:`Documents` directory:

.. code-block:: console

   $ cp info.txt Documents/

Copy a whole directory.
Here the command copies the :file:`Templates` directory into :file:`/home/hacker`:

.. code-block:: console

   $ cp -a Templates /home/hacker


Move (rename) files or directories
----------------------------------

When source and target file are in the same file system and the target file does not exist, the :command:`mv` command simply renames the source file:

.. code-block:: console

   $ mv file1 file2

It can also rename directories (provided the new directory doesn't exist):

.. code-block:: console

   $ mv ~/Documents ~/Docs

Move files (and directories) to a new directory:

.. code-block:: console

   $ mv file1 file2 dir1 dir2 ~/Documents

When renaming is not possible, the :command:`mv` command takes the contents of a file and copies it to a new file, then deletes the original file.


Create a new file
-----------------

The :command:`touch` command can create a new empty file:

.. code-block:: console

   $ touch newfile


Create a new directory
----------------------

This command will create a new directory.
The following command creates a directory called :file:`Projects`.
It will be created in the directory :file:`/home` of the current user or in the directory which one is in.
Note you can display your current working directory with the :command:`pwd` command:

.. code-block:: console

   $ mkdir Projects

Creation of a directory named :file:`script-1.0` in the :file:`Projects` folder:

.. code-block:: console

   $ mkdir Projects/script-1.0

You can also create a directory tree with the ``-p`` parents option:

.. code-block:: console

   $ mkdir -p one/two/three/four


Delete files or directories
---------------------------

The command :command:`rm` lets you delete a file.
Let's remove the file :file:`work.txt` which is in the current directory:

.. code-block:: console

   $ rm work.txt

The command :command:`rm` has several options.
To delete a directory and its contents, we use the ``-rf`` option.
Example:

.. code-block:: console

   $ rm -rf /home/hacker/Templates

.. important::
   Be careful when using this option.
   It will delete everything without asking!

Note you can also use the ``-i`` option to remove files or directories and their contents interactively:

.. code-block:: console

   $ rm -ir /home/hacker/Templates


View files
----------

To read the contents of a file or script, you can use the :command:`less`, :command:`more` or :command:`cat` commands, or the web browser :program:`Retawq`.
Examples with a :file:`README` file, :file:`essential.txt`, and :file:`script.sh`:

.. code-block:: console

   $ less -EM essential.txt

or:

.. code-block:: console

   $ more README

or:

.. code-block:: console

   $ cat /path/to/script.sh

Display a text or html file with the web browser :program:`Retawq`:

.. code-block:: console

   $ retawq /usr/share/doc/index.html


Edit files
----------

Editing text files, scripts, configuration files, etc, can be done easily using the text editor GNU :program:`Nano` in a console or graphical terminal.
Example with a file :file:`bookmarks.html` (:kbd:`Ctrl`\ +\ :kbd:`X` to quit and save):

.. code-block:: console

   $ nano Public/bookmarks.html


:command:`cat`
--------------

You can use the :command:`cat` command to create various text files.
``EOF`` signifies *End Of File*, this is where the file ends.
Example with a file :file:`packages.list`, this removes the current contents of the file and lets you add some new text:

.. code-block:: console

   $ cat > packages.list << "EOF"
   The text...
   and more text
   
   EOF

To append to the following text file, put two *greater than* signs (``>>``) after :command:`cat`, example:

.. code-block:: console

   $ cat >> packages.list << "EOF"
   The text...
   
   EOF


Navigate the web
----------------

Surf the web quickly and simply with the :program:`retawq` text-mode web browser.
Note that you can also use the local browser.
You can then navigate easily with the arrows on your keyboard — links are colored blue and can be followed by pressing :kbd:`Enter`:

.. code-block:: console

   $ retawq http://www.slitaz.org/en

or:

.. code-block:: console

   $ retawq http://localhost/


Talk on IRC
-----------

To discuss and transfer files via the many IRC servers available, SliTaz provides :program:`LostIRC`.
The IRC client is simple, fast and lightweight, providing a pleasant, easy to handle GTK configuration menu.
One of the main IRC channels for slitaz is ``irc.freenode.net#slitaz``


Download files
--------------

To download various file formats on the internet, you have the :command:`wget` command.
To grab a simple html page, the contents of a folder or an entire website:

.. code-block:: console

   $ wget http://www.slitaz.org/en/doc/


List the available partitions
-----------------------------

To list the partitions on an internal or external hard drive, you can use :command:`cat` to display the contents of :file:`/proc/partitions` or use the :command:`fdisk` utility with the ``-l`` option meaning *list*.
You can then mount the individual partition(s) that you want to use:

.. code-block:: console

   $ cat /proc/partitions

or:

.. code-block:: console

   # fdisk -l


Mount a partition, CD or USB drive
----------------------------------

To mount a local partition in the SliTaz filesystem, we recommend you use the :file:`/mnt` directory.
Example creating the necessary directory and mounting the :file:`hda6` partition of the first local hard drive on :file:`/mnt/hda6`:

.. code-block:: console

   # mkdir -p /mnt/hda6
   # mount -t ext3 /dev/hda6 /mnt/hda6

SliTaz functions in RAM, you can mount the same CD-ROM or remove it to mount another (:file:`/dev/cdrom` is a link to the first CD-ROM drive).
Note that a CD-ROM is a removable medium and should be mounted on :file:`/media`:

.. code-block:: console

   # mount -t iso9660 /dev/cdrom /media/cdrom

To mount a USB or flash drive you must specify the proper filesystem.
Normally a USB key is formatted in FAT32 which can be read from GNU/Linux and Windows operating systems.
On a GNU/Linux system is it generally recognized as the :file:`sda1` device — we now prepare a link :file:`sda1` on flash to facilitate the task.
Note it is also a removable medium and should be mounted on :file:`/media`:

.. code-block:: console

   # mount -t vfat /dev/flash /media/flash


Turn off the system or restart
------------------------------

To stop or restart SliTaz, you can use the :command:`halt` or :command:`reboot` commands or the :kbd:`Ctrl`\ +\ :kbd:`Alt`\ +\ :kbd:`Delete` key combination which enables a system reboot.
In case of any problems you can use the ``-f`` option signifing forced:

.. code-block:: console

   # halt

To restart:

.. code-block:: console

   # reboot

Or:

.. code-block:: console

   # reboot -f
