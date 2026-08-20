---
schema_version: "1.0.0"
document_id: "f8867561aab3e8e719554c1ab0f8bbc4cefd18b054ba0882746e31661cd98f67"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/generating-infrastructure-as-code-from-existing-cloud-resources"
published_at: "2024-04-04T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:b629085f04bb78c607a564474cac2a35f2660567f4d73dc0a2fd2ab5207e97ab"
---

# Generating Infrastructure-as-Code From Existing Cloud Resources

**Before we jump into importing resources, I need to rant about the state of tooling.** If you don’t care about that, click[here](https://www.massdriver.cloud/generating-infrastructure-as-code-from-existing-cloud-resources#getting-started) to jump to the tutorial.


For years, I’ve been writing infrastructure-as-code (IaC) from the start of projects I work on.


When I join a team that did ‘click ops’ through their infrastructure, I generally use` terraform import` to bring in cloud resources and their related infrastructure as I encounter them. It’s tedious, it’s annoying, but it works.


I was excited to dive into this topic. I have not used a “reverse Terraform” tool for generating infrastructure-as-code from existing infrastructure before, and this is a common ask of many of our customers.


But, I got bad news for you:


**If you think you’ve found a silver bullet for handling your ‘click ops’ debt, the ‘reverse Terraform’ tooling and process sucks.**


It’s hard to do if you aren’t familiar with Terraform and the Cloud™️ - which you may not be if you find yourself with a bunch of click ops staring at this article.


If you are experienced with the Cloud™️ and Terraform, I’ve found that it’s easier to write Terraform from scratch and import it using \`terraform import\`\` than using *any* of the ‘reverse Terraform’ tooling.


I suggest traveling back in time and doing IaC from the beginning, but if you can’t time travel, I apologize in advance: **Welcome to the Hell.**


‍


## A Whirlwind Tour of Reverse Terraform Tools


Ah, ‘Reverse Terraform’, the magic bullet for infrastructure debt.


I think it’s funny that we call it ‘reverse Terraform.’ I know it’s because naming things is hard, but if you think about what reverse Terraforming would be outside of code, it’d be destroying the planet’s habitability.


Pure apocalypse. Sorry, needed to set the tone.


Many tools claim to do the job, and developers-being-developers I assume there will be 6 more half-baked ideas by the time I finish writing this article.


A few of the more well-known:


### [terraforming](https://github.com/dtan4/terraforming)


The OG tool, of course unmaintained & archived.


The README suggests two deadmen walking: terraformer and terracognita. Both are discussed below.


### [terracognita](https://github.com/cycloidio/terracognita)


This is an ‘up-and-comer.’ It’s been under development for a few years but somehow feels like someone` git init` ’d it last week.


It supports a minimal set of resources for all of the clouds.


For AWS, it supports 127 resources. AWS has over[200 services](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref) , each with *many resources.* Terracognita offers very little coverage in the grand schema of things.


For any complex infrastructure, you will need to use` terraform import` as well if you are using terracognita.


I tried to import a pretty basic module (our[AWS VPC module](https://www.massdriver.cloud/marketplace/aws-vpc) ), and the amount of resources not supported at the network level was pretty astonishing.


```text
terracognita aws \
-i aws_iam_role \
-i aws_iam_role_policy_attachment -i aws_internet_gateway -i aws_eip -i aws_nat_gateway \
-i aws_route_table -i aws_subnet -i aws_vpc \
--hcl=terracognita-output \
--aws-default-region=us-west-2 \
--tags=md-package:tea-staging-network-jn09


```


- No flow logs
- No KMS (at all!?!?)
- No log groups


It supports route tables!!11! But not default table, routes, or route table associations.


I also tried to import an AWS Aurora PostgreSQL database, and it thought it was an AWS Neptune cluster.


```text
resource "aws_neptune_cluster" "tea_staging_database_z0x2" {
tags = {
managed-by  = "massdriver"
md-manifest = "database"
md-package  = "tea-staging-database-z0x2"
md-project  = "tea"
md-target   = "staging"
}


tags_all = {
managed-by  = "massdriver"
md-manifest = "database"
md-package  = "tea-staging-database-z0x2"
md-project  = "tea"
md-target   = "staging"
}


availability_zones                   = ["us-west-2c", "us-west-2b", "us-west-2a"]
backup_retention_period              = 1
cluster_identifier                   = "tea-staging-database-z0x2"
copy_tags_to_snapshot                = true
engine                               = "aurora-postgresql"
engine_version                       = "14.6"
kms_key_arn                          = "arn:aws:kms:us-west-2:YEAH_RIGHT_DAWG:key/33b8e85-b47b-4a2d-a188-db22320eba16"
neptune_cluster_parameter_group_name = aws_db_parameter_group.default_aurora_postgresql14.id
neptune_subnet_group_name            = "tea-staging-database-z0x2"
port                                 = 5432
preferred_backup_window              = "12:05-12:35"
preferred_maintenance_window         = "sun:06:56-sun:07:26"
storage_encrypted                    = true
vpc_security_group_ids               = [aws_security_group.sg_0845903aac706f466.id]
}


```


This is not a serious tool.


### [aws2tf](https://github.com/aws-samples/aws2tf) ,[aztfy / aztfexport](https://github.com/Azure/aztfexport) , etc


There are a ton of *cloud-specific* tools to turn a specific cloud’s resources into Terraform. They suffer the same problems we’ll see with` terraformer` below, but have the added weakness of only supporting one cloud. Womp.


### [terraformer](https://github.com/GoogleCloudPlatform/terraformer)


Terraformer is the tool the tutorial will focus in on. It’s the “best.”


Built by the team at Waze, but much like finding yourself in a random neighborhood trying to cross four lanes of traffic during rush hour with no stop light or stop sign, Terraformer sets you up to get side-swiped.


‍


## Generating Terraform / OpenTofu from Existing Cloud Resources


Note: Everything in this tutorial works for Terraform and OpenTofu. Feel free to follow along with your tool of choice. You'll need to substitute` tofu` for any place you see` terraform` .


Terraformer is *pretty* good.


It creates the terraform code **and** the state file, which is critical. It also supports AWS, GCP, Azure, as well as a bunch of other[clouds](https://github.com/GoogleCloudPlatform/terraformer/tree/master/docs) . It also supports a ton of[AWS resources](https://github.com/GoogleCloudPlatform/terraformer/blob/master/docs/aws.md) .


That’s about it on the niceties. Calling terraformer pretty good is a bit like rating a restaurant on Yelp 5 stars because they brought your food on a plate. It’s a low bar.


### Getting Started


Installation from[source](https://github.com/GoogleCloudPlatform/terraformer#installation) or just package install the thing:


```text
brew install terraformer
sudo port install terraformer
choco install terraformer


```


I’ll be using AWS resources for the example ([supported resources here](https://github.com/GoogleCloudPlatform/terraformer/blob/master/docs/aws.md) ), but this walkthrough would work for Azure or GCP as well.


‍


### Generating Terraform


#### Import All Terraform Resources


Generating IaC from all existing resources is the experience users wish these tools offered, a single command, magic. This will do that, but it does *not* set you up for success.


Importing all resources is also an extremely slow process as Terraform will hit the list/get APIs for every single cloud service.


An important note: running this on all` *` will create a terraform directory for *every* cloud service, not just the ones you use. The directory will have two moot files in it:` providers.tf` and` terraform.tfstate` . I would suggest deleting any directory with just those two files to prevent you from wasting too much (more) of your time.


```text
terraformer import aws --resources=* --regions=us-west-2


```


Terraform resources for an entire region grouped by service is not a great way to organize your code, and makes it extremely hard to connect terraform modules and enforce parity. Below, you’ll see my subnets in different modules than my VPC. I would recommend merging those two state files and modules and perform a substantial refactoring of the code to get this ready for a production CI/CD pipeline.


Couldn't come up with a worse organization strategy if I tried.


Importing all resources may be a good start *if* you have very few resources in your cloud account; and it is a *nice* approach for getting a bearing of all of your cloud assets, but I would consider the code throwaway (a lot of the code is going to be *bad* , but we’ll get to that in a minute).


#### Import Terraform resources by resource type


Another option is to generate IaC by resource types. This approach isn’t a great option if you have multiple instances of any cloud service.


For example, say you have two postgres databases: staging and prod. The result would be a terraform module for each of those resources. To enforce parity between environments we would want a single module with` staging` and` production` workspaces/variables.


```text
terraformer import aws --resources=vpc,subnet --regions=us-west-2 --path-pattern {output}


```


In this example, I added` --path-pattern {output}` , which will put all the resultant code into a single module.


When reverse Terraforming or writing IaC in general, you should be thinking of a single module use case. Omiting the path pattern would result in a directory per resource type like this:


Meh


Importing by resource type is a good approach if you have *very* few resources in the cloud. It’s optimal for when you only have one instance of any given service, i.e., one network, one aurora database, one ETL pipeline, etc. Upon running this example, I had 5 VPCs in the same module. This generated code isn’t an ideal as it ties all of the VPC lifecycles together in my CI/CD pipeline. To add a subnet to one VPC, I end up having to read all of them. That’s a big blast radius.


#### Import Terraform resources by resource IDs


Generating IaC by resource IDs will yield the best module structure *so far* . Still, it will be up to you to back out your modules to support any additional instances of a service (like the staging/prod example above).


This approach is really good for when you are using a service *multiple times* for **distinct** use cases.


For example a redis cluster configured for long-term storage vs a redis cluster for page caching. These are two very different use cases and warrant two different terraform modules with very different variable interfaces exposed to your team.


**NOTE:** It is common to see anemic terraform modules designed around no use case and instead just creates another abstration/interface on top of some Terraform resources. A clear indicator is a variables file with 10s if not 100+ settings. At Massdriver, IaC modules should be designed to be use case specific with operational expertise *built into* the module. We don’t think teams should have an “s3 bucket” module, instead, they should have a “logging bucket module,” “cdn bucket module,” and “datalake bucket module” each with curated variable interfaces so engineers can quickly provision without getting bogged down in figuring out tens to hundreds of fields.


```text
terraformer import aws --resources=vpc,subnet --filter=vpc=myvpcid --regions=us-west-2 --path-pattern {output}


```


This approach is extremely time-consuming if you have a lot of cloud resources. You have to go into the AWS dashboard, find all the IDs, figure out how they all fit together, and then run the import.


**FUN FACT** Before we get any further, the “import all” command is so slow. I’ve gotten this far in writing this blog post, and its still running. Yay.


#### Import Terraform resources by tags


This is by far the most efficient way to import resources and get a good baseline set of terraform modules to start your IaC journey. I want to be clear, running this command *gets you started* you still have a **lot** of work to do.


The one caveat with this approach is, you must have a good process for tagging resources in the cloud. If you haven’t been tagging your resources consistently by environment, purpose, project, etc, then this approach will not work for you.


In this example, I’m importing resource from our[AWS VPC module](https://github.com/massdriver-cloud/aws-vpc) .


When we design our infrastructure, we are pretty diligent about tagging, and we consider things like cloudwatch alarms and notification channels a part of the infrastructure (e.g. vpc) we are deploy.


This does a really good job of creating a baseline terraform module with all of the components we would expect. Again, this works for us because we are consistent in our tagging convention.` md-package` below is a unique identifier we assign to all cloud resources that are a part of a single "use-case."


```text
terraformer import aws \
--resources=vpc,subnet,igw,eip,route_table,sns,nat,cloudwatch \
--filter="Name=tags.md-package;Value=tea-staging-network-jn09" \
--regions=us-west-2 \
--path-output=generated \
--path-pattern="{output}"


```


‍


## The work you still have to do


With whatever approach you take to generating terraform from your existing cloud resources, there are going to be quite a few bumps in the road ahead of you.


### ` tag` ,` tag_all` &` default_tags`


If you are familiar with the AWS Terraform provider, it supports a notion of “default_tags” for all resources. When using this tool that method won’t be used, and instead tags are duplicated twice on each resource under` tag` and` tag_all` . You’ll probably want to get rid of at least one of those and/or move to` default_tags` to DRY up tagging all resources in the same module.


### Variables


This is the hard part, defining the interface to your module. None of these tools do the magic, and many modules on the official registry just project 80% of the resource fields out to variables. Good variable design in Terraform leads to easier to manage modules, less breaking changes, and gives you the **opportunity** to encode operational expertise into your module, instead of exposing all of its guts.


IMO this is the hardest part of IaC, good interface design. Good luck!


### No` for_each`


For every single resource that has multiple instances and new` resource` block will be added to the Terraform code - with an absolutely brutal resource identifier. You’ll probably end up refactoring this, but also make sure to refactor that state file! Good luck!


```text
resource "aws_subnet" "tfer--subnet-0cbe7d08a2a32f7d0" {
vpc_id            = "vpc-41bc19c86f3cd8be8"
cidr_block        = "10.0.1.0/24"
availability_zone = "us-west-2a"
}


resource "aws_subnet" "tfer--subnet-0dbe8308a2a32f7d0" {
vpc_id            = "vpc-41bc19c86f3cd8be8"
cidr_block        = "10.0.2.0/24"
availability_zone = "us-west-2b"
}


resource "aws_subnet" "tfer--subnet-1cbe7d08a2a32f7d0" {
vpc_id            = "vpc-41bc19c86f3cd8be8"
cidr_block        = "10.0.3.0/24"
availability_zone = "us-west-2c"
}


```


### No referencing or interlinking of resources


None of these tools do any inference on how your resources are connected together. Instead of using the ability to reference resources` aws_ec2_vpc.main.arn` they simply drop the string ARN into fields. You’ll need to cross reference the state file and the generated code to get it figured out. Good luck!


This is the same example from the last section, note that there isn't a reference to another resource, instead its the bare VPC ID string.


```text
resource "aws_subnet" "tfer--subnet-0cbe7d08a2a32f7d0" {
vpc_id            = "vpc-41bc19c86f3cd8be8"
cidr_block        = "10.0.1.0/24"
availability_zone = "us-west-2a"
}


resource "aws_subnet" "tfer--subnet-0dbe8308a2a32f7d0" {
vpc_id            = "vpc-41bc19c86f3cd8be8"
cidr_block        = "10.0.2.0/24"
availability_zone = "us-west-2b"
}


resource "aws_subnet" "tfer--subnet-1cbe7d08a2a32f7d0" {
vpc_id            = "vpc-41bc19c86f3cd8be8"
cidr_block        = "10.0.3.0/24"
availability_zone = "us-west-2c"
}


```


### Naming, the hardest button to button


The resource names that are generated are absolutely garbage. You’ll want to update these to something that makes sense, but you are also going to have to syncronize those changes to the state file so terraform doesnt think you want to delete and recreate the resource (that’d be bad). Good luck!


```text
output "aws_vpc_tfer--vpc-0004a66b0b36ecaf9_id" {
value = "${aws_vpc.tfer--vpc-0004a66b0b36ecaf9.id}"
}


output "aws_vpc_tfer--vpc-03ff27926df168eea_id" {
value = "${aws_vpc.tfer--vpc-03ff27926df168eea.id}"
}


output "aws_vpc_tfer--vpc-08dee04735e8785d8_id" {
value = "${aws_vpc.tfer--vpc-08dee04735e8785d8.id}"
}


output "aws_vpc_tfer--vpc-0bbe7d08a2a32f7d0_id" {
value = "${aws_vpc.tfer--vpc-0bbe7d08a2a32f7d0.id}"
}


output "aws_vpc_tfer--vpc-b8c9c5c0_id" {
value = "${aws_vpc.tfer--vpc-b8c9c5c0.id}"
}


```


‍


## What these tools could do better


All IaC import tools are prototyping tools, they aren’t a silver bullet, they aren’t giving you production-ready code.


In my (gasbag) opinion, these tools should stop optimizing for “IMPORT EVERYTHING.” Its never going to work, and its a shitty way to lay out your IaC. These tools shouldn’t be encouraging bad practices. I’ve seen so many teams fall into this trap, and its obnoxiously time-consuming to get out of.


When working through a set of resources (whether by type or ID or tag), they should find resources of the same type and group them together in a for each. Adding a new` resource` block in Terraform for every one of my subnets doesnt not give me flexibility in subnet design between networks.


The naming kind of sucks, if there is only one of a resource, just call it` main` or` this` .


If there is more than one of a resource, prompt the user for a resource name. It sucks to flip between` main.tf` and the state file trying to figure out what the resource is. During the generating process you have the params and the ID, as me what I want to call it.


Connect my resources with resource references. If a field looks like an ARN or an Azure Subscription ID, find that resource in the Terraform your generating and do a` bucket = aws_s3_bucket.main.arn` for god’s sake isntead of just putting the string of the ARN.


‍


## Conclusion


As someone that diligently codifies my infrastructure, I was very unimpressed by the state of generating IaC. You're going to end up doing a fair amount of refactoring, and in my opinion, its easier to start with a clean state (pun intended) and write your IaC by hand, and then import resources into it.


I also put together a **free** webinar on strategies for generating IaC on April 17, 2024. I'll also be sharing an open-source wrapper around Terraformer and some LLM tools that solves some of the issues above. Sign up below, its free!
