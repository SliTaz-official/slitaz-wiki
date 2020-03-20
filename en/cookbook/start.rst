.. http://doc.slitaz.org/en:cookbook:start
.. en/cookbook/start.txt · Last modified: 2015/11/12 14:54 by bellard

.. _cookbook:

Cookbook
========

The Cookbook brings together information about the project management, operation and development of the distribution.
It talks about creating packages, receipts, the wok, and scripts that start SliTaz.

At the base of the Cookbook is the :ref:`Scratchbook <scratchbook>`, this contains instructions to create your own LiveCD by describing the creation of the first ever public version of SliTaz in March 2007.
The Cookbook is modified by the SliTaz community and steadily improved, it provides technical instructions about the project useful to developers and advanced users.


.. rubric:: Table of contents

* :ref:`Receipts <cookbook receipt>` — Receipts (recipes) for creating (cooking) SliTaz packages.
* :ref:`Wok <cookbook wok>` — Creating SliTaz packages from source using Cookutils. (Uses a receipt).
* :ref:`Build Bot <cookbook buildbot>` — Automated cooking, testing and reporting of Wok packages.
* :ref:`Build Host <cookbook buildhost>` — The SliTaz host.
* :ref:`SliTaz Tools <cookbook slitaztools>` — The Toolbox.
* :ref:`Advanced usage of Mercurial <cookbook advancedhg>` — Add extensions to Mercurial and use MQ.
* :ref:`Release Tasks <cookbook reltasks>` — List of things to do before publishing a new version.
* :ref:`Boot Scripts <cookbook bootscripts>` — The start-up and shut-down scripts.
* :ref:`Root cdrom (rootcd) <cookbook rootcd>` — Descriptions of files contained on the cdrom.
* :ref:`Toolchain <cookbook toolchain>` — The SliTaz toolchain used to build the entire system.
* :ref:`Cross Compilation <cookbook cross>` — Technical notes about cross compilation on SliTaz (i486, ARM, X86_64).
* :ref:`Old Cookbook <oldcookbook>` — Deprecated Cookbook notes and manuals (using Tazwok).
* :ref:`Contributors <cookbook contributors>` — Contributors access to the SliTaz servers.


.. toctree::
   :maxdepth: 5

   receipt
   wok
   buildbot
   buildhost
   slitaztools
   advancedhg
   reltasks
   relnotes
   bootscripts
   rootcd
   toolchain
   cross
   contributors
