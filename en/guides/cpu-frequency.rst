.. http://doc.slitaz.org/en:guides:cpu-frequency
.. en/guides/cpu-frequency.txt · Last modified: 2011/02/22 23:49 by linea

.. _cpu frequency:

CPU Frequency Scaling
=====================

:author: kultex, linea


Introduction
------------

To reduce power consumption is not only essential for portable computers: it is also essential to use it on desktop machines to reduce unnecessary CO\ :sub:`2` emissions.
You can save power by turning off not needed hardware components like WiFi, Bluetooth, etc., switching off or reducing the monitor backlight, spinning down HDDs and controlling the CPU frequency.

CPU frequency scaling is built into the 2.6 kernel and available; but because Slitaz is very small and light, you have to install some additional tools and set it up by yourself.


Installation
------------

Install :program:`linux-cpufreq`, :program:`linux-acpi`, :program:`cpufrequtils` and optional :program:`powertop` (for easy controlling) — :program:`cpufrequtils` and :program:`powertop` we have to take from cooking, because :program:`cpufrequtils` does not exist in 3.0 and :program:`powertop` is buggy.

.. code-block:: console

   # tazpkg get-install linux-cpufreq
   # tazpkg get-install linux-acpi,
   # wget http://mirror.slitaz.org/packages/cooking/cpufrequtils-008.tazpkg
   # wget http://mirror.slitaz.org/packages/cooking/powertop-1.13.tazpkg
   # tazpkg install cpufrequtils-008.tazpkg
   # tazpkg install powertop-1.13.tazpkg


CPU driver
----------

You have to load the correct CPU kernel driver for your CPU — if you don't know, which CPU is in your PC you can get it from:

.. code-block:: console

   $ cat /proc/cpuinfo

Depending on the CPU, load one of the following modules — if you choose the wrong driver, you will get an error message, the module is not loaded and nothing goes wrong.

Generic ACPI P-States based driver:

.. code-block:: console

   # modprobe acpi-cpufreq

AMD mobile K6-2/3+ PowerNow!:

.. code-block:: console

   # modprobe powernow-k6

AMD mobile Athlon PowerNow!:

.. code-block:: console

   # powernow-k7

AMD Cool&Quiet PowerNow!(up to AMD "K10" CPU):

.. code-block:: console

   # modprobe powernow-k8

Intel SpeedStep using the SMI BIOS interface:

.. code-block:: console

   # modprobe speedstep-smi

Intel SpeedStep on ICH-based chipsets:

.. code-block:: console

   # speedstep-ich

Intel Enhanced SpeedStep (deprecated — use :program:`acpi-cpufreq`):

.. code-block:: console

   # modprobe speedstep-centrino

Intel Pentium4/Xeon — The kernel documentation says

  This adds the CPUFreq driver for Intel Pentium 4 / XEON processors.
  When enabled it will lower CPU temperature by skipping clocks.
  This driver should be only used in exceptional circumstances when very low power is needed because it causes severe slowdowns and noticeable latencies.
  Normally Speedstep should be used instead.

— the p4-clockmod supports only ``performance`` and ``powersave`` governors (due to the long transition latency of the module itself):

.. code-block:: console

   # modprobe p4-clockmod

NatSemi Geode GX / Cyrix MediaGXm:

.. code-block:: console

   # modprobe gx-suspmod

Transmeta Crusoe / Efficeon LongRun:

.. code-block:: console

   # modprobe longrun

VIA Cyrix Longhaul:

.. code-block:: console

   # /sbin/modprobe longhaul

nForce2 FSB changing cpufreq driver:

.. code-block:: console

   # modprobe cpufreq-nforce2

Enhanced PowerSaver driver for VIA C7 CPUs:

.. code-block:: console

   # modprobe e_powersaver


Scaling governors
-----------------

Governors can be thought of as pre-configured power schemes for the CPU.

Available governors:

``cpufreq_performance`` (default in SliTaz)
  The ``performance`` governor is built into the kernel and runs the CPU(s) at maximum clock speed

``cpufreq_ondemand`` (also built into the kernel)
  Dynamically increases/decreases the CPU(s) clock speed based on system load

``cpufreq_conservative``
  Similar to ``ondemand``, but more conservative (clock speed changes are more graceful)

``cpufreq_powersave``
  Runs the CPU at minimum speed

``cpufreq_userspace``
  Manually configured clock speeds by user

The built in governors must not be loaded — the others must be loaded with :command:`modprobe` — you may load as many governors as desired (only one will be active at any given time).

Which poses the question, which governor saves most energy?
The ``Powersave`` governor will only save you power if you're playing 3D games (it does not save power because application process completion times are prolonged at the lower processor frequency and the system does not enter a deep C-state.
The greatest power savings occur at idle in deeper C-states) and the ``performance`` governor will basically never give you extra performance.
Don't use them — therefore I am not happy that the ``performance`` governor is used by default in SliTaz.
So you might choose between ``ondemand`` and ``conservative``, which is a matter of taste.

The ``userspace`` governor is necessary for third party applications — like `cpupowerd <http://www.themaxer.com/index.php?option=com_content&view=article&id=86:undervolting-your-cpu&catid=41:nas&Itemid=81>`_ — unfortunately it does not work in SliTaz, because the kernel config for ``CONFIG_X86_MSR`` is not set.


Select the governor
-------------------

At this point everything is prepared but no active governor selected — so no power saving yet.

Use the :command:`cpufreq-set` command to activate one of the governors — for example::

  cpufreq-set -g ondemand

Please note, if you have a dual-core or multiple-core CPU, you must explicitly specify the CPU.
There are CPUs where each core can run with different settings!
Example for a dual-core CPU:

.. code-block:: console

   # cpufreq-set -c 0 -c 1 -g conservative
