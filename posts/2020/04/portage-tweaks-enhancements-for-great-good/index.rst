.. title: Portage tweaks & enhancements for great good
.. slug: portage-tweaks-enhancements-for-great-good
.. date: 2020-04-26 10:26:30 UTC-04:00
.. updated: 2020-05-08 18:18:38 UTC-04:00
.. tags: gentoo, linux, portage, tweak, enhancement
.. category: gentoo
.. link:
.. description: Tweak Gentoo Portage for parallel builds, more threads and nicer output. Find out how to enhance your Portage experience.
.. type: text

`Portage`_ is the official package manager, and software distribution system for
`Gentoo Linux`_. It allows packages to be configured, built from source, and
installed onto the system with a minimal of fuss and manual configuration. When
compared to the traditional ``./configure && make && make install`` of many
software packages, `Portage` provides a far nicer and more streamlined process.

`Portage` ships with a very sensible default configuration, but for most of us
on `Gentoo` -- there is near always room to further streamline the process. In
this post, I detail the tweaks and enhancements I have enabled for the `Gentoo`
install on my desktop. I would consider these to be "safe" enhancements in that
most should not negatively impact the system. Exceptions to that are noted
within.

.. TEASER_END

.. contents:: Jump to section

What this post is not
---------------------
This post does not intend to cover the basic aspects of `Portage`. How to manage
packages, unmask packages, synchronize with the main repos, etc can be read
about in much more detail over on the `Gentoo Handbook: Portage`_ page. The
features and use are covered very comprehensively thus I will assume a basic
understanding of `Portage` and how to work with it for the purposes of this
post.

Managing portage files
----------------------
`Portage` is configured through a series of variables, and configuration files
read out of the ``/etc/portage`` directory. Traditionally, this was done within
individual, flat files. At some point, presumably while I was away from Gentoo
over the past few years, the preferred method became to store these files inside
directories named after their content. As taken from the `Portage manpage`_...

.. pull-quote::

  Files in this directory including make.conf, repos.conf, and any file with a
  name that begins with "package." can be more than just a flat file. If it is a
  directory, then all the files in that directory will be sorted in ascending
  alphabetical order by file name and summed together as if it were a single file.

This ability to "break down" your configuration into individual files allows for
a much easier, and sensible way to store and configure `Portage`. For example,
if you want to try `AwesomeWM`_ but would like to configure some ``USE`` flags,
you can place that configuration into ``/etc/portage/package.use/awesomewm``.
Should you choose in the future to remove that package, and revert your changes
to ``USE`` -- it's as simple as deleting that individual file.

This provides a method to iteratively build up your `Portage` configuration
over time, tailored to your specific set of packages. No longer needing to
worry about remembering which flag was set, and why. Or needing to dig through
a massive single file, looking for instances of a specific flag.

If your `Gentoo` installation is still using the flat files, my recommendation
is to start moving them over into the directory structure bit by bit. You can
easily make the switch with a few commands. But the work of splitting the
individual packages, and variables into filenames that make sense for you is
something you must tackle on your own. If you've got an automated trick to
handle that, I'd love to hear about it.

The commands detailed below explain how to split your ``package.use`` file into
a directory. The steps will be the same for other files within ``/etc/portage``.

.. class:: alert alert-warning

  These commands should be run either as root with ``su -``, or with ``sudo``
  in front of them. Safe practices include making backups of the original files
  before moving them around, or changing them. Feel free to remove any of these
  ``*.backup`` files after verifying everything was sucessful.

.. code::

  cp /etc/portage/package.use ~/package.use.backup
  mv /etc/portage/package.use /etc/portage/package.use.old
  mkdir -p /etc/portage/package.use
  mv /etc/portage/package.use.old /etc/portage/package.use/use_flags

Once that is complete, start going through ``/etc/portage/package.use/use_flags``
and break them down into individual files. The format is the same as in the
standard ``package.use`` file. I personally use a system where each package
that I explicitly install, and it's dependencies (if need be) have their use
flags set into a file named ``/etc/portage/package.use/<PKG>`` As an example,
for ``package.use/vim`` I have the following::

  app-editors/gvim python
  app-editors/vim python terminal

