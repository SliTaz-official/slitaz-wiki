.. http://doc.slitaz.org/en:base:start
.. en/base/start.txt · Last modified: 2014/12/27 17:35 by linea

.. _base:

SliTaz Base Corner
==================

Animated by this `starting thread`_ at the forum, I felt the need for a central place to house information on how use SliTaz in pure console mode installing only the base of SliTaz!

As this place is now available, we can feed it with useful information and links with the intention of helping to start more easily — as the first objective of SliTaz is to be a graphical Linux system and not really destined to be used as base only…


Installation alt. use base without installation (frugal method)
---------------------------------------------------------------

.. rubric:: Read information!

.. compound::
   The installation of the base can use the same methods (as described in following pages) for the complete installation:

   * :ref:`Live-CD usage <handbook livecd>`
   * :ref:`Installation <handbook installation>`
   * :ref:`Unusual install methods <uncommoninst>`

   being conscious that you only install a part of them!
   That part is included in the ISO and is named :file:`rootfs4.gz`.


.. rubric:: Transfer onto hard disk for a frugal start

As you will work in the command line, I assume you also will make instructions in a command line!

Step-by-step operations:

First step: we need an ISO file containing the software that we will use:

SliTaz offers clear and strong organisation:

* home_ is the main site with a `special page to search`_ for all information about SliTaz in all SliTaz pages and media
* doc_ is the starting point for your search for documentation
* forum_ is our common forum in all languages
* mirror_ is the starting point for your downloads

So we will have to search for our ISO file in the last one on the mirror!
The organisation is very simple!
I will use the actual ISO for the new version 5.0 in elaboration.
I visit the mirror with my browser and find the file needed.
I will download it with :program:`wget` by typing in the command line:

.. code-block:: console

   $ wget http://mirror.slitaz.org/iso/5.0/slitaz-5.0-rc2.iso
   $ ls

wget echoes for ex. ``(914 KB/s) - slitaz-5.0-rc2.iso saved.``

I find the file ready in my home directory: :command:`ls` echoes the file name in the population of my home!

I now have to know a bit about my actual system.
Where is the location of my compact disk drive in the tree?
For example :file:`/mnt/cdrom` or :file:`/media/cdrom`?

I will mount the file at this point:

.. compound::
   I type in the command line (or copy it from here marking / hitting on the middle mouse key):

   .. code-block:: console

      # mount -o loop,ro

   marking the preceding lines with the name of the ISO file with the mouse and hitting after the beginning of the line with the middle mouse key,

   .. code-block:: console

      # mount -o loop,ro slitaz-5.0-rc2.iso

   and continue as follows:

   .. code-block:: console

      # mount -o loop,ro slitaz-5.0-rc2.iso /mnt/cdrom
      # ls /mnt/cdrom

   (or :file:`/media/cdrom` etc.)

:command:`ls /mnt/cdrom` echoes::

  boot  index.html  md5sum  README  style.css

I look deeper::

  ls /mnt/cdrom/boot

and can see the entries for the files I am looking for (marked in the following view)::

  **bzImage**  grub  ipxe  isolinux  memtest	rootfs1.gz  rootfs2.gz	rootfs3.gz  **rootfs4.gz**	**vmlinuz-3.2.53-slitaz**

.. compound::
   I will now copy those 3 files using:

   ::

     cp -a FILENAME

   into a new directory in the root of an adequate partition (please not a Windows NTFS! But it can be a fat32 from an old Windows…).

I only have to add now an adequate entry in my :file:`grub.cfg` or :file:`/.../grub/menu.lst` to start my new SliTaz base using Grub!

You need only a few minutes to do all this and can immediately begin after restarting to experiment in the frugal started SliTaz base system!

This method has a major inconvenience: All what you do in the frugal started SliTaz base is not persistent!
If you want to preserve your next steps you have to decide between two ways: remaster your SliTaz (if you find it is to much) or install fully your new SliTaz base somewhere (probably your own little partition for it, -see the following item-, or you can look `here <http://doc.slitaz.org/en:guides:uncommoninst>`_ for a **Subdirectory install in a Posix filesystem**).


Install the SliTaz base on an own little partition
--------------------------------------------------

