.. http://doc.slitaz.org/en:guides:faq-dependency
.. en/guides/faq-dependency.txt · Last modified: 2010/07/08 17:14 (external edit)

.. _faq-dependency:

An Application Cannot Find Libraries/Files
==========================================


Symptoms
--------

* When run in a Terminal, an application fails with the message::

    [name]: error while loading shared libraries: [library].so.*: cannot open shared object file: No such file or directory


Explanation
-----------

The program is missing some files it needs to run.
This is caused by the package information missing a reference to another, or not including the file.


Solution
--------

Check if the file required is available in another package.
Use the package manager (:program:`TazPkg`) to search for the file:

* start the package manager
* :guilabel:`Recharge` the package lists, if necessary
* click the :guilabel:`Search` tab
* in the :guilabel:`Search` box, enter the file-name (:file:`{library}.so`) and click :guilabel:`Files`
* install the package (pick that with the closest name if there is more than one) by double-clicking the entry and clicking :guilabel:`Install Package`

Alternatively, run the following command from a terminal::

  tazpkg search-pkgname library.so


… and install the appropriate package with::

  tazpkg get-install [package]

If no packages are given, report this as a bug over at the `Labs <http://labs.slitaz.org>`_ as it needs to be included, or the dependency removed during the build.
