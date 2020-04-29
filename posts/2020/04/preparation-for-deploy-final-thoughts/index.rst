.. title: Preparation for deploy, final thoughts
.. slug: preparation-for-deploy-final-thoughts
.. date: 2020-04-21 18:10:08 UTC-04:00
.. updated: 2020-04-29 15:33:08 UTC-04:00
.. tags: meta, webdev, blog, project
.. category: devncdulo
.. link:
.. description: Sharing my final thoughts as I prepare to deploy this website.
.. type: text

Hard to believe how quickly, and how well this project is coming together. It's
infancy began some months ago, but never quite got off the ground. Then about a
week ago, a rekindled interest sparked up. And just like that -- here we are!
Reaching a point now where I feel comfortable with this base, I'd like to get
this online. For final testing, and of course to open up for visitors. These
are my final thoughts as I prepare for a proper deploy.

.. TEASER_END

Simple enough, yet highly functional
------------------------------------
The goal for this website is simple. A place to collect my thoughts, write blog
posts, or show off my projects. I wanted something fast, and simple to work
with. While entirely doable, this did not feel like I need to set out and use
a big framework, or design my own dynamic solution. Though at the same time,
designing and maintaining a static website with more than a handful of pages is
not easy. Something I began to see evidence of with the original content on my
GitHub pages site. Keeping these basic criteria in mind, I began trialing a
couple different solutions.

Nikola_ was the first one I tried out. Seemed to tick all the boxes for me,
has a great amount of community, and a nice amount of documentation. However,
initially something just felt a bit off. I was having a hard time wrapping my
head around the work flow, and determining exactly how I should create my
project. Very quickly things felt too complex and like my original vision was
being lost. Not good at all!

When suddenly -- a Pelican_ caught my eye. Felt like a much simpler and nicer
to use version of Nikola. I *really* liked this one right out of the gate. It
felt fast enough. The output looked ideal for what I envisioned. And I even
began to see everything coming together -- finally! Yet those same feelings of
complexity, and loss of vision began creeping back in. What's going on here?
I don't normally jump around like this after I get it in my mind.

Honestly, I think a big part of that just boils down to the new work flow. I'm
not used to this at all. Previous projects could live happily on forever, in a
single `Git` repository. Never caring where they were, or who else was around.
This new setup.. While it *could* be that easy. I'm not sure I *want* it to be.
Just need to find some way to complicate things over here. There is a basic
organizational structure (not quite yet) in place which should maintain a more
full separation of code, design and content at the `Git` level. More on that
later.

Nikola: misunderstood, genius, innovator
----------------------------------------
And so I found myself nearly right back at the beginning. Firing up a fresh
project with Nikola, and slowly building my structure back up. This has taken
a very iterative approach thus far. Treating this as one big brainstorm session
in a way, and finding myself very happy with the results. Granted it did not
all come so easily. All these different ideas, different work flows, and new
ways of going about things is going to take some time. But we've (finally!) got
a workable base that we can build up on.

One of the more challenging bits for me has been deciding upon a directory
hierarchy to keep the content organized. A solid directory structure helps users
to navigate the site better. For me, human-readable URLS that make sense are
important, so I made several tweaks into the configuration to facilitate that.
This includes storing blog posts under the ``posts/<Year>/<Month>/`` directory
tree. As well as separating the tags, and categories pages into separate
destinations.

The sum of these changes leaving us with customized, and configured copy of
Nikola that should fit our needs wonderful for now. This is certain to change in
time. For now, I think it's really nice considering I have yet to do any real
theme work. That *will* be coming. I've got a few ideas floating around for
that.

``git add hopes_dreams && git commit``
--------------------------------------
The moment of truth is nearly upon us! I wanted to write out this post, as well
as leave myself some time to double, and triple check the output contents. There
are several items that should not be published. Nothing too good. Just a few
test posts that wouldn't otherwise make sense, images that are potentially
licensed, and the `Nikola` demo content installed by ``nikola init --demo``.

It's also now a good time to really think hard about how we version control this
project and related pieces. Due to the licensed, and demo content that was
committed during the initial test phases of this website, I want to break it up.
If nothing else, that will mean deleting the ``.git`` folder in my development
copy of this website and re-initializing it without a history. However, I think
a solution I like better will be to keep things separate.

Using a series of symlinks, I would like to use maintain separate repositories
for the custom theme being created, the Nikola project base, and the content.
I can foresee some areas that will need attention. Such as, do we commit
``pages/`` or ``output/``? How can we deploy to GitHub pages if the destination
repo is *not* the one we are currently inside? Is that even possible? For now
though, we've got something good to work off of. I have no problem to manually
deploy our content to our `GitHub Pages`_ repo.

For now, I'm just gonna keep at it, and have fun. I'll never have things 100%
right from the start. The best things take time, and this is no different.

.. _Pelican: https://blog.getpelican.com/
.. _Nikola: https://getnikola.com/
.. _`GitHub Pages`: https://github.com/ncdulo/ncdulo.github.io
