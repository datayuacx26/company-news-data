---
schema_version: "1.0.0"
document_id: "aaeac9ede21416d55525b1376e50d9aa6f6340e4c90341fee2dd6335d92f461e"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/getting-started-with-opentofu-on-azure"
published_at: "2024-10-30T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:4bed6714f4c52820d4afee400ca50d23c562bacf83463972424326e93bfae999"
---

# Getting Started with OpenTofu on Azure

OpenTofu is an infrastructure-as-code tool that lets you define both cloud and on-prem resources in human-readable configuration files that you can version, reuse, and share. It is hosted by the Linux Foundation.


**Update (Sept 18, 2024)** : Want to learn OpenTofu from the ground up? We're hosting a **free** 10-part hands-on instructor-led workshop on adopting infrastructure-as-code with OpenTofu. Seats are limited, sign up[here](https://www.massdriver.cloud/blogs/convention-over-configuration-building-developer-friendly-iac-with-opentofu) .


This repo aims to help you get started using OpenTofu to manage your Azure resources.


1. Setup OpenTofu and Azure
2. Set up state in Azure Storage
3. Review IaC best practices
4. Import existing resources into OpenTofu


This is a guide for setting up the Azure CLI and OpenTofu to deploy a resource group in Azure.


## Prerequisites


- An Azure account
- Azure CLI


## Create Azure Service Principal


If you want to follow the portal steps, feel free. I will be covering the CLI steps.


1. Log into Azure CLI and get your subscription ID (save` subscription_id` for step 3)


```text
az login
az account list --output table


```


1. Create a service principal (save` appId` ,` password` , and` tenant` for step 3)


```text
az ad sp create-for-rbac --name  "tofu-on-azure"   --role owner --scopes /subscriptions/<azure_subscription_id>


```


1. Set your environment variables


```text
export   ARM_SUBSCRIPTION_ID= "<azure_subscription_id>"
export   ARM_TENANT_ID= "<azure_subscription_tenant_id>"
export   ARM_CLIENT_ID= "<service_principal_appid>"
export   ARM_CLIENT_SECRET= "<service_principal_password>"


```


## Install OpenTofu


### Brew


```text
brew update
brew install opentofu


```


### Standalone


```text
## Download the installer script:
curl --proto  '=https'   --tlsv1.2 -fsSL https://get.opentofu.org/install-opentofu.sh -o install-opentofu.sh
## Alternatively: wget --secure-protocol=TLSv1_2 --https-only https://get.opentofu.org/install-opentofu.sh -O install-opentofu.sh


## Grant execution permissions:
chmod   +x install-opentofu.sh


## Please inspect the downloaded script at this point.


## Run the installer:
./install-opentofu.sh --install-method standalone


## Remove the installer:
rm   -f install-opentofu.sh


```


### PowerShell


```text
## Download the installer script:
Invoke-WebRequest -outfile "install-opentofu.ps1" -uri "https://get.opentofu.org/install-opentofu.ps1"


## Please inspect the downloaded script at this point.


## Run the installer:
& .\install-opentofu.ps1 -installMethod standalone


## Remove the installer:
Remove-Item install-opentofu.ps1


```


Verify version:


```text
tofu -version


```


> \[!TIP\] If you already have Terraform and want to migrate to OpenTofu, you can find out more about that here.


## Deploy a resource group


1. Create a new directory to deploy a new resource group and` cd` into it
2. Create a file named` providers.tf` and paste the following:


```text
terraform {
required_providers {
azurerm = {
source  = "hashicorp/azurerm"
version = "~>3.0"
}
random = {
source  = "hashicorp/random"
version = "~>3.0"
}
}
}


provider "azurerm" {
features {}
}


```


1. Create a new file named` main.tf` and paste the following:


```text
resource "random_pet" "rg_name" {
prefix = var.resource_group_name_prefix
}


resource "azurerm_resource_group" "rg" {
name     = random_pet.rg_name.id
location = var.resource_group_location
}


```


1. Create a new file named` variables.tf` and paste the following:


```text
variable "resource_group_location" {
type        = string
default     = "westus"
description = "Location of the resource group."
}


variable "resource_group_name_prefix" {
type        = string
default     = "tofu-on-azure"
description = "Prefix of the resource group name that's combined with a random ID so name is unique in your Azure subscription."
}


```


