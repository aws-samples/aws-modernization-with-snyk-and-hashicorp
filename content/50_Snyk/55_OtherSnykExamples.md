---
title: "Optional Example"
chapter: true
weight: 55
---

# Extra Credit
Our primary workshop experience focuses on an EC2 instance that has easy-to-observe issues with fast turnaround.  In this section, we provide alternative examples with more details, but take longer to deploy than is practical for the workshop.

We will follow the same steps as before of cloning the repository and running snyk iac.  We will also show additional information avabile when we run terraform plan, and how it provides greater context for the `snyk iac` run.

## Clone the Repository

We will follow the same checkout steps as before, starting from the top of your home directory.  Notice we'll navigate into a subdirectory of the entire repository.  This repository contains multiple examples of Infrastructure As Code, including the Amazon EKS example Snyk has provided in an AWS Live Hack series.
Go to your home directory on your terminal (e.g. if on AWS event using VS Code Server use `cd /Workshop`) and execute the commmands below.

```bash
git clone https://github.com/snyk-labs/terraform-goof.git
cd terraform-goof
```

This repository is a deliberately vulnerable example, and you should not run it in a production envioronment.  Instead, we encourage you to study it.  We include an output plan file for convenience and this next command removes it from your scans:

```bash
mv tf-plan.json tf-plan.json.original
```

## Run `snyk iac test` without context

As before, we'll change into the directory and collect our first round of results using the Snyk cli.  The example below contains a subset of the results.

```bash
snyk iac test

Snyk Infrastructure as Code

✔ Test completed.

Issues

Low Severity Issues: 8

*****MULTIPLE LINES DELETED*****

Test Summary

  Organization: hashicorp
  Project name: snyk-labs/terraform-goof

✔ Files without issues: 8
✗ Files with issues: 4
  Ignored issues: 0
  Total issues: 23 [ 0 critical, 5 high, 10 medium, 8 low ]
```

Note the number of issues, which is a total of 23 with 5 high at the time of this writing.

## Run `snyk iac test` with dedicated terraform plan option

If using an AWS Workshop, open the `variables.tf` file and change the region and ami-id to the following:

```bash
variable "region" {
  type    = string
  default = "us-west-2"
}

variable "ami" {
  type    = string
  description = "ami used for ec2 instance. example - ami-07336266b2c69c546 (terraform-goof-example-ami)"
  default = "ami-07336266b2c69c546"
}
```

Run these commands are to initialize your terraform environment and run a plan.  In the example below, we'll pass along your AWS keys as command-line variables.  Use values specific to you, and if on an AWS Workshop event you'll also need to add `-var="session_token=YOUR_SESSION_TOKEN"` to the terraform plan command:

```bash
terraform init
terraform plan -var="access_key=YOUR_ACCESS_KEY" -var="secret_key=YOUR_SECRET_KEY"
```
when you are prompted to copy your workspace to Terraform Cloud, answer no.

Now we're ready to run Snyk with a [terraform dedicated scan mode](https://docs.snyk.io/snyk-cli/commands/iac-test#scan-less-than-terraform_plan_scan_mode-greater-than).  This scan takes into account the final full state.

```bash
snyk iac test --scan=planned-values

Snyk Infrastructure as Code

✔ Test completed.

*****MULTIPLE LINES DELETED*****


Test Summary

  Organization: hashicorp
  Project name: snyk-labs/terraform-goof

✔ Files without issues: 20
✗ Files with issues: 6
  Ignored issues: 0
  Total issues: 32 [ 0 critical, 5 high, 14 medium, 13 low ]
  
```

At the time of this writing, this new run now contains 32 issues, including 5 high.  If we compare the diferences in the new high severity values, we'll see the following:


```bash
 [High] Credentials are configured via provider attributes
  Info:    Credentials are configured via provider attributes. Use of provider
           attributes can lead to accidental disclosure of credentials in
           configuration files, variable definition files, event logs or console
           logs
  Rule:    https://security.snyk.io/rules/cloud/SNYK-CC-TF-74
  Path:    provider[aws][0]
  File:    main.tf
  Resolve: Set access credentials via environment variables, and remove
           `access_key` and `secret_key` attributes from the configuration

  [High] Wildcard action in IAM Policy
  Info:    The IAM policy allows all actions on resource. Granting permission to
           perform any action is against 'least privilege' principle
  Rule:    https://security.snyk.io/rules/cloud/SNYK-CC-TF-69
  Path:    aws_iam_policy_document[admin-assume-role-policy] > statement
  File:    modules/iam/main.tf
  Resolve: Set `statement.action` attribute to specific actions e.g.
           `s3:ListBucket`

  [High] S3 block public ACLs control is disabled
  Info:    Bucket does not prevent creation of public ACLs. Anyone who can
           manage bucket's ACLs will be able to grant public access to the
           bucket
  Rule:    https://security.snyk.io/rules/cloud/SNYK-CC-TF-95
  Path:    resource > aws_s3_bucket[my-new-undeployed-bucket]
  File:    modules/storage/main.tf
  Resolve: Set the `aws_s3_bucket_public_access_block` `block_public_acls` field
           to `true` or use the default setting

  [High] S3 restrict public bucket control is disabled
  Info:    Bucket will recognize public policies and allow public access. If
           public policy is attached to the bucket, anyone will be able to read
           and/or write to the bucket.
  Rule:    https://security.snyk.io/rules/cloud/SNYK-CC-TF-98
  Path:    resource > aws_s3_bucket_public_access_block[snyk_public] >
           restrict_public_buckets
  File:    modules/storage/main.tf
  Resolve: Set `aws_s3_bucket_public_access_block` `restrict_public_buckets`
           attribute to `true`, or use default

  [High] S3 restrict public bucket control is disabled
  Info:    Bucket will recognize public policies and allow public access. If
           public policy is attached to the bucket, anyone will be able to read
           and/or write to the bucket.
  Rule:    https://security.snyk.io/rules/cloud/SNYK-CC-TF-98
  Path:    resource > aws_s3_bucket_public_access_block[snyk_private] >
           restrict_public_buckets
  File:    modules/storage/main.tf
  Resolve: Set `aws_s3_bucket_public_access_block` `restrict_public_buckets`
           attribute to `true`, or use default
```

The primary reason for this new context is that we've resolved submodules that include additional configuration details not seen because we were not utilizing planned values.  We are utilizing the contents of the new file, `tf-plan.json` in our scans which include greater context.

## Modifying Policies

## Custom ignore rules

## One more example

Another example of a vulnerable IaC project is illustrated below.  This is a popular repository for an Amazon EKS definition.  As a deliberately vulnerable repository, do not run this in a production environment and instead study it for its contents.

```bash
cd ..
git clone https://github.com/adversarialengineering/eks-demo-deployments.git
cd eks-demo-deployments/terraform/default
```

The primary different here is that the initial snyk iac test does not reveal any issues until you perform a plan to gain context.

```bash
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

Update variables.tf to specify the region in your aws provider block named `variable "region"`.
We'll need to configure our AWS Access Key ID plus AWS Secret Access Key on Terraform Cloud.

```bash
terraform init
terraform plan
snyk iac test

*****MULTIPLE LINES DELETED*****

Test Summary

  Organization: hashicorp
  Project name: default

✔ Files without issues: 110
✗ Files with issues: 6
  Ignored issues: 0
  Total issues: 16 [ 0 critical, 1 high, 3 medium, 12 low ]
```

## Next: HashiCorp Terraform Cloud
In the next module, we'll review results with Terraform Cloud.