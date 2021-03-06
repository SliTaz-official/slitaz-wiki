.. http://doc.slitaz.org/en:handbook:security
.. en/handbook/security.txt · Last modified: 2010/08/17 22:00 by linea

.. _handbook security:

SliTaz and System Security
==========================

:author: jozee, linea


Security Policy
---------------

SliTaz has given a lot of consideration to system security.
Applications are tested for many months before being included in the distribution.
At boot time, a minimum of services are launched by the rc scripts.
For a complete lists of daemons enabled, you can look at the ``RUN_DAEMONS`` variable in the :file:`/etc/rcS.conf` configuration file:

.. code-block:: console

   $ cat /etc/rcS.conf | grep RUN_DAEMONS

To view the actual processes, their PID and memory usage, you can use the :command:`ps` command or the :command:`htop` utility:

.. code-block:: console

   $ ps
   $ htop


Root — The system administrator
-------------------------------

In a GNU/Linux system, the **root** user is the system administrator.
**root** has all the rights to the system files and that of the users.
It is advisable never to log in as **root** by using the command :command:`su` followed by the password to obtain absolute rights over the system.
Never log in as **root** and surf the internet for example.
This allows you to create a double barrier in the case of an attack or intrusion after a download and makes it harder for a cracker to take control of your machine — first he must crack your password and then crack the **root** password of the system administrator.

A GNU/Linux system has secured at least two users, one to work and another to administer, configure or update the system (**root**).
It's also advisable to entrust the administration of the system to a person.


Passwords
---------

By default the SliTaz user *tux* doesn't have a password and the system administrator **root** comes with the password (``root``).
You can easily change these by using the :command:`passwd` command:

.. code-block:: console

   $ passwd
   # passwd


Busybox
-------

The file :file:`busybox.conf` configures the applets and their respective rights.
On the SliTaz LiveCD the commands :command:`su`, :command:`passwd`, :command:`loadkmap`, :command:`mount`, :command:`reboot` and :command:`halt` can be initiated by all users — the owner and group of these commands is **root** (``* = ssx root.root``).
The :file:`busybox.conf` file is readable by **root**, using the rights ``600``.
Note that the :command:`passwd` command will not allow users to change their own password if it is not ``ssx``.


LightTPD web server
-------------------

On SliTaz the LightTPD web server is enabled by default at system startup, if you don't intend to use SliTaz in a server environment, you can safely disable it by removing it from the ``RUN_DAEMONS`` variable in the :file:`/etc/rcS.conf` configuration file or to stop it manually:

.. code-block:: console

   # /etc/init.d/lighttpd stop


SSH Server
----------

This small section is a compliment to the Secure SHell (SSH) page.
On SliTaz the :program:`Dropbear` SSH server is not run by default, we must add it to the variable ``RUN_DAEMONS`` in the configuration file :file:`/etc/rcS.conf` for it to be enabled at system boot.
Or to start the server manually:

.. code-block:: console

   # /etc/init.d/dropbear start

By default, :program:`Dropbear` is launched with the following options:

-w  Disallow root logins.
-g  Disallow logins for root password.

You can add new options by editing the daemons configuration file :file:`/etc/daemons.conf`.
For all options, you can type: :command:`dropbear -h`.


Pscan — Ports scanner
---------------------

:program:`Pscan` is a small utility of the Busybox project that scans the ports of your machine.
You can use :program:`pscan` to scan the localhost or a remote host using the name or IP address of the machine.
:program:`Pscan` will test all the ports from 1 – 1024 by default and list those that are open, their protocol and associated service (ssh, www, etc):

.. code-block:: console

   $ pscan localhost