1. Create a new file named` outputs.tf` and paste the following:


```text
output "resource_group_name" {
value = azurerm_resource_group.rg.name
}


```


1. Run` tofu init -upgrade`


- ` tofu init` is used to initialize the OpenTofu environment and download the AzureRM provider
- ` -upgrade` parameter upgrades the provider plugins to the newest version


2. Run` tofu plan -out main.tfplan`


- ` tofu plan` creates an execution plan, but doesn't execute it. It determines which actions are necessary to create the configuration specified. Important for comparing changes.
- ` -out` is an optional parameter that allows you to specify an output file for the plan, otherwise it prints to console


3. Run` tofu apply main.tfplan`


- ` tofu apply` is used to deploy the configuration
- ` tofu apply` can be ran without specifying a plan file, and it'll run on your detected` .tf` files


## Verify results


Portal method.


### CLI


1. Get the Azure resource group name


```text
resource_group_name=$(tofu output -raw resource_group_name)


```


1. Run` az group show` to display the resource group


```text
az group show --name  $resource_group_name


```


## Cleanup resources


1. Run` tofu plan` and specify` -destroy` tag


```text
tofu plan -destroy -out main.destroy.tfplan


```


1. Run` tofu apply` to apply the execution plan


```text
tofu apply main.destroy.tfplan


```


*Alternatively, you can run` tofu destroy` without` tofu plan/tofu apply`*


OpenTofu state is used to reconcile deployed resources with the desired state. State allows OpenTofu to know what Azure resources to add, update, or delete.


By default, OpenTofu state is stored locally, which isn't ideal for a few reasons:


- Local state doesn't work well in a team or collaborative environment
- OpenTofu state can include sensitive information
- Storing state locally increases chance of accidental deletion


## Prerequisites


- Completed setup


## Configure remote state storage account


Assuming you have OpenTofu configured at this point, let's configure our remote state storage account using it!


1. Create a new directory for your remote state storage account and` cd` into it
2. Create a new file named` providers.tf` in the directory and paste the following:


```text
terraform {
required_providers {
azurerm = {
source  = "hashicorp/azurerm"
version = "~>3.0"
}
}
}


provider "azurerm" {
features {}
}


```


1. Create a new file named` main.tf` in the directory and paste the following:


```text
resource "random_string" "resource_code" {
length  = 5
special = false
upper   = false
}


resource "azurerm_resource_group" "tfstate" {
name     = "tfstate"
location = "westus"
}


resource "azurerm_storage_account" "tfstate" {
name                     = "tfstate${random_string.resource_code.result}"
resource_group_name      = azurerm_resource_group.tfstate.name
location                 = azurerm_resource_group.tfstate.location
account_tier             = "Standard"
account_replication_type = "LRS"
allow_nested_items_to_be_public = false
}


resource "azurerm_storage_container" "tfstate" {
name                  = "tfstate"
storage_account_name  = azurerm_storage_account.tfstate.name
container_access_type = "private"
}


```


1. Run` tofu init` then` tofu apply` to configure the Azure storage account and container


## Configure remote state backend


To configure the backend state, you need the following Azure storage information:


- ` storage_account_name` : The name of the storage account
- ` container_name` : The name of the container
- ` key` : The name of the state store file to be created


1. Fetch storage account name with the following command:


```text
az storage account list --resource-group tfstate --query  "[].name"   --output tsv


```


1. Once you have the required information, create a new directory and a new file named` backend.tf` and paste the following:


```text
terraform {
required_providers {
azurerm = {
source  = "hashicorp/azurerm"
version = "~>3.0"
}
}
backend "azurerm" {
resource_group_name  = "tfstate"
storage_account_name = "<storage_account_name>"
container_name       = "tfstate"
key                  = "terraform.tfstate"
}


}


provider "azurerm" {
features {}
}


resource "azurerm_resource_group" "state-demo-secure" {
name     = "state-demo"
location = "westus"
}


```


1.


Run` tofu init` then` tofu apply` to configure the backend state.


2.


Confirm the state is being stored by running the following:


```text
az storage blob list --account-name <storage_account_name> --container-name tfstate


```


> \[!NOTE\] Azure blobs are automatically locked when any operation writes to state. This pattern prevents concurrent state operations, which can cause corruption. For more info, see state locking


