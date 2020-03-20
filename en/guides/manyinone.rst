.. http://doc.slitaz.org/en:guides:manyinone
.. en/guides/manyinone.txt · Last modified: 2018/01/14 10:27 by bellard

.. _manyinone:

Create many-in-1 flavors
========================

Slitaz LiveCDs run fully in RAM.
The feature set depends on the available RAM size.
You can build an auto adaptive LiveCD to ensure that the right feature set is launched according to the detected RAM size.
You need to:

* define a flavor list (or an ISO list) where each flavor is nested in the previous one (like russian dolls),
* define the minimum RAM quantity necessary for each flavor.

The many-in-1 ISO may be a little bit larger than the largest flavor.
Sometimes the sizes are identical.

We will build the official slitaz-3in1 flavor which boots:

* the *core* flavor with 160MB or more
* the *justx* flavor with 96MB to 160MB
* the *base* flavor with 32MB to 96MB
* and displays an error message with less than 32MB…

The ISO images :file:`slitaz-3.0-3in1.iso` and the largest flavor :file:`slitaz-3.0.iso` share the same size: 31457280 bytes.
(caused by the 1MB padding: it would be 123351 bytes larger otherwise).


The best way: create a flavor
-----------------------------

A flavor holds all the necessary information to build a LiveCD.
The `build bot`_ will automagically keep the flavor file (:file:`.flavor`) up to date and this tiny file will also be used by :program:`tazlito` to (re)generate the ISO image.

`flavors <http://hg.slitaz.org/flavors>`_ are like software sources for the flavor file: the `build bot`_ is a make tool and :command:`tazlito pack-flavor` the compiler.
Providing flavors is like providing source files and providing ISOs is like providing executables only.

A meta flavor has no rootfs, rootcd or packages.list.
The ``ROOTFS_SELECTION`` in the receipt (see `<http://hg.slitaz.org/flavors/file/324757d594ef/core-3in1>`_) gives the nested flavors according to the ram sizes from the largest to the smallest. ::

  ROOTFS_SELECTION="160M core 96M justx 32M base"

The rootfs and the rootcd are taken from the largest flavor.


The easy way: using tazlitobox
------------------------------

Launch :program:`tazlitobox`, select tab :guilabel:`meta`.
Enter the ISO file in :guilabel:`ISO input` and add the according RAM size in :guilabel:`RAM needed` for each flavor with the :guilabel:`+` button.
Fill the :guilabel:`ISO output` name and then click :guilabel:`Build ISO`.

.. note::
   In recent SliTaz versions :program:`tazlitobox` has moved into :program:`tazpanel` in the menu :menuselection:`boot --> live --> Build a meta ISO`


The cmdline way: using tazlito
------------------------------

Usage: :command:`tazlito merge size1 iso size2 rootfs2 [sizeN rootfsN]...`

The largest flavor is given as an ISO file to get the additional rootcd files and will get the resulting ISO. ::

  tazlito merge 160M slitaz-core.iso 96M rootfs-justx.gz 32M rootfs-base.gz


Is it so useful?
----------------

OK we can boot the live CD with less RAM but nowadays the computers have a lot of RAM.
And booting in text mode is not so sexy…

* booting **live** a (very) old machine with less RAM may be in text mode.
  But the same PC can have a graphic desktop if SliTaz is installed on hard disk.
  The SliTaz *base* flavor can install the distribution.
  Without the russian dolls trick there is no way to use these machines with the default SliTaz cdrom.
* some people maybe don't like the default application set.
  They can boot a simpler flavor live (say *justx*), add their preferred packages, edit the config files and save the result with :command:`tazlito writeiso`

.. note::
   You can do something similar online with `<http://pizza.slitaz.me/>`_.


.. _build bot: http://bb.slitaz.org/
