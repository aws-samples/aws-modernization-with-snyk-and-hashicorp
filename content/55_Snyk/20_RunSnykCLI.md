---
title: "Run the Snyk CLI"
chapter: true
weight: 57
---

## Run Snyk IaC

At your command prompt, run this command at the base directory of your repository you checked out.

```bash
cd ~/eks-demo-deployments/terraform/default
snyk iac test
```

OR

```bash
cd ~/terraform-goof
snyk iac test
```



This process should only take a few seconds, and you will see many lines of output with a summary similar to what is shown below. The net result is no issues detected.  In the example below, our test Snyk Organization is hamed `hashicorp`.

```bash
$ snyk iac test

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

OR

```
🐧$ snyk iac test

Snyk Infrastructure as Code

✔ Test completed.

... many lines deleted

-------------------------------------------------------

Test Summary

  Organization: hashicorp
  Project name: snyk/terraform-goof

✔ Files without issues: 8
✗ Files with issues: 5
  Ignored issues: 4
  Total issues: 28 [ 0 critical, 7 high, 10 medium, 11 low ]

-------------------------------------------------------

Tip

  New: Share your test results in the Snyk Web UI with the option --report

```



## Review Results - with context

In the output above, we didn't find any issues and you may feel like you are in a pretty good spot.  While we appear to be off to a good start, we can do better once we initialize the enviornment with terraform and setup some context.  Run the following terraform commands and a new snyk iac test.


```bash
terraform init
terraform plan
snyk iac test
```

This time, you'll see output for the terraform init, and setup for the plan.  You'll be prompted for a cluster name and you can add a name specific to you.  For example, we'll include our name:

```bash
Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
var.cluster_name
  Enter a value: marco-eks
  
  ```
This completes the terraform plan operation and then we're onto the new snyk iac run.  You will much more detail this time, including this summary:

```bash
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

✔ Files without issues: 180
✗ Files with issues: 8
  Ignored issues: 0
  Total issues: 16 [ 0 critical, 1 high, 4 medium, 11 low ]

-------------------------------------------------------

Tip

  New: Share your test results in the Snyk Web UI with the option --report
```

When you examine the entire output, you'll see several pieces of useful information.

* Severity as Low, Medium, High, or Critical.  Here we see the severity of [Medium.]
* Title, Info, Path, File - details for you to locate the issue within your code, plus some context.
* Rule - A link to the publicly available description.
* Resolve - mitigation on how to address the issue


Let's fix the HIGH Severity issue included in the output above.  This is for an EKS cluster permitting public access.  The fix is to specify a narrow CIDR block, or to set a configuration value to false.  The definition of the EKS module is included when we do a terraform init operation and 

```
TODO: FIX FOR the HIGH vulnerability
What is the best way to fix the issue if we choose to set the endpoint_public_access to false?  Is it to define an entry like this in cluster.tf within the module block?

  cluster_endpoint_public_access = false

I may have to find out if snyk iac will detect the issue as fixed when we do this.  I believe the above line changes the definition, but snyk may not be picking it up.

```

Add that block and re-run the `snyk iac test` command to see one fewer issue.

```
snyk iac test

...

-------------------------------------------------------

Test Summary

TODO: Fill in with fewer results.

-------------------------------------------------------
```

In your environment, you will perform similar actions based on the feedback you see.  Here we show the CLI, and Snyk also works with IDEs such as IntelliJ.  You can also send results to snyk.io, which we'll do in the next section.
