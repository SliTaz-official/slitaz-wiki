.. http://doc.slitaz.org/en:handbook:genlivecd
.. en/handbook/genlivecd.txt · Last modified: 2017/01/05 17:32 by hgt

.. _handbook genlivecd:

Generating a Customised LiveCD or LiveUSB
=========================================

:program:`Tazlito` can create your own, personalised version of SliTaz in very little time.
It allows you to choose precisely the software packages that you want and make a flavor file (:file:`.flavor`) with it.
You can choose to make your own file starting from a general slitaz flavor (such as *base*, *justx* or *core*), or make it based on an already customised flavor supplied by the SliTaz community.

Once you've downloaded (and possibly — further — customized) the flavor you chose, you can make an iso-file with it using :program:`tazlito`, and then burn it to a CD.


.. rubric:: Overview

:program:`Tazlito` automates the process of building a customized ISO, and the method is quite straight-forward:

#. find a template (flavor) to work on

   .. code-block:: console

      $ tazlito list-flavors

#. download the template

   .. code-block:: console

      $ tazlito get-flavor flavor_name

#. look at the flavor's packages

   .. code-block:: console

      $ tazlito show-flavor flavor_name

#. extract the flavor

   .. code-block:: console

      $ tazlito extract-flavor flavor_name

#. put in a rootfs folder (make one by copying in files from :file:`/etc/` to :file:`/home/slitaz/distro`)
#. add additional files using the folder :file:`/home/slitaz/distro/addfiles` (optional)
#. change options

   .. code-block:: console

      $ tazlito configure

#. create the ISO image

   .. code-block:: console

      $ tazlito gen-distro

#. burn to CD or USB stick

   .. code-block:: console

      $ tazusb gen-iso2usb


.. rubric:: Notes

* Note that you can't delete packages, but you can use a stripped-down flavor to build your customized slitaz version.
* Note that you can modify the distro further by hacking the ISO once created, after which you can make it into a flavor again (see details below)
* Note that you can add some last changes (that require modification from inside SliTaz; like changing appearance, modifying the taskbar, …) by simply making an additional rootfs file with a usb stick made with the ISO of the SliTaz version you just made (using the :command:`# tazusb writefs lzma` command), and then adding it to the extracted ISO (using the :ref:`hacker account <handbook hacklivecd>`).
  You can probably also reinclude these changes finally into your flavor file by means of the :command:`# tazlito iso2flavor <your-ISO_image> <your-flavor>` command.
* Creating a LiveCD requires a minimum of 256MB of RAM.
* Compressing the file-system with LZMA may take some time.
  GZip can also be used, though its compression ratio is not as effective.
* It's possible to create a distribution from the LiveCD or from an installed system.
  In LiveCD mode, it's advisable to use persistent media or an USB drive to store generated files and save space in RAM.
* :program:`Tazlito` is used by developers to generate the official LiveCD.
  Unlike the (now deprecated :program:`tazwok`), it is installed by default on SliTaz.
  Note that you can also use it an a different distribution (non-SliTaz) if that Linux distro has the necessary packages (they can be downloaded if not).
  See :ref:`the instructions <installing tazlito on another distribution>` at the bottom of the page for more details.


A customized ISO in three commands
----------------------------------

A good way of becoming familiar with :program:`Tazlito` is to re-build the LiveCD.
To do this, download the default *core* flavor (a :file:`.flavor` file that contains all the necessary information to create a LiveCD) and build it with the default options.
This takes only three commands and will help to demonstrate the process:

* Get the default (*core*) flavor

  .. code-block:: console

     # tazlito get-flavor core

* Extract the flavor

  .. code-block:: console

     # tazlito extract-flavor core

* Optional: Amend the list of packages or add additional files.
* Create the LiveCD

  .. code-block:: console

     # tazlito gen-distro

That's it!
You will find the LiveCD ISO image in :file:`/home/slitaz/{version}/distro`.


The steps laid out in detail…
-----------------------------


Part I: Downloading a Flavor
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Flavors can be used to build various styles of LiveCDs by containing the list of packages (:file:`distro-packages.list`) and sometimes additional files.
This saves you the effort of saving the configuration files and provides a quick way of switching between them.
A list of flavors available on the SliTaz servers can be downloaded and updated with :program:`Tazlito`:

.. code-block:: console

   # tazlito list-flavors

or:

.. code-block:: console

   # tazlito list-flavors --recharge

