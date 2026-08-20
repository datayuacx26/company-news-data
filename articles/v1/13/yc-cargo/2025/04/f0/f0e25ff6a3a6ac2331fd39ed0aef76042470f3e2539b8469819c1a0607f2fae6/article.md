---
schema_version: "1.0.0"
document_id: "f0e25ff6a3a6ac2331fd39ed0aef76042470f3e2539b8469819c1a0607f2fae6"
company_key: "yc-cargo"
company: "Cargo"
source_id: "yc-cargo-news-import-f4a8864e899b"
canonical_url: "https://www.getcargo.ai/blog/nailing-plg"
published_at: "2025-04-30T00:00:00+00:00"
first_seen_at: "2026-07-21T12:34:39.888673+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:8eaa3aa37132975dbb55299dab076a1518970ed89bd59d56450f2945babf7f47"
---

# How to nail your PLG motion in 2025

Product-led sales is resurging. It’s a time for builders.


Outbound is in freefall. Buyers ignore mass outreach. “Spray and pray” is dead, and flooding more domains/emails doesn’t work.


PLG/PLS winners?
They’ve engineered zero-friction: buyers self-serve, see value, and pay only for what works.


That’s why Lovable, Augmentcode, Descript, and Replit are scaling faster than anyone.


But most teams are leaking pipeline everywhere:


- **Junk signups, zero prioritization, reps still chasing (or ignoring) Gmail addresses.**
- **Data scattered across tools:** Product, enrichment, sales all siloed. No single customer view.
- **CRMs full of noise:** Sales can’t focus; pipeline reviews are chaos.


**Want to win? Build your GTM like a machine:**
Unify your data. Automate enrichment and scoring upstream. Only send prioritized, context-rich leads to sales.


Let’s dive in


## Common Pitfalls (and How to Fix Them)#


## 1. Your CRM is Not Your System of Record#


The source of truth isn’t your CRM. It’s your data warehouse. The CRM should be a curated shortlist of leads worth a rep’s time.


Stop treating Salesforce or HubSpot as the mothership. PLG brings noise: bots, junk, and signups at a scale no CRM was built to handle.


Here’s the truth:


- Every time you dump raw signups into your CRM, you’re poisoning the well.
- Reps end up chasing junk. High-value accounts get lost in the mess.
- Gmail signups break lead-to-account matching.
- CRM bloat kills sales productivity and turns pipeline reviews into noise.


Operator move: Use your warehouse (Snowflake, BigQuery, etc.) as the source of truth. Orchestrate all revenue ops: usage, enrichment, signals, prioritization, and lead routing there.


The CRM is just the handoff: only matched, high-intent, ICP-fit leads should ever reach it.


"CRM is not the brain, it's the handoff. Scoring, routing, and intelligence all start upstream."


Nicolas Druelle


CEO


@Revenue Architect


When reps log in, they should see ten ICP warm accounts. Nothing else.


## 2. Too Many Signups, Zero Prioritization#


Everyone is a lead, so no one is.


PLG drives volume. That’s the upside and the problem. Without filtering, a bot, a student, and a Fortune 500 exec all look the same.


Research shows sales-assisted PLG triples conversion. But reps are expensive and limited, and most leads aren’t ready.


sales assisted


That’s where upstream scoring comes in. Prioritize based on intent, fit, and behavior, *before* sales ever gets involved.


Operator tip: High volume of signups? Only surface users who hit a product milestone (PQL), never just email submit. Lower volume? Fit-score every lead to keep only ICP. Everything else: automate, nurture, or hand to marketing or to automated outreach


## 3. Data & Signals Are Split Across Tools#


Product usage in Amplitude. Firmographics in ZoomInfo. Sales activity in Outreach. CRM in Salesforce.


Modern stacks are fragmented by default.


Without unified orchestration, data stays scattered, signals go cold, and buyers fall through the cracks.


- The same user looks anonymous in one tool, high-intent in another, and never surface as a real opportunity.
- Reps burn hours stitching a journey together, if they bother at all.


When your stack can’t sense and act on key moments, you can’t grow efficiently.


"Acquisition dominates initial growth. But at ~$10M ARR, retention becomes the main lever. Before $100M, expansion takes over as the biggest driver. Growth becomes a trifecta: acquisition, retention, and expansion."


Jacco Van Der Kooij


@Revenue Architecture


If you don’t have a unified orchestrator, you *can’t* operationalize the full funnel *(=bowtie).*


- **Expansion triggers** stay buried.
- **Churn risk** goes undetected.
- **Renewals** become reactive firefights.


