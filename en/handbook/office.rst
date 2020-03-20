.. http://doc.slitaz.org/en:handbook:office
.. en/handbook/office.txt · Last modified: 2015/08/19 13:56 by genesis

.. _handbook office:

Office
======


Abiword — Word Processor
------------------------

:program:`Abiword` is a word processor application rich in features.
It contains fast, simple, intuitive lightweight tools and proposes the proper format (:file:`.abw`), supporting Open Office and Microsoft Word documents, it can also export to PDF or HTML.
To install :program:`Abiword` on SliTaz just run:

.. code-block:: console

   # tazpkg get-install abiword


Osmo — Personal Organizer
-------------------------

:program:`Osmo` is a small personal organizer providing a timetable and a list of tasks and contacts with the possibility of opening them directly in a web browser via a URL or a mail client using an email address.
:program:`Osmo` also offers a calendar, a date calculator and the ability to take notes classified by day.
If you use USB media associated with the LiveCD, it will even retain your data for you.
On an installed system, you can syncronize data with USB media by using :program:`Grsync`.
:program:`Osmo` keeps its data in the hidden folder :file:`~/.osmo`.


SQLite — Tiny SQL Database engine
---------------------------------

:program:`SQLite` is a small relational SQL database engine whose entire database is stored in a single file.
It's fast, powerful, speeds applications and implements most of the SQL92 standard.
:program:`SQLite` is ideal for managing small websites, while requiring minimal deployment.
The official website for the project is: http://www.sqlite.org/


ePDFView — Lightweight PDF viewer
---------------------------------

To view PDF documents, SliTaz uses the :program:`epdfview` package.
This provides a fast, simple, easy to use PDF viewer.
:program:`ePDFview` uses GTK+ and the rendering library :program:`poppler`, this enables you to view, move from page to page and search or navigate the index.


Gnumeric — Spreadsheet
----------------------

:program:`Gnumeric` is free spreadsheet program and has the ability to import/export several file formats such as CSV, Microsoft Excel, Latex, HTML, etc.
As well as providing its own format: :file:`.gnumeric`, it is also one of the most statistically accurate spreadsheets around.
To install gnumeric on SliTaz:

.. code-block:: console

   # tazpkg get-install gnumeric


Homebank — Finance management
-----------------------------

:program:`Homebank` is a handy program to manage and compare bank accounts.
Lightweight, fast and comprehensive, :program:`homebank` is a good addition to :program:`Abiword` and :program:`Gnumeric` for an office suite that's light and easy to use.
Website: http://homebank.free.fr/.
To install :program:`homebank`, you can use the graphical package manager or the command:

.. code-block:: console

   # tazpkg get-install homebank


Notecase — Notes manager
------------------------

:program:`Notecase` is a software designed to organize and manage notes.
It allows you to link pages, import or export notes and format text (bold, italics, etc).
After installation :program:`notecase` is located in the :menuselection:`Menu --> Office --> Notecase notes manager`.

.. code-block:: console

   # tazpkg get-install notecase


Wikipedia — Online Encyclopedia
-------------------------------

Wikipedia is a free online encyclopedia where you can find information on various subjects.
To open the wikipedia, choose :menuselection:`Office --> Wikipedia Encyclopedia`.
Main website: http://wikipedia.org/


Zoho Viewer — Online document viewer
------------------------------------

If you need to quickly edit a document and the right spreadsheet or word processor is not at hand, you can use the document viewer to quickly upload your file (up to 10 MB).
And within a couple of seconds it will be available for reading and editing.
To open Zoho on the menu, select :menuselection:`Office --> Zoho Document Viewer`.

It currently supports most popular formats:

* Microsoft Word (:file:`doc`, :file:`docx`), Excel (:file:`xls`, :file:`xslx`), PowerPoint (:file:`ppt`, :file:`pptx`)
* OpenOffice.org Writer (:file:`odt`, :file:`sxw`), Spreadsheet (:file:`ods`, :file:`sxc`), Presentation (:file:`opd`, :file:`sxi`)
* Also :file:`pdf` (experimental stage), :file:`rtf`, :file:`html`, :file:`txt`, and others


Office Suites
-------------

If you want a complete office suite installed at Linux, there are some nice packages at SliTaz repositories:

* `Libre Office <http://www.libreoffice.org/>`_
* `Apache Open Office <http://www.openoffice.org/>`_
