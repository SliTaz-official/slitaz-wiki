.. http://doc.slitaz.org/en:cookbook:toolchain
.. en/cookbook/toolchain.txt · Last modified: 2014/11/19 18:00 by linea

.. _cookbook toolchain:

Toolchain
=========

The Toolchain is the set of packages used to build the entire system.
It consists of :program:`Binutils`, :program:`GCC`, :program:`Linux API headers` and the :program:`GNU libc` aka :program:`Glibc`.
SliTaz has an annual development cycle, so the toolchain gets a huge update once a year just after a stable release.
When the toolchain changes we must rebuild all packages to ensure quality and consistency.

On SliTaz you have 2 ways to rebuild a toolchain from scratch: :program:`Cookutils` and :program:`Tazwok`.
:program:`Tazwok` is historically the first packages builder for SliTaz and over time has evolved into a powerful tool that can rebuild the full system from scratch.
More info: :ref:`new tazwok illustrated`.
(Tazwok has been deprecated since SliTaz v4).

:program:`Cookutils` are the new tools, written from scratch as before and for SliTaz 4.0.
The build tools needed a huge improvement since they were first written when SliTaz had 400-500 packages, when :program:`Cookutils` was started SliTaz had 2960 packages, so the way to handle all these packages is even more complex than at the beginning of the project.
Also the :program:`Cookutils` have been written with simplicity and speed in mind and are used on the official SliTaz build host aka Tank.

To rebuild the official toolchain with :program:`Cookutils` you must use the package :program:`slitaz-toolchain`.
This package is used to install the toolchain in a development environment and builds the :program:`Toolchain` in the correct order with 2 passes for :program:`binutils` and :program:`GCC`.
You will find some info in the :program:`slitaz-toolchain` receipt itself and after building we keep a note in :file:`/usr/share/doc/slitaz/toolchain.txt`

Recooking the full toolchain can be slow:

.. code-block:: console

   # cook slitaz-toolchain
