---
layout: post
title:  "Packer Definition for Ubuntu Raring on VMware Fusion"
categories: virtualization
tags: packer vmware-fusion ubuntu
---

I needed a virtual machine on my laptop to use with [Oz][oz] for offline
testing. Since [Oz][oz] uses *kvm* to install the guest image, I would need
nested hypervisor support. Virtualbox doesn't provide this, but VMware Fusion
does.

Another requirement for any development testing cycle using virtual machines is
[Vagrant][vagrant] and so the first thing I needed was the [Vagrant VMware
Provider][vagrant-vmware]. Step 2 was setting up a base box with the required
environment.

[Hashicorp][hashicorp], the people behind vagrant, have a new product out
called [Packer][packer] that I used to create the base box. [Packer][packer]
could actually be used to replace [Oz][oz] in a lot of situations, but this is
for a larger project that relies on a number of [Oz][oz] specifics. You can
grab my [packer-raring-fusion][packer-raring-fusion] definition which sets up
a small image with:

* Installs VMware tools with some needed patches for 3.8 kernels
* Installs Vagrant insecure public key
* Downloads Packer and unzips in bin/
* Enables VHV for nested hypervisors on the guest
* Sets Vagrant User password to 'packer'

or the [packaged base box][packer-raring-fusion-box] (650MB).

[oz]: https://github.com/clalancette/oz/wiki
[vagrant]: http://www.vagrantup.com/
[vagrant-vmware]: http://www.vagrantup.com/vmware
[hashicorp]: http://www.hashicorp.com/
[packer]: http://www.packer.io/
[packer-raring-fusion]: https://github.com/whiteley/packer-raring-fusion
[packer-raring-fusion-box]: https://mwhiteley-vagrant-boxes.s3.amazonaws.com/packer_vmware_vmware.box
