---
schema_version: "1.0.0"
document_id: "9961ada0393de8cb45b33adbc0f3743729d16916ed204bb49a5dc0b7674c1347"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/it-doesnt-have-to-be-this-hard-a-devops-journey"
published_at: "2024-02-04T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:5b8da58ad8d719037d630040ece49bfa539102b95c75271d25eccdea99d2310a"
---

# It Doesn't Have to be This Hard: A DevOps Journey

All I wanted to do was optimize my Terraform…


## The Problem


Upgrading Azure Postgres Flexible Server in Azure can be a bit of a pain. Sure, the console takes care of the process for you with 2 clicks (although sometimes that process fails). But who uses click-ops? I want to upgrade my Postgres version using Terraform. How hard could it be?


## The Journey


Ok, so I need to upgrade my Postgres version. I’m using Terraform, so I’ll just update my \[.c-highlighted\]azurerm_postgresql_flexible_server\[.c-highlighted\] resource and run terraform plan to verify. Easy peasy.


```text
resource "azurerm_postgresql_flexible_server" "main" {
name                = var.name
resource_group_name = azurerm_resource_group.example.name
location            = azurerm_resource_group.example.location
version             = "13" -> "15"
}


```


‍


And then...


```text
# azurerm_postgresql_flexible_server.main must be replaced
-/+ resource "azurerm_postgresql_flexible_server" "main" {
~ version                       = "13" -> "15" # forces replacement


```


‍


Ugh, ok, let’s find out how to upgrade the version of a Postgres server in Azure without destroying and recreating the resource. Looks like I can use \[.c-highlighted\]create_mode = "Update"\[.c-highlighted\] to update the resource instead of destroying and recreating it.


‍


However…


Without affecting my production Postgres server, I’ll spin up a dev server. I’ll set \[.c-highlighted\]` create_mode = "Default"\[.c-highlighted\]` and deploy.


```text
resource "azurerm_postgresql_flexible_server" "main" {
name                = var.name
resource_group_name = azurerm_resource_group.example.name
location            = azurerm_resource_group.example.location
version             = "13"
create_mode         = "Default"
}


```


‍


Great, that deployed. I’ll update the resource to \[.c-highlighted\]` create_mode = "Update"\[.c-highlighted\]` , update the version, and deploy again.


```text
resource "azurerm_postgresql_flexible_server" "main" {
name                = var.name
resource_group_name = azurerm_resource_group.example.name
location            = azurerm_resource_group.example.location
version             = "15"
create_mode         = "Update"
}


```


‍


Success! That seems to update the version of the Postgres server without destroying and recreating it. So, what do I do with \[.c-highlighted\]` create_mode\[.c-highlighted\]` ? What if I set it back to \[.c-highlighted\]` "Default"\[.c-highlighted\]` ?


```text
resource "azurerm_postgresql_flexible_server" "main" {
name                = var.name
resource_group_name = azurerm_resource_group.example.name
location            = azurerm_resource_group.example.location
version             = "15"
create_mode         = "Default"
}


```


‍


Nope, that deletes the resource. Ok, so I’ll just leave it as` \[.c-highlighted\]"Update"\[.c-highlighted\]` I guess. Before adding this into my production server, I’ll deploy a Postgres server identical to production and test the update process. I add the field for` \[.c-highlighted\]create_mode = "Update"\[.c-highlighted\]` and run a` \[.c-highlighted\]terraform plan\[.c-highlighted\]` .


```text
# azurerm_postgresql_flexible_server.main must be replaced
-/+ resource "azurerm_postgresql_flexible_server" "main" {
+ create_mode                   = "Update" # forces replacement


```


‍


So, I can’t add in` \[.c-highlighted\]` create_mode = "Update"` \[.c-highlighted\]` without destroying and recreating the resource. But I can’t update the version without adding in` \[.c-highlighted\]` create_mode = "Update"` \[.c-highlighted\]` . What do I do? I’ll just add in` \[.c-highlighted\]` create_mode = "Default"` \[.c-highlighted\]` and see what happens.


```text
# azurerm_postgresql_flexible_server.main must be replaced
-/+ resource "azurerm_postgresql_flexible_server" "main" {
+ create_mode                   = "Default" # forces replacement


```


‍


I need to break this down a bit…


- I need to update the version of my Postgres server without destroying and recreating it
- I can’t update the version without adding in create_mode = "Update"
- I can’t add in create_mode = "Update without destroying and recreating the resource
- I can’t add in create_mode at all without destroying and recreating the resource


The only thing I can do at this point, which is not a great experience, is to create/deploy a new Postgres server with` \[.c-highlighted\]create_mode = "PointInTimeRestore"\[.c-highlighted\]` to migrate my data from my production Postgres server to the new server with the updated version. I’ll have to update my application to point to the new Postgres server, and then destroy the old Postgres server. This is not ideal, but it’s the only way I can update the version of my Postgres server without destroying and recreating it. I’m not sure if this is a limitation of Terraform or Azure, but it’s not a great experience and will cause **downtime** .


‍


## The Painful Reality


In the DevOps landscape, we often face a paradox where tools and practices intended to streamline operations inadvertently add complexity. This complexity isn’t just about technical configurations; it’s about the burden it places on teams. Each update or change, rather than simplifying, often feels like navigating a maze, with the real cost being sidelined innovation and agility.


The industry’s push towards DevOps has sometimes resulted in oversimplifications, overlooking the intricate realities of technology and team dynamics. This leads to a scenario where ‘you build it, you run it’ becomes more of a burden than empowerment. Teams find themselves caught in a cycle of managing both development and operational demands, detracting from their core work of creating value.


As we pursue the DevOps ideal, it’s vital to focus not just on deploying and managing infrastructure but on enabling meaningful, value-driven work. This means building robust internal platforms that evolve with team and business needs, while empowering the team to self-serve. The goal should be smarter, empathetic engineering that eases the complexities of DevOps, fostering an environment where teams can do their best work with minimal friction.


‍
