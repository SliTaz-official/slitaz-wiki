.. doc.slitaz.org/en:guides:database
.. en/guides/database.txt · Last modified: 2014/11/09 17:55 by linea

.. _database:

The SQLite database
===================

:author: emgi,  linea

:program:`SQLite` is a lightweight SQL database and as such perfectly suitable for use with Slitaz.
Lightweight does however not imply that its functionality is also limited; rather the opposite!

To begin with, the program in all its aspects is extensively documented on: http://www.sqlite.org/.
This leaves fairly little left to be documented elsewhere.
If you have never worked with SQL before, this guide will provide sufficient information to get you started and to determine whether SQLite is the proper choice for your database.

Contrary to virtually all other SQL database solutions, you don't need a server to run SQlite.
The database consists of a single file which is accessed via the :command:`sqlite` command line tool or through a programming language like Perl or Python.
Apart from the inherent simplicity, the big advantage of this approach is that it is VERY fast, even while running on low-end hardware.
Backups are easy too; you just save the file holding the database to another location.
These properties make SQLite probably the best choice for single user and embedded solutions.
Small-scale CGI is another application which works very well with SQLite.
All of this and more is described on: http://www.sqlite.org/whentouse.html


What do I need to get started with SQLite?
------------------------------------------

To begin with SQLite on Slitaz, you only need to install the sqlite client package:

.. code-block:: console

   # tazpkg -gi sqlite

This installs the sqlite client in your :file:`/usr/bin` directory.
The version offered with slitaz is a stable release but not the latest one available.
SQLite is under active development and new versions appear on a regular basis.
If you need a more recent version you may use the precompiled binary which is available on the SQLite.org download page.
To do this, you can simply download the precompiled binary and save it in the :file:`/usr/bin/` folder, overwriting the file from the tazpkg installation if it exists.

.. important::
   The version of the SQLite client is only backwards compatible with the database file.
   You cannot access a database which was created using, for example, version 3.7.16.2 with an older release of the client like 3.7.9.
   The opposite — a newer version of the client accessing a database which was created with a previous version though is generally possible.
   This can be relevant if you need to access a database from different client systems.
   Best practice is to use the same version of the client everywhere.

To run SQLite with Perl, one additional package must be installed; the perl-sqlite module:

.. code-block:: console

   # tazpkg -gi perl-dbd-sqlite

Using SQLite with Perl, Python or any other language is documented extensively on various websites and describing it here would be out of scope as far as this document is concerned.
The saying "Google is your friend" really applies in this case.

The sqlite client :file:`/usr/bin/sqlite3` allows you to do anything from the command line which also can be done using Perl, Python or any other language.
You can print, update, add or delete records and manipulate columns and tables.
Starting the program without any parameters or options returns a ``>`` prompt.
You may enter sql commands terminated by the semicolon (``;``) as is normal in all sql dialects.
Queries must contain a reference to the database file and the sql part must be between double quotes and terminated by a semicolon (``;``).
One small example:

.. code-block:: console

   $ sqlite3 /home/tux/yourdatabase "select * from sqlite_master;"

This command prints the layout of the tables in the database.
To create a new database, set the filename to whatever is deemed suitable.
A new database is created if the file does not exist.
 
Please refer to http://www.sqlite.org/lang.html for the commands to create tables and add columns to them.
Its all pretty straightforward and virtually the same as with most other SQL versions.
If you are completely new to SQL, you will have something of a learning curve but many things picked up here can be used with other SQL versions too.
It's certainly worth it! 

Copying or renaming the file containing the database is also trivial.
To make a copy simply type:

.. code-block:: console

   $ cp /home/tux/yourdatabase /home/tux/mydatabase

The filesize depends heavily on the type of database and its actual contents.
The more data is entered, the larger the file becomes.
Please note that the file does not shrink when a large amount of data is deleted.
Instead, the empty space is preserved and re-used before the file starts to increase in size again.
Something else to keep in mind is that the file permissions must be set correctly to be able to read or write the database.


Keeping sqlite up-to-date
-------------------------

SQLite is still under development and new versions are made available every one-two months.
You can run the latest client without compiling and installing a package every time.
In the end, it is just one binary file: :file:`sqlite3`.
This file is available as a precompiled binary for download from www.sqlite.org.
All that's needed is to move it to the correct folder, thereby replacing the old version.
A sample script to automate this action could look like this:

.. code-block:: shell

   cd /home/<your-home-dir>
   if [ -s sqlite3 ]; then
    rm sqlite3.old
    mv /usr/bin/sqlite3 sqlite3.old
    mv sqlite3 /usr/bin/
    chmod 755 /usr/bin/sqlite3
   fi

This script looks for a file named :file:`sqlite3` in :file:`/home/{your-home-dir}`, if found it moves the old :file:`sqlite3` binary to :file:`sqlite3.old` in :file:`/home/{your-home-dir}` and installs the latest version with the correct access mode in :file:`/usr/bin`.
