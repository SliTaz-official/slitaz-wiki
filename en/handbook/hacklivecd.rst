.. http://doc.slitaz.org/en:handbook:hacklivecd
.. en/handbook/hacklivecd.txt · Last modified: 2014/11/17 10:32 by brianperry

.. _handbook hacklivecd:

Hacking SliTaz LiveCD
=====================

:author: jozee, linea, brianperry


Introduction
------------

*Hacking SliTaz LiveCD* or how to have fun with the LiveCD ISO image.
Note that you can also create a custom flavor with :program:`Tazlito`.
Creating your own bootable ISO image is easily achievable and the steps are carefully described here.
The manipulation of a personal ISO image can add new files or modify existing ones found on the SliTaz Live CD.
The SliTaz ISO image is less than 30 MB and a CD-R or CD-RW provides around 700 MB, so there's plenty of scope for expansion.
For example, you could store your images and even provide a live slideshow using :program:`GQview`.
The hacking of the ISO image allows you to modify boot loader configuration files (boot loader), splash images and :program:`GRUB` itself.
You could also add the :program:`Memtest86` utility (tool used to test system RAM).
Using the same techniques it's even possible to modify the filesystem — this does however require some extra manipulation and a bit more time.


Organization and preparation
----------------------------

To begin, first we must define where we are going to work by creating a directory and several sub directories to accommodate all the different files.
The hacking of the ISO can be done from within a SliTaz system or any other GNU/Linux distribution such as Debian, Fedora, PCLinuxOS, etc.
If you use SliTaz LiveCD mode (where you can remove the CD once SliTaz has launched in RAM and burn your new ISO), it's advisable to use USB media to carry on working, otherwise your work will be lost on shutdown.
To begin you need to create a hacking directory that you can use inside :file:`/home/slitaz` within the root of your user space.
The use of a :file:`/home/slitaz` directory enables you to store an original ISO image and gives you the option to create a :file:`src/` directory to download possible source packages.
All the various stages of hacking can be done on the command line via a X terminal (:program:`Xterm`) or in console mode on a Linux terminal.
It's advisable to run all commands as root to avoid any permission problems.
To become the (root) adminsistrator, create a :file:`/home/slitaz/hacked` directory and proceed inside:

.. code-block:: console

   $ su
   # mkdir -p /home/slitaz/hacked
   (# mkdir -p /home/slitaz/src)
   # cd /home/slitaz/hacked


.. rubric:: Getting the contents of the ISO

Now that you are in the working directory, we must create the root of the amended CD-ROM and retrieve the files contained on the original SliTaz ISO — namely, the Linux Kernel (:file:`bzImage`), the compressed filesystem (:file:`rootfs.gz`) and the :file:`isolinux` bootloader files.
To recover these files you have two options, either take them from a burned CD or from an ISO image stored locally.
To create the root of your CD (:file:`rootcd`) and copy files from the CD-ROM device :file:`/dev/cdrom` mounted on :file:`/media/cdrom`:

.. code-block:: console

   # mount -t iso9660 /dev/cdrom /media/cdrom
   # mkdir rootcd
   # cp -a /media/cdrom/* rootcd

To mount an ISO image using loop in the temporary directory :file:`/tmp/loop` (with the ISO image :file:`slitaz-cooking.iso`), create the root of the CD (:file:`rootcd`), copy all the files and dismount the ISO image:

.. code-block:: console

   # mkdir /tmp/loop
   # mount -o loop slitaz-cooking.iso /tmp/loop
   # mkdir rootcd
   # cp -a /tmp/loop/* rootcd
   # umount /tmp/loop

Voilà, all the necessary files should now be present in the :file:`rootcd/` directory.
To be sure, you can list all of the files recursively with the :command:`ls` command:

.. code-block:: console

   # ls -R rootcd


Adding the files to the ISO
---------------------------

The addition of various files and directories to the ISO image simply consists of copying data to the root of the CD-ROM (:file:`rootcd/`) and generating a new image.
The data may be classified in one or two directories created in the root of the CD.
Once the ISO image is burned to a CD-R/CD-RW, you can use SliTaz as before, mounted on :file:`/media/cdrom` and navigate through your data using :program:`emelFM2`, :program:`Clex` or the command line.
Your data will also be legible from all GNU/Linux systems, BSD or even … Windows.


.. rubric:: Create directories and copy data

To create and copy files, you can start by using the command line and then continue on graphically as a simple user.
We will create an :file:`images/` directory as root and change the permissions so that all users have write access:

.. code-block:: console

   # mkdir rootcd/images
   # chmod 777 rootcd/images

Now that a directory exists that anybody can write to, you can start to fill it.
Once you've finished you can then generate a bootable ISO image.


.. _modify the isolinux configuration:

Modify the :program:`isolinux` configuration
--------------------------------------------

The modification of :program:`isolinux` allows you to create custom entries with pre-boot parameters, for example you can add a label launching SliTaz with the ``lang=en`` and ``kmap=en`` options.
At the design level you can easily change the splash image displayed at startup.
The :program:`isolinux` application manages the starting of the boot loader of the LiveCD and is provided by the :program:`Syslinux` package.
The source file of :program:`Syslinux` provides various applications whose role it is to start a GNU/Linux system.
The binary :file:`isolinux.bin` controls the actual boot loading.
The boot loader is simple, fast and easily configured either graphically or using a text editor.
The syntax of the configuration file :file:`isolinux.cfg` is easy to understand — to add new entries just copy and paste using the original file.
To edit the file graphically using :program:`Leafpad`:

.. code-block:: console

   # leafpad rootcd/boot/isolinux/isolinux.cfg &


.. rubric:: Configuration file :file:`isolinux.cfg`

The :file:`isolinux.cfg` file found on the standard LiveCD of SliTaz, begins with the value ``display``, this will either display a text file or a (:file:`isolinux.msg`) file using 24 ASCII characters and a splash image.
The default value defines the name of the label started by default after the (``timeout``) waiting time.
Timeout is the number of seconds to wait before booting the system, you can make it 0 to start booting immediately or choose a waiting time as long as 80s.
Finally the prompt can be deactivated using the value 0.
``F1``, ``F2``, ``F3`` display help files and ``F4`` displays a text file.
Example configuration::

  display isolinux.msg
  default slitaz
  label slitaz
        kernel /boot/bzImage
        append initrd=/boot/rootfs.gz rw root=/dev/null vga=788
  implicit 0
  prompt 1
  timeout 80
  F1 help.txt
  F2 options.txt
  F3 isolinux.msg
  F4 display.txt

Example of a label ``slitazen`` which you can add to the original to directly configure the language of the system as English and use the UK keyboard::

  label slitazen
        kernel /boot/bzImage
        append initrd=/boot/rootfs.gz rw root=/dev/null lang=en kmap=en

Once you've finished modifying the configuration file, don't forget to save your changes and generate a bootable ISO image with :program:`isolinux`.


Install and use :program:`Memtest86`
------------------------------------

The application :program:`memtest86` (92 kB) is a tool for testing your system memory (RAM).
:program:`Memtest86` performs indepth tests, that if failed, point heavily towards a hardware fault.
The tool resides in the :file:`boot/` directory and can be launched directly by typing ``memtest`` at the :program:`isolinux` boot prompt.
Navigate to :file:`/home/slitaz/src` (if the directory doesn't exist: :command:`mkdir -p /home/slitaz/src`), download the source and unpack:

.. code-block:: console

   # cd /home/slitaz/src
   # wget http://www.memtest86.com/memtest86-3.3.tar.gz
   # tar xzf memtest86-3.3.tar.gz

On unpacking the source of the :program:`memtest86` package you'll find a :file:`README` providing information about the tool.
Now you can install into the root CD of your hacked ISO.
Based on the premise that you'll be working with a :file:`/home/slitaz/hacked` directory, we will copy the binary you precompiled into the :file:`boot/` directory of the root of the CD:

.. code-block:: console

   # cp memtest86-3.3/precomp.bin \
     /home/slitaz/hacked/rootcd/boot/memtest

Now that the binary is installed in the root CD, we can just add an entry for :program:`memtest86` to the :program:`isolinux` configuration file and generate a bootable ISO image.
Navigate to :file:`/home/slitaz/hacked` and edit :file:`isolinux.cfg` using :program:`Leafpad`:

.. code-block:: console

   # cd /home/slitaz/hacked
   # leafpad rootcd/boot/isolinux/isolinux.cfg &

Adding the following lines::

  label memtest
        kernel /boot/memtest

Official website of `Memtest86 <http://www.memtest86.com/>`_


Manipulate the Live root system
-------------------------------

Changes to the Live root system allow you for example, to add a new user and password, customize graphics or execute commands automatically at boot time.
The necessary operations for changing the root file system are: extract the compressed file system :file:`rootfs.gz`, modify, rebuild the image and generate the ISO.
Based on the assumption that you've prepared a working directory, we begin by creating a directory to contain the files on the changed system.
Since the compressed root file system is named :file:`rootfs.gz`, we suggest you use :file:`rootfs/` to extract to.
Navigate to the :file:`hacked/` directory, create the root directory and copy the compressed file system from :file:`rootcd/boot/` (the root of the CD-ROM):

.. code-block:: console

   # cd /home/slitaz/hacked
   # mkdir rootfs
   # cp rootcd/boot/rootfs.gz rootfs

Now that you have the compressed copy of the system, just unpack with :program:`cpio`.
Technically :file:`rootfs.gz` is a :program:`cpio` file compressed with :program:`lzma` or :program:`gzip`.
It's recognized like an initramfs image by the Linux Kernel.
At the start up of the machine, the Kernel is loaded into memory and then decompresses the system image and carries out the initialization scripts.

To extract the file system into :file:`rootfs/` and delete the unarchived copy (remember you can copy & paste):

.. code-block:: console

   # cd rootfs
   # (zcat rootfs.gz 2>/dev/null || lzma d rootfs.gz -so) | cpio -id
   # rm rootfs rootfs.gz

The system is now ready to be hacked, you can list all files at the root of your system by using the :command:`ls` command.


.. rubric:: Modify a file

To keep things simple and to help you understand the principle, we are going to change a script file in order to execute some commands to be carried out automatically when the CD starts up.
The target is :file:`etc/init.d/local.sh` — just open with your favorite text editor such as :program:`Geany`:

.. code-block:: console

   # geany etc/init.d/local.sh &

We'll add a command displaying a message and letting the system sleep for 4 seconds.
Example using local script:

.. code-block:: shell

   echo "* Hacked SliTaz version booting..."
   sleep 4


.. rubric:: Rebuilding the image of the compressed system

Once the changes are completed, you can rebuild a compressed image of your system by using :command:`find` to find the files, :command:`cpio` for archiving, :command:`lzma` and :command:`gzip` for compression and the pipe :command:`|` to connect everything together.
This command must be launched from the root system (:file:`rootfs/`) and creates a compressed file :file:`rootfs.gz` in the preceding directory:

.. code-block:: console

   # find . -print | cpio -o -H newc | lzma e -si -so > ../rootfs.gz

Or with :command:`gzip`:

.. code-block:: console

   # find . -print | cpio -o -H newc | gzip -9 > ../rootfs.gz

Finally copy the compressed file system into the :file:`boot/` directory at the root of the CD and generate a bootable ISO image with :program:`isolinux`.
To copy the newly compressed rootfs into the working directory:

.. code-block:: console

   # cd ../
   # cp -a rootfs.gz rootcd/boot


Generate a bootable ISO image
-----------------------------

The following commands create an image with the boot loader :program:`isolinux`, using the :program:`genisoimage` application and a few options.
The name of the ISO is specified at the beginning, after the ``-o`` option and the root directory (:file:`rootcd/`) at the end, after the ``-boot-info-table`` option:

.. code-block:: console

   # genisoimage -R -o slitaz-hacked.iso -b boot/isolinux/isolinux.bin \
     -c boot/isolinux/boot.cat -no-emul-boot -boot-load-size 4 \
     -V "SliTaz-Hacked" -input-charset iso8859-1 -boot-info-table rootcd

If you want to check the contents of the ISO before burning, just mount the image in loop and list the files.
On SliTaz and most GNU/Linux systems, you can burn images in ISO format with the :program:`wodim` utility.


.. rubric:: Generate a new ISO via a script

If you wish to test out a lot of new possibilities and generate a lot of ISO images, you may want to semi-automate the process via a simple SHell script.
This tiny script can be created on the command line or edited graphically, but don't forget to make it executable.
You can create the script with :command:`cat`; note that ``EOF`` signifies *End Of File*.
To create the script :file:`gen_hacked_iso.sh` using two variables to change the name of the ISO image and the path to the root directory of the CD-ROM:

.. code-block:: console

   # cat > gen_hacked_iso.sh << "EOF"

.. code-block:: shell

   #!/bin/sh
   # Gen a new hacked ISO image.
   #
   ISO_NAME="slitaz-hacked.iso"
   ROOTCD="rootcd"
   
   genisoimage -R -o $ISO_NAME -b boot/isolinux/isolinux.bin \
      -c boot/isolinux/boot.cat -no-emul-boot -boot-load-size 4 \
      -V "SliTaz-Hacked" -input-charset iso8859-1 -boot-info-table $ROOTCD
   
   EOF

To use the script, just make it executable and execute:

.. code-block:: console

   # chmod +x gen_hacked_iso.sh
   # ./gen_hacked_iso.sh


See also
--------

* :ref:`persistence splash` — Guide on adding persistence
