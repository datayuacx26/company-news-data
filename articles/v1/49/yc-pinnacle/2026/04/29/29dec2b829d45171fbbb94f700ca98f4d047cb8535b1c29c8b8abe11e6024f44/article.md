---
schema_version: "1.0.0"
document_id: "29dec2b829d45171fbbb94f700ca98f4d047cb8535b1c29c8b8abe11e6024f44"
company_key: "yc-pinnacle"
company: "Pinnacle"
source_id: "yc-pinnacle-news-import-eb12d43fa2a4"
canonical_url: "https://pinnacle.sh/blog/sms-restricted-keywords"
published_at: "2026-04-29T00:00:00+00:00"
first_seen_at: "2026-07-24T09:16:03.254405+00:00"
fetched_at: "2026-07-28T22:15:34.522080+00:00"
content_hash: "sha256:0bbe9a9675fa13da22a84213d800a54db5186cb75238338c5ec953fe7ba599a5"
---

# SMS Carrier Filtering: What Actually Blocks Your Messages and How to Stop It

## The Filter Wall Nobody Tells You About


You registered your campaign, wrote your templates, and started sending. Your dashboard says "delivered to carrier." The recipient never got the message. No bounce. No useful error code. Nothing.


Welcome to carrier-level content filtering — the single most common cause of phantom-failed SMS we see at Pinnacle. There is no master list of forbidden words anywhere in public, and anyone telling you otherwise is selling an approximation. What CTIA and T-Mobile actually publish is a pile of category rules, severity tiers, and behavioral patterns. Your sender-side filter has to mirror that shape; it cannot pretend to match strings.


## What Carriers Actually Filter


US filtering hangs off **SHAFT** — Sex, Hate, Alcohol, Firearms, Tobacco. CTIA reserves its strictest tier (Severity 0) for SHAFT content that's federally illegal, age-restricted SHAFT sent without an actual age gate, and anything hateful, violent, or designed to incite.


Past SHAFT, the major enforcement categories are cannabinoid retail and adjacent products, controlled substances and prescription pharmaceuticals, and the long tail of phishing-pattern phrases (fake delivery notices, account-suspension scares, that kind of thing).


## Who Publishes What


T-Mobile is the most actionable public source — they enumerate restricted categories with examples and they're explicit about anti-evasion behavior. CTIA acts as the industry floor (the rules the major carriers agree on). Verizon's public posture is essentially "we comply with CTIA" rather than running its own published filter rules. AT&T sits at high-level AUP language.


Source What's public Token-level list? Practical takeaway


**CTIA** SHAFT framework, Severity 0 tier, opt-in / STOP / HELP requirements No Industry baseline. Treat as the floor.


**T-Mobile** Code of Conduct names illicit content, obscene language, hate, violence, controlled substances; explicit anti-evasion rules; age-gating requirements No Strictest enforcement. Clearest public source on anti-evasion behavior.


**Verizon** "Comply with CTIA"; reserves the right to filter generally No Defers to CTIA rather than publishing proprietary content rules.


**AT&T** High-level AUP language; warnings against misspelling and unusual capitalization No Broad-policy oriented rather than term-list oriented.


Anyone publishing a "master carrier blocklist" is reverse-engineering from the materials in column two. There is no document to leak.


The category-by-category reference below is compiled from the same public materials, hidden by default so this page doesn't trip generic content-policy crawlers. Use it to audit your own templates.


⚠️


### SHAFT Family-Level Reference (hidden by default)


This is a family-level taxonomy compiled from public materials — **not** a carrier-published master blocklist (none exists in public). The list stays hidden by default so this page doesn't register against generic spam heuristics or content-policy crawlers. Reveal it to copy categories into your own template-audit tooling.


## Why Token Lists Don't Hold Up


Plain matching stumbles on context. "Free" in "free shipping with your order" reads as benign. "Free" in "FREE iPhone, click here" reads as a phishing pattern. The same string passes one campaign and fails another, and a regex never knows the difference.


It also gets evaded the moment your list goes public. Inserted spaces, character swaps, homoglyphs, leetspeak — the attacker reads your list once and never matches it again.


Worst, trying to dodge filters is itself prohibited. T-Mobile bans filter-evasion assistance, snowshoeing, URL cycling, redirects, and number cycling. AT&T explicitly warns against misspelling and unusual capitalization. The major US messaging policies forbid intentional misspelling of any kind. A sender-side filter that quietly helps you work around carrier rules is itself a Severity 0 risk.


Which is why the public rules describe categories and behaviors, not exhaustive token regexes.


## What Drives Delivery


Filtering is contextual and behavioral. Whether your message reaches the handset depends on a stack of checks that compose with the content of the message itself:


