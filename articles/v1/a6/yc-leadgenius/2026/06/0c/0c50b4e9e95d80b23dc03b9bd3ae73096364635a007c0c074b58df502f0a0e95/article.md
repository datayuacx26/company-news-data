---
schema_version: "1.0.0"
document_id: "0c50b4e9e95d80b23dc03b9bd3ae73096364635a007c0c074b58df502f0a0e95"
company_key: "yc-leadgenius"
company: "LeadGenius"
source_id: "yc-leadgenius-news-import-60e73975c0bf"
canonical_url: "https://www.leadgenius.com/resources/i-analyzed-every-yc-backed-sales-tech-company-from-the-last-5-years-heres-what-the-data-actually-says"
published_at: "2026-06-06T20:00:02.043+00:00"
first_seen_at: "2026-07-25T11:51:49.719746+00:00"
fetched_at: "2026-07-28T21:46:31.447870+00:00"
content_hash: "sha256:2be0bbf0c6ebffeb6db33b98dfbaf9aa0ca5983ce183f37cbda241cf423ecfc1"
---

# I Analyzed Every YC-Backed Sales Tech Company from the Last 5 Years. Here's What the Data Actually Says.

What I Learned Analyzing Every YC-Backed Sales Tech Company from the Last 5 Years | LeadGenius 139


YC sales tech
companies analyzed


76 %


Still active
as of mid-2026


3 x


Increase in launches
post-ChatGPT


0 %


Failure rate in the
data/enrichment category


Every quarter I get asked the same question by GTM leaders. Some version of: "Is sales tech actually still a thing? Or have we hit peak SDR-replacement-tool?" So I stopped guessing. I pulled the data on every Y Combinator-backed sales tech, CRM, and GTM company from S20 through F25, filtered for actual sales technology (not "we have an SDR who uses our healthcare product"), and built a real dataset. 139 companies. Five years. Every batch.


This is what I learned. Not the hot takes you've been seeing on LinkedIn. The actual patterns the data is pointing at.


And I'll tell you up front: **two of my four going-in hypotheses were wrong** . One was sharper than I realized. The fourth I'm still arguing with myself about. Let me walk you through all of it.


Methodology


I started with every YC company in the public directory and filtered for sales tech, CRM, and GTM-focused products based on the actual tagline (not founder bio or hiring page mentions of "sales"). I excluded data pipeline tools, healthcare revenue cycle management, sales tax software, recruiting ATS, and e-commerce listing tools — different buyers, different motions, different patterns. What's left: 139 companies whose primary product sells into a sales org. Cross-referenced with public funding data, LinkedIn employee counts, and current status from the YC directory.


*Full disclosure: LeadGenius (S11) is in the dataset. So is one of our competitors. I treated both with the same lens as everyone else.*


## The Headline Numbers


Before we get into the meat, here's the basic shape of the YC sales tech universe over the last 14 batches:


YC Sales Tech Launches by Year


Founding year · 139 companies


Figure 1 · Source: LeadGenius analysis of YC directory + Crunchbase


The acceleration is obvious. The category essentially doubled between 2020 and 2023. ChatGPT launched in late 2022 and within twelve months we had a record year for sales tech launches. That's not a coincidence. That's an entire investor class deciding the GTM stack was about to be rebuilt.


But raw launches don't tell you what's working. Let's look at survival.


Survival Status by Founding Year


Active vs. inactive vs. acquired


Figure 2 · Status as of June 2026


Now this is where it gets interesting. **Pre-ChatGPT cohort (2018–2022): 18% inactive. Post-ChatGPT cohort (2023–2025): 6% inactive.**


I know what you're thinking. The post-ChatGPT companies are younger. They haven't had time to die yet. Fair. That's a real caveat. But even adjusting for that, the data is telling us something specific: **the wave of sales tech that's getting built today is being built on materially better primitives** than the wave that came before it. Companies that launched in 2019–2022 promising "AI-powered" anything were promising something they couldn't actually deliver. They're now the inactive list.


A generation of pre-LLM sales tech got obsoleted by the LLM wave. The new wave is still figuring out which of its bets sticks.


