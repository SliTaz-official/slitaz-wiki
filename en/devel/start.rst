.. http://doc.slitaz.org/en:devel:start
.. en/devel/start.txt · Last modified: 2015/12/31 11:11 by linea

.. _devel:

Developers Corner
=================

:author: bellard, linea

This page aims to list SliTaz tricks that you can't find in other distributions.


ISO9660 image file
------------------

The SliTaz GNU/Linux distrbution is published as a downloadable iso image file.
The different flavors (base, core64, loram...) are built in this format.

Nowadays booting on a CD-ROM is not so fashionable.
People prefer to boot their SliTaz from a USB key, a memory card or a hard disk.

It's easy to create a USB key under Linux with the dd_ command.
Some third party Windows tools aim to be able to create USB keys, but most of them don't support the SliTaz :ref:`many-in-one <manyinone>` format (also known as *russian dolls*).

.. tip::
   SliTaz ISO images are also a DOS/Windows program to create USB keys.

   .. code-block:: doscon

      C:\> ren slitaz.iso mkusbkey.exe

People would like to add some personal data to the ISO image such as a *Wifi configuration* or *SSH keys*.
But the ISO remastering is not an easy thing to do.

SliTaz provides a linux tool iso2exe_ and the DOS/Windows tool `isohybrid.exe`_ to add a custom initrd and some extra cmdline arguments.

.. note::
   By the way, these tools add the DOS/Windows program to create USB keys.

A Linux usage can be::

  iso2exe -a "rdinit=init.custom" -i initrd.gz slitaz.iso -f

A DOS (16 bits) or Windows (32 bits) usage can be:

.. code-block:: doscon

   C:\> isohybrid -a "rdinit=init.custom" -i initrd.gz slitaz.iso -f

.. tip::
   The executable shell script :file:`/init.custom` in the :file:`initrd.gz` file installs the extra software in the boot scripts:

   .. code-block:: shell

      #!/bin/sh
      
      # Add custom kernel modules
      sed -i 's/LOAD_MODULES="/&amodule anothermodule/' /etc/rcS.conf
      
      # Start extra daemons
      sed -i 's/RUN_DAEMONS="/&demon1 demon2/' /etc/rcS.conf
      
      # Custom boot commands
      cat >> /etc/init.d/local.sh <<EOT
      shell commands...
      EOT
      
      # Continue normal boot sequence
      exec /init

An iso9660 image file has three parts:

* a 32Kb header filled with zeros
* a `ISO 9660`_ filesystem
* a tail filled with zeros to round up the file size to the next megabyte


The ISO header: to create a USB key from DOS/Windows
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This is a 3 in 1 header:

* a `Master boot record`_ from isolinux_ to boot from a USB key
* a 16 bits DOS `.exe`_ file to launch a Linux utility menu with a USB creation item
* a 32 bits Windows `.exe`_ file to create a USB key

.. note::
   The `El Torito`_ boot is untouched (as a part of the ISO9660 filesystem)

It includes a iso9660 filesystem md5sum_ hash and its own checksum (in
the `.exe`_ file header).
Design rational can be found in the `README file`_

The taziso_ tool can both show and use the ISO header features.

.. tip::
   A taziso graphical/web interface is available in the tazpanel utility under the :menuselection:`boot --> mine` menu item.

.. tip::
   Most of the ISO header features can be used with other live Linux distributions such as `Tiny Core Linux`_, `Puppy Linux`_ or KNOPPIX_ (i.e. you can use :program:`iso2exe` or :program:`isohybrid.exe` with these distributions too).


The ISO tail: to store your configurations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ISO tail has a magic string and its own md5 hash.
The md5 can be checked by taziso and the ISO boot menu under DOS.
The ISO9660 filesystem is untouched.
The md5 hash in the boot area is still valid.

The :program:`isolinux` bootloader is modified by SliTaz to load the custom configuration unlike other distributions.

