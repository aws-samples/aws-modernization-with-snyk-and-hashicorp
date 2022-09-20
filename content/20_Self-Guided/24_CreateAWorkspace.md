---
title: "Create a Workspace" # MODIFY THIS TITLE
chapter: true
weight: 24 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

<!-- MORE SUBMODULES CAN BE ADDED TO DIVIDE UP THE SETUP INTO SMALLER SECTIONS -->
<!-- COPY AND PASTE THIS SUBMODULE FILE, RENAME, AND CHANGE THE CONTENTS AS NECESSARY -->


# Set Up The Workspace <!-- MODIFY THIS SUBHEADING -->

# Set Up Your Workspace
AWS Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. Cloud9 comes prepackaged with essential tools for popular programming languages, including JavaScript, Python, PHP, and more, so you don’t need to install files or configure your laptop for this workshop.

We will use Amazon Cloud9 to access our AWS accounts via the AWS CLI in this Workshop. There are a few steps to complete to set this up:

1. Create a new Cloud9 IDE environment
1. Create an IAM role for your workspace
1. Attach the IAM role to your workspace
1. Configure workshop specific requirements


### Create a new Cloud9 IDE environment <!-- MODIFY THIS SUBHEADING -->
Within the AWS console, use the region drop list to select us-east-1 (N. Virginia). This will ensure the workshop script provisions the resources in this same region.

Navigate to the Cloud9 console or just search for it under the AWS console services menu.

Click the Create environment button, and use the name use sh-workshop, then click Next step.

![aws-cloud9-step-1](/images/aws-cloud9-1.png)

In the configuration screens, 
- Select the default instance type t2.micro
- Leave all the other settings as default and click Next

![aws-cloud9-step-1](/images/aws-cloud9-2.png)

Verify your selections and press the button to **Create environment**.

Info: This will take about 1-2 minutes to provision

![aws-cloud9-step-3](/images/aws-cloud9-3.png)

### Configure Cloud9 IDE environment

Once your Cloud9 environment is running, customize by:
1. Close the welcome page tab
1. Close the lower work area tab
1. Open a new terminal tab in the main work area.

![aws-cloud9-step-3](/images/aws-cloud9-4.png)

Tip: You are free to customize the look and feel of your environment, including the color scheme.

Tip: Cloud9 requires third-party-cookies. You can whitelist the specific domains. You are having issues with this, Ad blockers, javascript disabler, and tracking blockers should be disabled for the cloud9 domain, or connecting to the workspace might be impacted.

### Next Section Heading <!-- MODIFY THIS HEADING -->
This paragraph block can optionally be utilized to lead into the next section of the workshop.

