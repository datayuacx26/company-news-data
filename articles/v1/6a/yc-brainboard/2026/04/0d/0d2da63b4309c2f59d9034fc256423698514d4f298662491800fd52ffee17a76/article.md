---
schema_version: "1.0.0"
document_id: "0d2da63b4309c2f59d9034fc256423698514d4f298662491800fd52ffee17a76"
company_key: "yc-brainboard"
company: "Brainboard"
source_id: "yc-brainboard-news-import-c1cb64b4ba70"
canonical_url: "https://www.brainboard.co/blog/what-are-terraform-locals-and-how-to-use-them"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-23T04:03:09.323194+00:00"
fetched_at: "2026-07-28T22:16:01.195245+00:00"
content_hash: "sha256:11e6c0c0dab1a1e1d95f3a6c5e8e7c70737dc39a047169ecdb91311bce368df4"
---

# What are Terraform Locals and How To Use Them?

If you’ve ever opened a Terraform configuration file with hundreds of lines, you know exactly what I’m talking about. The same environment name is repeated 20 times, the project tag is copied into every resource, and to change the prefix, you have to search the entire file with Ctrl+H and hope you didn’t miss anything. But fortunately, there are now Terraform locals that solve this problem.


This is a way to set a value once and use it everywhere without duplication or unnecessary headaches. In this article, we’ll break down how locals work, when to use them, and how they make infrastructure code truly convenient to work with.


## What Are Terraform Locals


Terraform local values are named expressions within a configuration that are evaluated once and can be used anywhere in the module. Simply put, they are variables “for internal use”: you set a value once and then refer to it by name as many times as you like.


Locals are fundamentally different from input (variable) and outputs (output). Input variables accept values from outside, such as the user or a parent module. Outputs pass data outward. Locals, however, exist only within the module and are not visible outside of it. This makes them an ideal tool for intermediate calculations and simplifying logic within the configuration.


When do developers use Terraform local values? Typically, in three situations:


- When the same value appears in multiple resources
- When you need to calculate something based on other variables
- When you want to give a complex expression a clear, human-readable name


Locals do not add new functionality, but they make the code significantly clearer, especially in large projects where consistency is important.


## Why Use Local Values in Terraform


The main reason to use Terraform locals is to eliminate duplication. Imagine that an environment name (dev, staging, prod) is used in the names of twenty resources. If you type it out manually every time, a single typo or change will require twenty edits. Locals let you define a value once and use it everywhere.


The second major benefit is readability. When you see “local.resource_prefix” instead of a complex expression like “${var.project}-${var.env}-${var.region}”, the code becomes clear at a glance. This is especially valuable in teamwork, when multiple engineers are working on a single configuration.


Another advantage is flexibility in code organization. Terraform allows you to use multiple locals blocks in a single file or distribute them across different files. This makes it possible to logically group locals: separately by tag, by resource name, and by network settings. Thanks to this, even large configurations remain structured and predictable. This approach reduces cognitive load and simplifies onboarding for new team members.


## Basic Terraform Locals Example


Let’s look at a simple Terraform locals example to understand how this works in practice:


```text
locals {
environment     = "production"
region          = "us-east-1"
resource_prefix = "${local.environment}-${local.region}"
}


resource "aws_s3_bucket" "main" {
bucket = "${local.resource_prefix}-data-bucket"
}


resource "aws_instance" "web" {
ami           = "ami-0abcdef1234567890"
instance_type = "t3.micro"


tags = {
Name        = "${local.resource_prefix}-web-server"
Environment = local.environment
}
}
```


Let’s break this down step by step. In the “locals” block, we define three values: the environment name, the region, and the resource prefix, which is derived from the first two. Note: a “local” can reference another “local” - this is standard practice.


Then we use “local.resource_prefix” in the S3 bucket name and in the EC2 instance tags. If tomorrow you need to change the environment to staging, update one line in the “locals” block, and all resources will automatically update without searching and replacing throughout the entire project.


This is the main value of this Terraform locals example: minimal effort for changes, maximum consistency in naming.


## Terraform Expressions and Locals


Locals become a truly powerful tool when you start using Terraform expressions within them - expressions that are evaluated dynamically.


You can construct values based on other variables, apply built-in functions, and use conditions:


```text
locals {
is_production = var.environment == "production"
instance_type = local.is_production ? "t3.large" : "t3.micro"
name_prefix   = lower("${var.project}-${var.environment}")
common_tags = {
Project     = var.project
Environment = var.environment
ManagedBy   = "Terraform"
}
}
```


