.. http://doc.slitaz.org/en:guides:wifi-hard
.. en/guides/wifi-hard.txt · Last modified: 2010/08/22 17:10 by linea

.. _wifi hard:

Wi-Fi, step by step
===================

:author: linea, jozee


Introduction
------------

If you want to use your Wi-Fi right away, this page is not for you.
You should look at the :ref:`wifi easy` tutorial which explains how to use the tools given by SliTaz.
But if you want to know how to use Wi-Fi (under Linux), this page will explain how, and help you to configure it *from scratch*.
Before continuing:

* You should know the kernel module needed by your Wi-Fi card.
* If you need any firmware; you should know where to download it.

We are going to:

#. Install any useful software and firmware.
#. Load the kernel module.
#. Configure the Wi-Fi connection.
#. Configure WPA.
#. Get connected and surf.
#. Shutdown Wi-Fi.

.. note::
   On SliTaz, the script :file:`/usr/bin/get-wifi-firmware` takes care of steps 1 and 2, and the script :file:`/etc/init.d/network.sh`; steps 3 to 6.


Install needed software
-----------------------

You'll need the Wi-Fi kernel modules and some software to manage Wi-Fi.
If you are using a WPA key, you'll also need :program:`wpa_supplicant`:

.. code-block:: console

   # tazpkg get-install linux-wireless
   # tazpkg get-install wireless_tools
   # tazpkg get-install wpa_supplicant

If you need firmware:

.. code-block:: console

   # cd /lib/firmware
   # wget http://www.address/of/my/firmware

Untar to install:

.. code-block:: console

   # tar -xvf my_firmware.tar*
   # rm my_firmware.tar*


Load the kernel module
----------------------

.. code-block:: console

   # modprobe -v my_module

If you've got errors, verify that your firmware is where it should be and look at :command:`dmesg`:

.. code-block:: console

   $ ls -l /lib/firmware
   $ dmesg

If you don't have any errors, you can continue.


Configure Wi-Fi interface
-------------------------

Before configuring a new interface, you should de-configure the old one.
If your ethernet interface is configured, you should:

.. code-block:: console

   # ifconfig eth0 down

:command:`iwconfig` allows you to configure your Wi-Fi card, so that it can connect to your access point.
You need to know the name of your Wi-Fi interface (usually :file:`wlan0` or :file:`eth1`).
If you don't know its name, just run :command:`iwconfig`:

.. code-block:: console

   # iwconfig

Now we can configure your Wi-Fi interface and start it:

.. code-block:: console

   # ifconfig WIFI_INTERFACE up
   # iwconfig WIFI_INTERFACE txpower on

Let's test that the card works:

.. code-block:: console

   # iwlist scan

If you've got a list of access points you can now tell your Wi-Fi interface which ESSID to connect to:

.. code-block:: console

   # iwconfig WIFI_INTERFACE essid MY_ESSID 

:command:`iwconfig` can also accept others args, look at its man page to know more.


Configure a WEP or WPA key
--------------------------

You can easily configure a WEP key with :command:`iwconfig`:

.. code-block:: console

   # iwconfig WIFI_INTERFACE key my_wep_key

But you should *always* use a WPA key, because WEP keys can be easily cracked with :program:`aircrack`, as noted `here <http://www.tuto-fr.com/tutoriaux/tutorial-crack-wep-aircrack.php>`_, :program:`wpa_supplicant` allows you to use a WPA key (some cards may use WPA without :program:`wpa_supplicant`).
It needs a config file.
Usually, :file:`/etc/wpa_supplicant.conf`.
If you are using ``wpa_psk`` (normally, you are), add this to the file:

.. code-block:: toml

   ap_scan=1
   network={
   	ssid="my_essid"
   	scan_ssid=1
   	proto=WPA
   	key_mgmt=WPA-PSK
   	psk="my_clear_key"
   	priority=5
   }

Or try:

.. code-block:: toml

   ap_scan=1
   network={
   	ssid="my_essid"
   	scan_ssid=1
   	key_mgmt=WPA-EAP WPA-PSK IEEE8021X NONE
   	group=CCMP TKIP WEP104 WEP40
   	pairwise=CCMP TKIP
   	psk="my_clear_key"
   	priority=5
   }

It's now possible to launch :command:`wpa_supplicant`:

.. code-block:: console

   # wpa_supplicant -B -w -c/etc/wpa_supplicant.conf -DWPA_DRIVER -iWIFI_INTERFACE

``WPA_DRIVER`` is the name of the driver used by :program:`wpa_supplicant`.
Usually, it's ``wext``, but sometimes, another is needed.
Here is a list of possible drivers:

.. list-table::

   * - ``wext``
     - Linux wireless extensions (generic, should work in most cases)
   * - ``hostap``
     - Host AP driver (Intersil Prism2/2.5/3)
   * - ``atmel``
     - ATMEL AT76C5XXx (USB, PCMCIA)
   * - ``wired``
     - :program:`wpa_supplicant` wired Ethernet driver

The option ``-B`` launches :program:`wpa_supplicant` as a daemon.
If you want to kill it:

.. code-block:: console

   # killall wpa_supplicant


Get connected
-------------

If you want to connect in DHCP, just run:

.. code-block:: console

   # /sbin/udhcpc -b -i WIFI_INTERFACE -p /var/run/udhcpc.WIFI_INTERFACE.pid

Normally, you should be surfing!


Turn off Wi-Fi
--------------

To stop Wi-Fi, you should shutdown your Wi-Fi card, and stop the :program:`wpa_supplicant` and UDHCPC daemons:

.. code-block:: console

   # iwconfig WIFI_INTERFACE txpower off
   # kill `cat /var/run/udhcpc.WIFI_INTERFACE.pid`
   # killall wpa_supplicant

You can also unload the kernel module:

.. code-block:: console

   # rmmod my_module
