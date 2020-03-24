.. http://doc.slitaz.org/en:cookbook:reltasks
.. en/cookbook/reltasks.txt · Last modified: 2014/05/02 17:19 by pankso

.. _cookbook reltasks:

Release tasks
=============

:author: pankso, linea, erjo, mojo

Things to do before publishing a new version.

.. note::
   20140502 <pankso> 5.0 is out RC.
   The current things to do are:

   * Write the :ref:`relnotes` in English here on the Wiki, then static xHTML for translation on the list and commits in :program:`slitaz-doc`
   * Update docs in this wiki.
     The Handbook must be hugely improved or removed since people use the guides section (ex liveUSB has 2 pages…)


Cooking & RC
------------

#. Make sure all repos are tagged to include latest changes and are updated into the wok.
   Release source tarballs and push them to mirror with :program:`tazdev`

   .. code-block:: console

      # hg tag 4.4 && hg push
      # tazdev relpkg slitaz-tools 4.4

#. Make sure all packages are built on Tank, generate a new package list with cook.

   .. code-block:: console

      # cooker
      # cook pkgdb --flavors

#. Sync the mirror with newly built packages.

   .. code-block:: console

      # tazdev -p username

#. Use a chroot and :program:`tazdev` (or :command:`conspy -f 4` on Tank) to build ISOs
#. IMPORTANT: Use the packages on mirror.slitaz.org to build the ISOs:

   .. code-block:: console

      # rm -rf /home/slitaz/packages && tazpkg -cc

#. If the flavors repo has been modified don't forget to update it in the chroot
#. Build a core with :program:`Tazlito`:

   .. code-block:: console

      # tazlito pack-flavor core
      # tazlito get-flavor core
      # tazlito gen-distro

#. Boot this ISO with :program:`Qemu`, burn it to a CD and boot, make a live USB and boot to make sure everything works.
#. When on the desktop, try ALL desktop entries and a few commands or latest code.
#. Connect to the web and install a few packages to test them
#. IMPORTANT: Install to HD and reboot, users should got an X environment without any other modifications.
   Installation and upgrade is really important for 4.0 since we have a new installer.
#. Prepare website and Distrowatch announcement and send it on the list.
#. Build all flavor ISOs and upload them to the mirror
#. Commit website news and update website — Post on SCN and Twitter
#. Open a new thread on the forum for feedback and share the URL


Stable
------

#. After some Cooking and a few RCs
#. Release version is specified by :file:`/etc/slitaz-release`, this file is controlled by :program:`slitaz-base-files`.
   Base files are tagged just before a stable release to change the version string and let :program:`Tazpkg` use the new packages.
#. Check that the SliTaz version specified in :file:`/etc/issue` message is the same as displayed in :file:`isolinux.cfg` for :program:`Vesa` menu, here the :program:`syslinux` package must be modified.
#. When all other repos are tagged like for a *cooking*, update all packages, we can tag the main wok.
#. Remove the current stable wok and copy the cooking wok in place.
#. Sync Tank with mirror
#. Go on mirror and copy all cooking packages to the stable string.
#. Remove stable string in cooking wok and slitaz-base-file repo (update it again) so it goes back to a normal state and devs can start commiting.
   Toolchain updates may be discussed at this moment.
#. Build ISOs from a stable chroot or running system and use packages on the main mirror as usual.
#. Test, test, and test again…
#. Prepare the website announcement and RSS feeds.
   The Mailing list is used for translation and any text should be submitted 1 or 2 days before release.
#. Upload ISOs to mirror, commits news and update the website
#. Spread the word and go to sleep…


Stable documentation
--------------------

SliTaz stable release provides the release notes on the LiveCD through the package :program:`slitaz-doc`, the repos are tagged just before release and are archived on the mirror.
After the wok has been copied to wok-stable, the docs are back to a cooking cycle and just provide an index with basic information.
On the system, docs are located in :file:`/usr/share/doc/slitaz`, a desktop file and icon are provided in the sources package and can be used to have quick access to the documentation.

* `SliTaz Doc repo <http://hg.slitaz.org/slitaz-doc/>`_
