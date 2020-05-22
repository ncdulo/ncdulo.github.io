.. title: Doom Emacs -- Best of both worlds?
.. slug: doom-emacs-best-of-both-worlds
.. date: 2020-05-21 20:15:13 UTC-04:00
.. modified: 2020-05-22 19:03:38 UTC-04:00
.. tags: thoughts, doom
.. category: emacs
.. link:
.. description: After some months away from Emacs, I may have found the best of both worlds for myself -- Doom Emacs.
.. type: text

For a couple months last year, I became borderline obsessed with `Emacs`_. While
initially a massive learning curve to begin grasping I found it extremely
powerful and intriguing. The first few days were quite challenging but I was
slowly finding my way through and thoroughly enjoying myself. Realizing the
power that was in my hands with `Emacs Lisp`, I couldn't help but dive headfirst
into configuring and tweaking into the perfect development and editor for
myself.

.. TEASER_END

With no prior experience with `Lisp`, not even realizing it was still in use in
the current day and age, I was very surprised at what something so simple (in
theory) could accomplish. It was not without headache and frustration though. It
seemed like every documentation, or introduction to `Emacs Lisp` was way over my
head and would require nothing short of a Computer Science degree to make sense
of. All this talk of ``cons``, ``car``, and ``cdr`` was enough to make my head
spin. Somehow, I trudged through and began tweaking my configuration. Simple
things. Nicer font. New colors. `Rainbow Mode`_. The list goes on.

It was never enough
###################
Thoughts of unlimited power coursed through my veins as I dove deeper into
the possibilities. I spent days (probably even weeks) tweaking and refining
an ``org-mode`` configuration based on the `Getting Things Done`_ system. It
even inspired me enough to buy the book, which I sadly haven't finished reading.
I was hooked, entranced by the power presented to me, with no practical
knowledge of how to apply it. That wouldn't stop me though. Not yet.

The deeper I dove, the more I began to see problems in my ``init.el`` & friends.
Simple solution? Try out some of the more minimal `Emacs` distributions and
configurations available online. Many of them were great, and I was particularly
fond of the `Purcell`_ and `Prelude`_ setups. Even using each of them for some
time on my own, expanding as necessary. Yet still finding reasons for wanting
more a totally custom setup. Eventually basing my last configuration off of
`Emacs Bootstrap`_ and setting off with a mostly blank slate and sensible
defaults.

Jumping in the deep end, with no idea how to swim
#################################################
Each passing day, I spent hours reading, adjusting and adding packages. At the
time I wasn't even working on any projects *except* for this `Emacs` config.
Yet I was setting things up for languages I hadn't touched in years. Things
mostly worked, but frequently they didn't. Strange errors popped up that left me
spending hours working to fix them. In some cases even entirely scrapping
whichever idea I had that I could not realize into code. My config quickly
became a horrible mess that *I* could hardly make sense of, let alone anyone
else.

In the beginning of October 2019 I migrated back home to `Gentoo`_, taking my
`Emacs` configuration with me. Mind you, before the migration, I was having
some strange errors which would totally lock up `Emacs` to the point it required
a little `kill -s SIGKILL` love to take down. As much as I attempted to track
down the issue, I could not figure it out. My config was too far gone, and too
far broken. Between that, and a couple issues other issues that seemed more
specific to how I built `Emacs`, it became unbearable. I just could not get any
work done. It might crash in five minutes, or it might take five hours. It got
to me. Bad. Eventually, I had enough and rage-purged `Emacs` from my system and
made it a point to learn `Vim`_. After all, it is included out of the box on
nearly every `Linux` distribution I had ever worked with. May as well, right?

Simplicity, but not much else
#############################
Once I began to get over the initial hurdle of finding my way around `Vim`, it
felt great. I was very impressed by what it could do, how quickly it worked,
and how lightweight it felt compared to my utterly destroyed `Emacs` config.

I actually found myself very happy with `Vim` and hardly felt the need to
install any plugins, or make heavy tweaks. Just my `Zenburn`_ theme to match
my system, and some edits to the `.vimrc`_ file kept me quite content for a
while.

Several months of enjoyment and `Python` development work blew by me before I
began to feel limited again. I was missing my ``org-mode``, ease of extending,
and just plain missing the power `Emacs` presented to me.

Vim is Doomed
#############
While I was not entirely feeling extremely limited by `Vim`, it wasn't the
end all be all that I originally felt it was. The built-in terminal support
felt lacking, and awkward to use so I made very heavy use of ``C-z`` to back
and forth as I worked on projects. It was almost distracting in a way, and
nearly always resulted in many forgotten instances of `Vim` floating around
various terminals. The lack of a decent GUI version was a bit limiting in it's
own way. Not that I want flashy UI elements, or toolbars and such. I just find
a certain appeal to being able to launch my editor of choice in a dedicated
window and keep it open at all times. `GVim` was not my idea of that. I was
feeling limited, and watching much of the `Mythical Crew` get themselves onto
`Emacs` made me realize more what I was missing out on.

