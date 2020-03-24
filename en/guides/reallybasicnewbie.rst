.. http://doc.slitaz.org/en:guides:reallybasicnewbie
.. en/guides/reallybasicnewbie.txt · Last modified: 2013/01/29 15:27 by oui

.. _reallyBasicNewbie:

SliTaz Newbie Guide — II
========================

:author: oui


.. rubric:: Training for really Basic Newbie coming from Windows

Linux is not only the successor of Unix: it was the clone of that already perfectly developed OS!
For this reason did Linux never need to copy Windows stuff or methods and did develop itself without getting distracted from its own objectives.
And all documentation concerning Unix and, later, Linux was already perfect as Linux came, so that very old documents can be deprecated but already very useful, especially translations into other languages as they often can't be published as soon as new developments.


Please remember
---------------

The **main** difference is, that Linux did never break with the use of command line:
All graphic figures can be considered as (often only) graphic front ends from a powerful unified processing at commando line (=binary) level.
All is considered as a “file” and will be handle so, so that the commands can generally be used on each type of objects and data.

The **second** difference is, that old version from Unix and Linux also did strongly manage the owners of all objects and data (the initial Windows did miss this performance!) so that you very often have to take caution in Linux don't to hurt the ownership of objects and data:
The system refuses you access as well as processing!

**Third** difference is, that Linux doesn't “*reinvent the wheel*”.
As the system is now generally free and open source, developers can use freely the work of predecessors if it is good, fast and secure, and include such works into her new development without to need to recode somewhat!
We are speaking from the as terrible considered “*dependencies*”!
The commando line use of Linux meets this tendency:
An application can be only a script in which programs are invoked and parameter passed and a lot of them are really so!
But the consequence is of course, that when the dependencies are missing, the application does not work at all.

If you remember that you will probably avoid some difficulties to accustom in Linux coming from Windows!


Some good habits
----------------

* Accept with realism to use the commando line where it is better (and it is often better for a major reason: you see in your monitor after that messages about your actual process; even for beginners can that be really precious and avoid long and unnecessary research!)
* Never login as “*root*”: it is not locked as some people are really pure administrators and need that access but it is the opened door for all kind of danger for your data and your system
* Install as beginner all dependencies even if they seem incomprehensible, improbable and memory-intensive!

For this reason will now look at


some commando line transactions
-------------------------------

(what else after above recommandations)


exit
^^^^

As I resented often about the difficulty to find a way to leave an application (a ridiculous situation:
The application make you prisoner!), I will try to begin my explanations immediately with this important figure!

* **exit** is a main commando name to leave a superordinated transaction.
  It ends for example the session, and, for people starting linux without *“sessions manager”* directly ends the graphic phase of processing to return from Xwindow back to the console level

In professional linux versions, you can generally not do more than close your session — You can not leave (reboot) or halt the system.
And I find it is good so, because it can be possible that the administrator did connect the command of an important object (pump that avoid, that the basement of the house be flooded by water infiltration, or your internet telephone box, nobody can call you any more, etc.).
You don't know that as simple user!
Thousand reason can prohibit you stop a computer…

In home computer it would be to much and the distributor did generally include (as commissioned by the administrator, private administrator are not always accustomed in such rare jobs) more easy way to do that for each.
SliTaz as distributor offers to administrator or user following commands (plus, of course, special virtual keys on the desktop or menu items in the graphic environment):

* **reboot**
* **halt**
* :kbd:`Ctrl`\ +\ :kbd:`Alt`\ +\ :kbd:`Delete`

Sometimes the password of the administrator *root*, **root** will be required.
In SliTaz 1,0 and graphic session, you have to enter one of those words in the main line of the session manager (User name line) and enter the password **root** into the password field!

In the applications, there is an awful mess!

* **quit**
* **:quit**
* q
* :kbd:`Alt`\ +\ :kbd:`q`
* :kbd:`M`

**etc.** are variants coming in consideration (and used by applications in the Slitaz Base training !)


su
^^


Command parameters
~~~~~~~~~~~~~~~~~~

Precedent commands are used without parameter in above cases.
A lot of commands must have parameters plus predefined objects.
For example::

  SomeCommand --SomeParameter [ [SourceObject] DestinationsObject]

.. tip::
   Some commands support more than one parameter at the same time!


Parameter --help
~~~~~~~~~~~~~~~~

A great number of commands is offering an Help parameter::

  SomeCommand --help

or shorted

::

  SomeCommand -h

If you try that with the command **su** you will see a very helpful parameter **-c**:


su -c 'CommandToPass'
~~~~~~~~~~~~~~~~~~~~~

With this combination of the command :command:`su -c` followed by an other command between to ' you can execute some commands reserved for the administrator *root* (the same thing does also the separate non preinstalled command :command:`sudo`; if you prefer :command:`sudo`, you have to install it and eventually adapt the parameter file :file:`/etc/sudoers`.
Using :command:`su -c`, you have to reenter each time the root password.
Using :command:`sudo` only at the first use in the same session!)

.. code-block:: console

   $ su -c 'SomeWhatBeeingOnlyAble --by root'

You will be invite to enter the valid password from **root**!
The command is in that form only for the entered line valid.
You don't need to exit!


su (without any parameter, or eventually with:) [-] [user]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. warning::
   exit this phase of processing as soon as possible with the command :command:`exit`!

:command:`su` opens a sequence of following commands giving you the right from the user, if you name an user, or **root**.
I have to enter the adequate password.

With those commands and variations it is easy to respect the rule never login as *root*!


man SomeCommand
~~~~~~~~~~~~~~~

.. tip::
   quit the manpage with **q**

Will show you the manual page about the command SomeCommand.
Please try

.. code-block:: console

   $ man su

and read it with attention!


Daily used commands
-------------------

Please continue :ref:`here <basicnewbie>`!