1. **Consent and registration.** Direct opt-in tied to the end business. Campaign registered with TCR. Use-case description matches your live traffic. If any one of these is weak, content rules don't matter; the campaign is already at risk.
2. **Category fit.** Does the message touch a SHAFT family or an adjacent restricted category? Is that category even allowed on your route?
3. **Age-gating.** Real DOB-based verification, not a yes/no popup. CTIA and T-Mobile both require it for anything age-restricted, and the major US messaging-policy frameworks reject simple yes/no age confirmations.
4. **URL hygiene.** Branded short domain. No public shorteners (bit.ly, tinyurl, t.co — heuristically blocked on 10DLC). HTTPS only. Domain over 30 days old. No chained redirects.
5. **Alignment.** Live message, sample messages submitted at registration, campaign metadata, and linked website all describe the same use case. Drift between any two triggers review.
6. **No evasion.** No misspellings to dodge filters. No spacing tricks. No homoglyphs. No number cycling. No snowshoeing.
7. **Ongoing monitoring.** Per-carrier delivery rates, opt-out rates, complaint rates. Spikes get a campaign throttled before any individual message blocks.


Skipping one of these and getting the others right won't compensate — carrier review usually catches the gap.


## Practical Mitigation Cheat Sheet


Risk Unsafe pattern Safer mitigation


Restricted product promotion Direct SMS promotion of regulated SHAFT or controlled-substance categories Move promotion off SMS, or use SMS only for neutral account/status messaging after lawful consent and (where applicable) DOB verification


Obscene or offensive tone Conversational templates with obscene language, insults, or violent rhetoric Clean, neutral templates with explicit brand identification and STOP/HELP language


Token-level evasion Misspellings, spacing tricks, homoglyph substitution, masked obscenities, "creative" URL behavior Ban obfuscation in your sender policy; lint at template time; reject anything that looks designed to evade filters


Campaign drift Sample messages describe one use case, live traffic does another Keep registration metadata, sample content, website, and live traffic in sync; review template changes before send


URL-based blocking Public shorteners, redirects, mismatched or freshly-registered domains Branded custom short domain; HTTPS; no chained redirects; landing page matches use case


Consent risk Shared, rented, or brokered opt-in lists Direct end-business consent only; never transfer consent across affiliates


If you're building the actual pre-send filter implementation, see[Building an SMS Pre-Send Filter](https://www.pinnacle.sh/blog/sms-pre-send-filter) for the three-tier architecture, normalization stack, and code.


## Secondary Public Lexicons


Audit teams sometimes want broader recall than a family-level taxonomy provides — for catching obscure euphemisms, controlled-substance aliases, or non-English variants in inbound moderation pipelines. Three resources are widely cited. **None of them is a carrier blocklist.** Treat them as recall amplifiers for internal review, not as send-time enforcement rules.


Resource What it provides Useful for Why it isn't enough alone


