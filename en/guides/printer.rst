.. http://doc.slitaz.org/en:guides:printer
.. en/guides/printer.txt · Last modified: 2010/10/15 19:22 by linea

.. _printer:

Printing
========

:author: papagoose, jozee, linea, seawolf

The CUPS (Common Unix Printing System) software manages printers connected to the local computer or over a network.
The local printers are attached via a loop-back connection (IP 127.0.0.1), a network connection pointing to the same system.


Installing a Network Printer
----------------------------

Install CUPS and add the user(s) to the ``lp`` group to gain permissions to access the devices.
This can be done as root in a terminal:

.. code-block:: console

   # tazpkg get-install cups
   # addgroup tux lp

Again as root, customise the default configuration in :file:`/etc/cupsd.conf`::

  # Administrator user group...
  SystemGroup lp
  
  # Restrict access to the admin pages...
  
    Order allow,deny
    Allow 127.0.0.1
  
  
  # Restrict access to configuration files...
  
    AuthType Default
    Require user @SYSTEM
    Order allow,deny
    Deny From None
    Allow From 127.0.0.1

Set the administrative password for CUPS and add an administrative user:

.. code-block:: console

   # lppasswd -g lp -a tux

You should now be able to have access to the CUPS administration through your browser by entering the address http://localhost:631/

Install the printer drivers with the following packages:

* :program:`hplip`: for HP printers
* :program:`gutenprint`: for Canon, Epson, Lexmark, Sony, Olympus
* :program:`foomatic-db`, :program:`foomatic-db-engine`, :program:`foomatic-db-nonfree` and :program:`foomatic-filters`: several free software printer drivers
* :program:`ufr2`: for Canon printers (extra drivers)
* :program:`splix`: for Samsung
* :program:`cups-pdf`: for "printing" to PDF files

For example, install the HP printer drivers with the following *root* command

.. code-block:: console

   # tazpkg get-install hplip

Now you add a new printer in the browser interface and choose: :guilabel:`LPD/LPR Host or Printer (Unknown)`, and add the address socket//192.168.2.1:9100

If you get the error, "*Returning IPP client-error-document-format-not-supported for Print-Job in /var/log/cups/error.log*" then add two files to the :file:`/etc/cups` directory.
The easiest method is to start a text editor such as :program:`Leafpad` from a root terminal; start the Terminal and switch to the root user.

* :file:`mime.convs` (:file:`/etc/cups/mime.convs`)::

    application/pdf          application/postscript          33  pdftops
    application/postscript   application/vnd.cups-postscript 66  pstops
    application/vnd.hp-HPGL  application/postscript          66  hpgltops
    application/x-cshell     application/postscript          33  texttops
    application/x-csource    application/postscript          33  texttops
    application/x-perl       application/postscript          33  texttops
    application/x-shell      application/postscript          33  texttops
    text/plain               application/postscript          33  texttops
    text/html                application/postscript          33  texttops
    image/gif                application/vnd.cups-postscript 66  imagetops
    image/png                application/vnd.cups-postscript 66  imagetops
    image/jpeg               application/vnd.cups-postscript 66  imagetops
    image/tiff               application/vnd.cups-postscript 66  imagetops
    image/x-bitmap           application/vnd.cups-postscript 66  imagetops
    image/x-photocd          application/vnd.cups-postscript 66  imagetops
    image/x-portable-anymap  application/vnd.cups-postscript 66  imagetops
    image/x-portable-bitmap  application/vnd.cups-postscript 66  imagetops
    image/x-portable-graymap application/vnd.cups-postscript 66  imagetops
    image/x-portable-pixmap  application/vnd.cups-postscript 66  imagetops
    image/x-sgi-rgb          application/vnd.cups-postscript 66  imagetops
    image/x-xbitmap          application/vnd.cups-postscript 66  imagetops
    image/x-xpixmap          application/vnd.cups-postscript 66  imagetops
    image/x-sun-raster       application/vnd.cups-postscript 66  imagetops
    
    
    image/gif                application/vnd.cups-raster 100 imagetoraster
    image/png                application/vnd.cups-raster 100 imagetoraster
    image/jpeg               application/vnd.cups-raster 100 imagetoraster
    image/tiff               application/vnd.cups-raster 100 imagetoraster
    image/x-bitmap           application/vnd.cups-raster 100 imagetoraster
    image/x-photocd          application/vnd.cups-raster 100 imagetoraster
    image/x-portable-anymap  application/vnd.cups-raster 100 imagetoraster
    image/x-portable-bitmap  application/vnd.cups-raster 100 imagetoraster
    image/x-portable-graymap application/vnd.cups-raster 100 imagetoraster
    image/x-portable-pixmap  application/vnd.cups-raster 100 imagetoraster
    image/x-sgi-rgb          application/vnd.cups-raster 100 imagetoraster
    image/x-xbitmap          application/vnd.cups-raster 100 imagetoraster
    image/x-xpixmap          application/vnd.cups-raster 100 imagetoraster
    image/x-sun-raster       application/vnd.cups-raster 100 imagetoraster
    
    application/vnd.cups-postscript application/vnd.cups-raster 100 pstoraster

