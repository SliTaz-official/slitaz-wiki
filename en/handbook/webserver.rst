.. http://doc.slitaz.org/en:handbook:webserver
.. en/handbook/webserver.txt · Last modified: 2012/12/24 17:52 by trixar_za

.. _handbook webserver:

LightTPD Web Server
===================

:author: jozee, linea, gokhlayeh, trixar_za


About LightTPD
--------------

This chapter describes the configuration and use of the LightTPD web server.
It's a fast, secure, flexible HTTP server, using a small memory footprint.
It enables intelligent management of the CPU load and offers FastCGI support, CGI, Auth, Output compression and the rewriting of URLs, etc.
LightTPD is a cheap way to host your own site on an old machine.

On SliTaz the server is automatically launched at system startup and is preconfigured with PHP.
The root documents served by default are in :file:`/var/www`, this contains the default page :file:`index.html`, images are stored in the :file:`images/` directory.
LightTPD website: http://www.lighttpd.net/


:file:`/var/www` — Root directory of documents
----------------------------------------------

The :file:`/var/www` folder is the root directory of documents — you can access this via the URL http://localhost/.
If you want to host a site, you can save all your documents in here.
If you want to host multiple sites, you'll need to create virtual hosts.
Note you can also check the http://localhost/server-status.


:file:`~/Public` — Public directory of users
--------------------------------------------

SliTaz provides the users of the system a public space to place documents, HTML in general.
This directory is named :file:`Public` and must be within the root of your user space, such as :file:`/home/{hacker}/Public`.
To create this directory, use the :command:`mkdir` command:

.. code-block:: console

   $ mkdir ~/Public

You can then have access via the URL: http://localhost/~hacker/.
You can also use the machine name or IP address if you connect from another computer.


:file:`/etc/lighttpd/lighttpd.conf` — LightTPD configuration file
-----------------------------------------------------------------

The main configuration file for LightTPD (:file:`lighttpd.conf`) is located in :file:`/etc/lighttpd/`.
This file provided by SliTaz is self-explanatory, just browse.
You can find other examples on the LightTPD website.
On SliTaz you'll also find a :file:`vhosts.conf` file for the configuration of any virtual hosts (hosting several sites on the same server).


Start, stop, restart the web server
-----------------------------------

By default, SliTaz starts the server automatically at boot, to prevent this you need to remove ``lighttpd`` from the variable ``RUN_DAEMONS`` located in the system file :file:`/etc/rcS.conf`.
To start, stop or restart the server, you can use the commands: :command:`/etc/init.d/lighttpd [start|stop|restart]`.
Example to restart the server after changing the configuration file:

.. code-block:: console

   # /etc/init.d/lighttpd restart


CGI scripts using Perl
----------------------

To configure the LightTPD server to locate the path of the perl binary and use CGI/Perl, you'll need to install :program:`perl` and modify the server configuration file.
Example using :program:`Geany`:

.. code-block:: console

   # tazpkg get-install perl
   # geany /etc/lighttpd/lighttpd.conf &

.. code-block:: lighttpd

   # CGI module. You can install Perl and assign .pl and .cgi scripts
   # to /usr/bin/perl
   $HTTP["url"] =~ "/cgi-bin/" {
     cgi.assign = (
       ".sh" => "/bin/sh",
       ".cgi" => "/usr/bin/perl",
       ".pl" => "/usr/bin/perl"
     )
   }


CGI scripts using Python
------------------------

To configure the LightTPD server to locate the path of the python binary and use CGI/Python, you'll need to to install :program:`python` and modify the server configuration file.
Example using :program:`Geany`:

.. code-block:: console

   # tazpkg get-install python
   # geany /etc/lighttpd/lighttpd.conf &

.. code-block:: lighttpd

   # CGI module. You can install Python and assign .py and .cgi scripts
   # to /usr/bin/python
   $HTTP["url"] =~ "/cgi-bin/" {
     cgi.assign = (
       ".sh" => "/bin/sh",
       ".cgi" => "/usr/bin/python",
       ".py" => "/usr/bin/python"
     )
   }

For the changes to be taken into effect and to use your first CGI scripts on SliTaz, just restart the LightTPD server:

.. code-block:: console

   # /etc/init.d/lighttpd restart


Authentication — Protection for the directories
-----------------------------------------------

LightTPD provides authentication modules that can for example, protect a directory.
The server offers several authentication methods, but we will begin by using the *basic* method without encrypting any passwords.
In order to be able to use the module ``mod_auth``, you must install the :program:`lighttpd-modules` package (:command:`tazpkg get-install lighttpd-modules`).
Once installed ``mod_auth`` must be added to the list of modules:

.. code-block:: lighttpd

   # Modules to load.
   # See /usr/lib/lighttpd for all available modules.
   #
   server.modules = (
     "mod_access",
     "mod_auth",
     "...",
   )

Now you can configure the modules by specifying the debug level and method (``plain``) and the path to the file containing a list of names using a protected password to access the directories.
You must also define the directories that require authorization.
In this example we'll protect the :file:`admin/` directory and authorize its access to user ``tux`` (``user=tux``):

.. code-block:: lighttpd

   # Authentication for protected directory.
   auth.debug = 2
   auth.backend = "plain"
   auth.backend.plain.userfile = "/etc/lighttpd/plain.passwd"
   auth.require = ( "/admin/" =>
     (
     "method" => "basic",
     "realm" => "Password protected area",
     "require" => "user=tux"
     )
   )

Finally, we now create the file containing the passwords, add a user and restart the server for testing.
The basic syntax for the file is ``user:password``.
You can create the file and add a user with the :command:`echo` command or edit with your favorite text editor.
To add ``tux:root`` to the password file :file:`/etc/lighttpd/plain.passwd`:

.. code-block:: console

   # echo "tux:root" > /etc/lighttpd/plain.passwd

or:

.. code-block:: console

   # nano /etc/lighttpd/plain.passwd

To test the address http://localhost/admin/, just restart the server:

.. code-block:: console

   # /etc/init.d/lighttpd restart
