.. http://doc.slitaz.org/en:releases:4.0:errata
.. en/releases/4.0/errata.txt · Last modified: 2012/08/20 22:48 by seacat

.. _slitaz 4 errata:

SliTaz 4.0 Errata
=================

* Xorg fails to start at the end of boot.
  Solution: Preselect your language at boot menu or from the command line with: ``slitaz lang= kmap=``
* Upgrade from 3.0 to 4.0 can be a pain from the Package manager.
  Solution: Use :program:`SliTaz Installer` from the command line or :program:`TazPanel` to perform an upgrade.
* Desktop bottom panel is missing.
  This is a bug from :program:`LXpanel`, only the main panel is copied to user config.
  Solution:

  .. code-block:: console

     $ cp /etc/lxpanel/slitaz/panels/* $HOME/.config/lxpanel/slitaz/panels

* tazpanel liveCD loram conversion does not work.
  Solution 1: use tazlito, example:

  .. code-block:: console

     # tazlito build-loram slitaz-4.0.iso slitaz-4.0-loram.iso ram

  Solution 2: stop tazpanel server with the :command:`kill` command and start it in an xterm

  .. code-block:: console

     # /etc/init.d/tazpanel start

* :command:`tazlito build-loram` does not update the :file:`/mdsum` file.
  The ``media-check`` option in boot menu reports unexpected broken files.
  slitaz-4.0-loram-cdrom media-check reports "177 files OK, 29 broken, 4488 not checked."
  Because :file:`isolinux.bin` does not support *rockridge* and does not see most files.
* 'tazlito build-loram some.iso result.iso cdrom' does not work.
  Solution: use this fixed version `tazlito <http://hg.slitaz.org/tazlito/raw-file/ad96fdd80b46/tazlito>`_
* bootfloppybox does not retrieve the boot cmdline of :file:`slitaz-4.0.iso`.
  Solution: use this fixed version `bootfloppybox <http://hg.slitaz.org/slitaz-tools/raw-file/fb636ac549ab/tinyutils/bootfloppybox>`_
* The ISO images :file:`slitaz-4.0-base.iso`, :file:`slitaz-4.0-core.iso`, :file:`slitaz-4.0-firefox.iso`, :file:`slitaz-4.0.iso`, :file:`slitaz-4.0-justx.iso`, :file:`slitaz-4.0-preinit.iso`, :file:`slitaz-4.0-proxy.iso` and :file:`slitaz-4.0-xorg-light.iso` are not hybrid.
  Solution 1: run :command:`isohybrid` on the image, example:

  .. code-block:: console

     $ isohybrid slitaz-4.0.iso

  Solution 2: use :file:`slitaz-4.0-loram-cdrom.iso` or :file:`slitaz-4.0-loram.iso`.
* The installer in :program:`tazpanel` does not find :program:`tazinst` if you use the ``pt_BR`` locale.
  Solution: this bug has been fixed, update your system with the latest :program:`tazpanel` and :program:`slitaz-tools` packages.
