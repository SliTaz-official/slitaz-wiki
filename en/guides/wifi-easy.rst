.. http://doc.slitaz.org/en:guides:wifi-easy
.. en/guides/wifi-easy.txt · Last modified: 2010/08/22 15:27 by linea

.. _wifi easy:

Wi-Fi configuration, easy method
================================


Introduction
------------

To install Wi-Fi under Linux, two methods are possible:

* Use the kernel module specific to your Wi-Fi card
* Use the :program:`Ndiswrapper` module to install a Windows driver


Here, we will discuss the usage of a kernel module specific to your card (:program:`Ndiswrapper` will one day have another page on the wiki).

To use a specific kernel module, you'll need to:

#. Know your Wi-Fi hardware
#. Know which module your card needs
#. Install this module, and possibly firmware too
#. Configure your connection to the access point
#. Get connected and surf

:program:`Netbox` is a tool which can do that just by clicking a few buttons.
But it's also possible to configure your Wi-Fi from the command line.


:program:`Wifibox`\ /\ :program:`Netbox` Graphical Utility
----------------------------------------------------------

With :program:`netbox` or :program:`wifibox` you can configure your Wi-Fi with only a few mouse clicks.
As you already know which module you need, it's easy to use :program:`wifibox`:

.. code-block:: console

   $ subox wifibox

:program:`Wifibox` will install all the software needed and then launch the Wi-Fi.
You won't have to manually install your Wi-Fi.

Here are the necessary steps for :program:`Wifibox`:

* First, go to the :guilabel:`Drivers` tab, and install the module you need.
  SliTaz will load the firmware (if needed), the kernel module, configure your Wi-Fi, and connect to the access point.
* Then, go to :guilabel:`Configuration`, and start to configure.
  If you don't know what to add, have a look at the sample :file:`/etc/network.conf` — :program:`wifibox` uses the same parameters.
  Click on :guilabel:`Start`
* If all goes well, you may start surfing!


Manually Configure your Wi-Fi
-----------------------------

If you want to understand how Wi-Fi works in SliTaz, then you can try configuring your Wi-Fi manually.

But if you want to know how Wi-Fi generally works under Linux, or if your kernel module is not on this list, you should look at the :ref:`wifi hard`.

Here is a quick summary of the steps needed:

* Know your Wi-Fi card
* Check and install if your Wi-Fi card needs any firmware, e.g., ``b43``
* Load the kernel module specific to your Wi-Fi card
* Check that your Wi-Fi card is detected and your module is loaded
* Configure :file:`/etc/network.conf`
* Load up your ``WIFI`` interface
* Start :file:`/etc/init.d/network.sh`

The following commands do all of the above steps.
This is also the easiest way:

.. code-block:: console

   # tazhw detect-pci --get-firmware
   # /etc/init.d/network.sh restart

Now for more detailed instructions:


Detailed Instructions
---------------------


Identifying your hardware (Which Wi-Fi card do I have?)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can list your hardware using the terminal.
It's useful to know which Wi-Fi card you have.

If you have an integrated Wi-Fi card:

.. code-block:: console

   $ lspci | grep -i network

If your card is an USB one, you'll need :command:`lsusb` which is available in the package :program:`usbutils`:

.. code-block:: console

   # tazpkg get-install usbutils
   $ lsusb

In either case, you'll see something like this::

  02:02.0 Network controller: Intel Corporation PRO/Wireless LAN 2100 3B Mini PCI Adapter (rev 04)

This tells us the following:

* ``Intel Corporation`` made the card
* The card is a ``Pro/Wireless Lan 2100 3B``
* The chipset is a ``IPW 2100 (Intel Pro Wireless 2100)``
* The interface is a ``Mini PCI``


Which module do I need, and do I need firmware?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The SliTaz Linux kernel is made to be light.
Some software modules, especially those needed by Wi-Fi cards are not installed by default, but have to be loaded by the user.

Many Wi-Fi cards will work if you load the correct module.
The easiest way is to let SliTaz auto-detect your hardware.

For integrated Wi-Fi cards:

.. code-block:: console

   # tazhw detect-pci

For USB cards:

.. code-block:: console

   # tazhw detect-usb

Sometimes, however, the module alone is not enough.
Some types of card (Intel for example) also need firmware.
Such firmware is not free software, so we can't distribute it as part of SliTaz.
You may need to obtain the firmware from the website of your card manufacturer and download it to :file:`/lib/firmware`.
But you'll see that SliTaz can sometimes do this for you!


For ``b43``, ``b43legacy``, ``ipw2100`` or ``ipw2200`` kernel modules
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If the kernel module you need is either ``b43``, ``b43legacy``, ``ipw2100`` or ``ipw2200``, you also need to install the package: :program:`get-wifi-firmware`.
You can install it like this:

.. code-block:: console

   # tazpkg get-install get-wifi-firmware


Launch Wi-Fi
^^^^^^^^^^^^

:program:`get-wifi-firmware` will install some commands named :file:`get-{a module}-firmware`.
To see the list:

.. code-block:: console

   # ls /usr/bin/get-*-firmware

Launch the software that corresponds to your module (``ipw2100`` in the earlier example):

.. code-block:: console

   # get-my_module-firmware

For example, say your module is ``ipw2200``, you can type:

.. code-block:: console

   # get-ipw2200-firmware

