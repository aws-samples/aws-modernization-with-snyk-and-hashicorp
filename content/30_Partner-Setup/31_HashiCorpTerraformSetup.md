---
title: "HashiCorp Terraform"
chapter: true
weight: 31 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# HashiCorp Terraform Setup Instructions 

Terraform is an open-source infrastructure as code software tool that enables you to safely and predictably create, change, and improve infrastructure Users define and provide data center infrastructure using a declarative configuration language known as HashiCorp Configuration Language, or optionally JSON.

This section covers installing the Terraform CLI.


## Terraform CLI

The Command-Line-Interface (CLI) to Terraform is via the terraform command, which accepts a variety of subcommands such as terraform init or terraform plan. A full list of all of the supported subcommands is by using terraform -help command.

Start by downloading the Terraform CLI to your environment. In this workshop, we’ll prescribe steps to save time and you can find more details on the Terraform learn site at:
https://learn.hashicorp.com/tutorials/terraform/install-cli

At the Cloud9 prompt, enter these commands to install the Terraform CLI using the yum package manager.

1. Install yum-config-manager to manage your repositories:

```sh
sudo yum install -y yum-utils
```

2. Next, Use yum-config-manager to add the official HashiCorp Linux repository:

```sh
sudo yum-config-manager --add-repo 'https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo'
```

3. Next, Install Terraform from the new repository.

```sh
sudo yum -y install terraform
```

4. Finally, verify successfull installation:

```sh
terraform -help
```
