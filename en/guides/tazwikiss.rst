.. http://doc.slitaz.org/en:guides:tazwikiss
.. en/guides/tazwikiss.txt · Last modified: 2017/03/15 19:58 by linea

.. _tazwikiss:

TazWiKiss
=========


Introduction
------------

:program:`TazWikiss` is a port of the wiki software `WiKiss <http://wikiss.tuxfamily.org/>`_.
As it is a CGI application, it needs a web server supporting CGI and a browser — on SliTaz these are by default :program:`httpd` (in busybox) and :program:`tazweb`.
The CGI scripts are shell scripts, so no other language interpreter is needed.

The software is installed in :file:`/var/www/wiki` and the wiki pages are (by definition in the config file: :file:`/var/www/wiki/config.sh`) stored in :file:`/var/www/wiki/pages`.

The wiki pages are plain text files without any subdirectory hierarchy.


Requirements
------------

A running web server supporting CGI and a web browser.


Start TazWiKiss
---------------


via a desktop menu
^^^^^^^^^^^^^^^^^^

:menuselection:`Accessories --> Wiki documents`


via a command
^^^^^^^^^^^^^

.. code-block:: console

   $ tazweb http://localhost.wiki.index.sh

When logged in on the same system where the web server is running, else localhost is to be replaced by a name or IP address of the system where the wiki resides.


Installation
------------

When :program:`TazWiKiss` is not incorporated in the flavor you installed, you can add it by installing the package :program:`slitaz-dev-tools`:

.. code-block:: console

   # tazpkg -gi slitaz-dev-tools


Hints
-----


Help on wiki syntax
^^^^^^^^^^^^^^^^^^^

Help on the syntax is available from the welcome page of the delivered wiki by hyperlink ``help`` or ``helptable``.


Hide wiki pages
^^^^^^^^^^^^^^^

To hide pages, the user can set a password on the page by inserting

.. code-block:: text

   {PASSWORD=read-password}


Protection of wiki
^^^^^^^^^^^^^^^^^^

To prevent unauthorised users from modifying the wiki, a password can be set in :file:`/var/www/wiki/config.sh` (or a language specific configuration file) by replacing ``PASSWORD=""`` with ``PASSWORD="modification-password"``.
