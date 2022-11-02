---
title: "Run the Snyk CLI"
chapter: true
weight: 52
---

## Run Snyk IaC

At your command prompt, run this command at the base directory of your repository you checked out.

```bash
cd ~/environment/vulnerable-ec2
snyk iac test
```

{{% notice info %}}
In this repository, we include several commented lined identified with the text `WORKSHOP` that you should review and modify.  In most cases, the changes we ask you to do will be to comment/uncomment lines of text, or to modify an existing line.  We will guide you through those steps.
{{% /notice %}}


This process should only take a few seconds, and you will see many lines of output with a summary similar to what is shown below. The net result is no issues detected.  In the example below, our test Snyk Organization is named `hashicorp`.  You will see more output than what is shown below.


```bash
snyk iac test

Snyk Infrastructure as Code

✔ Test completed.

Issues

Low Severity Issues: 5

  [Low] EC2 API termination protection is not enabled
  Info:    To prevent instance from being accidentally terminated using Amazon EC2, you can enable termination protection for the instance. Without this setting enabled the instances can be terminated by accident. This setting should only be used for instances with high availability requirements. Enabling this may prevent IaC workflows from updating the instance, for example terraform will not be able to terminate the instance to update instance type
  Rule:    https://snyk.io/security-rules/SNYK-CC-AWS-426
  Path:    resource > aws_instance[ec2] > disable_api_termination
  File:    main.tf
  Resolve: Set `disable_api_termination` attribute  with value `true`

*****MULTIPLE LINES DELETED*****

Medium Severity Issues: 3

  [Medium] Security Group allows open ingress
  Info:    That inbound traffic is allowed to a resource from any source instead of a restricted range. That potentially everyone can access your resource
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-1
  Path:    input > resource > aws_security_group[allow_port_80_from_anywhere] > ingress
  File:    main.tf
  Resolve: Set `cidr_block` attribute with a more restrictive IP, for example `192.16.0.0/24`

  [Medium] Security Group allows open ingress
  Info:    That inbound traffic is allowed to a resource from any source instead of a restricted range. That potentially everyone can access your resource
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-1
  Path:    input > resource > aws_security_group[allow_ssh_from_anywhere] > ingress
  File:    main.tf
  Resolve: Set `cidr_block` attribute with a more restrictive IP, for example `192.16.0.0/24`

  [Medium] Non-Encrypted root block device
  Info:    The root block device for ec2 instance is not encrypted. That should someone gain unauthorized access to the data they would be able to read the contents.
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-53
  Path:    resource > aws_instance[ec2] > root_block_device > encrypted
  File:    main.tf
  Resolve: Set `root_block_device.encrypted` attribute to `true`

-------------------------------------------------------

Test Summary

  Organization: hashicorp
  Project name: http://github.com/marcoman/vulnerable-ec2

✔ Files without issues: 1
✗ Files with issues: 1
  Ignored issues: 0
  Total issues: 8 [ 0 critical, 0 high, 3 medium, 5 low ]

-------------------------------------------------------
```

## Review Results - with context

When you examine the entire output, you'll see several pieces of useful information.

* Severity as Low, Medium, High, or Critical.  Here we see the severity of [Medium.]
* Title, Info, Path, File - details for you to locate the issue within your code, plus some context.
* Rule - A link to the publicly available description.
* Resolve - mitigation on how to address the issue
* A summary plus count of files and severities by issues.

In this example, we have several issues that have a severity of Low or Medium, which is good for this workshop.  The focus will be on the three Medium issues.

The first one is about your AWS Security Group allowing open ingress, or being open to the internet.  That is because the CIDR block access is specified as `0.0.0.0/0` for SSH access.  Typically this is not a good practice, so let's review the file entitled `main.tf` to address the issue.  Open the file in your editor and navigate to the section shown below.

```terraform
resource "aws_security_group" "allow_ssh_from_anywhere" {
  name        = "allow_ssh_from_anywhere"
  description = "Allow SSH inbound traffic from anywhere"

  ingress {
    description      = "SSH from anywhere"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    # WORKSHOP: Modify the following line to a CIDR block specific to you, and uncomment the next line with 0.0.0.0
    # This line allows SSH access from any IP address
#    cidr_blocks      = ["0.0.0.0/0"]
    cidr_blocks      = ["0.0.0.0/0"]
  }
```

Let's provision this instance to show how the application is misconfigured to permit access from the internet.

*Previously, we configured an AWS KEY.  We'll need those details to ensure the next parts work.  You have more than one option, and the option we specify here is to use a file named `secrets.auto.tfvars` with contents that look like this:

