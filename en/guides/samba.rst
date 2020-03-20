.. http://doc.slitaz.org/en:guides:samba
.. en/guides/samba.txt · Last modified: 2013/04/30 22:12 by linea

.. _samba:

Samba
=====


What is Samba?
--------------

The Samba or SMB suite is a collection of software to enable Windows-style networking on Linux computers.
The name Samba is a variation on SMB, short for Server Message Block, which is the file sharing protocol used by Windows machines.
The first version of Samba dates from 1992 and a lot of development has been invested over the years.
This has resulted in very stable and reliable code.
The Samba project also has its own website, www.samba.org where you can do more reading on all aspects of the program.


What can I do with it?
----------------------

With Samba, your SliTaz machine is able to access file shares which are on Windows servers.
Samba is a collection of programs which enable file sharing between Windows and Linux.
It is also possible to host a Windows share on your Linux machine, allowing Windows clients to access the files as if they were on a Windows server.


How does it work?
-----------------

The Samba Suite contains two components.
A server implementation (next section) and a client implementation.
For example if you have a storage device at home, it will most likely support SMB/CIFS.
This means you can use the Samba client to access it from Linux.
This is more convenient than FTP or HTTP, especially when you want to use the share for more than just an incidental file copy.
With Samba you can access the SMB share as if it were a local disk meaning you can read and write to it, both from the shell, from Perl or with the Graphic File Manager from the SliTaz distribution.

As usual, we use :program:`tazpkg` to install the Samba client:

.. code-block:: console

   # tazpkg -gi smbclient
   # tazpkg -gi smbfs

The :program:`tazpkg` manager will take care of the dependencies so probably you will see a lot more packages being installed.
Once this is completed, you can view the shares on your Windows server or storage system with:

.. code-block:: console

   # smbclient -L nas
   Enter User's password: ****

This will provide a list of the shares available on :file:`\\nas`.
The result should look similar to the example below:

.. code-block:: console

   Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.6.3-31a.osstech]
   
   	Sharename       Type      Comment
   	---------       ----      -------
   	IPC$            IPC       IPC Service ("My NAS")
   	webaxs          Disk      
   	share           Disk      LinkStation folder
   	info            Disk      LinkStation Utilities
   Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.6.3-31a.osstech]
   
   	Server               Comment
   	---------            -------
   	NAS                  My NAS
   
   	Workgroup            Master
   	---------            -------
   	WORKGROUP            NAS
   # 

From the above example, we are interested in access to Sharename 'share'.
Although not strictly required, it is recommended to add the server name to :file:`/etc/hosts` for name resolution when it has a fixed IP address.
Because we are running on Linux we need to 'escape' the backslashes in the UNC path, effectively needing twice the number of them.
Accessing :file:`\\share` on :file:`\\\\nas` goes like this:

.. code-block:: console

   # smbclient \\\\nas\\share
   Enter User's password: ****
   smb: \>

At the prompt you can issue FTP-style commands to browse the file system and read/write files.
Typing ``help`` gives you a summary of the options:

.. code-block:: console

   smb: \> help
   ?              allinfo        altname        archive        blocksize
   cancel         case_sensitive cd             chmod          chown
   close          del            dir            du             echo
   exit           get            getfacl        hardlink       help
   history        iosize         lcd            link           lock
   lowercase      ls             l              mask           md
   mget           mkdir          more           mput           newer
   open           posix          posix_encrypt  posix_open     posix_mkdir
   posix_rmdir    posix_unlink   print          prompt         put
   pwd            q              queue          quit           readlink
   rd             recurse        reget          rename         reput
   rm             rmdir          showacls       setmode        stat
   symlink        tar            tarmode        translate      unlock
   volume         vuid           wdel           logon          listconnect
   showconnect    ..             !
   smb: \> quit
   #

Fortunately you can also access a Windows share by mounting it on your box like this:

First create the mount point on your SliTaz box:

.. code-block:: console

   # mkdir /nas
   #

Then issue the :command:`mount` command:

.. code-block:: console

   # mount.cifs \\\\nas\\share /nas -o user=admin
   Password: ******
   #

An alternative method is like this:

.. code-block:: console

   # mount //nas/share /nas
   Password: ******
   #

If there is no access-denied-message in reply to the password, it means the share is mounted and can be accessed from your box.
That's basically it!

.. tip::
   The password must be the admin password for your storage device.
   This may not work when you have enabled user-based security.
   In some cases the mount may be read-only.

Many of the problems with Samba are caused by a lack of understanding the Windows security mechanism.
In its basic version, which is still used most of the time, this is user-based security so you will need a **valid windows user account** to access the file share.
When in trouble, verify the access is working from a windows machine and use the same credentials under Linux.


Running a Samba Server on SliTaz
--------------------------------

If you need to set up a Windows file server but you don't want to invest in over-the-top hardware and Windows licenses, Samba is the way to go.
Of course you can use SliTaz as the Linux platform to run the server.
With this solution you will also benefit from the superior stability of the Linux OS.
One of the problems of a Samba administrator is that he sometimes forgets the details of his installation because everything simply keeps on running once it has been set up properly.
This is one of the reasons why Linux people need documentation anyway.
;-)

To install the server component of Samba, install the following package:

.. code-block:: console

   # tazpkg -gi samba

In order to make the user experience even more seamless, it is recommended to keep the accounts synchronized.
This means the users should have the same username & password on their windows box as they are using on the Samba server.
Explanation: When Windows connects to a share, it always attempts to authenticate using the credentials of the user that requests the access.
If there is no match, the user is prompted for a username/password but when they are the same, the share is accessed without any further prompting.

The procedure to add a user to your samba server:

First add Linux user using the :command:`adduser` command.

.. code-block:: console

   # adduser smbuser01
   Changing password for smbuser01
   New password:
   Retype password:
   Password for smbuser01 changed by root

Then add the same user name to Samba using :command:`smbpasswd -a`

.. code-block:: console

   # smbpasswd -a smbuser01
   New SMB password:
   Retype new SMB password:
   Added user smbuser01.
   # 

Use :command:`smbpasswd -h` to view the options.

The file shares and printers you want to allow your Windows clients access to can be defined in the Samba configuration file which is :file:`/etc/samba/smb.conf`

There are countless options to configure here like login scripts, home folders etcetera.
Elaborating on all of them would be way beyond the scope of this guide.

A good place to start is http://www.tldp.org/HOWTO/SMB-HOWTO-2.html which provides a comprehensive overview of Samba but there are many other sites on the web with virtually the same content.
As Samba has been around for over two decades, almost all things have already been tried, done and extensively documented in the process.
Just search the web.
You may encounter some negative comments as well but these are generally posted by people who made some basic mistake or have been the victim of some undocumented change in Windows.
Don't let yourself be discouraged by this.
Samba is Alive and Kicking and when you know how to search for them, you can be sure to find detailed answers to all issues you may encounter while setting up your own server.
