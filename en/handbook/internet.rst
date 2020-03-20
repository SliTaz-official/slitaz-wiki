.. http://doc.slitaz.org/en:handbook:internet
.. en/handbook/internet.txt · Last modified: 2015/12/11 15:46 by genesis

.. _handbook internet:

Internet
========


Midori — Lightweight web browser
--------------------------------

:program:`Midori` is a lightweight web browser with very fast page rendering through the rendering engine :program:`Webkit`.
This is a serious alternative to :program:`Firefox` for systems with low resources or those seeking a fast and sleek alternative.
It supports most web standards, CSS stylesheets and images.
:program:`Midori` is configurable via a small interface and is located in the :menuselection:`Internet` menu once installed.


Mozilla Firefox — Web Browser
-----------------------------

SliTaz is proud to provide Mozilla :program:`Firefox`, one of the world's best web browsers.
It is secure, fast, standards compliant and customizable via a system of plugins.
To install:

.. code-block:: console

   # tazpkg get-install firefox-official

The web browser configuration files are stored in the hidden (dot) directory :file:`~/.mozilla/firefox`.

.. tip::
   When you combine the LiveCD with USB media, you can keep your bookmarks and plugins wherever you go.


Retawq — Text mode web browser
------------------------------

:program:`Retawq` is an interactive web brower that can be run from the console or a graphical terminal.
To install:

.. code-block:: console

   # tazpkg get-install retawq

To start :program:`retawq`, just type (with or without the URL):

.. code-block:: console

   $ retawq http://www.slitaz.org/en

The configuration files are found in :file:`~/.retawq`, you can edit them with a text editor.
Pressing :kbd:`b` will display the bookmarks (:file:`bookmarks.html`) and the :kbd:`h` key will display the home page.


Links — Graphical & Text browser
--------------------------------

:program:`Links` was the first graphical web browser on the SliTaz LiveCD, it has since been replaced by :program:`Firefox`, but :program:`Links` is always available as a package:

.. code-block:: console

   # tazpkg get-install links

:program:`Links` offers a graphical and a text mode.
To use the graphical mode, we can use the option ``-g``:

.. code-block:: console

   $ links -g &
   $ links -g http://www.slitaz.org/en &
   $ links

The configuration files are stored in :file:`~/.links`, though it's not advisable to modify them.
However, :program:`Links` provides a configuration interface via the toolbar at the top, where you can configure the languages, bookmarks, etc.
When you change options, you must save them via the :menuselection:`menu bar --> Configuration --> Save options`.


LostIRC — IRC chat client
-------------------------

:program:`LostIRC` is a simple, yet useful IRC client that supports multiple servers and automatic joining of servers/channels.
The configuration files are located in :file:`~/.lostirc`.
Simply select from the :menuselection:`menu --> Internet --> LostIRC`.
The documentation on the website contains a lot of useful information.

.. note::
   SliTaz channel: irc.toile-libre.org / #slitaz


Ghost In The Mail — Email client
--------------------------------

To send messages quickly without having to set up an email account, you can use :program:`Ghost In The Mail` (:program:`gitmail`).
The minimal mail client offers a simple GTK interface and supports attachments.
It allows you to send mail using SMTP with your existing mail account.
To install gitmail:

.. code-block:: console

   # tazpkg get-install gitmail


Sylpheed — Mail Client
----------------------

If you'd rather have a fully featured email client — you can download :program:`Sylpheed`.
Simple, reliable and easy to use, it offers powerful search and filters, and junk mail control.

.. code-block:: console

   # tazpkg get-install sylpheed


Transmission — Lightweight BitTorrent client
--------------------------------------------

:program:`Transmission` is a GTK+ BitTorrent client that is fast, light and easy to use.
It offers a :guilabel:`Preferences` option which allows you to limit the rate of uploads/downloads, specify port, download folders, etc.

To install:

.. code-block:: console

   # tazpkg get-install transmission

Project website: http://transmission.m0k.org/


gFTP — FTP client
-----------------

The :program:`gFTP` application is a fast, ergonomic client for FTP transfers.
It can resume interrupted transfers, manage bookmarks (favorites) and FTP or HTTP proxies.
In addition :program:`gFTP` supports the use of drag and drop, can make several transfers at the same time, compares windows, remembers passwords and can even define external applications for viewing or editing files.
To install :program:`gFTP`:

.. code-block:: console

   # tazpkg get-install gftp


Gtk-gnutella — P2P client
-------------------------

:program:`Gtk-gnutella` is a P2P file sharing application that uses the Gnutella network.
Written in C, it requires a lot less resources than other clients.
It supports the use of searches and filters, features for downloading large files and bandwidth control.
To install :program:`gtk-gnutella`:

.. code-block:: console

   # tazpkg get-install gtk-gnutella
