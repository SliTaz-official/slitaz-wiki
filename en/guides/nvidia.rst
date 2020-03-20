.. http://doc.slitaz.org/en:guides:nvidia
.. en/guides/nvidia.txt · Last modified: 2011/05/01 10:04 by domcox

.. _nvidia:

Install the latest non-free Nvidia driver
=========================================


Required Reading
----------------

* Handbook — `Xorg Installation <http://doc.slitaz.org/en:handbook:xwindow#xorg>`_
* Quickstart Guide — :ref:`kernel`


Prepare
-------

* Download the installer from the NVIDIA `website <http://www.nvidia.com/object/unix.html>`_.
  Latest version 190.53
* Install the Xorg server

  .. code-block:: console

     # tazpkg get-install xorg-server

* Install the :program:`linux-source` package and development tools.
  See :ref:`kernel`

  .. code-block:: console

     # tazpkg get-install linux-source


Install the driver
------------------

For this part you're going to need a pencil and paper as we now have to work in text mode without a X-server running.

When you're ready press :kbd:`Alt`\ +\ :kbd:`Ctrl`\ +\ :kbd:`Del`, you should now see a command line:

* Configure the X-server and copy the (generated) :file:`xorg.conf`

  .. code-block:: console

     # Xorg -configure
     # cp /root/xorg.conf.new /etc/X11/xorg.conf

* Prepare the Kernel

  .. code-block:: console

     # cd /usr/src/linux
     
     # make oldconfig && make prepare
     
     # make menuconfig  # (not required - but if you've come this far, you can take a peek)
     # make bzImage
     # make modules
     # make modules_install

* Now make executable and install the NVIDIA driver, change to directory you installed to

  .. code-block:: console

     # chmod +x NVIDIA-Linux-x86-177.80.pkg1.run
     # ./NVIDIA-Linux-x86-177.80.pkg1.run --kernel-source-path=/usr/src/linux

* Copy the Kernel Image to :file:`/boot`

  .. code-block:: console

     # cd /usr/src/linux
     # cp arch/x86/boot/bzImage /boot


Restart
-------

* Reboot into text mode

  .. code-block:: console

     # reboot                         # Hard drive users
     # tazusb writefs gzip && reboot  # USB users

  Don't forget to pass the ``screen=text`` option at startup, (it may well boot into text mode anyway, if not just press :kbd:`Alt`\ +\ :kbd:`Ctrl`\ +\ :kbd:`Del` again)

* Load the ``nvidia`` module

  .. code-block:: console

     # modprobe -v nvidia

* Edit SLiM configuration file to load Xorg server

  .. code-block:: console

     # vi /etc/slim.conf

  ::

    default_xserver     /usr/bin/Xorg
    #default_xserver     /usr/bin/Xvesa
    #xserver_arguments   -ac -shadow dpms +extension Composite -screen 1024x768x24

* Restart SLiM

  .. code-block:: console

     # /etc/init.d/slim start

Err… that's it

(If you want the ``nvidia`` module to persist, just add it to the ``LOAD_MODULES`` variable in :file:`/etc/rcS.conf`)
