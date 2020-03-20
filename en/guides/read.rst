.. http://doc.slitaz.org/en:guides:read
.. en/guides/read.txt · Last modified: 2015/04/04 01:45 by genesis

.. _read:

README
======

Yes we love your questions, your problems are ours and SliTaz has a very good Support Team, **“but sometimes it's hard”** for us when you forget to use the search button or read the official `documentation <http://doc.slitaz.org/en:start>`_.

Answering questions takes time, and to answer them well slightly longer.
This time could be better spent for other necessary things, like improving SliTaz or writing documentation.


Few things to note before asking for support
--------------------------------------------

.. rubric:: 1. Please provide us with as much information as possible

Telling us that something is broken doesn't help to solve anything, because it may run perfectly OK on our systems.
This means you'll have to provide us with some more information so that we can help you.
This will include the steps you took before and after installing the program, your current systems specifications and which version of SliTaz you happen to be using.
If a program fails to open, try running it in a terminal and copying down the error.
If a piece of hardware doesn't work, see what :command:`lsusb` (you may need the :program:`usbutils` package), :command:`lspci` and :command:`dmesg` commands have to say when you plug in the device.
The more information you can provide us with, the quicker we can solve your problem.


.. rubric:: 2. SliTaz is not Windows or Ubuntu

If you're expecting an easy :program:`Windows` or :program:`Ubuntu` replacement in a 30MB ISO, then you'll be sadly disappointed.
SliTaz was designed to be lightweight, fast and small so it can run on older hardware.
This means a lot of the larger packages have been stripped out to save space.
It is possible to make SliTaz act and look like either :program:`Windows` or :program:`Ubuntu`, but don't expect every kind of functionality to work out of the box.
Remember, even with :program:`Ubuntu` and :program:`Windows` you still have to install some other stuff before you can use them.
If you wish to use a Linux distribution that works out of the box, give :program:`Linux Mint` or :program:`PCLinuxOS` a go.
Just as a little warning — someone in the forum described SliTaz as “a no-go for newbies”.


.. rubric:: 3. SliTaz isn't based on any other distribution

SliTaz was made from scratch using the :program:`Linux Kernel`, :program:`Busybox`, :program:`OpenBox` and some :program:`LXDE` tools.
It has its own package management system and most of the packages are made by volunteers.
SliTaz does however boast the ability to convert packages from other distributions using the :command:`tazpkg convert` command.


.. rubric:: 4. We're unpaid volunteers and not your employees

Remember that you haven't paid the developers who designed SliTaz or the people online who provide the support.
They don't owe you anything.
So ask nicely and we'll try to help you in due time.

Here are some links to the most frequently asked questions:


.. rubric:: “Can't login, not even as root” or “startx does not work” or “failed to execute login command”

| http://vanilla.slitaz.org/index.php?p=/discussion/comment/5812/#Comment_5812
| http://doc.slitaz.org/en:guides:xorg-xvesa


.. rubric:: “How to change the desktop resolution”

| http://doc.slitaz.org/en:guides:xorg-xvesa


.. rubric:: “Adding a new user”

| http://vanilla.slitaz.org/index.php?p=/discussion/comment/2808/#Comment_2808


.. rubric:: “Copy / paste troubles with xterm”

Install sakura terminal as root::

  tazpkg get-install sakura


.. rubric:: See also

:ref:`Frequently Asked Questions <faq>`
