---
schema_version: "1.0.0"
document_id: "5f55b5ec1e55c150ae9639deb11ee435af8485c66721116f6eae69a6b0d22aa2"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/generating-opentofu-code-from-existing-cloud-resources-with-ai"
published_at: "2024-05-16T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:c61df712135f19b1f0cc30040932af77b6fd32ea9e12d4d3ab43758c33a49d01"
---

# Generating OpenTofu code from existing cloud resources with AI

**Update (Sept 18, 2024):** Want to learn OpenTofu from the ground up? We're hosting a **free** 10-part hands-on instructor-led workshop on adopting infrastructure-as-code with OpenTofu. Seats are limited, sign up[here.](https://www.massdriver.cloud/blogs/convention-over-configuration-building-developer-friendly-iac-with-opentofu)


---


‍


If you want to follow along in this tutorial you can find the code repo[here](https://github.com/massdriver-cloud/generating-opentofu-terraform-from-existing-cloud-resources/tree/main/azure) and original[webinar recording.](https://youtu.be/btqfPUbl_bQ?t=684)


‍


What do OpenTofu, Terraform, Azure, JQ, bash, mods, and OpenAI have in common?


Not much besides they'll be all up in your shell if you want to generate IaC with any sort of speed.


The key to doing this quickly is having a good tagging (or at least naming) convention.


Naming conventions are difficult to apply retroactively because:


a) some resources can't be named


b) renaming stuff sometimes destroys it


Before doing any IaC, I come up with at least a basic tagging strategy and go apply it to resources. You can do a full fledged one or something temporary like "iac-gen-wip=yourname" to make it easy if you have multiple people on your team generating IaC.


My general go to:


- iac-gen-wip=YOUR_NAME_HERE
- iac-gen-env=ENVIRONMENT_HERE


This is enough to deliniate environments and team members if multiple folks are reverse Terraforming. It's easy to remove any temporary tagging stragies once the infra is captured in IaC.


You'll need a few CLI tools to follow along:


- [jq](https://github.com/stedolan/jq)
- [az](https://github.com/Azure/azure-cli)
- [mods](https://github.com/charmbracelet/mods)


## Tutorial


A note before we get started, you'll see that I have two directories` module/staging` and` module/prod` . This is *not* the old terraform anti-pattern of having a root module per environment. When I'm generating IaC I work with two 'environments' individually so I can see the difference in parity/resources between environments, I'll then back this out to a single module with multiple workstations.


Reset the tutorial:


```text
export IAC_GEN_ENV_TAG="iac-gen-env=ENVIRONMENT_HERE"
export IAC_GEN_WHO_TAG="iac-gen-wip=YOUR_NAME_HERE"


#   For webinar:
#   export   IAG_GEN_ENV_TAG=md-target=staging
#   export   IAC_GEN_WHO_TAG=md-project=geniac
make


```


‍


Each of the clouds have CLI commands for getting resources by tags. Unfortunately Azure's is` OR` -based, so if you have multiple tags, it'll grab any resource w/ any of the tags.


```text
az resource list --tag $IAG_GEN_ENV_TAG --tag $IAG_GEN_WHO_TAG | jq '.[].id'


```


‍


I use this script to AND tags w/ jq and get the resource identifiers:


```text
./hack/list_resource_by_tags.sh azure $IAG_GEN_ENV_TAG $IAG_GEN_WHO_TAG


```


‍


You can use it for AWS as well:


```text
./hack/list_resource_by_tags.sh aws $IAG_GEN_ENV_TAG $IAG_GEN_WHO_TAG


```


‍


If you didn't do any tagging, but have maintained a good naming convention, you might have luck grepping for it, although this doesn't work well in AWS where you don't get to truly 'name' resources like VPCs and subnets.


```text
az resource list | jq -r .[].id | grep MY_NAMING_CONVENTION_HERE


```


‍


Dump the list of IDs to file. The OpenTofu / Terraform import block works on cloud IDs.


```text
./hack/list_resource_by_tags.sh azure "$IAG_GEN_ENV_TAG" "$IAG_GEN_WHO_TAG" > resources.json


```


‍


Take a look at the identifiers to make sure you didn't pick up any other resources accidentally.


‍


You may not get back *all* of the resources you tagged.


```text
code resources.json


```


‍


The script uses the az resource API which doesnt return all resource types like subnets, groups, or disks.


You may need to hit a few more Azure APIs to find those resources.


Example to get subnets:


```text
az network vnet subnet list --resource-group geniac-staging-network-r6cm --vnet-name geniac-staging-network-r6cm | jq -r .[].id >> /tmp/azimport-ids.txt;


```


‍


Now we'll start using` mods` to make calls to AI to do the 'hard' work for us. I've had the most success with ChatGPT-4o and Anthropic Claude 3.


You can tune your mods configurationg by running` mods --settings` . Make sure to set your word wrap to a fairly high number, 250 characters or more for Azure, the resource IDs are very long URL paths.


```text
cat prompts/generate resources.json | mods -r


```


‍


You can recall previous mods sessions with:


```text
mods -s ID_HERE -r


```


‍


Add the code to` module/staging/import.tf` and then run:` ‍`


```text
cd module/staging
tofu init -upgrade
tofu plan -generate-config-out=generated.tf


```


‍


You may have gotten some terraform errors on generation. The AI's particularly w/ Azure don't always get the Azure URLs right. It can mess up casing and Azure is very casing sensitive.


If you got errors, try this. The Terraform errors are usually enough to get a good fix.` ‍`


```text
rm generated.tf
tofu plan -generate-config-out=generated.tf 2> errors.txt
cat ../../prompts/refactor/fix errors.txt | mods -r


#   Copy the import statements
tofu plan -generate-config-out=generated.tf


```


‍


Look at the generated code:` ‍`


```text
cat generated.tf


```


‍


Run a plan to see how it looks, we should see that OpenTofu / Terraform want to import a few resources.


```text
tofu plan


```


‍


This may well error, some providers can generate invalid code.


If you got errors:` ‍`


```text
tofu plan 2> errors.txt
cat ../../prompts/refactor/fix errors.txt generated.tf | mods -r


```


‍


The` generate` prompt should have output` import` commands for each of the resources. When working with production resources, I'll generally import one at a time using the command once I am sure I want my local state to be the 'owner' of the resource.


You can run each of the commands or simply run: (remember that you can recall and list previous mods sessions!)


```text
tofu apply


```


‍


After importing, run plan again and we should see "No changes"` ‍`


```text
tofu plan


```


‍


**Woohoo!**


‍


So what we've got now is a single Terraform source file and state. Thats good, but we've got work to do.


‍


The generated code has:


- no variables
- no for_each for repeated resource
- may have some minor issues
- no referencing between resources


‍


I don't have a good for_each example here, so lets just put a naive one at the bottom of` generated.tf`


```text
cat generated.tf /assets/for_each-files-example.tf > /tmp/azimport.tf; cp /tmp/azimport.tf generated.tf


```


‍


I'm going to get this into state, and we can just "make believe" that this is a set of subnets or db instances in a cluster. Some resources that we would want to have a variable number of per environment.` ‍`


```text
tofu init -upgrade
tofu apply


```


‍


Ok, you should have two files in your state and on disk now.


Now how this` for_each` step works is, if you look at the` generate` prompt, I request that any resources of the same type have their Terraform resource name set to` main-0` ,` main-1` , etc. This` for_each` takes advantage of that and backs resources out to a local (which I back out to variables during the workspaces step).


I've seen this occassionally back out single resources when running on the more naive AI models.


```text
cat ../../prompts/refactor/for_each generated.tf | mods -r


```


‍


Update your` generated.tf` with the output of the` for_each` step and run:


```text
tofu plan


```


‍


Running` tofu plan` will try to destroy two resources because the state file is now wrong! State` mv` commands should have been generated by the model.


Lets run those:


```text
tofu state mv ...


```


‍


Running plan again should result in "No changes."


```text
tofu plan


```


‍


Now lets back out a *few* common variables. We'll end up doing a second variable when we generalize the 'staging' and 'prod' modules into a single module.


```text
cat ../../prompts/refactor/variables_common generated.tf | mods -r


```


‍


Update your` generated.tf` and run plan again, you should see "No changes":


```text
tofu plan


```


‍


At this point you should have about 60-70% of the work done for reverse Terraforming / Tofuing cloud resources into IaC.


From here, I'll replicate these tasks above in the "prod" module, obtaining the import statements for production and beginning the process of diffing out a common module that can be used across multiple workspaces.


I typically leave resource referencing and variable integration until after the production environment has a solid baseline. This approach allows me to identify disparities between environments and abstract them into a unified module interface.


Join Us for Part Two of the Webinar!


Are you interested in learning more?[Sign up for part two of our webinar!](https://streamyard.com/watch/t4rBcSR2cRAh)


We'll take the staging and production modules we've created and consolidate them into a single, cohesive module with workspaces. Don't miss this opportunity to streamline your infrastructure as code practices.


‍
