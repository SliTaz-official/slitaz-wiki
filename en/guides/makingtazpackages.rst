.. http://doc.slitaz.org/en:guides:makingtazpackages
.. en/guides/makingtazpackages.txt · Last modified: 2016/01/28 15:07 by brianperry

.. _makingtazpackages:

Making tazpackages
==================

Before starting on making slitaz packages, we'll begin with a quick overview of how a package installs programs:

When you click on a package to install (in :program:`tazweb`), you tell the system to automatically download the package to :file:`/var/lib/tazpkg/5.0/packages/`.
It then automatically un-cpio's itself to :file:`~/fs/` (or :file:`/home/tux/fs/` when using the absolute path).

Then, it automatically puts all of the files you included in :file:`/fs/` into the correct place on the hard disk, overwriting the old files bearing the same name, …
It knows where to place them as you use the exact same path of the files as the path the files are installed to on your harddisk (so if for instance, you want the package to copy a file called :file:`foo.bar` to :file:`/usr/share`, put :file:`foo.bar` into :file:`/fs/usr/share/` in the package).

After that, it executes the compile rules, genpkg rules, ... (assuming you packed it with :program:`tazwok`; if packed with :program:`tazpkg` it won't call these rules !)

So, with this, we're all ready now to make a tazpackage ourselves:

* put all the files/folders you need in :file:`/fs/path_of_files/`; make sure the :file:`fs` contents only contains binaries, scripts, configuration files, …
  It should not contain source files, license, readme's, …
  Compress the files as :command:`cpio`, and then as :command:`lzma` (do this using the cpio-command :command:`cpio -o -H newc --quiet`, and the lzma-command :command:`xz --format=lzma fs.cpio.lzma fs.cpio` or alternatively with a gui-based compression utility (like :program:`xarchiver`, …)
* put the file size details of the :file:`cpio.lzma` folder in the receipt (you can find these by right-clicking → properties in :program:`PCManFm` on the file).
  Also put in the following lines (replace "text" with your commands; for more rules, see `<http://doc.slitaz.org/en:cookbook:receipt>`_):

  .. code-block:: shell

     compile_rules()
     {
     	text
     }
     
     genpkg_rules()
     {
     	text
     }
     
     post_remove()
     {
     	text
     }

* make the :file:`files.list` file by :command:`cd`\ -ing to your tazpkg's :file:`fs` folder, and then using following commands:

  .. code-block:: shell

     find . -type f -print > ../files.list 
     find . -type l -print >> ../files.list
     cd ..; sed -i s/'^.'/''/ files.list 

* make the :file:`md5` file by typing the command :command:`md5sum filename`, or by using a program (on windows, you can use :program:`winmd5`, :program:`FCIV`, :program:`certutil`, :program:`hashtab`, :program:`hashcheck`, …)
* compress the :file:`fs.cpio.lzma` file together with the :file:`receipt`, :file:`files.list`, :file:`md5sum`, and :file:`description.txt` into a cpio file renamed as :file:`filename.tazpkg`.
  Do so by using the cpio compress command: :command:`cpio -oH < fs.cpio.lzma receipt files.list md5sum description.txt > filename.tazpkg` or using a gui-based compression utility (like :program:`xarchiver`, …)

Alternatively, you can also make the package by using the :command:`tazpkg pack` command; to see what that does exactly, see `<http://hg.slitaz.org/tazpkg/file/3af642cd5e69/modules/pack>`_.
In general it will make the md5 checksum and the filelist, and update the filesizes declared in the receipt.
If you have already made the description and receipt it will also make the finished tazpackage in :file:`/home/tux` (make sure you also pre-made the :file:`fs` folder with the files therein for it to make a working tazpackage).
Note that the wok too can be of use here (see `<http://doc.slitaz.org/en:oldcookbook:wok>`_).

Once you have made a package, you can put it on a personal ftp site/webspace, or you can upload it to the slitaz package database using :program:`mercurial`.
See `<http://doc.slitaz.org/en:oldcookbook:wok>`_ on how to do this.

Also note you may also want to read:

* `<http://hg.slitaz.org/tazpkg/raw-file/tip/doc/tazpkg.en.html>`_
* `<http://hg.slitaz.org/cookutils/raw-file/tip/doc/cookutils.en.html>`_
