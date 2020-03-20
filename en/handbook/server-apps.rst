.. http://doc.slitaz.org/en:handbook:server-apps
.. en/handbook/server-apps.txt · Last modified: 2010/07/08 17:14 (external edit)

.. _handbook server-apps:

Server applications
===================


Dokuwiki
--------

`Dokuwiki <http://www.dokuwiki.org/>`_ is a light and powerful Wiki engine using :program:`PHP` and flat files as a backend; so no database server is needed.
To use :program:`Dokuwiki` you just need to install a web server (:program:`lighttpd` or :program:`apache`) and :program:`PHP`.


.. _handbook server-apps drupal:

Drupal
------

:program:`Drupal` is a powerful CMS (Content Management System) using :program:`PHP` server side language and a MySQL database.
To install :program:`Drupal` and have it running in a few minutes you must first install the web server (:program:`lighttpd`), :program:`PHP` and :program:`MySQL`:

.. code-block:: console

   # tazpkg get-install lighttpd
   # tazpkg get-install php
   # tazpkg get-install mysql

Download the latest version from http://drupal.org/ and create a virtual host or go into your public directory and untar the :program:`Drupal` sources:

.. code-block:: console

   $ mkdir ~/Public && cd Public
   $ tar xzf drupal-*

Now we have to modify some file permissions so that :program:`Drupal` can write to the filesystem during the installation process and when running:

.. code-block:: console

   $ cd drupal-*/sites
   $ chmod 777 default
   $ cp default/default.settings.php default/settings.php
   $ chmod 666 default/settings.php

Create a MySQL database and then use your favorite web browser to install :program:`Drupal` through the web interface.
When the installation process has ended you must modify file permissions and can start to customize the application.
Change permissions for production and clean-up:

.. code-block:: console

   $ cd ..
   $ chmod 755 sites/default
   $ chmod 644 sites/default/settings.php

To ensure easy upgrades of your :program:`Drupal` core installation you should put all additional modules and themes into the directory: :file:`site/all/`.
So to prepare the addition of any future modules and themes:

.. code-block:: console

   $ mkdir sites/all/modules
   $ mkdir sites/all/themes


Online
------

* `Drupal Modules <http://drupal.org/project/modules>`_
* `Drupal Themes <http://drupal.org/project/themes>`_
