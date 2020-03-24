.. http://doc.slitaz.org/en:guides:xbindkeys
.. en/guides/xbindkeys.txt · Last modified: 2017/09/03 15:33 by hgt

.. _xbindkeys:

Xbindkeys
=========

:author: sidini, linea, hgt


Introduction
------------

:program:`Xbindkeys` is a program that allows you to launch shell commands with your keyboard or your mouse under X Windows.
It links commands to keys or mouse buttons using a configuration file.
It's independent of the window manager and can capture all keyboard keys (ex: :kbd:`Power`, :kbd:`Wake`…).

It is handy to make "dead keys" of a multimedia keyboard work (ex: :kbd:`play`, :kbd:`pause`, :kbd:`browser homepage`…)


Installing
----------

Use :program:`tazpkg` to quickly install :program:`xbindkeys` package.
Open a terminal as *root* and type:

.. code-block:: console

   # tazpkg get-install xbindkeys


Configuration and detecting key codes
-------------------------------------

:program:`Xbindkeys` uses a configuration file to link a command to a key on your keyboard.
Usually this file is :file:`$HOME/.xbindkeysrc`

You can have a default one created by using:

.. code-block:: console

   $ xbindkeys --defaults > $HOME/.xbindkeysrc

Start the program using:

.. code-block:: console

   $ xbindkeys

To add a custom keyboard shortcut, first you have to detect the key code.
To do this, use:

.. code-block:: console

   $ xbindkeys -k

When a small white window shows up on screen, just press the desired key.
Terminal will show you the 3-line code of the pressed key.
Example::

  "(Scheme function)"
  m:0x10 + c:180
  Mod2 + XF86HomePage

Now, close :program:`xbindkeys` application to make changes on configuration file:

.. code-block:: console

   $ killall xbindkeys

Open :file:`.xbindkeysrc` file on :program:`Leafpad` (or on your favorite text editor) and copy the 3-line code above to the end of the file.
We need to change the first code line to link the key code to a desired function.
For example, if we want to open :program:`Midori` when :kbd:`XF86Homepage` key is pressed, we must change the first line to::

  "midori"
  m:0x10 + c:180
  Mod2 + XF86HomePage

Save and close the :file:`.xbindkeysrc` file.
Restart :program:`xbindkeys` program and your new shortcut is already working!
To add other shortcuts, repeat the procedure above.

If you want to detect a multi-key shortcut code like :kbd:`Ctrl`\ +\ :kbd:`F`, use:

.. code-block:: console

   $ xbindkeys -mk


Summary
^^^^^^^

Summary of commands:

.. code-block:: console

   # tazpkg get-install xbindkeys
   $ xbindkeys --defaults > $HOME/.xbindkeysrc
   $ xbindkeys
   $ xbindkeys -k
   $ killall xbindkeys
   $ leafpad HOME/.xbindkeysrc
   $ xbindkeys


Adding xbindkeys in autostarted applications
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In the :program:`PCManFM` in your :file:`home` folder search for hidden folder :file:`.config`, open it, next open :file:`openbox` folder and open :file:`autostart.sh` in your text editor.
Add these lines at the end of your file:

.. code-block:: shell

   # Fn Keys
   xbindkeys &

Save and quit text editor.
All custom shortcuts will stay after you reboot or switch off/on your machine.


Examples and tips
-----------------

This section shows you many command codes ready to use.
Key codes (last two of 3-line key code) may vary from keyboard to keyboard, so it's up to you to catch them using :program:`xbindkeys`.


Volume control (Alsa mixer)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

  #Muter/UnMute
  "amixer set "Master" toggle"
  m:0x00 + c:121
  XF86AudioMute

  #Volume up
  "amixer set "Master" 5%+"
  m:0x0 + c:123
  XF86AudioRaiseVolume

  #Volume down
  "amixer set "Master" 5%-"
  m:0x0 + c:122
  XF86AudioLowerVolume


Alsaplayer
^^^^^^^^^^

::

  #stop alsaplayer
  "alsaplayer --pause"
  m:0x0 + c:172
  XF86AudioPlay

  #next alsaplayer
  "alsaplayer --next"
  m:0x0 + c:171
  XF86AudioNext

  #previous alsaplayer"
  "alsaplayer --prev"
  m:0x0 + c:173
  XF86AudioPrev


Midori Browser
^^^^^^^^^^^^^^

::

  #Open Midori at Homepage
  "midori --execute Homepage"
  m:0x10 + c:180
  Mod2 + XF86HomePage

  #Back for previous page
  "midori --execute Back"
  m:0x10 + c:166
  Mod2 + XF86Back

  #Forward to next page
  "midori --execute Forward"
  m:0x10 + c:167
  Mod2 + XF86Forward

  #Stop loading current page
  "midori --execute Stop"
  m:0x10 + c:136
  Mod2 + Cancel

  #Reload/Refresh current page
  "midori --execute Reload"
  m:0x10 + c:181
  Mod2 + XF86Reload

.. note::
   If you use :program:`Firefox` or Google :program:`Chrome`, there's no need to modify :file:`xbindkeysrc` file: these browsers automatically recognize the multimedia keys.
   Also, if you change from :program:`Midori` to :program:`Firefox` or :program:`Chrome`, you must comment (``#``) or delete the shortcuts above.
   If you don't do this, it will open a :program:`Midori` window when you press a navigation button.


Power management
^^^^^^^^^^^^^^^^

::

  #suspend to ram
  "sudo pm-suspend"
  m:0x0 + c:150
  XF86Sleep

.. note::
   This one (suspend to RAM) works if you install :program:`pm-utils`.

::

  #power off button
  "poweroff"
  m:0x0 + c:124
  XF86PowerOff


Screenshots
^^^^^^^^^^^

::

  #screenshot
  "mtpaint -s"
  m:0x0 + c:107
  Print


Applications Menu (Start Menu)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

  #Show Start Menu with left windows-key
  "lxpanelctl menu"
  m:0x50 + c:133
  Mod2+Mod4 + Super_L

  #Show Start Menu with right windows-key
  "lxpanelctl menu"
  m:0x50 + c:134
  Mod2+Mod4 + Super_R


References
----------

Xbindkeys homepage:
  http://www.nongnu.org/xbindkeys/xbindkeys.html

Forum topics:
  * http://forum.slitaz.org/topic/make-fn-keys-work
  * http://forum.slitaz.org/topic/slitaz-40-how-to-made-screen-shots-captures-d-ecran
  * http://forum.slitaz.org/topic/keyboard-shortcutshooks
  * http://forum.slitaz.org/topic/keyboard-shortcuts-how-create-them
  * http://forum.slitaz.org/topic/magic-sysrq-keys-power-button
  * http://forum.slitaz.org/topic/power-button-shutdown
  * http://forum.slitaz.org/topic/fn-key
