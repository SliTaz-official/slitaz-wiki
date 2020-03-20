.. http://doc.slitaz.org/en:cookbook:buildhost
.. en/cookbook/buildhost.txt · Last modified: 2017/04/25 19:32 by linea

.. _cookbook buildhost:

SliTaz Build Host (tank)
========================

SliTaz build host info and howto.


Folders in :file:`/home/slitaz`
-------------------------------

* :file:`cooking/` — Cooking chroot and flavors.
* :file:`stable/` — Stable tree.
* :file:`undigest/` — Undigest tree.
* :file:`repos/` — All the project repos (where the commits are pushed).
* :file:`www/` — Virtual hosts (``boot``, ``people``, etc).


Using :command:`tazdev`
-----------------------

To help maintain the mirror, flavors and other services, the :command:`tazdev` command is used.
It is mostly configured for the cooking version.
Usage:

.. code-block:: console

   $ tazdev usage


Cooking official packages
-------------------------

Maintainers have root access and some have write access to the main mirror at mirror.slitaz.org, if you want to help in this task please contact one of the active developers (check hg repos).

Everything is cooked in a chroot environment, the default path for the build wok is :file:`/home/slitaz/cooking/chroot`.
No changes must be done in the chroot and don't install any packages to keep it clean.
We run the cooking chroot under ``tty4``:

.. code-block:: console

   # conspy -f 4

.. tip::
   Don't forget to update the chroot with :command:`tazpkg up -r`

In the event of a reboot or trouble you may be prompted to a login, login as root then use :command:`tazdev chroot` to chroot into the build env.
To cook all the last committed packages or cook a single package:

.. code-block:: console

   #/ cooker
   #/ cooker pkg pkgname

To get out of conspy press :kbd:`Esc` 3:sup:`×` times, don't :command:`exit` the chroot.
If you have write access to the mirror, you can make a dry-push to check and then upload; push will also remove any old packages on the mirror.


Cooking undigest packages
-------------------------

.. compound::
   On Tank we run the undigest cooker under tty6, using :program:`conspy`:

   .. code-block:: console

      $ su
      # conspy -f 6
      # cooker

   or

   .. code-block:: console

      # cooker pkg pkgname


Stable packages
---------------

Packages for the stable release are also built in a chroot environment like the Cooking packages:

.. code-block:: console

   # conspy -f 2


Upload by hand on mirror.slitaz.org
-----------------------------------

Mirror maintainers can upload by hand with :program:`tazdev` (``-dp`` for a dry push):

.. code-block:: console

   # tazdev -p $USER


Recreate a chroot
-----------------

If a chroot seems broken or needs to be rebuilt.
Exit the chroot and unmount the virtual system (:file:`/home` is kept) as root.
From the corresponding :program:`conspy` session:

.. code-block:: console

   # tazdev clean-chroot
   # tazdev gen-chroot
   # tazdev chroot
   #/ tazpkg recharge
   #/ cook setup
   #/ cooker