> \[!NOTE\] Data stored in an Azure blob is encrypted before being persisted. When needed, the OpenTofu retrieves the state from the backend and stores it in local memory. If this pattern is used, state is never written to your local disk. For more info about Azure storage encryption, see Azure storage encryption for data at rest


## Cleanup


- Cleanup the backend state by running` tofu destroy`
- Cleanup the storage account by running` tofu destroy`


The guide below outlines recommended best practices to follow when using OpenTofu in Azure.


## Code


- **Use Modules** : Organize your OpenTofu code using modules promote reusability and maintainability. Modules help to encapsulate logic, reduce duplication, and simplify code.
- **Use Variables and Parameters** : Implement variables and parameters to make your code more flexible and reusable. This customization reduces duplication and eases code maintenance.
- **Use Consistent Naming Conventions** : Adopt a consistent naming convention for resources, variables, and outputs to enhance readability and manageability of your code. This helps avoid confusion and ensures a uniform structure.


##### Example naming


Resource Type Naming Convention Example


Resource Group` rg-<project>-<env>`


Virtual Network` vnet-<project>-<env>`


Storage Account` store<project><env>`


- **Implement Input Validation** : Validate input variables to ensure they meet expected criteria and prevent misconfigurations. Use validation blocks to enforce constraints on variables. Richer validation can be achieved using JSON Schema.


##### Example


```text
variable "environment" {
description = "The environment to deploy to"
type        = string


validation {
condition = contains(["dev", "staging", "production"], var.environment)
error_message = "Invalid environment. Must be one of: dev, staging, production"
}
}


variable "resource_name" {
description = "The name of the resource"
type        = string


validation {
condition = length(var.resource_name) <= 24
error_message = "Resource name must be 24 characters or less"
}
}


```


##### Example


```text
{
"$schema"  :    "https://json-schema.org/draft-07/schema#"  ,
"type"  :    "object"  ,
"properties"  :    {
"environment"  :    {
"description"  :    "The environment to deploy to"  ,
"type"  :    "string"  ,
"enum"  :    [  "dev"  ,    "staging"  ,    "production"  ]  ,
"errorMessage"  :    {
"enum"  :    "The environment must be one of 'dev', 'staging', or 'production'."
}
}  ,
"resource_name"  :    {
"description"  :    "The name of the resource"  ,
"type"  :    "string"  ,
"maxLength"  :    24  ,
"errorMessage"  :    {
"maxLength"  :    "The resource name must be 24 characters or less."
}
}
}  ,
"required"  :    [  "environment"  ,    "resource_name"  ]  ,
"additionalProperties"  :    false
}


```


- **Use Locals** : Utilize locals to encapsulate complex expressions around variable manipulation, string formatting, and conditional logic. Makes your code more readable, maintainable, and reusable.


##### Example locals


```text
locals {
max_length        = 24
alphanumeric_name = substr(replace(var.name, "/[^a-z0-9]/", ""), 0, local.max_length)
}


resource "azurerm_storage_account" "storage" {
name     = local.alphanumeric_name
location = var.location
}


```


- **Manage Dependencies** : Ensure proper sequencing and avoid circular dependencies by using` depends_on` to manage resource dependencies. Terraform providers typically handle dependencies, but` depends_on` can enforce the correct order of operations when needed.


##### Example` depends_on`


```text
resource "azurerm_virtual_network" "vnet" {
name = "my-vnet"
}


module "subnet" {
source               = "./modules/subnet"
name                 = "my-subnet"
virtual_network_name = azurerm_virtual_network.vnet.name
depends_on           = [azurerm_virtual_network.vnet]
}


```


- **Use Azure's Terraform Providers** : Leverage Azure's Terraform providers (AzureRM, AzureAD, AzureDevops, AzAPI, AzureStack) to interact with Azure's services and APIs, enabling seamless integration and management.
- Use Workspaces: Manage multiple environments (e.g., dev, staging, production) within a single OpenTofu configuration using workspaces. This segregation helps manage configurations and maintain consistency across environments.


## VCS & CI/CD