## Hypothesis 1: Sales Tech Is Cooling Off


#### Verdict — Wrong


I had this hypothesis going in because every other day someone tells me "AI SDRs are dead." And in narrow ways, that take has some merit (we'll get to it). But the overall sector? Not cooling. Accelerating.


Cohort Companies % Inactive % Acquired % Active


**Pre-ChatGPT (2018–2022)** 62 18% 13% 69%


**Post-ChatGPT (2023–2025)** 51 6% 0% 94%


**Pre-2018 (legacy)** 26 15% 15% 70%


Here's the part that doesn't fit the "AI SDRs ate the world" narrative: **the median active YC sales tech company has 6 employees** . The mean is 25 only because of a handful of breakouts like Apollo.io (800), Snapdocs (285), and CaptivateIQ (280) skewing it.


The actual shape of this category isn't a few massive AI agents replacing entire sales orgs. It's *hundreds* of 4-to-8-person teams trying to find a real wedge in a stack that's getting rebuilt. That's what an accelerating, healthy market looks like before consolidation. Not before death.


## Hypothesis 2: Going Vertical Is Winning


#### Verdict — Sharper than I expected, with a twist


The vertical thesis is one of the few things in GTM tech that the data actually backs up cleanly. Let me show you the failure rates by category, because this chart changed how I think about what's defensible:


Failure Rate by Category


% inactive within each segment · n = 139


Figure 3 · Failure = company marked inactive on YC directory


Three categories have a **zero percent failure rate** in my dataset: data and enrichment, RFP/proposals, and sales training & coaching. Vertical players are next at 12.5%. The horizontal "we're going to replace your whole stack" plays are the ones doing the dying.


If data is the new oil, the companies selling you "lists" of generic logos and undifferentiated TAM are offering barrels of sludge. The ones building structured, verified, vertical-aware data infrastructure — Cargo, Persana, Ciro, the LeadGenius core thesis — those are the picks-and-shovels businesses of this cycle. The data confirms it.


Here's the YC vertical roster, sorted by status:


Table 1


· Vertical-First YC Sales Tech Companies


Company Vertical Founded Employees Status


Distro Industrial wholesale distributors 2023 59 Active


Avoca Service-based industries (HVAC, etc.) 2022 118 Active


Sameday Home services 2021 7 Active


Aether Home services installers 2024 4 Active


SpadeWorks Trade businesses 2024 3 Active


Cohesive Blue-collar services 2024 2 Active


Caseflood.ai Law firms 2024 3 Active


Obento Health Medtech sales 2023 2 Active


Zorba Real estate sales 2024 5 Active


Claim Health Post-acute healthcare 2023 8 Active


MedPiper Health data ("Salesforce for") 2020 37 Active


OneLocal Local businesses — 90 Active


GovEagle Government RFPs 2024 3 Active


Bonfire Gov procurement RFPs 2012 95 Acquired


Caucus AI CRM for government — 3 Inactive


Of 15


vertical-first companies, 13


are still active, one had a real exit (Bonfire), and only one is fully inactive (Caucus — and "AI CRM for government" was always going to be a long road, because the government moves at the speed of the government).


But here's the twist that doesn't show up in the static dataset and that I think matters more: **founders are starting vertical and quietly pivoting horizontal.** I see it constantly in the broader market. A founder launches "AI agent for construction sales." Six months later, their best customer turns out to be a marketing agency selling *into* construction. So they pivot. Now they're a horizontal LinkedIn automation tool competing with 200 other companies.


Why does this keep happening? Because founders consistently overestimate how ready a vertical is to adopt AI. The HVAC owner doesn't want an AI agent. He wants the phones to ring. Tech companies adopt AI in weeks. "Real" industries take years. And YC founders are wired for two-week iteration cycles, so when the vertical doesn't move fast enough, the temptation to chase tech-buyer revenue becomes overwhelming.


The vertical winners are the ones whose founders *lived* the industry pain before they wrote a line of code. And even those founders are mostly building boring software with AI features sprinkled on — not "agentic AI revolution." That's not a bug. That's the playbook.


Operator takeaway


If you're building vertical, the question isn't "can AI do this thing." It's "does the buyer in this vertical know what they want, and will they pay for it on their existing budget cycle." If the answer to either is no, you don't have a vertical company. You have a science project.


## Hypothesis 3: We're Heading Toward Fully Autonomous AI Agents


#### Verdict — Wrong, and the market has already told us so


This was the hypothesis I held most confidently going in. The trajectory *seemed* obvious: copilot → assistant → agent → autonomous worker. Every cohort, less human in the loop. By 2026 you'd just press a button and pipeline would happen.


The data — and especially what's happened to the loudest companies in this category over the last twelve months — is screaming the opposite.


Let's look at the YC AI SDR / outbound automation roster:


Table 2


· YC AI SDR & Outbound Automation Companies


Company Pitch Founded Employees Status


Warmly, Autonomous sales platform, auto-email warmest leads 2020 75 Active


Artisan AI employees, starting with AI BDR (Ava) 2023 37 Active


Floworks AI sales employees | SDR | RevOps | EA 2022 20 Active


Kular AI lead generator 2021 12 Active


Sameday AI sales agents for home services 2021 7 Active


Bluebirds Automate outbound with signals 2022 6 Acquired


AiSDR "Replace your SDR with AiSDR" 2023 5 Active


Lightmeter Managed deliverability for cold outreach 2022 5 Active


Topo Custom-trained AI sales agent per company 2023 5 Active


CoffeeAI Hyper-personalized AI outreach 2019 5 Inactive


Roger AI SDR that automates outbound 24/7 2024 2 Active


Venta AI EU-compliant AI SDR (letters, email, LinkedIn) 2023 2 Active


Twelve companies. Most of them tiny. Two of the largest (Warmly and Artisan) have quietly repositioned away from the "replace your SDR" pitch.


And outside YC, the loudest name in the category — **11x.ai** , which raised $24M from Benchmark and $50M from a16z — became the cautionary tale of the entire AI SDR market this year. Investigations found 11x had been using "Contracted ARR" that counted three-month trial customers as if they were on annual plans. The company faced accusations of putting customer logos on its website without permission; ZoomInfo and Airtable publicly said they were not, in fact, customers. The CEO was reportedly fired amid the fallout.


And Artisan — the YC poster child for this category — raised about $47M total (including a $25M Series A in April 2025) and hit roughly $5M ARR with 250 customers. That's solid growth. But Artisan also reportedly delayed their broader "digital workers" roadmap, citing the need for "fundamental improvements" to their AI BDR. The pitch had outrun the product.


The market voted on full autonomy. The market said no. At least not for cold outbound. At least not yet.


So why didn't this work? Here's the operator answer:


- **Cold email is judged on outcome, not output.** AI got really good at output (volume of personalized emails) almost overnight. It has not gotten meaningfully better at outcome (booked meetings with qualified buyers). The gap shows up in reply rates, in deliverability collapse, in prospects learning to spot the templates instantly.
- **The buyer in B2B outbound is a defensive system, not a passive one.** When the volume of AI-generated outreach hits a threshold, the inbox itself starts treating it as noise. We're past that threshold. By a lot.
- **"Replace your SDR" is a hostile pitch to the person buying.** The buyer is usually a VP of Sales whose entire org structure includes SDRs. You're asking them to fire their team. Most don't.


The companies actually scaling in this category have stopped pitching replacement and started pitching *leverage* : a tool for existing SDRs that automates the boring 80%, or a way to make outbound work for accounts that previously weren't worth a rep's time. That's a tool-for-humans pitch, not an autonomous-worker pitch.


## Hypothesis 4: AI-Native CRMs Will Eat Salesforce


#### Verdict — Not in this decade. Maybe ever. But something more useful is happening.


The CRM category has 19


companies in my dataset. Roughly half showed up in the last three years. The pitch is some variant of: "Salesforce was built for managers who want reports. We're built for reps who hate data entry. And by the way, we have AI."


It's a great pitch. It also has a 21% failure rate, which is the second-highest in my data. Caucus, Xkit, Elucify, Sonnet — all gone.


CRM Companies by Status


Full replacement vs. layer-on-top approaches


Figure 4 · Within CRM segment only · n = 19


The reason replacement is so hard isn't technological. It's organizational. When you replace a CRM, you're not replacing software. You're replacing years of workflows, integrations, sales ops rituals, manager dashboards, board reports, comp calcs, and institutional muscle memory. You don't rip that out for a prettier UI. You rip it out when there's a fundamentally different way of running revenue that the incumbent literally cannot accommodate.


So far, "AI-native" isn't that fundamentally different thing. It's a layer. A really useful layer. Just not a replacement.


The companies in this category that are actually winning are the ones that have stopped trying to replace Salesforce and started fixing the things Salesforce does badly:


Table 3


· The Smarter CRM Plays — Layer, Not Replace


Company The Wedge Founded Status


DryMerge "AI that updates your CRM for you" — fixes data entry without asking anyone to switch 2023 Active


Stacksync Real-time two-way sync between CRMs and databases — picks-and-shovels 2022 Active


Paragon AI Turns sales calls into CRM data automatically 2022 Active


Scape AI-native CRM that captures all conversations 2024 Active


Streak Turns Gmail into a CRM — $10M+ ARR on $2M of funding, quietly profitable 2012 Active


Close SMB sales CRM, never tried to be Salesforce 2013 Active


Twenty Open-source Salesforce alternative — 44K+ GitHub stars 2023 Active


Paloma AI-native CRM for post-sales operations 2025 Active


The two oldest CRM companies on this list — Streak (2011) and Close (2013) — are also two of the healthiest. They survived Salesforce's heyday, the SaaS gold rush, the "death of CRM" thinkpieces of 2020, and they're still here. Both because they picked specific buyers (Gmail-native SMBs; SMB sales teams) and never tried to be everything to everyone.


**Twenty** is the open-source wildcard I'm watching. 44,800+


GitHub stars, modern UI, Y Combinator-backed. Whether that converts into a venture-scale business is genuinely uncertain — open-source CRM is a graveyard of well-loved projects that never made it to enterprise revenue. But if they convert even a fraction of self-hosters to a managed Pro tier, they have a path. And every $175/seat/month Salesforce charges is a margin Twenty can give back to its customers.


## Where the Money Actually Is


Let me tell you what surprised me most when I sorted by failure rate. The categories getting all the attention — AI SDRs, AI-native CRM, "agentic everything" — are not the categories that are quietly succeeding.


The unglamorous categories are.


Category Health Quadrant — Where the Boring Money Hides


Failure rate vs. # of companies · bubble size = active companies


Figure 5 · Companies in upper-right are crowded but stable; lower-left = early/under-explored


### The Quiet Winners (0% Failure)


**Sales training and coaching.** Seven companies including Hyperbound, SilkChart, Trellus, Demodesk, Flockjay, Ambition. Plus Wingman, acquired by Clari. Zero outright failures (one acquisition counts as a win). Why this works: training is measurable, expensive to do manually, and scales by AI in a way that doesn't put the buyer on the wrong side of the table. When an AI roleplays a tough buyer, nobody is being deceived. When an AI reviews 200 calls and finds patterns, nobody is unhappy about it.


**RFPs, proposals, and quoting.** Eight companies. Two real acquisitions (Procoto, Bonfire — Bonfire had 95 employees at exit, real outcome). Active players include Inventive AI, Outlit, GovEagle, Powerdocs, GovernGPT, Veles. This is the most under-appreciated AI sales tech category in YC. RFPs are long, structured documents LLMs are genuinely good at. They're massive time sucks. Companies answer the same 200 questions across every RFP they touch. Existing solutions are 2014-era SharePoint. You don't need autonomy. You don't need agents. You need an LLM that knows your last 1,000 answers and drafts the first 80%. High willingness to pay. Drowning buyer. Solvable problem.


**Data and enrichment.** Zero failures. Period. The companies in this category — Cargo, Persana AI, Ciro, Fiber AI, Meticulate, Ponyrun, Origami, Anglera — are the picks-and-shovels of the AI GTM gold rush. They feed every other tool. (And yes, Clay is the elephant in the room here. Clay is *not* a YC company, founded 2017, separately backed, now at ~$100M ARR and a $3.1B valuation. But Clay's existence is the dominant force in this category, and the YC players are mostly positioned *against* or *underneath* Clay's footprint.)


### The Quiet Unicorns


The three YC sales tech outcomes that quietly became massive businesses share a pattern. Let's look at them:


Table 4


· YC Sales Tech Unicorns & Scaled Outcomes


Company Category Founded Valuation ARR (est.) Employees


Apollo.io Sales intelligence + outreach 2015 $1.6B ~$150M 800+


Snapdocs Mortgage closing workflow 2012 $1.5B — 385


CaptivateIQ Sales commissions / SPM 2017 $1.25B ~$42M 280


What do they share?


1. **All founded before the AI hype cycle.** 2012–2017. They had time to build deep workflow integration before the market got loud.
2. **All sell a clear, measurable outcome.** Faster mortgage closings. Accurate commission payouts. More leads in your funnel. Not "AI-powered" anything.
3. **All are picks-and-shovels companies that added AI later** , not AI-first companies looking for a workflow to attach to.
4. **All survived at least one category transition.** Apollo went from "ZoomInfo cheaper" → "GTM platform" → "AI-powered GTM." CaptivateIQ went from "Excel replacement" → "RevOps platform." Products kept changing. Customer relationships stayed.


If you're starting something in sales tech today, the lesson the data is screaming at you is this: **AI isn't the moat.** The workflow ownership, the data, the integration depth, the customer relationship — that's the moat. AI is the price of admission now, not the differentiator.


## The Failures, In Detail


Twenty-three inactive companies. When I look at them as a group, the pattern that jumps out isn't bad ideas. It's bad timing, or bad wedges, or both.


Table 5


· The Inactive List — What Killed Them


Company What They Built Founded Likely Cause of Death


Flike AI that crafts sales emails 2021 Pre-GPT-3.5 AI. Tech wasn't ready.


CoffeeAI Hyper-personalized AI outreach 2019 Same — launched 4 years too early


Echo Founder-led sales co-pilot 2022 Tiny team, no wedge vs. Apollo


Sonnet Meeting notes + automated CRM 2022 Out-executed by Gong/Fireflies/Granola


Buzzle Voice-of-customer trends from sales calls 2021 Buyer didn't exist at the right budget


Penguin AI AI sales rep for website intent 2022 2-person team vs. Apollo (800 emp)


Flyflow "Easiest way to find sales leads" 2024 No differentiation in saturated market


Unhaze Right leads at the right time 2023 Same — undifferentiated


Xkit Universal CRM integration for B2B SaaS 2018 Salesforce/HubSpot built it natively


Elucify API to auto-clean CRM data 2015 Same fate


Laserfocus Streamlined Salesforce processes 2019 Same fate


PhoneSys Phone system for sales teams 2011 Lost the wedge between dialers + UCaaS


Compgun Sales commission software — Out-executed by CaptivateIQ and Spiff


Caucus AI CRM for government — Government doesn't move that fast


Vessel Native integration platform for GTM tools 2022 5-person team, niche infra play


Slik Email outreach for the long tail — 2-person team, no defensible wedge


Zillabyte "Palantir for sales people" — Vague positioning, no execution


Kanava AI AI sales hire for wholesale distributors 2025 Too new to call (still listed inactive)


Protego Recover revenue from chargebacks 2021 Adjacent to sales, narrow buyer


Criya AI that builds GTM assets 2021 Pre-LLM, undifferentiated post-LLM


CoinTent Recover revenue lost to ad blockers — Wrong buyer for the wedge


Start Closing Homeowner ↔ contractor interactions — Marketplace, not sales tech


Zenbox Aggregator of SaaS for SMBs — No defensible positioning


If you sort these failures into buckets, you get:


- **Pre-LLM AI plays** (Flike, CoffeeAI, Buzzle, Echo, Sonnet, Criya) — built something that needed LLMs to actually work, launched 1–4 years too early
- **Horizontal AI sales reps without a wedge** (Penguin AI, Unhaze, Slik, Flyflow) — 2-to-4-person teams trying to take down Apollo
- **Salesforce integration plays that Salesforce absorbed** (Xkit, Elucify, Laserfocus) — moat evaporated when the incumbent shipped a native version
- **Right idea, wrong buyer or wrong budget** (PhoneSys, Compgun, Caucus, Protego, CoinTent)


Notice what's *not* on this list: nobody died because the technology didn't work. Everyone died because the market structure didn't.


## The F25 Bets — What's Being Built Right Now


The newest companies in my dataset are from F25. The themes are revealing:


Table 6


· F25 Sales Tech Cohort


Company Pitch Category Saturation


Item AI-native CRM ("find me leads in X industry") High — Octolane, Paloma, Cohesive ahead


Aside Notetaker for sales engineers Low — niche, overlooked wedge


Karumi Agentic product demos Medium — different angle than horizontal demo tools


Throxy Vertical AI agents running sales funnel High — crowded "agent runs your funnel" space


Leadbay Prospecting data Very high — Clay + Apollo own this


Three out of five F25 bets are entering already-crowded categories. That's a saturation signal, not a death signal — when a category attracts new entrants at this pace, it usually means the market is real, the unit economics work, or there's a perceived window before incumbents lock things down. Often all three.


If I had to bet on one from this cohort, it wouldn't be the loudest. It would be **Aside** . Sales engineers have completely different workflows from AEs. Tools built for AEs don't work for them. The buyer has budget and influence over deals. Small TAM, but the people inside that TAM care a lot.


## What This Means for Operators


If you're a GTM leader trying to make sense of the noise — and that's most of you — here's what the data actually supports:


#### The Operator's Playbook from 139 Companies


- **Stop pitching autonomy.** The "fire your SDR" pitch has been litigated. The market voted no. Pitch leverage on existing reps, or expansion into accounts they previously couldn't reach.
- **Pick the workflow before the AI.** Apollo, CaptivateIQ, and Snapdocs all owned a workflow first. The companies that died led with AI capability and tried to find a workflow to apply it to.
- **Vertical is real, but only if you've lived it.** Tech founders parachuting into HVAC mostly fail. Operators-turned-founders who learned to ship software have an uncommonly clean shot.
- **Underpriced categories: sales training, RFPs, SPM.** No one writes thinkpieces about them. The buyers want them. The AI value is obvious without being scary. Good signals all.
- **AI-native CRM is a real opportunity, but a brutal one.** The switching costs are extreme. The companies with a shot are either open-source (Twenty), built day-zero for AI-native businesses, or wedging on top of incumbents (DryMerge, Paragon, Scape).
- **Don't compete with Clay on horizontal enrichment.** $3.1B valuation, $100M ARR projected. They own the GTM engineer community. Find the niche they don't cover — vertical data, voice, post-sales, partnerships — and own that.
- **The data infrastructure is the moat.** Before you add another AI tool to your stack, audit your data foundation. Most "AI doesn't work for us" stories are actually "our data is garbage" stories.


## The Boring Truth


Most sales tech that wins is unglamorous. It compounds slowly. It's built by founders who would rather own a workflow than write a manifesto. It looks more like CaptivateIQ doing commissions than 11x trying to be your whole sales team.


The AI is the cherry. The work is the cake.


I'll keep tracking this. If you're building in this space and have a different read on any of it — or you want to argue about whether the 11x collapse is the canary or the floor — find me on LinkedIn. I genuinely want to hear it. The dataset is only useful if the people in the trenches are pressure-testing what I'm seeing.


⁂


— Derek Rahn · VP of Demand Gen, LeadGenius


## The Money Is in the Data.


If you're rebuilding your GTM data foundation — before adding more AI tools on top — that's what we do at LeadGenius. Verified contact data, vertical-aware enrichment, and an API built for the GTM engineers building the next wave.


Talk with a Strategist →
