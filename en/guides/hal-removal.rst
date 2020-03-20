.. meta::
   :canonical: http://doc.slitaz.org/en:guides:hal-removal
   :local: en/guides/hal-removal.txt
   :last-modified: 2012-09-13 20:29
   :modified-by: pankso

.. _hal removal:

hal-removal for 5.0
===================

just to avoid errors its good to plan and document this process

.. list-table::
   :class: longtable
   :header-rows: 1
   :widths: 1 1 1 4

   * - Package
     - Status
     - Who
     - Link / Discussion / Bugreport
   * - udev
     - open
     -
     - udev rules maybe should be in :file:`/lib/udev/rules.d` — because applications drop it there
       
       https://wiki.archlinux.org/index.php/Udev

       udev rules written by the administrator go in :file:`/etc/udev/rules.d/`, their file name has to end with :file:`.rules`.
       The udev rules shipped with various packages are found in :file:`/lib/udev/rules.d/`.
       If there are two files by the same name under :file:`/lib` and :file:`/etc`, the ones in :file:`/etc` take precedence.
       There is an error in the arch doku — because all rules are in :file:`/lib` — :file:`/usr/lib/udev` does not exits — corrected here but maybe not a must, because opensuse 12.2 still has the udev rules still in :file:`/etc` http://doc.opensuse.org/documentation/html/openSUSE/opensuse-reference/cha.udev.html

       also useful — the gentoo doku (nvidia troubles) http://www.gentoo.org/doc/en/udev-guide.xml

       -- Pankso note -- Now udev (from version 182) is shipped in systemd source tree... so we will have a systemd package but split udev into a light package and by default last udev install all rules in /usr/lib/udev...
   * - systemd
     - open
     -
     - should replace init.d
       
       http://en.wikipedia.org/wiki/Systemd — NO (at least not now) — Systemd will replace all our boot scripts and process!
       First our boot scripts are realy special, even not sysV and easily modifiable since ther pure shell scripts.
       Second systemd is a huge package for our base system (more than 5 Mb).
       Third, if we switch we must heavely customize it and have :file:`*.service` file for each daemon (slim, lighttpd, etc).
       Last, systemd aims is to speed up boot time, but we alredy boot faster than sysV.
       If one want to see the amount of work before using systemd (even as replacement, not default), you can install it from undigest repo and add an entry to grub menu.list with ``init=/bin/systemd`` — Pankso
   * - libpng
     - open
     -
     - up to libpng 1.5.x
   * - dashel
     - open
     -
     - http://svn.gna.org/viewcvs/dashel/trunk/readme.txt?view=markup&pathrev=144
       
       should not be a problem
   * - gnomad2
     - open
     -
     - http://gnomad2.sourceforge.net/
       
       Gnomad 2.9.6 is released — we delete HAL support and slam in libgudev support in its place, HAL is deprecated now
   * - gtkpod
     - open
     -
     - should not be a problem, when gtkpod finds no hal depencies
   * - gxine
     - open
     -
     - should be no problem
       
       http://archives.gentoo.org/gentoo-dev/msg_1761183caa2cae78c9623485142a372b.xml
   * - hal-cups-utils
     - open
     -
     - must be replaced by udev-config-printer
       
       http://cyberelk.net/tim/2009/07/20/re-writing-hal-cups-utils-to-avoid-hal/
   * - hal-dev
     - open
     -
     - just to delete
   * - libexo-dev
     - open
     -
     - maybe update to 0.6.0 -1, but perhaps just to delete the depency in libexo-dev, because all other exo packages have no depency on hal
       
       https://launchpad.net/ubuntu/+source/exo/0.6.0-1
   * - files hal-extra
     - open
     -
     - just to delete
   * - hal-info
     - open
     -
     - just to delete
   * - hal-scripts
     - open
     -
     - just to delete
   * - ivman
     - open
     -
     - just to delete
   * - libgphoto2
     - open
     -
     - maybe it must be configured with option without hal
       
       libgphoto2_port - added --with/--without configure options for: bonjour, hal

       http://www.gphoto.org/news/
   * - hplip
     - open
     -
     - udev rules should be shipped with hplip
       
       https://bugs.launchpad.net/hplip/+bug/401091
   * - libgphoto2-dev
     - open
     -
     - same as libgphoto2
   * - sane-backends
     - open
     -
     - should be no problem - needs the 55-libsane.rules shiped with sane either in lib or in etc
   * - brscan
     - open
     -
     - should not be a problem perhaps edit the the udev-rules
       
       http://welcome.solutions.brother.com/bsc/public_s/id/linux/en/instruction_scn1c.html#u9.10
   * - brscan2
     - open
     -
     - same as brscan
   * - sane-backends-dev
     - open
     -
     - same as sane-backends
   * - xsane
     - open
     -
     - indirect depency from sane-backends
   * - pcmanfm
     - done
     - `Christophe <http://hg.slitaz.org/wok/rev/473b2d729747>`_
       
       `Pascal <http://hg.slitaz.org/wok/rev/2f1b673c2701>`_
     - just to delete / update in flavors to pcmanfm2
       
       pcmanfm from 0.9 series has ported to use gvfs / udisk for volume management
   * - lxde
     - done
     - fixed with
       
       pcmanfm
     - indirect depency from pcmanfm — was fixed with the fix of pcmanfm
       
       https://bbs.archlinux.org/viewtopic.php?id=123650

       relevant only lxsession (0.4.4-2 supports now power management through upower)
   * - pmount
     - open
     -
     - should not be a problem — not much infos
       
       https://bbs.archlinux.org/viewtopic.php?id=120558
   * - thunar-vfs
     - open
     -
     - Released Thunar 1.1.1 and thunar-volman 0.5.0 with full support for udev and GIO and no dependency on HAL
       
       http://wiki.xfce.org/dev/thunar-volman-udev

       http://gezeiten.org/post/2010/01/Thunar-volman-and-the-deprecation-of-HAL
   * - thunar-vfs-dev
     - open
     -
     - same as thunar
