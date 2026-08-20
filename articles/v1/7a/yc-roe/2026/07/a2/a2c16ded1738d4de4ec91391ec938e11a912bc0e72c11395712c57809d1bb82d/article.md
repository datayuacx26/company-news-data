---
schema_version: "1.0.0"
document_id: "a2c16ded1738d4de4ec91391ec938e11a912bc0e72c11395712c57809d1bb82d"
company_key: "yc-roe"
company: "ROE"
source_id: "yc-roe-news-import-46d91611392b"
canonical_url: "https://www.roe-ai.com/blog/bad-actors-dont-use-real-names"
published_at: null
first_seen_at: "2026-07-22T12:09:17.894045+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:68e33ecc7671dbd01faaeea17749f75be9bbda7b4e3afe647e857bcaa0ed3f92"
---

# Bad Actors Don't Use Their Real Name

[All posts](https://www.roe-ai.com/blog) Alternative Data Intelligence


# Bad Actors Don't Use Their Real Name


Why KYC alone can't stop what's coming, and what to do about it.


[Derrick Ongchin](https://www.linkedin.com/in/derrickongchin/) &


[Matt Yao](https://www.linkedin.com/in/mattyao/)


April 24, 2026


10 min read


TL;DR. A well-built synthetic identity costs $15 and 30 minutes. Treating KYC and transaction monitoring as separate workflows is the gap professional fraudsters exploit. The fix is a single investigation that runs identity and behavior together, at machine speed.


Afterpay was one of the fastest-growing fintech companies in the world. The Australian BNPL fintech let consumers split purchases into four installments with no credit check, and by 2018 it was processing billions in transactions, expanding globally, and its stock was up 400% over the past year.


That same year, a governance research firm in Australia ran a simple test on Afterpay's onboarding process. They created an account under the name "Miguel Laucha" (Spanish for Mickey Mouse). The account used a prepaid anonymous SIM card, a prepaid Visa registered to "Giftcardholder," and a fake address in rural Western Australia. The date of birth was fabricated. The IP address was in Melbourne, hundreds of miles from the listed address. The user passed KYC, then proceeded to spend $260 on wine and other goods.


The same firm also had a 16-year-old child of a staff member attempt to create an account. No age verification was triggered. The teenager was approved and used the account to buy over $300 of alcohol.


Afterpay issued a statement to the Australian Securities Exchange promising process upgrades. But the deeper lesson wasn't about one platform's controls. It was about what KYC was actually designed to catch, and what it wasn't.


A synthetic identity that clears onboarding doesn't just represent one bad transaction. It's a foothold. The fraudulent purchases, stolen payment methods, and money movement that follow only happen because the identity cleared the gate.


Derrick Ongchin ran product for risk, identity, and machine learning at Afterpay, and watched these problems up close. The decade before: building the risk, abuse, and security product teams at Uber from scratch during its global expansion, and before that, Trust and Safety at Google.


## What Derrick saw at Uber, and what's showing up again now


At Uber, the identity problem played out at a completely different scale. Organized fraud syndicates ran operations that looked less like opportunistic crime and more like product teams. Stolen identities, stolen payment methods, spoofed devices and locations, and a development cycle fast enough to ship updates within hours of Uber patching a loophole. Small operators were pulling in hundreds of thousands of dollars a month.


That pattern is still happening. It has just migrated to the platforms that came after.


People across the country are receiving IRS tax bills for thousands of dollars in Uber and DoorDash income they never earned. Someone used their Social Security numbers to create driver accounts, worked under their names, and left them with the tax liability.


A class-action lawsuit was filed against Uber in March 2026 by a Florida man who never drove for the company but had over $1,200 in income reported to the IRS under his name. His attorney says he has received dozens of similar calls. In another case, a Wisconsin accountant reported multiple clients receiving IRS notices for $24,000 to $30,000 in fraudulent DoorDash income, with tax bills running $8,000 to $12,000 after penalties and interest.


The supply side isn't even hidden. An investigation in 2025 identified 80 Facebook groups with a combined membership of over 800,000 users openly buying and selling driver accounts for Uber, DoorDash, and Lyft. Posts advertise accounts "created with real data and without history," a phrase that points directly to stolen identities.


The fraud pipeline hasn't changed. It is still stolen identity in, money out. What's changed is the cost of executing it, which is heading toward zero.


## LLMs are the accelerant


The fraud rings Derrick fought at Uber required real infrastructure: SIM racks, coordinated teams, custom spoofing software, human operators managing accounts around the clock. That overhead was what kept the operations expensive and, eventually, what made them detectable.


LLMs and the tools built on top of them are removing that overhead.


What used to require a skilled fraud ring now costs as little as $15 and takes about 30 minutes. The tools have been productized. Underground services like OnlyFake sell high-fidelity fake government IDs specifically optimized to pass automated KYC checks. Fraud-as-a-service kits bundle fake ID generation, deepfake selfie tools, and step-by-step playbooks for bypassing specific platforms' onboarding flows. The barrier to entry has dropped so far that you no longer need technical skill to commit identity fraud at scale. You just need a subscription.


Deepfake tools can now produce synthetic video realistic enough to fool liveness detection, and attackers are injecting those feeds directly into verification pipelines using virtual camera software. Voice cloning adds another layer, enabling social engineering calls that sound like real customers or executives.


Derrick breaks the attacks into three types:


-


Stolen identities: a real person's information used without their knowledge.


-


Fully synthetic identities: every element fabricated from scratch, often polished by generative tools that pass automated KYC at first glance.


-


Cobbled-together identities: a real SSN paired with a fake name and an AI-generated photo, designed to clear vendor checks one signal at a time.


Fraudsters rarely stop at one account. Duplicate accounts, sometimes dozens created from variations of the same underlying identity or payment method, let them multiply exposure across a platform while making each individual account harder to flag in isolation.


> Stolen identities aren't new. What's new is that you don't need to be technical anymore. A year ago, the polish you're seeing on fake IDs and deepfake selfies was nation-state level. Now it's a $15 subscription. The cost to attack has collapsed, and the cost to defend keeps going up.


Deepfake fraud in North America surged 1,740% between 2022 and 2023. Losses linked to deepfake-enabled fraud exceeded $200 million in the first quarter of 2025 alone. Sumsub's 2025 identity fraud report found synthetic identities were used in one out of every five detected cases of first-party fraud. ID.me reported suspending over 130 digital wallets linked to North Korean threat actors using synthetic identities to infiltrate employment and financial platforms.


LLMs don't create new fraud objectives. They remove the bottlenecks that used to keep fraud small: the time to produce convincing documents, the skill needed to fake a live selfie, the coordination required to operate at scale. The playbooks are the same ones Derrick saw a decade ago. The execution cost has just collapsed.


## KYC is a checkpoint, not a wall


Most companies treat KYC as a gate at the front door. The identity gets verified at onboarding, the person passes, and from that point forward the identity is trusted. Transaction monitoring runs separately, often managed by a different team with different tools, looking at activity patterns with no connection back to the signals collected during onboarding.


Bad actors understand this architecture better than most of the companies running it. A fraudster will not use their real identity during KYC. They use a stolen, synthetic, or cobbled-together identity to clear the gate, because once they're past it, the transactions that follow get evaluated on their own terms. A stolen identity paired with a stolen payment method only becomes visible when someone looks at the full picture: who is this person, what are they doing, and does the behavior match what the identity would predict?


Existing KYC vendors compound the problem because none of them have full coverage or full accuracy. One vendor verifies the ID. Another misses that the SSN was flagged. Another runs a sanctions screen but lacks visibility into payment method provenance. Most companies end up stitching together a patchwork of vendors and manual workflows to cover the gaps, and that patchwork itself creates seams that sophisticated actors exploit.


When an analyst does flag a case, the experience is fragmented. They might see a KYC vendor's pass/fail output in one tool, then have to manually pull up transaction records in a separate system, then check a third dashboard for device or behavioral signals. By the time they've assembled the full picture (if they assemble it at all), twenty minutes have passed.


A well-constructed synthetic ID can clear each individual check in sequence while the combination of signals never gets evaluated together: the Melbourne IP with the rural Western Australia address, the prepaid card registered to "Giftcardholder," the fabricated date of birth.


> Treating identity and transactions as separate problems is a gift to fraudsters. A stolen identity only looks fake once you see it behaving fake. A rural address with high-frequency city spend, or one device fingerprint across twenty 'unique' users. If your onboarding data doesn't talk to your transaction data, you're checking IDs at the door and then letting everyone wear masks inside.


Identity signals from onboarding, output from KYC vendors, payment gateway data, and downstream transaction behavior all belong in the same case file. When they live in separate systems reviewed by separate teams, you get exactly the kind of blind spots that professional fraudsters are trained to find.


## The queue is the vulnerability


Even companies with all the right signals hit the same wall: not enough analysts to keep up.


KYC reviews, transaction alerts, SAR filings, EDD requests. The queue grows faster than any team can work through it. When volume spikes (a new product launch, a geographic expansion, a fraud ring hitting all at once), the backlog becomes its own problem. SLAs slip. Analysts start triaging by gut rather than by process. Cases that needed a second look get dispositioned in one pass because the queue behind them is 400 cases deep.


> The math doesn't favor human teams anymore. When a fraud ring automates, they can generate a week of manual reviews in ten minutes. Your analysts start triaging by gut because the SLA clock is running. That backlog itself becomes the vulnerability. If you don't have AI handling the first pass, you're already behind.


The vendors and the analysts aren't failing because they're bad at their jobs. They're failing because the volume and sophistication of what's coming through the door has outpaced what any human-driven workflow can sustain.


## Fighting fire with fire


If the attack surface is being automated, the investigation process has to be too.


The fix is treating identity and transaction data as a single investigation, not two separate workflows. KYC vendor output, payment gateway data, device and behavioral signals collected at onboarding, and downstream transaction patterns all feed into the same case.


An AI agent follows the same SOP your best analyst would. It pulls data from multiple sources. It writes SQL against the warehouse. It cross-references identity signals against transaction behavior. And it flags the things that only show up when you look at all of it together. The agent might link a flagged SSN from a KYC vendor's output to a cluster of transactions originating from the same device fingerprint across three separate accounts, each opened with slight variations of the same address. An analyst doing those steps manually would need to query multiple systems and might never think to look for the cross-account pattern. The agent does it in minutes across hundreds of cases.


The ambiguous cases, the judgment calls, the ones that could go either way: those still need an experienced analyst. What changes is which cases actually reach that analyst. Instead of a human spending twenty minutes on a clear-cut synthetic identity that any well-structured rule set could have caught, the agent handles the first pass and routes the genuinely hard cases to the people who can make the call.


> In the fraud wars I lived through at Uber, the syndicates had their own R&D. They shipped 'product updates' against us. Static rules don't beat that. You need a system that investigates with the same intent as the attacker. The attackers are already automated. Defenders have to be too.


If you're still running identity verification and transaction monitoring as separate operations, you're building the architecture that modern fraud is designed to exploit. That's the problem we built Roe to solve. Our agents investigate identity and transaction signals together, end-to-end, the same way your best analyst would, just without the queue.


## What comes next


> Ultimately, the playbooks haven't changed that much. It's still about stolen info and moving money. What has changed is the scale and the intent behind the tools. In the old days, we were fighting scripts; today, we're fighting AI. If you're still running a manual-first architecture, you're bringing a knife to a drone fight. It's time to stop treating risk as a queue to be managed and start treating it as a signal to be automated.


If you're rethinking how your team handles KYC, EDD, or fraud investigations, we'd like to talk.