.. class:: alert alert-info

  When `Portage` reads through these directories and begins sourcing from the
  files within, it follows an alphabetical order by file name with the contents
  summed together as if one larger file. This means that any variables, or
  flags set in files that are loaded *after* another will override those initial
  values. A commonly used technique to manually control the order files are
  read is to prefix a number in front of the file such as ``10-vim`` or
  ``25-nano``. In that case, the ``nano`` file is read last even though
  alphabetically the letters come after ``vim``.

Compiler options (``CFLAGS``)
-----------------------------
This is probably my favorite part in setting up a new `Gentoo` installation --
Compiler optimization! I would probably say this is one of the more misunderstood
areas. The wrong settings can prevent programs from building, or can cause
problems at run-time. There are a huge number of different configurations, or
optimizations you can set. I will focus on the ones I use and recommend, and
explain how I come up with those ``CFLAGS``. It's very straight-forward to get
the basic flags set. Deeper optimizations, optimizing for cross-compilation,
and similar will not be directly covered in this section but I have listed out
a variety of resources if you want more.

The ``CFLAGS`` and ``CXXFLAGS`` variables are defined in ``/etc/portage/make.conf``
and are exported into the environment that `Portage` uses when building a
package. Most commonly, they are passed into ``gcc``, ``clang``, or whichever
C/C++ compiler is being used. Essentially, these are command line arguments.
These are used to control the level of optimization, or to enable certain
instructions sets when the compiler does it's thing. I personally keep my flags
a bit conservative to ensure my packages build and run properly.

.. class:: alert alert-danger

  If you will be building packages to be run on *a different* system, such as
  with ``distcc`` -- do not use ``-march=native``. Packages built using that
  flag will be optimized for the host only and may not run at all on other
  CPU types. Consult the `distcc CFLAGS`_ page for more information.

A basic, and recommended default set of ``CFLAGS``, which will attempt to
automatically detect the CPU architecture is to use ``-march=native``. This
detection is said to work well in nearly all cases and should be fine for most
users. If you know your CPU architecture, and wish to state is explicitly, you
can simply replace ``native`` below with your own, such as ``ivybridge`` or
``bdver4``. These basic flags were taken from the `Safe CFLAGS`_ article on
the `Gentoo Wiki`::

  CFLAGS="-O2 -pipe -march=native"
  CXXFLAGS="${CFLAGS}"

If you would like to specify your CPU architecture, but are unsure of which
you are running. The following command will output a few lines with the CPU
family, and model number. Those two numbers can be used in the `Safe CFLAGS`_
wiki page to determine your architecture, as well as a sensible set of flags
to start off with. ::

  $ grep -m1 -A3 "vendor_id" /proc/cpuinfo
  vendor_id	: GenuineIntel
  cpu family	: 6
  model		: 58
  model name	: Intel(R) Core(TM) i5-3570K CPU @ 3.40GHz

That is all I use in my ``CFLAGS`` and ``CXXFLAGS``. Keep it simple, and don't
go overboard trying to optimize. Most of the heavy optimizations will be
specific to certain packages, and when enabled globally can cause various issues.
Larger binaries, slow performance, more room for strange bugs and crashes. For
a bit of an explanation into the heavy optimizations and why you might not want
to enable them right off the bat, check out the `Optimization FAQs`_ on the
``Gentoo WIki``.

CPU Features
============
This is along the same lines as the above configuration, however this changes
focus from how programs are optimized when being built, to enabling certain
advanced instruction sets within the CPU. Things like AVX, SSE, MMX, and similar.

There are some tricks to determine which instruction sets your CPU supports.
I am going to describe the method I used, which is an automatic detection. By
utilizing the `cpuid2cpuflags`_ program, the guess-work is eliminated. The
program will print out a valid ``CPU_FLAGS`` line which can be placed into
a file such as ``/etc/portage/package.use/00-cpuflags`` with a simple one-liner.
You may need to install ``cpuid2cpuflags`` as it is not included by default
in Gentoo.

