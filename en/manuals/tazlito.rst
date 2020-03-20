.. _tazlito manual:

Tazlito Manual
==============


NAME
----

Tazlito — SliTaz Live Tool.


SYNTAX
------

.. code-block:: shell

   tazlito [command] [list|iso|flavor] [dir]


DESCRIPTION
-----------

:program:`Tazlito` is a small utility to extract a LiveCD, rebuild the ISO image and regenerate the root filesystem of the LiveCD.
:program:`Tazlito` can also generate a distribution from a list of packages previously downloaded.
To run, :program:`Tazlito` uses the configuration file :file:`/etc/tazlito/tazlito.conf` or an easily generated :file:`tazlito.conf` found in the current directory.
It specifies the name of the ISO, volume, maintainer and the paths of the packages to distribute and the generated ISO.
:program:`Tazlito` can also set up a directory containing additional files which will be copied to the LiveCD when generating the distribution.

:program:`Tazlito` is distributed under the free GNU licence GPL v.3, installed by default on SliTaz and installed/successfully tested on Debian GNU/Linux.
You will find additional information about creating a LiveCD in the Handbook.


COMMANDS
--------


.. _tazlito usage:

usage
~~~~~

The ``usage`` command displays a summary of available commands with a short description:

.. code-block:: console

   # tazlito usage


.. _tazlito stats:

stats
~~~~~

``stats`` displays the configuration variables, the paths to the various files and directories, and information on the ISO image:

.. code-block:: console

   # tazlito stats


.. _tazlito gen-config:

gen-config
~~~~~~~~~~

The ``gen-config`` command allows you to generate a configuration file ready to be edited.
By default the file is created in the current directory, but can be in another directory if specified via the command line:

.. code-block:: console

   # tazlito gen-config
   # tazlito gen-config /path/to/distro


.. _tazlito configure:

configure
~~~~~~~~~

This command configures the system configuration file or one found in the current directory:

.. code-block:: console

   # tazlito configure


.. _tazlito gen-iso:

gen-iso
~~~~~~~

The ``gen-iso`` command can generate a new LiveCD image following modifications and additions to the root filesystem of the CD-ROM.
To function, this command needs a directory containing the distro-tree of the Live system.
This tree can easily be built with the :ref:`tazlito extract-distro` command, modified and rebuilt via:

.. code-block:: console

   # tazlito gen-iso


.. _tazlito gen-initiso:

gen-initiso
~~~~~~~~~~~

The ``gen-initiso`` command will do the same work as ``gen-iso``, but it rebuilds the initramfs compressed system prior.
The initramfs contains the root filesystem and must be rebuilt if modified:

.. code-block:: console

   # tazlito gen-initiso


.. _tazlito list-flavors:

list-flavors
~~~~~~~~~~~~

The ``list-flavors`` command downloads (if necessary) and displays a list of the different flavors available.
You can force the download with the ``--recharge`` option:

.. code-block:: console

   # tazlito list-flavors
   # tazlito list-flavors --recharge


.. _tazlito get-flavor:

get-flavor
~~~~~~~~~~

The ``get-flavor`` command downloads (if necessary) and prepares the files for :ref:`tazlito gen-distro` to generate a flavor:

.. code-block:: console

   # tazlito get-flavor particular-flavor


.. _tazlito show-flavors:

show-flavor
~~~~~~~~~~~

The ``show-flavor`` command displays the description of the flavor and its size after regeneration.
The options ``--brief`` and ``--noheader`` reduce the output displayed:

.. code-block:: console

   # tazlito show-flavor particular-flavor
   # tazlito show-flavor particular-flavor --brief
   # tazlito show-flavor particular-flavor --brief --noheader


.. _tazlito gen-flavor:

gen-flavor
~~~~~~~~~~

The ``gen-flavor`` command creates a description file of a new flavor from the results of generating a distro (:ref:`tazlito gen-distro`).
The :file:`.flavor` file can then be sent to slitaz.org:

.. code-block:: console

   # tazlito gen-flavor new-flavor


.. _tazlito gen-liveflavor:

gen-liveflavor
~~~~~~~~~~~~~~

The ``gen-liveflavor`` command creates a description file of a new flavor from the results of generating a distro based on the current system.
The ``--help`` option provides more information:

.. code-block:: console

   # tazlito gen-liveflavor
   # tazlito gen-liveflavor --help


.. _tazlito upgrade-flavor:

upgrade-flavor
~~~~~~~~~~~~~~

The ``upgrade-flavor`` command refreshes a flavor file by updating the list of packages with the latest versions available:

.. code-block:: console

   # tazlito upgrade-flavor this-flavor


.. _tazlito extract-flavor:

extract-flavor
~~~~~~~~~~~~~~