## The Product-Led Revenue Engine (Built Right)#


Think of your GTM engine like a factory: signals come in, get refined by automation, and only qualified leads get passed to reps.


## Step 1: Filter Noise at the Gate#


Most teams let junk pollute their pipeline. Operators filter at the source.


[Up to 50% of free trial signups may be fake accounts](https://www.growthunhinged.com/p/stop-fake-accounts?utm_source=chatgpt.com) .
They pollute metrics, waste enrichment credits, and distract sales.


Block junk and personal emails *before* they hit downstream systems. At Descript, the team’s first Cargo Play was an AI classifier and blocklist of fraudulent domains. It filtered thousands of fake signups instantly.


- The very first action is to maintain a constantly updated blocklist of disposable, temporary, and fraudulent email domains, catching thousands of junk signups instantly.
- The second action is to evaluate whether a signup was coming from a personal or work email, triggering unique and optimized waterfall enrichment for each type.


## Step 2: Classify and Enrich Every Lead#


Personal email Enrichment


Every signup starts with a simple question: **Is this a work email, or personal?**


- **Personal email?** Route to enrichment tools that can map Gmail/Outlook to LinkedIn. If the match is ICP, proceed. Otherwise, discard.
- **Work email?** Run a separate enrichment logic tuned to that segment/email type.


**Waterfall enrichment is the new standard.**
One provider doesn’t cut it. Chain vendors, validate matches, and tune the logic for your mix.


Operator tip: Waterfall enrichment always trades off reach vs. accuracy. Bad data is worse than no data. When you chain enrichment vendors, don’t just take the first “match”. You need to prioritize by accuracy. 1. If you own the ground truth, battle-test every provider for coverage and accuracy. 2. Ask an LLM to validate, Example: “Does this LinkedIn profile really match the original signup? Does the job title, geography, and domain fit?” Only pass a lead downstream if confidence is high.


At Descript, custom waterfalls led to 80% enrichment coverage and 2x pipeline from PLG signups.


Operator tip: Don’t ignore personal emails. Too many teams block them or treat them as low intent by default, cutting out a massive chunk of real buyers. We’ve seen enterprise VPs, perfect ICP, using a Gmail just to test-drive the product before raising their hand. Action: For every personal email, always run LinkedIn and enrichment. If you catch a senior exec from a top logo, route to high-touch. Don’t miss your next six-figure deal because you filtered by domain.


## Step 3: Score Before You Sync#


PQA/PQL flow


The only way to keep reps focused (and pipeline healthy) is to *ruthlessly* filter and score every lead before it gets to sales.


Your PQL isn’t just a usage metric. It’s a combination of:


- Company fit
- User behavior
- Buyer role (optional)


Only PQLs should reach the CRM and be allocated to reps. Everyone else should be pushed for automated nurture.


Operator tip: 1. Build a list of targeted accounts (Tiers 1) and store it. Anytime someone from a *dream account* signs up, route that lead directly to your best AE for white-glove treatment. 2. As soon as an account is PQL, automatically find other stakeholders so your reps can easily triangulate within the accounts.


Once your leads are filtered and scored, the next step is deciding how to act on them.


## Step 4: Allocate the right effort for each lead tier#


Resource allocation framework for lead tiers


The best GTM systems don’t just identify good leads, they decide what to do with them. Today more than ever, it’s all about maximizing engagement while allocating the right resources based on revenue potential and maturity.


At Descript, the team designed a system that tailors effort based on lead tier.


- **Top-tier (high-scoring) leads** go straight to a rep, bundled with everything they need: account research, user behavior summary, and recommended outreach angle, so they can act instantly.
- **Mid- and low-intent leads** get routed through automated nurture tracks. Product usage data (piped through Cargo) feeds into Octave and Instantly, triggering personalized automated sequences. Automated, but relevant.


This *hybrid model* means every lead gets the right touch, AEs focus only where it counts, and nothing falls through the cracks.


See an extract below from our talk with[G Cabane on how Ramp thinks about this](https://youtu.be/tOgx9CJgUyU?feature=shared) .


G Cabane quote from Ramp about PLG strategy


This hybrid logic, AI agents handling the volume, humans focusing on what matters, will define the next era of GTM in the coming months.


Your edge isn’t volume. It’s precision. Know where to put humans in the loop.


## Step 5: Enable your sales team with the right information#


Workflows without enablement is just plumbing.


**Give reps instant, actionable context:**
Before every meeting, your AE should have a single snapshot: key product actions, recent engagement, enrichment highlights, and relevant stories, all in one place, zero digging. If reps are clicking through Salesforce to piece this together, you’re losing valuable time that should be spent on selling.


Example: Trigger an account research each time a new account has been assigned to a rep, so the owner has the entire *(ie: product, CRM, third party)* context on the lead without clicking everything in SFDC.


Operator tip: The best teams deliver context and ammo: surfacing not just the ‘what happened’, but also the why it matters (proof points, talk tracks, segment-specific plays, customer logos to namedrop) and how to relate it to the specific prospect.


Only pass a lead downstream if confidence is high.


👉 Read more:[Unleashing Sales Potential: The Synergy of Sales Ops & Enablement](https://www.getcargo.io/blog/unleashing-sales-potential-the-synergy-of-sales-ops-enablement)


## It’s Not Just About Acquisition: Orchestrate the Whole Bowtie Funnel#


Most revenue teams stop at acquisition, but a unified GTM engine runs across[the bowtie funnel](https://winningbydesign.com/wp-content/uploads/2024/05/The-Bowtie-A-Proposed-Standard.pdf) :


- **Expansion:** Combine billing (Stripe), usage, and support signals to spot upsell moments.
- **Churn:** Tie contract risk to NPS drops, declining usage, and third-party signals all together.
- **Renewals:** Proactively summarize customer journeys and flag risks *before* the CSM steps in.


Example: At Veriff, plugging Stripe and product usage data into Cargo, they jumped from **42 upsell opportunities created in one month to 363 the very next month,** a nearly **10x increase,** just by orchestrating their signals and surfacing expansion-ready accounts proactively.


👉 Watch the clip: See the actual[pipeline numbers jump](https://youtu.be/6unbfdgBCRw?feature=shared&t=347)


## Takeaway#


PLG isn’t just a motion. It’s a system.


**Don’t duct-tape your funnel. Engineer your revenue engine:**


### Unify data and signals


Centralize all customer data and behavioral signals in one place


### Automate enrichment before CRM


Clean, enrich, and validate data upstream before it hits your CRM


### Prioritize and route by fit or behavior


Use scoring models to surface high-intent, ICP-fit leads automatically


### Arm reps with actionable context


Give sales instant, meaningful insights, not noise


### Let AI agents do the grunt work


Put humans where it counts, high-touch deals and strategic plays


## Key Takeaways#


- **CRM is NOT your system of record, warehouse is** : Dumping raw PLG signups into Salesforce/HubSpot poisons pipeline (bots, junk, Gmail addresses break lead-to-account matching), kills rep productivity, turns pipeline reviews into noise, orchestrate all revenue ops (usage, enrichment, scoring, routing) in warehouse, CRM is just the curated handoff (only matched, high-intent, ICP-fit leads)
- **Filter noise at the gate: 50% of PLG signups are fake accounts** : Block junk/personal emails before downstream systems using AI classifiers + domain blocklists (Descript filtered thousands instantly), classify work vs. personal emails to trigger optimized enrichment waterfalls for each type
- **Waterfall enrichment is the new standard (80% coverage at Descript)** : One provider doesn’t cut it, chain vendors (LinkedIn mappers for personal emails, firmographic enrichers for work emails), validate matches with LLM (“Does this profile match signup?”), prioritize accuracy over reach (bad data worse than no data), personal emails ≠ low intent (enterprise VPs test with Gmail, don’t miss six-figure deals)
- **Score BEFORE sync, only PQLs reach CRM** : PQL = company fit + user behavior + buyer role. Surface Tier 1 dream accounts instantly → white-glove AE treatment. Tier 2/3 → automated nurture (Octave, Instantly fed by product usage). Reps see 10 ICP warm accounts, not noise. Sales-assisted PLG triples conversion but reps are limited, ruthlessly filter upstream
- **Orchestrate full Bowtie (acquisition → retention → expansion)** : Combine billing (Stripe), usage, NPS, support signals for expansion (Veriff: 42 → 363 monthly upsells, 10x jump), churn (contract risk + declining usage), renewals (proactive CS flagging), acquisition dominates early growth, retention kicks in ~ 10MARR,expansionbecomesbiggestdriverpre−10M ARR, expansion becomes biggest driver pre-


10


M


A


R


R


,


e


x


p


an


s


i


o


nb


eco


m


es


bi


g


g


es


t


d


r


i


v


er


p


r


e


−


100M


## Frequently Asked Questions#


Once you mastered the PLG motion, see the[3 workflows that build atop your PLG motion](https://www.getcargo.ai/blog/outbound-flywheel-plg)
