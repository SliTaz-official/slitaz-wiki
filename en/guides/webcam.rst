.. http://doc.slitaz.org/en:guides:webcam
.. en/guides/webcam.txt · Last modified: 2014/04/20 09:21 by linea

.. _webcam:

Webcam on SliTaz
================

:author: pankso, linea

SliTaz provides all the necessary tools to take pictures, record and stream webcam output.


Installation
------------

By default your webcam may not be detected due to missing kernel drivers and this is normal since SliTaz doesn't provide webcam support on the LiveCD.
To install and load the drivers:

.. code-block:: console

   # tazpkg -gi linux-media
   # modprobe uvcvideo

You can check if the kernel correctly loaded the module with:

.. code-block:: console

   $ dmesg | tail

The webcam may be used with :program:`Skype` and other visual tools.
To try the webcam with :program:`Mplayer`:

.. code-block:: console

   $ mplayer tv:// -tv driver=v4l2:device=/dev/video0 -vo x11


Pictures and recording
----------------------

To take pictures or record using the webcam you can install :program:`Mplayer`:

.. code-block:: console

   # tazpkg -gi mplayer

Take a picture:

.. code-block:: console

   mplayer tv:// -tv driver=v4l2:width=640:height=480:device=/dev/video0 \
   -fps 15 -vo png -frames 1

Record a video to a file named :file:`myvideo.avi`:

.. code-block:: console

   mencoder tv:// -tv \
   driver=v4l2:width=640:height=480:device=/dev/video0:forceaudio:adevice=/dev/dsp \
   -ovc lavc -oac mp3lame -lameopts cbr:br=64:mode=3 \
   -o myvideo.avi
