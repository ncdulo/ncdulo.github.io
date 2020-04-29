.. title: Python3: Out with the old, in with the new
.. slug: python3-out-with-the-old-in-with-the-new
.. date: 2020-04-23 04:53:01 UTC-04:00
.. updated: 2020-04-29 15:34:41 UTC-04:00
.. tags: python, gentoo, linux, python2, python3
.. category: gentoo
.. link:
.. description: Updating the default Python 3.6 to 3.7 on Gentoo Linux
.. type: text


Oh, happy day! I truly do enjoy Python version bumps. With `Python 2` at EOL,
and `Gentoo Linux`_ making big moves over the past couple of months to bump the
Python3 default target to `Python 3.7`, it has been a very exciting time for
me to be working so much with Python. Though a bit confusing at times, the
care and attention put in by the `Gentoo Python Team`_ has made this an easier
transition to go through.

.. TEASER_END

``diff gentoo``
---------------
As part of my usual morning routine, one of the first things I check on the
computer is a ``diff`` of the changes introduced to the Gentoo package
repositories since my last sync [#]_. Honestly, it's a great way to start the
day for me. I highly recommend the eix_ utility for *much* quicker package
searching than `Portage` allows for, if nothing else.

Anyway, the point of that mention -- this morning I noticed `Python 3.8` had
been bumped to stable on ``amd64``. Finally! I couldn't quite determine if this
was intended to be another slotted version, or if it would coincide with other
updates to the `Gentoo Python` ecosystem. Checking a bit deeper into this now,
I noticed a news item, `Python 3.7 to become the default target`_.

Wow! This is getting exciting! Reading through that was initially a bit
confusing, but it *really* made my day. Being that it's a weekday, I did have to
put that on hold and head to work. It did give me an opportunity to read over
the news item a bit more, and consider how to handle this update. With Python
being *just* a bit integral to Gentoo, I have decided to not rush into this,
and let `Portage` do what it wants to do. For me, this means simply accepting
the update without modifying either of the ``PYTHON_{TARGETS,SINGLE_TARGET}``
values. At this moment, if you are using the default targets, no further action
is required at this time. Simply ``emerge -uDNatv @world`` as usual, and carry
on. For me, the only Python related change there was pulling in version 3.8 as
it is now marked stable. At least on ``amd64`` that is.

What if I don't want to wait?
-----------------------------
Two weeks is quite a long time away, I know. Thankfully, you can go ahead with
the migration, if you like. How cleanly, or painfully this may happen will
somewhat depend on the packages installed to your system which utilize `Python
3.6` as their default target. If you utilize any services, or daemons that target
that version, you may wish to apply the update in parts, described below.
Alternatively, if those programs can be stopped during the update, it may help
ensure no errors occur. The news post linked here_ does mention that already
loaded & running programs should not encounter errors during this update.
However, if they are restarted or load additional modules during this process,
there may be errors. If you *do* keep them running during the update, you will
need to restart them after it has completed so they can load up again using the
new `Python 3.7` target they will have been rebuilt for. My personal
recommendation is to apply the update in parts to minimize any risk of breakage
to currently running programs.

.. sidebar:: Why reiterate?

  The `Gentoo News` post lists out all of the relevant steps, though I at first
  had a slight bit of difficulty making them all out. Thus I have compiled the
  steps here, with my own thoughts in hope it may be of some help to another
  person.

How do we do this?
------------------
The news item lists out all of the relevant steps, and should provide ample
information to proceed without any major issues. The processes laid out below
come from that news item, with my own thoughts. Three different scenarios are
described below

Update all at once
==================
So long as you have not overridden either ``PYTHON_{TARGETS,SINGLE_TARGET}`` on
your system, the update should happen "When It's Ready™". This is expected to be
on, or after May 05, 2020 as per the `Gentoo News` item. Simply stay on top
of your regular system updates and it will happen on it's own. Once it *does*
happen, you will want to clean up any stray packages with commands such as:

.. class:: float-left alert alert-danger

  The ``depclean`` command has the the potential to remove important packages
  from your system. **Always** check the list of packages to remove before
  proceeding. If you are uncertain and cannot determine on your own, ask or
  skip over it, leaving the package in question installed.

.. code:: sh

  emerge --depclean --ask
  emerge -1vUD --ask @world
  emerge --depclean --ask

Now may be a good time to update your default Python version as well

.. code::

  $ eselect python list
    Available Python interpreters, in order of preference:
      [1]   python3.6
      [2]   python3.8 (fallback)
      [3]   python3.7 (fallback)
      [4]   python2.7 (fallback)
  $ sudo eselect python set 3


This series of commands will check `Portage` for any packages which are no
longer required, based on the dependency tree. Then update any packages which
have had changed ``USE`` flags (such as ``PYTHON_TARGETS``), while taking the
entire dependency tree into account. Finally, a last ``--depclean`` to clear
out any new packages which may no longer be required. This should leave your
system with packages rebuilt to prefer `Python 3.7` whenever possible. Any
packages installed in the future will follow those new defaults as well.

If you would like to improve the stability of the upgrade, use the steps
outlined below instead.

Update in stages
================
Temporarily enabling both `Python 3.6` and `Python 3.7` targets may improve
stability of the upgrade. This will cause the dependencies effected by the
update to build with support for both versions wherever possible. The file
edited to enable both Python targets will depend on whether you have a file, or
a directory at ``/etc/portage/package.use``. If there is a file at that path,
edit it with the following values. If there is a directory at that path, edit a
new file with a name such as ``/etc/portage/package.use/python-targets``::

  */* PYTHON_TARGETS: python3_6 python3_7
  */* PYTHON_SINGLE_TARGETS: -* python3_6

With those two lines added, you should proceed with the first stage of the
update. This will rebuild packages with additional support for `Python 3.7`
where applicable, without removing any `Python 3.6` support yet.

.. class:: float-left alert alert-danger

  The ``depclean`` command has the the potential to remove important packages
  from your system. **Always** check the list of packages to remove before
  proceeding. If you are uncertain and cannot determine on your own, ask or
  skip over it, leaving the package in question installed.

.. code:: sh

  emerge --depclean --ask
  emerge -1vUD --ask @world
  emerge --depclean --ask

Take the time now to restart any running scripts, daemons, or programs that were
included in that update list. Then either comment out, or delete the two lines
previously added into your ``/etc/portage/package.use``. You may also want to
update your default Python version at this time through the ``eselect``
command::

  $ eselect python list
  Available Python interpreters, in order of preference:
    [1]   python3.6
    [2]   python3.8 (fallback)
    [3]   python3.7 (fallback)
    [4]   python2.7 (fallback)
  $ sudo eselect python set 3

The final step here is to re-apply the update. This time only `Python 3.7`
support will be built, where applicable. Certain packages may not yet have
been updated to support `Python 3.7`. In these cases, the `Gentoo Python Team`_
is working to push out the updates in time. For those reasons, not every package
will rebuild into `Python 3.7` support right away. As mentioned in the beginning
of this section, **pay attention to the `depclean` command -- it can remove
important system packages!**

.. code:: sh

  emerge --depclean --ask
  emerge -1vUD --ask @world
  emerge --depclean --ask

Finally! This all complete, you should be finished up now. Your Gentoo system
should now be using `Python 3.7` as the default version. Packages should have
been rebuilt to prefer `Python 3.7` over other versions, where possible. And
packages built in the future will follow in that preference.

Postpone the update
===================
Should you not wish to update the system default Python target for whatever
reason, you can simply block the target. This will prevent `Portage` from
attempting to build that target, using `Python 3.6` instead as it does
currently. The file edited to enable both Python targets will depend on whether
you have a file, or a directory at ``/etc/portage/package.use``. If there is a
file at that path, edit it with the following values. If there is a directory
at that path, edit a new file with a name such as
``/etc/portage/package.use/python-targets``::

  */* PYTHON_TARGETS: -python3_7 python3_6
  */* PYTHON_SINGLE_TARGETS: -* python3_6

Should you decide to go ahead with the update in the future, comment out or
remove those two lines. Then proceed as described above, or in the main `Gentoo
News` item here_.

In conclusion
-------------
With `Python 3.6` no longer receiving bug fixes, and will be approaching it's
own end-of-life just before Christmas of 2021, it is a good time to update.
Enjoy the added features such as nanosecond timers in the ``time`` module, or
Data Classes. Lots more details at official `What's new in Python 3.7`_ page.
Whether you update all at once, next year, or jump straight to `Python 3.8`,
it is important to keep your system regularly updated. This guide was written
to help facilitate that, and assist any other `Gentoo Linux` users who find
themselves uncertain on how to proceed.

.. rubric:: Footnotes

.. [#] To create, and check the repo ``diff``, I utilize the ``eix`` utility,
  in combination with Portage's ``postsync`` hook. The method I used to set this
  up is on this `Gentoo Wiki`_ page. This utility has become almost essential to
  me as I want to know exactly what changed throughout the repos.

.. _`Gentoo Linux`: https://www.gentoo.org/
.. _`Gentoo Python Team`: https://wiki.gentoo.org/wiki/Project:Python
.. _`eix`: https://github.com/vaeth/eix/
.. _`Python 3.7 to become the default target`: https://www.gentoo.org/support/news-items/2020-04-22-python3-7.html
.. _here: https://www.gentoo.org/support/news-items/2020-04-22-python3-7.html
.. _`Gentoo Wiki`: https://wiki.gentoo.org/wiki/Eix#Managing_the_cache
.. _`What's new in Python 3.7`: https://docs.python.org/3/whatsnew/3.7.html
