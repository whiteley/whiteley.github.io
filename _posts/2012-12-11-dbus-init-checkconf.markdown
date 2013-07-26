---
layout: post
title: Checking Upstart configuration syntax
categories: scripts
tags: ubuntu upstart dbus
---

There is a handy tool `init-checkconf` you can use to validate your freshly
written Upstart configs. If you're trying to use this on an Ubuntu server
install or similar environment, you might have run into this issue:

{% gist 6073365 %}

The problem is that `init-checkconf` is assuming you have a `dbus-daemon`
running and the related environment variables set to find it. A graphical login
session generally will start a dbus session for the user, but on a server
install you are most likely running without one.

I wrote a quick [script][gist] for a coworker that wraps `init-checkconf` with
a temporary dbus session. It might come in handy for someone else as well.

{% gist 4256487 %}

[gist]: https://gist.github.com/4256487
