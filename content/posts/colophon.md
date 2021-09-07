+++
title = "Colophon"
author = ["Carl Flippin"]
date = 2021-09-06
tags = ["meta", "org", "hugo"]
categories = ["meta"]
draft = false
+++

This blog is managed with a monolithic org file representing all posts and Hugo
managing the generation of the HTML. Some people just manage their blogs using
org directly but using Hugo as an intermediary makes some things like theming
easier and allows me to continue to work in org for my blog. This post goes over
the major moving parts.


## Hugo {#hugo}


### Installation {#installation}

If you are on a Mac, installation is very simple. All it requires is a `brew
install hugo`. On linux, your package manager may be of use but some distros
like Ubuntu tend to offer fairly vintage versions. For that reason, it may be
simpler to just download the static binary and put it somewhere in your path.
You can verify Hugo is installed correctly with `hugo version`


### Initial Site Generation {#initial-site-generation}

Once Hugo is installed, you want to generate a skeleton site. In my case this
was as simple as `hugo new site carlf.github.io -f yml`. This generates a new
empty skeleton the `-f yml` tells the generator that I would like to use yaml
for my config. Other options include json and toml. I'm fairly comfortable with
yaml so I went with that but they should be equivalent in practice.


### Initialize go.mod {#initialize-go-dot-mod}

In older versions of Hugo, the common practice was to check out things like
themes as submodules. This is not always ideal as submodules make initial
checkout trickier and removing them is a hassle. See
[here](<https://www.hugofordevelopers.com/articles/master-hugo-modules-managing-themes-as-modules/>)
for more details. To initialize your website as a module, run the following:

{{< highlight shell >}}
hugo mod init github.com/carlf/carlf.github.io
{{< /highlight >}}


### Configure Hugo {#configure-hugo}

Now we need to configure Hugo so we get some decent looking output. The major
parts here are picking a theme and configuring it. Here I have chosen the
[m10c](<https://github.com/vaga/hugo-theme-m10c>) theme. There are many available.
Take a look at [Hugo Themes](<https://themes.gohugo.io/>) for more choices.

My config ended up looking like this:

{{< highlight yaml >}}
# config.yml
baseURL: https://carlf.io
languageCode: en-us
title: carlf.io
theme: github.com/vaga/hugo-theme-m10c

markup:
  highlight:
    guessSyntax: true
    style: nord
  tableOfContents:
    endLevel: 3
    ordered: false
    startLevel: 2

params:
  author: Carl Flippin
  description: I'm just as confused as the rest of you
  avatar: avatar.png
  social:
    - icon: github
      name: "My Github"
      url: https://github.com/carlf
    - icon: key
      name: "My GPG Key"
      url: https://keyoxide.org/hkp/50f1a8d452b5c094a656ce6aac084c5eec19856d
  style:
    darkestColor: "#2e3440"
    darkColor: "#3b4252"
    lightColor: "#d8dee9"
    lightestColor: "#eceff4"
    primaryColor: "#8fbcbb"

menu:
  main:
    - identifier: home
      name: Home
      url: /
    - identifier: tags
      name: Tags
      url: /tags
    - identifier: rss
      name: RSS
      url: /index.xml
{{< /highlight >}}

Note that my theme is referenced by github URL rather than a simple name. This is enabled by the `hugo mod init` above.


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
markdown files for Hugo to process. There's some metadata for setting things
like categories and tags that come in handy. Let's look at the beginning of that
file for an example:

{{< highlight org >}}
#+TITLE: Posts
#+HUGO_BASE_DIR: ~/repos/carlf.github.io
#+HUGO_SECTION: posts
#+HUGO_CODE_FENCE: nil

* Meta :@meta:
** Colophon :meta:org:hugo:
:PROPERTIES:
:EXPORT_FILE_NAME: colophon
:EXPORT_DATE: 2021-09-06
:END:
{{< /highlight >}}
