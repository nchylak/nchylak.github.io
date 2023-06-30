---
layout: default
title: AWS infrastructure as code
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/20aws-infrastructure-as-code/
nav_order: 20
---

# AWS infrastructure as code

There are different ways of coding infrasturcture:

* AWS command line interface (CLI)
* AWS software development kits (SDK): manipulate AWS objects using the programming language of your choice
* CloudFormation: deploy entire "stacks" in one go. The advantage is that either the stack is deployed entirely successfully or nothing is deployed. Similarly when tearing down the stack, you are sure all ressources are removed properly.

## Cloudformation

CloudFormation is an AWS service for cloud provisioning with infrastructure as code (IaC).

The desired resources and their dependencies are described  with code in a `.yaml` or `.json` file (called **Template** in AWS jargon) and AWS CloudFormation provisions and configures the resources specified.

A **Stack** is a group of Templates, which are deployed at the same time.

**Changesets** are the diffs between the prior deployment and the one you are attempting.

Benefits:

* Review: All changes and configurations are saved and can be reviewed by others
* Repeat: All changes and configurations can be replicated to other regions/accounts, etc.
* Revert: All changes and configurations can be undone
* Roll-back: If at any point in time the deployment fails, changes made in the script are rolled back
* Integration with CI Pipeline: With a GitHub hook, deployment is triggered by changes to the Templates with CodePipeline (another AWS service)

Cons:

* Beware of drift: There can be differences between how your account is supposed to be setup (the sum of all your templates) and how your account is actually setup (the sum of all your templates and anything done manually from the console) - It is important that ALL changes are made with code!

## Cloud development kit (CDK)

CDK allows you to compile CloudFormation Templates using the programming language of your choice (e.g. Python).

#### Upgrade `aws-cdk`

```bash
sudo npm install -g aws-cdk@latest
```
