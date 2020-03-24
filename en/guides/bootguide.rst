.. http://doc.slitaz.org/en:guides:bootguide
.. en/guides/bootguide.txt · Last modified: 2020/03/15 12:59 by bellard

.. _bootguide:

Boot tree
=========

:author: linea, bellard


From BIOS to :file:`/etc/init.d/rcS`
------------------------------------

.. figure:: image/boot-tree.*

   Boot tree

   .. list-table::
      :widths: 1 28
      :class: longtable

      * - 1
        - :ref:`Uncommon <uncommoninst>` is LVM, RAID, crypto, loop or subdir mount with preinit rootfs.
      * - 2
        - `Versatile boot floppy <http://mirror.slitaz.org/boot/floppy-grub4dos>`_
      * - 3
        - `Boot floppy set <http://mirror.slitaz.org/floppies/>`_.
      * - 4
        - LAN :ref:`PXE <pxe>` or `WEB boot <http://boot.slitaz.org/>`_.
      * - 5
        - :ref:`gpxe.pxe <advanced web booting configuration>`.
      * - 6
        - Tiny slitaz, see http://tiny.slitaz.org/
      * - 7
        - :ref:`Lowram CD <lowramcd>`.
      * - 8
        - :command:`kexec` command from the `kexec-tools <http://pkgs.slitaz.org/search.sh?package=kexec-tools>`_ package.


SliTaz ISO image boot tricks
----------------------------

The CD-ROM image has a hybrid format from version 5.0.

* It boots from a CD-ROM drive according to the *El Torito* specification as usual (BIOS or UEFI)
* It boots from a memory card / USB key using the syslinux hybrid format (BIOS) or using a FAT partition mapped in the ISO image (UEFI).
* It launches a USB boot key creation utility from Windows (32 bits).
* It can boot from DOS (real mode or virtual 8086 with EMM386) directly:

  .. code-block:: doscon

     C:\> ren slitaz.iso slitaz.exe
     C:\> slitaz.exe

  .. tip::
     The files :file:`bzImage`, :file:`memtest` and :file:`ipxe` can boot with DOS too:

     .. code-block:: doscon

        C:\> ren bzimage bzimage.exe
        C:\> bzimage.exe root=/dev/hda3 autologin

        C:\> ren memtest memtest.exe
        C:\> memtest.exe

        C:\> ren ipxe ipxe.exe
        C:\> ipxe.exe http://myserver.org/boot.php

* It is easily customizable with the :program:`iso2exe` (compatible with :program:`taziso` or :menuselection:`tazpanel --> boot --> ISO mine`) tool:

  .. code-block:: shell

     iso2exe -a "lang=fr_FR kmap=fr-latin1 tz=Europe/Paris" -i myconfig.gz slitaz.iso
     iso2exe -l slitaz.iso
     iso2exe -r slitaz.iso custom.append custom.initrd
     taziso slitaz.iso getcustomconf
     taziso slitaz.iso isomd5

  .. tip::
     You can tune the boot process with your own :file:`/init` script:

     .. code-block:: shell

        #!/bin/sh
        
        sed -i 's,^RUN_DAEMONS=",RUN_DAEMONS="dropbear ,' /etc/rcS.conf
        cat >> /etc/init.d/local.sh <<EOM
        /my/special/inits.sh
        EOM
        exec /init "$@"

     Update the kernel command line:

     .. code-block:: shell

        iso2exe -a "rdinit=/myinit lang=fr_FR kmap=fr-latin1 tz=Europe/Paris" -i myconfig slitaz.iso -f
