---
layout: post
title: Ubuntu Cloud AMI ID Finder
categories: virtualization
tags: ubuntu ec2
---

Recently, I've found myself loading up the [Ubuntu Cloud
Images][ubuntu-cloud-images] site way too frequently just to copy and paste an
up to date AMI id.

I hacked together a [script][gist] to grab the json source, filter it on some
provided attributes and output a table in your terminal. My buddy [JD][jd]
helped out with some refactoring and it's a much more pleasant way to find that
AMI id you need.

{% gist whiteley/9b473ad1a695cc521f6c %}

{% highlight console %}
$ ubuntu-cloud-images suite:precise region:us-west-2 type:ebs
+-----------+---------+-------+------+------------+--------------+---------------------------------------------+
| region    | suite   | arch  | type | date       | ami          | launch                                      |
+-----------+---------+-------+------+------------+--------------+---------------------------------------------+
| us-west-2 | precise | amd64 | ebs  | 20121026.1 | ami-7eab224e | ec2-run-instances -k mwhiteley ami-7eab224e |
| us-west-2 | precise | i386  | ebs  | 20121026.1 | ami-7cab224c | ec2-run-instances -k mwhiteley ami-7cab224c |
+-----------+---------+-------+------+------------+--------------+---------------------------------------------+
{% endhighlight %}

Next up, I'll post my script that I use to run instances that waits for ssh
access and drops you into a shell.

[ubuntu-cloud-images]: http://cloud-images.ubuntu.com/
[gist]: https://gist.github.com/9b473ad1a695cc521f6c
[jd]: https://github.com/jdhuntington
