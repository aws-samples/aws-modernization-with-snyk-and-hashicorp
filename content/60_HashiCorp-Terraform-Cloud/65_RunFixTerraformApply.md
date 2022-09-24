---
title: "Fix issues locally and re-run apply"
chapter: true
weight: 65
---

## Fixing IaC Issues with Snyk like before

Let's edit the file named `main.tf` again in your cloned repository.  In this file, navigate to the terraform block for the `aws_instance` named `ec2`.  This block is commented with `# WORKSHOP` to indicate it is part of the workshop activities.


```terraform
  ingress {
    description      = "SSH from anywhere"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    # WORKSHOP: Modify the following line to a CIDR block specific to you, and uncomment the next line with 0.0.0.0
    # This line allows SSH access from any IP address
    cidr_blocks      = ["42.43.44.45/32"]
#    cidr_blocks      = ["0.0.0.0/0"]
  }
```

Next, find the section that specifies the port access for HTTP and make similar modifications for the CIDR block to your IP adress.

```terraform
  ingress {
    description      = "HTTP from anywhere"
    from_port        = 80
    to_port          = 80
    protocol         = "tcp"
    # WORKSHOP: Modify the following line to a CIDR block specific to you, and uncomment the next line with 0.0.0.0
    # This line allows HTTP access from any IP address
    cidr_blocks      = ["68.80.18.164/32"]
#    cidr_blocks      = ["0.0.0.0/0"]
  }

```

Now let's fix the Non-Encrypted root block device issue. Configure the resource defintion for the *aws_instance* named "ec2."  Uncomment those lines:

```terraform
resource "aws_instance" "ec2" {

.
.
.
  # WORKSHOP: Add the name of your key here
  key_name = "mam-workshop-keypair"

  # WORKSHOP: uncomment the lines below to enable encrypted block device
  root_block_device {
    encrypted = true
  }
.
.
.

}

```


## Let's run terrafrom apply again


```bash
terraform apply -auto-approve
```

There you have it! Apply successful!


```sh
Running apply in Terraform Cloud. Output will stream here. Pressing Ctrl-C
will cancel the remote apply if its still pending. If the apply started it
will stop streaming the logs, but will not stop the apply running remotely.

Preparing the remote apply...

To view this run in a browser, visit:
https://app.terraform.io/app/alliances/aws-workshop/runs/run-yWsiQth5LqsQKZrN

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

.
.
.

Plan: 3 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + ec2_url = (known after apply)

Run Tasks (post-plan):

All tasks completed! 1 passed, 0 failed

│ snyk-run-task-for-workshop ⸺   Passed
│ No issues found.
Severity threshold is set to medium.
│ Details: https://app.snyk.io/org/gautambaghel/project/9601bbe2-ee14-4b3e-ad69-8e7cad0672cc/history/18680950-90e9-4977-bb1f-34b1768c3734
│ 
│ 
│ Overall Result: Passed

------------------------------------------------------------------------


------------------------------------------------------------------------

Cost Estimation:

Resources: 1 of 1 estimated
           $3.744/mo +$3.744

------------------------------------------------------------------------

.
.
.

aws_instance.ec2: Creating...
aws_instance.ec2: Still creating... [10s elapsed]
aws_instance.ec2: Creation complete after 13s [id=i-032a46ac602277bfa]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:

instance_ip = "3.218.238.185"
```


## Let's destroy the resources again

```bash
terraform destroy -auto-approve
```

```
Plan: 0 to add, 0 to change, 3 to destroy.

Changes to Outputs:
  - instance_ip = "3.218.238.185" -> null

.
.
.

aws_instance.ec2: Still destroying... [id=i-0e691ad63a73cd0ae, 10s elapsed]
aws_instance.ec2: Still destroying... [id=i-0e691ad63a73cd0ae, 20s elapsed]
aws_instance.ec2: Still destroying... [id=i-0e691ad63a73cd0ae, 30s elapsed]
aws_instance.ec2: Destruction complete after 40s
aws_security_group.allow_port_80_from_anyone: Destroying... [id=sg-0da18f732ef595122]
aws_security_group.allow_ssh_from_anyone: Destroying... [id=sg-0c849366c9e2a9bf2]
aws_security_group.allow_port_80_from_anyone: Destruction complete after 0s
aws_security_group.allow_ssh_from_anyone: Destruction complete after 0s

Apply complete! Resources: 0 added, 0 changed, 3 destroyed.
```