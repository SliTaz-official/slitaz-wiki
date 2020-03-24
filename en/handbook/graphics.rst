.. http://doc.slitaz.org/en:handbook:graphics
.. en/handbook/graphics.txt · Last modified: 2016/10/24 14:26 by hgt

.. _handbook graphics:

Graphics
========

:author: jozee, linea, genesis, hgt


:program:`Gcolor2` — Select and manage colors
---------------------------------------------

:program:`Gcolor2` is a tool to select and retain palette colors.
It can be useful for the creation of SliTaz themes, for example.
It can be found in the :menuselection:`Graphics` category or run from the command line:

.. code-block:: console

   $ gcolor2 &


:program:`GIMP` — Manipulate and create images
----------------------------------------------

The :program:`GIMP` (GNU Image Manipulation Program) is software that can manipulate images to a very high quality level.
It allows you to do what you would expect from an application that processes images, ie. layers, filters, support scripts adding functionality, etc.
:program:`GIMP` supports a large number of image formats such as: PNG, JPEG, XPM, PPM, TIFF, PostScript, PSD, it also offers its own XCF format.
To install :program:`GIMP`:

.. code-block:: console

   # tazpkg get-install gimp

:program:`GIMP` is scalable and can be configured with the main interface — configuration files, brushes and personal scripts are located in the :file:`~/.gimp-2.2` directory.


:program:`GQview` — Image manager
---------------------------------

:program:`GQview` is very light and quick and allows you to navigate rapidly between images by selecting files in a directory tree with a single mouse click.
It supports slideshows, image rotation, adding keywords and tags, drag and drop, and can display EXIF data.
It also allows you to edit images in the software of your choice (:program:`mtPaint`, :program:`GIMP` for example).
To install :program:`GQview`:

.. code-block:: console

   # tazpkg get-install gqview


:program:`jpeg` — JPEG command line tools
-----------------------------------------

To allow applications that use JPEG to function, linked libraries must be provided by the package :program:`jpeg-6b`, this package also contains some tiny utilities that can be used on the command line such as :command:`cjpeg` and :command:`djpeg`.
To modify JPEG images on the command line you can also use :command:`jpegtran`, installed by default on SliTaz; :command:`jpegtran` allows you to rotate images via the ``-rotate`` option.
To find out all of the options available for these tools, just specify the ``--help`` option.
Example:

.. code-block:: console

   $ cjpeg --help


:program:`mtPaint` — Image processing
-------------------------------------

:program:`mtPaint` is an application for the creation and retouching of PNG, TIFF, XPM and BMP images.
It offers many simple, lightweight, fast functions like capture screen (screenshot) which you can access from the :menuselection:`menu --> Graphics --> Grab screenshot`, or via a terminal:

.. code-block:: console

   $ mtpaint -s


:program:`Viewnior` — Elegant image viewer
------------------------------------------

:program:`Viewnior` is a fast and simple image viewer with a minimalistic interface.
It can rotate, flip, crop, save, delete images and supports fullscreen, slideshow, etc.
To install:

.. code-block:: console

   # tazpkg get-install viewnior
