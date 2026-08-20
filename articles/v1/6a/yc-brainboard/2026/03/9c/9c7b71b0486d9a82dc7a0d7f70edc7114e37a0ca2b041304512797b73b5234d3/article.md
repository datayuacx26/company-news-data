---
schema_version: "1.0.0"
document_id: "9c7b71b0486d9a82dc7a0d7f70edc7114e37a0ca2b041304512797b73b5234d3"
company_key: "yc-brainboard"
company: "Brainboard"
source_id: "yc-brainboard-news-import-c1cb64b4ba70"
canonical_url: "https://www.brainboard.co/blog/terraform-lifecycle-managing-resource-changes-effectively"
published_at: "2026-03-05T00:00:00+00:00"
first_seen_at: "2026-07-23T04:03:09.323194+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:4147209dbe0a6b75179b17cd4579d520db548aaeb1c9f564d1eece1a00c3ecde"
---

# Terraform Lifecycle: Managing Resource Changes Effectively

Terraform is great at creating infrastructure. But changes can be painful. You updated your AMI, and Terraform decided to recreate the instance. You manually changed the tag, and the plan shows a 40-line diff. Sound familiar? Terraform lifecycle is a mechanism that gives you control over how resources are created, updated, and deleted. Without it, you are at the mercy of default behavior, and defaults are not always user-friendly.


## Terraform Lifecycle Overview


Terraform's lifecycle manages each resource's lifecycle from creation to destruction. By default, Terraform acts straightforwardly - if the configuration has changed, the resource is updated in-place. If the update is not possible, the resource is deleted and recreated.


Terraform lifecycle rules allow you to intervene in this process. Do you want the new resource to be created before the old one is deleted? There is a rule for that. Want Terraform to ignore changes to certain attributes? There's a rule for that, too. Want to prohibit the deletion of a resource altogether? You got it.


Without lifecycle rules, any change is a lottery. With them, it's a predictable, controllable process. This is especially critical in production, where unexpected database recreation is a disaster.


## Lifecycle Block


A Terraform lifecycle block is a block within a resource where behavior rules are set. The syntax is simple:


resource "aws_instance" 'web' {


ami = “ami-12345”


instance_type = “t3.small”


lifecycle {


create_before_destroy = true


ignore_changes = \[tags\]


prevent_destroy = true


}


}


Three main attributes:


- create_before_destroy - first create a new resource, then delete the old one
- ignore_changes - do not respond to changes in the specified attributes
- prevent_destroy - prohibit deletion of the resource via Terraform


