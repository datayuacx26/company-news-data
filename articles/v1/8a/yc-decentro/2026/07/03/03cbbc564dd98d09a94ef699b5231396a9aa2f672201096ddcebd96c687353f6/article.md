---
schema_version: "1.0.0"
document_id: "03cbbc564dd98d09a94ef699b5231396a9aa2f672201096ddcebd96c687353f6"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-news-import-cdeb59691175"
canonical_url: "https://docs.decentro.tech/changelog/flow-advisory-autopay-mandates-version-111"
published_at: null
first_seen_at: "2026-07-25T01:33:54.013008+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:5c917f816463530c0f25375a2d59bbb72307229e3ec601957a0425ba908320a9"
---

# Flow Advisory | Autopay Mandates | Version 1.1.2

[Back to All](https://docs.decentro.tech/changelog)


added


##


Advisory: Important Update - UPI AutoPay Interoperability


**Deployment Date** : July 1st, 2026


This is to inform you of an important update to UPI AutoPay mandates on our platform, in line with NPCI's directive on **UPI AutoPay Interoperability** , effective **July 1st, 2026.**


**What is UPI AutoPay Interoperability?**


NPCI has introduced UPI AutoPay Interoperability to enhance the mandate execution experience for end users.
This allows mandates to be executed across payment gateways and provides end users with visibility of their mandates across UPI apps.
As part of this, NPCI has introduced a new purpose code **AZ** replacing the earlier purpose code **14** .


**What is changing on our platform?**
We will be handling all r **equired changes at our end** . Here is a summary of what changes on July 1st:


1.


**New Mandates**
All new UPI AutoPay mandates registered on or after July 1st 2026 will be automatically registered with purpose code **AZ** . No changes are required on your end for new mandate registrations.


2.


**Existing Mandates - Created via Intent Flow**
Existing mandates created by the payer through the intent flow cannot be migrated to purpose code **AZ** as per current NPCI and provider guidelines. These mandates will continue to execute as before on the originating bank. Interoperability will not be available for these mandates. There is no impact on execution for your end users - debits will continue to work normally.


**What do you need to do?**


All changes are being handled at our end. Your existing API integration requires no modifications.


**Timeline Summary :**


1. New mandates registered with purpose code AZ - 1st July 2026
2. Intent mandates - no change, continue on purpose code 14


If you have any questions about this change or how it impacts your integration, please reach out to us.
*Please note that you may experience some temporary issues during this deployment. Our team is actively monitoring and will address any concerns promptly to ensure a smooth transition.*


---


##


Advisory: Update - UPI AutoPay Interoperability Deployment Date Revised to July 25th, 2026


Further to our advisory dated 30th June 2026 regarding UPI AutoPay Interoperability, please note that NPCI has revised the effective date from July 1st, 2026 to July 25th, 2026.


**What this means:**


The rollout of purpose code AZ for new UPI AutoPay mandates, originally scheduled for July 1st, 2026, will now take effect from July 25th, 2026.
Until July 25th, 2026, all new UPI AutoPay mandates will continue to be registered under the existing purpose code 14, as per current process.
There is no change to how existing intent-flow mandates are handled - these will continue on purpose code 14 as communicated earlier, with no impact on execution.


**What you need to do:**


No action is required on your end.
This is a date revision only; all other details from our previous advisory - including the fact that changes are being handled entirely at our end and your existing API integration requires no modification - remain unchanged.


We will keep you posted as per the latest cicular/communications with NPCI.
