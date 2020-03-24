.. http://doc.slitaz.org/en:guides:faq-slim
.. en/guides/faq-slim.txt · Last modified: 2019/02/03 13:55 by linea

.. _faq-slim:

Halt, reboot or shutdown via slim
=================================

:author: hgt, linea


Symptoms
--------

Slim, the SliTaz Login Manager asks for a password when a special command like ``halt``, ``reboot`` or ``shutdown`` was entered in place of a user name.


Explanation
-----------

The required password is that from user ``root``!


Solution
--------

.. compound::
   Enter the root password and the appropriate action for ``halt``, ``reboot`` or ``shutdown`` as defined in :file:`/etc/slim.conf` with ::

     halt_cmd    /sbin/poweroff

   and ::

     reboot_cmd    /sbin/reboot

Will be performed and no login.
