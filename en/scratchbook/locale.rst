.. http://doc.slitaz.org/en:scratchbook:locale
.. en/scratchbook/locale.txt · Last modified: 2011/04/23 23:06 by domcox

.. _scratchbook locale:

Locale & i18n
=============

Installation and configuration of locales.


Locale installation
-------------------

This chapter describes the installation of locales in SliTaz GNU/Linux from a SliTaz GNU/Linux host system.
The installation of locales contained in the X server are described in the chapter :ref:`scratchbook x window system`.
The various files copied in this chapter come from the compiliation package glibc-2.3.6 forming part of the *toolchain*.


.. rubric:: Various file directories

We begin by creating the directories that contain libraries and files relevant to the different locales.
The directory :file:`LC_MESSAGES` contains the files for the translated messages (:file:`.mo`), if they exist:

.. code-block:: console

   # mkdir -p rootfs/usr/share/{i18n,locale}
   # mkdir -p rootfs/usr/lib/{locale,gconv}
   # mkdir -p rootfs/usr/share/i18n/{charmaps,locales}
   # mkdir -p rootfs/usr/share/locale/fr/LC_MESSAGES

Copy the localization files for French, Swiss-French and Swiss-German in :file:`/usr/share/i18n/locales`:

.. code-block:: console

   # cp -a /usr/share/i18n/locales/{de_CH,fr_CH,fr_FR,i18n,iso14651_t1} \
     rootfs/usr/share/i18n/locales

Copy the :file:`translit_*` files in :file:`/usr/share/i18n/locales`:

.. code-block:: console

   # cp -a /usr/share/i18n/locales/{translit_circle,translit_cjk_compat} \
     rootfs/usr/share/i18n/locales
   # cp -a /usr/share/i18n/locales/{translit_combining,translit_compat} \
     rootfs/usr/share/i18n/locales
   # cp -a /usr/share/i18n/locales/{translit_font,translit_fraction} \
     rootfs/usr/share/i18n/locales
   # cp -a /usr/share/i18n/locales/{translit_narrow,translit_neutral} \
     rootfs/usr/share/i18n/locales
   # cp -a /usr/share/i18n/locales/{translit_small,translit_wide} \
     rootfs/usr/share/i18n/locales

Copy the :file:`charmaps` files in :file:`/usr/share/i18n/charmaps`:

.. code-block:: console

   # cp -a /usr/share/i18n/charmaps/ANSI_X3.* rootfs/usr/share/i18n/charmaps
   # cp -a /usr/share/i18n/charmaps/{ISO-8859-1.gz,ISO-8859-2.gz,ISO-8859-15.gz} \
     rootfs/usr/share/i18n/charmaps

Copy the :file:`gconv` libraries in :file:`/usr/lib/gconv` to rootfs of SliTaz:

.. code-block:: console

   # cp /usr/lib/gconv/{ANSI_X3.110.so,gconv-modules,UNICODE.so} \
     rootfs/usr/lib/gconv
   # cp /usr/lib/gconv/{ISO8859-1.so,ISO8859-2.so,ISO8859-15.so} \
     rootfs/usr/lib/gconv
   # strip -v rootfs/usr/lib/gconv/*.so

Copy the :file:`locale` utility:

.. code-block:: console

   # cp /usr/bin/locale rootfs/usr/bin

It's necessary that the file :file:`/usr/lib/locale/locale-archive` is generated, for that we use the :command:`localedef` utility while chrooted in SliTaz:

.. code-block:: console

   # cp /usr/bin/localedef rootfs/usr/bin
   # chroot rootfs /bin/ash

Use of :command:`localedef` for French-speaking Switzerland and France:

.. code-block:: console

   # localedef -i fr_CH -f ISO-8859-1 fr_CH
   # localedef -i fr_FR -f ISO-8859-1 fr_FR
   # exit

You can delete the :file:`localedef` binary to gain some space:

.. code-block:: console

   # rm rootfs/usr/bin/localedef


Config and use of locale
------------------------

To use a language in a session, you can create a script launched at boot, or add 2 lines to the :file:`~/.profile` specific to each user with:

.. code-block:: shell

   export LANG=fr_CH
   export LC_ALL=fr_CH

Voilà, the French language should now function.
If you installed :program:`retawq` or :program:`nano`, you can check the performance of locales by copying the :file:`.mo` files in the sources of :program:`Retawq` or :program:`Nano` to :file:`/usr/share/locale/fr/LC_MESSAGES`.


.. rubric:: Following chapter

SliTaz uses the :file:`/etc/init.d/i18n.sh` script and the :file:`/etc/locale.conf` configuration file to manage the system locale.
This is detailed in the next chapter :ref:`scratchbook boot scripts`.
On a working system, just modify :file:`/etc/locale.conf` with a text editor or launch :command:`tazlocale` to change the default system locale Or to specify the language as a boot option: ``lang=xx``.
