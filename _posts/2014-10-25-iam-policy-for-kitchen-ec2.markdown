---
layout: post
title: "IAM Policy For Kitchen EC2"
published: true
---

[IAM][iam] policies can be tricky but that is certainly not a reason to use your AWS root account credentials all over the place. In the [test-kitchen-berks-api][test-kitchen-berks-api] example I created a new user and granted it permissions for the [kitchen-ec2][kitchen-ec2] tests. This requires being much more explicit than authorizing `ec2:RunInstances`. The user needs to be able to tag the running instances and to prevent complications we limit the instance termination permissions.

To create a working policy, you'll want to use the [iam-policy-generator][iam-policy-generator] and the [iam-policy-simulator][iam-policy-simulator]. The permissions required for this test are very well explained on the [AWS Security Blog][ec2-permissions].

{% gist whiteley/c1e2cd106b70c2243ce0 %}

1. `iam-usercreate -u test-kitchen-berks-api -k`
1. `iam-useruploadpolicy -f iam.json -p test-kitchen-berks-api-instances -u test-kitchen-berks-api`

It should be noted that the restrictions in this policy are not *secure* in that they allow the tagging of all resources in the account and do not limit tags to instances created by this user. To force a tag onto a newly booted instance it would need to be applied to the AMI.

[ec2-permissions]: http://blogs.aws.amazon.com/security/post/Tx2KPWZJJ4S26H6/Demystifying-EC2-Resource-Level-Permissions
[iam-policy-generator]: http://awspolicygen.s3.amazonaws.com/policygen.html
[iam-policy-simulator]: https://policysim.aws.amazon.com/
[iam]: http://aws.amazon.com/iam/
[kitchen-ec2]: https://github.com/test-kitchen/kitchen-ec2
[test-kitchen-berks-api]: https://github.com/whiteley/test-kitchen-berks-api
