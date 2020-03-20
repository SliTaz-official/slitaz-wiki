.. http://doc.slitaz.org/en:guides:cricket-a600
.. en/guides/cricket-a600.txt · Last modified: 2010/08/22 17:31 by linea

.. _cricket a600:

Setting Up The Cricket A600 Broadband Modem
===========================================


Introduction
------------

First thing you need to do is download :program:`wvdial` and all it's dependencies.
It will do that automatically.
From terminal as root:

.. code-block:: console

   # tazpkg get-install wvdial

Download this: `Jozee's USB_Modeswitch <http://people.slitaz.org/~jozee/packages/3G_modem/usb_modeswitch-1.0.5.tazpkg>`_.
Then from terminal as root install it:

.. code-block:: console

   # tazpkg install /path/to/usb_modeswitch-1.0.5.tazpkg

:file:`/path/to` is where you downloaded it to.
So you will need to adjust the above.

Make a script called :file:`flipflop.sh`.
You can place it anywhere but I am going to use where I have it as an example: :file:`/home/{tux}/scripts/`.

If you use another name besides "tux" then adjust it accordingly.
Now open up :file:`flipflop.sh` with your prefered editor.
Put the following in it and save the file:

.. code-block:: shell

   /usr/sbin/usb_modeswitch
   sleep 15
   usb_modeswitch -v 0x1f28 -p 0x0020 -R 1

From terminal we need to :command:`chmod` it so you can use the script:

.. code-block:: console

   # chmod a+x /home/tux/scripts/flipflop.sh

Again if you have it saved somewhere else then adjust accordingly.
Sorry to repeat that LoL.


:file:`wvdial.conf`
-------------------

We have now installed all that we need to install.
There are just a few more steps involved.
We need to edit :file:`wvdial.conf`.
As root from terminal enter this:

.. code-block:: console

   # leafpad /etc/wvdial.conf

In :file:`wvdial.conf` we need to make it look like this:

.. code-block:: ini

   [Dialer Defaults]
   Modem = /dev/ttyACM0
   Baud = 460800
   Stupid Mode = 1
   Auto DNS = 1
   Init1 = ATZ
   Init2 = ATQ0 V1 E1 S0=0 &C1 &D2 +FCLASS=0
   ISDN = 0
   Modem Type = USB Modem
   Phone = #777
   FlowControl = Hardware (CRTSCTS)
   Dial Command = ATDT
   Username = Your Cricket Number Goes Here.
   Password = Cricket

Where it says "Your Cricket Number Goes Here" replace that with your cricket number.
Your cricket number should be on your monthly bill if you don't know what it is.
Once it is done save the file.

Now again as root in terminal issue this command:

.. code-block:: console

   # /home/tux/scripts/flipflop.sh

You should get something like this below:

.. code-block:: console

   Looking for target devices ...
    No devices in target mode or class found
   Looking for default devices ...
    Found default devices (1)
    Found a default device NOT in target class mode
   Accessing device 003 on bus 002 ...
   Using endpoints 0x08 (out) and 0x87 (in)
   Inquiring device details; driver will be detached ...
   Looking for active driver ...
    OK, driver found ("dummy")
    OK, driver "dummy" detached
   
   Received inquiry data (detailed identification)
   -------------------------
     Vendor String: Cricket 
      Model String: T-Flash Disk    
   Revision String: 2.31
   -------------------------
   
   Device description data (identification)
   -------------------------
   Manufacturer: Cal-comp E&CC Limited 
        Product: USB Micro SD Storage
     Serial No.: XXXXXXXXXXXXXXX
   -------------------------
   Setting up communication with interface 0 ...
   Trying to send the message to endpoint 0x08 ...
    OK, message successfully sent
    Device is gone, skipping any further commands
   -> Run lsusb to note any changes. Bye.
   
   
   Looking for target devices ...
    No devices in target mode or class found
   Looking for default devices ...
    No default device found. Is it connected? Bye.
   
   tux@slitaz:~$

It looks like it totally failed but it didn't.
It just doesn't know what the driver is.
Again from root execute this command:

.. code-block:: console

   # modprobe cdc_acm

That should be the last time you have to enter that command.
We will now add it to to the ``LOAD_MODULES`` line in :file:`/etc/rcS.conf` to load at boot.


:file:`rcS.conf`
----------------

From terminal as root:

.. code-block:: console

   # leafpad /etc/rcS.conf

Go down to the following line:

.. code-block:: shell

   LOAD_MODULES="snd_intel8x0 yenta_socket rtc agpgart intel-agp"

Add this to the line: "cdc_acm".
It should now look like this:

.. code-block:: shell

   LOAD_MODULES="snd_intel8x0 yenta_socket rtc cdc_acm agpgart intel-agp"

Save it and exit.

We are almost done.
From terminal as root again give this command:

.. code-block:: console

   # wvdial

It should now dial and connect.
Do not close the terminal or it will disconnect.
Everytime you poweroff or unplug the modem you have to do the :file:`flipflop.sh` like I showed you above.
If you just reboot your computer you don't have to do the :file:`flipflop.sh`.
Good Luck!