This command will:

#. Get the needed firmware for "*my_module*", make the package :file:`{my_module}-firmware`, and install it.
#. Get useful software for Wi-Fi support (:command:`iwconfig`, :command:`wpa_supplicant` if needed…)
#. Load the module "*my_module*" into the Linux kernel.
#. Launch :command:`/etc/init.d/network.sh restart`, which starts Wi-Fi.

If :file:`/etc/network.conf` is correctly configured, you can surf!

The easiest way is to do this is to download the firmware graphically using :program:`Wifibox` (SliTaz Wireless Manager).
Yes, SliTaz graphical wireless manager (:program:`wifibox`) can do these steps on the :guilabel:`Driver` tab (after selecting the correct module, say ``ipw2200`` and pressing :guilabel:`install`).


Troubleshoot your Wi-Fi
-----------------------

Here is a quick summary of the steps needed (Repeated again for understanding):

* Know your Wi-Fi card
* Check and install if your Wi-Fi card needs any firmware, e.g., ``b43``
* Load the kernel module specific to your Wi-Fi card
* Check that your Wi-Fi card is detected and your module is loaded
* Configure :file:`/etc/network.conf`
* Load up your ``WIFI`` interface
* Start :file:`/etc/init.d/network.sh`

The following commands do all of the above steps.
This is also the best way to troubleshoot.
To get maximum help on the forums, please post the output of each of these commands.

.. code-block:: shell

   lspci | grep -i network
   modprobe your_module
   dmesg | tail
   lsmod
   nano /etc/network.conf
   ifconfig eth1 up
   ifconfig -a
   iwconfig
   /etc/init.d/network.sh restart
   ifconfig -a


Configure :file:`/etc/network.conf`
-----------------------------------

SliTaz launches Wi-Fi using the script :file:`/etc/init.d/network.sh`.
This script uses the config file :file:`/etc/network.conf`.
You should first edit your config file using the instructions in the sample :file:`/etc/network.conf`.
You'll find more information about :file:`/etc/init.d/network.sh` in :ref:`network script`

Now your module must be loaded at each boot.
To automate, add "*my_module*" to the line ``LOAD_MODULES`` in :file:`/etc/rcS.conf`:

.. code-block:: console

   # geany /etc/rcS.conf

.. code-block:: shell

   LOAD_MODULES="nls_utf8 my_module"


Sample :file:`/etc/network.conf` file with comments
---------------------------------------------------

.. code-block:: shell

   # Start Wi-Fi on boot: "yes" or "no".
   WIFI="yes"
   
   # Wi-Fi interface (usually "wlan0" or "eth0").
   WIFI_INTERFACE="wlan0"
   
   
   # ESSID of access point: "my_essid" or "any".
   # If "any" is chosen, SliTaz will try to connect to the first access point.
   # Be careful:
   # In some states (in France for example), you are not allowed to connect
   # to a private access point if it is not secured.
   # WIFI_ESSID="any"
   
   
   # Type of connection:
   # You can chose between:
   # * ad-hoc    : Connect to one cell without access point.
   # * managed   : One or more cell, with access point.
   # * master    : Your card is a master card.
   # * repeater  : Your card acts as a repeater.
   #               Useful for long distances.
   # * secondary : Your card is a backup for master or repeater
   # * monitor   : Your card only receives messages.
   # For most of the time, you'll use "managed".
   # (Home, cyber-coffe, work, university...)
   WIFI_MODE="managed"
   
   
   # Key
   WIFI_KEY="ma8clef8de8chiffrement8difficilement8déchiffrable"
   
   
   # Key type "wep" or "wpa" or "any" or "none"
   # If you're using WPA-EAP (at work for example), try "any".
   WIFI_KEY_TYPE="wpa"
   
   
   # Driver needed by wpa_supplicant.
   # It depends on your kernel module.
   # The possible drivers are:
   # * wext   : Linux wireless extensions (in most cases, use this one.)
   # * hostap : Host AP driver (Intersil Prism2/2.5/3)
   # * atmel  : ATMEL AT76C5XXx (USB, PCMCIA)
   # * wired  : wpa_supplicant wired Ethernet driver
   WPA_DRIVER="wext"
   
   
   # Wi-Fi channel. Leave it blank, if you don't know what it is.
   WIFI_CHANNEL=""
   
   
   # More args to pass to iwconfig.
   # Look at iwconfig man page for more information.
   WIFI_IWCONFIG_ARGS=""


Extra Information
-----------------

Now for more detailed instructions:

First do :command:`lspci` to know which card you have.
Once we know which card you have, we can surf the internet to find which module you require, and if we need any special firmware.
To help you, here are some useful links:

* `Linux wireless LAN support <http://linux-wless.passys.nl/>`_
  (The column on the right, tells you where to get the firmware, if applicable)
* `Ubuntu documentation <http://help.ubuntu.com/>`_
  (Good Ubuntu website, Wi-Fi page)
* `Google <http://www.google.com/>`_ with "*the name of your Wi-Fi card*" + "*modprobe*" or "*linux*"

If these links are not useful, you can ask at the `forum <http://forum.slitaz.org/>`_

So we learn that the ``Intel Corporation PRO/Wireless LAN 2100 3B Mini PCI Adapter`` works with the ``IPW2100`` module and the firmware is available at http://ipw2100.sourceforge.net/.
