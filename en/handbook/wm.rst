.. http://doc.slitaz.org/en:handbook:wm
.. en/handbook/wm.txt · Last modified: 2016/10/30 16:45 by hgt

.. _handbook wm:

Window Managers
===============

:author: jozee, linea, seawolf, hgt


Openbox
-------

More details in :ref:`Desktop <handbook desktop>`


Enlightenment (e17)
-------------------

:program:`Enlightenment` is a complete desktop environment, fully configurable with the mouse and offering many themes.
The version supplied by the SliTaz project is known as :program:`e17` and is still in development, this version is considered stable enough to be incorporated into the distribution.
:program:`Enlightenment` was designed to be deployed on systems with limited resources, as well as more powerful systems.
It allows for wallpapers, menus, animated and interactive gadgets and knows how to manage virtual desktops.
To install :program:`e17`:

.. code-block:: console

   # tazpkg get-install enlightenment

Logout your current X session, type :kbd:`F1` at SLiM login and choose ``e17`` to start :program:`Enlightenment`.


.. rubric:: Menu and desktop icons

:program:`Enlightenment` is compliant to the Freedesktop standards.
Applications are sorted by category and icons automatically appear if a desktop file is supplied.
The (:file:`.desktop`) system files are contained in the :file:`/usr/share/applications` directory or hidden home :file:`~/.local/share/applications` directory.
These files have a simple syntax and are editable from the panel or with a simple text editor.
The file menus displaying categories in the :program:`Enlightenment` menu are found in the :file:`slitaz-menus` package:

.. code-block:: console

   # tazpkg get-install slitaz-menus


.. rubric:: An icon on the desktop

To have an icon on the desktop launching applications, you can create by hand a :file:`.desktop` file (`Recognized desktop entry keys <http://standards.freedesktop.org/desktop-entry-spec/latest/ar01s05.html>`_) in your local directory :file:`~/Desktop`.
Desktop files placed in this directory are automatically recognized by :program:`Enlightenment`.
A single :file:`.desktop` file can contain eight lines with respectively: the name (Name), generic name, comment, the command to execute (Exec), icon, type and Freedesktop categories.
Example of a :file:`.desktop` file for :program:`Xterm` icon:

.. code-block:: ini

   [Desktop Entry]
   Name = XTerm
   GenericName = Terminal
   Comment = Run commands in a shell
   Exec = xterm -bg black -fg white -cr orange -sb -bd violet -rightbar
   Icon = /usr/share/icons/Tango/jwm/utilities-terminal.png
   Type = Application
   Categories = Utility;Terminal;

Additional themes can be found on: http://exchange.enlightenment.org/


JWM — Joe's Window Manager
--------------------------

Joe's window manager, written in C is quick, simple, clean, stable and efficient.
:program:`JWM` proposes a taskbar, a menu of icons and a pager for the management of virtual desktops.
The taskbar can also act as a dock.
In addition it is easily configurable with a single text file that can change the menu, fonts and their sizes, and different colors.
To install :program:`jwm` on SliTaz:

.. code-block:: console

   # tazpkg get-install jwm

Logout your current X session, type :kbd:`F1` at SLiM login and choose :guilabel:`jwm` to start :program:`JWM`.
To make :program:`JWM` your default Window Manager, just type: :command:`tazx jwm`.


.. rubric:: Use and configure JWM

The application of Joe's Window Manager is very fast.
To view the menu just click somewhere on the desktop.
You can resize a window through the edges or corners, minimize or pass a virtual desktop to another via a pager.
You also have configurable keyboard shortcuts for faster access to the applications that you often use.
On SliTaz the system configuration file is :file:`/etc/jwm/system.jwmrc`.
Apart from this file, each user can use its own configuration file hidden in :file:`~/.jwmrc`.
This is a text file using XML syntax, it can edited with a simple text editor — lines beginning with: ``<!--`` are comments that let you understand what each tag does.

To facilitate the customization of the desktop, SliTaz automatically copies at the launch of the first (graphical) session, a system configuration file to the root directory of the user.
You can directly modify this file and test without risk.
To edit with your favorite text editor:

.. code-block:: console

   $ geany $HOME/.jwmrc &

To retrieve an original configuration file, you can copy the system configuration file and rename it :file:`.jwmrc` in your home directory:

.. code-block:: console

   $ cp /etc/jwm/system.jwmrc $HOME/.jwmrc

