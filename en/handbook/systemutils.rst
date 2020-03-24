.. http://doc.slitaz.org/en:handbook:systemutils
.. en/handbook/systemutils.txt · Last modified: 2016/11/27 16:44 by hgt

.. _handbook systemutils:

System Administration
=====================

:author: jozee, linea, emgi, hgt


Devices and disk access
-----------------------

With Linux your disks and USB media are seen as devices.
To access them you must first mount a device on a mount point (directory).
On SliTaz you can graphically mount devices by using :program:`mountbox` or the command line.
To mount the first disk of a local hard disk on :file:`/mnt/disk`:

.. code-block:: console

   # mkdir -p /mnt/disk
   # mount /dev/hda1 /mnt/disk

To mount a CD-ROM or an USB media you should use mount points located in :file:`/media`.
Note that for a CD-ROM, you just have to specify the device path.
For a flash key, the mount point already exists:

.. code-block:: console

   # mount /dev/cdrom
   # mount /dev/sda1 /media/flash


.. rubric:: NTFS filesystem

If you need read/write access to Windows NTFS filesystems you must install a few additional packages from the mirror.
The :program:`ntfs-3g` driver provides stable access to NTFS partitions and :program:`ntfsprogs` provides manipulation tools dependent on FUSE.
Note that you can format, move or resize NTFS partitions graphically with :program:`Gparted`.


.. rubric:: NFS

NFS (Network File System) is the native Unix/Linux method for sharing file systems.
In this respect its function is similar to SAMBA.
The most popular version is still NFSv3 which is able to use either UDP or TCP as the network protocol.
The older NFSv2 was only capable of using UDP.
On a local LAN, UDP is still the fastest protocol; TCP is to be preferred when the machines are connected over a WAN.
NFSv3 has been superseded by NFS version4 which has notable improvements (security) over v3 but its configuration has become a lot more complex as a result.

An NFS server exports a part of it's file system; i.e. makes it available on the network.
The server is configured with details about client IP addresses or host names and can restrict their access to the file system.
Access can be read-only, read-write or no access at all.
An NFS client simply mounts the exported file systems as if they were local devices.

The NFS software in SliTaz makes it possible to run both as a server or a client.
To start the NFS processes, you need to run the init script in :file:`/etc/init.d/`.

.. code-block:: console

   # /etc/init.d/nfsd start|stop

.. tip::
   The NFS daemons must be running also when the machine acts as a client.


.. rubric:: NFS Software Installation

To start using NFS, the following packages are required:

.. code-block:: console

   # tazpkg -l | grep nfs
   linux-nfsd              2.6.37            base-system
   nfs-utils               1.2.2             system-tools

Install them using:

.. code-block:: console

   # tazpkg -gi linux-nfsd
   # tazpkg -gi nfs-utils


.. rubric:: :file:`/etc/exports`

Here is a sample exports file:

.. code-block:: shell

   # /etc/exports: the access control list for filesystems which may be
   #               exported to NFS clients.  See exports(5).
   #
   # Example for NFSv2 and NFSv3:
   # /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
   #
   # Example for NFSv4:
   # /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
   # /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
   #
   /usb1           192.168.1.0/24(rw,sync,no_subtree_check)

The server is only exporting one drive: usb1.
Clients must be in the 192.168.1.0/24 network and they have read-write access.

To use nfs on the client; all you need to do is start nfsd and mount the share:

.. code-block:: console

   # /etc/init.d/nfsd start
   # mount server:/usb1 /mnt/usbdrive1

Please note the specific format for nfs shares **servername** colon **slash-mountpoint**.
Naturally you must also make sure that the specified mount point (directory) exists on the client.


Users, groups and passwords
---------------------------

To manage users and groups on your SliTaz system you must use the command line, but file permissions can be changed graphically using the :program:`PCmanFM` file manager.
To add or remove users and groups you must be root.
Root can also change all user passwords and a single user can only change his/her own password.
To add or remove a user named linux:

.. code-block:: console

   # adduser linux
   # deluser linux


.. rubric:: Manipulating users & group membership

Linux groups are a mechanism to manage a collection of computer system users.
All Linux users have a user ID and a group ID and a unique numerical identification number called a userid (UID) and a groupid (GID) respectively.
Groups can be assigned to logically tie users together for a common security, privilege and access purpose.
It is the foundation of Linux security and access.
Access to files and devices may be granted based on a user ID or a group ID.
This mechanism is the same for all of Linux but the way it is configured varies per distribution.
Sometimes additional or different commands are used, like for example :command:`usermod`, :command:`chgrp`, :command:`useradd` or :command:`groupadd`.
Below is an overview of how to handle users, groups and group membership on Slitaz.

