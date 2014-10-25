---
layout: post
title: "Test Kitchen on Travis CI"
published: true
---

The world of [Chef][chef] cookbook development and testing has improved a ton in the last year or two. [We][ey] can be far more confident in our configuration quality since we run them via [Test Kitchen][test-kitchen] using our internal custom AMIs.

At this point, [Berkshelf][berkshelf] can't directly index cookbooks hosted in chef-server and a [Berkshelf API][berkshelf-api] server is required. We had need for multiple configurations during development and I wanted to save every engineer on the team from running their own [API server][berkshelf-api] locally.

We use [Travis CI][travis] to test our cookbooks and why not have the server running right where the information is needed. To simplify this [example][test-kitchen-berks-api], I replaced hosted chef in the [`config.json`][config.json] with the [Supermarket][supermarket] configuration but this would work with any of the supported endpoints.

1. `ENV['BERKSHELF_API_PATH']` is set to the root of the repository to override the default of `~/.berkshelf/api-server`
1. Travis encrypted environment variables are provided for `aws_access_key_id` and `aws_secret_access_key`
1. The SSH private key is added via `travis encrypt-file`
1. Cache for APT and Bundler are both enabled to speed up test runs
1. Package containing `libarchive.so` installed for `berks-api` dependency
1. AWS resource tags are applied to the EC2 instances to help identify them

After figuring out the Bundler cache hiccups, I ran into an [issue][test-kitchen-issue] that one of my [coworkers][souza] had reported earlier this year. As pointed out there, uploading compressed tarballs or switching to `sftp` may be the best course of action but for now [removing the empty directories][test-kitchen-pr] results in a **massive** performance improvement.

The trickiest part of this test was getting the IAM configuration correct, so I'll explain that in another post.

[berkshelf-api]: https://github.com/berkshelf/berkshelf-api
[berkshelf]: http://berkshelf.com/
[chef]: https://www.getchef.com/
[config.json]: https://github.com/whiteley/test-kitchen-berks-api/blob/master/config.json
[ey]: https://www.engineyard.com/
[souza]: http://ryansouza.net/
[supermarket]: https://supermarket.getchef.com/
[test-kitchen]: http://kitchen.ci/
[test-kitchen-berks-api]: https://github.com/whiteley/test-kitchen-berks-api
[test-kitchen-issue]: https://github.com/test-kitchen/test-kitchen/issues/429
[test-kitchen-pr]: https://github.com/test-kitchen/test-kitchen/pull/530
[travis]: https://travis-ci.org/
