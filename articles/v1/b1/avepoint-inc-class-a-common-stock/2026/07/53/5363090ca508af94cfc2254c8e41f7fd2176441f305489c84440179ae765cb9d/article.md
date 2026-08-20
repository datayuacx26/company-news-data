---
schema_version: "1.0.0"
document_id: "5363090ca508af94cfc2254c8e41f7fd2176441f305489c84440179ae765cb9d"
company_key: "avepoint-inc-class-a-common-stock"
company: "AvePoint Inc."
source_id: "avepoint-inc-class-a-common-stock-news-import-1c9c9e9520bc"
canonical_url: "https://www.avepoint.com/blog/manage/august-2026-updates-microsoft-ews-retirement"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-28T15:21:06.855246+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:d7b315e32567b773a79b0afab37c7dfa8f5427ec119f2952e04c0a686ef1e634"
---

# Microsoft EWS Retirement: AvePoint's August 2026 Updates for Fly, Cloud Backup for Microsoft 365, and Fly Server

Microsoft's retirement of Exchange Web Services (EWS) in Exchange Online is entering its final phase, and AvePoint's **August 2026 releases** help customers continue the transition to Microsoft Graph. Beginning with these releases, AvePoint’s **Fly** and **Cloud Backup for Microsoft 365** will use Microsoft Graph by default for supported Exchange Online operations, with a corresponding **Fly Server** update following later in the month.


For most customers, Graph becomes the default with no change to their day-to-day AvePoint experience. Customers with a confirmed, temporary need to remain on EWS will have a limited transition path — but the steps differ by product. EWS ends permanently on **April 1, 2027** , with a recommended cutover back to Graph by the **end of February 2027** to allow for a safe changeover.


## **Microsoft's EWS Retirement Timeline**


Microsoft's October 2026 date marks the **beginning of a phased, tenant-by-tenant disablement** — not a single simultaneous cutoff:


- **October 1, 2026** : Microsoft begins phased EWS disablement across Exchange Online tenants.


- **October 1, 2026** : Microsoft also begins blocking EWS for mailboxes licensed **only** with Exchange Online Kiosk, Microsoft 365 and Office 365 F1, or Microsoft 365 and Office 365 F3, because those licenses don't include EWS rights. Affected requests return an **HTTP 403** response.


- **April 1, 2027:** EWS becomes **fully and permanently unavailable** , with no option for an administrator to restore access.


This retirement applies to **Exchange Online only** . EWS in on-premises Exchange Server is not part of Microsoft's retirement.


## **August 2026 Release Schedule: AvePoint Online Services**


The dates below are the **AvePoint Online Services (AOS) environment** release dates — that is, when the update reaches the AvePoint platform where your product runs. **These AOS environments are not the same as your Microsoft 365 tenant type** :


### AvePoint Fly and Cloud Backup for Microsoft 365


**AOS Environment**


**Release Date**


AOS Commercial **August 9, 2026**


AOS Government **August 16, 2026**


AOS FedRAMP **August 23, 2026**


### AvePoint Fly Server


- **August 28, 2026**


## **Important: AOS Environments vs. Microsoft 365 Tenant Types**


It's important not to confuse the **AOS environment** (the AvePoint platform where your product is hosted) with your **Microsoft 365 tenant type** (Commercial, GCC, GCC High, or 21Vianet). They are separate things, and the availability of Microsoft Graph depends on your **Microsoft 365 tenant type** — regardless of which AOS environment you use.


**Microsoft Graph is not yet available for GCC, GCC High and 21Vianet Microsoft 365 tenants.** As a result, mailboxes in **GCC** , **GCC High, and 21Vianet Microsoft 365 tenants will continue to use EWS** even after the August release, no matter which AOS environment manages them. AvePoint will transition these tenant types to Microsoft Graph once Microsoft makes Graph available for them. If you operate in these Microsoft 365 tenant types, plan to remain on EWS for now and follow Microsoft's guidance on the tenant-level requirements below.


## **What Changes for AvePoint Fly**


After your applicable August AOS release, **Fly uses Microsoft Graph by default** for supported Exchange Online mailbox migrations in Graph-enabled Microsoft 365 tenants. Customers do not need to take a manual step to select Graph.


