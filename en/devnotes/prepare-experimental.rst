.. http://doc.slitaz.org/en:devnotes:prepare-experimental
.. en/devnotes/prepare-experimental.txt · Last modified: 2011/02/22 22:19 by linea

.. _prepare experimental:

Prepare the experimental wok
============================

Warning
-------

Before using the experimental wok, it's advisable to read and understand the :ref:`Advanced usage of Mercurial <cookbook advancedhg>` as the experimental wok uses MQ to handle the patch queue.

Please note that the source code, wok & packages use a lot of space, don't store all of that in your RAM unless you're sure that it will run fine.
If you use a live system, you can use a physical disk to store data using the boot option ``home=``.
You can also mount the disk (i.e.: on :file:`/mnt`).


Enabling Mercurial Queue
------------------------

The experimental wok is shared as patches which can't be integrated into the cooking wok for now.
To use them, you need the extension MQ.
So your :file:`~/.hgrc` should contain these lines:

.. code-block:: ini

   [extensions]
   mq =


Download cooking wok & patch it
-------------------------------

First check that the destination disk is mounted.
In the directory which contains the data, create a repository called :file:`experimental` and enter in it::

  cd destination
  mkdir experimental
  cd experimental

Download the wok::

  hg clone http://hg.slitaz.org/wok wok

If you were root, enable read/write permissions to your normal account; then exit the root one::

  chown tux.tux -R wok

The patches will apply well on the revisions for which they was created, but problems can appear with the other revisions.
The revisions correlated to the last update of the patches can be found here: http://hg.slitaz.org/wok-experimental.
Update the wok to the good revision::

  hg update -C rev

Download experimental patches::

  cd .hg
  hg clone http://hg.slitaz.org/wok-experimental patches

Apply patches::

  cd ..
  hg qpush -a


Update the experimental wok
---------------------------

Go into the wok::

  cd ?/wok

Unapply patches (doesn't work if modifications are uncommitted)::

  hg qpop-a

Update cooking wok::

  hg pull

Check the revision to use at http://hg.slitaz.org/wok-experimental and update the working directory to this revision::

  hg update -C rev

Update patches repository::

  cd .hg/patches
  hg pull -u

Apply the new patches::

  cd ../..
  hg qpush -a
