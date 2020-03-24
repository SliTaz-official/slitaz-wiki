.. http://doc.slitaz.org/en:guides:pxe
.. en/guides/pxe.txt · Last modified: 2018/03/05 16:48 by bellard

.. _pxe:

PXE
===

:author: jozee, linea, seawolf, bellard

The *Preboot eXecution Environment* (or *PXE*, pronounced 'pixie') is the process of booting a computer from a network connection.
It is comparable to booting a LiveCD from a remote CD drive.

This network boot method requires:

* a server to store files running DHCP and TFTP (each could be on a separate server);
* a client with a PXE boot-loader, stored in the BIOS firmware.
  It maybe disk-less.


.. _pxe server set up:

PXE Server Set-Up
-----------------

A PXE server comprises:

* a DHCP server to accept clients;
* a DHCP boot-file to configure them;
* a TFTP server to deliver an OS.


Quick start with the Live CD
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The SliTaz LiveCD can be used as a PXE server.
To begin the process, launch the Netbox application (from the System Tools menu).

* From the :guilabel:`Static IP` tab, click :guilabel:`Start`.

  * This box will be the DHCP server.
    It can't use DHCP to get an IP configuration.

    .. tip::
       Since SliTaz 3.0 the Netbox application is now split into Netbox and Serverbox.
       If you are running a recent SliTaz version, please read **Serverbox application** instead of **Server tab** below

* From the :guilabel:`Server` tab, select the :guilabel:`INETD` sub-tab and ensure the ``tftpd`` line is **uncommented** in :file:`/etc/initd.conf`.
  This is the default behaviour.
  Click :guilabel:`Start`.

  * This will launch the TFTP server, which will deliver the SliTaz LiveCD across the network.

* From the :guilabel:`Server` tab, select the :guilabel:`PXE` sub-tab.

* Edit the configuration to add your boot options.

  * This will update the DHCP server configuration automatically.

* From the :guilabel:`Server` tab, select the :guilabel:`DHCP` sub-tab.
  Check that the configuration aligns with your network.
  The previous step has added the lines ``boot_file`` and ``siaddr``.
  Click :guilabel:`Start`.

  * This will launch the the DHCP server.
    If clients to do not receive an IP address, check this configuration.

* Ensure the files :file:`bzImage` and :file:`rootfs.gz` are stored in the :file:`/boot` directory of the LiveCD.


Customize your PXE server
^^^^^^^^^^^^^^^^^^^^^^^^^

* You can have multiple PXE configurations for the different client groups, see `PXElinux wiki <http://syslinux.zytor.com/wiki/index.php/PXELINUX#How_do_I_Configure_PXELINUX.3F>`_.
* You can store :file:`/home` on a client local drive only (like tazusb does), example append :file:`/etc/fstab` with :

  ::

    /dev/hda1    /home   ext3    defaults   0  0

* Since SliTaz 3.0, you can have a hydrid installation on (some) clients.
  These clients have SliTaz installed with some huge packages like libreoffice.
  They boot with PXE and most of the system runs in RAM except the huge software linked to the hard disk (could be a network disk too).
  Example, append to :file:`/etc/init.d/local.sh`:

  ::

    mount -t nfs -o ro bootserver:/slitaz  /media/slitaz
    tazpkg link libre-office /media/slitaz

* Since SliTaz 3.0, you can stack multiple initramfs in the pxelinux configuration file — An easy way to upgrade SliTaz and keep your customizations, example:

  ::

    label slitaz
    kernel /boot/bzImage
    append initrd=/boot/rootfs.gz,/boot/configs/extra-packages.gz,/boot/configs/special-configuration.gz rw root=/dev/null vga=normal autologin

* Example of a PXE server configuration:
  The `SliTaz web boot <http://boot.slitaz.org/>`_ server http://mirror.slitaz.org/pxe/ (start with `pxelinux.cfg/default <http://mirror.slitaz.org/pxe/pxelinux.cfg/default>`_)


