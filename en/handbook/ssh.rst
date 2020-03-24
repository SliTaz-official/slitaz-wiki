.. http://doc.slitaz.org/en:handbook:ssh
.. en/handbook/ssh.txt · Last modified: 2015/11/12 19:45 by linea

.. _handbook ssh:

Secure SHell (SSH)
==================

:author: jozee, linea, bellard


About Dropbear
--------------

Control and administer remotely with the :program:`Dropbear` SSH secure server.
:program:`Dropbear` is a small SSH client/server supporting SSH 2.
It's compatible with :program:`OpenSSH` and uses :file:`~/.ssh/authorized_keys` for the management of public keys.
:program:`Dropbear` also provides its own version of :program:`scp`, allowing you to copy files between machines in a secure manner.

Project website: http://matt.ucc.asn.au/dropbear/dropbear.html


Connecting to a remote host with :program:`dbclient`
----------------------------------------------------

The configuration files for the SSH client are located in the :file:`~/.ssh` directory of each user, this contains the :file:`authorized_keys` and :file:`known_hosts` files.
The directory :file:`~/.ssh` and :file:`known_hosts` file are automatically created the first time you run the :program:`Dropbear` client (:program:`dbclient`).

To connect to a remote host employing the user and machine name:

.. code-block:: console

   $ dbclient user@machine.org

You can also connect using the IP address of the machine:

.. code-block:: console

   $ dbclient user@192.168.0.2


Transfer of remote files with :program:`scp`
--------------------------------------------

To copy a file from one computer to another, :program:`scp` can be utilized in the following ways.
To copy a file named :file:`page.html` to a remote directory of the user (don't forget the ``:`` after the machine name or IP address):

.. code-block:: console

   $ scp page.html user@machine.org:path/remote/directory

Copy a file from a remote machine to your local machine:

.. code-block:: console

   $ scp user@machine.org:path/remote/directory/page.html /path/your/directory


Generate RSA/DSS keys with :program:`dropbearkey`
-------------------------------------------------

:program:`Dropbear` provides :program:`dropbearkey` to generate the protected RSA and DSS keys.
Note that when you start the server for the first time, secure keys will be automatically generated if they don't already exist.
You can use :program:`dropbearkey` with the following arguments:

.. code-block:: console

   # dropbearkey -t rsa -f /etc/dropbear/dropbear_rsa_host_key
   # dropbearkey -t dss -f /etc/dropbear/dropbear_dss_host_key


Start, stop, restart the SSH server
-----------------------------------

By default SliTaz will not start the SSH server at boot.
To be launched automatically, ``dropbear`` must be added to the variable ``RUN_DAEMONS`` in the :file:`/etc/rcS.conf` file.
To start, stop or restart the SSH server, use the following commands: :command:`/etc/init.d/dropbear [start|stop|restart]`.
Example to start the server:

.. code-block:: console

   # /etc/init.d/dropbear start

Note that the server supports the passing of various options when launched.
To change the default values, simply modify the daemons configuration file :file:`/etc/daemons.conf`.


Dropbear and the X server
-------------------------

:program:`Dropbear` supports X11 tunneling on the server side only.
The client :program:`dbclient` has no support for X11 tunneling.
SliTaz provides a tiny shell script named :file:`/usr/bin/sshx` to do the work.
It opens a terminal with remote X11 protocol support if the environment variable ``DISPLAY`` is set.
You can launch any remote X windows application on this terminal.


Dropbear and VNC
----------------

SliTaz provides a tiny VNC client named :file:`/bin/fbvnc`.
The VNC connections are not secure (neither encryption nor authentication).
A tiny shell script named :file:`/usr/bin/sshfbvnc` move the VNC connection to a SSH tunnel ending at the localhost interface of the remote VNC server.
You will have an encrypted connection and authentication for your VNC sessions.