.. class:: alert alert-info

  In the above command, the ``"*/*"`` is telling `Portage` to apply those USE
  flags to *all* packages. Should you want those flags only for certain packages,
  or want to use specific sets of flags for certain packages, the same syntax
  as in ``/etc/portage/package.use`` applies.

.. code::

  $ cpuid2cpuflags
  CPU_FLAGS_X86: aes avx f16c mmx mmxext pclmul popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3
  # echo "*/* $(cpuid2cpuflags)" >> /etc/portage/package.use/00-cpuflags

Further reading
===============
That should be all you need to set up a safe level of optimization, while
ensuring the CPU architecture is utilizing all available instruction sets.
Modern compilers are very good at optimizing on their own. And modern CPUs
are fast enough that most people don't need to worry about saving a couple
extra clock cycles. For every optimization you make, there is a trade somewhere.
That trade can come at the cost of disk space, time spent compiling, or even
performance. It would be wise to maintain a set of sensible defaults, only
enabling more advanced optimizations (such as LTO, PGO) for specific packages.
Even then, there is a trade off and they may not be free of problems.

If you would like to read more in-depth on these settings, there are plenty of
resources in addition to the ones linked throughout this section. These are
a few that I referenced when writing this post, and when originally setting
this installation up.

- `Safe CFLAGS`_
- `GCC Optimization`_
- `Invoking GCC`_

Use system RAM to reduce disk I/O
---------------------------------
During a build, many files are read and written in very quick succession. This
can present a significant bottleneck where the CPU is simply waiting on disk
I/O. It is possible to use a temporary file system in the system RAM, ``tmpfs``,
as the location where `Portage` will store build files. By doing so, you can
eliminate some of that bottleneck as system RAM is many magnitudes faster than
even the fastest hard drives. This has the potential to reduce disk I/O and
wear, and increase build speed at the cost of less available system RAM while
the ``tmpfs`` is mounted.

However it also has more potential for things to break, as certain packages
will require large amounts of storage to build. Packages such as ``chromium``,
``libreoffice``, ``rust``, and ``gcc`` are several known to be "heavy" packages.
As such, unless the system has a large amount of RAM, this may not be feasible
or worth the trouble. The more memory you allocate in the ``tmpfs``, the less
system RAM is available for other purposes. This may increase your risk of
encountering an out-of-memory condition, which is not a fun time, trust me.

.. class:: alert alert-info

  Do note that the actual ``tmpfs`` does not use memory (possibly a tiny bit to
  set up an empty filesystem) until files are added into it. For example, if you
  were to allocate 8GB for the ``tmpfs``, and have 16GB of RAM, you will still
  have 16GB available RAM until there are files inside the ``tmpfs``. Once the
  files go in, RAM is used. Think of a ``tmpfs`` as a sort of dynamically sized
  partition, which expands as it is used, up to the size specified in your
  ``/etc/fstab`` file. More on that in the link below.

It is relatively simple to set this up, so long as you pay attention, and I
found it did show benefits in my case. Due to the more advanced setup, and
other considerations to be made when doing so, I am not going to detail the
full process here. The `Gentoo Wiki` has a wonderful, and well-written page
which helped me to set this up some time ago. Set up and `build packages in RAM with Portage`_.
Pay attention to "Per-package choices at compile time" section. This is
how you would configure `Portage` to *not* use the ``tmpfs`` for building known
to be heavy packages.

Parallel builds
---------------
One of the newer tweaks I enabled for `Portage` is allowing multiple packages
to install in parallel, when possible. The way this works is by specifying some
arguments, and values passed into the ``emerge`` command by default which tell
`Portage` to build up to a set number of packages at once, based on the current
system load average. I found this to have improved the overall build times for
*whole groups of packages* -- not single builds. The values I set for my system
have not shown any decrease in responsiveness, even while heavier packages
such as ``sys-devel/gcc`` or ``sys-devel/llvm`` turned my desktop into a space
heater as they compile.