Test the PXE server with QEMU
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Install qemu

  .. code-block:: console

     # tazpkg get-install qemu

* Launch the VM

  .. code-block:: console

     # qemu -boot n -bootp /pxelinux.0 -tftp /boot


PXE Client Set-Up
-----------------

Most recent machines with on-board Ethernet have a PXE-capable BIOS.
Look for this feature in BIOS menus and the BIOS boot menu and ensure it is activated.
It may require you press a key, such as :kbd:`F12`, during the boot process.

If your computer does not support PXE booting, you can use SliTaz as a client instead.
Create a bootable CD-ROM or floppy disk with the :guilabel:`Boot Floppy/CDROM` tool found in the :guilabel:`System Tools` menu.

In the :guilabel:`PXE Network` tab click :guilabel:`Write floppy`.
Use this to boot the client computer.

.. tip::
   Is your Ethernet card not recognised?
   See `ROM-O-Matic <http://rom-o-matic.net/>`_


Web Booting
-----------

The SliTaz LiveCD has configuration settings to start your computer via the Internet.
This is useful for using a newer version of SliTaz from older media.

You can start the automatic process with the following command at the SliTaz LiveCD boot-splash::

  web

That's it!

You can find more information on using an Internet connection to boot your computer at the `SliTaz Web Boot home-page <http://boot.slitaz.org/>`_.

You will need a DHCP server to get an IP address, netmask, gateway address, as per a normal network connection — a standard home router should be sufficient for this.


PXE boot without DHCP server: Web Boot & Command Line
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you have no device that can function as a DHCP server, you need an IP address with a netmask, gateway address and, optionally, a DNS address. ::

  title Slitaz Web
    kernel /boot/gpxe ip=192.168.0.12/24 gw=192.168.0.1 dns=192.168.0.1 url=http://mirror.slitaz.org/pxe/pxelinux.0

You can modify the URL thus::

  title Slitaz Web
    kernel /boot/gpxe ip=192.168.0.12/24 gw=192.168.0.1 dns=192.168.0.1 ip=192.168.0.12/24 gw=192.168.0.1 dns=192.168.0.1 url=http://mirror.slitaz.org/pxe/pxelinux.0

Note that only the following keywords are recognised:

* ``ip=``
* ``gw=``
* ``dns=``
* ``url=``
* ``nodhcp`` (useful to avoid a DHCP timeout error)


.. _advanced web booting configuration:

Advanced Web Booting Configuration
----------------------------------

The Web Booting process can be embedded into routers and other devices, as well as being customised.


Embedded Web Boot, with PXE boot PROM (PXE forwarder)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configure a PXE server with http://download.tuxfamily.org/slitaz/boot/gpxe.pxe as the boot file, a 42Kb second stage loader.
This was successfully tested with an `OpenWRT <http://openwrt.org/>`_ router:

* install http://mirror.slitaz.org/boot/mips/tftpd (mips version) in /jffs/usr/sbin
* install gpxe.exe in /jffs/boot
* add dhcp bootfile option in dnsmasq config file

  .. code-block:: console

     # echo "dhcp-boot=gpxe.pxe" >> /tmp/dnsmasq.conf

Launch the tftp server for your lan (say 192.168.0.1/24)

.. code-block:: console

   # /jffs/usr/sbin/tftpd 192.168.0.1 /jffs/boot

.. tip::
   You can avoid the tftp server installation and use the SliTaz tftp server directly:

   .. code-block:: console

      # echo "dhcp-boot=gpxe.pxe,mirror.slitaz.org" >> /tmp/dnsmasq.conf


Modifying the Default GPXE Web Boot URL
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The URL is stored at offset 519 in 255 bytes max.

* Show the current URL with:

  .. code-block:: console

     $ dd bs=1 skip=519 count=255 if=gpxe 2> /dev/null | strings

* Change the URL with:

  .. code-block:: console

     $ echo -n "http://myurl.org/myboot" | cat - /dev/zero | dd conv=notrunc bs=1 seek=519 count=255 of=gpxe

