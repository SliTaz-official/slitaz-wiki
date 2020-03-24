.. http://doc.slitaz.org/en:cookbook:advancedhg
.. en/cookbook/advancedhg.txt · Last modified: 2015/11/26 21:16 by llev

.. _cookbook advancedhg:

Advanced usage of Mercurial
===========================

:author: gokhlayeh, linea, llev

Use an external tool to merge
-----------------------------

If you want to use tools presented on this page, :program:`MQ` particularly has a tool to manage merges (when several commits overwrite each other and you have to edit the result manually) which will probably be useful to you.
SliTaz proposes :program:`Meld`, a light software that can do that well.
After installation, tell :program:`Mercurial` to use it if necessary by putting it in your :file:`~/.hgrc`:

.. code-block:: ini

   [ui]
   merge = meld


Useful extensions
-----------------

To add an extension, you can use the :file:`~/.hgrc` file:

.. code-block:: ini

   [extensions]
   name = adress

Some extensions are packed within :program:`Mercurial`, so it's not necessary to give them addressess.
It's the case of the four following:

color:
  Add color in :program:`Mercurial`.
  Useful when displaying differences between several versions of a file.

hgext.fetch:
  Add the command :command:`hg fetch`, which regroups :command:`hg pull && hg merge && hg update`.

hgext.graphlog:
  Add the command :command:`glog`, which displays the revision tree along with the log.
  It's advised to limit the length of the log with option ``-l`` (i.e.: ``-l 10``).
  Option ``-p`` displays the detail of differences introduced at each commit.

mq:
  This tool is explained in detail below.
  It allows you to manage a patch-list for a Mercurial repository: apply them, unapply, update, etc.
  This extension adds several commands which generally start with 'q'.
  Some webpages detail this tool, search: ``mercurial mq``.


Basic functionality of MQ
-------------------------

In a mercurial repository, create a patch repository with a controlled revision; it's a repository of patches into which the changes can be committed using :program:`Mercurial`, like a repository within a repository::

  hg qinit -c

After modifications, save them as a patch instead of committing them::

  hg qnew nom_du_patch

List applied/unapplied patches::

  hg qseries -v

Add changes to the current patch (the last one applied)::

  hg qrefresh

Apply the next patch from the queue::

  hg qpush

Apply all patches::

  hg qpush -a

Unapply current patch::

  hg qpop

Unapply all patches::

  hg qpop -a

Go to a given patch in the queue::

  hg qgoto patch

Add a message to the current patch (before committing it)::

  hg qrefresh -m "Message"

Transform a patch into a commit::

  hg qfinish patch

Commit changes made in the patch repository::

  hg qcommit -m "Message de commit"

.. note::
   Patches are saved into :file:`.hg/patches`.
   The file :file:`.hg/patches/series` can be manually edited to change the application order of the patches; but take care if several patches have the same target file: it can create problems.


MQ & Merge
----------

**General idea**

Patches can be updated using the merge tool of :program:`Mercurial`: it's easier than editing them manually.
To do this, it's necessary to have two heads in the repository.
One being the repository with patches applied ontop; the other the repository with the new commits/updates/etc::

  o New repository revision
  |
  |
  | o Patches
  | |
  | /
  |
  o Repository before patch application

The patches branch will next be merged into the new branch and :program:`MQ` will use the merge function from :program:`Mercurial` to update the patches.
Please note that using an external merge tool (such as :program:`meld` proposed previously) is highly recommended.

Create the head patches:

.. code-block:: shell

   qpush -a
   hg tags  # Remember/Note the number of the revision qparent
   qsave -e -c # Save the status of the patches,
               # this save will be used during the merge.
               # (Remember/Note the number at the end of patches.N;
               # it's generally 1)

Create the new head:

.. code-block:: shell

   hg update -C <N qparent> # Go to the revision noted before.
   
   # Next, do what you planned to:
   # Update:
   hg pull -u
   # Commit new changes, make the modifications then:
   hg commit -m "message"
   # Edit a patch:
   hg qgoto patch # Then do the modifications and:
   hg qrefresh

To launch the merge::

  hg qpush -a -m

Clean the repository::

  hg qpop -a
  hg qpop -a -n patches.N
  rm -r .hg/patches.N

Save the changes made into the patch repository::

  hg qcommit -m "Updated to match rev???"

Re-apply the patch queue::

  hg qpush -a