`Portage` will attempt to honor dependencies by waiting until the required
dependencies are built before starting on the dependants. As an example, if you
are building ``app-editors/nano``, which depends on ``sys-libs/ncurses``,
`Portage` will wait for the latter to finish building before it begins on
``nano`` -- even if the system load is below the set value, and there are "free
spots" for a new build to begin. The system is not perfect and *some packages
may fail*. However, I have not had any failures since enabling this tweak. If
you do encounter one, you may want to try again with these tweaks disabled
to determine if the problems lies here, or in something else.

.. class:: alert alert-info

  When parallel builds are enabled in `Portage`, the build output is not sent
  to the terminal. This can be a speed improvement in itself, but can make it
  harder to diagnose errors if a build fails. If you would like to see the
  output as the package builds, in another terminal you can run ``tail -f
  /var/tmp/portage/<CAT>/<PKG>/temp/build.log``, where ``<CAT>`` and ``<PKG>``
  make up the package's name such as ``sys-apps/sed``.

Alright, let's get this set up. The values we will be adjusting all live in
``/etc/portage/make.conf``. If that path is a directory for you, then you
may want to save the file with a name such as ``/etc/portage/make.conf/parallel``.
The values set to each variable is going to be system-dependant, and what works
for me may not work for you. There are some guidelines to help you determine
sensible starting points for these values, where you can adjust as needed. I've
laid out the relevant `Portage` variables, my values for them, and a bit about
how to determine a good value for your own system.

``MAKEOPTS = "-j4"``
  Typically set to the number of *logical* CPU cores. This number will start up
  to that many jobs in parallel, within a single package. This is separate from
  the ``--jobs`` option below. This represents the maximum number of tasks to
  start at once for a single job.

  The ``lscpu`` command can be used to determine the number of logical cores, if
  you are unsure. In general this will be the number of physical cores you have,
  multiplied by the number of threads per core. If you have a quad-core system
  with SMT or Hyper-Threading, this might be ``4 cores * 2 threads/core = 8``.
  More information available on the `MAKEOPTS`_ Gentoo Wiki page.

``PORTAGE_NICENESS = 15``
  Standard Linux niceness values apply here. ``15`` to ``20`` is a good starting
  point. The higher the number, the "nicer" a process will be in terms of giving
  up CPU time for a less "nice" process. This will play a big impact into how
  responsive the system feels during a built. Setting this too low may cause
  the system to perform sluggishly, though builds may complete faster. A more
  in-depth explanation on "niceness" is available on `Wikipedia`_.

``EMERGE_DEFAULT_OPTS = "--jobs 2 --load-average 3.6"``
  This variable can be used to pass in a set of default command line arguments
  that will be passed to the ``emerge`` command automatically. In this case,
  ``--jobs`` tells `Portage` to attempt to build up to this many number of
  packages at the same time with respect to dependencies. The ``--load-average``
  is used in combination with the previous argument. It tells `Portage` that
  if the system load average is above the argument's value, ``3.6``, it should
  *not* start any more than a single build at once. This helps prevent your
  system be being overloaded with more builds at once than it can handle. A
  typical starting value might be ``Number of CPU cores * 0.9`` which will
  limit the load to ``90%`` to help ensure responsiveness. `EMERGE_DEFAULT_OPTS`_
  on the Gentoo Wiki was a helpful reference for me when I set these   tweaks
  up a couple months back.

``FEATURES = "parallel-install"`` (Optional -- recommended)
  By enabling this feature, `Portage` will "use finer-grained locks when
  installing packages, allowing for greater parallelization." From what I can
  tell, this adjusts the way `Portage` looks for potential file collisions
  when installing packages. I do not know entirely what this does beneath
  the surface. While not *required* for parallel builds to work, I recommend
  it as it claims to allow more room for parallelization and has not given
  me any problems in the couple months I have used it.