I assume now you used the above frugal start to proceed to that full installation and that your ISO file is still in the same partition in your preceding :file:`~` (home) directory!
If not you only have to repeat later some of preceding steps but I will not describe them again…

I also assume you have already created the newly needed partition (if not look for information on how to do that in the pages linked under the above item **Read Information!**).
The partition doesn't have to be empty: Only the usual Linux directories have to be moved somewhere else in that partition (for ex. :file:`/oldSystem`!).

You started using Grub and the **login** console appears.
Please enter the SliTaz standard username **tux**.
It needs no password :-) !
To work as superuser you will later need the superuser password.
In the new system it is **root** (you can change it in the full installation using the command :command:`passwd` and following the dialog!).
But as user, you can continue with **tux**, why not!


Choice of standard SliTaz packages useful in a SliTaz base system
-----------------------------------------------------------------


.. rubric:: Installation of packages

As you are always in a command line system, you have only options:

* install packages with `Tazpkg <http://hg.slitaz.org/tazpkg/raw-file/tip/doc/tazpkg.en.html>`_ (hit on that link to download the HTML manual).
  Example:

  ::

    tazpkg get-install gpm

* compile a non SliTaz package (very useful in console mode: :program:`didiwiki`, works well in :program:`lynx` or :program:`links`, :program:`wordgrinder`, text processor for CLI)
* add per simple copy (buuuuuuuuuuh!) :-| a binary package from another distro and hope it works and does nothing bad…


.. rubric:: Usual jobs

* mouse driver under console mode: :program:`gpm` (this author did also write an interesting text editor with full performance using … the mouse of course!)
* :program:`sudo`
* directories and file manager: :program:`clex`
* commander: :program:`mc`
* email client: :program:`alpine`
* :program:`alpine` includes the very interesting text editor: :program:`pico` (SliTaz includes :program:`e3` in :program:`vi` mode, uses trad. :program:`nano`, and proposes also :program:`vim`)
* irc client: :program:`rhapsody`
* web browser: :program:`lynx` (or :program:`links`, but :program:`links` is having a few dependencies as it would be also usable in graphical mode, where it can show pictures)
* *retawq* did show me only the code of http://encrypted.google.com (retawq was the trad. SliTaz browser)
* *elinks* has too many dependencies in SliTaz…
* spreadsheet: :program:`sc`


.. rubric:: Server jobs



Helpful documents
-----------------

* the, of course, most interesting documents are the man pages of diverse bash commands including bash itself and of the above app's!
  This may be really difficult to get in foreign languages and I can't say that the Google translation is very useful in this case depending on the language.
  For my own use, I did collect the text of the following man pages: :command:`adduser`, :command:`alpine`, :command:`ar` (useful to extract sources from Debian), :command:`arch`, :command:`ark`, :command:`cat`, :command:`chmod`, :command:`chown`, :command:`chroot`, :command:`clear`, :command:`cp`, :command:`cut`, :command:`date`, :command:`dd`, :command:`debmany` (to search Debian man pages), :command:`didiwiki`, :command:`dpkg` (regarding Debian), :command:`e2freefrag`, :command:`e2fsck`, :command:`export`, :command:`filefrag`, :command:`fim` (see on the internet, perhaps `<http://savannah.nongnu.org/projects/fbi-improved/>`_!),
  :command:`free`, :command:`gpm`, :command:`groupadd`, :command:`imagemagick`, :command:`init`, :command:`less`, :command:`libsvga`, :command:`links`, :command:`ln`, :command:`locate`, :command:`ls`, :command:`lynx`, :command:`man2html` (good!), :command:`mkdir`, :command:`mkfs`, :command:`more`, :command:`mount`, :command:`mv`, :command:`pico`, :command:`pr`, :command:`retawq`, :command:`rm`, :command:`rsync`, :command:`sc`, :command:`su`, :command:`tail`, :command:`tar`, :command:`uuid`, etc. …


Example of Grub texts
---------------------


.. _starting thread:        http://forum.slitaz.org/topic/how-to-use-my-pc-mainly-in-cli
.. _home:                   http://www.slitaz.org/
.. _special page to search: http://www.slitaz.org/en/search.php
.. _doc:                    http://doc.slitaz.org/
.. _forum:                  http://forum.slitaz.org/
.. _mirror:                 http://mirror.slitaz.org/
