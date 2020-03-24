.. http://doc.slitaz.org/en:guides:network-script
.. en/guides/network-script.txt · Last modified: 2014/11/15 21:00 by linea

.. _network script:

Secrets of :file:`/etc/init.d/network.sh`
=========================================

:author: pankso, linea, mojo


Introduction
------------

SliTaz launches the :file:`/etc/init.d/network.sh` at startup to initialize the network.
It configures the hostname, loopback interface, and internet connection.

It's also possible to call the script when SliTaz is started, and use it to open or close internet connections.
For example netbox and any software linking :program:`get-wifi-firmware` (:program:`get-ipw2100-firmware`, :program:`get-b43-firmware`…) use it.


Usage
-----

By default, :file:`/etc/init.d/network.sh` uses :file:`/etc/network.conf` as the conf file.
Parameters written in that file are made for the default network connection.

To start the default connection, as when booting:

.. code-block:: console

   # /etc/init.d/network.sh start

The ``start`` arg should be used only at boot.
To stop the connection:

.. code-block:: console

   # /etc/init.d/network.sh stop

To stop and restart:

.. code-block:: console

   # /etc/init.d/network.sh restart

But, most interestingly, is that :file:`/etc/init.d/network.sh` may also use another config file.
It's useful if you're using a laptop, as you can configure multiple connections for multiple access points.

For example, we can create a directory :file:`/etc/network`, containing some config files, named:

* :file:`Home`, for home, using an ethernet connection and a static IP.
* :file:`Desktop`, for the desktop, with a WEP encryption, and a static IP.
* :file:`Univ`, Wi-Fi without encryption, and with DHCP.

Now to get connected at :file:`Home`, later at a :file:`Desktop` and finally at :file:`Univ`, before stopping connection, we only have to:

.. code-block:: console

   # /etc/init.d/network.sh restart /etc/network/Home
   # /etc/init.d/network.sh restart /etc/network/Desktop
   # /etc/init.d/network.sh restart /etc/network/Univ
   # /etc/init.d/network.sh stop


:program:`sudo`
---------------

Since :file:`/etc/init.d/network.sh` can only be used by root, if you want a normal user to use it, you should install :program:`sudo`:

.. code-block:: console

   # tazpkg get-install sudo

And then configure it:

.. code-block:: console

   # visudo

For user ``tux``, which must use :file:`network.sh` from every host; without a password you should add::

  tux  ALL=NOPASSWD: /etc/init.d/network.sh,

For user ``tortux``, which may only get connected from localhost, and which should use a password each time, you should add::

  tortux my_hostname=PASSWD: /etc/init.d/network.sh,

If you forgot your hostname, just run:

.. code-block:: console

   $ cat /etc/hostname

Here is some help to use :program:`visudo`:

* ``i`` insertion mode (to write).
* ``Escape`` exit insertion mode.
* ``:wq`` record and quit.
* ``:q!`` quit without recording.


Openbox
-------

All this is not that really user friendly…

That's why I'll give you a perfect treat: A way to integrate all this in an Openbox menu!
Create a script :file:`/usr/lib/openbox/network-menu.sh`, and add this to it:

.. code-block:: shell

   #!/bin/sh
   #
   # Openbox pipe menu to start network connections
   # (This script is only useful if sudo is installed, and correctly configured)
   
   echo '<openbox_pipe_menu>'
   
   # For default file:
   echo '<item label="Load Default network.conf">'
   echo -n '<action name="Execute"><execute>'
   echo -n "sudo /etc/init.d/network.sh restart"
   echo '</execute></action>'
   echo '</item>'
   
   # For other configuration files (you may state a different directory here
   # depending on your setup):
   # e.g nice for frugal installs:
   # ls /home/tux/network/ | while read; do
   ls /etc/network/ | while read; do
   echo '<item label="'"${REPLY}"'">'
   echo -n '<action name="Execute"><execute>'
   echo -n "sudo /etc/init.d/network.sh restart '/etc/network/${REPLY}'"
   echo '</execute></action>'
   echo '</item>'
   done
   
   # To stop connections:
   echo '<item label="stop Connection">'
   echo -n '<action name="Execute"><execute>'
   echo -n "sudo /etc/init.d/network.sh stop"
   echo '</execute></action>'
   echo '</item>'
   
   echo '</openbox_pipe_menu>' 

Make it executable:

.. code-block:: console

   # chmod +x /usr/lib/openbox/network-menu.sh

And now you only have to add these lines in :file:`~/.config/openbox/menu.xml`:

.. code-block:: xml

   <menu id="network-menu" label="Network" 
   	execute="/usr/lib/openbox/network-menu.sh" />

Then reconfigure Openbox:

.. code-block:: console

   $ openbox --reconfigure

Enjoy!
