.. title: Quick & easy management of Nikola websites
.. slug: quick-easy-management-of-nikola-website
.. date: 2020-04-21 21:44:50 UTC-04:00
.. updated: 2020-04-29 15:35:42 UTC-04:00
.. tags: nikola, project, git, deploy
.. category: devncdulo
.. link:
.. description: Deploy Nikola code, and content with GitHub Pages.
.. type: text

I mentioned a bit about this in my last post -- trouble coming to terms with a
sensible way to maintain this website in a `Git` repository. Right under my
nose it looks like there is a great solution. Not sure how I missed this way
of handling it so easily. This guide was written with Nikola_ in mind, but it
can be applied to *any* project repo on your `GitHub` account.

.. TEASER_END

There exists a wonderful service to host a small website alongside your `GitHub`
user account, or an individual repo. `GitHub Pages`_. It provides a quick, and
simple way to serve static content via `GitHub` to web browsers. Free to get
started with, I have been enjoying my use of it over the past couple months or
so. As described on their page:

 | Websites for you and your projects.
 | Hosted directly from your GitHub repository. Just edit, push, and your changes are live.

Through this service is how I currently host this blog. Creating a repo named in
the format ``[github_username].github.io`` will get you started. From there, you
can either push some HTML, or Markdown files (other formats too, I'm sure)
directly to the master branch and try visiting your page at the URL the repo's
name forms. Such as ``https://ncdulo.github.io/``. You may need to wait a few
minutes after the push command for GitHub to update. For me, that's generally
no less than 5-10 minutes.

This service provides the *perfect* environment to upload your Nikola output to
enjoy some free hosting. Using the method below will guide you through a basic
**project-level** deploy. The output will live in the ``docs/`` folder of
the ``master`` branch, and a setting will be flipped so that `GitHub` knows
to serve your content from there. The default method pushes onto a ``gh-pages``
branch, which should *only* contain the files you intend to be served on your
pages. I am opting for the ``docs/`` method for simplicity at the cost of
slightly less straight-forward directory hierarchy.

Alright, how are we going to do this?
-------------------------------------
This guide does not cover the initial setup, or design of your project, website,
or the repo itself. I am assuming you already have a project repo on `GitHub`,
some content which should *not* be part of your web-content, and some content
which *should* be part of your web-content. The web-content should exist within
a folder named ``docs/``, on your ``master`` branch.

.. class:: alert alert-info

If you are using `Nikola`, I personally include the ``output/``,
``cache/`` and ``.doit.db`` files in the ``.gitignore`` file. This prevents
any of the variable data, or generated content from being included in the
repository. This makes testing new content a bit easier for me, as the repo
will still show up as clean. Once my content is ready to deploy, I replace the
updated files in ``docs/`` with the output from ``content/``. You can just as
easily update `Nikola`'s ``conf.py`` so that your output goes directly to the
``docs/`` folder, effectively eliminating the need to manually copy when ready
to deploy.

With the repository set up, and your web content living inside of ``docs/``,
make sure that you ``git push origin master`` so the content exists in your
`GitHub` repository as well. From here, we simply toggle a setting so that
`GitHub` knows which directory and branch to look for your content in.

1. From your `GitHub` project's repository, click on the `Settings` link in
   the menu bar just beneath the `Star`, `Fork` and `Watch` links.
2. In the main `Settings` page, scroll down until you reach the `GitHub Pages`
   section.
3. Select the `Source` drop-down menu and select the option which specifies
   `Master branch /docs folder`. Once selected, the change should automatically
   be saved.
4. Visit ``https://[GITHUB_USERNAME].github.io/[PROJECT_REPO_NAME]``

   - It may take a few minutes for recently pushed changes to appear.
5. Enjoy your new project page hosting!

The above steps are mostly borrowed from the `GitHub Pages documentation`_. If
you encounter a problem, or need additional information. The documentation
linked may be of some help. That documentation also covers several other topics,
such as using Jekyll to build your site from Markdown, or how to set up a
user-specific `Pages` site.

What about customizing Nikola too?
----------------------------------
There is a certain beauty to me when maintaining our modified Nikola_ base
alongside the final output content in the same repository. By using the
``docs/`` folder we also eliminate some need for tricks and slightly more
complicated setup, or management. To me, it is a lot simpler, and easy to
maintain like this. I can tweak `Nikola` to my hearts content, and keep the
actual `generator` part version controlled. Once I have a new post, or new
content that I want to make available from the `GitHub Pages` URL, I can
simply copy the contents of ``output/`` over to ``docs/`` and ``git push`` to
deploy online.

Personally, the way I have chosen to organize the local repo for my `Nikola`
site makes the attempt to include all resources within the repo. This means
any theme, plugins, extra files, and content sources living together. For me,
this keeps it a bit easier to maintain. Of course, the best way to handle your
organization is whatever works best for *you* and your team.

Can I deploy *only* the content?
--------------------------------
Of course! That is exactly how I have been handling the initial deploy of
``/dev/ncdulo``. I had a pre-existing `GitHub Pages` site that fell into a lack
of maintenance as I realized over time the work to keep things working properly.
If I wanted to update the HTML, or navigation links -- I had to go through each
file individually to update everything. Not so bad when the site is just a
handful of pages. Turns into a job of it's own if you want to maintain anything
like a blog. That reason primarily is what got me started searching out a better
solution. And here we are, having loads of fun with this setup.

To deploy only the content to a `Pages` site, such as with a user-level site,
you will need to create a `GitHub` repo with a name following the format of
``[GITHUB_USERNAME.github.io``. With your new repo in hand, go ahead and
``git clone`` the new repo onto your computer. You may also want to include a
``README.md`` file so that any visitors to the actual `GitHub` repo know a bit
about the site. Otherwise from here, simply copy the ``output/`` directory from
`Nikola` into the root directory of the new repo you just created. Personally,
I use the following command to copy my content over.

.. class:: alert alert-warning

  Notice the ``/.`` directly after the ``output`` named here. This will tell
  ``cp`` to copy the files from *within* ``output`` rather than copying the
  ``output`` directory itself. This is probably what you want to do.
  This command effectively reads as copy the files *inside* of ``output`` into
  the ``ncdulo.github.io`` directory located one level above our current working
  directory. And copy them recursively so that all sub-directories and files
  within are copied.

.. code:: bash

  cp -r output/. ../ncdulo.github.io

Once the files are copied over, I suggest double-checking the directory
structure. Make sure pages are where they should be in your new repo, and that
things look to be in a working order. And simply ``git commit`` and ``git push``
the master branch up to `GitHub` once you are ready to deploy. Simply visit
``https://[GITHUB_USERNAME].github.io`` and check it out! It may take 5-10
minutes after your push before the page appears online. The full documentation
for creating a `GitHub Pages` site using this method can be found at
`GitHub Pages Help`_

.. class:: alert alert-warning

  If any of the files within your content include an underscore as their first
  character, `Jekyll` may decide they are special files and not publish them.
  To avoid this behavior, create a file named ``.nojekyll`` inside the root
  directory of the repo and commit to ``master``. If Jekyll sees this file, it
  does not parse your repo and should not hide these "special files".

Closing remarks
---------------
This is intended as a temporary solution for me. At least until I can do a final
bit of thinking, and create a standalone project repo for this site. In the end,
I plan to host this content on a domain using a proper web host. There are
things I would like to try out that `GitHub Pages` cannot do. Such as dynamic
content, and expanding my `Full Stack` related knowledge through managing a
VPS. The `Nikola` portion of ``/dev/ncdulo`` will live in a GitHub repository,
but the content will be hosted separately.

In all, this has been a lot of fun as I settle in to a new work flow. I'm not
sure the methods detailed above will be the best for any, or every situation.
This is simply to share my thought processes, and possibly help other's who may
find themselves in a similar situation. If there are any issues found with this
post, please inform me via `GitHub` `@ncdulo`_.

.. _GitHub Pages: https://pages.github.com
.. _Nikola: https://getnikola.com
.. _`GitHub Pages documentation`: https://help.github.com/en/github/working-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-your-github-pages-site-from-a-docs-folder-on-your-master-branch
.. _`GitHub Pages help`: https://help.github.com/en/github/working-with-github-pages/creating-a-github-pages-site
.. _`@ncdulo`: https://github.com/ncdulo
