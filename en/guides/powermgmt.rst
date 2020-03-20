.. http://doc.slitaz.org/en:guides:powermgmt
.. en/guides/powermgmt.txt · Last modified: 2011/01/15 19:32 by kultex

.. _powermgmt:

ACPI (Advanced Configuration and Power Interface)
=================================================

To install the :program:`acpi` daemon and modules:

.. code-block:: console

   # tazpkg get-install acpid
   # tazpkg get-install linux-acpi

The :program:`acpi` daemon needs to be started before :program:`dbus` and :program:`hal`, so edit your :file:`/etc/rcS.conf` to look like this:

.. code-block:: shell

   RUN_DAEMONS="acpid, dbus, hald, etc..."

Or, to start manually:

.. code-block:: console

   # /etc/init.d/hald stop
   # /etc/init.d/dbus stop
   # /etc/init.d/acpid start
   # /etc/init.d/dbus start
   # /etc/init.d/hald start

To check it's working, just:

.. code-block:: console

   $ cat /proc/acpi/battery/{BAT0}/info

You can find some example scripts in :file:`/etc/acpi`.
