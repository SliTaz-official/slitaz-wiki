.. http://doc.slitaz.org/en:devnotes:cook-flavor-from-scratch
.. en/devnotes/cook-flavor-from-scratch.txt · Last modified: 2011/02/22 22:59 by linea

.. _cook flavor from scratch:

Cook a flavor from scratch
==========================

:author: gokhlayeh, oui, linea


Introduction
------------

Welcome to this howto!
it's the base for a second scratchbook which explains how to recompile SliTaz entirely from scratch including the toolchain.
All jobs will be done using tazwok-experimental (actually in Alpha state).
Even if the scripts have some major issues, the steps explained here should do the job without too many problems.
Please report any problems to the author (tazpkg info tazwok-experimental gives you my mail).

Having some knowledge about how the legacy SliTaz tools and basic commands work will help you to understand what's going on in this howto.
However, simply copying/pasting the given command lines will probably work.


Explanation
^^^^^^^^^^^

:program:`Tazwok` now provides tools to cook the wok from scratch using a minimal chroot.
You have to use a special version of the wok called wok-experimental.
It contains the needed patches which should be applied on top of the cooking wok.
Explanations about how to obtain the wok experimental are here: :ref:`prepare experimental`.


.. rubric:: Tools needed

* A functional cooking system (live or installed), you also can use the `slitaz-experimental-base <http://people.slitaz.org/~gokhlayeh/experimental/iso/slitaz-experimental-base.iso>`_ ISO.
* tazwok-experimental-0.0.2, tazchroot-0.0.1 and libtaz-0.0.1. Note: installing tazwok-experimental removes tazwok legacy.
* An internet connection to download sources.


.. rubric:: Steps

* Make the chroot (in this how-to we use the packages of the new toolchain from the experimental repository; but this should work with the cooking toolchain).
* Cook a temporary toolchain: it's a cross-compiled toolchain which will cook the "real" toolchain (the packaged one) without linking anything to the host system.
* Cook the definitive toolchain.
* Cook some of the other packages of one of the SliTaz flavors.
* Create the iso.
* Burn, boot & enjoy :).


Install the tools
^^^^^^^^^^^^^^^^^

.. tip::
   Note: all the following commands should be executed as root.
   You don't have to add the experimental repository if you wish to use the cooking toolchain.

Add the cooking-experimental repository as undigest:

.. code-block:: console

   # tazpkg add-undigest experimental http://people.slitaz.org/~gokhlayeh/experimental/packages

Give it priority over the main repository:

.. code-block:: console

   # echo experimental > /var/lib/tazpkg/priority

Install the cooking tools:

.. code-block:: console

   # tazpkg get-install tazwok-experimental
   # tazpkg get-install tazchroot


Cook toolchain
^^^^^^^^^^^^^^

.. tip::
   The following command-lines work for a wok at: :file:`/home/slitaz/experimental/wok`.
   If you put the wok elsewhere, you can use the option ``--SLITAZ_DIR=address``; where the address is equivalent to :file:`/home/slitaz`.
   This option must be used each time ``--SLITAZ_VERSION`` is used.
   You can also define ``SLITAZ_VERSION`` & ``SLITAZ_DIR`` globally using :file:`/etc/slitaz/slitaz.conf`.

Configure the *chroot*:

.. code-block:: console

   # tazwok configure-chroot --SLITAZ_VERSION=experimental

If you have at least 1GB RAM free you can put the minimal chroot in RAM speeding-up the cooking process:

.. code-block:: console

   # sed 's~chroot_dir=.*~chroot_dir=/tmp/chroot-experimental~' -i /home/slitaz/experimental/tazchroot.conf

All-in-one command to cook the toolchain packages:

.. code-block:: console

   # tazwok cook-toolchain --SLITAZ_VERSION=experimental

At the end of this operation the chroot should be removed.
If you have modified its address before, you have to do this manually:

.. code-block:: console

   # rm -r /tmp/chroot-experimental

