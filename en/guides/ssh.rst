.. http://doc.slitaz.org/en:guides:ssh
.. en/guides/ssh.txt · Last modified: 2015/08/22 10:53 by linea

.. _ssh:

Dropbear SSH
============

:author: kultex, linea

:program:`Dropbear` is a small SSH server and client written by Matt Johnston.

https://matt.ucc.asn.au/dropbear/dropbear.html

By default SliTaz will not start the SSH server at boot.
To be launched automatically, ``dropbear`` must be added to the variable ``RUN_DAEMONS`` in the :file:`/etc/rcS.conf` file.
(Normally :program:`Dropbear` logs in :file:`/var/log/messages`, but in SliTaz it logs in :file:`/var/log/auth.log`)

More info: :ref:`handbook ssh`

A good doku of dropbear is on OpenWRT — http://wiki.openwrt.org/doc/uci/dropbear
