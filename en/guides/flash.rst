.. http://doc.slitaz.org/en:guides:flash
.. en/guides/flash.txt · Last modified: 2010/08/24 17:42 by linea

.. _flash:

How-To install Adobe Flash Player
=================================

Open a terminal.
Become super-user:

.. code-block:: console

   $ su -

The default password for the root account is ``root``.

Type the following as one command:

.. code-block:: console

   # tazpkg get-install get-flash-plugin && get-flash-plugin

The package installs a :command:`get-flash-plugin` command which creates a pseudo-package that downloads the source code from Adobe.
It packages the build source code and pulls in any necessary dependencies.

By using this method the Flash plugin can be managed through :program:`tazpkg`; it will update itself as the repo package is updated.

When you install Flash to SliTaz 3.0 and :program:`Midori` is not recognizing the plugin — just reinstall Midori with

.. code-block:: console

   # tazpkg get-install midori --forced

Thats it.

.. note::
   This was successfully tested in Midori, Firefox/Shiretoko & Opera, on Cooking 2010-03-14 (the v3 RC).