Yet I didn't want to go totally back to `Emacs`. I would have to re-learn the
keybinds and toss away much of what I learned during the few months in `Vim`.
To me, that left one main option. `Evil mode`_. It would allow me to use a
horrific combination of keybinds and functionality from both worlds.

I needed something that would come mostly pre-configured out of the box. With
sensible defaults, support for the languages and features I use, and the ability
to expand that setup as I find areas in need of adjustment to suit my flow.

`Spacemacs`_ was the first distribution that came to mind. But I *really* did
not like that when I tried it out some time ago. Couldn't put my finger on it,
but something just felt off. It felt a bit heavier than what I wanted, and
did not seem to be as easily expandable without a deeper knowledge of `Lisp`.
Thus I took a pass on trying it again.

Tossing around thoughts of a few of the previously mentioned distributions from
my original experience with `Emacs` had me feeling I was about to repeat the
process that got me into this mess in the first place. Something entirely new
to me was needed. Something like `Doom Emacs`_.

  "It is a story as old as time. A stubborn, shell-dwelling, and melodramatic
  vimmer—envious of the features of modern text editors—spirals into despair
  before he succumbs to the dark side. This is his config."

That alone was enough to tell me -- `Doom` is your best shot at success. I had
to give it a shot. Admittedly, I was a bit thrown off by the inclusion of a
separate script for managing the `Doom` installation but I went ahead with it
anyway. As it turned out, everything went without a hitch and I find myself
actually grateful for that script. It made the installation, and future
management of my setup *much* easier and pain-free than I imagined it would
be. Nowhere near as much micromanagement of packages, user-config, or updates.
I was loving it from the first moment I fired it up.

Welcome back home, I've missed you, Emacs
#########################################
Of course, the first few hours were spent struggling to discover my old
functionality or keybinds to properly use `Doom`. Slightly complicated in that
reading the official `Emacs` documentation wouldn't help me with the keys. I
did catch my footing, and rediscovered the beautiful way `Emacs` is
self-documenting and incredibly helpful. It did not take me long to begin, to
some extent, proficient enough to begin using `Emacs` over `Vim`.

Granted, it hasn't even been a week yet, and I'm nowhere near as proficient
as I was originally. With `Evil mode`, much of that re-learning was mitigated
and I feel comfortable handling basic edits and workspace management. Not to
mention how much more responsive and fluid working in `Emacs` feels.

Granted, we can't let ourselves fall back into the same trap that got me before.
I have let myself settle into knowing the `Doom` setup is working very well for
my needs, and I *do not* actually need to write a configuration of tens of files
spanning hundreds (thousands?) of lines which hardly work together. It just
works. Make changes as they are needed, not because I might want a specific
feature some day in the distant future, if ever.

Off into the sunset
###################
Here we are now, comfortably writing this post in `Doom Emacs`, enjoying every
bit of it. Especially the ``fill-paragraph`` command. Words don't even describe
the frustration I had at times, manually re-flowing a paragraph in `Vim` because
I changed a single word somewhere. I've even been taking the opportunity to give
`Lisp` a second shot. I haven't actually created anything truly useful with it
yet, but I feel I am beginning to understand it. Much of that is thanks to Steve
Yegge's `Emergency Elisp`_ post, which goes over the basics of `Elisp` syntax
in a way that actually makes sense to me.

While I don't plan (for now at least) on writing the next `Magit`_ or major
extension to `Doom Emacs`, I do have some ideas. Simple things that will make
my life easier when I work. Almost certainly these ideas will grow into
something larger over time. For now, I'm making it a point to just enjoy `Emacs`
and take it slow. No sense jumping right in to the deep end straight from the
start.

Here's to you, `Emacs`, and many more buffers to come in the days (years?) to
come. Thank you for taking me back, with a gentle and caring hand, right when I
needed you the most.

.. _`Emacs`: https://www.gnu.org/software/emacs/
.. _`Rainbow Mode`: https://elpa.gnu.org/packages/rainbow-mode.html
.. _`Getting Things Done`: https://gettingthingsdone.com/
.. _`Purcell`: https://github.com/purcell/emacs.d
.. _`Prelude`: https://github.com/bbatsov/prelude
.. _`Emacs Bootstrap`: https://github.com/editor-bootstrap/emacs-bootstrap
.. _`Gentoo`: https://www.gentoo.org/
.. _`Vim`: https://www.vim.org/
.. _`Zenburn`: http://kippura.org/zenburnpage/
.. _`.vimrc`: https://github.com/ncdulo/dotfiles/blob/master/.vimrc
.. _`Evil mode`: https://github.com/emacs-evil/evil
.. _`Spacemacs`: https://www.spacemacs.org/
.. _`Doom Emacs`: https://github.com/hlissner/doom-emacs
.. _`Emergency Elisp`: https://steve-yegge.blogspot.com/2008/01/emergency-elisp.html
.. _`Magit`: https://magit.vc/