**With AvePoint Fly, you have full control over whether to use EWS or Graph.** That control comes from two settings working together:


1. The **Microsoft 365 tenant-level EWS setting** you control in Exchange Online


2. A **Customized Feature parameter** in the Fly migration policy that tells Fly which API to use:


```text
UseGraphMode=False
```


Because Fly migrations are discrete jobs, you can set this parameter on the applicable migration policy whenever you need to, giving you flexibility to move between EWS and Graph. Note that the parameter alone does not preserve EWS access; Exchange Online must also permit the AvePoint application to use EWS (see the tenant requirements below).


## **What Changes for AvePoint Fly Server**


AvePoint will release the Fly Server update on **August 28, 2026** . After you install it, Fly Server uses Microsoft Graph by default for supported Exchange Online migrations in Graph-enabled Microsoft 365 tenants. **Like Fly, Fly Server gives you full control** . You manage the Microsoft 365 tenant-level EWS setting, and you use the same migration-policy parameter to choose the API:


```text
UseGraphMode=False
```


Plan to install the August 28 release before Microsoft's October 1 phased disablement begins.


## **What Changes for AvePoint Cloud Backup for Microsoft 365**


Following the August release, AvePoint transitions Cloud Backup for Microsoft 365 from EWS to Graph in **controlled batches** , beginning **August 9** and completing by **mid-September (targeting September 15)** — ahead of Microsoft's October 1 phased disablement.


## **Known Microsoft Graph Gaps: Microsoft 365 Group Mailboxes, Archive Mailboxes, and Public Folders**


Microsoft Graph does not yet support **Microsoft 365 Group mailboxes, archive mailboxes, or public folders** . Microsoft has publicly acknowledged these on its EWS deprecation roadmap, which lists mailbox import/export as being *"in preview (except Microsoft 365 Groups and public folder mailboxes)"* and lists *"Import and export of Microsoft 365 Groups"* and *"Import and export of public folders"* as prioritized work still to be completed. This was confirmed when the Graph mailbox import/export APIs reached general availability, covering primary and shared mailboxes only and explicitly excluding archive mailboxes, public folders, and Group mailboxes.


Because of these gaps, **Fly, Fly Server, and Cloud Backup for Microsoft 365 will continue to process Microsoft 365 Group mailboxes, archive mailboxes, and public folders via EWS — even for customers who have otherwise switched to Microsoft Graph.** Once Microsoft delivers Graph parity for each type, AvePoint will switch those operations from EWS to Graph.


This is subject to Microsoft's EWS retirement timeline. If you have these mailbox types in scope, continuing EWS use after **October 1, 2026,** requires the tenant-level EWS configuration described below, and EWS is **permanently disabled on April 1, 2027** . Microsoft has not published dates for closing these gaps, so if you rely on Group mailboxes, archive mailboxes, or public folders, we recommend contacting AvePoint Support to plan accordingly.


## **Temporarily Extending EWS Through March 31, 2027**


**Recommended cutover: Switch back to Microsoft Graph by the end of February 2027.** If you extend EWS past October, do **not** plan to run on EWS up to the final April 1, 2027, shutdown. Switching from EWS to Graph is **not instantaneous** – it cannot pick up mid-stream during an in-progress migration or during an initial backup – and requires lead time to complete the changeover. **We strongly recommend completing your switch from EWS to Graph by the end of February 2027** to leave enough buffer before Microsoft's permanent shutdown.


Extending EWS is **not automatic** and requires configuration in your Microsoft 365 tenant. How you exercise the extension differs by product, as described in the product sections above.


### **Why You Need Lead Time Before April 2027**


The EWS-to-Graph switch takes effect on new operations, not retroactively on ones already running:


- **Fly and Fly Server:** If a migration to or from Exchange Online is already in progress on EWS, switching to Graph will not seamlessly resume that migration mid-stream. Plan the switch around your migration cycle so it doesn't interrupt an active job.


- **Cloud Backup for Microsoft 365:** The switch is especially sensitive because it converts your incremental backup state (EWS sync token to Graph delta token) in a single forward step that must occur while EWS is still functioning. It cannot be reversed, and it cannot occur partway through an initial full backup. Allow time for this changeover to complete cleanly.


