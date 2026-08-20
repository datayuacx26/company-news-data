---
schema_version: "1.0.0"
document_id: "2a665c31e280da63b56069a55c20bfcc6a5c9581bd8e2dbfc960b5cc151e879f"
company_key: "yc-brainboard"
company: "Brainboard"
source_id: "yc-brainboard-news-import-c1cb64b4ba70"
canonical_url: "https://www.brainboard.co/blog/what-is-policy-as-code-a-practical-guide"
published_at: null
first_seen_at: "2026-07-23T04:03:09.323194+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:0c264cc52659c3a63fd06fa6e3d35461b1cd675fbab416752d5c8c0e5614c4f5"
---

# What Is Policy As Code: A Practical Guide

Security policies in PDF format that no one reads. Checklists in Confluence are outdated the day they are created. Manual checks before deployment depend on the reviewer's mood. If you recognize your team, it's time to understand what policy-as-code is and why this approach is changing the rules of the game in infrastructure management.


## Defining Policy-as-Code


Policy-as-code is the translation of corporate rules, security standards, and compliance requirements into a machine-readable format. Instead of a document that says "all S3 buckets must be encrypted," you write a rule in Rego, Sentinel, or Python that automatically checks each bucket when it is created.


The idea is simple: if a rule can be formulated in words, it can be written in code. And code can do what humans cannot - work without breaks, never forget, and never make exceptions "because it's Friday."


