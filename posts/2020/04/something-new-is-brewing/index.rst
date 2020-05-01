.. title: Something new is brewing!
.. slug: something-new-is-brewing
.. date: 2020-04-30 20:24:12 UTC-04:00
.. updated: 2020-04-30 21:07:00 UTC-04:00
.. status: featured
.. tags: nikola, blog, webdev
.. category: devncdulo
.. link:
.. description: Not quite getting everything I want from the original design at /dev/ncdulo, follow my journey as I begin redesigning around a new base template.
.. type: text

Pardon our dust -- a redesign of ``/dev/ncdulo`` is underway! Read more and
catch all the details.

.. TEASER_END

While I was happy with the `Bootstrap4` template on ``/dev/ncdulo`` -- I was
feeling a bit like I wanted something slightly different. The design just was
not fitting the content in my mind. This disconnect was starting to leave me
some problems where I felt I could not envision what I want properly. No matter
how much I tried digging through `Bootstrap4` template, I could not figure
things out the way I wanted.

.. pull-quote:: Let's just start over again. Something simpler.

`BootBlog4` was actually the original theme I had planned to go with. It was
simple, plain, and looks great right out of the box. Not perfect. But the lack
of flashy features, and simpler design means my perceived level of effort to
change things up to fit my mind is lower. This is a good thing. I can happily
hack away and actually have an idea of what I'm doing.

So I went ahead and installed my own copy of `BootBlog4` into the project that
manages the `Nikola` back-end over here, and have been banging away at the
keyboard since. Not massively quick progress. I'm starting to understand it
though. And very much liking the progress I have been making.

We are not all there yet. And as of this writing, I'm still not at a point
where I want to actually deploy anything. Couple loose ends to tie up before
that. I do plan on getting a rough draft finished up, along with some better
design choices compared to my previous attempt. Then deploy, and continue
tweaking the small bits over time.

As of right now, the main areas in need of attention would include:

- The footer's gray background does not extend the full page width
- Featured posts no longer include preview images (Feature, not a bug!)

  - May be possible to add the preview images back, but use them as a
    container background image instead of side-by-side. The side-by-side was
    screwing things around and leaving scrollbars where they should not be.
  - Jumbotron does not look as nice as it should -- colors

- Sidebar should include category, archive listing
- Column widths may require additional tweaking
- Fonts should not include Google. Use System UI Stack.
- Need a proper color scheme. For real now.

After much frustration, and failure
-----------------------------------
I just cannot seem to get things working properly in terms of color changes.
The "proper" way to handle this would be to download the `Bootstrap4` source
code, modify the SCSS files, and rebuild. For whatever reason `Node.js` just
flat out refuses to properly work. My own lack of understanding there is more
than likely the culprit. Just more fuel to the fire that is my growing dislike
of Javascript over the past twenty years.

For now, I'm going to keep hacking away at is. Progress will likely be slow,
and unfortunately, I feel the color scheme is going to be the last thing I
manage to get set up properly. Even the web-based tools I have tried seem to
be failing me.

Moving forward -- I may be able to compile the SCSS on it's own *without* using
`Node.js`. If that continues to be an exercise in futility, my last ditch
effort will be manually overriding the `Bootstrap4` CSS directly. I recognize
that is not the best way to do so, as updates may break my changes. If that's
how it is going to be, so be it. I *will* make this work, somehow.
