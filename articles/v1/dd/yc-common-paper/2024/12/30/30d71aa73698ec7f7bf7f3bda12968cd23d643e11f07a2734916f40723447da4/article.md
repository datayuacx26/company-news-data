---
schema_version: "1.0.0"
document_id: "30d71aa73698ec7f7bf7f3bda12968cd23d643e11f07a2734916f40723447da4"
company_key: "yc-common-paper"
company: "Common Paper"
source_id: "yc-common-paper-rss-3b76f5ee637c"
canonical_url: "https://commonpaper.com/blog/baa-agreement/"
published_at: "2024-12-06T14:10:00+00:00"
first_seen_at: "2026-07-20T23:20:15.555416+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:575e8633059367e59844914cf839f43de00cfadfe3ca815bb16cfa528ccb0e01"
---

# BAA and Underlying Agreements

The Business Associate Agreement is one of the least-well-understood contracts that we deal with, and one of the most common questions we get is about the Underlying Agreement. I’m writing this post to help explain what you need to know about BAAs and how the underlying agreement is handled in Common Paper.


## The Basics: BAAs and HIPAA


First, some terminology. In the US, there’s a law called the[Health Insurance Portability and Accountability Act](https://www.cdc.gov/phlp/php/resources/health-insurance-portability-and-accountability-act-of-1996-hipaa.html#:~:text=The%20Health%20Insurance%20Portability%20and,from%20disclosure%20without%20patient's%20consent.) (HIPAA). Among other things, this law governs how[protected health information](https://cphs.berkeley.edu/hipaa/hipaa18.html) (PHI) is handled when covered entities share PHI with a business associate.


HIPAA requires that a[Business Associate Agreement](https://commonpaper.com/standards/business-associate-agreement/) (BAA) is signed between the covered entity and its business associate, and between business associates and their business associates. This agreement supplements the main contract between those parties, and covers how compliance with HIPAA will be maintained. You probably haven’t needed a BAA before if neither you nor your customer are subject to HIPAA and deal with PHI.


In most software vendor relationships where a BAA is required, the basic outline of roles is as follows:


- The customer is the “covered entity” (bound by HIPAA)
- The vendor is the “business associate” (handling PHI on behalf of the covered entity)
- The BAA sits on top of whatever agreement governs the relationship between the vendor and the customer, which is usually some kind of sales contract, or what we call the “underlying agreement”.


Another variation is when the customer is a business associate itself, so the covered entity is one additional step removed from the vendor.


It’s important to note that the BAA is only one aspect of HIPAA compliance. There are also rules about how you handle PHI and related data, but those are out of scope for this post.


# **What Counts as an Underlying Agreement?**


Here’s where people often get stuck. The most common types of underlying agreements for BAAs signed in Common Paper are:


- [Cloud Service Agreement](https://commonpaper.com/standards/cloud-service-agreement/)
- [Software License Agreement](https://commonpaper.com/standards/software-license-agreement/)
- [Professional Services Agreement](https://commonpaper.com/standards/professional-services-agreement/)
- [Design Partner Agreement](https://commonpaper.com/standards/design-partner-agreement/)


Pro tip: Most software companies have some kind of agreement in place with their customers. Even if it’s not a signed contract, a[Terms of Service](https://commonpaper.com/standards/terms-of-service/) that customers agree to during signup would count.


# **Handling the Underlying Agreement in Common Paper**


You have two main options:


1) Select the underlying agreement from the dropdown


- If the agreement is already in Common Paper, just pick it from the menu
- If you’ve a signed an agreement but it’s not in Common Paper, upload a PDF and then it will appear as an option in your dropdown


2) Describe the agreement in free text


- Sometimes there might not be a signed PDF, like if your terms are posted online. Or perhaps it will be challenging for you to get the PDF and upload it into Common Paper.
- If they are posted online, describe them it as something like: “Terms of Service posted at yourcompany.com/terms, last updated 11/7/23”
- If it’s a custom contract executed separately, describe it as something like “Master Service Agreement between Vandelay Industries, Inc. and Cramblin Duvet LLC, signed on 12/15/23”


# **Why This Matters**


Getting your contracts in a good place today can save you a lot of headaches down the road. This is a good rule of thumb, but it’s especially important if you’re dealing with highly regulated data like PHI.


Here’s why:


- Investors check on your contracts during due diligence if you raise venture capital
- Acquirers want to make sure they are not inheriting any landmines hidden in your contracts
- It protects both parties if things go sideways (downtime, bugs, disputes)


You might be thinking, “We’re just getting started – do we really need all this?” The answer is that you need something, and the specifics depend on the details of your company, customers, and priorities. Even a basic Terms of Service is better than nothing.


Common Paper provides free,[open source standard contracts](https://commonpaper.com/standards/) as well[tools for understanding and managing your contracts](https://commonpaper.com/product/) . If you need help to go deeper or figure out the right solution for your specific situation, reach out and we can connect you with an attorney.
