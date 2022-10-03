---
title: "Attach IAM Role"
chapter: true
weight: 26
---

# Attaching an IAM Role

In this section we attach the IAM role to grant your EC2 instance permission to create resources.

## Attach the IAM role to your instance
Follow [this link](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:search=hs-workshop;sort=desc:launchTime) to find your Cloud9 EC2 instance.

1. Select the instance
1. Select Actions / Security / Modify IAM role Attach IAM role

![aws-assign-role-step-1](/images/aws-assign-role-1.png)

## Modify IAM Role

2. Choose the role that we created in the previous step: hs-workshop-cloud-9
2. Click on Update IAM Role

![aws-assign-role-step-1](/images/aws-assign-role-2.png)


This completes the process to assign the new role.

### Next Section: Configure the workshop
This paragraph block can optionally be utilized to lead into the next section of the workshop.
