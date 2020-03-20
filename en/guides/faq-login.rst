.. http://doc.slitaz.org/en:guides:faq-login
.. en/guides/faq-login.txt · Last modified: 2012/04/28 05:30 by mojo

.. _faq-login:

Cannot Login to Desktop
=======================


Symptoms
--------

* :program:`SLiM`, the SliTaz Login Manager, fails with the message::

    failed to execute login command

.. warning::
   Verify the SliTaz install partition is not full and formated with a linux filesystem such as ext3.
   Using fat32 or ntfs filesystems cause login failure.


Explanation
-----------

The following files must exist, this can be verified with the :command:`ls -la` command:

* :file:`.Xdefaults`
* :file:`.xinitrc`
* something else?


Solution
--------

You should copy the default files from the template located in the :file:`/etc/skel` directory.
This happens automatically when a new user is created with the SliTaz Control Box, but not when using the command-line utilities.
Occasionally users experience these files being removed or modified/broken.

.. compound::

   Switch to the root (super) user::

     su root

   Change to the affected users :file:`/home` directory::

     cd /home/USERNAME

   Set the shell options to allow the dot (.) to be included in file names::

     shopt -s dotglob

   Copy all files, recursively::

     cp -r /etc/skel/* /home/USERNAME

   Change ownership of all files and directories in the user's home to that of the affected user::

     chown -R USERNAME:USERGROUP /home/USERNAME/*

   Restore the shell options::

     shopt -u dotglob

   The essential files should now be restored!


EDIT::

  shopt -u dotglob

is not working in :file:`slitaz-3.0.iso` — instead of this run in addition::

  chown -R USERNAME:USERGROUP /home/USERNAME/.[a-zA-Z0-9]*
