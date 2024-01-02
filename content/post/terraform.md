+++
date = "2016-02-28T13:20:53+01:00"
draft = true
title = "Terraform: Open Source Cloud Orchestration"

+++


## What is Terraform

[Terraform](https://terraform.io/) is a tool for managing Infrastructure as Code, developed by Hashicorp, makers of other tools such as Vagrant and Packer.

<!--more-->

Terraform allows you to model computing infrastructure in a configuration language which provides some useful primitives such as creating sets of identical resources, and interpolating various values into resource attributes.

Terraform is an [open source project](https://github.com/hashicorp/terraform) written in Go, under heavy development by Hashicorp and very open to contributions and bug fixes - [here](https://github.com/hashicorp/terraform/pulls?utf8=%E2%9C%93&q=is%3Apr+is%3Aclosed+author%3Aelblivion) are my own small contributions to the project, mostly bug fixes and small tweaks I have needed as I have converted my current employer's infrastructure to be managed by this tool.

Before switching to Terraform I was writing an in-house custom tool that took YAML config files and generated [AWS CloudFormation](https://aws.amazon.com/cloudformation/) templates. These templates are then sent to AWS CloudFormation APIs, which attempt to apply them - creating new infrastructure or modifying existing one.

CloudFormation templates are written in JSON and quickly become huge - one of the reasons I wrote an in-house tool, and why tools such as [SparkleFormation](http://www.sparkleformation.io/) and [troposphere](https://github.com/cloudtools/troposphere) exist, providing DSLs to make template creation easier. In contrast, Terraform uses a more concise, powerful and flexible configuration language, HCL - although you can also write HCL-compatible JSON, which is the best bet for machine-generated Terraform config ([here's](https://gist.github.com/elblivion/f52c38f720b8a200a04c) an example Ruby script that generates a Terraform config for looking up the current CoreOS stable channel AMI for any AWS region).


## Our Terraform module and file organisation

## Gotchas

## Resources

* Terraforming
