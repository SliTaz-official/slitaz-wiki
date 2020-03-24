.. http://doc.slitaz.org/en:guides:microcode
.. en/guides/microcode.txt · Last modified: 2018/04/29 09:10 by linea

.. _microcode:

Updating the processor microcode
================================

:author: ceel, linea

.. important::
   Updating the microcode of your processor can help to preserve your system against Spectre, a vulnerability that affects most of the modern processors.

   For more information about Spectre (and Meltdown), read `Important info about Meltdown and Spectre`_.

In the normal way, the microcode is loaded on boot from the BIOS.
However it is possible to update it on Linux; this must be repeated on every boot.
This document decribes how to proceed for Intel processors.


Install the :program:`intel-ucode` package
------------------------------------------

First of all, update your system:

.. code-block:: console

   # tazpkg up

Then get-install the :program:`intel-ucode` package:

.. code-block:: console

   # tazpkg -gi intel-ucode

The package manager installs microcodes for Intel processors in :file:`/lib/firmware/intel-ucode`; files are stored with names in the format ``XX-YY-ZZ``.


.. _microcode step 2:

Which microcode for your processor
----------------------------------

The command

.. code-block:: console

   # head -n7 /proc/cpuinfo

should return something like this:

.. code-block:: console

   # head -n7 /proc/cpuinfo
   processor       : 0
   vendor_id       : GenuineIntel
   cpu family      : 6
   model           : 60
   model name      : Intel(R) Core(TM) i3-4000M CPU @ 2.40GHz
   stepping        : 3
   microcode       : 0x1c

Convert the cpu ``family-model-stepping`` numbers in 3 pairs of hexadecimal values.

From the example above,

* cpu family= 6 → 06,
* model= 60 → 3c and
* stepping=3 → 03.

The microcode for the processor is ``06-3c-03``.

If you don't find your ``XX-YY-ZZ`` file in :file:`/lib/firmware/intel-ucode`, this means there is no microcode available for your processor; if there is one, force its loading:

.. code-block:: console

   # echo 1 > /sys/devices/system/cpu/microcode/reload

Check if the revision of the microcode has changed:

.. code-block:: console

   # dmesg | grep microcode

.. code-block:: text
   :emphasize-lines: 3,6,9,12

   microcode: CPU0 sig=0x306c3, pf=0x10, revision=0x1c
   microcode: CPU0 sig=0x306c3, pf=0x10, revision=0x1c
   microcode: CPU0 updated to revision 0x24, date = 2018-01-21
   microcode: CPU1 sig=0x306c3, pf=0x10, revision=0x1c
   microcode: CPU1 sig=0x306c3, pf=0x10, revision=0x1c
   microcode: CPU1 updated to revision 0x24, date = 2018-01-21
   microcode: CPU2 sig=0x306c3, pf=0x10, revision=0x1c
   microcode: CPU2 sig=0x306c3, pf=0x10, revision=0x1c
   microcode: CPU2 updated to revision 0x24, date = 2018-01-21
   microcode: CPU3 sig=0x306c3, pf=0x10, revision=0x1c
   microcode: CPU3 sig=0x306c3, pf=0x10, revision=0x1c
   microcode: CPU3 updated to revision 0x24, date = 2018-01-21
   microcode: Microcode Update Driver: v2.00 <tigran@aivazian.fsnet.co.uk>, Peter Oruba

If the revision hasn't changed, then there is no update for your processor; if it did change, then continue with this procedure.


Loading the microcode at boot
-----------------------------

The best thing consists of loading the microcode as soon as possible at boot, before the userspace has started.
This is done by adding an initrd in the GRUB configuration file.

Create the appropriate environment (no matter where you do it):

.. code-block:: console

   # mkdir -p initrd/kernel/x86/microcode

Change in initrd:

.. code-block:: console

   # cd initrd

Copy your microcode (replace ``XX-YY-ZZ`` with your microcode file name):

.. code-block:: console

   # cp -v /lib/firmware/intel-ucode/XX-YY-ZZ kernel/x86/microcode/GenuineIntel.bin

Create the initrd:

.. code-block:: console

   # find . | cpio -o -H newc > /boot/microcode.img

Add the following line in your :file:`/boot/grub/menu.lst` (or :file:`grub.cfg`):

.. code-block:: text

   initrd /boot/microcode.img

