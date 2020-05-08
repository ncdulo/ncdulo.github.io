.. title: Python 3.7: Smoother than expected
.. slug: python-37-smoother-than-expected
.. date: 2020-05-08 17:33:47 UTC-04:00
.. tags: gentoo, python, python3, linux, portage, thoughts
.. category: gentoo
.. link:
.. description: Gentoo Python now defaults to version 3.7. The switch went much more smoothly than originally anticipated. Thanks goes out to the Gentoo maintainers, and developers.
.. type: text

As I wrote in `another post`_ a few weeks ago, `Gentoo` has officially made
the switch. The new default `Python` version is `Python 3.7`! I'm was not
totally sure just *what* to expect with this update. Surely some things would
go wrong, and I would encounter some issues somewhere. As it turns out, I was
wrong.

.. TEASER_END

All week, even the week before, I have been anticipating with excitement the new
default target to switch. I could have made the switch early without too much
difficulty, I believe. I wanted to wait it out and get the full experience here.
Part of that reasoning is because I knew I had some packages which have not
been updated to `Python 3.7` yet. Mostly though, I wanted to see how `Portage`
would handle this on it's own with minimal intervention. And so I waited.

Early in the week, around Monday or Tuesday, ``readline`` also received a major
version bump. Due to how many packages rely on ``readline``, the system library
status of the package, I was in for quite a few rebuilds. While not a problem
on it's own, I noticed quite a few `Python` packages which would be rebuilt as
part of that ``readline`` update. This was not going to sit well with me. Why
spend the time to update several packages, only to rebuild them in another day
or two with a new `Python` version? And so I waited some more.

Wednesday: Python day!
----------------------
Wow, was I excited when I woke up Wednesday morning! Had the day off work (for
other reasons -- not specifically Python related) so the whole day is mine for
updating and shenanigans. Unfortunately, when I ran my first ``eix-diff`` of the
day, nothing. Tried a little ``portageq`` action as well, to check the actual
values of ``PYTHON_TARGETS`` and ``PYTHON_SINGLE_TARGET`` to no avail. The
thought did not occur to me until then that there was no set schedule for the
profile update. It will be available when it's ready.

Not going to let that get me down though. I found other ways to occupy my time.
Namely a whole lot of `Witcher 3`_. Thankfully, that game was more than enough
to pass the time. And could even be said to be the main reason I ended up
running the actual update the following day. Either way, I sneaked in a couple
extra ``emerge sync`` throughout the day, waiting to see when the new targets
were actually pushed out.

Later in the afternoon, they finally came, and initially I was presented with
a whole slew of errors. Unfortunately, I did not think to save them. Mostly
they were errors about inability to build certain packages with the new targets.
Some of my installed packages had not yet been updated to 3.7. Due to the fact
that those particular packages were *not* things I use regularly, or really at
all anymore, the fix was simple. Remove them. Just a couple packages to remove
and ``depclean`` and we're in business.

Just a little more back and forth with that process until `Portage` finally
stopped complaining, and I was presented with a quite large list of packages
to rebuild or update. By this time, it was already nearing 10pm, and with work
starting in another 8 hours, it was time for bed. Though I was glad knowing I
had already handled the bulk of the hard work and can simply let it run after
work.

Thursday: Make it so
--------------------
Little fast forward action and we're back at home. Double, and triple check
once more. Make sure everything looks sane. *Only* 126 packages on my list. No
big deal. ``emerge @world`` -- get it going!

My original estimate on the total build time was about 3 hours, possibly up to
4. Much to my surprise, the ``@world`` update took *exactly* two hours. Good
stuff! During a couple packages, I was quite a bit anxious. A friend of mine
mentioned having a lot of package slot errors, and some trouble getting
`Portage` to update at one point. I'm not sure what the cause of their errors
was, but I (very thankfully) did not encounter any.

With the update complete, I went ahead and updated the default Python version
through ``eselect python``, and verified the new version loads properly. Hit
up a ``emerge @preserved-rebuild`` as well, which required the `Python 3.6` to
be rebuilt against the new ``readline``. Not sure why this did not happen in
place with the others, but it went just fine. Finally, the small process to
restart several daemons/services running which depend on either ``readline`` or
`Python`, followed by a re-login to my `X` session and we're almost through. My
last set of things to check was my own `Python` projects. With the exception of
one, which was not properly using a `virtual environment`, everything handled
the update flawlessly. Color me impressed.

Thank you, Gentoo Python Team
-----------------------------
In the end, this was a fairly large update, with a lot of behind the scenes work
by the `Gentoo Developers`_, the `Gentoo Python Team`_, and of course the
package maintainers and developers of the actual packages which use `Python`.
I have noticed a huge uptick in `Python` related package work over the past
couple of months, at least. This work has not gone unnoticed, and I am very
thankful to have such a great team working to continue improving `Gentoo`.

Very big thanks goes out to the maintainers and developers who helped to ensure
this process went smooth and easy as best they could. No major version update
is going to be without problems, especially if the package being updated is
part of the "core" required packages by that distribution. The work put in over
the past couple months has done a great deal in helping to ensure there were as
few issues as possible. Thank you all for the hard work and dedicated. I truly
do appreciate the effort and care put into your work. Thank you for helping to
make `Gentoo` the one `Linux` distribution that truly feels like "home" to me.

.. _`another post`: link://slug/python3-out-with-the-old-in-with-the-new
.. _`Witcher 3`: https://thewitcher.com/witcher3
.. _`Gentoo Developers`: https://www.gentoo.org/inside-gentoo/developers/
.. _`Gentoo Python Team`: https://wiki.gentoo.org/wiki/Project:Python
