.. http://doc.slitaz.org/en:handbook:development
.. en/handbook/development.txt · Last modified: 2016/10/24 16:24 by hgt

.. _handbook development:

Development
===========


About Development
-----------------

SliTaz provides development tools for web design, editing scripts and source code.
On the website, the Development page will give you general information about the developers and opportunities for involvement.


SHell scripts
-------------

Writing SHell scripts is the easiest way to start coding, they can provide quick results and the only prerequisites are being able to open a terminal and using a text editor such as :program:`Nano`, :program:`Leafpad` or :program:`Geany`.
SHell scripts can do many things on a GNU/Linux system — initialize the system, make backups, perform repetitive tasks, display system information, create or modify files and so on.
In a SHell script you can use variables, functions or calls to include a file.
Note that you can name your script as you see fit and the :file:`.sh` extension is widely used.


.. rubric:: Create a SHell script

Before starting a new SHell script, you must pay attention to the interpreter used.
Most SHell scripts use :file:`/bin/sh`, because it's more portable, but there are scripts that rely on :file:`/bin/bash` and this must be installed on the system.
For a SHell script to function it must be made executable by changing permissions on the command line using the :command:`chmod` tool.
To create a :file:`script.sh` and make it executable:

.. code-block:: console

   $ touch script.sh
   $ chmod +x script.sh

Now that you have a new executable file, you can edit it.
You can continue to stay in the terminal and use the :program:`Nano` editor (:kbd:`Ctrl`\ +\ :kbd:`X` to save & exit) or IDE :program:`Beaver` to edit:

.. code-block:: console

   $ nano script.sh

Or:

.. code-block:: console

   $ beaver script.sh &

Here's a script that contains a variable ``NAME`` and displays the value with the :command:`echo` command:

.. code-block:: shell

   #!/bin/sh
   
   NAME="kayam"
   
   echo "$NAME is nice."

Once you have created/modified your :file:`script.sh`, you can execute it to see the result:

.. code-block:: console

   $ ./script.sh

So much for this brief introduction to SHell scripts.
The Web is full of information if you wish to explore further.


Dialog
------

:program:`Dialog` can create GUI-based consoles such as the SliTaz 'installer'.
The configuration files are :file:`/etc/dialogrc` and/or :file:`~/dialogrc` for each user.
Here's a simple example of using :command:`dialog` via a console or terminal:

.. code-block:: console

   $ dialog --title "Hello $USER" \
     --msgbox "Message made by dialog." 5 54

You can find plenty of example scripts in the :file:`/sample` directory inside the source code of :program:`dialog` which can be downloaded from: http://invisible-island.net/dialog/dialog.html.
Download sources and decompress:

.. code-block:: console

   $ wget ftp://invisible-island.net/dialog/dialog.tar.gz
   $ tar xzf dialog.tar.gz


Beaver
------

:program:`Beaver` is a simple, lightweight and fast code editor offering syntax.
:program:`Beaver` can be found in the :menuselection:`Menu --> Development --> Code Editor`.
Once launched for the first time you can adjust your preferences through the :guilabel:`Edit` button.
You can run it through a X terminal with the following command:

.. code-block:: console

   $ beaver &


Geany IDE
---------

:program:`Geany` is an IDE, or Integrated Development Environment.
It is a simple, small and lightweight application, offering syntax, tabs and self-completion.


.. rubric:: Executing Geany

You can install :program:`Geany`:

.. code-block:: console

   # tazpkg get-install geany

After it runs for the first time, you can set your preferences through the menu :menuselection:`Edit --> Preferences` option.
It is also possible to run :program:`Geany` via terminal:

.. code-block:: console

   $ geany &

.. note::
   When compiling source code, the :file:`./configure` script offers the ``-enable-the-force`` option, which you could use if you feel the need to became a Jedi warrior!


Perl or Microperl — Code/use Perl scripts
-----------------------------------------

On SliTaz you can use the powerful scripting language :program:`Perl` via the :file:`perl` or :file:`microperl` binary.
:program:`Microperl` is a streamlined version of :program:`perl` compiled from official sources, :program:`Perl` scripts running :program:`Microperl` are compatible with the complete version of :program:`Perl`.
One of :program:`Perl`\ 's strengths is its portability, it can be used on any system and it's an interpreted language, which means that the code doesn't need to be compiled and can be used directly.
On SliTaz, :program:`Perl` and :program:`Microperl` are not installed by default on the LiveCD: you can either rebuild your ISO or install through the package manager.

