.. http://doc.slitaz.org/en:guides:chroot
.. en/guides/chroot.txt · Last modified: 2010/09/24 21:17 by linea

.. _chroot:

Chroot
======

:author: gokhlayeh, linea, jozee

This guide explain how to setup a *chroot* to *cook* packages in a separate environment.
The *chroot* can be built on a USB or HDD device to save RAM when using a live session.
There is also a script which removes any packages installed during *cooking* on exit in order to keep the *chroot* light and also checks the ``build_depends``.


With :program:`Tazdev`
----------------------

Over time the SliTaz developers created tools for the automation of various tasks.
The :program:`slitaz-dev-tools` package provides the :program:`tazdev` utility and its configuration file :file:`/etc/slitaz/tazdev.conf`; it can create a *chroot* to use:

.. code-block:: console

   # tazpkg get-install slitaz-dev-tools
   # tazdev gen-chroot
   # tazdev chroot

By default the *chroot* is created in :file:`/home/slitaz/cooking/chroot` and is slitaz-based.
For more info and available commands you can use :command:`tazdev usage` and/or take a look at the configuration file.


Create the chroot
-----------------

.. code-block:: shell

   #!/bin/sh
   mkdir /home/chroot
   # You can mount a device to /home/chroot
   # with mountbox or mount
   tazpkg get-install busybox --root="/home/chroot"
   echo "No" | tazpkg get-install bash --root="/home/chroot"
   tazpkg get-install slitaz-toolchain --root="/home/chroot"
   tazpkg get-install tazwok --root="/home/chroot"
   tazpkg get-install tazpkg --root="/home/chroot"
   tazpkg get-install lzma --root="/home/chroot"
   mkdir /home/chroot/home/slitaz

Note, you can keep these lines in a script file if needed.
Just add ``#!/bin/sh`` to the first line and make executable with:

.. code-block:: console

   # chmod +x script_file


Add a script file to automate some actions
------------------------------------------

.. code-block:: console

   # cat > /home/chroot/chroot_script.sh << "EOF"

.. code-block:: shell

   #!/bin/sh
   /bin/sh --login
   for pkg in $(cat /var/log/tazpkg.log | grep -v Removed | sed 's/\(.*\)\(- Installed - \)\(.*\)\( (.*\)/\3/'); do
   	echo "y" | tazpkg remove $pkg
   done
   rm /var/log/tazpkg.log

.. code-block:: console

   EOF
   
   # chmod +x "/home/chroot/chroot_script.sh"

Note, :command:`/bin/sh --login` logs you into the *chrooted* environment.
The commands after that auto-remove any packages added when *cooking* on exit.
You can hack this file to execute various automated actions when entering and exiting the *chroot*.


Add a script to mount and umount *chroot*
-----------------------------------------

.. code-block:: console

   # cat > /usr/bin/tazchroot << "EOF"

.. code-block:: shell

   #!/bin/sh
   cat /etc/resolv.conf > /home/chroot/etc/resolv.conf
   if [ ! -d "/home/chroot/proc/1" ]; then
   	echo "Mounting virtual filesystems..."
   	mount -t proc   proc   /home/chroot/proc
   	mount -t sysfs  sysfs  /home/chroot/sys
   	mount -t devpts devpts /home/chroot/dev/pts
   	mount -t tmpfs  shm    /home/chroot/dev/shm
   	mount /home/slitaz /home/chroot/home/slitaz
   	chroot /home/chroot ./chroot_script.sh
   	until [ "$ps" = "2" ]; do
   		echo "Waiting for the end of all other chroot processes..."
   		ps=$(ps | grep `basename $0` | grep -v grep | grep -v basename | wc -l)
   		sleep 1
   	done
   	umount /home/chroot/home/slitaz
   	umount /home/chroot/dev/shm
   	umount /home/chroot/dev/pts
   	umount /home/chroot/sys
   	umount /home/chroot/proc
   else
   	echo "The chroot is already mounted"
   fi

.. code-block:: console

   EOF
   
   chmod +x /usr/bin/tazchroot

Note, this script mounts :file:`/home/slitaz` in your *chroot*, so you can use :program:`tazwok` as if it was in your normal environment.
