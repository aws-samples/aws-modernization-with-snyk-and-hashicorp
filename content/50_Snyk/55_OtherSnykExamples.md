---
title: "Optional Example"
chapter: true
weight: 55
---

# Extra Credit
Our primary workshop experience focuses on an EC2 instance and easy-to-observe issues.  We see results fast, and the examples are easy for us to see and understand for the purposes of the workshop.

A real-world example is more sophisticated and complex.  THis optional module covers the analysis of Amazon EKS infrastructure that is deliberately vulnerable.

We will follow the same steps as before of cloning the repository and running snyk iac.  We will also show additional information avabile when we run terraform plan, and how it provides greater context for the `snyk iac` run.

## Clone the Repository

We will follow the same checkout steps as before, starting from the top of your home directory.  Notice we'll navigate into a subdirectory of the entire repository.  This repository contains multiple examples of Infrastructure As Code, including the Amazon EKS example Snyk has provided in an AWS Live Hack series.

```bash
cd ~/
git clone https://github.com/adversarialengineering/eks-demo-deployments.git
cd eks-demo-deployments/terraform/default
```

## Run `snyk iac test` without context

Change into the working directory and run snyk iac to get the first set of results.  The example below contains the entire output.

```bash

 snyk iac test

Snyk Infrastructure as Code

✔ Test completed.

Issues
  No vulnerable paths were found!

-------------------------------------------------------

Test Summary

  Organization: hashicorp
  Project name: default

✔ Files without issues: 7
✗ Files with issues: 0
  Ignored issues: 0
  Total issues: 0 [ 0 critical, 0 high, 0 medium, 0 low ]

-------------------------------------------------------

Tip

  New: Share your test results in the Snyk Web UI with the option --report
```

The lack of results may lead you to believe the project is in great shape and there isn't any more to do.  Let's now add context from the terraform apply command to see better results.

## Run `terraform plan`

Initialize your terraform environment and run the plan.  When you run your plan, you'll be asked for the name of your cluster.  Provide a name suitable for your testing, such as `hashi-snyk-workshop`.  In the example below, we show the first several lines to include the prompt.  There are hundreds of lines of code we won't show in this output, reflecting the many different bits of infrastructure we are creating.

```bash
terraform init
terraform plan

var.cluster_name
  Enter a value: hashi-snyk-workshop

*****MULTIPLE LINES DELETED*****

```

Next, we'll run snyk iac to get more details.

## Run `snyk iac`, with context

We now have terraform context and can run the command as shown below with output context.

```bash
snyk iac test

*****MULTIPLE LINES DELETED*****

High Severity Issues: 1

  [High] EKS cluster allows public access
  Info:    API endpoint of the EKS cluster is public. Anyone may be able to establish network connectivity to the API server
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-94
  Path:    resource > aws_eks_cluster[this] > vpc_config[0]
  File:    .terraform/modules/cluster/main.tf
  Resolve: Set `vpc_config.public_access_cidrs` attribute to specific net address e.g. `192.168.0.0/24`, or set `vpc_config.endpoint_public_access` attribute to `false`

-------------------------------------------------------

Test Summary

  Organization: hashicorp
  Project name: default

✔ Files without issues: 181
✗ Files with issues: 7
  Ignored issues: 0
  Total issues: 15 [ 0 critical, 1 high, 3 medium, 11 low ]

-------------------------------------------------------
```

At this point, we now include many more files to find 1 High and several Medium and Low issues.  If we look closely at the High, we'll notice the issue is focused on the definition of the VPC.  Let's compare the results with a hardened example.

## Run `snyk iac test` in a hardended environment

Run this commands to navigate to the hardened example:

```bash
cd ../hardend
snyk iac test

Snyk Infrastructure as Code

✔ Test completed.

Issues

Medium Severity Issues: 1

  [Medium] Wildcard principal in KMS key access policy
  Info:    Wildcard principal has been specified in access policy. Using wild card will grant unnecessary access to any user in the account
  Rule:    https://snyk.io/security-rules/SNYK-CC-AWS-709
  Path:    resource > aws_kms_key[eks] > policy
  File:    cluster.tf
  Resolve: Set `Principal` attribute in the policy to specific entities, for example `arn:aws:iam::123456789012:user/JohnDoe`

-------------------------------------------------------

Test Summary

  Organization: hashicorp
  Project name: hardened

✔ Files without issues: 6
✗ Files with issues: 1
  Ignored issues: 0
  Total issues: 1 [ 0 critical, 0 high, 1 medium, 0 low ]
```

Now let's add some context as before.  In this case, we're naming the cluster "hsw-2" for the hasi-snyk-workshop (version 2).

```bash
terraform init
terraform plan
var.unique_suffix
  Enter a value: hsw-2

*****MULTIPLE LINES DELETED*****

snyk iac test
```




## TODO Notes

```bash
snyk iac test
snyk iac test

Snyk Infrastructure as Code

✔ Test completed.

Issues
  No vulnerable paths were found!

-------------------------------------------------------

Test Summary

  Organization: hashicorp
  Project name: private

✔ Files without issues: 7
✗ Files with issues: 0
  Ignored issues: 0
  Total issues: 0 [ 0 critical, 0 high, 0 medium, 0 low ]

-------------------------------------------------------
```

Update backend.tf to specify an organization (left blank for you to fill out.)
Also, update variables.tf to incude your initials, such as "mm."
Also, update to specify the region in your aws provider block.
We'll need to configure our AWS Access Key ID plus AWS Secret Access Key on Terraform Cloud.

```bash
terraform init
terraform plan
snyk iac test

*****MULTIPLE LINES DELETED*****

Test Summary

  Organization: hashicorp
  Project name: private

✔ Files without issues: 110
✗ Files with issues: 6
  Ignored issues: 0
  Total issues: 14 [ 0 critical, 1 high, 2 medium, 11 low ]
```