- **Use Version Control** : Track changes to code and collaborate with others using a version control system like Git. This is essential for managing changes, tracking the history of OpenTofu configurations, and effective collaboration.
- **Branching Strategy** : Implement a branching strategy (e.g. GitFlow) to manage feature development, bug fixes, and releases. This ensures that code changes are organized and integrated smoothly.
- **Plan and Review Changes** : Always run` tofu plan` to review the changes before applying them. This helps to identify potential issues and understand the impact of your changes.
- **Integrate with CI/CD Pipelines** : Automate testing, building, and deployment of infrastructure by integrating the OpenTofu GitHub Action with your CI/CD pipelines. This ensures consistency, reliability, and repeatability in your deployments.


##### Example CI/CD


```text
name:    OpenTofu    CI/CD    Pipeline


on:
push:
branches:
-    main
pull_request:
branches:
-    main


jobs:
opentofu:
name:    'OpenTofu'
runs-on:    ubuntu-latest


steps:
-    name:    'Checkout Code'
uses:    actions/checkout@v2


-    name:    'Set up Terraform'
uses:    opentofu/setup-opentofu@v1
with:
tofu_version:    1.8  .0


-    name:    'OpenTofu Format'
run:    tofu    fmt    -check


-    name:    'OpenTofu Init'
run:    tofu    init


-    name:    'OpenTofu Plan'
run:    tofu    plan    -no-color


-    name:    'OpenTofu Apply'
run:    tofu    apply    tfplan


```


## State management


- **Use Cloud Provisioning and Orchestration Platforms** : Leverage platforms that offer features like remote state management, collaborative tools, deployment histories, user-friendly GUI, security & scalability. This simplifies managing infrastructure as code, automating deployments, and collaborating effectively.
- **Implement State Locking** : Prevent concurrent operations that could lead to conflicts and inconsistencies by implementing state locking mechanisms.
- **Establish Backup and Restore Procedures** : Ensure data integrity and recover from failures by establishing backup and restore procedures for your state files.
- **Use Data Lookups Instead of Remote State References** : Avoid using potentially stale data and adding dependencies on your state files by opting for data lookups instead of remote state references.
- **State File Security** : Encrypt your state files to protect sensitive information. Ensure that state files are stored securely and access is restricted to authorized users.
- **Periodic State File Cleanup** : Regularly clean up- old state file versions to reduce clutter and improve performance. This helps in maintaining a clean state management system


## Documentation


- **Tag Resources** : Enhance visibility, organization, and management by tagging your resources. This helps in identifying resources, tracking costs, and managing resources effectively.


##### Example


```text
resource "azurerm_resource_group" "rg" {
name     = "my-rg"
location = "East US"
tags = {
environment = "dev"
owner       = "John Doe"
}
}


```


- **Maintain Clear Documentation** : Ensure your OpenTofu configurations are well-documented and up-to-date. This helps onboard new team members, troubleshoot issues, and manage infrastructure effectively.
- **Include Architecture Diagrams** : Provide architecture diagrams that illustrate the infrastructure setup. This helps new users to visualize the deployment and understand the relationship between resources.
- **Review OpenTofu and Azure Provider Changelogs** : Stay informed about new features, bug fixes, and improvements by regularly reviewing the OpenTofu changelog and changelog for each Azure Terraform provider used.


## Testing


- **Include Automated Tests** : Validate your IaC to ensure correctness and prevent errors by including automated tests. This helps identify issues early, reduce risks, and improves reliability and consistency.
- **Define Rollback Strategies** : Plan for failures by defining rollback strategies to revert changes in case of errors or issues during deployments.
- **Test Infrastructure Changes in Isolation** : Test changes in a separate environment before applying them in production. This minimizes the risk of disruptions and allows for thorough testing.


## Security


- **Adopt Security Scanning Tools** : Integrate tools like Checkov or Terrascan into your CI/CD pipelines to scan for security vulnerabilities and compliance violations.
- **Regular Security Audits** : Conduct regular security audits to identify and address vulnerabilities in your OpenTofu configurations. This ensures that your infrastructure remains secure over time.
- **Secure Sensitive Information** : Manage and store sensitive information such as API keys, passwords, and certificates using dedicated secrets management services like Azure Key Vault.
- **Least Privilege Principle** : Apply the principal of least privilege by granting minimal permissions required for resources. This reduces the risk of unauthorized access and enhances security.
- **Isolate Environments** : Improve security by isolating environments and using separate subscriptions, resource groups, and networks to prevent unauthorized access and reduce risks.
- **Use Sensitive Keyword** : Prevent sensitive information from being displayed in logs, state files, and other outputs by using the sensitive keyword on sensitive variables and outputs.