The ``extract-flavor`` command converts a flavor into an easily modifiable tree structure in :file:`/home/slitaz/{VERSION}/flavors` which can be managed with mercurial: http://hg.slitaz.org/flavors.
For example on cooking you will have the work directory in :file:`/home/slitaz/cooking`.

.. code-block:: console

   # tazlito extract-flavor this-flavor


.. _tazlito pack-flavor:

pack-flavor
~~~~~~~~~~~

The ``pack-flavor`` command converts a tree structure in :file:`/home/slitaz/{VERSION}/flavors` into a flavor file (:file:`.flavor`).
It is inverse of :ref:`tazlito extract-flavor`:

.. code-block:: console

   # tazlito pack-flavor this-flavor


.. _tazlito extract-distro:

extract-distro
~~~~~~~~~~~~~~

The ``extract-distro`` command is used to extract an ISO image from the LiveCD to rebuild the structure of the root CD-ROM and system.
It is then possible to make the desired changes or additions and rebuild the ISO image via :ref:`tazlito gen-iso` or :ref:`tazlito gen-initiso`.
Example of use:

.. code-block:: console

   # tazlito extract-distro slitaz-cooking.iso


.. _tazlito gen-distro:

gen-distro
~~~~~~~~~~

The *Generate Distribution* command can generate the distro-tree and an ISO image via a list of packages.
To function, this command needs a list of packages, a directory containing all the (:file:`.tazpkg`) packages on the list, and a directory to generate the distribution.
The list of packages can be extracted from a flavor with the :ref:`tazlito get-flavor` command.
If one uses the LiveCD, the options ``--cdrom`` and ``--iso=`` permit the regeneration of packages that place files in :file:`/boot` without being obliged to download them and recovers the additional files of the LiveCD.
The path to the various directories are configured in the configuration file and packages can be downloaded from the SliTaz mirrors or generated by Cookutils.
To generate a distribution:

.. code-block:: console

   # tazlito gen-distro
   # tazlito gen-distro --cdrom
   # tazlito gen-distro --iso=slitaz.iso
   # tazlito gen-distro package-list


.. _tazlito clean-distro:

clean-distro
~~~~~~~~~~~~

Removes all files generated or extracts of the structure of the LiveCD:

.. code-block:: console

   # tazlito clean-distro


.. _tazlito check-distro:

check-distro
~~~~~~~~~~~~

This command simply verifies if files installed by the packages are present on the system:

.. code-block:: console

   # tazlito check-distro


.. _tazlito writeiso:

writeiso
~~~~~~~~

This command will write the current filesystem to a cpio archive (:file:`rootfs.gz`) and then generate a bootable ISO image.
Writeiso can be used in a HD install or in live mode and will also archive your current :file:`/home` directory.
This command lets you easily remaster and build your own LiveCD image, just boot, modify any files, and then:

.. code-block:: console

   # tazlito writeiso [gzip|lzma|none]
   # tazlito writeiso gzip
   # tazlito writeiso gzip image-name


.. _tazlito check-list:

check-list
~~~~~~~~~~

Checks if the :file:`distro-packages.list` is updated with the latest package versions:

.. code-block:: console

   # tazlito check-list


.. _tazlito repack:

repack
~~~~~~

Recompresses the rootfs with the best possible compression:

.. code-block:: console

   # tazlito repack slitaz.iso


.. _tazlito merge:

merge
~~~~~

Combines several flavors like nested Russian dolls.
Each rootfs is a subset of the previous.
The first rootfs is extracted from the ISO image used in the third argument.
The flavor will then be chosen to launch at startup according to the amount of RAM available:

.. code-block:: console

   # tazlito merge 160M slitaz-core.iso 96M rootfs-justx.gz 32M rootfs-base.gz


.. _tazlito build-loram:

build-loram
~~~~~~~~~~~

Creates an ISO image flavor for low RAM systems from a SliTaz ISO image.
You can build a flavor with :file:`/` always in RAM or where :file:`/` resides on the CD-ROM:

.. code-block:: console

   # tazlito build-loram slitaz.iso loram.iso
   # tazlito build-loram slitaz.iso loram-cdrom.iso cdrom


.. _tazlito emu-iso:

emu-iso
~~~~~~~

The ``emu-iso`` command uses the Qemu emulator to start and run SliTaz.
Qemu is used to test the newly built ISO image without burning to a CD-ROM or booting into frugal mode:

.. code-block:: console

   # tazlito emu-iso
   # tazlito emu-iso path/to/image.iso


.. _tazlito burn-iso:

burn-iso
~~~~~~~~

``burn-iso`` will guess the CD-ROM device and its speed, and :command:`wodim` (part of :program:`cdrkit`) will begin to burn an ISO image.
The default ISO image is the one located in the current configuration file, but it's possible to specify a different image via the command line:

.. code-block:: console

   # tazlito burn-iso
   # tazlito burn-iso slitaz-hacked.iso


FLAVORS
-------

