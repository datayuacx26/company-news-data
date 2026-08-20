---
schema_version: "1.0.0"
document_id: "f943e3da5211284939a154818654b62726238a7132dbc3ad3a11bf1a4de9d298"
company_key: "yc-freshpaint"
company: "Freshpaint"
source_id: "yc-freshpaint-news-import-105ad9035257"
canonical_url: "https://www.freshpaint.io/blog/how-to-drive-enrollment-when-pixels-are-off-the-table"
published_at: "2026-06-24T15:41:56.218+00:00"
first_seen_at: "2026-07-24T09:09:59.893678+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:214913c80c4f3507b00bbe65590e49ab135f435282ea57e52852ae8e66eaa624"
---

# How a Payer Marketing Agency Drives Enrollment When Pixels Are Off the Table

**Anatomy of a Campaign** is a Freshpaint series that takes you inside real healthcare marketing campaigns – breaking down the strategy, execution, and measurement behind what made them work. In each edition, we deconstruct a real-world campaign so you can:


- Understand the strategic thinking behind it
- See how it was activated
- Learn how performance was measured
- Walk away with practical, transferable insights for your own campaigns


When the tracking infrastructure that healthcare marketers rely on gets stripped away — no pixels, no same-day conversion data, a week-plus lag between enrollment and reportable results — the instinct is to retreat toward what's measurable. Run more search. Report last-click. Hope it holds.