The SliTaz way is using only four commands.
Simple & Elegant. ;-)

.. code-block:: console

   # adduser    <username>              # adds a user
   # deluser    <username>              # deletes a user
   # addgroup   <groupname>             # adds a group
   # delgroup   <groupname>             # deletes a group
   # addgroup   <username>  <groupname> # adds a user to a group
   # adduser -G <groupname> <username>  # adds the user to an additional group
   # delgroup   <username>  <groupname> # deletes a user from a group

Any user can be member of any group and the combination of user & group permissions allows for granular access to system resources.


.. rubric:: :command:`passwd`

To change the current user's password or change the password of a specific user, you must use the :command:`passwd` command:

.. code-block:: console

   $ passwd
   # passwd username


.. rubric:: ``audio`` group

If you want a new user to be able to listen to music he must be in the ``audio`` group.
To add an existing user to the ``audio`` group:

.. code-block:: console

   # adduser -G audio user_name


Language and keyboard layout
----------------------------

SliTaz saves the configuration of the default locale in :file:`/etc/locale.conf` which is read by :file:`/etc/profile` on each login and the keyboard setting is stored in :file:`/etc/keymap.conf`.
These two files can be edited with your favorite editor or configured respectively with :program:`tazlocale` and :program:`tazkeymap`.
You can modify the settings you chose on the first boot by typing as root administrator:

.. code-block:: console

   # tazlocale

Or:

.. code-block:: console

   # tazkeymap

To check all available locales or your current configuration, you can use the command :command:`locale` as a single user or root (C for English):

.. code-block:: console

   $ locale -a
   $ locale


Custom SHell
------------

SliTaz uses the ash shell linked to sh provided by busybox.
Ash is light, fast and standards compliant.
To change the default shell for a user you can edit the :file:`/etc/passwd` file using the corresponding line.
After you login, :file:`/etc/profile` is read first and then the user file :file:`~/.profile`.
You can edit these files with a text editor to configure the language, any aliases, etc.


.. rubric:: Example: :file:`~/.profile`

.. code-block:: shell

   # ~/.profile: executed by Bourne-compatible login shells.
   #
   
   # Aliases.
   alias ls='ls -F'
   alias df='df -h'
   
   # Env variables.
   export EDITOR=nano


BASH Shell
----------

On SliTaz you have the ``ash`` and ``sh`` shell with a link to :program:`Ash`, this shell is provided by Busybox.
If you wish to use the BASH (Bourne Again SHell), first as root install :program:`bash`, copy the :file:`.profile` found in your home directory and rename it :file:`.bashrc`, then edit the :file:`/etc/passwd` file with your favorite text editor and change your shell to :file:`/bin/bash`

.. code-block:: console

   # tazpkg get-install bash
   $ cp ~/.profile ~/.bashrc
   # nano /etc/passwd

The next time you login BASH will be your default shell, you can confirm this by typing :command:`env` on the command line.


Editors
-------

Busybox supplies a clone of :program:`vi` for normal text editing, but it does have its limitations.
You can install the full :program:`vim` editor with the command:

.. code-block:: console

   # tazpkg get-install vim

Or alternatively if you prefer :program:`emacs`, SliTaz offers a tiny version:

.. code-block:: console

   # tazpkg get-install emacs


:command:`sudo`
---------------

The :command:`sudo` command can be applied on SliTaz:

.. code-block:: console

   # tazpkg get-install sudo

Note that the configuration file :file:`/etc/sudoers`, should always be edited by the :command:`visudo` command which locks the file and checks for errors.


System Time
-----------

To check the current system time, you can simply type:

.. code-block:: console

   $ date


.. rubric:: TimeZone

On SliTaz, the timezone configuration file is saved in :file:`/etc/TZ`, you can edit this with your favorite text editor or simply :command:`echo` the changes.
To view the available timezones, you can look in the :file:`/usr/share/zoneinfo` directory.
Here's an example using the timezone Europe/London:

.. code-block:: console

   # echo "Europe/London" > /etc/TZ


.. rubric:: :command:`rdate`

To synchronize the system clock with a network time server, you can (as root) use the :command:`rdate -s` command:

.. code-block:: console

   # rdate -s tick.greyware.com

To display the time on the remote server, use the :command:`rdate -p` command.

.. code-block:: console

   $ rdate -p tick.greyware.com


.. rubric:: Using NTP