Here, “is_production” is a Boolean expression that checks the “environment” variable. “instance_type” uses a ternary operator to select the instance size based on the environment. “name_prefix” uses the “lower()” function to convert the string to lowercase.


Such Terraform expressions in “locals” allow you to centralize logic in one place and avoid duplicating conditions throughout the configuration. This is especially important when the same solution affects multiple resources at once.


## Using Multiple Local Blocks in Terraform


Terraform does not limit the number of “local” blocks in a single module. You can use multiple local blocks and place them in different parts of a single file or in different project files.


```text
# tags.tf
locals {
common_tags = {
Project   = var.project
ManagedBy = "Terraform"
}
}


# naming.tf
locals {
name_prefix = "${var.project}-${var.environment}"
}
```


Grouping locals by purpose makes the project more organized. One block for tags, another for name prefixes, and a third for network settings - this structure simplifies code navigation.


Important to remember: even though Terraform multiple local blocks are described in different places, they are all accessible within a single module. Terraform combines them into a single namespace, so name conflicts must be avoided.


### Working with Terraform Local Variables in Real Projects


In real projects, Terraform local variables are used everywhere, especially when configuration consistency across environments is important.


A typical example is tag management. In most companies, every cloud resource must have a mandatory set of tags: project, environment, team, and creation date. By defining them once as Terraform local variables, you ensure that no resource is left out:


```text
locals {
standard_tags = {
Project     = var.project
Environment = var.environment
Team        = var.team
CreatedBy   = "Terraform"
}
}


resource "aws_vpc" "main" {
cidr_block = var.vpc_cidr
tags       = local.standard_tags
}


resource "aws_subnet" "public" {
vpc_id     = aws_vpc.main.id
cidr_block = var.subnet_cidr
tags       = merge(local.standard_tags, { Name = "public-subnet" })
}
```


Another common scenario is building resource names according to a single template. When naming conventions are defined in locals, all team engineers automatically follow a single standard without having to think about it every time.


It is precisely for such scenarios that the[Brainboard](https://www.brainboard.co/) platform offers a visual infrastructure designer: you design the architecture visually, and Terraform code with the correct locals and tags is generated automatically. Convenient and fast? Yes, and that’s exactly what we strive for.


### Using Locals with Modules in Terraform


When working with modules, Terraform local modules - that is, using locals within or alongside modules - becomes a particularly valuable pattern.


Locals help prepare and simplify the input data for the module, avoiding complex expressions directly in the module block:


```text
locals {
db_name       = "${var.project}-${var.environment}-db"
db_identifier = lower(replace(local.db_name, "_", "-"))
}


module "database" {
source     = "./modules/rds"
identifier = local.db_identifier
name       = local.db_name
tags       = local.common_tags
}
```


Without locals, all this code with string transformations would end up directly in the “module” block, making it cumbersome and hard to read. Moving logic into locals keeps the module block clean and clear. This is especially important when the same module is called multiple times with similar but slightly different parameters.


## Common Mistakes When Using Terraform Locals


Even experienced engineers sometimes overuse Terraform locals or use them incorrectly. Here are a few common mistakes:


- **Overcomplicating expressions.** When too much complex logic is crammed into a single local, the code becomes hard to read again. If an expression spans more than three lines, it’s better to break it down into several intermediate locals.
- **Confusion with variables.** Locals are not a replacement for input variables. If a value needs to be changed externally (e.g., by a user or a CI/CD system), use a variable rather than a local variable.
- **Overuse.** Not every value needs to be put into locals. If a line is used only once and contains no logic, locals will only add noise. Use them where there is actual repetition or a non-trivial expression.


### Best Practices for Using Terraform Locals


To ensure that Terraform locals are helpful rather than confusing, follow a few simple principles.


- **Use meaningful names.** resource_prefix is better than rp. The name should explain what is stored in the local.
- **Group by meaning.** Use several blocks of locals organized by topic: tags, names, and network settings.
- **Keep it simple.** One local - one clear idea. If you need complex logic, break it down into steps.
- **Document the non-trivial.** If a local contains non-obvious logic, add a short comment next to it.


Terraform locals is one of those tools that’s easy to learn but delivers tangible results right from your first project. Start small: move repetitive tags and name prefixes into locals, and you’ll immediately feel how much cleaner your configuration becomes.


If you want to take it further and manage your entire infrastructure visually - with automatic Terraform code generation, built-in CI/CD, and team support - try[Brainboard](https://www.brainboard.co/) . Our platform offers a free trial and helps take your Infrastructure as Code to the next level.
