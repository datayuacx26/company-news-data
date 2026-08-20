---
schema_version: "1.0.0"
document_id: "759963b10752101817e0657f7c725f7a9fdcb3e3bfd7036edea84379526d2eb0"
company_key: "everpure-inc"
company: "Everpure Inc."
source_id: "everpure-inc-rss-a7fca946ec64"
canonical_url: "https://blog.everpuredata.com/news-events/announcing-general-availability-of-everpure-cloud-vm-extension/"
published_at: "2026-07-07T13:00:00+00:00"
first_seen_at: "2026-07-25T03:30:11.662383+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:db6f531ede4dc4d72c0d241ff25c69e9ff68a58b6b59305632bdfbf70a8fea1e"
---

# Simplify Azure VM Storage with Everpure Cloud VM Extensions- Now Generally Available

### Summary


Now generally available, the Everpure Cloud VM Extension for Azure VMs automates storage setup and volume mounts for faster, more consistent deployments.


External block storage for Azure Virtual Machines (VMs) gives organizations more flexibility in how they scale performance and capacity. But anyone who has deployed iSCSI-based storage for virtual machines also knows where friction usually shows up: host-side configuration. Getting everything set correctly on the guest OS can mean manual steps, post-deployment scripts, and too much room for drift.


Today, we’re announcing the general availability of the Everpure Cloud VM extension for Azure VMs, available across Azure regions. The extension automates external storage configuration, simplifies volume attachment workflows, and helps connect Everpure Cloud storage to Azure VMs faster and more consistently.


The extension supports streamlined volume mount workflows for[Everpure Cloud Azure Native](https://www.everpuredata.com/docs.html?item=/type/pdf/subtype/doc/path/content/dam/pdf/en/solution-briefs/sb-azure-vmware-powered-by-pure.pdf) and simplifies host-side setup for[Everpure Cloud Dedicated](https://www.everpuredata.com/docs.html?item=/type/pdf/subtype/doc/path/content/dam/pdf/en/solution-briefs/sb-pure-storage-cloud-dedicated.pdf) by automating guest OS configuration and storage mounting on Azure VMs.


## **Remove the manual work from storage for Azure VMs setup**


For many organizations, attaching any external storage to Azure VMs involves runbooks with manual steps or PowerShell/bash scripts to configure the operating system. While effective, these steps can slow deployments, introduce configuration drift, and increase operational overhead. That often means configuring iSCSI services, enabling and tuning MPIO, setting target connectivity, and making sure everything is aligned with vendor best practices. The process works, but it takes time, introduces variability, and creates yet another place where operational mistakes can happen.


The Everpure Cloud VM Extension changes that model. Instead of relying on manual configuration or custom scripting, you can use the extension to move more quickly to a validated, best-practice configuration. The goal is simple: make external storage mounting more seamless, repeatable, and resilient to human error when deploying Azure VMs.


With a single deployment, the extension automatically configures iSCSI, enables and optimizes MPIO, establishes target connectivity, and applies Everpure-recommended best practices. This eliminates repetitive setup tasks and helps ensure consistent storage connectivity across Azure VM deployments.


## **Built to follow best practices automatically**


Automation is useful only when it reflects the right operational model. The value of the Everpure Cloud VM Extension is not just that it saves clicks. It’s that it applies guest OS configuration according to Everpure best practices, so you do not have to reinvent the same setup steps for each VM or each workload.


This matters for repeatability, especially in larger environments where multiple teams or deployment pipelines may be involved. Standardizing how services such as iSCSI and MPIO are configured helps reduce inconsistency across hosts and makes it easier to onboard new workloads with a predictable storage setup.


The extension can be deployed during new VM provisioning or added to an existing running Azure VM resource, giving teams flexibility in how they introduce the capability into current and future environments. It also supports both Linux and Windows VMs, making it applicable across a broad set of enterprise workload patterns.


## **Full infrastructure-as-code support**


Modern cloud operations depend on repeatable deployment patterns, not one-off manual configuration. The Everpure Cloud VM Extension was designed with that in mind.


You can automate deployment of the extension through ARM templates, Azure Bicep, and Terraform. That makes it straightforward to include storage connectivity and guest OS configuration directly in VM deployment pipelines, golden templates, or broader environment provisioning workflows.


For teams building standardized landing zones or CI/CD-driven infrastructure, this is an important step forward. Storage attachment no longer needs to be treated as a separate post-deployment task. It can become part of the same declarative workflow used to deploy the virtual machine itself.


## **One extension, two Everpure Cloud experiences**


The Everpure Cloud VM Extension supports both Everpure Cloud Azure Native and Everpure Cloud Dedicated. That gives you a common way to simplify VM storage setup across the two deployment models while preserving the experience that best fits each environment and operating model.


For Everpure Cloud Azure Native, the extension delivers the simplest storage attachment experience yet. You can select an Azure VM directly from the Azure Portal and use a one-click mount workflow that automatically configures the guest operating system and mounts storage volumes.


*Figure 1: The Everpure Cloud VM Extension automates Azure VM storage setup and volume mounting, reducing manual configuration and speeding deployment.*


For Everpure Cloud Dedicated the extension automates the guest-side work that previously depended on manual configuration and/or scripting, and makes external storage for Azure VMs easier to adopt at scale.


## **A simpler path to external storage for Azure VMs**


As more organizations look for ways to modernize Azure-based workloads without giving up enterprise storage capabilities, operational simplicity becomes just as important as raw functionality. External storage can unlock flexibility, but only if the day-to-day experience is easy to deploy, automate, and maintain.


The general availability of the Everpure Cloud VM Extension is an important milestone because it removes a common operational barrier. It helps eliminate manual host-side setup, supports full infrastructure-as-code deployment patterns, applies configuration according to best practices, and delivers a streamlined one-click setup experience in Everpure Cloud Azure Native.


For customers using Everpure Cloud with Azure VMs, that means less scripting, less guesswork, and a faster path from infrastructure deployment to usable storage.


## **Get started with the Everpure Cloud VM Extension**


Ready to stop configuring external storage on your Azure VMs manually? Get started today by following the setup guide for your environment:


- [Everpure Cloud Dedicated](https://support.everpuredata.com/r/everpure-cloud-dedicated-azure/pscd-vm-extensions) **:** Learn how to automate your guest-side setup with step-by-step instructions.
- **[Start Your Free Trial of Everpure Cloud Azure Native](https://marketplace.microsoft.com/en-us/product/saas/purestoragemarketplaceadmin.psc_contact_me?tab=Overview&utm_theater=all&utm_region=all&utm_campaign=oth&utm_medium=qrcode&utm_source=blog&utm_content=other)**


***Note:*** *Everpure Cloud was formerly called Pure Storage Cloud. We’re currently updating our portals, release notes, and related materials to reflect the new name. Updates will be rolled out soon.*


## **Everpure Cloud Azure Native**


Start your free trial of Everpure Cloud Azure Native


[Sign up here](https://marketplace.microsoft.com/en-us/product/saas/purestoragemarketplaceadmin.psc_contact_me?tab=Overview&utm_theater=all&utm_region=all&utm_campaign=oth&utm_medium=qrcode&utm_source=blog&utm_content=other)