Small custom configurations should not change the ISO image size thanks to the megabyte alignment.
Larger configurations can extend the ISO image without limitation.

.. tip::
   Custom configurations help to test the `SliTaz weekly build`_ with automatic wifi setups or SSH keys…
   You can add the following entry in your grub config, have a custom automatic setup and still test the real CD-ROM boot sequence.

   ::

     SliTaz rolling iso
     	map --mem --heads=0 --sectors-per-tracks=0 /boot/slitaz-rolling.iso (hd32)
     	map --hook
     	chainloader (hd32)


Packages enhancements
---------------------


.. rubric:: dropbear

:program:`Dropbear` is a SSH2 client and server.
The server side supports X11 forwarding but the client does not.
SliTaz provides a tiny shell script named sshx_ to restore this feature.
By the way 2 other scripts are given:

* :program:`pppssh` a poor mans VPN.
  TCP based, it can add delays to the VPN network.
* :program:`sshfbvnc` adds authentication and encryption to the :program:`fbvnc` viewer.


.. rubric:: sshfs-fuse

:program:`Sshfs-fuse` can mount a remote filesystem on the local machine.
SliTaz provides a tiny script named rsshfs_ to mount a local filesystem on a remote machine.


.. rubric:: cloop

Cloop packages (:program:`fusecloop`, :program:`cloop-utils`…) are able to mount **any** cloop formats.
The official software fails to mount earlier formats.

They add a `new format`_ to reduce the memory consumption during the compressed file creation.


.. rubric:: ipxe

This PXE has a `built in configuration`_ to boot from a SliTaz server and can be used without a local PXE server.


.. rubric:: sane-backends

The scanner drivers package adds a GUI in a `tazpanel module`_.


.. _iso2exe: http://cook.slitaz.org/cooker.cgi?download=../wok/syslinux-extra/taz/syslinux-extra-4.06/fs/usr/bin/iso2exe
.. _taziso:  http://cook.slitaz.org/cooker.cgi?download=../wok/syslinux-extra/taz/syslinux-extra-4.06/fs/usr/bin/taziso

.. _isohybrid.exe:          http://mirror.slitaz.org/boot/isohybrid.exe
.. _SliTaz weekly build:    http://mirror.slitaz.org/iso/rolling/slitaz-rolling.iso

.. _README file:            http://hg.slitaz.org/wok/file/tip/syslinux/stuff/iso2exe/README#l1
.. _sshx:                   http://hg.slitaz.org/wok/file/tip/dropbear/stuff/sshx#l1
.. _rsshfs:                 http://hg.slitaz.org/wok/file/tip/sshfs-fuse/stuff/rsshfs#l1
.. _new format:             http://hg.slitaz.org/wok/file/tip/fusecloop/description.txt#l1
.. _built in configuration: http://hg.slitaz.org/wok/file/tip/ipxe/stuff/ipxe.cmd#l1
.. _tazpanel module:        http://hg.slitaz.org/wok/file/tip/sane-backends/stuff/tazpanel/sane.cgi#l1

.. _dd:                     http://en.wikipedia.org/wiki/dd_(Unix)
.. _ISO 9660:               http://en.wikipedia.org/wiki/ISO_9660
.. _Master boot record:     http://en.wikipedia.org/wiki/Master_boot_record
.. _isolinux:               http://en.wikipedia.org/wiki/isolinux
.. _.exe:                   http://en.wikipedia.org/wiki/.exe
.. _El Torito:              http://en.wikipedia.org/wiki/El_Torito_(CD-ROM_standard)
.. _md5sum:                 http://en.wikipedia.org/wiki/md5sum
.. _Tiny Core Linux:        http://en.wikipedia.org/wiki/Tiny_Core_Linux
.. _Puppy Linux:            http://en.wikipedia.org/wiki/Puppy_Linux
.. _KNOPPIX:                http://en.wikipedia.org/wiki/KNOPPIX
