---
schema_version: "1.0.0"
document_id: "37e4d00f7000ce779e0ddedb703ef8a1a09e62cf63d9ef5788cab1d22c1e91af"
company_key: "yc-clearly-ai"
company: "Clearly AI"
source_id: "yc-clearly-ai-news-import-066e8c806377"
canonical_url: "https://clearly-ai.com/blog/part-2-okay-privacy-is-different-from-security.-now-what"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T21:55:10.582550+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:ae24cac1437118d9f0bece6cc457e6ac849c9a8710eb5461440ab3f1cf7ec441"
---

# Part 2! Okay, privacy is different from security. Now what?

In my last post, I discussed some of the major differences between privacy and security work. In this post, we’ll talk about how to put this knowledge into practice, covering nuances in cataloguing, deletion, filtering, and more.


## Cataloging: What's this? What's this?


You cannot protect what you can't find or describe, and privacy demands a far richer description than security ever did. As I noted in the previous post, security data inventory is hard, and privacy inventory is worse, because you need semantics at the row *and column* level, not just a sensitivity tier.


Start by sitting down with your lawyers (who you are now hopefully very good friends with) and figuring out exactly what metadata you'll need to meet your business’s privacy obligations. Do you handle consumer or B2B data? Children, or adults only? Financial and/or medical data? Do you have a consent decree? What countries do you do business in? Again, there is no general purpose checklist, and you can waste huge amounts of time and money if you try to use out-of-the-box metadata taxonomies,


The coarse "Critical/Confidential" or "Red/Yellow" labels that your security classification scheme uses are not going to cut it. You need metadata robust enough to know for a given record or field whether it's personal data, what kind, and what purposes it may be used for.


Then update your data inventory tools to capture those detailed semantics. Your current security data inventory tool may not be good at this, and you may need to upgrade. Go dataset by dataset and catalog what is and isn't personal data. Commit to keeping it current, because a stale catalog is almost worse than none at all. Don’t just look for new datasets, look for schema changes in existing ones, because that can change your obligations. Catalog your *systems* by purpose too, paying special attention to advertising and marketing systems, ML systems, systems that make financial decisions, and anything else with extra restrictions, especially where consent or children's data is in play. Those are the places where a purpose violation becomes a headline. Run your privacy program like the NYT is watching.


Automate this everywhere you can, because manual cataloging doesn't scale. (Tools like Clearly AI are really good at accelerating continuous inventory and cataloging. Just saying.) But also be honest about the tradeoff: privacy fines are extraordinarily expensive, and getting your data correctly annotated by an actual expert may sometimes be the right call even when it's slow and costly. Cheap-and-wrong is the most expensive option of all here. An approach which has automation with baked-in escalation rubrics is a nice compromise.


## Child data and consent: Why can't you behave?


If there is one thing that makes regulators cranky, it is mishandling of child data. It needs to be provably filtered out for a long list of use cases (starting with ads targeting) If not? It’s bad. Anything that might have touched that misused child data is now tainted. We’re talking[delete your entire trained ML model bad](https://www.wilmerhale.com/en/insights/blogs/wilmerhale-privacy-and-cybersecurity-law/20220309-ftc-settles-with-weight-loss-company-over-childrens-data) . Tens or hundreds of millions in fines bad.


Consent! You know that that nice popup on your website? It needs to hook to something. A lot of things, actually. Un-consenting user data poses many of the same problems as child data. Unlike child data, consent can flip-flop repeatedly, so you have to check consent status every time, not cache by user and forget. (Yes, this is horrible. I am so sorry. I do not make the rules.)


If you are in a position to handle either of these issues, invest heavily in reusable building blocks for filtering in your data pipeline tool of choice. Make it easy for your data scientists to do the right thing. And build a robust canary testing program that includes children, unconsenting users, etc. to make sure the filters are being used and working.


## Access control and filtering: build the wall


Once you know what you have, you have to control how it's used. This is where privacy demands more than classic access control.


Access control needs to be *purpose-aware* . It's not enough to ask "is this person allowed to see customer data?" You have to ask "is this person allowed to use this customer data *for this purpose* ?" A data scientist might work on fraud one day, and ads the next, especially on a smaller team. You can model this with roles, but the catch is that both the humans and the automated systems consuming the data have to actually invoke the correct role for each individual task (spoiler: they won’t, not consistently, not unless you force them to pick every time, and maybe not even then). Attribute-based access control, where the decision factors in the purpose and the data's attributes, is another option.


But access control alone is not enough. You need *filtering* controls sitting directly in the data pathway. The canonical example is the one from earlier: don't give children's data to the ad targeting system. That's not really an access decision. You want the ads targeting system to access things like activity and interest data, just not all of the data. That’s a filtering decision, and it has to happen automatically as the data flows, not as a gate someone remembers to check. A library of commonly used filter patterns for your data science platform of choice will serve you well. Make it easy for people to do the right thing.


And one piece of hard-won advice: make sure user IDs and collection timestamps travel everywhere the personal data goes. Every pipeline, every derived dataset, every export. Don’t skimp because of storage costs. You will hate yourself later if you don't, because the day someone asks you to delete a user's data or prove you didn’t retain it too long, the answer lives in metadata you can’t recreate.


### Deletion: What do we leave? Nothing much


Deletion is the final boss, and it's where all the earlier work pays off or falls apart. If you cataloged your data well and propagated user IDs everywhere, deletion becomes tractable: you can find every place a user's data lives and remove it. If you didn't, deletion becomes an archaeology project you run against a deadline, and you will miss things.


This is also where the security-versus-privacy tension peaks one last time. Your job is to delete the data thoroughly, including from backups, while *preserving* the narrow set of things you're legally required or permitted to keep, like security logs, records under litigation hold, and data retained for tax and accounting. Getting that distinction right, at scale, automatically, is how you know you’ve nailed it at every preceding step. A number of very skilled people have already written treatises on reliable deletion architectures, so I will not reiterate them here, but please don't re-invent the wheel. The proceedings of[PEPR](https://www.usenix.org/conferences/byname/1046) are an excellent place to start.


## Becoming the very model of modern data handling


The hoodie-wearing hacker wants to steal your data. The data scientist wants to *use* it, productively and enthusiastically, in ways the law may not allow and the customer may not have agreed to. Your existing security controls are tuned to stop the first person and wave the second one right through.


Closing that gap requires extending your existing security program and tools with the things privacy uniquely demands: semantic cataloging instead of coarse tiers, purpose-aware access control plus inline filtering, and scalable deletion that respects both the right to be forgotten and the obligations to remember. Catalog, control, delete. Get those three right and you become the department that lets the data scientists move fast with earned confidence.


It's messier than security (which is pretty messy to start with). It's less mature than security. The frameworks are weaker and the law is a moving target with a bunch of ambiguity. But it's also where a huge amount of real risk now lives, and security practitioners that learn to do it well are invaluable at any company. (Even better if you learn AI governance. We’ll talk about that another time.)


Tell your lawyer I said hi.
