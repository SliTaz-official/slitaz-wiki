.. http://doc.slitaz.org/en:guides:tor
.. en/guides/tor.txt · Last modified: 2017/02/09 20:46 by linea

.. _tor:

The Tor Installation Guide
==========================

This guide allows you to use the anonymous TOR network with most web browsers and command line tools such as :program:`wget`.
Begin by installing TOR:

.. code-block:: console

   # tazpkg get-install tor
   # tazpkg get-install privoxy
   # echo 'forward-socks5t /        127.0.0.1:9050   .' >> /etc/privoxy/config
   # sed -r '/RUN_DAEMONS/s/(" *)$/ privoxy\1/' -i /etc/rcS.conf 
   # /etc/init.d/privoxy start

.. compound::
   You can use :program:`Vidalia` to start and configure :program:`tor`, the proxy server is launched automatically when the SliTaz system boots.
   Once installed, you will find :program:`Vidalia` in the :menuselection:`Menu --> Internet`.
   To configure preferences via the proxy server with the following values:

   :Proxy address: 127.0.0.1
   :Port: 8118

   and the path to the configuration file: :file:`/home/{USER}/.config/torrc` (changing :file:`{USER}` by User name).

.. code-block:: console

   # tazpkg get-install vidalia
   # exit
   $ cp /etc/tor/torrc ~/.config

Use Bridges to get tor working if the above options are not working.

::

  # echo '
  UseBridges 1
  UpdateBridgesFromAuthority 1
  bridge 66.160.141.98:6085 ' >> ~/.config/torrc


Verification and Advice
-----------------------

For more added security; do not launch :program:`TOR` via the 'root' user, :program:`Vidalia` is perfect for this (or the command line).
To check your current browser with TOR: https://check.torproject.org/
