.. http://doc.slitaz.org/en:guides:xorg-xvesa
.. en/guides/xorg-xvesa.txt · Last modified: 2010/09/12 15:49 by linea

.. _xorg xvesa:

Xorg & XVesa
============

By default, SliTaz v3 uses Xorg 7.4 with the :program:`xorg-xf86-video-vesa` v2.0.0 driver.
This basic driver can cause problems with certain hardware combinations and can sometimes result in a very low resolution.
Occasionally, X does not start at all and instead falls back to the log-in screen or to a text-based prompt.
There are some simple solutions that can yield major improvements:

* check that you have enough RAM to run the default ISO.
  If not, use the `Low RAM ISO <http://mirror.slitaz.org/iso/3.0/flavors/>`_ instead.
* try the `XVesa ISO <http://mirror.slitaz.org/iso/3.0/flavors/slitaz-3.0-xvesa.iso>`_ (a.k.a. TinyX)
* install a more specific driver for your graphics card
* customise your Xorg configuration file (:file:`xorg.conf`)

Good introductions to Xorg Configuration can be found at `The FreeBSD Handbook <http://www.freebsd.org/doc/en/books/handbook/x-config.html>`_ and `X Configuration from Ubuntu <https://wiki.ubuntu.com/X/Config>`_.

.. tip::
   Up to v7.3, the :kbd:`Ctrl`\ +\ :kbd:`Alt`\ +\ :kbd:`Backspace` key combination could be used to quit the X server.
   To enable it in version 7.4 and later, type the following command from any X terminal emulator:

   .. code-block:: console

      # setxkbmap -option terminate:ctrl_alt_bksp

