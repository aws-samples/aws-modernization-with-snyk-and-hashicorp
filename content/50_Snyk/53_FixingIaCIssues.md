---
title: "Fixing IaC issues with Snyk"
chapter: true
weight: 53
---

## Fixing IaC Issues with Snyk

Here we edit the file named `main.tf` in your cloned repository.  In this file, navigate to the terraform block for the `aws_instance` named `ec2`.  This block is commented with `# WORKSHOP` to indicate it is part of the workshop activities.

You will see text similar to what is shown below for the *ingress* or what is entering your EC2 instance from the outside.  We'll change the CIDR block from the `0.0.0.0/0` string, which allows *any* internet address to access your port 22 to something much more specific.  You'll need to find your internet IP address.  Sometimes this is as simple as a [search in google is sufficient](https://www.google.com/search?q=what+is+my+ip&oq=what+is+my+ip), and other times you may have to use a service such as [whatismyipaddress.com](https://whatismyipaddress.com/) or others.  Get your IP address and modify the block in an editor to specify your IP address followed by "/32" to limit the range to exactly one IP.  For example, if you IP address is 42.43.44.45, the file will specified as shown below:

TODO: state how we'll take advantage of the Cloud9 IP is different from your working machine.  Get the IP address for your machine to verify it works from there, but it will not work from the Cloud9 instance.


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

Finally, scroll down to the section where you specify the keypair to access your instance. If you don't have one, don't worry because we can skip the small command later but it may help you see the complete picture.

```terraform
  # WORKSHOP: Add the name of your key here
  key_name = "mam-workshop-keypair"
```

Save your file and re-run your snyk iac scan and observe how you now have 1 issue.

```bash
snyk iac test

*****MULTIPLE LINES DELETED*****

Medium Severity Issues: 1

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
  Total issues: 6 [ 0 critical, 0 high, 1 medium, 5 low ]

```

This is great!  Now let's look at the last Medium issue for the Non-Encrypted root block device.  We'll take the cue of the name for **root_block_device**, and the dot, and then **encrypted** to know this means a block that is visible in the resource defintion for the *aws_instance* named "ec2."  Uncomment those lines:

```terraform
resource "aws_instance" "ec2" {
  ami           = data.aws_ami.amazon2.id
  instance_type = "t3.nano"
  associate_public_ip_address = true
  vpc_security_group_ids = [ aws_security_group.allow_ssh_from_anywhere.id, aws_security_group.allow_port_80_from_anywhere.id]

  user_data = <<-EOF
    #!/bin/bash
    # install httpd (Linux 2 version)
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
    echo "<h1>Hello World from the AWS HashiCorp + Snyk Workshop on $(hostname -f)</h1>" > /var/www/html/index.html
  EOF

  # WORKSHOP: Add the name of your key here
  key_name = "mam-workshop-keypair"

  # WORKSHOP: uncomment the lines below to enable encrypted block device
  root_block_device {
    encrypted = true
  }
}
```

Now we can re-run our snyk iac test command and observe we no longer have Medium issues.

```bash
snyk iac test

*****MULTIPLE LINES DELETED*****

-------------------------------------------------------

Test Summary

  Organization: hashicorp
  Project name: http://github.com/marcoman/vulnerable-ec2

✔ Files without issues: 1
✗ Files with issues: 1
  Ignored issues: 0
  Total issues: 5 [ 0 critical, 0 high, 0 medium, 5 low ]
```

We can now re-deploy the ec2 instance and observe your access is only permitted to your specific IP.  Here it is worth reviewing the output of the terraform plan command to see some differences.  The + and - indicators in terraform output show what is being added or removed, and you will see comments in parenteheses indicating some of the changes.  The blocks below highlight those changes of interest to us.

```bash
terraform plan

*****MULTIPLE LINES DELETED*****
Terraform will perform the following actions:

  # aws_instance.ec2 must be replaced
-/+ resource "aws_instance" "ec2" {

*****MULTIPLE LINES DELETED*****
      ~ public_ip                            = "3.238.195.45" -> (known after apply)
      ~ secondary_private_ips                = [] -> (known after apply)
      ~ security_groups                      = [
          - "allow_port_80_from_anywhere",
          - "allow_ssh_from_anywhere",
        ] -> (known after apply)

*****MULTIPLE LINES DELETED*****
      ~ root_block_device {
          ~ device_name           = "/dev/xvda" -> (known after apply)
          ~ encrypted             = false -> true # forces replacement

*****MULTIPLE LINES DELETED*****
  # aws_security_group.allow_port_80_from_anywhere will be updated in-place
  ~ resource "aws_security_group" "allow_port_80_from_anywhere" {
        id                     = "sg-0eab5d8decccc18b4"
      ~ ingress                = [
          - {
              - cidr_blocks      = [
                  - "0.0.0.0/0",
                ]

*****MULTIPLE LINES DELETED*****
          + {
              + cidr_blocks      = [
                  + "68.80.18.164/32",
                ]

*****MULTIPLE LINES DELETED*****
  # aws_security_group.allow_ssh_from_anywhere will be updated in-place
  ~ resource "aws_security_group" "allow_ssh_from_anywhere" {
        id                     = "sg-0b8b128de07211a85"
      ~ ingress                = [
          - {
              - cidr_blocks      = [
                  - "0.0.0.0/0",
                ]

*****MULTIPLE LINES DELETED*****
          + {
              + cidr_blocks      = [
                  + "68.80.18.164/32",
                ]

*****MULTIPLE LINES DELETED*****
Plan: 1 to add, 2 to change, 1 to destroy.

Changes to Outputs:
  ~ instance_ip = "3.238.195.45" -> (known after apply)
```

It's good to see the changes we planned are accounted for, and that we have to replace items for some of them.

Run the apply command and re-do the tests to confirm lack of access from other IP addresses.

```bash
terraform apply
ssh  ec2-user@3.238.195.45
curl http://3.238.195.45
```

When you are complete with these tests, let's delete the provisioned infrastructure.

```bash
terraform destroy
```

## Next: Sending results to Snyk
In the next section, we'll send results to the Snyk UI.

