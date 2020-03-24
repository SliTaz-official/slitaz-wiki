.. http://doc.slitaz.org/en:cookbook:start
.. en/cookbook/start.txt · Last modified: 2015/11/12 14:54 by bellard

.. _cookbook:

Cookbook
========

:author: jozee, int3, seawolf, linea, gokhlayeh, pankso, brianperry, bellard

The Cookbook brings together information about the project management, operation and development of the distribution.
It talks about creating packages, receipts, the wok, and scripts that start SliTaz.

At the base of the Cookbook is the :ref:`scratchbook`, this contains instructions to create your own LiveCD by describing the creation of the first ever public version of SliTaz in March 2007.
The Cookbook is modified by the SliTaz community and steadily improved, it provides technical instructions about the project useful to developers and advanced users.


.. rubric:: Table of contents

* :ref:`cookbook receipt` — Receipts (recipes) for creating (cooking) SliTaz packages.
* :ref:`cookbook wok` — Creating SliTaz packages from source using Cookutils. (Uses a receipt).
* :ref:`cookbook buildbot` — Automated cooking, testing and reporting of Wok packages.
* :ref:`cookbook buildhost` — The SliTaz host.
* :ref:`cookbook slitaztools` — The Toolbox.
* :ref:`cookbook advancedhg` — Add extensions to Mercurial and use MQ.
* :ref:`cookbook reltasks` — List of things to do before publishing a new version.
* :ref:`cookbook bootscripts` — The start-up and shut-down scripts.
* :ref:`cookbook rootcd` — Descriptions of files contained on the cdrom.
* :ref:`cookbook toolchain` — The SliTaz toolchain used to build the entire system.
* :ref:`cookbook cross` — Technical notes about cross compilation on SliTaz (i486, ARM, X86_64).
* :ref:`oldcookbook` — Deprecated Cookbook notes and manuals (using Tazwok).
* :ref:`cookbook contributors` — Contributors access to the SliTaz servers.


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
