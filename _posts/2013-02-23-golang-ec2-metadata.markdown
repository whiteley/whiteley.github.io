---
layout: post
title: Crawl EC2 meta-data and output JSON
categories: virtualization
tags: ubuntu ec2 golang
---

Last week, while debugging an issue with some EC2 instances at work, I wanted
to quickly analyze and compare some of the instance meta-data. Our typical
interaction with this is in Chef as learned by Ohai, but I wanted something
standalone. I keep wanting to play with [Go][golang] more, and this seemed like
a fun Sunday morning hack (translation: homework avoidance).

{% gist 4751059 %}

{% highlight console %}
# setup Go
wget -P /tmp http://go.googlecode.com/files/go1.0.3.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf /tmp/go1.0.3.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin

# grab the source file
wget https://gist.github.com/whiteley/4751059/raw/md.go

# print meta-data
go run md.go
{"ami-id":"ami-aa941e9a","ami-launch-index":"0","ami-manifest-path":"ubuntu-us-west-2/images/ubuntu-precise-12.04-amd64-server-20130204.manifest.xml","block-device-mapping":{"ami":"sda1","ephemeral0":"sda2","root":"/dev/sda1","swap":"sda3"}, …

# pretty print meta-data
go run md.go --prettyprint
{
  "ami-id": "ami-aa941e9a",
  "ami-launch-index": "0",
  "ami-manifest-path": "ubuntu-us-west-2/images/ubuntu-precise-12.04-amd64-server-20130204.manifest.xml",
  "block-device-mapping": {
    "ami": "sda1",
    "ephemeral0": "sda2",
    "root": "/dev/sda1",
    "swap": "sda3"
  },
  …
}
{% endhighlight %}

[Go][golang] has a number of exciting features, but the most interesting in this
experiment is possibly the compilation to an executable binary. I can now
quickly grab this file on a running instance and inspect the meta-data easily
without installing [Go][golang] or anything else.

{% highlight console %}
# grab the binary
wget http://go-mwhiteley.s3.amazonaws.com/md && chmod 0755 md
# run
./md
{% endhighlight %}

Next up, I plan to investigate [Rust][rust].

[golang]: http://golang.org/
[rust]: http://www.rust-lang.org/