.. class:: alert alert-info

  From `EMERGE_DEFAULT_OPTS`_:
  When the ``MAKEOPTS="-jN"`` and ``EMERGE_DEFAULT_OPTS="--jobs K"`` settings
  are used in combination together, the number of possible tasks created would
  be up to ``N * K``. Keep these values in mind as you balance them towards
  each other. ``K`` being the number of separate packages to build, and ``N``
  being the number of tasks to create in *each* package.

With those new settings entered, `Portage` should now be able to build multiple
packages at once. It will not do so in all cases, as noted above, it was a
noticeable difference for me in CPU utilization and total time to install a
set of packages. In some cases, certain packages may not build when using these
tweaks. I have not encountered any of these cases yet. If you do encounter one,
try again with ``MAKEOPTS="-j1"``, or with the ``--jobs`` argument reduced or
removed.

Where to go from here
---------------------
`Portage` is a highly configurable package management system. The options
presented to you when customizing, or optimizing are nearly limitless. Granted,
attention should be given as some may negatively effect system performance.
This post should have provided you enough information to set up a few tweaks
that I personally found benefit in. Your mileage may vary, and I make no claims
or guarantees that any of this will work for you. That being said, these should
be safe, and I have not had any major issues myself with them.

This post hardly scratches the surface of everything `Portage` can do. There are
many other options, or features [#]_ built-in that I did not cover. The full
detail and supported features can be seen in the manpages for `Portage`
and ``emerge``. Read them. Learn to love them. They are very helpful.

Additionally, there are many additional packages which can work with `Portage`
to make life easier, or display additional information. Some of my favorites,
which I use near daily would be `eix`_, `gentoolkit`_, and `genlop`_. There
will likely be another post in the future regarding some of these packages
and how I use them to manage my Gentoo install.

Thank you for lending me your time, I hope you were able to benefit from this
post in some way. If there are issues, or suggestions, I can be reached at
`@ncdulo`_ on GitHub. May all your builds go quickly, and without error!

.. rubric:: Footnotes

.. [#] A fun little tweak I enjoy is to add ``candy`` into your ``FEATURES``
   such that ``FEATURES="candy"``. This replaces the default "spinner" animation
   when `Portage` is resolving dependencies with some scrolling text. How many
   different messages can you see in the scrolling text?

.. _`Portage`: https://wiki.gentoo.org/wiki/Portage
.. _`Gentoo Linux`: https://gentoo.org/
.. _`Portage manpage`: https://dev.gentoo.org/~zmedico/portage/doc/man/portage.5.html
.. _`AwesomeWM`: https://awesomewm.org/
.. _`Gentoo Handbook: Portage`: https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage
.. _`distcc CFLAGS`: https://wiki.gentoo.org/wiki/Distcc#CFLAGS_and_CXXFLAGS
.. _`Safe CFLAGS`: https://wiki.gentoo.org/wiki/Safe_CFLAGS
.. _`Optimization FAQs`: https://wiki.gentoo.org/wiki/GCC_optimization#Optimization_FAQs
.. _`cpuid2cpuflags`: https://github.com/mgorny/cpuid2cpuflags
.. _`GCC Optimization`: https://wiki.gentoo.org/wiki/GCC_optimization
.. _`Invoking GCC`: https://gcc.gnu.org/onlinedocs/gcc/Invoking-GCC.html
.. _`build packages in RAM with Portage`: https://wiki.gentoo.org/wiki/Portage_TMPDIR_on_tmpfs
.. _`MAKEOPTS`: https://wiki.gentoo.org/wiki/MAKEOPTS
.. _`Wikipedia`: https://en.wikipedia.org/wiki/Nice_%28Unix%29
.. _`EMERGE_DEFAULT_OPTS`: https://wiki.gentoo.org/wiki/EMERGE_DEFAULT_OPTS
.. _`eix`: https://wiki.gentoo.org/wiki/Eix
.. _`gentoolkit`: https://wiki.gentoo.org/wiki/Gentoolkit
.. _`genlop`: https://wiki.gentoo.org/wiki/Genlop
.. _`@ncdulo`: https://github.com/ncdulo
