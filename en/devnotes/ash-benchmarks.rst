.. http://doc.slitaz.org/en:devnotes:ash-benchmarks
.. en/devnotes/ash-benchmarks.txt · Last modified: 2011/03/08 14:53 by linea

.. _ash benchmarks:

ASH benchmarks
==============

Some tips to speed-up scripts…
Add yours, and put the better ones on the top of the page :)


Sed substitution vs Variable substitution
-----------------------------------------

.. rubric:: Information

In some cases, both tools can do the same job.
As a built-in ash/bash command, variable substitution is faster.

.. note::
   This is always true — you can compare each type of variable substitution with an equivalent using an external tool.


.. rubric:: Benchmark

::

  $ echo '#!/bin/sh
  for i in $(seq 1 1000); do
  	echo "slitaz" | sed 's/slitaz/SliTaz/'
  done' > /tmp/slow
  $ echo '#!/bin/sh
  for i in $(seq 1 1000); do
  	A=slitaz
  	echo "${A/slitaz/SliTaz}"
  done' > /tmp/speed
  $ chmod +x /tmp/slow /tmp/speed
  $ time /tmp/slow
  > real	0m 12.40s
  $ time /tmp/speed
  > real	0m 0.04s


Group command vs. Sub-process
-----------------------------

.. rubric:: Information

Group command "{}" groups several commands into one (as a function).
So, output can be grouped too: "{ com1; com2; } | com3".
Sub-process "()" achieves something similar, but creates a shell sub-process; which costs a lot more resources.
Another difference is that when you kill an application using :kbd:`Ctrl`\ +\ :kbd:`C`, a sub-process is killed instead of a main application — while :kbd:`Ctrl`\ +\ :kbd:`C` on grouped commands kills the application itself.
Finally, changing directory or variables into sub-processes will not affect the main script which it does with grouped commands.
Conclusion: always use group commands instead of sub-processes, and take care ;D

.. note::
   Group commands need a newline before closing — or "; }".


.. rubric:: Benchmark

::

  $ echo '#!/bin/sh
  for i in $(seq 1 10000); do
  	( echo yo )
  done' > /tmp/slow
  $ echo '#!/bin/sh
  for i in $(seq 1 10000); do
  	{ echo yo; }
  done' > /tmp/speed
  $ chmod +x /tmp/slow /tmp/speed
  $ time /tmp/slow
  > real	0m 5.36s
  $ time /tmp/speed
  > real	0m 0.23s


Grep vs Fgrep
-------------

.. rubric:: Information

:command:`fgrep` is exactly the same thing as :command:`grep` if you don't use patterns (^,$,*,etc.).
:command:`Fgrep` is optimized to handle such cases, particularly when you look for several different plain patterns.
A difference can be found even if you look in only one pattern.


.. rubric:: Benchmark

::

  $ echo -e "line1\nline2\nline3" > /tmp/file
  $ echo '#!/bin/sh
  for i in $(seq 1 1000); do
  	grep 3 /tmp/file
  done' > /tmp/slow
  $ echo '#!/bin/sh
  for i in $(seq 1 1000); do
  	fgrep 3 /tmp/file
  done' > /tmp/speed
  $ chmod +x /tmp/slow /tmp/speed
  $ time /tmp/slow
  > real	0m 11.87s
  $ time /tmp/speed
  > real	0m 3.21s


``[ -n "text" ]`` vs ``[ "text" ]``
-----------------------------------

.. rubric:: Information

The two commands test if ":file:`text`" exists.
Using ``-n`` slows the process and weighs down the script a little too.


.. rubric:: Benchmark

::

  $ echo '#!/bin/sh
  for i in $(seq 1 1000000); do
  	[  -n "$i" ]
  done' > /tmp/slow
  $ echo '#!/bin/sh
  for i in $(seq 1 1000000); do
  	[ "$i" ]
  done' > /tmp/speed
  $ chmod +x /tmp/slow /tmp/speed
  $ time /tmp/slow
  > real	0m 15.56s
  $ time /tmp/speed
  > real	0m 14.11s


``[ -z "text" ]`` vs ``[ ! "text" ]`` vs ``! [ "text" ]``
---------------------------------------------------------

.. rubric:: Information

These three commands test if :file:`text` **doesn't** exist.
``[ ! "text" ]`` and ``[ -z "text" ]`` have a similar processing time, while ``! [ "text" ]`` is speedier.


.. rubric:: Benchmark

::

  $ echo '#!/bin/sh
  for i in $(seq 1 1000000); do
  	[  -n "$i" ]
  done' > /tmp/slow1
  $ echo '#!/bin/sh
  for i in $(seq 1 1000000); do
  	[  -n "$i" ]
  done' > /tmp/slow2
  $ echo '#!/bin/sh
  for i in $(seq 1 1000000); do
  	[ "$i" ]
  done' > /tmp/speed
  $ chmod +x /tmp/slow1 /tmp/slow2 /tmp/speed
  $ time /tmp/slow1
  > real	0m 15.53s
  $ time /tmp/slow2
  > real	0m 15.60s
  $ time /tmp/speed
  > real	0m 14.27s


Awk vs Cut
----------

.. rubric:: Information

:command:`Awk`, as :command:`cut`, can be used to cut a field of a line.
:command:`Awk` can do many other things, while :command:`cut` is a tool dedicated to this usage; it's why :command:`cut` is a little faster for this task.


.. rubric:: Benchmark

::

  $ echo -e "field1\tfield2\tfield3" > /tmp/file
  $ echo '#!/bin/sh
  for i in $(seq 1 5000); do
  	awk '"'"'{ print $2 }'"'"' /tmp/file
  done' > /tmp/slow
  $ echo '#!/bin/sh
  for i in $(seq 5000); do
  	cut -f2 /tmp/file
  done' > /tmp/speed
  $ chmod +x /tmp/slow /tmp/speed
  $ time /tmp/slow
  > real	0m 16.61s
  $ time /tmp/speed
  > real	0m 15.90s


``[ condition1 -a condition2 ]`` vs ``[ condition1 ] && [ condition2 ]``
------------------------------------------------------------------------

.. rubric:: Information

While ``&&`` is a fast built-in function, in this case it uses two processes (two test functions) instead of one.
So, using ``-a`` is a little faster, as the "AND" function itself is slower but makes it possible to use only one process.


.. rubric:: Benchmark

::

  $ echo '#!/bin/sh
  for i in $(seq 1 1000000); do
  	[ "$i" ] && [ "$i" ]
  done' > /tmp/slow
  $ echo '#!/bin/sh
  for i in $(seq 1 1000000); do
  	[  "$i"  -a "$i" ]
  done' > /tmp/speed
  $ chmod +x /tmp/slow /tmp/speed
  $ time /tmp/slow
  > real	0m 23.94s
  $ time /tmp/speed
  > real	0m 22.29s
