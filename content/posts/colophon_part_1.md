+++
title = "Colophon (Part 1)"
author = ["Carl Flippin"]
date = 2021-09-06
tags = ["meta", "hugo"]
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
parts here are picking a theme and configuring it. I ended up writing my own
called [olympus](<https://github.com/carlf/hugo-theme-olympus>) There are many
available. Take a look at [Hugo Themes](<https://themes.gohugo.io/>) for more
choices.

My config ended up looking like this:

{{< highlight yaml >}}
baseURL: https://carlf.io
languageCode: en-us
title: carlf.io
theme: github.com/carlf/hugo-theme-olympus

markup:
  highlight:
    guessSyntax: true
    style: nord

params:
  author: Carl Flippin

menu:
  main:
    - identifier: categories
      name: Categories
      url: /categories
      weight: 2
    - identifier: tags
      name: Tags
      url: /tags
      weight: 3

module:
  imports:
  - path: github.com/carlf/hugo-theme-olympus
{{< /highlight >}}

Note that my theme is referenced by github URL rather than a simple name. This
is enabled by the `hugo mod init` above.

In the [next part]({{< relref "colophon_part_2" >}}) of this series, we will go over how to manage the posts
themselves and how `ox-hugo` factors into all this.
