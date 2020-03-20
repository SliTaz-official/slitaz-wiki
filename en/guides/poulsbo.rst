.. http://doc.slitaz.org/en:guides:poulsbo
.. en/guides/poulsbo.txt · Last modified: 2015/04/04 12:40 by linea

.. _poulsbo:

Poulsbo (aka GMA500)
====================

This is a short guide on getting the Poulsbo graphics working nicely with SliTaz 4.0 or new rolling-core.

Boot SliTaz and establish a working network connection.
After a successful installation you will need to do the following steps to get the graphics working:

.. code-block:: console

   # nano /etc/rcS.conf

Look for the row called ``LOAD_MODULES`` and remove ``poulsbo`` from the list, also make sure that ``psb_gfx`` is present.

Then remove VESA and add the ``psb_gfx`` module (which is inside the :program:`linux-staging` package):

.. code-block:: console

   # tazpkg remove xorg-xf86-video-vesa
   # tazpkg recharge
   # tazpkg get-install linux-staging

The fbdev-driver needs to be installed/updated:

.. code-block:: console

   # tazpkg -gi xorg-xf86-video-fbdev

Simply reboot and the graphics should work nicely with the correct resolution.