There is also replace_triggered_by - forced replacement of a resource when another is changed. More on this below. Each attribute solves a specific problem, and[Brainboard](https://www.brainboard.co/) helps visualize which lifecycle rules are applied to which resources, right on the architecture diagram.


## Terraform Lifecycle Rules


In practice, Terraform's lifecycle boils down to a few typical scenarios. Terraform creation before destruction is the most common request: to ensure zero downtime when replacing a resource.


But there are others:


- **Protecting critical resources.** prevent_destroy = true on an RDS instance or S3 bucket with data. If someone accidentally removes a resource from the configuration, Terraform will refuse to delete it and return an error.
- **Ignoring external changes.** Auto Scaling Group changes desired_capacity at runtime. Terraform sees the discrepancy and wants to "fix" it. ignore_changes says: don't touch it.
- **Forced replacement.** The user_data script has changed, but Terraform still considers an in-place update sufficient. replace_triggered_by will force the instance to be recreated.


The rules can be combined. A single resource can have both create_before_destroy and ignore_changes - there are no restrictions.


## Terraform Ignore Changs


Terraform ignores changes as a lifesaver when external processes change resource attributes, and Terraform starts to "fight" with reality.


Classic examples:


- The EKS cluster updates tags automatically. Terraform sees the diff and wants to roll back. ignore_changes = \[tags\] - and there's no problem.
- A Lambda function is deployed through a separate pipeline that updates the filename and source_code_hash. Terraform only manages the configuration; the code is not its domain. ignore_changes = \[filename, source_code_hash\].
- Terraform ignore changes also accepts wildcards: ignore_changes = all ignores everything. This is useful for resources imported via terraform importthat are not yet fully described in the code.


An important nuance: ignore_changes does not mean "this field is unimportant." It means "this field is managed by someone else." Abuse leads to drift that no one controls. Use it sparingly.


## Create Before Destroy


Terraform creates before it destroys, solving a specific problem: when replacing a resource, Terraform defaults to deleting the old one first, then creating the new one. There is downtime between these actions.


With create_before_destroy = true, the order is reversed: the new resource is created first, traffic is switched, and the old one is deleted - zero downtime.


Where this is critical:


- SSL certificates in ACM - the new certificate must be ready before the old one is deleted
- Launch templates for ASG - the new template is applied, the old one is removed
- DNS records - the new record is created in parallel, then the old one is deleted


Pitfall: not all resources support the simultaneous existence of two instances. If a resource has a unique identifier (e.g., name or IP), creating a new one may result in a conflict. Test in staging.[Brainboard](https://www.brainboard.co/) helps to check such dependencies visually - the diagram shows which resources are linked and where replacement can cause a cascade effect.


## Terraform Triggers


Terraform triggers via replace_triggered_by forcing a resource to be recreated when another resource or attribute changes. This is not the default behavior - normally, Terraform will only recreate a resource when its own arguments change.


resource "aws_instance" 'app' {


ami = “ami-12345”


instance_type = “t3.medium”


lifecycle {


replace_triggered_by = \[


aws_launch_template.app.latest_version


\]


}


}


Typical scenarios:


- The launch template has been updated - the instance must be recreated with the new configuration
- Secret in Secrets Manager has changed - the application needs to be restarted with new credentials
- user_data has been updated - but it does not trigger instance replacement on its own


Triggers make dependencies between resources explicit. Without them, such connections remain in engineers' minds - and that is an unreliable storage medium.


## Lifecycle Hooks


The Terraform lifecycle hook term is often confused with the Terraform lifecycle block. Strictly speaking, there are no native hooks (pre-apply, post-destroy) in Terraform. But some mechanisms perform a similar function:


- provisioner "local-exec" - executes a command on the machine where Terraform is running
- null_resource with triggers - performs actions when dependencies change
- Terragrunt hooks - before_hook and after_hook for each stage


AWS Auto Scaling Group has real lifecycle hooks - they pause the instance at startup or shutdown, giving you time to perform custom actions. But this is an AWS feature, not a Terraform feature.


### Managing Resource Changes


Strategies for safe change management:


- **Plan before applying.** A Terraform plan is mandatory. See what will be deleted and what will be created. If the plan shows destruction of the production base, stop.
- **Use -target with caution** . Spot apply is convenient for testing, but regular use leads to desynchronization of the state.[Brainboard](https://www.brainboard.co/) visualizes dependencies between resources, helping to predict cascading changes.
- **State backup before risky operations.** Especially with Terraform state mv or resource import.


### Best Practices


A few rules proven in practice:


- Set prevent_destroy on stateful resources: databases, buckets, encryption keys. It's cheaper to err on the side of caution.
- ignore_changes - only for attributes controlled by external processes. Not for "too lazy to figure out why diff."
- create_before_destroy test in staging. Not all resources survive the simultaneous existence of two copies.
- Version the **Terraform lifecycle** rules along with the rest of the code. Code review is mandatory - lifecycle rules affect behavior during deployment.


[Brainboard](https://www.brainboard.co/contact-us) simplifies lifecycle rule control by displaying them in the context of the architecture, rather than in sheets of HCL code.


### FAQ


**1. What is the Terraform lifecycle, and why is it important?**


A mechanism for controlling how resources are created, updated, and deleted. Without it, Terraform follows default rules that are often not suitable for production.


**2. How does the lifecycle block help manage resource changes?**


The lifecycle block within a resource defines rules for protection against deletion, ignoring changes, and replacement order. This controls Terraform's behavior with each application.


**3. What does terraform ignore changes do, and when should it be used?**


Tells Terraform not to react to changes in specific attributes. Used when an attribute is managed by an external process - ASG, CI/CD pipeline, or other tool.


**4. How does Terraform create before it destroys to prevent downtime?**


A new resource is created before the old one is deleted. The switch occurs without an interval when the resource is missing.


**5. What are Terraform triggers, and how do they automate resource updates?**


replace_triggered_by recreates a resource when another resource or attribute changes. Makes implicit dependencies explicit.