A :file:`.flavor` file contains just a few KB of information needed to (re)manufacture a custom LiveCD of SliTaz.


.. _tazlito rebuild-flavor:

Manufacture a flavor
~~~~~~~~~~~~~~~~~~~~

You can choose the flavor to (re)manufacture from among those available:

.. code-block:: console

   # tazlito list-flavors
   
   List of flavors
   ==========================================================================
   Name      ISO   Rootfs Description
   ==========================================================================
   base       6.9M  13.1M Minimal set of packages to boot
   core-3in1 31.5M 105.6M SliTaz core system with justX and base alternatives
   core      31.5M 104.6M SliTaz core system
   eeepc     31.2M 105.4M SliTaz eeepc system
   justX     16.1M  51.2M SliTaz with a minimal X environment

We will start by remanufacturing the *eeepc* flavor which uses 105.4M of RAM and has a CD-ROM size of 31.2M:

.. code-block:: console

   # tazlito clean-distro
   # tazlito get-flavor eeepc
   # tazlito gen-distro


.. _tazlito create-flavor:

Create a flavor
~~~~~~~~~~~~~~~

To create a flavor, you must:

* Either create an ISO image with :ref:`tazlito gen-distro` and then create a flavor file with :ref:`tazlito gen-flavor`
* Either directly create the tree structure that defines the flavor (see :ref:`tazlito extract-flavor`) and then create the flavor with :ref:`tazlito pack-flavor`
* Either use the `online builder <http://pizza.slitaz.org/>`_


.. _tazlito publish-flavor:

Post a flavor
~~~~~~~~~~~~~

Because a :file:`.flavor` file contains just a few KB, it can be easily sent via the `mailing list <http://www.slitaz.org/en/mailing-list.php>`_.

The results of :ref:`tazlito extract-flavor` can also be put in `mercurial <http://hg.slitaz.org/flavors>`_.
This method is preferred because the tree will be directly visible with the `mercurial web interface <http://hg.slitaz.org/flavors/file/tip>`_.

This tree includes:

* A :file:`receipt` file describing the flavor thanks to the variables:

  * ``FLAVOR``: The flavor name.
  * ``SHORT_DESC``: Short description.
  * ``VERSION``: Free format.
  * ``MAINTAINER``: Email address of maintainer.
  * ``FRUGAL_RAM``: Minimum RAM required (optional).
  * ``ROOTFS_SIZE``: Size of :file:`rootfs.gz` decompressed into RAM (optional).
  * ``INITRAMFS_SIZE``: Size of :file:`rootfs.gz` on the CD-ROM (optional).
  * ``ISO_SIZE``: Size of CD-ROM (optional).
  * ``ROOTFS_SELECTION``: Optional, see :ref:`tazlito meta-flavor` below.

* The file :file:`packages.list` containing the list of packages without specifying the version (:program:`tazlito` uses the latest available).
  This file is missing if ``ROOTFS_SELECTION`` exists in the receipt.
* The optional :file:`mirrors` file containing the list of unofficial mirrors (undigest) to be added to include personal packages.
* The optional directory :file:`rootfs` containing the tree to add to the root filesystem :file:`rootfs.gz` (configuration files usually).
* The optional directory :file:`rootcd` containing the tree to add to the root of the CD-ROM.


.. _tazlito tune-flavor:

Adapt a flavor
~~~~~~~~~~~~~~

It is often easier to modify an existing flavor than to create one from scratch.
To adapt the eeepc flavor for example:

.. code-block:: console

   # tazpkg get-install mercurial
   # cd /home/slitaz
   # hg clone http://hg.slitaz.org/flavors
   # cd flavors
   # cp -a eeepc myslitaz

Files in :file:`myslitaz` can then be changed, and:

.. code-block:: console

   # tazlito pack-flavor myslitaz

Will simply create the new flavor.

.. tip::
   You can skip :program:`mercurial` installation by extracting a flavor.
   Using the previous example:

   .. code-block:: console

      # tazlito get-flavor eeepc
      # tazlito extract-flavor eeepc.flavor
      # cd /home/slitaz/flavors
      # cp -a eeepc myslitaz


.. _tazlito meta-flavor:

Meta flavor
~~~~~~~~~~~

A meta flavor contains several flavors like nested Russian dolls.
The flavor will be launched at startup according to the amount of RAM available.
The ``ROOTFS_SELECTION`` variable defines the minimum RAM and corresponding flavor parameters, `example <http://hg.slitaz.org/flavors/file/tip/core-4in1/receipt>`_:

.. code-block:: shell

   ROOTFS_SELECTION="160M core 96M justX 32M base"

A meta flavor doesn't contain a list of packages (:file:`packages.list`).
SliTaz kernels prior to 2.6.30 do not support meta flavors.


MAINTAINER
----------

* Christophe Lincoln <pankso@slitaz.org>
* Pascal Bellard <pascal.bellard@slitaz.org>