If you already use an initrd to boot your system, you must load the microcode first; below an example to start SliTaz Rolling in frugal mode:

.. code-block:: text

   #title SliTaz Rolling core64 - frugal install (kernel 3.16.55)
      root (hd0,2)
      kernel /slitaz/boot/vmlinuz-3.16.55-slitaz64 root=/dev/null autologin
     initrd /slitaz/boot/microcode.img /slitaz/boot/rootfs.gz

Reboot the computer and check the microcode has been loaded:

.. code-block:: console
   :emphasize-lines: 8

   $ head -n7 /proc/cpuinfo
   processor       : 0
   vendor_id       : GenuineIntel
   cpu family      : 6
   model           : 60
   model name      : Intel(R) Core(TM) i3-4000M CPU @ 2.40GHz
   stepping        : 3
   microcode       : 0x24

If you are using a 32bit kernel, it is possible that the CPUs aren't all updated; this is not a problem because the kernel doesn't use them.
Below, an example with Rolling Core:

.. code-block:: console
   :emphasize-lines: 8,9

   $ cat /var/log/messages | grep -i microcode
   Mar 27 18:05:01 (none) user.warn kernel: [<c1020ea2>] ? save_microcode.constprop.5+0x55/0xba
   Mar 27 18:05:01 (none) user.warn kernel: [<c1748ae5>] ? save_microcode_in_initrd_intel+0x25/0x50
   Mar 27 18:05:01 (none) user.warn kernel: [<c1748920>] ? save_microcode_in_initrd+0x1d/0x30
   Mar 27 18:05:01 (none) user.err kernel: Cannot save microcode patches from initrd.
   Mar 27 18:05:01 (none) user.info kernel: microcode: CPU0 sig=0x306c3, pf=0x10, revision=0x24
   Mar 27 18:05:01 (none) user.info kernel: microcode: CPU1 sig=0x306c3, pf=0x10, revision=0x24
   Mar 27 18:05:01 (none) user.info kernel: microcode: CPU2 sig=0x306c3, pf=0x10, revision=0x1c
   Mar 27 18:05:01 (none) user.info kernel: microcode: CPU3 sig=0x306c3, pf=0x10, revision=0x1c
   Mar 27 18:05:01 (none) user.info kernel: microcode: Microcode Update Driver: v2.00

The same computer with Rolling Core64:

.. code-block:: console
   :emphasize-lines: 8,9

   $ cat /var/log/messages | grep -i microcode
   Mar 27 18:05:01 (none) user.warn kernel: [<c1020ea2>] ? save_microcode.constprop.5+0x55/0xba
   Mar 27 18:05:01 (none) user.warn kernel: [<c1748ae5>] ? save_microcode_in_initrd_intel+0x25/0x50
   Mar 27 18:05:01 (none) user.warn kernel: [<c1748920>] ? save_microcode_in_initrd+0x1d/0x30
   Mar 27 18:05:01 (none) user.err kernel: Cannot save microcode patches from initrd.
   Mar 27 18:05:01 (none) user.info kernel: microcode: CPU0 sig=0x306c3, pf=0x10, revision=0x24
   Mar 27 18:05:01 (none) user.info kernel: microcode: CPU1 sig=0x306c3, pf=0x10, revision=0x24
   Mar 27 18:05:01 (none) user.info kernel: microcode: CPU2 sig=0x306c3, pf=0x10, revision=0x24
   Mar 27 18:05:01 (none) user.info kernel: microcode: CPU3 sig=0x306c3, pf=0x10, revision=0x24
   Mar 27 18:05:01 (none) user.info kernel: microcode: Microcode Update Driver: v2.00


Update the processor microcode regularly
----------------------------------------

Intel provide frequent updates of their microcodes.
You can download the latest version of the Intel microcodes at `<https://downloadcenter.intel.com/download/27591/Linux-Processor-Microcode-Data-File>`_.

As the root user, uncompress the downloaded :file:`microcode-{YYYYMMDD}.tgz` file in :file:`/lib/firmware`.

.. code-block:: console

   # tar -xzf microcode-YYYYMMDD.tgz /lib/firmware/

Then follow this HOWTO from :ref:`microcode step 2`.


.. _Important info about Meltdown and Spectre: http://forum.slitaz.org/topic/-important-info-about-meltdown-and-spectre-
