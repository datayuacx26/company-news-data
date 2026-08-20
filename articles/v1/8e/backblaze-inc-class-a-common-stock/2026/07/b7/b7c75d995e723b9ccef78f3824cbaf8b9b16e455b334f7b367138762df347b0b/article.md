---
schema_version: "1.0.0"
document_id: "b7c75d995e723b9ccef78f3824cbaf8b9b16e455b334f7b367138762df347b0b"
company_key: "backblaze-inc-class-a-common-stock"
company: "Backblaze Inc."
source_id: "backblaze-inc-class-a-common-stock-rss-a06767c1ff83"
canonical_url: "https://www.backblaze.com/blog/cloud-vendors-make-it-easy-to-get-in-theyre-counting-on-it-being-hard-to-leave/"
published_at: "2026-07-22T15:27:03+00:00"
first_seen_at: "2026-07-22T15:41:13.356634+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:c2da588df7b6d32061aa01e514a00b8e170b305555050dc43e6ad330917de02b"
---

# Cloud Vendors Make It Easy to Get In. They’re Counting on It Being Hard to Leave.

You’ve done the storage evaluation. The per-terabyte price is right. The durability numbers check out. Compliance boxes are ticked. And still, the cloud migration project hasn’t been approved.


That’s not a coincidence.


The cloud storage industry has spent years competing on what happens after you’re already locked in: performance, redundancy, features. Almost nobody competes on what it costs to get there—or what it costs to leave.


Migration friction isn’t an oversight. For most hyperscalers, it’s a business model.


## **Why cloud migration projects stall before they start**


The business case for cloud storage usually looks solid on paper. Lower per-terabyte costs. Less hardware to maintain. A path off aging tape libraries and overloaded NAS environments.


Then someone runs the actual migration math.


Egress fees from the current provider. Data transfer charges. Migration software licenses. Professional services. Tape digitization. Internal engineering hours. Project coordination overhead. For a large dataset, those costs can erase years of projected storage savings before a single byte moves.


Teams spend months building an approval-ready business case, only to find the upfront migration cost makes the model unworkable. The project stalls. Infrastructure the organization already knows is unsustainable stays in place. Modernization gets pushed to next quarter.


This is where most cloud vendors win. The storage decision becomes moot if the organization can never afford to move.


## **How egress fees trap organizations with their current provider**


By the time most IT teams discover what cloud egress fees actually cost, they’re already mid-negotiation with a new provider.


The pricing model is deliberately asymmetric: getting data in is cheap, often free. Moving it out is where providers charge—and for multi-petabyte environments, those charges can run to hundreds of thousands of dollars before a migration has even started. Technically, the organization owns its data. Financially, moving it is a different question.


This reframes the evaluation in a way that favors incumbents. The question stops being which platform best fits long-term needs and becomes whether the organization can afford to leave at all. Once you’re in a major cloud platform with a large archive, exit costs are a structural retention mechanism.


Ask any prospective provider, early: what does it cost to leave? If they’re vague, that’s the answer.


## **How long does a cloud data migration actually take?**


Cost gets scrutinized. Time usually doesn’t—until a migration is already underway and slipping.


A large-scale migration means inventorying data and metadata, evaluating and procuring tooling, coordinating across vendors, monitoring transfer jobs, validating integrity at the destination, and troubleshooting the inevitable edge cases. For multi-petabyte environments, self-managed projects routinely stretch from months into years.


Every quarter that drags on is a quarter the organization is paying to maintain infrastructure it’s already committed to replacing, while its engineering team runs a file-moving operation instead of working on anything strategic. The total cost of a slow migration almost always exceeds the initial estimate—and almost nobody builds that into the business case upfront.


## **What actually goes wrong during cloud data migration**


Migration risk tends to be underestimated until something breaks.


The core questions—will files transfer without corruption, will metadata survive intact, will dependent applications keep working—are harder to answer than they look for LTO tape archives that haven’t been accessed in years, NAS and SAN environments with proprietary metadata structures, media archives with irreplaceable assets, regulated content with chain-of-custody requirements, and datasets large enough that verification at scale is its own engineering problem.


The cost of getting this wrong isn’t just the migration itself. Data loss, integrity gaps, or application failures discovered post-migration can be significantly more expensive than any egress fee. Validation and verification need to be designed into the plan before transfers start, not bolted on after something fails.


## **Why most cloud providers leave migration to you**


Infrastructure teams evaluating cloud storage aren’t looking for a transfer tool. They want infrastructure modernized without burning their engineering team on a multi-year internal project. Predictable costs. A path to cloud that doesn’t require standing up a program management office just to move data.


The standard provider response is documentation and an onboarding checklist. After that, you’re largely on your own.


This isn’t an accident. Selling storage is straightforward. Owning migration means taking on cost, risk, and operational complexity that most providers would rather leave with the customer. The economics of the business favor making entry easy and exit expensive, with as little friction to growth as possible in between.


Providers that treat migration as their problem to solve are a different category. They’re betting that making it genuinely easier to get to their platform is worth more than one-time migration revenue—because a customer who gets there successfully tends to stay.


## **How Backblaze Universal Data Migration works**


We built[Universal Data Migration](https://www.backblaze.com/cloud-storage/features/cloud-migration) because we kept seeing the same thing: organizations that had already decided to move to Backblaze B2 getting stuck on the migration itself. The technology decision was made. The budget was approved. The project just couldn’t get started.


The program moves data from virtually any source—AWS S3, Microsoft Azure Blob, Google Cloud Storage, Wasabi, NAS and SAN, file servers, LTO tape across all generations, physical hard drives, legacy archives—with Backblaze managing the process rather than handing the customer a tool and a runbook.


The migration cost doesn’t have to be a reason the project stalls. That’s the point.


## **Before you sign a cloud storage contract, ask these two questions**


The storage evaluation isn’t complete until you know what it costs to get there and what it costs to leave.


Most providers make the second number hard to find. If you have to dig for egress pricing, or if a sales rep answers the exit question with “we’d work with you on that,” build the worst-case number into your model before signing anything.


The right provider won’t make you ask. They’ll make migration part of the conversation from the start—because they’re confident enough in their platform to compete on the full picture, not just the monthly storage line.


---


*Have a migration project that keeps getting pushed?*[Talk to our team](https://www.backblaze.com/contact-sales/) *about what it would actually take to move your environment to B2.*