NTP is a protocol to synchronize the time on many different systems via a network.
NTP is a client-server application which uses UDP port 123.
This section describes how to configure your system as an NTP client deriving its time from the Internet.
There are many servers available on the Internet which provide an NTP service. 

.. tip::
   Experience has shown that NTP servers seldom have a high availability, rather the opposite!
   This means it may not be such a good idea to configure a particular server as your time source, not even two or three.
   After a while you may find out that none of them is active any more and your time is drifting freely!
   A better way is to use the service from `ntp.org <http://www.ntp.org/>`_.
   They provide pools of NTP servers per country or region.
   Selecting one of these provides a more reliable connection to an NTP time source.


Although SliTaz is a small distribution, it provides several NTP implementations.
Most notably:

#. :command:`busybox ntpd` (included in the base installation).
   Using :command:`busybox ntpd` from the command line:

   .. code-block:: console

      # busybox ntpd -p nl.pool.ntp.org

   or

   .. code-block:: console

      # ntpd -p nl.pool.ntp.org

#. :program:`ntp.tazpkg` (install from packages repository).
   To install :program:`ntp.tazpkg`:

   .. code-block:: console

      # tazpkg -gi ntp

   This package includes a decent set of NTP related binaries + the config file :file:`/etc/init.d/ntp`

   .. code-block:: console

      # tazpkg list-files ntp
      
      Installed files with: ntp
      =========================
      /etc/init.d/ntp
      /etc/ntp.conf
      /usr/bin/ntpd
      /usr/bin/ntpdate
      /usr/bin/ntpdc
      /usr/bin/ntp-keygen
      /usr/bin/ntpq
      /usr/bin/ntptime
      /usr/bin/ntptrace
      /usr/bin/ntp-wait
      /usr/bin/sntp
      /usr/bin/tickadj

Be aware that SliTaz has several NTP daemons available.
We have the Busybox app but also the :program:`ntp` package.
Both provide virtually the same functionality.
If you have limited resources, the busybox version can provide all you need.
If you want all the diagnostic stuff as well, you should rather go for installing :program:`ntp.tazpkg`.


.. rubric:: Starting :command:`ntpd` at boot

Probably the easiest way to start :command:`busybox ntpd` at boot is to add an entry like above to :file:`/etc/init.d/local.sh`.
The explanation below focuses on :program:`ntp.tazpkg`.
It is unclear which one was intended by the developers to be started by the Server Manager.
This can be somewhat confusing.
The verified way to configure the NTP daemon is to use the command line as detailed below.

To start :file:`/usr/bin/ntpd` (:program:`ntp.tazpkg`) at boot:

#. Make sure to install the package as shown above ;-).

#. Edit :file:`/etc/daemons.conf` as follows.
   Add one line at the end: 

   .. code-block:: shell

      NTP_OPTIONS="-p xx.pool.ntp.org"

   (where xx = country.)

   .. tip::
      The NTP daemon gets it options from :file:`/etc/daemons.conf`.
      The configuration file :file:`/etc/ntp.conf` which is referred to by the Server Manager seems to be unused and may be deleted.

#. Edit :file:`/etc/rcS.conf` as follows.
   On the line with daemons to start, add ``ntp`` to the list:

   .. code-block:: shell

      RUN_DAEMONS="inetd dbus hald slim firewall httpd ntp "

   .. tip::
      Make sure to enter just ``ntp``, not ``ntpd``!
      The name is a reference to :file:`/etc/init.d/ntp`

   These are the required steps.
   After completion, you may reboot to verify everything is indeed working as expected.

Use the following to check if the daemon is running:

.. code-block:: console

   $ ps -ef | grep ntpd
    1934 root       0:00 /usr/bin/ntpd -p nl.pool.ntp.org
    2193 root       0:00 grep ntpd

In this example, the first line shows the process we want to see.

.. tip::
   Use :file:`/etc/init.d/ntp` {start | stop | restart} to control the NTP daemon or specify the full path (:file:`/usr/bin/ntpd`).
   Using :command:`ntpd` on the command line without the full path causes the busybox version to be invoked.


.. rubric:: Verifying :program:`ntpd` operation

You may use :command:`ntpq` to verify your connection to NTP servers

.. code-block:: console

   # ntpq -p nl.pool.ntp.org
        remote           refid      st t when poll reach delay offset jitter
   =========================================================================
   *ntp0.nl.uu.net  .PPS.            1 u  632 1024  377  2.700  0.233  0.096
   +ntp1.nl.uu.net  .PPS.            1 u  504 1024  377  1.742  0.356 41.789
   -chime1.surfnet. 194.171.167.130  2 u  298 1024  377  0.677  0.102  0.134
   +chime4.surfnet. .PPS.            1 u  422 1024  367  9.652 -2.669  0.366
    tt165.ripe.net  .STEP.          16 u    - 1024    0  0.000  0.000  0.000

