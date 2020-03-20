.. http://doc.slitaz.org/en:cookbook:slitaztools
.. en/cookbook/slitaztools.txt · Last modified: 2015/11/26 21:06 by llev

.. _cookbook slitaztools:

SliTaz Tools
============

The SliTaz Toolbox

* Mercurial repository.
* :program:`*box` — dialog/GTKdialog.
* Archives source: `HTTP <http://download.tuxfamily.org/slitaz/sources/tools/>`_.

The SliTaz Tools contain useful scripts that enable you to customize SliTaz, such as a script to create a new initramfs or an ISO image, :file:`Makefile`, etc.
They accompany the :ref:`Scratchbook <scratchbook>` and help the SliTaz developers.
The archive is also distributed because it contains files that might be useful to hacker type individuals…
The tools are constantly evolving and continue to expand, following the cycle of changes made by the Cooking and Stable versions.


Mercurial repository
--------------------

The SliTaz Tools have their own Mercurial repository on the SliTaz server, they can be cloned via the command:

.. code-block:: console

   $ hg clone http://hg.slitaz.org/slitaz-tools/


:program:`*box`
---------------

:program:`Mountbox`, :program:`Netbox`, :program:`Bootfloopybox`, :program:`Tazlocale`, etc are tools for creating SliTaz using :program:`dialog` (:program:`ncurses`) or :program:`GTKdialog`; the scripts are contained in the directory :file:`tinyutils/`.
:program:`Desktopbox` is able to launch various boxes (:command:`desktopbox usage`) scripted or created with :program:`Glade3`.