**[Hatebase](https://hatebase.org/)** Multilingual hate-speech archive with contextual tooling (active service retired; archive remains browsable) Identifying hate-term families across languages and geographies, especially for inbound moderation Not carrier-specific. Heavily context-dependent. Reclaimed and ambiguous terms require human judgment.


**[DEA slang and code-word reports](https://www.dea.gov/sites/default/files/2018-07/DIR-022-18.pdf)** Controlled-substance slang and code-word coverage published by the US Drug Enforcement Administration Understanding the breadth of regulated-substance aliases, and why carrier rules ban product classes rather than chasing slang Not a carrier rule. Not an SMS allow/deny list. A surface match is not by itself a block.


**[LDNOOBW](https://github.com/LDNOOBW/List-of-Dirty-Naughty-Obscene-and-Otherwise-Bad-Words)** Open-source list of obscene and otherwise prohibited language, originally used to filter autocomplete and recommendations Reasonable seed list for internal QA of offensive language across many locales Maintainers explicitly note inclusion is subjective and culture-dependent. Not a carrier blocklist.


Pinnacle's content review pipeline blends these public lexicons with carrier-specific rejection-pattern intelligence we maintain internally. The public lists alone would either over-block legitimate copy or miss patterns that aren't documented anywhere.


## Compliant Templates


The single best template pattern is **boring on purpose** : brand name first, neutral payload second, support and opt-out language last. Every one of these passes carrier review:


text


```text
Acme Dental: Reminder—your appointment is tomorrow at 2:30 PM.
Reply C to confirm or call 555-0100 to reschedule.
Reply STOP to opt out.
```


text


```text
Acme Store: Your order 48291 has shipped.
Track at acme.example/track/48291.
Reply STOP to opt out of shipping alerts.
```


text


```text
Acme Bank: Your security code is 482913.
It expires in 10 minutes. Do not share this code with anyone.
```


All three examples share the same shape: an identified sender, one purpose per message, a branded short domain instead of a public shortener, and clean STOP/HELP language. CTIA and T-Mobile both prefer this over stylized or promotional copy.


## Caveats


Most of this is reliable for US and Canada A2P, where the deepest public materials live. The honest gaps: Verizon's public posture is "comply with CTIA" rather than detailed proprietary content rules; AT&T sits at broad acceptable-use language; EU and UK regimes are less specific in their public docs, so international filter intelligence comes from operator-by-operator empirical patterns rather than published specs.


That's why the advice above is built around intent, linked content, and behavioral controls rather than a master dictionary.


## How Pinnacle Handles It


Pinnacle's API runs the entire pipeline before a message ever touches the carrier. Specifically:


- **Pre-send template scanning at the API layer.** The` /messages` endpoint matches templates against the family-tier policy at send time. Block-tier matches return a` 400` with the matched family. Flag-tier matches return` 202 Accepted` and hold the message in a review queue.
- **Normalization built in.** Unicode confusables, whitespace collapsing, leetspeak detection, and zero-width-char stripping all run before family matching. Your code doesn't have to think about evasion patterns.
- **Per-carrier delivery analytics.** The dashboard separates "accepted by carrier" from "delivered to handset" and breaks divergence down by carrier. Silent T-Mobile filtering surfaces in minutes, not days.
- **Branded short domains.** Every Pinnacle account ships with a custom short domain. No` bit.ly` traffic, ever.
- **Anti-evasion lint.** Messages with misspelling-to-evade, homoglyph substitution, or number/URL cycling get rejected at the API to protect your sender reputation across the entire 10DLC ecosystem.
- **Vertical templates.** For healthcare, financial services, and other age-restricted SHAFT verticals, our compliance team maintains template patterns that pass review on the first submission.


Building this from scratch is roughly a quarter of engineering plus a year of feedback from real carrier rejections. We've spent that time already.


## Book a Call


A lot of A2P stacks in production today were built for an older compliance landscape. The teams whose messages still deliver reliably are the ones who fold compliance into the messaging stack itself, instead of running it as a separate audit project.


We've worked with teams across SMS-first startups, healthcare and fintech programs, and regulated retail.[Book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+SMS+carrier+filtering+and+pre-send+pipeline) . We'll go through your sender setup, look at how our pre-send filter handles your actual templates, and put you on a configuration that delivers.


## Key Takeaways


- There is no public exhaustive carrier blocklist. CTIA and T-Mobile publish the most specific public guidance; AT&T and Verizon stay at broad acceptable-use language.
- The categories that matter: SHAFT (sex, hate, alcohol, firearms, tobacco), cannabinoid retail, controlled-substance promotion, and phishing patterns.
- Filtering is contextual: consent, registration, age-gating, URL hygiene, alignment, anti-evasion, and per-carrier monitoring all compose with content rules.
- Trying to evade filters is itself a Severity 0 risk.
- Pinnacle's API ships the entire pipeline so most senders don't need to build their own.


## FAQ


**1. Will Pinnacle tell me if my message got filtered?** Yes. The dashboard distinguishes "accepted by carrier" from "delivered to handset" with per-carrier breakdowns. Silent drops on a single carrier surface within minutes.


**2. Can I send messages about cannabinoid retail products?** On US/Canada A2P routes, cannabinoid content is filtered regardless of state legality. The compliant path is moving promotional content off SMS and using SMS only for neutral account or status messaging. Our team works with regulated-industry senders on the specific patterns that thread that needle.


**3. Does this also apply to RCS?** RCS messages from a registered, branded sender are far less aggressively filtered, because the sender is verified end-to-end. Same SHAFT category rules, much lower false-positive rate. See our[RCS for finance](https://www.pinnacle.sh/blog/rcs-for-finance) ,[healthcare](https://www.pinnacle.sh/blog/rcs-for-healthcare) , and[e-commerce](https://www.pinnacle.sh/blog/rcs-for-ecommerce) guides.


**4. What's the difference between Severity 0 and a regular rejection?** Severity 0 (CTIA's strictest tier) covers federally illegal SHAFT, age-restricted content sent without a real age gate, and hateful or violent content. A Severity 0 violation can result in immediate brand-level bans across the 10DLC ecosystem, not just a single message reject. Other rejections affect deliverability and sender reputation but are recoverable with template fixes.


**5. Do international carriers filter the same way?** Filter behavior varies significantly by country and operator. The public record is strongest for US/Canada A2P (CTIA plus the Tier-1 operators plus the major US messaging-policy frameworks). EU and UK regimes are higher-level in public materials, so international filter intelligence comes from operator-by-operator empirical patterns. Pinnacle maintains filter intelligence per market.


**6. How do I know if my campaign is at risk before I send?**[Talk to us](https://www.pinnacle.sh/contact?message=I%27d%20like%20a%20review%20of%20my%20SMS%20program%20for%20keyword%20and%20compliance%20risk.) . We'll review your templates, registration metadata, and landing pages against the SHAFT framework and the per-carrier rejection patterns we see across our network.