The ``*`` at the start of a line indicates the server you are currently synchronized to.

The column "st" shows the *stratum* or quality of the time source.
1 is best, 16 means unavailable.
Important to check are the columns "reach" and those behind.
Reach should be 377, everything else means polls were missed.
Your daemon should be running for a while to get reliable output.


.. rubric:: :program:`hwclock`

:program:`hwclock` allows you to synchronize the time of your hardware clock to the system clock or vice versa.

Synchronize the system clock to the hardware clock (``--utc = universal time``, ``-l = local time``):

.. code-block:: console

   # hwclock -w --utc

Synchronize the hardware clock to the system clock:

.. code-block:: console

   # hwclock -s --utc

.. code-block:: shell

   hwclock -u, --utc | -l, --localtime

Indicates that the Hardware Clock is kept in Coordinated Universal Time or local time, respectively.
It is your choice whether to keep your clock in UTC or local time, but nothing in the clock tells which you've chosen.
So this option is how you give that information to :program:`hwclock`.
If you specify the wrong one of these options (or specify neither and take a wrong default), both setting and querying of the Hardware Clock will be messed up.

.. note::
   On SliTaz, :command:`hwclock` must always be set to UTC.
   The result of a non-UTC hardware clock setting is an incorrect time for your timezone.


.. rubric:: Synchronizing the :program:`hwclock` with NTP

There are several ways to set the hardware clock to NTP time:

.. code-block:: console

   # busybox ntpd -dnqp nl.pool.ntp.org && hwclock -w -u

or:

.. code-block:: console

   # ntpdate -u nl.pool.ntp.org && hwclock -w -u

Alternative three: (quite old, may not work on all servers)

.. code-block:: console

   # rdate -s nl.pool.ntp.org && hwclock -w -u

Note that in all examples we used the ``-u`` option to set :program:`hwclock` to UTC time.

Further reading: http://linux.die.net/man/8/hwclock


Execute scheduled commands
--------------------------

The daemon :program:`crond` allows you to run commands automatically at a scheduled specific date or time.
This is very useful for routine tasks such as system administration.
The directory :program:`cron` uses is :file:`/var/spool/cron/crontabs`.

Each user on the system can have his/her own tasks, they are defined in the file: :file:`/var/spool/cron/crontabs/{user}`.
This file can be created oder modified by any user with the :command:`crontab -e` command, using the default text editor.
The crontab utility allows you (amongst other things), to list the tasks specific to the user.

.. code-block:: console

   # crontab -l # To list the crontab for user root.

or:

.. code-block:: console

   # crontab -l -u tux # To list the crontab for another user.

The syntax of the crontab files is as follows::

  mm hh dd MMM DDD command > log

We will create a file with root privileges and test the daemon :program:`crond` with a task performed every minute — writing the date to a file :file:`/tmp/crond.test`, using GNU :program:`nano` (:kbd:`Ctrl`\ +\ :kbd:`X` to save & exit):

.. code-block:: console

   # nano /var/spool/cron/crontabs/root

Add the line::

  * * * * * date >> /tmp/crond.test

Launch :program:`crond` with the option ``-b`` (background), configured via :file:`/etc/daemons.conf` and using the startup script:

.. code-block:: console

   # /etc/init.d/crond start

You can wait a few minutes and view the contents of the file: :file:`/tmp/crond.test`…
OK:

.. code-block:: console

   # cat /tmp/crond.test

To stop or restart the daemon :program:`crond`:

.. code-block:: console

   # /etc/init.d/crond stop

or:

.. code-block:: console

   # /etc/init.d/crond restart


.. rubric:: Invoke the daemon crond on every boot

To launch the daemon :program:`crond` each time you boot the system, just add it to the variable ``START_DAEMONS`` in the configuration file :file:`/etc/rcS.conf`, either before or after the web server or SSH server.


Add commands to be executed at boot
-----------------------------------

During the boot process, various scripts are executed to configure services, such as the start of the web server, networking, etc.
On SliTaz there is a script :file:`/etc/init.d/local.sh` which allows you to add commands to be launched at system startup.
You can also create new scripts in :file:`/etc/init.d`, their links in :file:`/etc/rc.scripts` for shell scripts and use :file:`/etc/rc.d` for links to the startup script daemon in :file:`/etc/rcS.conf`:

.. code-block:: console

   # nano /etc/init.d/local.sh
