.. http://doc.slitaz.org/en:handbook:multimedia
.. en/handbook/multimedia.txt · Last modified: 2015/12/24 17:16 by linea

.. _handbook multimedia:

Multimedia
==========


SliTaz sound and video
----------------------

SliTaz LiveCD mode provides sound support, but no video player, however you can watch YouTube by installing the :program:`get-flash-plugin` package.
To watch videos you can use :program:`Xine`, it supports most formats such as avi, mov, mpeg, etc.
Note :program:`Xine` can also act as an audio player.
At the sound level, you have a mixer (:program:`alsamixer`) and audio player (:program:`alsaplayer`), both installed by default on the standard LiveCD.
SliTaz also provides tools to configure your sound card.


Configure the sound card
------------------------

In LiveCD mode SliTaz automatically configures the sound card by launching a dialog.
In most cases the sound card is instantly recognized and you just press :kbd:`Enter` to continue to login and have your sound working.
However, sometimes your card is not recognized at startup or simply not supported.
Before throwing in the towel, you can try to manually configure your card by asking for help on the mailing list or on the hardware support forum.


.. rubric:: Soundconf

To configure or reconfigure a sound card, SliTaz provides a script (:program:`soundconf`), this utility is used at startup and doesn't pose any questions, supports the right modules and automatically configures :file:`/etc/rsS.conf` so that the right driver is loaded on each start.
:program:`Soundconf` will also automatically adjust volume levels, you can change the settings later through the mixer and :program:`alsactl`.
To start the sound configuration interface, you must have administrator priviledges and type:

.. code-block:: console

   # soundconf


.. rubric:: Alsactl

:command:`Alsactl` controls ALSA and can store settings or restore sound for example, to retain preferences for each start.
When configuring sound with :command:`soundconf`, volumes are automatically adjusted.
You can use :command:`alsamixer` as root to change the values and then launch :command:`alsactl` to store the configuration.

First, start :command:`alsactl`:

.. code-block:: console

   # alsactl init

Then use :command:`alsamixer` to set up sound levels according to your preferences (you can find them at menu :menuselection:`Applications --> Multimedia`).
Lastly, use :command:`alsactl` again to store your preferences:

.. code-block:: console

   # alsactl store

To restore the configuration you can use the :command:`alsactl restore` command or to restore your preferences at each system startup you can edit :file:`/etc/init.d/local.sh` with your favorite text editor or simply :command:`echo` the changes:

.. code-block:: console

   # echo 'alsactl restore' > /etc/init.d/local.sh


Alsamixer
---------

:command:`Alsamixer` is the official mixer of the ALSA project.
It is simple and effective and can be run from the menu or directly from a terminal.
The volumes can be regulated with the up/down arrows or muted using the :kbd:`M` key.
To start from a Linux terminal:

.. code-block:: console

   $ alsamixer


Alsaplayer
----------

The audio player :program:`Alsaplayer` is designed to be simple, clean and intuitive.
:program:`Alsaplayer` on SliTaz provides support for ogg, mp3 (via *libmad*) and wav files, playlists and viewers to accompany sound and the ability to adjust pitch and volume.
Once launched from the menu, just click the play button or load a playlist to start.


Asunder — Audio CD Ripper
-------------------------

:program:`Asunder` retrieves and encodes audio tracks from a CD.
The toolkit is simple and easy to use, encoding songs into wav, ogg or mp3 (via *lame* package).
It can search CDDB, create playlists and edit file names.
You will find :program:`Asunder` in the :menuselection:`Multimedia` category on the menu.


mhWaveEdit — Audio editor and recorder
--------------------------------------

:program:`mhWaveEdit` is a graphical application for playing, recording and editing sound files.
Supported formats: wav (default), and a few others.
A variety of other formats can also be imported through :program:`Mplayer`.
To start from the menu, select :menuselection:`Multimedia --> mhWaveEdit`.


MPD — Music Player Daemon
-------------------------

:program:`MPD` is a great little music player that uses the server/client architecture, this means it can be even run remotely without a X server operating.
To install :program:`MPD` on SliTaz:

.. code-block:: console

   # tazpkg get-install mpd

:program:`MPD` on SliTaz drops root priviledges, so to start the server, just run:

.. code-block:: console

   $ mpd

And to stop it:

.. code-block:: console

   $ mpd --kill

Simply drag your music files into the :file:`~/music` folder (or create a link) and then run :command:`mpd --create-db` as root to update the database and you're ready to use one of the many clients.
The :command:`mpd --version` command lets you have a full list of available formats and outputs.


MPC — Music Player Client
-------------------------

:program:`MPC` is a popular client for :program:`MPD`, to load all the files into :program:`MPC` from the :program:`MPD` database and start to play them, simply install and run:

.. code-block:: console

   # tazpkg get-install mpc
   $ mpc add /
   $ mpc play

The command :command:`mpc --help` gives you a full list of all the available options that allow you to edit the playlist, enable crossfade, adjust the volume and shuffle tracks, etc.


mpg123
------

:program:`mpg123` is a command line audio player and file converter, this means that you can listen to music or convert files from a terminal.
To install:

.. code-block:: console

   # tazpkg get-install mpg123

To display help, use the ``--help`` option.
To play a mp3 file, just launch :program:`mpg123` followed by the name of the audio file:

.. code-block:: console

   $ mpg123 sound.mp3

:program:`mpg123` can also encode a file into another format, for example you can convert a *wav* file into a mp3 file.
Example:

.. code-block:: console

   $ mpg123 -w sound.mp3 sound.wav


Mplayer
-------

:program:`Mplayer` is a popular movie player for Linux suppporting many formats including DVD, VCD, mpeg, wmv, realvideo, etc.
It can also play various audio codecs such as aac, wma, realaudio, as well as ogg, flac, etc.
:program:`Mplayer` is configurable via a right click menu and customizable using various skins and GUIs that easily enable you to configure your own video drivers, output devices and so on.
To install :program:`mplayer`:

.. code-block:: console

   # tazpkg get-install mplayer-svn


Xine
----

:program:`Xine` is a multimedia project providing various video viewers and audio players.
SliTaz provides libraries and a media player contained in the package :program:`xine-ui`.
:program:`Xine` uses a *Xlib* interface, a control panel, a right click configuration menu and various plugins.
It can play ogg, mp3 and flac audio codecs, and mov, avi or mpg video formats.
To install :program:`xine` and its dependencies:

.. code-block:: console

   # tazpkg get-install xine-ui

Official `Xine website <http://xinehq.de/index.php/home>`_


VLC
---

:program:`VLC` media player is a highly portable multimedia player and multimedia framework capable of reading most audio and video formats (MPEG-2, MPEG-4, H.264, DivX, MPEG-1, mp3, ogg, aac…) as well as DVDs, Audio CDs VCDs, and various streaming protocols.
To install :program:`vlc`:

.. code-block:: console

   # tazpkg get-install vlc

Official `VLC website <http://www.videolan.org/vlc/>`_
