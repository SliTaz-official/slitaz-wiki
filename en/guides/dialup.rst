.. http://doc.slitaz.org/en:guides:dialup
.. en/guides/dialup.txt · Last modified: 2010/08/22 17:21 by linea

.. _dialup:

3G-UMTS
=======

:author: kultex, linea, jozee

The ``#`` sign means that you do this command as root on your console:


Automatic Installation
----------------------

The new :program:`hwsetup` in SliTaz 3.0 now adds automatic installation for 3G (still in trial version).

.. code-block:: console

   # tazhw setup 3g-modem

Now open the :program:`wvdial` box by typing :kbd:`y`, enter your data (as described in :file:`wvdial.conf`), save the configuration (if you don't save you risk breaking your *pin*), dial the *pin* (you have to do this only once you plug in your modem), then start dialing and you should be connected.


Manual Installation
-------------------

.. code-block:: console

   # tazpkg get-install wvdial
   # tazpkg get-install linux-dialup 

You have to bring down your network, otherwise you will have a route problem:

.. code-block:: console

   # ifconfig eth0 down
   # ifconfig eth1 down

You have to switch on the modem with:

.. code-block:: console

   # modprobe option  # (Huawei E220, E160g, Venus-VT12)

Please help update which modem or phone is using which module using the forum post: http://labs.slitaz.org/issues/show/149.
You will find if your modem is supported by :program:`linux-dialup`.

There is an extra guide for the :ref:`Cricket-a600 <cricket a600>`

For automatization load the module at startup with your SliTaz Control Box.


:file:`wvdial.conf`
-------------------

Check the modem if it is :file:`/dev/ttyUSB*` or :file:`/dev/ttyACM*` (e.g. with :command:`# dmesg`) and edit your :file:`wvdial.conf` (here as an example):

.. code-block:: console

   # leafpad /etc/wvdial.conf

.. code-block:: ini

   [Dialer pin]
   Modem = /dev/ttyUSB0
   Init1 = AT+CPIN=1234                     # here's your pin
   
   [Dialer umts]
   Modem = /dev/ttyUSB0
   ISDN = off
   Modem Type = USB Modem
   Baud = 460800
   Init = ATZ
   Init2 = ATQ0 V1 E1 S0=0 &C1 &D2 +FCLASS=0
   Dial Prefix =
   Dial Attempts = 1
   Dial Command = ATM1L3DT
   Ask Password = off
   Auto Reconnect = off
   Abort on Busy = off
   Carrier Check = on
   Check Def Route = on
   Abort on No Dialtone = on
   Stupid Mode = off
   Idle Seconds = 0
   Init3 = AT+CGDCONT=1,"IP","drei.at"      # here's your string
   Username = drei.at                       # here's your Username
   Password = drei.at                       # here's your Password
   Phone = *99#                             # here's your Phone

All the needed data can be found here:

* `Austria, Germany, Switzerland <http://linux.frankenberger.at/Huawei_E220_Daten.html>`_
* `World <http://www.flexispy.com/Mobile%20APN%20Setting%20to%20use%20GPRS.htm>`_

But you can search the net with the name of your provider and :file:`wvdial.conf` and I think you will need to.

To connect do:

.. code-block:: console

   # wvdial pin
   # wvdial umts

You can put the *pin* also in ``Dialer umts``, but I use my modem most of the time on the train, and you would get an error when you reconnect (when the connection breaks) — so use :program:`wvdial` *pin* only the first time you plug in the modem.

The only thing thats left is to put the DNS in :file:`resolv.conf` (you will find the DNS in the two lists with all the data).

.. code-block:: console

   # leafpad /etc/resolv.conf

.. code-block:: ini

   nameserver 213.94.78.16                  # here's your DNS

If you want to dial out as user — add yourself to the group "dialout" (:command:`cat /etc/group`) and set the permissions for :file:`wvdial` and :file:`wvdial.conf`.

For automatization you can put a script in :file:`/usr/local/bin` called :file:`umts-connect` (executable)!
From here it's not proved, because I connect from the console, but it should work like this!

.. code-block:: shell

   #! /bin/bash
   ifconfig eth0 down
   ifconfig eth1 down
   wvdial pin
   wvdial umts

But don't reconnect with this.


Desktop entry
-------------

You can also put a desktop entry in :file:`/usr/share/applications` with the name :file:`umts.desktop`

.. code-block:: ini

   [Desktop Entry]
   Encoding=UTF8
   Name=UMTS connection
   Name[de]=UMTS-Verbindung
   Comment=UMTS-Verbindung
   Type=Application
   Exec=/usr/local/bin/umts-connect
   Icon=/usr/share/icons/...         # whatever you want
   Categories=Application;Network;

Please check ``Exec=/usr/local/bin/umts-connect`` for permissions.

Here is the `forum post <http://forum.slitaz.org/index.php/discussion/comment/440/#Comment_440>`_ about this.
