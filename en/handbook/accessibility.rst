.. http://doc.slitaz.org/en:handbook:accessibility
.. en/handbook/accessibility.txt · Last modified: 2012/08/14 22:28 by linea

.. _handbook accessibility:

Accessibility
=============


About
-----

SliTaz aims to provide some Assistive Technology (AT) tools.


Screen magnifier
----------------

SliTaz supplies a screen magnifier whose edges can be resized and that can magnify up to 16\ :sup:`×`.
It also provides graphical tools that enable a crosshair, pixel positions and RGB values.
It can be found in the :menuselection:`Menu --> Utilities`:

.. code-block:: console

   # tazpkg get-install magnifier


Espeak — Text to speech (TTS)
-----------------------------

The :program:`espeak` speech synthesizer run from the command line can speak text from an input file or from *stdin* and supports many languages.
:program:`Espeak` can adjust the amplitude, pitch, word gap, speed, etc.
It can also write its output to a wave file rather than speaking it directly.
Type :command:`espeak --help` for a full list of available options:

.. code-block:: console

   # tazpkg get-install espeak

Example of use:

.. code-block:: console

   $ espeak -f example.txt


On-screen keyboard
------------------

Virtual keyboard (:program:`xvkbd`) can be used to enter characters into the software of your choice.
The menu can be used to change the keyboard layout, function keys, etc.
It supports word completion, the removal of unwanted keys and can connect to a remote display.
The configuration can be found in the :file:`~/.Xdefaults` file:

.. code-block:: console

   # tazpkg get-install xvkbd


Yasr — Screen Reader
--------------------

:program:`Yasr` is a console (text-based) screen reader that operates through the :program:`speech-dispatcher` interface and :program:`espeak`:

.. code-block:: console

   # tazpkg get-install yasr

To configure :program:`speech-dispatcher`:

.. code-block:: console

   # spd-conf 

:program:`Speech-dispatcher` can be started/stopped like a daemon:

.. code-block:: console

   # speech-dispatcher
   # killall speech-dispatcher


.. rubric:: Usage

.. code-block:: console

   $ yasr
   $ exit

The configuration files can be found in the :file:`/etc/speech-dispatcher` or :file:`~/.speech-dispatcher` directories
and the :program:`yasr` configuration file is located in :file:`/usr/share/yasr`.


Sticky/Slow/Mouse Keys in X
---------------------------

To enable the accessibility keys just edit your :file:`/etc/slim.conf` file and logout of your X session::

  default_xserver     /usr/bin/Xorg 
  xserver_arguments   +accessx 

After you login again, to enable/disable:

* **Sticky keys**: Press the :kbd:`Shift` key 5 times
* **Slow keys**: Hold down the :kbd:`Shift` key for 8 seconds
* **Mouse keys**: Hold down the left :kbd:`Shift`, left :kbd:`Alt` and :kbd:`Num Lock` keys

There is also a package in the repositories that can do all this in GUI or command line mode: :program:`accessx`.
To install and run:

.. code-block:: console

   # tazpkg get-install accessx
   $ ax help
   $ accessx
