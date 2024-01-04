+++
date = "2016-02-13T22:55:14+01:00"
title = "Adventures in User-Data"

+++

One of my first tasks at my current employer was to automate bootstrapping of our AWS EC2 instances: to get them in a state where Chef could run on them without having to do any manual fiddling after launch.

To achieve this two things were needed, both requiring a lot of trial and error: an [IAM instance profile](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) to let EC2 instances access other AWS resources without having to distribute API keys, and a [User-Data script](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html#user-data-shell-scripts), which is a shell script that is run automatically on boot to set up. This is part of [Cloud-init](http://cloudinit.readthedocs.org/en/latest/index.html), a standard for early initialisation of cloud instances.

<!--more-->

Generally the less you do in these scripts the better, because they are a royal pain to debug. (Hint: it involves creating and destroying a lot of test instances.) These days most of our set-up has been farmed out to [Packer](https://www.packer.io/), which can build a custom AMI server image based on a stock Linux image on which it runs our set-up commands, install the versions of Ruby and Chef we want, and so on. This means the instances boot faster and that the User-Data script has less things to do.

Our custom AMIs include tools like the [jq](https://stedolan.github.io/jq/) command-line JSON processor, which we can use to process the output of [goFog](https://github.com/hungryblank/gofog), Contentful CTO Paolo's Go tool for basic interactions with the AWS API. Currently it supports reading information about EC2 instances, including their tags, which we use for environment discovery, and also publishing to SNS topics. Also being a Go program it's distributed as a statically-linked binary which from a deployment and operations point of view is great.

All the files we need for bootstrapping Chef, and our own packaged apps, are stored on S3. So [s3cmd](http://s3tools.org/s3cmd) is another important piece. I've been looking for a Go-based replacement, and mulling adding S3 support to goFog - for bootstrapping at least we only need GetObject operations - but I haven't yet found one that works out of the box.

With this in mind we need an IAM role policy sort of like:

{{% gist elblivion 2b4cffc483285253197b %}}

## But instance bootstrapping can still fail. What then?

Even with custom AMIs we still depend on external services on boot. The User-Data script refreshes the APT cache, fetches files from S3, reads instance data from the EC2 API... all of these have been known to fail. Currently we run some Auto Scaling groups without attaching them to Elastic Load Balancers, so we have no health check that can fail a new instance that wasn't able to set up correctly.

After a recent near-miss incident where we could have ended up swapping all the app servers in one group with new ones that weren't able to set up correctly (we use termination policies that favour terminating older instances) I looked at ways of getting warned when a new instance failed to set up. I was about to find and add error checking to every bit of code that might make requests when I recalled recently making use of the Bash [`trap` builtin](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_12_02.html). On investigation it turned out that using `set -e`, which I knew to make a Bash script exit immediately on failure, can be caught by setting a `trap` - so this could be used to warn an operator:

{{% gist elblivion eb714b33549f002da05b %}}

`sns_region` and `sns_topic` are template variables, since this User-Data script is templated by [Terraform](https://www.terraform.io/), but that's a subject for another post. :-)

And so - hopefully - we get paged and total disaster is averted!