The list of flavors will be automatically displayed, showing the size of the LiveCD and a brief description.
To use a flavor, use the :command:`get-flavor` command to download the flavor file, automatically extract the package list and description in the current directory, and put additional files in the :file:`/home/slitaz/distro/addfiles` directory:

.. code-block:: console

   # tazlito get-flavor flavor_name

The description of the flavor can be displayed with the :command:`show-flavor` command.
Once your chosen flavor is downloaded, you can start to customise your distribution!

.. tip::
   To store flavors, we suggest that you use a specific directory such as :file:`/home/slitaz/flavors`.
   You can create this yourself and proceed inside:

   .. code-block:: console

      # mkdir -p /home/slitaz/flavors
      # cd /home/slitaz/flavors


Part II: Generating the Distribution
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. important::
   When generating a distro, there are some essential :program:`base-system` packages that must be included.
   The dependency information in each package will take effect so these will be automatically included, even if they are not specified in the package list.

For your first ISO, we advise you re-build the default LiveCD without modifying the list.
When you are comfortable with the process, you can start to delete or add packages as required.
The :command:`gen-distro` command generates an ISO image with all the packages on the list.
All dependencies will be fulfilled automatically, just as with normal package installation.
It must be run as root and be in the same directory as the list and the configuration file.

:program:`Tazlito` uses the :file:`distro-packages.list` file to download each package and 'install' it into a pseudo-file-system.
This is the distro tree and contains the whole of the LiveCD's operating system.
This is not unlike *chroot*\ -ing in to another system from a LiveCD.
It is then compressed and added to a CD ISO image with booting information.

To generate a LiveCD:

.. code-block:: console

   # tazlito gen-distro

Voilà!
Your first LiveCD ISO image is generated in :file:`/home/slitaz/distro`!


.. rubric:: Cleaning & Re-Generating

As you have generated a LiveCD, you need to clean-up so another can be generated.
Removing the resulting ISO image, cleaning the generated distro tree (but not your additional files!) can be done through the ``clean-distro`` option:

.. code-block:: console

   # tazlito clean-distro

You can now customise your LiveCD further.
To start, add one or two packages to the list.
When you are ready, re-generate a new ISO image with the ``gen-distro`` command:

.. code-block:: console

   # tazlito gen-distro

All the customisations applied so far should be effective on your new LiveCD ISO image.

.. tip::
  :program:`Tazlito` also offers several possibilities such as burning the ISO, copying additional files into the system or directly on to the CD-ROM.
  See the `Tazlito Manual <http://hg.slitaz.org/tazlito/raw-file/tip/doc/tazlito.en.html>`_ for more information.


.. rubric:: Hacking your customized SliTaz ISO

See also :ref:`handbook hacklivecd`

If you want to modify the splash screen, boot loader, configuration files, GRUB itself, … you need to "hack" the ISO you made using your flavor file using a "hacker user account".
Note that you can also update your flavor-file with these extra modifications you made at the boot loader, splash screen, … by means of the

.. code-block:: console

   # tazlito iso2flavor <your-ISO_image> <your_flavor>

command.

To make a hacker user account, we must decompress the root file-system, add the account, then re-compress the root file-system.
The first two stages can be executed as one command, the last stage another:

.. code-block:: console

   # tazlito addhacker
   # tazlito gen-initiso

This adds the account only to the LiveCD; your running system will not be affected.


Adding the final changes
------------------------

See also :ref:`persistence splash`

As mentioned above, you can add some last changes (that require modification from inside SliTaz; like changing appearance, modifying the taskbar, …) by simply making an additional :file:`rootfs` file with a USB stick made with the ISO of the SliTaz version you just made (using the

.. code-block:: console

   # tazusb writefs lzma

command), and then adding it to the extracted ISO (using the :ref:`hacker account <handbook hacklivecd>`).
You can probably also reinclude these changes finally into your flavor file by means of the

.. code-block:: console

   # tazlito extract-iso <your-ISO_image> <destination>

command, combined with the

.. code-block:: console

   # tazlito pack-flavor <flavor_name>

