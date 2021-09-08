+++
title = "Colophon (Part 2)"
author = ["Carl Flippin"]
date = 2021-09-07
tags = ["hugo", "org", "meta"]
categories = ["meta"]
draft = false
+++

This is part 2 of the colophon for this site. For part 1 see [here]({{< relref "colophon_part_1" >}}). This
continuation will cover how to use ox-hugo to manage your content and publish it
from org-mode files.


## ox-hugo {#ox-hugo}

For generation of the markdown used in Hugo, we use
[ox-hugo](<https://ox-hugo.scripter.co/>). This module allows you to write your
blog posts in regular old org but to export them as markdown suitable for Hugo's
blackfriday markdown parser. It allows us to operate on org, which we are all
comfortable with, but still take advantage of the conventiences of Hugo.


### Installation {#installation}

I am using [doom emacs](<https://github.com/hlissner/doom-emacs>) so enabling
ox-hugo was as simple as:

{{< highlight emacs-lisp >}}
(doom! :lang
       (org +hugo))
{{< /highlight >}}

If you are rolling your own emacs, it is available
[here](<https://melpa.org/#/ox-hugo>) on MELPA as a package. If you are using the
popular [use-package](<https://github.com/jwiegley/use-package>) macro, you could
do the following:

{{< highlight emacs-lisp >}}
(use-package ox-hugo
  :ensure t
  :after ox)
{{< /highlight >}}


### Structure your posts {#structure-your-posts}

Posts are held in a monolithic org document that gets exported to individial
markdown files for Hugo to process. There are some handy properties you can set
to make things come out nice and neat and we'll go over them now.


#### Global Settings {#global-settings}

First let's look at the preamble for the whole file.

{{< highlight org >}}
#+HUGO_BASE_DIR: ~/repos/carlf.github.io
#+HUGO_SECTION: posts
#+HUGO_CODE_FENCE: nil
{{< /highlight >}}

Here we set some global options for the whole file. The `HUGO_BASE_DIR` setting
should be set to the root of your hugo repo. This is where all the markdown will
end up. The `HUGO_SECTION` should match the subdirectory that your posts would
end up in. For most hugo installations this should just be `posts`. If you have
done something tricky with your taxonomy, feel free to change that.

The `HUGO_CODE_FENCE` set to `nil` will make ox-hugo use the highlight
shortcode. If set to `t` ox-hugo will use the triple-backtick code fence.
Looking at the generated HTML, they seem to have the same effect but it worked
first when I had it set to `nil` so it has stayed `nil` for me. Note that both
of these modes use [Chroma](https://github.com/alecthomas/chroma) for their highlighting which supports most common
languages and has a ton of [themes](https://xyproto.github.io/splash/docs/) available.


#### Per-post Settings {#per-post-settings}

Each post also has a number of settings. Let's look at the start of a post.

{{< highlight org >}}
** Colophon (Part 1) :meta:hugo:
:PROPERTIES:
:EXPORT_FILE_NAME: colophon_part_1
:EXPORT_DATE: 2021-09-06
:ID:       a35b08a0-d4a5-4b60-967d-dbfcf68490ea
:END:
{{< /highlight >}}

Note here that `EXPORT_FILE_NAME` is the name for the markdown file ox-hugo will
generate. If you do not have an `EXPORT_FILE_NAME`, the file will not be
exported.

The `EXPORT_DATE` will be used for the published date in hugo. The `EXPORT_DATE`
field is not strictly necessary. If you make your top-level headings TODO items,
the timestamp for `CLOSED` will be used for the published data. If the heading
is in a TODO status, it will be exported as a draft. If it is in a DONE state,
it will be exported with draft set to false.

The `ID` property is handy if you ever need to link between pages. The link to
the first part of this series was created by a link to the id of that heading.
The tags for the post are the tags on the top-level heading for that post. In
this case we have the `meta` and `hugo` tags. Those will easily populate your
taxonomy. Also note that the category at the top of the tree will be inherited
by all posts below it.


### Exporting {#exporting}

Now that we have our posts in a nice neat bundle or org, we need to export them
so Hugo can process them and generate HTML. With standard keybindings, this is
as simple as `C-c C-e H A`. This will take all of our posts in the current file
and export them as individual markdown files ready for processing by hugo. Once
this is done, fire up hugo with `hugo serve` and take a look at [localhost:1313](http://localhost:1313)
to witness the fruits of your labors.
