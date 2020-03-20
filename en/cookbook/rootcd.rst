.. http://doc.slitaz.org/en:cookbook:rootcd
.. en/cookbook/rootcd.txt · Last modified: 2010/09/14 22:49 by linea

.. _cookbook rootcd:

Rootcd
======

Descriptions of files contained on the CD-ROM.


Syslinux/isolinux
-----------------

:program:`Syslinux` and the (SliTaz) main bootloader — we use the :program:`isolinux` version to start the system contained on the CD-ROM.
Simple effective and configurable, :program:`isolinux` was installed during the creation of the base system.
The binary is named :file:`isolinux.bin` and uses the configuration file :file:`isolinux.cfg`.
Here's an example of an :file:`isolinux.cfg` using :file:`isolinux.msg` to post the splash image and displayable help files via :kbd:`F1`, :kbd:`F2`, :kbd:`F3` and :kbd:`F4`.
You will find the files :file:`help.txt`, :file:`options.txt`, etc in :ref:`SliTaz tools <cookbook slitaztools>`.

::

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


Isolinux boot splash image
--------------------------

We can configure :program:`isolinux` to display a splash image when booting SliTaz or any other operating system.
This image has a particular format :file:`.lss`, suitable for :program:`Syslinux` and must be indexed using the 16 color mode.
You can use the official logo, :program:`ppmforge`, :program:`imagemagick`, :program:`GIMP` or other tools to create your image.

The :program:`Syslinux` file (:file:`sample/syslogo.lss`) provides an official logo which you can directly use by copying to the root of the CD-ROM.
SliTaz provides a logo (:file:`rootcd/boot/isolinux/splash.lss`) which you can find in :ref:`SliTaz tools <cookbook slitaztools>`.
To display a splash image when booting, it's necessary that the 'display' option calls the :file:`isolinux.msg` file which loads the :file:`*.lss` format image.
Note that the :file:`isolinux.msg` file uses 24 ASCII characters.
Example using :command:`echo` and an :file:`isolinux.msg` file incorporating a :file:`.lss` splash image:

.. code-block:: console

   # echo -e "\24isplash.lss\n" > isolinux.msg

You can also add a text message underneath the splash image by modifying the file with your favorite text editor, :command:`echo` or :command:`cat` and so on.


ISO bootable with isolinux
--------------------------

To create a bootable ISO image using :program:`isolinux` and :command:`genisoimage`:

.. code-block:: console

   # genisoimage -R -o slitaz-test.iso -b boot/isolinux/isolinux.bin \
   	-c boot/isolinux/boot.cat -no-emul-boot -boot-load-size 4 \
   	-V "SliTaz" -input-charset iso8859-1 -boot-info-table rootcd


GRUB
----

:program:`GRUB` (GRand Unified Bootloader) is a bootloader distributed by the GNU project.
This is used during installation to a hard drive; it can boot Linux, BSD, HURD and Window$.
GRUB provides :file:`stage2_eltorito` to start the ISO images.
To find :file:`stage2_eltorito` on your system, you need to have the GRUB package installed.
Finally you copy :file:`stage2_eltorito` to the root of the cdrom.
Note that SliTaz provides a (:file:`.tazpkg`) package :program:`grub-0.97` that you can find on the mirrors or you can rebuild :program:`grub-0.97` from sources.
Example using a :file:`stage2_eltorito` image from a Debian system or SliTaz:

.. code-block:: console

   # mkdir -p rootcd/boot/grub
   # cp /usr/lib/grub/i386-pc/stage2_eltorito \
   	rootcd/boot/grub

The GRUB configuration file is called :file:`menu.lst` and can be edited with your favorite text editor.
Example::

  # By default, boot the first entry.
  default 0
  
  # Boot automatically after 20 secs.
  timeout 20
  
  # Change the colors.
  color yellow/brown white/black
  
  title  SliTaz GNU/Linux 1.0 (vga 800x600) (Kernel 2.6.20)
         kernel /boot/bzImage root=/dev/null vga=788
         initrd /boot/rootfs.gz
  
  title  SliTaz GNU/Linux 1.0 (vga 1024x768) (Kernel 2.6.20)
         kernel /boot/bzImage root=/dev/null vga=771
         initrd /boot/rootfs.gz


ISO bootable with GRUB
----------------------

To create a bootable ISO image using :program:`GRUB` and :command:`genisoimage` or :command:`mkisofs`:

.. code-block:: console

   # genisoimage -R -o slitaz-test.iso -b boot/grub/stage2_eltorito \
   	-no-emul-boot -V "SliTaz" -boot-load-size 4 -input-charset iso8859-1 \
   	-boot-info-table rootcd


Memtest86
---------

The application :program:`memtest86` is a tool to test random access memory (RAM).
We download the utility into the src directory, decompress the archive, and copy the (precompiled) binary:

.. code-block:: console

   # mkdir -v -p src
   # cd src
   # wget http://www.memtest86.com/memtest86-3.2.tar.gz
   # tar xzfv memtest86-3.2.tar.gz
   # cd memtest86-3.2
   (# more README)
   # cp precomp.bin ../../rootcd/boot/memtest
   # cd ../..

Once installed, you can add the label for the :file:`memtest86` file to :file:`isolinux.cfg`, specifing the path to the utility::

  label memtest
  	kernel /boot/memtest

Or if you want to use :program:`GRUB`, here's the line to launch :program:`memtest86`::

  title 	Memtest86 (Test system memory)
  kernel 	/boot/memtest

Once the lines are added, you can then create a new ISO and test.