[Amsive Health](https://health.amsive.com/) , a full-service healthcare marketing agency serving payer clients across the country, took a different path. Their paid media team faced all of these constraints simultaneously on a D-SNP (Dual Eligible Special Needs Plan) enrollment campaign: Meta pixels gone, CDPs unable to capture the micro-conversions that mattered, a multi-week sales cycle that broke standard attribution models, and platform restrictions that varied by channel and client compliance team.


Jon Kagan is a Senior Search Strategist at Amsive Health who runs paid media for several insurance payer clients, including campaigns targeting Medicare and Medicaid dual-eligible populations. When his team redesigned the targeting, channel sequencing, and geographic strategy for a D-SNP enrollment campaign, they didn't just solve for the constraints — they built a repeatable framework that improved performance without increasing budget.


**By applying their audience and measurement framework at the zip code level — and rerouting approximately 20–25% of spend out of a single non-performing area — they reduced cost per lead and increased net lead volume simultaneously.**


##### This is the anatomy of that campaign — and what payer marketers and agency teams running enrollment campaigns can learn from it.


# Anatomy of a Campaign


##### The Market Context


**Client Type:** Insurance payer — Medicare/Medicaid Dual Eligible Special Needs Plan (D-SNP)


**Agency:**[Amsive Health](https://health.amsive.com/)


**Challenge:** Drive D-SNP enrollment among low-income, 65+ populations in specific geographies, with nearly every standard tracking mechanism either removed or non-functional — and a multi-week conversion lag that made real-time optimization almost impossible.


*Inside the Campaign's Growth Engine:*


- *Audience profiling built from observational behavioral data when pixel-based signals weren't available*
- *Channel selection matched to how low-income seniors actually consume media — not how they're assumed to*
- *Platform income targeting applied inversely across Google and Meta to triangulate the D-SNP-eligible population*
- *Multi-touch attribution used for decisions; last-click reserved for reporting*
- *Marketing spend allocation tied directly to cost per lead and net lead volume — not impressions or clicks*


### **1. Campaign Objective: Drive Enrollment Among a Legally Constrained, Compliance-Heavy Audience**


D-SNP campaigns sit at the intersection of two of healthcare marketing's hardest problems: reaching a vulnerable, underserved population and doing it inside a compliance framework that removes most of the tools paid media teams depend on.


The target audience — low-income adults 65 and older who qualify for both Medicare and Medicaid — doesn't behave like a typical Medicare prospect. Key characteristics that shaped the campaign from the start:


- Less likely to self-research online; more likely to respond to phone than form
- Primarily on mobile
- High consumers of YouTube and Instagram, not search


At the same time, tracking was stripped down to almost nothing. The signal environment the team had to work within:


- Meta pixels and third-party tags removed by payer compliance teams
- Legacy CDPs unable to capture micro-conversions (zip code entry, early funnel events)
- Enrollment-to-data lag of roughly two weeks — one week to enroll, another for the data to flow back
- No same-day feedback loop of any kind


**Standard optimization was off the table.** The campaign needed a targeting architecture that built on the few signals that hadn’t been removed.


### **2. Audience Strategy: Engineer a Human Profile From Observational Data**


The team approached the challenge as an audience problem rather than a channel problem, reflecting Amsive Health's[Audience Science®](https://www.amsive.com/services/strategy/audience-science/) approach to using a high-propensity audience framework to guide targeting, activation, and optimization decisions.


Without pixel-level signals, that meant building audience precision from the few platform and analytics signals that remained — engineering as much accuracy as possible at the front end instead of relying on individual journey tracking.


Jon calls it building a "human profile" — a composite of behavioral attributes that approximated the D-SNP-eligible population with enough fidelity to drive targeting decisions.


The **income** layer came first, using a platform-by-platform inverse strategy:


- Google Ads: Target the bottom 50% of household incomes directly
- Meta: Exclude the top 50% (the only income tier Meta exposes) to approximate the same population
- Trade Desk: Access all income brackets programmatically when budget and scope justified it


From there, the team layered in **observational behavioral signals** from Google Analytics — run across all income levels initially, then narrowed by:


- Device type (Android signals price-point sensitivity)
- Time-of-day patterns for lead activity
- Browser usage
- In-market and affinity categories (fast food, couponing, sports, political news — not luxury travel)


**Language** targeting was applied last and was the one variable that shifted meaningfully by market:


- Maryland metro: French (for Haitian Creole)
- Miami and San Diego: Spanish as a primary signal
- Device type, interests, and time-of-day patterns remained largely stable across markets


### **3. Channel Strategy: Match Platform Selection to How This Audience Behaves Online**


Channel selection for a D-SNP campaign isn't intuitive. The defaults — heavy search, broad social — don't match how this audience actually consumes media. The Amsive team made deliberate include/exclude decisions based on audience behavior, not habit.


**YouTube (primary awareness layer)**


Low-income seniors over-index on YouTube consumption. As a channel strategy, it's accurate, inexpensive, and gives early performance feedback — after one week of running, the team could see which content types and channels were driving engagement and refine targeting accordingly.


**CTV (television screens only)**


CTV was added as a complementary TV placement, but constrained to television screens exclusively — not the full CTV inventory that bundles in desktop and mobile. The audience watches television; reaching them on that device, versus another screen, was the point.


**Google Search (primary capture channel)**


For D-SNP specifically, heavy Google weighting made sense: the audience is mobile-first and Google dominates mobile search. Bing, by contrast, holds roughly 2% of mobile search market share and skews higher-income — the wrong audience and the wrong device. Bing budget was reallocated to Google entirely for D-SNP. (For standard Medicare campaigns with a broader 65+ audience, Bing is included — its desktop-heavy, higher-income skew is less of a liability there.)


**Meta (limited, workaround-dependent)**


Health plan advertising triggers Meta's Special Ad Categories, which restrict lookalike modeling. The team's workaround: pre-model lookalikes against CRM lists offline, then upload the result as a custom audience. It preserves some lookalike functionality without violating platform policy.


### **4. Measurement & Attribution: Two Models, One Campaign**


A D-SNP enrollment event takes roughly a week to complete — then another week for the data to surface in reporting systems. In practice, the team is always making spend decisions on signals that are one to two weeks old. Rather than treat this lag time as a bug, it became a structural constraint to design around.


Amsive's response was to run two attribution models simultaneously, for two different purposes:


Model Purpose


Last-click Client reporting — auditable, standard


Multi-touch Optimization decisions — sequence, channel contribution


The multi-touch model tracked whether early-funnel touchpoints (a CTV impression, a YouTube view) had any predictable relationship to downstream enrollment — and whether throttling spend on those channels moved conversion volume.


**Attribution windows shifted by season:**


- **Open enrollment (Oct–Dec 7):** Two-week lookback. Prospects are building awareness; they may see an ad today and act 10 days later. The longer window captures real intent.
- **Off-season:** One week. The only people acting are already motivated. A longer window introduces noise.


**Micro-conversions bridged the feedback gap:**


Because full enrollment data arrived two weeks late, the team optimized in near real-time against earlier funnel events — zip code entry, phone call initiation — as proxies for downstream performance. Imperfect, but the only signal available at the pace optimization required.


### **5. Results: Geography Audit Unlocks Spend Efficiency Without Adding Budget**


The campaign's clearest performance win came not from a new channel or creative test — it came from a zip code analysis.


After the campaign had been running, the team broke down performance at the zip code level. One zip code was consuming 20-25% of total campaign spend while producing a disproportionately low share of leads. The culprit: the zip code was dominated by a transit corridor, senior centers, and a hospital campus. High foot traffic, low enrollment intent. The people there weren't the people who needed a D-SNP plan.


Removing it from targeting freed up 20-25% of the budget with no loss of demand. That spend redistributed naturally into surrounding residential geographies — where the eligible population actually lived.


**The outcome:**


- **Market Specific cost per lead:** decreased by $17.15 (18%)
- **Market specific net lead volume:** Flat (-3)
- **Market specific spend:** -25%
- **Market specific clicks:** +25%
- **Market specific CPC:** -39%
- **Total cost per lead:** decreased $1.83 (3%)
- **Total net lead volume** : +4% (+33)
- **Total spend:** unchanged
- **Total clicks:** +19%
- **Total CPC:** -15%


### **Why This Campaign Works**


Payer enrollment campaigns operate inside constraints that most digital marketers haven't encountered at this level:


- compliance requirements that remove tracking infrastructure,
- conversion cycles that break real-time optimization, and
- audiences that don't behave like the default assumptions baked into platform targeting tools.


The conventional response to those constraints is to simplify — run search, report last-click, measure what's easy.


What this campaign demonstrates is that precision is still achievable without pixels. It requires a different kind of engineering — behavioral profiling built from the signals platforms do surface, channel selection based on online behaviors, and a willingness to interrogate geography at the zip code level rather than the metro level. The result a more durable form of targeting, built on understanding rather than instrumentation.


## 5 principles that payer marketers and agency teams running enrollment campaigns can apply in any market


**Jon's five tactical takeaways are:**


1. When pixel-based tracking is restricted, build audience precision at the front end through behavioral profiling from platform analytics.
2. Use income targeting platforms inversely — Google to target the bottom 50%, Meta to exclude the top 50% — to triangulate audiences that the platforms won't let you reach directly.
3. Keep your reporting attribution model and your decision attribution model separate; last-click and multi-touch serve different purposes.
4. Set attribution windows to match the enrollment cycle, not the calendar — and compress them during off-season when only high-intent prospects are acting.
5. Audit geography before creative: underperforming spend often lives in specific zip codes, not specific headlines.


Together, these principles can turn payer enrollment campaigns from blunt-instrument media buys into precision audience programs — even when compliance restrictions remove most of the precision tools.


### **Ready to Run Compliant Enrollment Campaigns Without Sacrificing Performance?**


The challenge Jon describes — wanting full-funnel visibility into what's actually driving enrollment while keeping the compliance team satisfied — is exactly what Freshpaint was built to solve. Freshpaint lets payer marketing teams capture the downstream conversion signals that CDPs and compliance restrictions have blocked, so you can optimize off real enrollment data instead of micro-conversion proxies.


- [Book a demo](https://www.freshpaint.io/contact-sales) to see how payer marketing teams are using Freshpaint to connect enrollment outcomes to the campaigns that drove them — without touching protected health information.
- See how Freshpaint helped one leading regional payer[reduce cost per lead by 40%](https://www.freshpaint.io/case-studies/how-freshpaint-clears-the-way-for-a-health-plans-digital-marketing) .


‍
