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

```bash
cd ~/environment
git clone https://github.com/snyk-labs/terraform-goof.git
cd terraform-goof
```

This repository is a deliberately vulnerable example, and you should not run it in a production envioronment.  Instead, we encourage you to study it.  We include an output plan file for convenience and this next command removes it from your scans:

```bash
cd ~/environment/terraform-goof
mv tf-plan.json tf-plan.json.original
```

## Run `snyk iac test` without context

As before, we'll change into the directory and collect our first round of results.  The example below contains a subset of the results.

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
  Ignored issues: 3
  Total issues: 24 [ 0 critical, 6 high, 10 medium, 8 low ]
```

Note the number of issues, which is a total of 24 with 6 high at the time of this writing.

## Run `snyk iac test` with dedicated terraform plan option

Next, let's make two modification to the file `main.tf`.

The first is to omit the verison of the terraform provider.  In main.tf, comment out the line assigning the version to be "~> 3.0" by adding the pound "#" sign in front of the line.
The second change is to personalize the organization to match yours and name the workspace something with your initials:

```terraform
terraform {
  cloud {
    organization = "partner-snyk"

    workspaces {
      name = "terraform-goof-mam2"
    }
  }

  required_providers {
    aws = {
      source  = "hashicorp/aws"
#      version = "~> 3.0"
    }
  }
}
```

The next commands are to initialize your terraform environment and run a plan.  In the example below, we'll pass along your AWS keys as command-line variables.  Use values specific to you:

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

✔ Files without issues: 19
✗ Files with issues: 8
  Ignored issues: 6
  Total issues: 57 [ 0 critical, 12 high, 24 medium, 21 low ]
  
```

At the time of this writing, this new run now contains 57 issues, including 12 high.  Previously we had 24 total issues with 6 high.  If we compare the diferences in the 6 new high values, we'll see the following:


```bash
 [High] S3 block public ACLs control is disabled
  Info:    Bucket does not prevent creation of public ACLs. Anyone who can
           manage bucket's ACLs will be able to grant public access to the
           bucket
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-95
  Path:    resource > aws_s3_bucket[snyk_public_storage]
  File:    tf-plan.json
  Resolve: Set `block_public_acls` attribute to `true`

  [High] S3 block public ACLs control is disabled
  Info:    Bucket does not prevent creation of public ACLs. Anyone who can
           manage bucket's ACLs will be able to grant public access to the
           bucket
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-95
  Path:    resource > aws_s3_bucket[snyk_storage]
  File:    tf-plan.json
  Resolve: Set `block_public_acls` attribute to `true`

  [High] S3 block public policy control is disabled
  Info:    Bucket does not prevent creation of public policies. Anyone who can
           manage bucket's policies will be able to grant public access to the
           bucket.
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-96
  Path:    resource > aws_s3_bucket_public_access_block[snyk_public] >
           block_public_policy
  File:    tf-plan.json
  Resolve: Set `block_public_policy` attribute to `true`

  [High] S3 ignore public ACLs control is disabled
  Info:    Bucket will recognize public ACLs and allow public access. If public
           ACL is attached to the bucket, anyone will be able to read and/or
           write to the bucket.
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-97
  Path:    resource > aws_s3_bucket_public_access_block[snyk_public] >
           ignore_public_acls
  File:    tf-plan.json
  Resolve: Set `ignore_public_acls` attribute to `true`

  [High] S3 restrict public bucket control is disabled
  Info:    Bucket will recognize public policies and allow public access. If
           public policy is attached to the bucket, anyone will be able to read
           and/or write to the bucket.
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-98
  Path:    resource > aws_s3_bucket_public_access_block[snyk_public] >
           restrict_public_buckets
  File:    tf-plan.json
  Resolve: Set `restrict_public_buckets` attribute to `true`
```

The primary reason for this new context is that we've resolved submodules that include additional configuration details not seen because we were not utilizing planned values.  We are utilizing the contents of the new file, `tf-plan.json` in our scans which include greater context.

## Modifying Policies

## Custom ignore rules

## One more example

Another example of a vulnerable IaC project is illustrated below.  This is a popular repository for an Amazon EKS definition.  As a deliberately vulnerable repository, do not run this in a production environment and instead study it for its contents.

```bash
cd ~/environment
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

## Next: HashiCorp Terraform Cloud
In the next module, we'll review results with Terraform Cloud.