command.
These 2 commands replace the old :command:`tazlito iso2flavor` command (which can't be used any more).

It should be noted that using this technique, you can probably delete packages too (see :ref:`the manual on making many-in-one flavors <manyinone>`).
However, you best avoid this method however since the files are then probably still in the flavor/ISO and the flavor/ISO is hence larger than what it needs to be.
The files/applications will simply not be used.


Including Additional Files
--------------------------

The files containing flavors usually have additional files copied directly into the file-system or the root of the CD-ROM.
The path to the files used is specified in the configuration file :file:`tazlito.conf` — by default, these files are located in :file:`/home/slitaz/distro/addfiles`.
The additional files the *core* flavor provides are for example: the window manager :program:`JWM` and some wallpapers.
It is easy enough to modify, add or delete files in the root file-system (rootfs) or the root of the CD (rootcd) and regenerate the distribution.
To clean and regenerate the distribution:

.. code-block:: console

   # tazlito clean-distro
   # tazlito gen-distro


Packages Used
-------------

To create your distro, you need SliTaz packages.
You can easily create your own packages with :program:`Tazpkg`, or recreate packages from a SliTaz system in operation with :program:`Tazlito`.
By default, SliTaz packages are located in the :file:`/home/slitaz/packages` directory but you can change this in the :program:`Tazlito` configuration file (see :ref:`the bottom of this page <tazlito configuration file>` for more information).

If you want to retrieve packages manually, you can use FTP software such as :program:`gFTP` (installed by default on SliTaz) or the command line and :program:`wget`.
The direct URL to package versions is: ``ftp://download.tuxfamily.org/slitaz/packages/``

You can also use your own packages by putting them in the :file:`packages` directory.
For more information on creating your own SliTaz packages, see the :ref:`Cookbook <cookbook>`.


.. _tazlito configuration file:

Configuration File
------------------

:program:`Tazlito` uses the :file:`tazlito.conf` located in the current directory, if it exists, or defaults to the system configuration file :file:`/etc/tazlito/tazlito.conf`.
This means that you can use the default file or a configuration file specific to the distro you want to create.

This file specifies a number of properties:

* the name of the ISO image (``ISO_IMAGE``)
* the label used when mounted (``VOLUME_NAME``)
* the name of the maintainer (``PREPARED``)
* the paths to the working directories:

  * where the distro tree is located (``DISTRO``); default: :file:`/home/slitaz/distro`
  * any packages that may be installed (``PACKAGES_REPOSITORY``); default: :file:`/home/slitaz/packages`
  * the files that should be added to the built LiveCD (``ADDFILES``); default: :file:`$DISTRO/addfiles`

* options for running :program:`Tazlito` inside QEMU (``QEMU_OPTS``)
* the compression algorithm used on the root file-system (``COMPRESSION``):

  * lzma (default)
  * gzip
  * bzip2

To create and configure your own configuration file, you just need to run :program:`Tazlito` with the ``configure`` command from the working directory of the distro.
Assuming you have the tree of the distro in :file:`/home/slitaz/distro` and you're working from it, use the command:

.. code-block:: console

   $ tazlito configure

Once you've answered the questions, you can either create the ISO image, rebuild the root file-system and ISO, or generate a new distro with the list of packages.

.. tip::
   If you generate many LiveCDs, including the :command:`date` command in the ISO name may be beneficial.
   To do this, use a line such as:

   .. code-block:: shell

      ISO_NAME="MyLiveCD-`date +%Y%m%d-%H%M`"


Advanced: Creating a Flavor
---------------------------

:program:`Tazlito` allows you to easily create your own flavor from the results of generating the distribution (``gen-distro``).
The flavor will contain all the additional files, a description and a list of packages (which can be reused and updated later, according to their versions).
To generate your own flavor responding to one or two questions:

.. code-block:: console

   # tazlito gen-flavor new-flavor

Once your flavor is fully-functioning and well-tested, you can send it to SliTaz to make it available to everyone!
It will then be listed via ``list-flavor`` and usable via ``get-flavor``.
You can send a flavor by several ways:

* announce through a post on the `Mailing List <http://www.slitaz.org/en/mailing-list.html>`_;
* save the file on-line and report on the `Forum <http://forum.slitaz.org/>`_;
* send an e-mail to a SliTaz contributor.

A contributor with sufficient privileges will then publish your flavor on the SliTaz servers.


.. _installing tazlito on another distribution:

Installing Tazlito on Another Distribution
------------------------------------------

:program:`Tazlito` does not generate dependencies for the LiveCD, but it depends on :program:`Cdrkit` to burn the image and :program:`Tazpkg` to regenerate SliTaz packages you want installed.
To retrieve and install :program:`Tazlito`, follow the instructions in the :file:`README` file in the `sources of Tazlito <http://download.tuxfamily.org/slitaz/sources/tazlito/>`_.