Code-as-policies transform governance from a reactive process ("found a violation during an audit - corrected it") to a proactive one ("violations are impossible because the system won't let them happen"). For teams working with Terraform,[Brainboard](https://www.brainboard.co/) embeds policy checks directly into the architecture design process - violations are visible before the code enters the repository.


## How Policy-as-a-Code Works


The mechanics of policy-as-code look like this. There are three components: rules, data, and an engine.


Rules describe what is allowed and what is not allowed. For example: "EC2 instances in production must only use approved AMIs" or "no Security Group should open port 22 for 0.0.0.0/0."


Data is the current state of the infrastructure. Terraform plan, CloudFormation template, Kubernetes manifest - anything that describes resources.


The engine takes the rules, applies them to the data, and returns a verdict: pass or fail. If it fails, the deployment is blocked, and the team receives a specific message: which rule was violated and where exactly.


When does this happen?


- **In the CI/CD pipeline** - before applying. The most common scenario: Terraform plans are run through OPA or Sentinel, and if anything is wrong, the merge is blocked.
- **At runtime** , admission controllers in Kubernetes check each resource upon creation.
- **On schedule** - periodic scans of the existing infrastructure catch policy drift.


The main thing is that feedback should be fast and clear. If an engineer learns about a violation three days later from the security team via email, that's not policy-as-code. That's bureaucracy with an extra step.


## Compliance-as-Code and Automation


Compliance-as-code is a special case of policy-as-a-code, tailored to regulatory requirements. SOC 2, HIPAA, PCI DSS, GDPR - all these frameworks consist of specific rules that can be coded.


Why is this necessary?


- Manual audits take weeks. Automatic checks take seconds.
- Humans miss violations. Code does not (if the rule is written correctly).
- The auditor report is generated automatically: which rules are checked, when the last check was performed, which resources comply, and which do not.


A typical example: instead of manually checking that all databases are encrypted, you write a rule that blocks the creation of unencrypted RDS instances. The auditor receives a log showing that the rule has been in effect since a certain date and that no violations have been recorded. No more questions.


Brainboard helps integrate compliance checks into the visual infrastructure design process, enabling policies to be applied at the design stage.


## Policy Enforcement in Modern Systems


Writing a rule is half the battle. Policy enforcement is a mechanism that ensures the execution of policies. Without enforcement, a policy remains a recommendation.


How it works in practice:


- **Preventive enforcement** - blocking an action before it is performed. Admission webhook in Kubernetes rejects pods without resource limits. Sentinel in Terraform Cloud prevents the application from proceeding if the policy is violated.
- **Detective enforcement** - detecting violations after the fact. AWS Config Rules scans resources and flags inconsistencies. This is useful for legacy infrastructure that cannot be transitioned to preventive control immediately.
- **Corrective enforcement** is automatic correction. A Lambda function that restricts public access to an S3 bucket seconds after it is enabled.


The ideal option is all three levels simultaneously. But it's worth starting with prevention: it's cheaper to prevent a problem than to catch and fix it.


## Network-as-Code and Infrastructure Governance


Network-as-code extends the same approach to the network level. Instead of manually configuring firewalls, routes, and VPN tunnels, the network is described in configuration files and managed through pipelines.


What this gives you:


- Security Groups, NACLs, route tables - everything is in Git. Every change goes through code review.
- Network security policies are checked automatically: "no resource in a private subnet should have a public IP."
- Rolling back network changes is done with git revert, not by panicking in the console.


A concrete example: a rule prohibits creating VPC peering connections with accounts outside the organization. An engineer tries to add peering - CI/CD blocks the PR with an explanation of which policy has been violated.[Brainboard](https://www.brainboard.co/features/synchronized-architectures) visualizes network policies on an architectural diagram, simplifying control.


## Practical Examples of Policy-as-Code


Theory is good. But policy-as-code examples from real projects are more convincing.


- **Access control.** Rule: IAM policies must not contain "Effect": "Allow", 'Action': "*". OPA checks every Terraform plan and blocks wildcard permissions.
- **Encryption.** All EBS volumes, S3 buckets, and RDS instances must be encrypted. The rule checks for the presence of encryption configuration - if it's missing, the resource is not created.
- **Tagging.** Each resource must have the tags team, env, and cost-center. Without tags, deployment is blocked. This is not a whim: without tags, it is impossible to calculate costs.
- **Instance size.** Only t3.micro and t3.small are allowed in the dev environment. If someone tries to run p4d.24xlarge for a "quick test," the rule will not allow it.


## Advantages and Challenges of Policy-as-Code


Code-as-policy has obvious advantages: automation of routine tasks, scalability (one rule works for thousands of resources), consistency, and transparency for auditing.


But there are also challenges. Writing good rules requires an understanding of both the infrastructure and business requirements. Policies that are too strict slow down development - engineers start looking for workarounds. Policies that are too lenient don't provide protection. Finding the right balance is not easy and requires iteration.


Another problem is the governance of the policies themselves. Who writes the rules? Who reviews them? How are exceptions handled? Without answers to these questions, policy-as-code becomes a tool of chaos rather than order.


### Why Policy-as-Code Is Essential


Policy-as-code is not an optional "best practice" but a necessity for teams working with cloud infrastructure. Manual checks are not scalable. Compliance requirements are becoming more stringent. Deployment speed is increasing. The only way to maintain control is to automate the rules and embed them into the workflow.[Brainboard](https://www.brainboard.co/contact-us) does this through a visual designer that ties policies to the architecture.


### FAQ


**1. What is policy-as-code, and why is it important?**


It is the practice of writing corporate rules in machine-readable code. It is important because it allows you to automatically check and block violations - faster and more reliably than manual checks.


**2. How does policy-as-code differ from traditional compliance?**


Traditional compliance involves manual checklists and periodic audits. Policy-as-code involves automated, real-time checks built into CI/CD.


**3. What are the main benefits of using policy-as-code?**


Automation, consistency, speed of feedback, scalability, and transparency for auditing.


**4. Can policy-as-code be used for security and access control?**


Yes. Control of IAM policies, encryption, network rules, and tagging are the most common scenarios.


**5. What tools are commonly used to implement policy-as-code?**


OPA (Open Policy Agent), HashiCorp Sentinel, AWS Config Rules, Checkov, tfsec. For visual management -[Brainboard](https://www.brainboard.co/) .
