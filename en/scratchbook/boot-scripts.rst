.. http://doc.slitaz.org/en:scratchbook:boot-scripts
.. en/scratchbook/boot-scripts.txt · Last modified: 2011/04/23 23:08 by domcox

.. _scratchbook boot scripts:

Boot scripts
============

The startup and shutdown scripts with their configuration files.


SliTaz and startup
------------------

SliTaz does not use a level of execution (runlevel), the system is initialized via a primary script and its main configuration file.
This script itself launches some other smaller scripts which deal with the internationalization or the commands placed for the system to start.


:file:`/etc/init.d/*` — Directory of scripts and daemons
--------------------------------------------------------

The directory :file:`/etc/init.d` contains all of the rc scripts, scripts finishing with :file:`.sh` are simple shell scripts and daemons such as :file:`dropbear` or :file:`lighttpd` are scripts that launch a service.
The daemon scripts can start, stop or restart through the command:

.. code-block:: console

   # /etc/init.d/daemon [start|stop|restart]

On SliTaz you will find a :file:`/etc/init.d/README` describing the basic function of rc scripts.
Also note that all startup scripts and daemons can call upon the :file:`/etc/init.d/rc.functions` file.
This file makes it possible to include various functions in rc scripts.
SliTaz uses a function ``status`` to check whether the previous command has succeeded (0) or not.


:file:`/etc/init.d/rcS` — Primary initialization script
-------------------------------------------------------

The :file:`/etc/init.d/rcS` script configures all the basic services and initializes the base system.
It begins by mounting the filesystems and starts services like ``syslogd``, ``klogd``, ``mdev`` and cleans up the system and so on.
It uses the configuration file :file:`/etc/rcS.conf` to locate which daemons and scripts to launch at startup.
You can browse the script to know which commands are executed:

.. code-block:: console

   # nano rootfs/etc/init.d/rcS


Specific scripts and daemons
----------------------------


.. rubric:: :file:`bootopts.sh` — LiveCD mode options

This script is used to configure the LiveCD options passed at boot time and is readable via the :file:`/proc/cmdline` file.
This is the script that allows you to use a USB key or external hard disk :file:`/home` partition with the option ``home=usb`` or ``home=sda[1-9]`` or directly specify the language and keyboard parameters.


.. rubric:: :file:`network.sh` — Initializing the network

This script searches the :file:`network.sh` configuration file :file:`/etc/network.conf` for the network interface to use; if one wants to launch the DHCP client (or not) or if you want to use a fixed (static) IP.
On SliTaz the :file:`/etc/init.d/network.sh` script configures the network interfaces to start using the information contained in :file:`/etc/network.conf`.
If the variable ``$DHCP`` is equal to ``yes``, then the :file:`/etc/init.d/network.sh` script launches the DHCP client on the ``$INTERFACE`` interface.


.. rubric:: :file:`i18n.sh` — Internationalization

SliTaz backs up the configuration of the default locale in :file:`/etc/locale.conf` which is read by :file:`/etc/profile` at each login.
The :file:`/etc/locale.conf` is generated during boot time thanks to the :file:`/etc/i18n.sh` script.
This script launches the :command:`tazlocale` application if :file:`/etc/locale.conf` doesn't exist.
We use the same process for the keyboard layout using :command:`tazkmap` and the :file:`/etc/kmap.conf` configuration file.
Both applications are installed and located in :file:`/sbin` and use :program:`dialog` and the :program:`ncurses` library.
The script also checks whether the configuration file for the time zone :file:`/etc/TZ` exists, otherwise it creates one relying on the keyboard configuration.


.. rubric:: :file:`local.sh` — Local commands

The :file:`/etc/init.d/local.sh` script allows the system administrator to add local commands to be executed at boot.
Example:

.. code-block:: shell

   #!/bin/sh
   # /etc/init.d/local.sh: Local startup commands.
   # All commands here will be executed at boot time.
   #
   . /etc/init.d/rc.functions
   
   echo "Starting local startup commands... "


.. rubric:: file:`rc.shutdown`

This script is invoked by :file:`/etc/inittab` during system shutdown.
It also stops all daemons via the variable ``RUN_DAEMONS`` in the primary :file:`/etc/rcS.conf` configuration file.


:file:`/etc/inittab` — Configuration file init
----------------------------------------------

The first file read by the Kernel at boot.
It defines the initialization script (:file:`/etc/init.d/rcS`), shells (ttys) and actions in the event of a reboot or disruption.
You will find a complete example with accompanying notes in :ref:`cookbook slitaztools`:

.. code-block:: shell

   # /etc/inittab: init configuration for SliTaz GNU/Linux.
   # Boot-time system configuration/initialization script.
   #
   ::sysinit:/etc/init.d/rcS
   
   # /sbin/getty respawn shell invocations for selected ttys.
   tty1::respawn:/sbin/getty 38400 tty1
   tty2::respawn:/sbin/getty 38400 tty2
   tty3::respawn:/sbin/getty 38400 tty3
   tty4::respawn:/sbin/getty 38400 tty4
   tty5::respawn:/sbin/getty 38400 tty5
   tty6::respawn:/sbin/getty 38400 tty6
   
   # Stuff to do when restarting the init
   # process, or before rebooting.
   ::restart:/etc/init.d/rc.shutdown
   ::restart:/sbin/init
   ::ctrlaltdel:/sbin/reboot
   ::shutdown:/etc/init.d/rc.shutdown


.. rubric:: Following chapter

The next chapter continues on with the :ref:`scratchbook x window system`.
