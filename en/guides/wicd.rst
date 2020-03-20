.. http://doc.slitaz.org/en:guides:wicd
.. en/guides/wicd.txt · Last modified: 2010/08/21 19:55 by linea

.. _wicd:

WICD
====

:ref:`Pankso's Openbox treat <network script>` is very close to :program:`WICD`, but if you want to set up SliTaz for not very experienced users it's recommended that you use :program:`WICD` for Wireless Internet Control.

Do as root:

.. code-block:: console

   # tazpkg get-install wicd
   # leafpad /etc/rcS.conf

In :file:`/etc/rcS.conf` remove from the section ``RUN_SCRIPTS``: ``network.sh`` and add to the section ``RUN_DAEMONS``: ``wicd``

Then as user:

.. code-block:: console

   $ leafpad .config/openbox/autostart.sh

And add to the end:

.. code-block:: shell

   # wicd Network Configuration
   wicd-client &

Then remove the network-plugin from the LX Panel by right clicking on the network-plugin.

Reboot — don't test before rebooting, because :file:`network.sh` is interfering with :program:`WICD` (even if you stop it by typing :command:`/etc/int.d/network.sh stop`).
