---
title: "Technical Issue / Problem" # MODIFY THIS TITLE
chapter: true
weight: 12 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

# The technical hurdles to managing infrastructure 
In this section, we provide context for Infrastructure as Code and additional security requirements.

## Infrastructure as Code
The migration to cloud has brought an evolution of development behaviors and processes.  Previously, people would use a web browser interface to configure their infrastructure with many clicks and manual text entry.  With maturity, Infrastructure as Code (IaC) is the logical next phase in the evolution.  IaC allows teams to define their infrastructure with code and this leads to treating your infrastructure like a software project.  This includes utilizing a Software Development LifeCycle (SDLC) to the development phases and the familiarity of Git repositories, pull requests, testing, and peer development.  Overall, IaC is a significant advantage for development teams.

## Infrastructure Security
Security misconfigurations are the top source of security issues in infrastructure.

IaC saves teams from unmanaged button clicks and related time-consuming tasks, but there still is an increase of work for already busy teams.  For many teams, IaC is a new discipline with new languages, tools, and processes.  Most teams are ramping up in their knowledge whilie focusing on delivering infrastructure quickly and efficiently.  The priority is often about delivering infrastructure and people focus their study on ensuring their deployments work.  Security suffers primarily because the outputs are not easy to see.  After all, we're highly motivated to ensure application traffic is solid, and we're not usually testing the boundaries of security.  Furthermore, some default settings are more permissive than required or our teams choose to create permissive infrastructure in the interest of getting things to work.

Consider how many people search online for examples to help them solve an infrastructure problem.  Nearly all examples show the essentials of how to get something work and defer on enhanced features.  Many examples even state how it is incumbent on you to ensure production-ready or secure deployments.  Very few examples exist to show you well-secured infrastructure.

Development teams strongly prefer to use tooling that includes expert guidance and Easy Buttons such as automatic code completion in an IDE.  IaC security is now available to developers as an Easy Button.

## Next: The Technical Problem
In the next section, we provide an overview of HashiCorp and Snyk and how they help people with their Infrastructure As Code projects plus Security.
