---
layout: post
title:  "My Adventures with Jekyll"
date:   2013-07-23 13:45:32
categories: jekyll update
---

I've decided to give Jekyll a try for posting some of my snippets of code,
thoughts, and other work. This post is just here to get me started.

A random trick I pulled off today, that I'm sure to forget:

{% highlight bash %}
$ brew list emacs | xargs \
    pax -w -z -s ,$(brew --prefix),, \
    -f ~/Downloads/emacs-$(stat -f '%Sc' -t '%Y%m%d')-brewed.tgz
{% endhighlight %}

This created an archive of the [Homebrew][homebrew] installed emacs package and
I could then try to install it fresh from the git repo but restore the working
version if it failed.

I'm sure that I will need this link to the [Jekyll docs][jekyll] in the near
future and maybe at some point, I'll even have a commit in [Jekyll's GitHub
repo][jekyll-gh].

[homebrew]:  http://mxcl.github.io/homebrew/
[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com