The tag ``RootMenu`` corresponds to the menu displayed by clicking on one of the (three) buttons on the mouse.
To add a category, you must use the tag ``Menu`` — this contains entries for various programs.
Any entry in the :program:`JWM` menu can fit on one line.
Example using the :program:`GQview` image management application:

.. code-block:: xml

   <Program icon="gqview.png" label="GQview">gqview</Program>

There are still many opportunities to configure RootMenu according to the mouse buttons; the choice of method to move windows, create groups, etc.
The Manual is available online at the official website of the project.
To view a list of command-line options, just type :command:`jwm -h` in a terminal.


.. rubric:: Create your own JWM style

Creating your own graphical style with :program:`JWM` is relatively quick and easy, the tags are clear and the attributes possible are given in the comments.
When preparing your work, you can see your amendments by restarting the window manager from the menu or via the :command:`jwm -restart` command.
In the configuration file, style tags start after the ``<!-- Visual Styles -->`` comment.
To begin, here is a short list of the main style tags with a short description:

* ``Background`` manages the wallpaper.
  This tag supports the solid, gradient, image or tile attributes, to respectively: use a solid color, create a gradient, display a resized image or tile an image.
* ``BorderStyle`` controls the windows border.
* ``TrayStyle`` controls a taskbar.
  The taskbar may, among other things, be automatically hidden or only fill a part of the screen with the width attribute.
* ``TrayListStyle`` controls the style of the list of open windows on the current desktop.
* ``PagerStyle`` controls the pager displaying different virtual desktops (4 by default).
* ``MenuStyle`` defines the menu style.
* The icons are defined by the ``IconPath`` tag, you can use your own personal icons by specifing the full path to the directory that contains them.
  Note that you can specify more than one path, if you want, you can use your own icons and those contained in the :file:`/usr/share/pixmaps` and :file:`/usr/share/icons` system directories.
  SliTaz uses the Tango theme icons: *tango.freedesktop.org* for the menu, these are 16×16 and are stored in :file:`/usr/share/icons/Tango`.
  You can add, edit, delete these…
  If you want to install new icons in your user space, we advise you to use :file:`~/Picture/Icons` (set as default) or a hidden directory :file:`~/.Icons`.

The colors can be defined by their name or RGB number such as ``#3A4956``.
To use colors in their gradient mode, you must specify the two colors separated by a colon, example ``#6C0023:#3E1220``.
You can change fonts and their sizes by using the ``Font`` tag.
There are still some other small things that you can change to customize your desktop: such as the name of a menu item and its icon.
Before restarting :program:`JWM` with your new configuration file, you can check its syntax by using the command: :command:`jwm -p`.
To explore further, the official handbook describes all the tags, options and valid attributes.
You can view it online at the :program:`JWM` website.


.. rubric:: JWM website

* http://www.joewing.net/programs/jwm/ — The official website of Joe's Window Manager, providing news and a comprehensive manual.
* #jwm on irc.freenode.net — The :program:`JWM` IRC discussion channel on Freenode server.


.. _handbook wm pekwm:

Pekwm
-----

`Pekwm Documentation <https://www.pekwm.org/doc/git/index.html>`_


.. _handbook wm dwm:

DWM
---

`DWM Documentation <http://dwm.suckless.org/tutorial>`_


.. _handbook wm xfce:

Xfce
----

`Xfce <http://www.xfce.org/>`_ is a lightweight desktop environment.
It replaces the default :program:`Openbox` and :program:`PCManFM`, and is also based on GTK+.

To install :program:`Xfce`, select the 'xfce4' meta-package from the Package Manager.
This will install all the related packages.

To use :program:`Xfce`, ensure you have the correct command for :program:`Xfce` in the SLiM (log-in manager) configuration.
Do this by appending ``xfce4`` to the ``sessions`` line of the :file:`/etc/slim.conf` file — note that you will need root permissions to modify the file::

  sessions            openbox,e17,jwm,xfce4,

You can then select :program:`Xfce` by pressing :kbd:`F1` at the log-in screen, as you enter your user-name and password.

Extras are available at `Xfce Goodies <http://goodies.xfce.org/>`_, including plug-ins, artwork and bindings.

To remove :program:`Xfce`, use the following command as *root* user:

.. code-block:: shell

   for PKG in xfce4 xfce4-session xfce4-panel xfwm4 libxfcegui4 xfce-utils \
              libxfce4util thunar thunar-volman xfconf; do
     yes y | tazpkg remove $PKG
   done
