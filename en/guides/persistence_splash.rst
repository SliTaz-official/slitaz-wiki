.. http://doc.slitaz.org/en:guides:persistence_splash
.. en/guides/persistence_splash.txt · Last modified: 2014/11/26 16:14 by brianperry

.. _persistence splash:

Persistence & splash
====================

This document details how you can add persistence to your live SliTaz distro, and how you can modify the splash screen.
There are different ways on how you can do add persistence (see `How to make Slitaz USB persistent <https://superuser.com/questions/123399/how-to-make-slitaz-usb-persistent>`_).
Unless you decide not to keep the :program:`SLiM` login manager in SliTaz however, all methods first require you to have the live SliTaz distro placed on a media (USB stick, …) that is formatted in ext2 or ext3 (if you don't want to keep the :program:`SLiM` login manager, FAT32 can also be used instead).
FAT32 has the advantage that you can view/mount the media in Windows too, and you can format it in Windows simply by means of the windows explorer application.
If you don't need this functionality though, you might want to use ext2 or ext3 however, as it is a more secure filesystem.


Method 1
--------

By typing ``slitaz home = sdxx`` or ``slitaz home=UUID`` at the command line at the splash/boot screen.
See :ref:`livecd slitaz parameters`.
This option hence involves typing in this line every time you want persistence.
It's also only useful when keeping the SLiM login manager in place, as you can't specify to (immediately) log in as user or root (either with or without persistence).

.. compound::
   The ``home=sdxx`` or ``home=UUID`` basically just tells SliTaz which disk it is where it needs to write the system changes to (which are being written away as a :file:`rootfs.gz` file, see below).
   It should btw be noted here that ``home=UUID`` is preferred over ``home=sdxx`` or even ``home=usb`` (the latter is the same as ``home=sda1``).
   This, as depending on which drives you connect at any given time, your drive name may change, which will give problems (this happens as depending on the access speed of the drive (speed depending on the type of drives connected/inserted, i.e. SATA drive, USB drive, …), Linux will assign a different drive name).
   Then, to effectively write away the changes, you need to type in

   .. code-block:: console

      # tazusb writefs gzip

   (for fast compression, average filesize),

   .. code-block:: console

      # tazusb writefs lzma

   (for slow compression, smaller filesize) or

   .. code-block:: console

      # tazusb writefs none

   (for no compression, larger filesize).
   Each of these will write everything to a :file:`rootfs.gz` archive to be loaded the next time you boot.

One other way to write away the changes are to right-click on desktop :menuselection:`SliTaz Live --> TazUSB Writefs (gzip)`.
With this latter method, you will however then also need to rename the :file:`rootfs.gz` file (i.e. to :file:`rootfs5.gz`) and move the file from the root folder (:file:`/`) to the :file:`/boot` folder on your USB stick drive.
Finally, you need to manually add this file to :file:`syslinux.cfg` (or :file:`extlinux.conf`, see below).


Method 2
--------

Another way to tell SliTaz the disk it needs to write to is by simply modifying or adding an entry to the :file:`extlinux.conf` file (which is located at :file:`/home/boot/extlinux`).
See also `How to make changes persistent on a usb key live installation? <http://forum.slitaz.org/topic/how-to-make-changes-persistent-on-a-usb-key-live-installation>`_.
This method is the only method that doesn't require typing in commands every time you boot and require persistence (more automated).
Note that the same method can also be followed when you have a FAT32/non-SLiM login manager setup, however, you will then need to do the changes in the :file:`syslinux.cfg` file (located at :file:`/home/boot/syslinux`), see `How to make Slitaz USB persistent <https://superuser.com/questions/123399/how-to-make-slitaz-usb-persistent>`_, and not :file:`extlinux.conf`.
To change the :file:`extlinux.conf` or :file:`syslinux.cfg` (which one you need to alter will depend on the filesystem you use), you need to pick one of the SliTaz entries you're currently not using at the boot screen (i.e. *base*, *justx*, … entry), and then modify it.
Alternatively, you can make an entirely new label as well (see `How to make changes persistent on a usb key live installation? <http://forum.slitaz.org/topic/how-to-make-changes-persistent-on-a-usb-key-live-installation>`_.
Make it look like this::

  LABEL my slitaz
  MENU LABEL My slitaz or "whatever you like"
  KERNEL /boot/bzImage
  APPEND initrd=/boot/rootfs4.gz,/boot/rootfs3.gz,/boot/rootfs2.gz,/boot/rootfs1.gz rw root=/dev/null vga=normal autologin home=UUID lang=en_GB kmap=uk

Note that the UUID will be a range of numbers which you can copy from another entry, or which you find by looking at the properties of the (removable USB) drive you want to write to.

Note that you can also make both a root and a user account (each with or without persistence, so 4 accounts in total).
This can be done by adding the ``home=UUID`` or by leaving it out (no persistence).


Method 3
--------

The last method is done by simply typing in some commands at the terminal (see `Slitaz on USB / persistent <http://forum.slitaz.org/topic/slitaz-on-usb-persistent>`_.
So, after booting into SliTaz, just bring up the terminal, log in as root, and then type:

.. code-block:: console

   # blkid

Edit your boot code so ``home=UUID``

.. code-block:: console

   # blkid
   /dev/sda1: UUID="2c55c420-760a-4fa3-871b-64191dcc338a" TYPE="ext2"
   root@slitaz:~# cat /proc/cmdline
   initrd=/boot/rootfs4.gz,/boot/rootfs3.gz,/boot/rootfs2.gz,/boot/rootfs1.gz rw root=/dev/null
   vga=normal autologin home=2c55c420-760a-4fa3-871b-64191dcc338a BOOT_IMAGE=/boot/bzImage

.. important::
   Having read the :ref:`modify the isolinux configuration` page, I seem to understand that what was the "main boot loader" was actually the :program:`isolinux` boot loader, which in turn starts :program:`syslinux` (with FAT32 USB sticks).
   With ext-formatted drives, :program:`syslinux` itself then also starts :program:`extlinux` before that boot loader can load SliTaz.

   Is this correct?
   If so, update the text accordingly above.
   One really need to now this to know what files to edit.
   Also, is it possible to leave out booting the :program:`syslinux` boot loader completely when booting a SliTaz LiveUSB stick formatted in ext3?
   If possible, I suppose one can just alter the :program:`isolinux` files to accomplish this, and then make 2 versions of the customised slitaz distro if one wants to; knowlingly

   * one for FAT32-formatted sticks (:program:`isolinux` booting :program:`syslinux` which then boots SliTaz)
   * one for ext-formatted sticks (:program:`isolinux` booting :program:`extlinux` which then boots SliTaz)

.. important::
   I looked at the ext3-based SliTaz 5 version I made a few days ago to check whether there is indeed an :program:`extlinux` as well as an :program:`syslinux` folder at :file:`/boot/` (and see whether there are any :program:`isolinux` files in :file:`/`).
   I didn't find a :file:`syslinux` folder though (which would imply it automatically loads :program:`extlinux` after loading the main boot loader :program:`isolinux`).
   The only folder I found was an :file:`extlinux` folder at :file:`/boot/`.
   Other files at :file:`/boot/` were :file:`bzImage`, :file:`rootfs.gz`, :file:`rootfs2.gz`, :file:`rootfs3.gz`, :file:`rootfs4.gz`.
   So besides not finding a :program:`syslinux` boot loader, I also didn't find any :program:`isolinux` files at :file:`/`, but I did found an :program:`isolinux` file at :file:`/boot/extlinux`, confirming that this :program:`isolinux` boot loader is indeed the first boot loader that is started.

   In the :file:`/boot/extlinux` folder, I found both the :file:`isolinux.cfg` file mentioned above, as well as :file:`extlinux.conf`.
   I added in the files in annex as texts file.
   I'm not sure but this would hence seem to imply that there is also no :file:`/boot/syslinux/` folder made at all (for any SliTaz version, fat32 or ext3).
   I'm not sure on this however.


Rolling back
------------

If anything should go wrong when writing your filesystem, you can simply rollback to your previous filesystem by typing ``previous`` at the ``boot:`` prompt.
Older backups are named :file:`rootfs.gz.{unixtimestamp}` and can be safely deleted from the :file:`/home` folder to save disk space using :command:`tazusb clean`.


See also
--------

* :ref:`Modifying isolinux <modify the isolinux configuration>` (:program:`isolinux` appearantly being the "main boot loader" which starts :program:`syslinux`, :program:`syslinux` itself starting :program:`extlinux` if you have an ext-formatted USB drive with SliTaz on it.


Additional links/references
---------------------------

* http://forum.slitaz.org/topic/slitaz-40-liveusb-writefs
* http://forum.slitaz.org/topic/a-couple-of-newbie-questions
