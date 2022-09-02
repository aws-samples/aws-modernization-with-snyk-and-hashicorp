---
title: "Send Results to Snyk"
chapter: true
weight: 58
---

## Sending results to Snyk

At your command prompt, ensure you are authorized with Snyk by issuing the command below.  In the setup steps, you were asked to get the API_TOKEN from snyk.io.  See the [Snyk setup page for those instructions.]({{<ref "30_Partner-Setup/40_SnykSetup.md" >}})


```bash
snyk auth
```

Once you are authorized, we'll re-run the same snyk command from before, with the extra parameter `--report` that sends the results to Snyk.

```bash
$ snyk iac test --report
```

You'll see the same type of output as before, plus a little more shown below.  This extra information is the link and the name of the project in Snyk:

```bash
Report Complete

  Your test results are available at: https://snyk.io/org/<the name of your organizatio>/projects
  under the name: http://github.com/gautambaghel/vulnerable-ec2
```

Copy-and-paste the link, or click through the link if your CLI supports it.  When you navigate to Snyk you'll arrive at a list of projects for your organization.  If this is your first time using Snyk, you will only see one project entry.  If you are an existing user, you will see the name of your project .

The image looks like the following:
![snyk-project-entry](/images/snyk-iac-project-entry.png)

As expected, the information on the screen matches the status of your working project.  On the web UI, we add a little more context to help.  In the view below, you also see the option to ignore the issue or create a Jira issue.  The ignore feature is available on all Snyk tiers, including free.  Jira integration, on the other hand, is availble on paid tiers.

![snyk-project-entry](/images/snyk-iac-issue-detail.png)

In the user interface, when you click on the Ignore button you are presented with choices.  These include marking the issue as "Not Vulnerable" or "Ignore temporarily" or "Ignore permanently."  The intention of these choices is to let you configure Snyk to work within your desired workflow. 

![snyk-project-entry](/images/snyk-iac-ignore-issue.png)

Let's click on the Ignore Temporarily option and add a comment.  When you save your selection, your number of vulnerabilities changes but the added onscreen filter of `Ignored` now shows you a count.  When you enable that checkbox, you see your previous issue with an explanation of its Ignored status.  From here you can edit the ignore condition, or exit the ignore state.

![snyk-project-entry](/images/snyk-iac-ignore-temporarily.png)



