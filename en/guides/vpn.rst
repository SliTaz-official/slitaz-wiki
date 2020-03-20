.. http://doc.slitaz.org/en:guides:vpn
.. en/guides/vpn.txt · Last modified: 2013/04/09 09:56 by bellard

.. _vpn:

Virtual Private Network
=======================

A VPN binds isolated networks with tunnels over the Internet network.
A tunnel encrypts and encapsulates network frames.

* frames are hidden (data and header, including routing info)
* encapsulation sets tunnel ends as source and destination
* only traffic volume is visible.
  But you can send dummy frames


Tunnel using SSH and PPP
------------------------

SliTaz can build such tunnels out of the box (without additional packages) with menu :menuselection:`System --> Netbox`.
You need on remote server:

* a user account
* a SSH access
* PPPD launch rights (group rights or *setuid* bit)
* remote stations know how to route frames to the tunnel (peer is the default router or is running routing protcols such as rip, ospf, …)


Tunnel setup
------------

* launch :menuselection:`System --> Netbox`
* select :guilabel:`VPN` tab
* fill :guilabel:`Peer` and update :guilabel:`Route(s)` fields.
  Routes are the local area networks on peer side you want to reach.
  You only need to update :guilabel:`Local IP` and :guilabel:`Remote IP` when you create multiple tunnels.
  They are tunnel end IP points
* click :guilabel:`Send Key` and enter password
* click :guilabel:`Connect`


Limitations
-----------

* This tunnel should not be used for real time traffic like VoIP.
  Based on TCP, the tunnel tries to avoid data loss instead of respecting timing.
  VoIP should use UDP based tunnels
* Only the SliTaz box can see the remote network.
  Other stations on the LAN can't, until you add your LAN routes to peer (assuming SliTaz is the default router or is running routing protcols)


OpenSSH
-------

The :program:`openssh` package is available on the mirror and provides TCP based tunnels:

.. code-block:: console

   # yes y | tazpkg get-install openssh

See http://man.openbsd.org/ssh#SSH_BASED_VIRTUAL_PRIVATE_NETWORKS


OpenVPN
-------

The :program:`openvpn` package is available on the mirror:

.. code-block:: console

   # yes y | tazpkg get-install openvpn

See documentation at http://openvpn.net/


Cisco EasyVPN
-------------

The :program:`vpnc` package is available on the mirror:

.. code-block:: console

   # yes y | tazpkg get-install vpnc

See documentation at http://www.unix-ag.uni-kl.de/~massar/vpnc/


Others VPN
----------

SliTaz packages search tool shows every VPN supported by SliTaz:
http://pkgs.slitaz.org/search.sh?tags=vpn
