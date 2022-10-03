---
title: "Try to run terraform apply"
chapter: true
weight: 64
---

## Let's try to run Terraform Apply

We can see the result of post-plan run task failed with the message "Error: the run failed because the run task, snyk-run-task-for-workshop is required to succeed"

This ensures that a DevOps engineers mistakenly or a bad actor intentionally doesn't exploit your infrastructure by adding malicious code.


```bash
terraform apply
```

```sh
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
│ Details: https://app.snyk.io/org/gautambaghel/project/9601bbe2-ee14-4b3e-ad69-8e7cad0672cc/history/29d4bb88-68d9-4f64-8858-369ce5f89df1
│ 
│ 
│ Overall Result: Failed

------------------------------------------------------------------------

╷
│ Error: the run failed because the run task, snyk-run-task-for-workshop, is required to succeed
│ 
│ 
```

Let's fix the issues, sames as before and try to apply again!