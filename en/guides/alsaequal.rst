.. http://doc.slitaz.org/en:guides:alsaequal
.. en/guides/alsaequal.txt · Last modified: 2012/08/14 22:26 by linea

.. _alsaequal:

Alsaequal
=========

:author: linea


Installation
------------

First download alsaequal:

.. code-block:: console

   # tazpkg get-install alsaequal

Then create a :file:`/home/tux/.asoundrc` file:

.. code-block:: toml

   ctl.equal {
     type equal;
   }
   
   pcm.plugequal {
     type equal;
     # Modify the line below if you don't
     # want to use sound card 0.
     # slave.pcm "plughw:0,0";
     # or if you want to use with multiple applications output to dmix
     slave.pcm "plug:dmix"
   }
   
   pcm.equal {
     # Or if you want the equalizer to be your
     # default soundcard uncomment the following
     # line and comment the above line.
   # pcm.!default {
     type plug;
     slave.pcm plugequal;
   }


mpg123
------

.. compound::
   Change (:command:`cd`) into your music directory and then run:

   .. code-block:: console

      $ mpg123 -a equal *

   Or

   .. code-block:: console

      $ mpg123 -a equal track1

Now you should be able to open up a separate terminal and use:

.. code-block:: console

   $ alsamixer -D equal


mpd
---

Just edit the ``audio_output`` section of your :file:`/etc/mpd.conf`:

.. code-block:: toml

   audio_output {
     type             "alsa"
     name             "equal"
     device           "plug:plugequal"
     ## format        "44100:16:2"     # optional
     ## mixer_device  "default"        # optional
     ## mixer_control "PCM"            # optional
     ## mixer_index   "0"              # optional
   }


And start/restart :program:`mpd` and you should be able to use :command:`alsamixer -D equal`


moc
---

Copy the :file:`config.example` file in :file:`/usr/share/doc/moc` to your :file:`~/.moc` folder:

.. code-block:: console

   $ cp /usr/share/doc/moc/config.example ~/.moc/config

Then change the :program:`alsa` output device line to::

  # ALSA output device
  AlsaDevice    = equal

And then start/restart :program:`moc`.


References
----------

http://www.thedigitalmachine.net/alsaequal.html