* Change the URL and IP configuration with:

  .. code-block:: console

     $ echo -n "ip=192.168.0.10/24 gw=192.168.0.1 dns=192.168.0.1 url=http://myurl.org/myboot" | cat - /dev/zero | dd conv=notrunc bs=1 seek=519 count=255 of=gpxe

* Remove URL to behave as a normal GPXE with:

  .. code-block:: console

     $ dd if=/dev/zero conv=notrunc bs=1 seek=519 count=255 of=gpxe


Hack the gpxe.pxe Default Web Boot URL
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The URL is stored at offset 5 in 255 bytes max.

Show the current URL with:

.. code-block:: console

   $ dd bs=1 skip=5 count=255 if=gpxe.pxe 2> /dev/null | strings

Change the URL with:

.. code-block:: console

   $ echo -n "http://myurl.org/myboot" | cat - /dev/zero | dd conv=notrunc bs=1 seek=5 count=255 of=gpxe.pxe

Remove the URL and behave as a normal :file:`gpxe.pxe` with:

.. code-block:: console

   $ dd if=/dev/zero conv=notrunc bs=1 seek=5 count=255 of=gpxe.pxe


Using Redundancy with Web Boot Servers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Comma separated URL lists are supported.

The PXE client will try to load the first URL.
If the load fails, it will try the next URL, and so on.

Example with current Slitaz Web boot servers:

.. code-block:: console

   $ echo -n "http://mirror.slitaz.org/pxe/pxelinux.0,http://mirror.switch.ch/ftp/mirror/pxe/pxelinux.0,http://download.tuxfamily.org/slitaz/pxe/pxelinux.0" | cat - /dev/zero | dd conv=notrunc bs=1 seek=519 count=255 of=gpxe


Why use PXE? The VNC example
----------------------------

Let's say that your company is working on some very sensitive data.
You don't want people copying anything on to removable media such as USB keys.
Only a few users can use this data.

* PXELINUX chooses a special configuration by the MAC address in :file:`pxelinux.cfg/{client-mac-address}`
* It checks the md5 (or sha256) password of the user boot entry with menu.c32
* It sends a kernel and an initramfs with a **fbvnc** package built by http://tiny.slitaz.org/ (total size < 1.44MB)
* The client boots in 1 to 5 seconds with a VNC framebuffer client
* The VNC server can send any OS display
* The client has no media driver and can use 20 year old hardware (may avoid theft risk)
* The target OS can run in a VM: more scalable and easier to maintain than multiple desktops
* No data is stored on the client machine.
  It may also have no disk.
  It only needs an ethernet card
* Of course, the sessions in the target OS must have a connection timeout and need a username and a password…


Increase security a bit
^^^^^^^^^^^^^^^^^^^^^^^

The VNC listens to the network without a password (fbvnc has no authentication support) and the VNC traffic is not encrypted on the network.

* Build an initramfs with a **fbvnc-ssh** package on http://tiny.slitaz.org/
* On the server, VNC should listen on localhost only
* The SSH public key of the client is installed in :file:`$HOME/.ssh/authorized_keys` on the VNC server
* The VNC traffic can be compressed in the SSH tunnel (fbvnc supports raw frames only)


A quick demo
^^^^^^^^^^^^

The menu :menuselection:`Tiny SliTaz --> Tiny VNC` of the `SliTaz Web Boot <http://boot.slitaz.org/>`_ launches the VNC client without ssh (you need a VNC server running on your network…).

.. tip::
   You can directly download the `kernel <http://mirror.slitaz.org/pxe/tiny/vnc/bzImage.gz>`_ and then the `initramfs <http://mirror.slitaz.org/pxe/tiny/vnc/rootfs.gz>`_ and test it on your network or with qemu

.. tip::
   Try with the cmdline argument ``vga=ask`` first.
   This will find the best VESA mode to use (example ``vga=0x33B``)
