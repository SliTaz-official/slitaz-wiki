.. http://doc.slitaz.org/en:guides:conspy
.. en/guides/conspy.txt · Last modified: 2017/05/19 18:51 by hgt

.. _conspy:

Conspy: tiny screen or VNC
==========================

:author: jozee, linea, bellard, t0n1, hgt

SliTaz core provides the 10Kb :program:`conspy` to get remote control of Linux virtual consoles.
See http://conspy.sourceforge.net/

SliTaz opens 6 virtual consoles which you can access with :kbd:`Ctrl`\ +\ :kbd:`Alt`\ +\ :kbd:`F1` to :kbd:`Ctrl`\ +\ :kbd:`Alt`\ +\ :kbd:`F6`.
You can connect to console 1 with :command:`conspy 1` and console n with :command:`conspy n` or the current active console with :command:`conspy` (root user only).

To exit from :program:`conspy` (and the virtual console) press the :kbd:`Esc` key three times in quick succession.


Conspy as screen (session manager)
----------------------------------

Linux supports up to 63 virtual consoles.
You can have up to 62 (63 — X11 on console 7) sessions.
Six sessions are already opened by SliTaz.
You can open a new console / new session (say console 28) with :command:`openvt -c 28 /bin/login` or :command:`openvt -c 28 /bin/ash`.
You can free this virtual console with :command:`deallocvt 28`.

Example:

.. code-block:: console

   home$ ssh tux@slitazbox
   box$ su
   box# openvt -c 28 /bin/ash
   box# conspy 28
   # some commands
   ...
   # <ESC><ESC><ESC>
   box# exit
   box$ exit

.. tip::
   With a recent :program:`busybox` (see below) you can skip the openvt step:

   .. code-block:: console

      home$ ssh tux@slitazbox
      box$ su
      box# conspy -cs 28
      # some commands
      ...
      # <ESC><ESC><ESC>
      box# exit
      box$ exit

Later:

.. code-block:: console

   home$ ssh tux@slitazbox
   box$ su
   box# conspy 28
   # more commands
   ...
   # <ESC><ESC><ESC> 
   box# exit
   box$ exit

To close the session:

.. code-block:: console

   home$ ssh tux@slitazbox
   box$ su
   box# conspy 28
   # exit
   <ESC><ESC><ESC>
   box# deallocvt 28
   box# exit
   box$ exit

.. tip::
   With a recent :program:`busybox` (see below) you can skip the deallocvt step:

   .. code-block:: console

      home$ ssh tux@slitazbox
      box$ su
      box# conspy 28
      # clear; exit
      <ESC><ESC><ESC>
      box# exit
      box$ exit

If you prefer to use :program:`screen`, see http://www.gnu.org/software/screen:

.. code-block:: console

   # tazpkg get-install screen
   $ screen -S MySession


Conspy as VNC (shared console)
------------------------------

You can share a virtual console between two or more users.
Say RemoteUser wants to show some commands to SlitazUser using SlitazBox.
RemoteUser selects SlitazUser's console with :command:`chvt`:

.. code-block:: console

   home$ ssh SlitazBox
   SlitazBox$ su
   SlitazBox# chvt 1
   SlitazBox# conspy 1

Now both users show the same terminal.
A third user can do :command:`conspy 1` too.

If you prefer to share the X11 display, install :program:`x11vnc` (VNC server) and :program:`x11vnc-extra` (java VNC client) see http://www.karlrunge.com/x11vnc/:

.. code-block:: console

   SlitazBox# tazpkg get-install x11vnc
   SlitazBox# tazpkg get-install x11vnc-extra
   SlitazBox# /etc/init.d/x11vnc start

.. code-block:: console

   home$ su
   home# get-java-jre
   home# exit
   home$ firefox http://SlitazBox:5800/ultrasigned.vnc


X11VNC Autostart
----------------

If you'd like to start X11VNC automatically you must edit :file:`/home/$USER/.xinitrc` with:

.. code-block:: console

   # nano /home/tux/.xinitrc

.. code-block:: shell

   # ~/.xinitrc: Executed by slim login manager to startx X session.
   # You can use F1 with Slim to change your window manager or configure
   # it permanently with your personal applications.conf file.
   #
   . $HOME/.config/slitaz/applications.conf
   
   ############################################################
   ## ATTENTION!!! INSERT THE FOLLOWING LINE AFTER .conf FILE CALL
   /etc/init.d/x11vnc start &
   
   case $1 in
   	e17|enlightenment*)
   ...

**DO NOT USE** the :guilabel:`Autostart Programs` option under :program:`OpenBox` or Daemon tricks, it will start a :program:`X11VNCserver` before a :program:`X11server` and crash the VNC after the first client connection.
So you'll only connect once to the server (and it's not desirable in support environments).


Conspy and slow connections
---------------------------

Launch a very verbose command into a :program:`conspy` and the output is displayed at full speed in the virtual console whatever your connection speed to the remote box is (even if your connection is broken).


Conspy and Busybox
------------------

:program:`Conspy` is a busybox applet since busybox 1.17.0.
It adds 2.5Kb to busybox, has better terminal support and supports some new options:

* ``-c`` to create missing devices (:file:`/dev/vcsaXX` and :file:`/dev/ttyXX`)
* ``-d`` for screen shot

  .. code-block:: console

     # conspy -nd 28 > screen28.txt

* ``-s`` to launch a shell

  .. code-block:: console

     # conspy -cs 28

  (no more openvt/deallocvt)

* ``-x`` COL ``-y`` LINE upper left corner position
* ``-f`` follow cursor with automatic scrollings

The conspy applet is enabled in the busybox package.
The conspy package is no longer more useful than the recent busybox and will be removed from the packages database.
