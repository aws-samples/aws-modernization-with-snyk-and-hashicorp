---
title: "Run terraform init & plan"
chapter: true
weight: 63
---

## Initialize the terraform repository and plan

Run terraform init command again to initialize Terraform Cloud

```bash
terraform init
```

```sh
Initializing Terraform Cloud...

Initializing provider plugins...
- Reusing previous version of hashicorp/aws from the dependency lock file
- Using previously-installed hashicorp/aws v4.28.0

Terraform Cloud has been successfully initialized!

You may now begin working with Terraform Cloud. Try running "terraform plan" to
see any changes that are required for your infrastructure.

If you ever set or change modules or Terraform Settings, run "terraform init"
again to reinitialize your working directory.
```

Run terraform plan command to see the upcoming changes. You'll notice that the plan is being run remotely instead of using your local machine

```bash
terraform plan
```

```sh
Running plan in Terraform Cloud. Output will stream here. Pressing Ctrl-C
will stop streaming the logs, but will not stop the plan running remotely.

Preparing the remote plan...

To view this run in a browser, visit:
https://app.terraform.io/app/alliances/aws-workshop/runs/run-KgrNYsAgGJaxdPFW

Waiting for the plan to start...

Terraform v1.3.0
on linux_amd64
Initializing plugins and modules...
data.aws_ami.amazon2: Reading...
data.aws_ami.amazon2: Read complete after 0s [id=ami-027651f3c0057f627]

Terraform used the selected providers to generate the following execution
plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.ec2 will be created
  + resource "aws_instance" "ec2" {

.
.
.

Plan: 3 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + ec2_url = (known after apply)

Run Tasks (post-plan):

All tasks completed! 0 passed, 1 failed

│ snyk-run-task-for-workshop ⸺   Failed (Mandatory)
│ Found:
3 medium severity issue(s).
5 low severity issue(s).
Severity threshold is set to low.
│ Details: https://app.snyk.io/org/gautambaghel/project/9601bbe2-ee14-4b3e-ad69-8e7cad0672cc/history/5825101f-1762-4b3e-9f5b-a110edf5018a
│ 
│ 
│ Overall Result: Failed

------------------------------------------------------------------------

╷
│ Error: the run failed because the run task, snyk-run-task-for-workshop, is required to succeed
│ 
│ 
╵
```