## Monitoring


- **Enable Logging** : Enable logging for your infrastructure resources to track changes, monitor performance, and troubleshoot issues effectively. Logs provide valuable insights into the health and behavior of your infrastructure.
- **Monitor Infrastructure** : Track performance, detect issues, and ensure availability by monitoring your infrastructure. This is crucial for identifying problems, troubleshooting issues, and optimizing performance.


##### Monitoring


Monitoring Tool Description Configuration Documentation


Azure Monitor Comprehensive monitoring solution Azure Monitor


Log Analytics Log data collection and analysis Log Analytics


Application Insights Application performance monitoring Application Insights


- **Conduct Regular Reviews** : Ensure compliance, security, quality, and reliability by regularly reviewing code, monitoring infrastructure changes, and tracking deployments.


In this guide, we're going to import some resources from Azure into OpenTofu using Azure Export for Terraform. Azure Export for Terraform (` aztfexport` ) is a tool that allows you to export your Azure resources into Terraform configuration files (` .tf` ).` aztfexport` enables you to:


- Simplify migration to OpenTofu on Azure by using a single command to migrate your resources.
- Export user-specified resources into HCL code and state.` aztfexport` enables you to specify a predetermined scope, which can be a subscription, resource group, or resource.
- Inspect your preexisting resources with all properties exposed using` aztfexport` 's read-only option to expose all configurable properties.
- Follow plan/apply workflow to migrate resources to OpenTofu. Export the HCL code and state, inspect the resources, and effortlessly integrate them into your production environment and remote backends.


## Prerequisites


- Setup completed
- Azure Export for Terraform


## Configuration and usage


By default,` aztfexport` collects telemetry data, which Microsoft aggregates to identify trends and usage patterns. To disable telemtry, run:


```text
aztfexport config  set   telemetry_enabled  false


```


The basic commands for` aztfexport` are:


Command Description


` aztfexport resource \[option\] <resource id>` Export a single resource using the Azure resource ID


` aztfexport resource-group \[option\] <resource group name>` Export all resources in a resource group using the resource group name (not ID)


` aztfexport query True` Exports all resources in the subscription


` aztfexport query \[option\] <ARG where predicate>` Export resources using an Azure Resource Graph query


## Test run


` aztfexport` supports interactive and non-interactive modes. This guide will be using interactive mode. For non-interactive mode, append` --non-interactive` to the command.


1. Create a new test directory to store the imported HCL and state file and` cd` into it.
2. Create a test resource to import using the Azure CLI:


```text
az group create --name tofu-export-test --location westus


```


```text
az vm create --resource-group tofu-export-test --name tofu-export-test-vm --image UbuntuLTS --admin-username azureuser --generate-ssh-keys --image Debian11 --public-ip-sku Standard


```


1. Import the resource group using` aztfexport` :


```text
aztfexport resource-group tofu-export-test


```


After the tool initializes (may take a few minutes), a list of resources to be imported is displayed. Each line will have an Azure resource ID matched to the resource type. There is a list of commands on the bottom of the screen.


- ↑ and ↓ arrow keys to navigate the list
- ` delete` key will skip the resource so it is not exported
- ` w` to run the import
- ` s` to save
- ` q` to quit


1. Press` w` to run the import.
2. Once the import is finished, exit the tool by pressing any key.
3. View the imported resources in your new directory using` ls` . You should see the following files:


- ` aztfexportResourceMapping.json` - A mapping of Azure resource IDs to Terraform resource names
- ` main.tf` - Contains all of your Azure resources that were imported
- ` provider.tf` - Contains the provider configuration for` azurerm`
- ` terraform.tf` - Initializes the` azurerm` provider and local state backend, and pins the version
- ` terraform.tfstate` - Contains the state of the imported resources


1. Review the imported resources and make any necessary changes.
2. Run` tofu init --upgrade` then` tofu plan` . If the terminal outputs` No changes. Your infrastructure matches the configuration.` then congratulations! You have successfully imported your resources and its state to Terraform.


## Cleanup


1. Navigate to your test directory.
2. Run` tofu destroy` then enter` yes` to the prompt to delete the resources from Azure.
