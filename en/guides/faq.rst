.. http://doc.slitaz.org/en:guides:faq
.. en/guides/faq.txt · Last modified: 2019/01/30 15:37 by hgt

.. _faq:

Frequently Asked Questions
==========================

Listed here are the most frequently asked questions from the user forums.
Please check here to see if your problem has been fixed before starting a new forum thread.

These are organised into per-scenario groups — from system-level through to the desktop.
Within each, a symptom describes a give-away identification of the problem; a widely-working solution is then outlined.


.. rubric:: General

* :ref:`Log-In Problems <faq-login>`
* :ref:`Shutdown via slim <faq-slim>`
* :ref:`Terminals (Copy/Paste etc.) <faq-terminal>`


.. rubric:: Package or Software

* :ref:`An Application Cannot Find Libraries/Files <faq-dependency>`
* :ref:`A Program Doesn't Run or Quits Unexpectedly <faq-programfailstorun>`
* :ref:`Can I install a package on SliTaz from another distribution? <faq-pkgconv>`


.. rubric:: Hardware

* :ref:`Mouse <faq-mouse>`


.. rubric:: Questions Still Unanswered?

No problem!
We are happy to hear your question over at our `Support Forum <http://forum.slitaz.org/>`_ — or even the `Mailing List <http://www.slitaz.org/en/mailing-list.html>`_ if it is a more lengthy discussion — once you have looked through our `documentation <http://doc.slitaz.org/en:start>`_!

Answering questions takes time however, to answer them well slightly longer.
This time could be better spent for other necessary things, like improving SliTaz or writing documentation.

**Your first post should include the following things:**

* detailed information about your hardware

  * e.g the manufacturer and model of your computer, motherboard or (even better) detailed chip information.
    The latter can be found by running the following command and uploading the generated :file:`SysInfo.txt` file:

    .. code-block:: shell

       echo '=== PCI Devices: ===' >> SysInfo.txt && lspci >> SysInfo.txt &&
       echo '=== USB Devices: ===' >> SysInfo.txt && lsusb >> SysInfo.txt &&
       echo '=== Kernel Modules: ===' >> SysInfo.txt && lsmod >> SysInfo.txt

* precise information about your SliTaz installation:

  * which version to you have?
    Stable or Cooking?
  * are you using the standard ISO or a specific flavor (XVESA, Lo-RAM etc.)?
  * is it installed on a HD or on an USB stick?
  * do you know which file-system you used?

* identify when your problem appeared:

  * was it just after installation or after an update?
  * were there any changes to your normal usage pattern or configuration?

This should provide enough system information for help to be provided.


.. toctree::
   :hidden:

   faq-login
   faq-slim
   faq-terminal
   faq-dependency
   faq-programfailstorun
   faq-pkgconv
   faq-mouse
