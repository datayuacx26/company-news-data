---
schema_version: "1.0.0"
document_id: "77dff7563578465bd75657450ff5de5e0373ade93e9eca4a200174905fa8caae"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/the-cidr-problem-and-how-we-fixed-it"
published_at: "2023-03-06T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:e770a1e87a5f1fb5d6247df86d65379323dd79ec88917b456fa8254474ea5b7c"
---

# The CIDR problem, and how we fixed it

Who needs CIDR?Who are we kidding? We do. You likely already know that CIDR notation is a necessary evil for DevOps and operations teams. However, for those who don’t have experience in the networking field, CIDR notation may seem confusing, irrelevant, and potentially devastating when done incorrectly.


What is CIDR notation?CIDR notation allows us to express a range of IP addresses using a single number, called the subnet mask. The subnet mask tells us how many of the IP address bits are used to represent the network and how many bits are used to represent the host. Understanding CIDR notation is essential for managing networking infrastructure and ensuring secure communication between services. A typical CIDR configuration may look like this:


```text


VPC - 10.0.0.0/16 (65k addresses)
Subnet1 - 10.0.128.0/18 (16k addresses)
Subnet2 - 10.0.192.0/18 (16k addresses)
(33k addresses unallocated)


```


‍


If you know CIDR math and this doesn’t phase you, great! But, what if this doesn’t make much sense to you? Some tools and services ease some of the burdens of networking. Cloud providers such as AWS, Azure, and GCP offer services that handle most network configurations and routing decisions, which helps. However, developers and DevOps professionals are still stuck with making moderately challenging decisions regarding their VNets or VPCs, such as picking a subnet mask.


Why should I care?In a perfect world, you shouldn’t have to care. Unfortunately, with the current state of DevOps, teams with little-to-no network experience are left making critical decisions about the networks they run their applications on. This can be a daunting task, as it requires a deep understanding of networking concepts and infrastructure, which may be beyond the scope of a typical developer. Using tools and services that abstract away the complexities of networking, you can focus on building and deploying applications without worrying about the intricacies of CIDR notation.


The tool for the jobCue Massdriver’s new Auto-CIDR feature. We have recently released a new Auto-CIDR utility feature in our massdriver-cloud Terraform provider . This feature automatically selects a non-conflicting CIDR range for the resource, based purely on the number of IP addresses you want, making it even easier to manage your networks without worrying about IP address conflicts or overlaps. Not having to worry about selecting the right CIDR notation will help streamline the development process, reduce the likelihood of errors, and increase productivity.


How do I use it?If you’re using Massdriver as your cloud platform, the Auto-CIDR feature is already baked into the bundles that need it. Don’t want us to select your CIDR ranges for you? No problem, easily disable the feature within the bundle to manually set your own CIDR.


If you’d like to use our Auto-CIDR feature in your own Terraform, that’s cool too! Check out our massdriver-cloud utility provider . Here is an example of how to use Auto-CIDR to create an Azure VNet:


```text


terraform {
required_version = ">= 1.0"
required_providers {
utility = {
source  = "massdriver-cloud/utility"
version = "~> 0.0"
}
jq = {
source  = "massdriver-cloud/jq"
version = "~> 0.0"
}
azurerm = {
source  = "hashicorp/azurerm"
version = "~> 3.0"
}
}
}


data "http" "token" {
url    = "https://login.microsoftonline.com/${var.tenant_id}/oauth2/token"
method = "POST"


request_body = "grant_type=Client_Credentials&client_id=${var.client_id}&client_secret=${var.client_secret}&resource=https://management.azure.com/"
}


data "http" "vnets" {
url    = "https://management.azure.com/subscriptions/${var.subscription_id}/providers/Microsoft.Network/virtualNetworks?api-version=2022-07-01"
method = "GET"


request_headers = {
"Authorization" = "Bearer ${jsondecode(data.http.token.0.response_body)}"
}
}


data "jq_query" "vnet_cidrs" {
data  = data.http.vnets.0.response_body
query = "[.value[].properties.addressSpace.addressPrefixes[]]"
}


resource "utility_available_cidr" "cidr" {
from_cidrs = ["10.0.0.0/8", "172.16.0.0/20", "192.168.0.0/16"]
used_cidrs = jsondecode(data.jq_query.vnet_cidrs.0.result)
mask       = var.vnet_mask // desired size of the VNet
}


resource "azurerm_virtual_network" "main" {
name                = var.vnet_name
resource_group_name = azurerm_resource_group.main.name
location            = azurerm_resource_group.main.location
address_space       = [utility_available_cidr.cidr.0.result]
}


```


And here's an example of how to add a subnet to an existing Azure VNet:


```text


terraform {
required_version = ">= 1.0"
required_providers {
utility = {
source  = "massdriver-cloud/utility"
version = "~> 0.0"
}
azurerm = {
source  = "hashicorp/azurerm"
version = "~> 3.0"
}
}
}


data "azurerm_virtual_network" "lookup" {
name                = var.vnet_name
resource_group_name = var.vnet_resource_group
}


data "azurerm_subnet" "lookup" {
for_each             = toset(data.azurerm_virtual_network.lookup.subnets)
name                 = each.key
virtual_network_name = var.vnet_name
resource_group_name  = var.vnet_resource_group
}


resource "utility_available_cidr" "cidr" {
from_cidrs = data.azurerm_virtual_network.lookup.address_space
used_cidrs = flatten([for subnet in data.azurerm_subnet.lookup : subnet.address_prefixes])
mask       = var.subnet_mask // desired size of the subnet
}


resource "azurerm_subnet" "main" {
name                 = var.subnet_name
resource_group_name  = var.vnet_resource_group
virtual_network_name = var.vnet_name
address_prefixes     = [utility_available_cidr.cidr.result]
}


```
