.. http://doc.slitaz.org/en:guides:lowramcd
.. en/guides/lowramcd.txt · Last modified: 2011/05/19 10:34 by bellard

.. _lowramcd:

Live CD for Low-RAM Systems
===========================

:author:  jozee, seawolf, linea, bellard

The minimum RAM requirement for the SliTaz core Live CD is 160MB (128MB for 1.0).
Many graphical applications won't run with such a low amount of memory; so, using the text-mode ``screen=text`` boot option is recommended.

The packages :program:`slitaz-loram`, :program:`slitaz-loram-http` or :program:`slitaz-loram-cdrom` can be used to build a LiveCD for systems with RAM larger than 64MB, 32Mb or 24MB respectively.
These can be installed directly on the host system, rather than specified in the packages list for the LiveCD you are building.

* :program:`slitaz-loram` will compress the :file:`/usr` tree and the system will still run in RAM.
  It will not use a CD-ROM, harddisk or USB key
* :program:`slitaz-loram-http` will get the :file:`/usr` tree from an ISO image stored in http://mirror.slitaz.org/ built using :program:`slitaz-loram-cdrom`, and pass the ``tiny`` keyword while booting from the web
* :program:`slitaz-loram-cdrom` will move the :file:`/usr` tree onto the CD-ROM

:file:`/usr` will be read-only.
If the package :program:`funionfs` or :program:`aufs` is installed you will have read-write access to :file:`/usr`.

These packages patch :file:`/etc/init.d/rcS` to mount :file:`/usr` and install two scripts in :file:`/etc/tazlito`:

* :file:`loram.rootfs` is called by :command:`tazlito gen-distro` to compress or move :file:`/usr`
* :file:`loram.extract` is called by the :command:`slitaz-installer` to uncompress or move :file:`/usr` and install the same distribution as the SliTaz core LiveCD

Let's build a :file:`slitaz-loram-cdrom.iso`!


slitaz-loram-cdrom
------------------

We boot the LiveCD and install the :program:`slitaz-loram-cdrom` package:

.. code-block:: console

   # tazpkg get-install slitaz-loram-cdrom

:command:`tazlito gen-distro` will then create an ISO with the packages listed in :file:`/etc/tazlito/distro-packages.list` or :file:`./distro-packages.list`.
Since some more packages are installed, we remove these files to force :program:`tazlito` to use all of the installed packages.

.. code-block:: console

   # rm -f /etc/tazlito/distro-packages.list ./distro-packages.list

.. compound::
   Now we can build the ISO image…

   .. code-block:: console

      # tazlito gen-distro

   …and burn it.

   .. code-block:: console

      # wodim dev=1,0,0 /home/slitaz/distro/slitaz-hacked.iso


Variations of slitaz-loram & slitaz-loram-cdrom
-----------------------------------------------

:program:`slitaz-loram` compresses :file:`/usr` with *cromfs* by default, which gives a higher compression ratio but is very slow.
You can use *squashfs* instead:

* refuse to install *cromfs* during the :program:`slitaz-loram` installation

  .. code-block:: console

     # yes n | tazpkg get-install slitaz-loram

* install :program:`squashfs` with its dependencies

  .. code-block:: console

     # yes y | tazpkg get-install squashfs

:program:`slitaz-loram-cdrom` moves :file:`/usr` uncompressed to the CD-ROM and produces a 90MB ISO.
If you install *cromfs* or :program:`squashfs`, :file:`/usr` will be compressed on the LiveCD and the ISO size will be around 30 megabytes.


Let's build a :file:`slitaz-loram-cdrom-sqfs.iso`!
--------------------------------------------------

Install the package :program:`slitaz-loram-cdrom` and :program:`squashfs` (sqfs) on the host system:

.. code-block:: console

   # tazpkg get-install slitaz-loram-cdrom
   # yes y | tazpkg get-install squashfs

Now we repeat the latter points of the above process:

.. code-block:: console

   # rm -f /etc/tazlito/distro-packages.list ./distro-packages.list
   # tazlito gen-distro
   # wodim dev=1,0,0 /home/slitaz/distro/slitaz-hacked.iso


slitaz-loram-cdrom and Large Memory Systems
-------------------------------------------

When the :program:`slitaz-loram-cdrom` LiveCD detects enough memory during boot, :file:`/usr` is copied from the CD-ROM to RAM.
You can eject and/or use the CD drive.
The system behaves as a regular LiveCD in this case:

* a slitaz LiveCD (:file:`/usr` was not compressed on the CD-ROM)
* a slitaz-loram LiveCD (:file:`/usr` was compressed on the CD-ROM by :program:`squashfs` or *cromfs*)


slitaz-loram-cdrom and Tiny Memory Systems
------------------------------------------

The boot command line is usually::

  boot: slitaz args...

SliTaz boots on a 9MB RAM system with the boot command::

  boot: loram single root=/dev/hdc

Where :file:`/dev/hdc` is the CD-ROM device, the loram boot entry avoids RAM disk creation and CD-ROM detection.

.. tip::
   Note that on a system with such a low amount of memory, the first thing to do is add swap!

You need 10MB to use the boot scripts with::

  boot: loram root=/dev/hdc

In this case you can add arguments like ``kmap=``, ``config=``, etc.


slitaz-loram Auto-Extraction
----------------------------

Each slitaz-loram* flavor can be extracted into RAM at boot time (if enough memory is available) by using the boot argument ``extract-loram``.
You will get a core flavor running without read-only restrictions for :file:`/usr`.

For example, assuming you boot the slitaz-loram-cdrom-sqfs::

  boot: slitaz extract-loram

You will get:

* :file:`/usr` read-only squashfs on a CD-ROM with a smaller RAM size
* :file:`/usr` read-only squashfs in RAM with a medium RAM size (like slitaz-loram)
* :file:`/usr` read-write tmpfs in RAM with a larger RAM size (like slitaz-core)


Build a slitaz-loram with tazlitobox
------------------------------------

Since SliTaz 3.0, you can now build a slitaz-loram LiveCD more easily:

* launch :program:`tazlitobox`
* click on the :guilabel:`Low RAM` tab
* select :guilabel:`The filesystem is always in RAM` (for slitaz-loram) or :guilabel:`The filesystem may be on a CD-ROM` (for slitaz-loram-cdrom)
* fill the ISO input with your SliTaz flavor (3.0 or more recent)
* update the ISO output
* click :guilabel:`build ISO`

The filesystem root (:file:`/`) is compressed (not :file:`/usr` only) and mounted read-write thanks to the :program:`aufs` package.
The :command:`mount` and :command:`df` commands will show a strange output, handy to detect that this kind of loram is running.

.. compound::
   You can also use the following command in text mode:

   .. code-block:: console

      # tazlito build-loram original.iso loram.iso

   or:

   .. code-block:: console

      # tazlito build-loram original.iso loram-cdrom.iso cdrom

Meta flavors are supported; you can loramize a slitaz-3in1.iso!


And what to do with only 8MB RAM?
---------------------------------

Try Tiny SliTaz: http://tiny.slitaz.org/!
