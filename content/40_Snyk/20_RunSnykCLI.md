---
title: "Run the Snyk CLI"
chapter: true
weight: 42
---

## Run Snyk IaC

At your command prompt, run this command at the base directory of your repository you checked out.

```bash
cd ~/vulnerable-ec2
snyk iac test
```

This process should only take a few seconds, and you will seee output similar to what is shown below:

```bash
$ snyk iac test

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

  [Low] EC2 instance accepts IMDSv1
  Info:    Instance Metadata Service v2 is not enforced. Metadata service may be vulnerable to reverse proxy/open firewall misconfigurations and server side request forgery attacks
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-130
  Path:    resource > aws_instance[ec2] > metadata_options
  File:    main.tf
  Resolve: Set `metadata_options.http_tokens` attribute to `required`

  [Low] Resource has public IP assigned
  Info:    AWS resource could be accessed externally via public IP. Increases attack vector reachability
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-51
  Path:    resource > aws_instance[ec2] > associate_public_ip_address
  File:    main.tf
  Resolve: Set `associate_public_ip_address` attribute to `false`

  [Low] AWS Security Group allows open egress
  Info:    The inline security group rule allows open egress. Open egress can be used to exfiltrate data to unauthorized destinations, and enable access to potentially malicious resources
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-73
  Path:    resource > aws_security_group[allow_port_80_from_anywhere] > egress
  File:    main.tf
  Resolve: Set `egress.cidr_blocks` attribute to specific ranges e.g. `192.168.1.0/24`

  [Low] AWS Security Group allows open egress
  Info:    The inline security group rule allows open egress. Open egress can be used to exfiltrate data to unauthorized destinations, and enable access to potentially malicious resources
  Rule:    https://snyk.io/security-rules/SNYK-CC-TF-73
  Path:    resource > aws_security_group[allow_ssh_from_anywhere] > egress
  File:    main.tf
  Resolve: Set `egress.cidr_blocks` attribute to specific ranges e.g. `192.168.1.0/24`

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

  Organization: atlassian-bitbucket
  Project name: http://github.com/gautambaghel/vulnerable-ec2

✔ Files without issues: 0
✗ Files with issues: 1
  Ignored issues: 0
  Total issues: 8 [ 0 critical, 0 high, 3 medium, 5 low ]

-------------------------------------------------------

```

## Review Results

In the output above, you'll see several Low and Medium configuration issues.  We'll focus on the Medium issues, starting with the one identified below:

```bash
[Medium] Non-Encrypted root block device
Info:    The root block device for ec2 instance is not encrypted. That should someone gain unauthorized access to the data they would be able to read the contents.
Rule:    https://snyk.io/security-rules/SNYK-CC-TF-53
Path:    resource > aws_instance[ec2] > root_block_device > encrypted
File:    main.tf
Resolve: Set `root_block_device.encrypted` attribute to `true`
```

In that part of the report, you see several pieces of information.  

* Severity as Low, Medium, High, or Critical.  Here we see the severity of [Medium.]
* Title, Info, Path, File - details for you to locate the issue within your code, plus some context.
* Rule - A link to the publicly available description.
* Resolve - mitigation on how to address the issue

When you review the contents of the file `main.tf` you will find a block defining the instance named `ec2` of type `aws_instance`.  The mitigation here is to add the block within the definition of the "ec2" resource:

```
  root_block_device {
    encrypted = true
  }
```

Add that block and re-run the `snyk iac` command to see one fewer Medium issue.  At the time of this workshop, we drop from 3 to 2:

```
snyk iac test

...

-------------------------------------------------------

Test Summary

  Organization: atlassian-bitbucket
  Project name: http://github.com/gautambaghel/vulnerable-ec2

✔ Files without issues: 0
✗ Files with issues: 1
  Ignored issues: 0
  Total issues: 7 [ 0 critical, 0 high, 2 medium, 5 low ]

-------------------------------------------------------
```

In your environment, you will perform similar actions based on the feedback you see.  Here we show the CLI, and Snyk also works with IDEs such as IntelliJ.  You can also send results to snyk.io, which we'll do in the next section.