.. note::
   :program:`Microperl` is only 1 MB and provides no modules:

   .. code-block:: console

      # tazpkg install perl

   Or:

   .. code-block:: console

      # tazpkg install microperl


.. rubric:: Hello world!

The purpose of this script is to display *Hello World*.
You can start by creating the file and making it executable on the command line and then editing with :program:`Beaver`.
Note the script is called :file:`hello.pl`, but you can name it as you see fit, with or without the :file:`.pl` extension:

.. code-block:: console

   $ touch hello.pl
   $ chmod +x hello.pl
   $ beaver hello.pl &

The first line of a :program:`Perl` script begins by defining the path to the :program:`Perl` interpreter, usually :file:`/usr/bin/perl` and to display text, just use the :command:`print` command.
It should be noted that :program:`Perl` is case sensitive and a line of code should always end with a semicolon.
Example code (you can copy and paste):

.. code-block:: perl

   #!/usr/bin/perl
   #
   
   print "Hello World!\n";

To execute and test the script:

.. code-block:: console

   $ ./hello.pl


.. rubric:: CGI Scripts and Perl

CGI scripts are designed to display dynamically generated web pages.
The :program:`Perl` language associated with the :program:`LightTPD` web server allows you to use CGI scripts through your public space or via virtual hosts.
:program:`Perl` is quite adapted to Web 2.0 and can generate xHTML pages.
On SliTaz you must have :program:`Perl` or :program:`Microperl` installed and the :program:`LightTPD` server configured before you can use CGI scripts coded in :program:`Perl`.
Note that by default SHell scripts (:file:`.sh`) can be placed in :file:`/cgi-bin/`.

Once the server is properly configured, you can put your CGI in your :file:`$HOME/Public/cgi-bin` using the :file:`.pl` or :file:`.cgi` extension and view them either locally or remotely.
Example of using a :program:`Perl` CGI script:

.. code-block:: perl

   #!/usr/bin/perl
   #
   print "content-type : text/html\n\n";
   
   print "Hello World!\n";


Python
------

The :program:`Python` programming language is available as an installable package.
Once installed, you can create your own scripts/programs and use CGI applications with the :program:`LightTPD` web server, taking care to configure the server properly.
The official SliTaz Mercurial repositories are provided by a CGI/Python web interface — a solution suited to a product that's reliable and robust.
To install the :program:`python` package with :program:`tazpkg`:

.. code-block:: console

   # tazpkg get-install python


Ruby
----

The :program:`Ruby` programming language is available as an installable package.
:program:`Ruby` is (to quote the official website):

  "A dynamic, open source programming language with a focus on simplicity and productivity.
  It has an elegant syntax that is natural to read and easy to write".

Ruby handles exceptions, supports Object-Orientated Programming (OOP), automatic memory management and is highly portable.
To install the :program:`ruby` package with :program:`tazpkg`:

.. code-block:: console

   # tazpkg get-install ruby


Toolchain — Libraries, C compiler and tools
-------------------------------------------

To compile software from sources or your own code, you need at least the basic *toolchain*, comprising of :program:`Binutils`, :program:`Glibc`, C compiler, Kernel headers and the :program:`Make` utility.
Note that the *toolchain* is used by the SliTaz developers to compile the entire system from source.
To install the meta package and all dependencies:

.. code-block:: console

   # tazpkg get-install slitaz-toolchain

The installation of the *toolchain* can now compile basic applications in console mode without a problem using the :program:`Busybox` Ash SHell, but some other packages will not compile without :program:`Bash`.
GNU :program:`Bash` is available as a package along with various other development tools such as :program:`Flex`, :program:`M4`, :program:`Bison` or :program:`Pkg-config`.
If you are looking for :program:`pkg-config` for example:

.. code-block:: console

   $ tazpkg search pkg-config

If you would like to compile applications utilizing the :program:`Ncurses` library, you must install the :program:`ncurses-dev` package.
Note the :program:`ncurses` package also provides a variety of small programs such as :command:`tic` or :program:`tac`:

.. code-block:: console

   $ tazpkg search ncurses
