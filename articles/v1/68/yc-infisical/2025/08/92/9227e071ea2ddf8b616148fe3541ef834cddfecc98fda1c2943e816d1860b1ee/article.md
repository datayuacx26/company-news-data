---
schema_version: "1.0.0"
document_id: "9227e071ea2ddf8b616148fe3541ef834cddfecc98fda1c2943e816d1860b1ee"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/terraform-vs-opentofu"
published_at: "2025-08-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:d3e0712ddc3661645d58b0586c0968b9d4544cc210b7d9aa3b9c4b49d8733d0c"
---

# Terraform vs. OpenTofu: A Comprehensive Comparison

## Forward


[Terraform](https://developer.hashicorp.com/terraform) and[OpenTofu](https://opentofu.org/) are popular declarative Infrastructure as Code (IaC) tools—they enable developers to provision backends by describing their infrastructure in a declarative coding language. It’s similar to how database tables can be provisioned with a declarative ORM like Prisma or how Typeform runtimes are configured with a` .tsconfig` file. Notably, IaC removes the need to imperatively stage backends with API calls; instead, developers could provision servers, databases, load balancers, and even relationships from a single static file.


Terraform and OpenTofu are constantly compared because they share the same ancestor: Terraform. Specifically, they share the *last* open source version of Terraform, Terraform 1.6.x. This makes the comparison a bit … one note. Terraform and OpenTofu are virtually the *same product* . Their primary difference is their license: OpenTofu is proudly and loudly Open Source Software (OSS), while Terraform is now distributed under a Business Service License (BSL).


When Terraform *used* to be open source, it sported a[Mozilla Public License](https://www.mozilla.org/en-US/MPL/) (MPL), a OSI-compliant OSS license. Then, in 2023, Terraform’s commercial developer, HashiCorp, shifted to a BSL license that granted developers free access to its source code, but restricted devtools from selling Terraform-powered IaC products. This wasn’t received positively by many of Terraform’s fans and OSS diehards. Many flocked to OpenTofu, the most successful fork of Terraform that is supported by popular IaC products like[Gruntwork](https://www.gruntwork.io/) ,[Spacelift](https://spacelift.io/?utm_term=spacelift&utm_campaign=FY25+-+NA+-+Google+Ads+-+Search+-+Brand&utm_source=google&utm_medium=ppc&hsa_acc=6631252447&hsa_cam=20312909906&hsa_grp=151195843632&hsa_ad=693373320217&hsa_src=g&hsa_tgt=kwd-526144202295&hsa_kw=spacelift&hsa_mt=e&hsa_net=adwords&hsa_ver=3&gad_source=1&gad_campaignid=20312909906&gbraid=0AAAAACSwa055X1hpX8FBJQfUKAafpqRZ6&gclid=Cj0KCQjw18bEBhCBARIsAKuAFEa0BcpkDTDrOhjWauLeKkxsbks6sQpgrOf3LBE1OBeRjtIqQWiJQY4aAucMEALw_wcB) ,[Harness](https://www.harness.io/) ,[Env0](https://www.env0.com/) , and[Scalr](https://scalr.com/) .


## Is this duality common?


Yes and no.


The split between Terraform and OpenTofu is not an anomaly. It’s the latest casualty of an ongoing trend, where popular OSS software shifts to a non-OSS license to target competitive commercial distributions of that OSS software. For example,[Elasticsearch](https://www.elastic.co/elasticsearch) (temporarily) abandoned OSS, spawning[Opensearch](https://opensearch.org/) . MongoDB did the same, creating[FerretDB](https://github.com/FerretDB/FerretDB) .


Typically, the split happens, and the formerly open source project quietly competes with the open source fork. This is usually not a challenge for the flagship product. Typically, the OSS fork trails the flagship project in features, as the community developers typically cannot keep up with the original project.


However, Terraform and OpenTofu remain *very* similar, virtually identical—even years after the split. This interchangeability has created some tensions. In 2024, HashiCorp sent OpenTofu’s maintainers[a cease and desist letter,](https://devops.com/opentofu-denies-hashicorps-code-stealing-accusations) alleging that OpenTofu had stolen code from Terraform’s releases after the license shift. OpenTofu has denied the claim, and public evidence strongly indicated that OpenTofu was strictly using code from the last MPL release of Terraform. Naturally, the lawsuit[attracted ire](https://www.reddit.com/r/programming/comments/1c1isys/our_response_to_hashicorps_cease_and_desist) from many community developers who saw it as a stunt to scare developers away from OpenTofu.


## Licenses: How do Terraform and OpenTofu compare?


Let’s begin with the actual differences between Terraform and OpenTofu’s license. After all, some OSS projects are “OSSish” in nature with a very specific restriction.


To best understand Terraform’s new license, let’s begin with Terraform’s initial product line.


Previously, Hashicorp had *three* products: Open Source, Enterprise, and Cloud. The core features of Terraform were found in Terraform’s Open Source distribution; the more enterprise-facing features were built atop of the OSS code and available under a proprietary license. This structure is typically referred to as *open core* . Terraform’s Cloud products, meanwhile, were hosted versions of their open source and Enterprise products.


Eventually, Terraform was broadly adopted by the community, and other IaC tools encouraged developers to adopt Terraform to their default orchestration language. HashiCorp, however, wouldn’t benefit from these products as those external IaC tools were simply using the OSS distribution of Terraform. To target these cases, Hashicorp suspended Terraform’s Open Source product and launched a Community Edition under their new[Business Source License](https://www.hashicorp.com/en/bsl) (BSL or BUSL). This license permitted the community to fork the codebase, modify it, contribute to it, and deploy it on their own servers. In other words, the end users of Terraform had no new restrictions placed on them. In fact, they could continue to use a free, self-hosted version of Terraform if they desired. Rather, the license shift strictly impacted *distributors* of Terraform that were selling managed Terraform products. Now, those distributors had to purchase a special commercial license from HashiCorp to distribute Terraform.


This restriction is not considered open source by OSI because it places restrictions on the commercial use of software. OpenTofu, meanwhile, forked the last MPL release of Terraform and has since maintained a[MPL 2.0 license](https://www.mozilla.org/en-US/MPL/2.0/) . MPL 2.0 is weakly copyleft: forks of OpenTofu files must be distributed under the same open source license, but additional files could be released under a different license.


## The Features: How do Terraform and OpenTofu compare?


Beyond license differences, there are some *minor* differences between Terraform and OpenTofu’s core features given that they are developed by different teams. These differences, however, are constantly being eliminated by constant, incremental feature parity from OpenTofu. Even today, OpenTofu is considered a “drop-in replacement” to Terraform, maintaining the same conventions and syntax as the original Terraform.


Since the split, OpenTofu will update their codebase to imitate new features built by Terraform. For instance, on February 27, 2025, Terraform[introduced](https://github.com/hashicorp/terraform/releases?page=3) write-only attributes. On July 9, 2025, OpenTofu released[the same feature](https://search.opentofu.org/provider/opentofu/vault/latest/docs/guides/using_write_only_attributes) . Accordingly, it doesn’t make sense to mull on the minor differences between Terraform’s latest community edition and OpenTofu’s last trailing release, as those differences will likely be closed by future editions of OpenTofu.


Beyond features, OpenTofu can install Terraform providers. In other words, all of Terraform’s extensibility features map over to OpenTofu without hassle.


## Deployment: How do Terraform and OpenTofu compare?


Terraform and OpenTofu have virtually the same deployment options.


For Terraform, developers can either use HashiCorp’s managed solutions or entrust one of HashiCorp’s partners. These partners include Gruntwork, Env0, and SpaceLift. For OpenTofu, the same products are available. In fact,[they encourage developers to use OpenTofu over Terraform](https://www.gruntwork.io/blog/make-the-switch-to-opentofu) and remain financial backers of OpenTofu’s development.


The only difference is that some features developed by IaC tools may be temporarily pegged to recent Terraform features that aren’t (yet) available on OpenTofu.


## Secrets: How do you manage secrets on Terraform and OpenTofu?


Terraform and OpenTofu are IaC tools, not secrets managers. However, when provisioning infrastructure, secrets are necessary to configure environments. Terraform’s registry includes integrations for basic cloud secrets managers like[Infisical](https://infisical.com/) to scale secrets.


Infisical is a consolidated secrets platform that is compatible with both[Terraform](https://registry.terraform.io/providers/Infisical/infisical/latest/docs) and[OpenTofu](https://search.opentofu.org/provider/infisical/infisical/v0.15.21) . With Infisical, secrets can be[easily rotated or dynamically generated](https://infisical.com/blog/secret-rotation-vs-dynamic-secrets) . Infisical also manages certificates and certificate authorities, audits repositories for potential breaches, and is OSS like OpenTofu.


To set-up Infisical with Terraform, developers simply need to register the[Infisical Terraform](https://infisical.com/docs/integrations/frameworks/terraform) provider.


```text
terraform {
required_providers {
infisical = {
# version = <latest version>
source = "infisical/infisical"
}
}
}


provider "infisical" {
host = "https://app.infisical.com" # Only required if using self hosted instance of Infisical, default is https://app.infisical.com
auth = {
universal = {
client_id     = "<machine-identity-client-id>"
client_secret = "<machine-identity-client-secret>"
}
}
}


```


Likewise, to set-up Infisical with OpenTofu, developers can register the exact same provider given the cross-compatibility of Terraform and OpenTofu. That is, literally, the same provider—they’ll still have` terraform{}` in their OpenTofu file as it’s referencing the same endpoint.


## Branding: does Terraform or OpenTofu have a better name?


OpenTofu. Who doesn’t like Tofu?


## In Summary: how do Terraform and OpenTofu compare?


Generally speaking, OpenTofu and Terraform only differ in license. To summarize this, we’ve arranged a small cheatsheet:


However, OpenTofu has being received with open arms by the open source community; at the same time, Terraform remains a commercial powerhouse and leads the direction of OpenTofu passively.


Both tools don’t have support for robust secrets management. For that, you should rely on an (open source) platform like[Infisical](https://infisical.com/) .