```terraform
access_key = "YOURACCESSKEY"
secret_key = "YOURSECRET"
```


{{% notice info %}}
The workshop depends on the existence of a default VPC.  Some people remove the default VPC in their environments.  If you see an error message as below, you need to ensure you define a default VPC in the region you are working from.
{{% /notice %}}

In practical applications, auto.tfvars files do not get committed to github by virtue of gitignore exclusions.  Think of this approach as a quick way to provide credentials.  Other alternatives are to use environment variables or configure IAM roles.  We're taking the easy path for this workshop.


```bash
aws_security_group.allow_port_80_from_anywhere: Creating...
╷
│ Error: creating Security Group (allow_ssh_from_anywhere): VPCIdNotSpecified: No default VPC for this user
│       status code: 400, request id: cdf2f4d5-c0dc-4683-b123-dae5b002351c
│ 
│   with aws_security_group.allow_ssh_from_anywhere,
│   on main.tf line 16, in resource "aws_security_group" "allow_ssh_from_anywhere":
│   16: resource "aws_security_group" "allow_ssh_from_anywhere" {
│ 
╵
╷
│ Error: creating Security Group (allow_port_80_from_anywhere): VPCIdNotSpecified: No default VPC for this user
│       status code: 400, request id: fdf22495-df5d-4c20-aa3a-d222c2756f99
│ 
│   with aws_security_group.allow_port_80_from_anywhere,
│   on main.tf line 44, in resource "aws_security_group" "allow_port_80_from_anywhere":
│   44: resource "aws_security_group" "allow_port_80_from_anywhere" {
│ 
╵
```

Terraform init will initialize all the providers you're using, providers are plugins that Terraform uses to spin up different resources using APIs. Since we're spinning up AWS resources only the AWS provider will be initialized.

```bash
terraform init

Initializing the backend...

Initializing provider plugins...
- Finding hashicorp/aws versions matching "4.28.0"...
- Installing hashicorp/aws v4.28.0...
- Installed hashicorp/aws v4.28.0 (signed by HashiCorp)

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

Terraform plan will give you compiled output of everything you're trying to deploy or subsequent changes to your infrastructure if it already exists. 

```bash
terraform plan

data.aws_ami.amazon2: Reading...
data.aws_ami.amazon2: Read complete after 0s [id=ami-027651f3c0057f627]

Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

.
.
.


Plan: 3 to add, 0 to change, 0 to destroy.

```

There are many lines of output, and worth review.  However, we'll focus on the last lines:

```bash
terraform apply -auto-approve

data.aws_ami.amazon2: Reading...
data.aws_ami.amazon2: Read complete after 1s [id=ami-027651f3c0057f627]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.ec2 will be created
  + resource "aws_instance" "ec2" {
.
.
.


Apply complete! Resources: 3 added, 0 changed, 0 destroyed.

Outputs:

instance_ip = "3.238.195.45"
```

Given this IP address, you can attempt to access the instance via SSH with a command like this (your IP address will vary):

```bash
ssh ec2-user@3.238.195.45
```

The results should look like the text below.  Note how we answer "yes" to the prompt, and we are denied access.  We are denied access because we don't have the ssh key to access the instance, so we can't connect.  However, anybody on the internet can access this instance via SSH and that is the crux of our problem.

```bash
marco@potato ~/code/hashicorp/workshop/mam-vulnerable-ec2 (main)
🐧$ ssh ec2-user@3.238.195.45
The authenticity of host '3.238.195.45 (3.238.195.45)' can't be established.
ED25519 key fingerprint is SHA256:hUp1LZEsGqsaUKkDqOjwutzSKJSpWiny1wJFNIhk3E0.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '3.238.195.45' (ED25519) to the list of known hosts.
ec2-user@3.238.195.45: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

Similiarly, run the following curl command to see how anybody can access your hosted application.  Remember, your instance has to be in the Running state in order to be able to provide a response to the curl command.  


```bash
curl http://3.238.195.45
<h1>Hello World from the AWS HashiCorp + Snyk Workshop on ip-172-31-12-224.ec2.internal</h1>
```

In many cases, you may want the world to access your instance via port 80, and in other cases you may want to limit access.  We'll assume we prefer to limit access for the purposes of this workshop.

In order to address these issues, we'll destroy the instance because we'll be making changes to the instance before we try these operations again.

```bash
terraform destroy -auto-approve
```

## Next: Fixing IaC issues 
Now that your EC2 instance is destroyed, we can address the issues starting in the next section.
