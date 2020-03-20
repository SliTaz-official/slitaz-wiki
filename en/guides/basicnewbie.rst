.. http://doc.slitaz.org/en:guides:basicnewbie
.. en/guides/basicnewbie.txt · Last modified: 2013/03/01 19:58 by linea

.. _basicnewbie:

Daily used Linux commands
=========================

After starting our SliTaz base (see :ref:`here <training>`) we logged on and can see now, following a Welcome message, a short command line prompt:

.. code-block:: console

   tux@dolilive:~$ _

Now we can immediately start to understand who is doing what:

* **tux**, is my login name
* **@**, is where I am "at"
* **dolilive**, something *like do_Linux_live*
* **:**, what we chose to do is awaiting us at the prompt and it will be actived here (in absence of some concrete object that would redefine the destination)
* **~**, is my «home» (**~** is the symbol for the home directory of the user logged in!)

.. warning::
   **~** depends on the administrative rights you actually have

If you are *root*, any data will be automatically registered in the special directory */root*.
The tree of the main directory is also named the *root of the file system*.
The symbol / is the first sign that you see alone or in the first place of all the other paths!
It is not possible to unbind the *root of the file system* and the main directory *root* is used by the administrator *root* for all of their transactions!
This directory */root* is a special directory as for example as it has to be in the same partition as the tree of main directories as well as hold the contents of the first level!

The *home* of users is a subdirectory in the main directory */home*!
The path is as follows: :file:`/home/tux`

tux can be defined differently at the installation if you don't want to be called tux!
In such a case the path would be: **/home/SomeWhatYouWillBe**.
Both the contents of home, as well as the contents of *SomeWhatYouWillBe* can also be contained in a separate partition.
The automatic installer in the SliTaz Panel can help you to do that if you wish.

We can enter now at the prompt our first command:

ls
--

**You don't need to exit**

**ls** is a command to list files located in the actual directory.
Adding parameters and/or paths it can do that in each directory where you have the rights and can do more than just showing the file names.

Please test::

  ls --help

(more? see `here <http://en.wikipedia.org/wiki/Ls>`_)

And test now please::

  ls /usr/bin


See Also
--------

:ref:`The SliTaz Handbook <handbook>`
