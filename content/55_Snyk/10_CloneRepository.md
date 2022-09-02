---
title: "Clone the repository"
chapter: true
weight: 56
---

## Clone the repository

Let's start by cloning a fresh copy of this public repository from GitHub.  In your working environment, run these commands to start from your home directory and perform a clone of the repository:

```bash
cd ~/
git clone https://github.com/adversarialengineering/eks-demo-deployments.git
cd eks-demo-deployments/terraform/default
```

In this repository, we'll scan and review the results of the IaC project and make local changes to see how a developer would fix issues at the environment.

You will be asked to make and commit changes to the code in a later module.  We'll provide instructions at that time about creating a fork under your GitHub ID.

Note: If you have an existing Snyk account with more than one organization, and you wish to use a non-default org, you can set this.  Find your org ID (ORG_ID below) and it with the command below.  Your desired ORG_ID should be a 32 character UUID unique to you.

```
snyk config set org=ORG_ID
```
