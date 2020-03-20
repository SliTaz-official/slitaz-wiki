.. http://doc.slitaz.org/en:guides:conky
.. en/guides/conky.txt · Last modified: 2015/03/24 19:02 by linea

.. _conky:

Conky
=====


Introduction
------------

:program:`Conky` is a highly customizable, lightweight system monitor that sits on the desktop.
It is able to show loads of information about your computer, such as CPU usage, RAM, disk usage, CPU temperature, and also the weather or the number of new messages in your email.
It allows color graphical formatting of its indicators, offering a stylish look to the desktop.


Installation
------------

Installation is made through :program:`TazPkg`:

.. code-block:: console

   # tazpkg -gi conky


Basic Configuration
-------------------

To set up your :program:`Conky`, first copy the file :file:`/etc/conky/conky.conf` to your folder :file:`home` and rename it to :file:`.conkyrc`.
Now, you can modify the :file:`.conkyrc` file as you like; if you want to return to the original settings, simply repeat this procedure.

To start :program:`Conky`, open a terminal and type:

.. code-block:: console

   $ conky

Note that the :program:`Conky` window appears at the top-right of the screen, with a black background.

:program:`Conky` should start automatically when you turn on/restart your computer.
If it doesn't, go to the :menuselection:`Applications menu --> Preferences --> Desktop Session Settings` — on the :guilabel:`Automatically Started Applications` tab, select :guilabel:`Conky` box to boot with Linux.


Appearance and meters settings
------------------------------

The appearance of :program:`Conky` and the items it monitors are highly configurable.
For a complete list of configuration options, access the `official Conky site <http://conky.sourceforge.net>`_, "Documentation" link.

.. tip::
   There are several websites that offer custom settings ready for download.
   Just pick one and copy it to your :file:`~/.conkyrc`.

For example, to get your :program:`Conky` window transparent, allowing you to see the wallpaper behind it instead of the default black background, there are two ways:

#. .. compound::
      Change the following line of your file :file:`.conkyrc`::

        own_window_transparent no

      to::

        own_window_transparent yes

   This creates a simple transparency effect, forcing :program:`Conky` to copy the wallpaper as its background image.

#. For a more sophisticated effect, in which you can control the level of transparency of the :program:`Conky` window, change your :file:`.conkyrc` as follows:

   .. compound::
      After this line::

        own_window_transparent no

      add these two::

        own_window_argb_visual yes
        own_window_argb_value 150

   The added two lines make :program:`Conky` use the Linux composite (by default :program:`Xcompmgr`) to create the transparency effect.

   Note that you must activate the composite at Linux boot to get this effect working.
   To do this, go to the :menuselection:`Applications menu --> Preferences --> Desktop Session Settings` — on the :guilabel:`Automatically Started Applications` tab, select :guilabel:`Desktop effects with Xcompmgr` box to boot with Linux.
   Now right-click in an empty area of your desktop, select :menuselection:`Desktop effects --> Activate composite`.
   You should be able to see the semi-transparent :program:`Conky` window.

   .. compound::
      To control the level of transparency, change the end of the line::

        own_window_argb_value 150

      entering any value between 0 and 255, according to your preference.


References
----------

`official Conky site <http://conky.sourceforge.net>`_