* :file:`mime.types` (:file:`/etc/cups/mime.types`)::

    application/pdf		pdf string(0,%PDF)
    application/postscript	ai eps ps string(0,%!) string(0,<04>%!) \
    	contains(0,128,<1B>%-12345X) + \
    	(contains(0,4096,"LANGUAGE=POSTSCRIPT") \
    	 contains(0,4096,"LANGUAGE = Postscript") \
    	 contains(0,4096,"LANGUAGE = PostScript") \
    	 contains(0,4096,"LANGUAGE = POSTSCRIPT") \
    	 (contains(0,4096,<0a>%!) + \
    	  !contains(0,4096,"ENTER LANGUAGE")))
    application/vnd.hp-HPGL		hpgl \
    	string(0,<1B>E<1B>%0B) \
    	string(0,<1B>%-1B) string(0,<201B>)\
    	string(0,BP;) string(0,IN;) string(0,DF;) \
    	string(0,BPINPS;) \
    	(contains(0,128,<1B>%-12345X) + \
    	 (contains(0,4096,"LANGUAGE=HPGL") \
    	  contains(0,4096,"LANGUAGE = HPGL")))
    
    ######################################################################
    #
    # Image files...
    #
    
    image/gif                 gif string(0,GIF87a) string(0,GIF89a)
    image/png                 png string(0,<89>PNG)
    image/jpeg                jpeg jpg jpe string(0,) &&\
    	(char(3,0xe0) char(3,0xe1) char(3,0xe2) char(3,0xe3)\
    	 char(3,0xe4) char(3,0xe5) char(3,0xe6) char(3,0xe7)\
    	 char(3,0xe8) char(3,0xe9) char(3,0xea) char(3,0xeb)\
    	 char(3,0xec) char(3,0xed) char(3,0xee) char(3,0xef))
    image/tiff              tiff tif string(0,MM<002A>) string(0,II<2A00>)
    image/x-photocd           pcd string(2048,PCD_IPI)
    image/x-portable-anymap   pnm
    image/x-portable-bitmap   pbm string(0,P1) string(0,P4)
    image/x-portable-graymap  pgm string(0,P2) string(0,P5)
    image/x-portable-pixmap   ppm string(0,P3) string(0,P6)
    image/x-sgi-rgb           rgb sgi bw icon short(0,474)
    image/x-xbitmap           xbm
    image/x-xpixmap           xpm ascii(0,1024) + string(3,"XPM")
    #image/x-xwindowdump      xwd string(4,<00000007>)
    image/x-sun-raster        ras string(0,<59a66a95>)
    
    #image/fpx                fpx
    image/x-alias             pix short(8,8) short(8,24)
    image/x-bitmap            bmp string(0,BM) && !printable(2,14)
    image/x-icon              ico
    
    ######################################################################
    #
    # Text files...
    #
    
    application/x-cshell      csh printable(0,1024) + string(0,#!) +\
    	(contains(2,80,/csh) contains(2,80,/tcsh))
    application/x-perl        pl printable(0,1024) + string(0,#!) +\
    	contains(2,80,/perl)
    application/x-shell       sh printable(0,1024) + string(0,#!) +\
    	(contains(2,80,/bash) contains(2,80,/ksh)\
    	 contains(2,80,/sh) contains(2,80,/zsh))
    application/x-csource     c cxx cpp cc C h hpp \
    	printable(0,1024) + \
    	(string(0,/*) string(0,//)
    	 string(0,#include) contains(0,1024,<0a>#include) \
    	 string(0,#define) contains(0,1024,<0a>#define))
    text/html                 html htm printable(0,1024) +\
    	(istring(0,"") istring(0,"))
    text/plain                txt printable(0,1024)
    text/css                  css
    
    
    ######################################################################
    #
    # RSS feed type...
    #
    
    application/rss+xml       rss
    
    
    ######################################################################
    #
    # CUPS-specific types...
    #
    
    application/vnd.cups-command     string(0,'#CUPS-COMMAND')
    application/vnd.cups-form        string(0,"")
    application/vnd.cups-pdf
    application/vnd.cups-postscript
    application/vnd.cups-ppd         ppd string(0,"*PPD-Adobe:")
    application/vnd.cups-raster      string(0,"RaSt") string(0,"tSaR")
    application/vnd.cups-raw      (string(0,<1B>E) + !string(2,<1B>%0B)) \
    	string(0,<1B>@) \
    	(contains(0,128,<1B>%-12345X) + \
    	 (contains(0,4096,"LANGUAGE=PCL") \
    	  contains(0,4096,"LANGUAGE = PCL")))
    
    ######################################################################
    #
    # Raw print file support...
    #
    # Comment the following type to prevent raw file printing.
    #
    
    application/octet-stream

.. tip::
   If at the end of the procedures the printer is not working, restarting the service or computer may help.


Installing an USB Brother HL 2030 Printer
-----------------------------------------

To install :program:`cups`, :program:`hal-cups-utils`, :program:`usbutils`.
As root, type:

.. code-block:: console

   root@slitaz:# tazpkg get-install cups
   root@slitaz:# tazpkg get-install hal-cups-utils
   root@slitaz:# tazpkg get-install usbutils

:program:`Cups` is used to manage the printer, :program:`hal-cups-utils` allows CUPS to use HAL for printer connections and :program:`usbutils` gives us the :program:`lsusb` utility which lets us know how the printer is connected.

Now we can add ``tux`` to the ``lp`` (printer) group.
As root, we do:

.. code-block:: console

   root@slitaz:# addgroup tux lp

For the web interface of CUPS to be properly activated, we still need to change a few things in the :file:`/etc/cupsd.conf` file:

.. code-block:: console

   root@slitaz:# leafpad /etc/cups/cupsd.conf

::

  # Administrator user group...
  SystemGroup lp
  
  # Restrict access to the admin pages...
  
  Order allow,deny
  Allow 127.0.0.1
  
  
  # Restrict access to configuration files...
  
  AuthType Default
  Require user @SYSTEM
  Order allow,deny
  Deny From None
  Allow From 127.0.0.1

To modify permissions on the printer you need to know the details of the bus and device.
For this, we do as root:

.. code-block:: console

   root@slitaz:# lsusb

And get the following output::

  Bus 001 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
  Bus 001 Device 002: ID 04f9:0027 Brother Industries, Ltd HL-2030 Laser Printer

You can now change the permissions on the associated file:

.. code-block:: console

   root@slitaz:# chmod 666 /dev/bus/usb/001/002

And then start CUPS:

.. code-block:: console

   root@slitaz:# /etc/init.d/cupsd start

We can now configure the printer and fetch the openprinting ppd file for that printer model: http://www.openprinting.org/printer/Brother/Brother-HL-2030 and also add the following packages: :program:`foomatic-filters`, :program:`foomatic-db`, :program:`foomatic-db-engine`:

.. code-block:: console

   root@slitaz:# tazpkg get-install foomatic-filters
   root@slitaz:# tazpkg get-install foomatic-db
   root@slitaz:# tazpkg get-install foomatic-db-engine

Now we can restart CUPS and point midori to http://localhost:631

CUPS then asks for:

#. The login "root" (default tux)
#. The corresponding password.

Then navigate to :menuselection:`Printers --> Add Printer`, click on the printer name that you recognize and do not forget to indicate the path in the connection box: :file:`/dev/bus/usb/001/002` (in my case).

Then go to the web interface of CUPS ppd and install the new printer using the file :file:`Brother HL-2030-hl1250.ppd` located in the user's account.
Now you can automatically restart :program:`cupsd` each time you start the system by using menu :menuselection:`System Tools --> Control box --> Initialization` and allow the :program:`cupsd` daemon to run by adding it to the :guilabel:`Run daemons` section:

::

  dbus hald firewall slim cupsd


Installing a HP Printer
-----------------------

.. note::
   This is for a clean installation of SliTaz GNU/Linux cooking-20100314

#. With the printer plugged in and powered on, run

   .. code-block:: console

      # su root
      # tazpkg recharge
      # tazhw setup printer

#. Install only :program:`hplip` and :program:`hal-cups-utils`
#. The printer should show up in the list of printers displayed as part of this command.
   It will then attempt to open the CUPS admin pages in Midori (at http://localhost:631)
#. On my system, the printer does not show up in the browser under :guilabel:`Find printers` and I get a 'Forbidden' error when trying to add a printer via the browser
#. Instead, run

   .. code-block:: console

      # hp-setup

   and follow the instructions.
   The test page should print correctly.
   The printer should also show up in the list of printers in applications (like the text editor) and will also appear in the list of printers on the CUPS browser pages, although in my case it is still not possible to make any changes to it.


Installing a HP All-In-One Printer/Scanner
------------------------------------------


Preparation & Packages
^^^^^^^^^^^^^^^^^^^^^^

Power on the scanner before installing the following packages:

* :program:`xsane`
* :program:`sane-backends`
* :program:`libusb`
* :program:`libusb-compat`
* :program:`usbtools`
* :program:`usbutils`

.. tip::
   You can automate the process with the following BASH command as *root* user:

   .. code-block:: shell

      for PKG in xsane sane-backends-libusb libusb-compat usbtools usbutils; do
          tazpkg get-install $PKG
      done


Detection
^^^^^^^^^

With those packages installed, use the *Hardware Detection Tool* (:menuselection:`Menu --> System Tools --> Hardware Detection And Drivers`). Click on :guilabel:`Scanner` button.

If your scanner does not show up immediately, enter for none in my set-up; now the scanner device should be listed in purple text, e.g.::

  /dev/bus/usb/004/002


Verify Permissions
^^^^^^^^^^^^^^^^^^

Your scanner must be in scanner group with 666 permissions ::

  crw-rw-rw-    1 root     scanner  189, 385 Jun 22 19:44 /dev/bus/usb/0

Reply ``y`` to start scanner.
Warning pops up about running scanner as root.
Click :guilabel:`continue at your own risk` button
Agree to license.

:program:`Xsane` should open and be working!

The *tux* user is automatically added to ``scanner`` group, so *tux* may scan, but if you run under another user name that user can't scan until you add them to ``scanner`` group.
Do this with the command (as *root*):

.. code-block:: console

   # addgroup <username> scanner


Links
-----

* http://www.openprinting.org/printers


TODO
----

* Parallel Printer