Toolchain packages are now ready to be used.
They're actually are in the packages-incoming repository.
If all was cooked fine it's possible to push them to the classical packages directory using:

.. code-block:: console

   # tazwok check-incoming --SLITAZ_VERSION=experimental

You need to tell to :program:`tazpkg` that you now have a local version of experimental:

.. code-block:: shell

   # If you had already defined the experimental repository:
   echo "/home/slitaz/experimental/packages" > /var/lib/tazpkg/undigest/experimental/mirror
   tazpkg recharge
   # Else:
   tazpkg add-undigest experimental /home/slitaz/experimental/packages
   echo experimental > /var/lib/tazpkg/priority
   tazpkg recharge

Recook the toolchain packages except core (linux-api-headers/glibc/binutils/gcc) over themselves to consolidate it; in fact it's not really needed with an actual configuration but it's generally good to do to solve loop dependencies.
It warrants consistency of the toolchain before and after an update of non-core toolchain packages.
Note that core-toolchain should never be updated in this way — but by reusing the cook-toolchain script:

.. code-block:: console

   # tazwok chroot --SLITAZ_VERSION=experimental
   # tazwok build-depends toolchain-cooklist | sed '1,/^gcc$/d' > /tmp/consolidate.list
   # tazwok cook-list /tmp/consolidate.list
   # rm /tmp/consolidate.list

As packages has been re-cooked, you have to update the packages repository once again:

.. code-block:: console

   # tazwok check-incoming

Still in the chroot for the next step.


Cook packages of a flavor
^^^^^^^^^^^^^^^^^^^^^^^^^

First you need data about cooking flavors:

.. code-block:: console

   # cd /home/slitaz/experimental/flavors
   # hg clone http://hg.slitaz.org/flavors .

Generate the cooklist for a given flavor.
Note: for the next step, use the chosen flavor name instead of FLAVOR in the command line:

.. code-block:: console

   # tazwok gen-cooklist --list=/home/slitaz/experimental/flavors/FLAVOR/packages.list > /tmp/FLAVOR.list

This list contains some already cooked packages, remove them:

.. code-block:: shell

   cat /tmp/FLAVOR.list | while read p; do
   	grep -q ^$p$ /home/slitaz/experimental/packages/packages.txt &&
   		sed "/^$p$/d" -i /tmp/FLAVOR.list
   done

The cooklist is now ready to create the packages:

.. code-block:: console

   # tazwok cook-list /tmp/FLAVOR.list

Update packages repository:

.. code-block:: console

   # tazwok check-incoming

.. tip::
   You can repeat these steps each time you wish to add new packages to your repository.

Still in chroot for the next step.


Create the ISO
^^^^^^^^^^^^^^

Actually :program:`tazlito` uses :file:`/home/slitaz/flavors`.
Create a symbolic link:

.. code-block:: console

   # ln -s /home/slitaz/experimental/flavors /home/slitaz

Set release as experimental for the future ISO:

.. code-block:: console

   # cd /home/slitaz/flavors/FLAVOR
   # mkdir -p rootfs/etc
   # echo experimental > rootfs/etc/slitaz-release

Configure your ISO to use your local repository:

.. code-block:: console

   # mkdir -p rootfs/var/lib/tazpkg
   # echo /home/slitaz/experimental/packages > rootfs/var/lib/tazpkg/mirror

Use tazwok-experimental instead of tazwok and add tazchroot:

.. code-block:: console

   # sed 's/tazwok/tazwok-experimental/' -i packages.list
   # echo tazchroot >> packages.list

Pack FLAVOR:

.. code-block:: console

   # tazlito pack-flavor FLAVOR

Get FLAVOR:

.. code-block:: console

   # tazlito get-flavor FLAVOR

Generate ISO:

.. code-block:: console

   # tazlito gen-distro

Save ISO in your home dir:

.. code-block:: console

   # mv /home/slitaz/distro/slitaz-FLAVOR.iso /home/slitaz/distro/slitaz-FLAVOR.md5 /home/slitaz/experimental/iso

Exit chroot:

.. code-block:: console

   # exit
