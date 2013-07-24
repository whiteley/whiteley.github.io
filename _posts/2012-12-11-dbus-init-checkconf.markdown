---
layout: post
title: Checking Upstart configuration syntax
categories: ubuntu upstart
---

There is a handy tool `init-checkconf` you can use to validate your freshly
written Upstart configs. If you're trying to use this on an Ubuntu server
install or similar environment, you might have run into this issue:

{% highlight console %}
# /bin/init-checkconf -d /etc/init/procps.conf
DEBUG: upstart_path=/sbin/init
DEBUG: initctl_path=/sbin/initctl
DEBUG: confdir=/tmp/init-checkconf.lNM8R1C04a
DEBUG: file=/etc/init/procps.conf
DEBUG: job=procps
DEBUG: ok - no other running instances detected
DEBUG: upstart_out=/tmp/init-checkconf-upstart-output.x4d3nUnxKn
DEBUG: upstart_cmd=/sbin/init --session --no-sessions --no-startup-event --verbose --confdir /tmp/init-checkconf.lNM8R1C04a
DEBUG: Waiting for Upstart to reply over D-Bus (attempt 1)
DEBUG: Waiting for Upstart to reply over D-Bus (attempt 2)
DEBUG: Waiting for Upstart to reply over D-Bus (attempt 3)
DEBUG: Waiting for Upstart to reply over D-Bus (attempt 4)
DEBUG: Waiting for Upstart to reply over D-Bus (attempt 5)
ERROR: failed to ask Upstart to check conf file
DEBUG: stopping secondary Upstart (running with PID 20402)
{% endhighlight %}

The problem is that `init-checkconf` is assuming you have a `dbus-daemon`
running and the related environment variables set to find it. A graphical login
session generally will start a dbus session for the user, but on a server
install you are most likely running without one.

I wrote a quick [script][gist] for a coworker that wraps `init-checkconf` with
a temporary dbus session. It might come in handy for someone else as well.

[gist]: https://gist.github.com/4256487
