.. http://doc.slitaz.org/en:guides:faq-mouse
.. en/guides/faq-mouse.txt · Last modified: 2015/04/08 19:19 by linea

.. _faq-mouse:

Mouse
=====


I can't change the mouse for left-handed people
-----------------------------------------------


.. rubric:: Symptoms

I can't change the mouse for left-handed people using the computer.


.. rubric:: Explanation

You need to install the :program:`xorg-xmodmap` package to configure the mouse buttons.


.. rubric:: Solution

Install the :program:`xorg-xmodmap` package.
In a terminal, type:

.. code-block:: console

   # tazpkg -gi xorg-xmodmap

To configure the mouse for left-handed:

.. code-block:: console

   # xmodmap -e "pointer = 3 2 1"

To configure the mouse for right-handed:

.. code-block:: console

   # xmodmap -e "pointer = 1 2 3"

To check the configuration of your mouse:

.. code-block:: console

   # xmodmap -pp

To make the changes permanent for your user, create a new file called :file:`xmodmap.desktop` in the :file:`~/.config/autostart` directory with the following content:

.. code-block:: ini

   [Desktop Entry]
   Type=Application
   Name=Left hand mouse
   Exec=xmodmap -e "pointer = 3 2 1"
   NotShowIn=XFCE;Razor;LXDE;

Logout and Login to X session and the mouse buttons are configured for left-handed people.

To make the changes permanent for all users at the same time, repeat the procedure above as *root*, creating a new :file:`xmodmap.desktop` file in the :file:`/etc/xdg/autostart` directory.

References:

* `Mouse left-handed <http://forum.slitaz.org/topic/mouse-left-handed>`_
* `Configure left-handed mouse <http://forum.slitaz.org/topic/help-configure-left-handed-mouse#post-1792>`_
