---
layout: post
title: "Travis Makes a Better Launchpad"
published: true
---

At [work][ey] we have a love/hate relationship with Launchpad ... we can turn Debian source packages into APT repositories but only if we jump through the hoops just right. Discussing the many limitations of Launchpad is a black hole, so I just want to share an [experiment][hello] I completed recently. Most Debian packaging tutorials start with the [GNU Hello][gnu-hello] program, so I used that for consistency.

I first started with a list of requirements:

1. Store packaging files in git
1. Build packages for multiple Ubuntu releases
1. Create APT repositories

We use (and love) [Travis CI][travis] a ton and it seemed like the perfect place to iterate on packages. With the typical build environment already available, the only extra tools I used were:

1. [`deb-s3`][deb-s3] for creating and managing APT repositories in the cloud without rsyncing local copies of everything
1. [`pbuilder`][pbuilder] to isolate the package building environment

The contents of the [`hello/debian`][hello-debian] directory will be straightforward to anyone familiar with Debian packaging and all of the interesting bits are contained in the [`.travis.yml`][hello-travis-yml] file.

The resulting repository is hosted on S3 and can be added to your system with the following APT source:

```
deb https://s3.amazonaws.com/s3pa-mwhiteley-hello precise main
```

Next steps:

1. Use [`sbuild`][sbuild] or similar to solve the multiple distributions issue
1. Inject the APT repository into the build sources so that dependent packages can be created
1. [Sign][secure-apt] the packages and repository catalogs
1. Create a community index for GitHub repositories and S3 buckets

[deb-s3]: https://github.com/krobertson/deb-s3
[ey]: https://www.engineyard.com/
[gnu-hello]: http://www.gnu.org/software/hello/
[hello-debian]: https://github.com/whiteley/hello/tree/master/hello/debian
[hello-travis-yml]: https://github.com/whiteley/hello/blob/master/.travis.yml
[hello]: https://github.com/whiteley/hello
[pbuilder]: http://pbuilder.alioth.debian.org/
[sbuild]: https://wiki.debian.org/sbuild
[travis]: https://travis-ci.org/
[secure-apt]: https://wiki.debian.org/SecureApt
