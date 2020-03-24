.. http://doc.slitaz.org/en:guides:bugs
.. en/guides/bugs.txt · Last modified: 2010/08/12 23:00 by linea

.. _bugs:

BUGS
====

:author: stork, linea

The problems listed in this page refer to the Slitaz 3.0 environment.


Burning a CD with :program:`Burnbox`
------------------------------------

To burn a CD, you have to be root (it will NOT work from your default TUX account, even if TUX has been added to the ``cdrom`` group).

In :program:`Burnbox`; enclose the file and folder names between quotes if they contain a space (eg ``"My Documents"``), otherwise it will fail to find the document.


:program:`Abiword`
------------------

With :program:`AbiWord` you MUST install :program:`glibc-locale` too, it was forgotten as a dependency, but it is needed for :program:`AbiWord` to work properly.


Chat entry of the Internet menu
-------------------------------

This should install :program:`pidgin` and :program:`skype`.
Practically it has sometimes (but not always) installed :program:`pidgin`, and always failed to install :program:`Skype`.
Install :program:`Pigin` and :program:`Skype` (well, :program:`get-skype`) from the package manager.