Because of this, February 2027 is the practical deadline to begin your cutover — not April.


### Microsoft 365 Tenant Requirements


Microsoft introduced the


` **EWSAllowedAppIDs**


` organization setting to control which Entra applications may keep using EWS. For a controlled, application-specific extension:


- ` **EWSEnabled=True**


` at the Exchange Online organization level, **and**


- ` **EWSAllowedAppIDs**


` populated with the approved AvePoint application ID(s).


Timing matters:


- **Before October 1, 2026:** If


` EWSEnabled=True


` but the allow list is empty, EWS still works.


- **After October 1, 2026:** You **must** populate the allow list, or EWS will fail; enabling the tenant flag alone is not enough.


This App ID–based control is separate from Microsoft's older user-agent–based EWS allow list.


## **Important Limitation for Kiosk, F1, and F3 Mailboxes**


The extension **does not override Microsoft's licensing rules.** Starting **October 1, 2026** , Microsoft blocks EWS for mailboxes licensed only with Exchange Online Kiosk, Microsoft 365 and Office 365 F1, or Microsoft 365 and Office 365 F3, returning **HTTP 403** unless a license with EWS rights is assigned.


For these mailboxes, keeping EWS enabled will not bypass the restriction — AvePoint may attempt the EWS request, but Exchange Online rejects it. Move these mailboxes to Microsoft Graph before October 1, 2026.


## **What Happens on April 1, 2027**


On April 1, 2027, Microsoft **fully and permanently disables** EWS in Exchange Online. At that point:


- ` EWSEnabled=True


` no longer restores access.


- ` EWSAllowedAppIDs


` no longer preserves access.


- AvePoint configurations cannot override the shutdown.


- Any remaining EWS-dependent operation will fail.


- Because the EWS-to-Graph switch is not instantaneous and cannot resume an in-progress migration or initial backup mid-stream, waiting until close to this date risks failed or incomplete operations. **Complete your cutover by the end of February 2027.**


Treat any temporary exception as a short-term migration window — not a long-term alternative to Graph.


## **What Customers Should Do**


**For most customers moving to Microsoft Graph:**


- Complete any requested Graph permissions and AvePoint app reauthorization.


- Review the release date for your AOS environment and plan your Fly Server install for August 28.


- If your Microsoft 365 tenant is GCC, GCC High, or 21Vianet, understand you'll remain on EWS until Microsoft enables Graph for those tenant types.


**Fly / Fly Server customers needing more time:**


- Confirm affected mailboxes have licenses with EWS rights (Kiosk/F1/F3 will not qualify after October 1).


- Configure the Microsoft 365 tenant EWS settings, and add


` UseGraphMode=False


` to the applicable migration policy.


- Plan to **complete your switch back to Graph by the end of February 2027** — not April — so an in-progress migration is not interrupted.


**Cloud Backup for Microsoft 365 customers needing more time:**


- **Contact AvePoint Support and configure your Microsoft 365 tenant before August 9, 2026,** so we can enable the EWS extension for your environment.


- Remember the switch to Graph is **one-way** — there is no supported path to revert to EWS once it occurs.


- Plan to **complete your switch back to Graph by the end of February 2027** , allowing time for the changeover to finish before the April 1 shutdown.


**Customers with Group mailboxes, archive mailboxes, or public folders in scope:**


- Understand that these mailbox types continue on EWS until Microsoft closes each Graph gap.


- If you need them after October 1, 2026, ensure your tenant EWS settings are configured, and contact AvePoint Support to plan.


## **Moving Forward with Microsoft Graph**


AvePoint's August releases are designed to move customers to Microsoft Graph before Microsoft begins disabling EWS across Exchange Online tenants. For most customers, the transition happens through the normal AvePoint release process. Customers with a legitimate need for additional time have a temporary exception path — subject to Microsoft licensing, Microsoft 365 tenant type, Exchange Online configuration, the recommended end-of-February 2027 cutover, and the April 1, 2027 final shutdown.


AvePoint will continue to provide clear, product-specific guidance as the transition progresses and as Microsoft delivers new Graph capabilities. If you have questions about your specific environment, contact AvePoint Support or your AvePoint account team.
