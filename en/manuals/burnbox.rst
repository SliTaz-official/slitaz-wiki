.. _burnbox manual:

Burnbox Manual
==============

About
-----

Burnbox supports:

* ISO burning
* Backup of data and audio CDs
* Data CD/DVD burning (DVD burning requires an optional package :program:`dvd+rw-tools`)
* Audio CD burning (MP3 requires an optional package :program:`mpg123`)
* Video CD (VCD/SVCD) burning (requires packages :program:`vcdimager` and :program:`ffmpeg`)


General Device Settings
-----------------------

In the last tab, make sure the CD-ROM settings are correct (Device: :file:`/dev/cdrom`; Speed (auto-detected): 48; Options: ).
Input is taken from the ``DEVICE`` settings.


Blank CD/DVD-RW
---------------

Use :guilabel:`Blank disk` to erase contents and prepare for burning.


ISO Burning
-----------

This is supported in the third tab.

* Press button :guilabel:`Browse` to specify the ISO file path
* Press button :guilabel:`Burn ISO`

You can also create or manipulate ISO images with the ISO Master utility.


Backup of data and audio CD
---------------------------

This is supported in the second tab.
Optional package :program:`cdrkit-isoinfo` may help to improve the burn speed.

* Insert CD
* Select backup option: "Save backup on Hard Disk Folder" (default) or "Backup on CD disc"
* If backup option is "Save backup on Hard Disk Folder" (default), then specify the folder to do a "CD/DVD backup"
* Press button :guilabel:`Backup DataCD`

Audio CD ripping is supported using the asunder package.


Data CD/DVD burning
-------------------

This is supported in the First tab.
One can specify the burn type before adding files.
Depending upon the burn type, the files are decoded on the fly.
One can remove the added files by simply double-clicking on the file in the tree view.
Total track size shows the CD/DVD space occupied.

* Select data-cd in Burn Type
* Specify file/folder or :guilabel:`Browse`
* Press button :guilabel:`Add`
* Press button :guilabel:`Burn Disc`


Audio CD burning
----------------

This is supported in the first tab.
WAV, OGG, MP3 files are supported.
OGG and MP3 are automatically converted into an uncompressed WAV format for burning.

* Select audio-cd in Burn Type
* Specify file/folder or :guilabel:`Browse`
* Press button :guilabel:`Add` to auto-decode to uncompressed WAV (OGG, MP3 are decoded)
* Press button :guilabel:`Burn Disc`


Video CD (VCD/SVCD) burning
---------------------------

This is supported in the first tab.
Only burning MPG video format is supported directly: MPEG-1 video for VCD and MPEG-2 for SVCD and DVD.
AVI, MOV, FLV, WMV videos are automatically converted into a VCD/SVCD/DVD compatible MPG video format.

* Select video-dvd, vcd or svcd in Burn Type
* Specify file/folder or :guilabel:`Browse`
* Press button :guilabel:`Add` and select Enable decoding video for MPG files (AVI, FLV, MOV, WMV are decoded)
* Press button :guilabel:`Burn Disc`


Common Problems
---------------

* Burnbox works as root but not for a user: This happens when the user is not added to the "cdrom" group

  .. code-block:: console

     # addgroup tux cdrom

* CD-ROM not readable / writable: Make sure the user has proper permissions and is added to the "cdrom" group
* MP3 audio burn does not work: Check if you have installed :program:`mpg123`
* VCD option does not work: Check if you have installed :program:`vcdimager`
* AVI, FLV, WMV files are not decoded to MPG: Check if you have installed :program:`ffmpeg`
* DVD burn does not work: Check if you have installed :program:`dvd+rw-tools`
