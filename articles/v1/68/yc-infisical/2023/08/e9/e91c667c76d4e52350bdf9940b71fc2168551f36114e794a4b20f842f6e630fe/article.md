---
schema_version: "1.0.0"
document_id: "e91c667c76d4e52350bdf9940b71fc2168551f36114e794a4b20f842f6e630fe"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/hashicorp-new-bsl-license"
published_at: "2023-08-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:f36ee735c3c272236da8c850a599bb2f4087d2d77e361aca421500f47810e40f"
---

# Hashicorp's New BSL License – What Changed?

HashiCorp, the company known for creating developer tools like Vault, Terraform, and Nomad, has recently made a big announcement. As reported in[the company blog](https://www.hashicorp.com/blog/hashicorp-adopts-business-source-license) today, it is giving up its previous MPL 2.0 (Mozilla Public License v2.0) in favor of a more restrictive BSL v1.1 (Business Source License, also known as BUSL). In other words, as explained in the official statement, HashiCorp does not consider itself an open source company anymore. Instead of "open source", they will start calling this version of their product "community".


After this announcement, we have had many companies reaching out to[Infisical](https://infisical.com/) with intentions of moving away from HashiCorp's products. For this reason, we decided to write this guide and answer the most common questions.


## What is BSL V1.1?


The Business Source License (BSL) offers an alternative to both closed source and open source licensing approaches. With BSL, the source code is accessible to the public (also known as "source available"). Non-production usage of the code remains free, and the licensor has the option to provide an Additional Use Grant, permitting production use with specific limitations. Importantly, the source code is guaranteed to transition into an open source status at a designated point in time. This transition occurs either on a specified Change Date or upon the fourth anniversary of the initial public release of the code under the BSL, depending on which event comes first. This automatic transition leads to the code becoming accessible under the Change License. Presently, HashiCorp's projects are governed by the MPL 2.0 as the Change License.


Note: BSL v1.1 (Business Source License) should not be confused with the[Boost Software License](https://www.boost.org/users/license.html) which is much more permissive and does not enforce many restrictions besides the fact that it requires that the same license appears with all copies \[including redistributions\] of the software source code.


## When can you use Hashicorp products for free?


All non-production uses are permitted. All production uses are allowed other than hosting or embedding the software in an offering competitive with HashiCorp products or services.


## What is considered a competitive offering?


Hashicorp is being particularly vague about this – not naming exact scenarios of non-competing use cases. As for their definition, competitive offering implies` a product or service provided to users or customers outside of your organization that has significant overlap with the capabilities of HashiCorp’s commercial products or services` .


At the first glance, this might sound completely innocent. However, as Sid Sijbrandij, co-founder and CEO of GitLab, pointed out, company scopes change – what might not be competitive right now may become competitive in future if HashiCorp decides so. Not having particular use cases specified by HashiCorp leaves them a lot of wiggle room for future. This is the risk that a lot of companies are not willing to take. In fact, historically, many companies have internally outright banned similar licenses such as SSPL.


Even worse, imagine your company has a freelancer that sets up and maintains a Vault or your infrastructure through Terraform. Now, this person is officially considered a competitor to HashiCorp because HashiCorp offers paid professional services too.


## Workarounds


It should be noted, that this change affects only the future releases of HashiCorp products. There have been examples of companies who have decided to freeze their version's of HashiCorp products (e.g.,[Rivet](https://rivet.gg/) ) – these versions will still be under the previous MPL 2.0 license.


## Competitive Options


If you are looking for alternatives to HashiCorp Vault, you may find this article on[the best secret management tools in 2024](https://infisical.com/blog/best-secret-management-tools) helpful.


[Infisical](https://infisical.com/) is an open source alternative to HashiCorp Vault. The main difference is that Infisical provides multiple tools inspired by the` security shift left` trend. For example, outside of all the traditional secret management features, you are able to perform secret scanning and continuous secret leak prevention. On top of that, Infisical provides automatic integrations with services like GitHub Actions, Vercel, and other secret managers, while giving a simple dashboard, CLI for auto-injecting secrets based on permissions, and much more! You can find the full comaprison of Infisical and vault[here](https://infisical.com/infisical-vs-hashicorp-vault) .


If you have any questions, you can[signup here](https://app.infisical.com/signup) or[talk to an expert](https://infisical.com/talk-to-us) .


## Looking for alternatives?


Besides license change, there are many other reasons why developers might be looking into switching of Hashicorp's tools like Vault. The reasons include[HashiCorp Vault being very expensives](https://infisical.com/blog/hashicorp-vault-pricing) , very hard to setup and maintain, and incompatibility with certain use cases. If you are looking for[alternatives to HashiCorp Vault](https://infisical.com/blog/hashicorp-vault-alternatives) , you should consider the[top secret management tools in 2024](https://infisical.com/blog/best-secret-management-tools) . Our[Vault TCO calculator](https://infisical.com/vault-tco-calculator) can also help you estimate what a move away from Vault would actually save.