.. important::
   Throughout this page, commands preceeded with a hash sign (``#``) should be executed as the **root** user.
   This is best under a terminal window.
   Otherwise, the dollar symbol (``$``) denotes a regular (tux) user.


Using Xorg & Vendor-Specific Drivers
------------------------------------

When you are using SliTaz as your main system, it's recommended to use Xorg over XVesa.
You will get a much better display and performance than when using Xvesa tinyX.


AGP Cards
^^^^^^^^^

All AGP video cards need extra kernel modules to function under Xorg.
Check if you have an AGP video card with :command:`lspci`; if so, install the necessary modules in the :program:`linux-agp` package before using Xorg:

.. code-block:: console

   # tazpkg get-install linux-agp

Load the modules using the SliTaz hardware detection tool:

.. code-block:: console

   # tazhw detect-pci

You can now use the SliTaz X configuration tool to detect your settings:

.. code-block:: console

   # tazx

Select the appropriate driver for your video card from the list.


.. _dri drm problem:

DRI / DRM Problem
^^^^^^^^^^^^^^^^^

Sometimes, the auto-detection is not enough.
Cards that require `DRI / DRM <http://www.bitwiz.org.uk/s/how-dri-and-drm-work.html>`_ are supported under SliTaz, but v3 has a couple of bugs!
The file :file:`/dev/dri` should be a directory and not a file and, to get DRI working correctly, we have to add ``tux`` to the group ``video`` or modify permissions in the Xorg configuration file:

.. code-block:: console

   # tazpkg get-install linux-drm
   # tazpkg get-install mesa-demos   # for glxinfo and glxgears
   # rm /dev/dri
   # mkdir /dev/dri/
   # addgroup tux video

The ``drm`` module is not loaded by :program:`tazhw` so you have to do it manually:

.. code-block:: console

   # modprobe drm

.. tip::
   All modules, which are loaded by :program:`tazhw` and yourself to make your changes permanent, have to be added to the :program:`SliTaz Control Box` under :guilabel:`Initialization` in :guilabel:`Load Modules`.


Intel cards
^^^^^^^^^^^

The :program:`xorg-xf86-video-vesa` 2.0.0 driver has a lot of trouble with Intel chips.
For example, on a 82945GM chipset it does not display 1280×1024 and 1024×768, but 1600×1200 is not a problem.

Use :program:`tazx` to select the ``intel`` driver, then :command:`tazhw detect-pci`, solve the ``dri`` problem and load ``drm`` and restart X — normally that's it (perhaps you must change your :file:`xorg.conf` too).

Some users may need to add the ``intel_agp`` module to the :file:`xorg.conf` file:

.. code-block:: xorg.conf
   :emphasize-lines: 3

   Section "Module"
     # ...
     Load  "intel_agp"
     # ...
   EndSection

for the driver to work.

Depending on the hardware, the *mode-setting* feature must be turned on or off.
To turn it off append one of the following to the ``kernel`` line in the GRUB boot-loader configuration:

* ``nomodeset``
* ``i810.modeset=0``
* ``i915.modeset=0``

If mode-setting is off by default and should instead be turned on, append one of the following:

* ``modeset``
* ``i810.modeset=1``
* ``i915.modeset=1``


Trident cards
^^^^^^^^^^^^^

.. compound::
   Use :command:`tazx` to select the ``trident`` driver, install :program:`mesa-dri-trident`

   .. code-block:: console

      # tazpkg get-install mesa-dri-trident

   and restart X


nVidia cards
^^^^^^^^^^^^

SliTaz provides automatic configuration for nVidia cards.
There are two drivers available, the Xorg-provided ``nv`` and the nVidia-provided, non-free ``nvidia``.
The ``nv`` driver should be tried first as this has been compiled for SliTaz, whereas the nVidia-supplied driver is a *binary blob* that tries to fit each and every Linux distribution.


.. rubric:: Free Driver (nv)

To set-up the free nVidia drivers, use the SliTaz Hardware Configuration tool:

.. code-block:: console

   # tazhw setup nvidia

Alternatively, you can do this process manually:

* Download the following packages: :program:`mesa`, :program:`mesa-demos`, :program:`linux-agp`, :program:`xorg-xf86-video-nv`

  .. code-block:: console

     # tazpkg get-install xorg-xf86-video-nv
     # tazpkg get-install mesa
     # tazpkg get-install mesa-demos
     # tazpkg get-install linux-agp</code>

* Load the kernel modules

  .. code-block:: console

     # tazhw detect-pci

* Replace the standard ``vesa`` driver with ``nv``

  .. code-block:: console

     # sed -i 's/vesa/nv/' /etc/X11/xorg.conf


.. rubric:: Non-Free Driver (nvidia)

To set-up the non-free nVidia drivers, use the SliTaz Hardware Configuration tool with the ``--non-free`` switch:

.. code-block:: console

   # tazhw setup nvidia --non-free

Alternatively, you can do this process manually:

* Download the following packages: :program:`mesa`, :program:`mesa-demos`, :program:`linux-agp`, :program:`nvidia`

  .. code-block:: console

     # tazpkg get-install nvidia
     # tazpkg get-install mesa
     # tazpkg get-install mesa-demos
     # tazpkg get-install linux-agp

* Load the kernel modules

  .. code-block:: console

     # tazhw detect-pci

* Attempt to configure the card with nVidia's tool:

  .. code-block:: console

     # nvidia-xconfig

* To test if rendering is working,

  .. code-block:: console

     # glxinfo | grep render

* To change nVidia's settings, use nVidia's Settings tool:

  .. code-block:: console

     # nvidia-settings


.. rubric:: Hiding the Logo

To hide the nVidia logo when the system boots, add the following to :file:`/etc/X11/xorg.conf` at the end of the ``Device`` section:

.. code-block:: xorg.conf

   Option "NoLogo" "True"


ATI cards
^^^^^^^^^

SliTaz provides automatic configuration for ATI cards.
There are two drivers available, the Xorg-provided ``radeon`` and the ATI-provided, non-free ``catalyst``.
The ``ati`` driver should be tried first as this has been compiled for SliTaz, whereas the ATI-supplied driver is a *binary blob* that tries to fit each and every Linux distribution.


.. rubric:: Free Driver (radeon)

To set-up the free ATI drivers, install the AGP & DRM modules, then use the SliTaz Hardware Configuration tool:

.. code-block:: console

   # tazpkg get-install linux-agp
   # tazpkg get-install linux-drm
   # tazhw setup ati</code>

— but perhaps this is not enough — check :file:`/var/log/Xorg.0.log` to see if other modules must be loaded: See :ref:`dri drm problem`

Alternatively, you can do this process manually:

* Download the following packages: :program:`xorg-xf86-video-ati`, :program:`mesa-dri-ati`, :program:`mesa-demos`, :program:`linux-agp`

  .. code-block:: console

     # tazpkg get-install xorg-xf86-video-ati
     # tazpkg get-install mesa-dri-ati
     # tazpkg get-install mesa-demos
     # tazpkg get-install linux-agp
     # tazpkg get-install linux-drm

* Load the kernel modules 

  .. code-block:: console

     # tazhw detect-pci

* To install the free ATI driver radeon, replace ``vesa`` with ``radeon`` in the :file:`/etc/X11/xorg.conf` file:

  .. code-block:: console

     # sed -i 's/vesa/radeon/' /etc/X11/xorg.conf


.. rubric:: Non-Free Driver (catalyst)

To set-up the non-free ATI drivers, use the SliTaz Hardware Configuration tool with the ``--non-free`` switch:

.. code-block:: console

   # tazhw setup ati --non-free


Modifying the Xorg Configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Normally Xorg will start successfully, but perhaps not with the correct resolution.
See the :ref:`adding resolutions` tips at the end of this page or sometimes search the Internet for the :file:`xorg.conf` for your card and your monitor — or use another LiveCD and copy the working configuration file to SliTaz.


Configuring X
-------------

While X will do it's bets to auto-configure itself for your graphics card set-up, sometimes it needs a tweak.
Its configuration file, :file:`/etc/X11/xorg.conf`, is the place to customise the configuration.
The best example of this is switching drivers or adding resolutions it doesn't detect.


.. _adding resolutions:

Adding Resolutions
^^^^^^^^^^^^^^^^^^

#. Include ``HorizSync`` and ``VertRefresh`` (refresh timings) in the ``Monitor`` section:

   .. code-block:: xorg.conf
      :emphasize-lines: 5, 6

      Section "Monitor"
        Identifier   "Monitor0"
        VendorName   "Monitor Vendor"
        ModelName    "Monitor Model"
        HorizSync    28-64
        VertRefresh  60
      EndSection

#. Include a ``DefaultDepth`` in the ``Screen`` section:

   .. code-block:: xorg.conf
      :emphasize-lines: 5

      Section "Screen"
        Identifier    "Screen0"
        Device        "Card0"
        Monitor       "Monitor0"
        DefaultDepth  24
      EndSection

#. Add an extra ``Modes`` to the line in the ``Display`` sub-section:

   .. code-block:: xorg.conf
      :emphasize-lines: 4

      SubSection "Display"
        Viewport  0 0
        Depth     24
        Modes     "1024x768" "800x600"
      EndSubSection

#. Also, add the default Font paths in the ``Files`` section:

   .. code-block:: xorg.conf

      Section "Files"
        ModulePath  "/usr/lib/X11/modules"
        FontPath    "/usr/share/fonts/X11/misc/"
        FontPath    "/usr/share/fonts/X11/TTF/"
        FontPath    "/usr/share/fonts/X11/OTF"
        FontPath    "/usr/share/fonts/X11/Type1/"
        FontPath    "/usr/share/fonts/X11/100dpi/"
        FontPath    "/usr/share/fonts/X11/75dpi/"
        FontPath    "/usr/share/fonts/truetype/ttf-dejavu"
      EndSection

  * If you have DRI/DRM enabled, it may be easier to change its permission so all users can use it, rather than adding each to the ``video`` group.
    Append the following section:

    .. code-block:: xorg.conf

       Section "DRI"
         Mode 0666
       EndSection

.. tip::
   You can use :program:`xrandr` to identify your monitor(s).
   This utility is in the :program:`xorg-xrandr` package:

   .. code-block:: console

      # tazpkg get-install xorg-xrandr

   See the `Debian RandR 1.2 Wiki <http://wiki.debian.org/XStrikeForce/HowToRandR12>`_ for more information.


Restarting Xorg
^^^^^^^^^^^^^^^

For changes to take effect, you need to restart Xorg.
This can be done by logging out of your session and back in again.
Choose :guilabel:`Logout` from the Menu and select the :guilabel:`Logout X session` button.
If you see the SLiM log-in manager, Xorg has restarted successfully!


Using XVesa and Generic Drivers (TinyX)
---------------------------------------

The `SliTaz-3.0-xvesa.iso <http://mirror.slitaz.org/iso/3.0/flavors/slitaz-3.0-xvesa.iso>`_ uses the XVesa system instead of Xorg, which offers a more generic driver at the cost of performance.
It boots on nearly all computers and laptops, but can only display a 4:3 resolution.

To find out which resolutions are possible with your card, type:

.. code-block:: console

   # Xvesa -listmodes

Here's a selection of resolutions for an example Intel Atom Board::

  VBE version 3.0 (Intel(r) 82945GM Chipset Family Graphics Chip Accelerated VGA BIOS)
  DAC is switchable, controller is VGA compatible, RAMDAC causes snow
  Total memory: 7872 kilobytes
  0x015A: 1600x1200x24 TrueColor [8:8:8:8]
  0x011B: 1280x1024x24 TrueColor [8:8:8:8]
  0x0118: 1024x768x24 TrueColor [8:8:8:8]
  0x0112: 640x480x24 TrueColor [8:8:8:8]
  0x0115: 800x600x24 TrueColor [8:8:8:8]

This output shows widescreen resolutions cannot be displayed with XVesa.

If X does not start with the default SliTaz ISO, you could use the XVesa ISO to install your default Xorg driver.
When you are asked for your resolution at boot-time, scroll down the window and find the option to install your Xorg driver *before* XVesa is started.
However, don't use :program:`tazx` and :command:`tazhw setup ati or nv` on the XVesa ISO to install your Xorg driver because you will end up with a blank and confused screen!
