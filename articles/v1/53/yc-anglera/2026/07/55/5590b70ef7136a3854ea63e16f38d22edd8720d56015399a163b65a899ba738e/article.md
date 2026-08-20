---
schema_version: "1.0.0"
document_id: "5590b70ef7136a3854ea63e16f38d22edd8720d56015399a163b65a899ba738e"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/cross-sell-catalog-completeness-2026"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-08-06T19:14:01.668988+00:00"
fetched_at: "2026-08-06T19:14:09.920895+00:00"
content_hash: "sha256:9dc892da135985f3f034f37304a4a62aac92af19044d40f25a0177dfc83e2e13"
---

# Cross-Selling Fails in the Catalog, Not in the Call

Distribution Strategy Group has published cross-sell advice on a roughly two-year cycle since 2012, and share-of-wallet in existing accounts is stuck almost exactly where it started. That's not evidence reps need another playbook. It's evidence the playbook has been diagnosing the wrong organ. Cross-selling doesn't fail in the call. It fails upstream, in the catalog, before a rep or a self-serve customer ever gets a chance to attach anything — and that failure is measurable, category by category, if you know where to look.


## The same advice, four times


Line up[Distribution Strategy Group's](https://distributionstrategy.com/) coverage of this problem and the diagnosis barely moves. The[2012 piece](https://distributionstrategy.com/2012/05/the-economics-of-cross-selling/) makes the economic case — "once a customer has been acquired, there is nominal incremental cost to sell other products" — and points reps toward market-basket analysis: support and confidence scores, twenty or thirty well-chosen rules, the diapers-and-beer story. The[2020 follow-up](https://distributionstrategy.com/2020/09/why-is-cross-selling-effectively-so-difficult-for-distributors/) names the actual bottleneck almost by accident: "there's just no good way to push critical cross-selling opportunities to a rep," and lands on AI recommendation software as the fix. The[2022 piece](https://distributionstrategy.com/2022/12/why-cross-selling-is-hard-and-4-actionable-ways-to-make-it-easier/) and the[2025 roundup](https://distributionstrategy.com/2025/04/8-upselling-and-cross-selling-strategies-that-drive-sales-growth/) repeat the pattern with more tactics layered on: training, incentive redesign, better CRM prompts, tighter playbooks.


Every one of those prescriptions assumes the same thing: that somewhere behind the rep or the recommendation engine sits a catalog with the raw material to support an attach. None of them interrogate that assumption. That's the gap thirteen years of the same advice should have forced open by now.


## Where the attach actually breaks


A rep can't recommend a fitting he can't find. A recommendation engine can't score a "bought together" pair if one of the two SKUs has no category, no spec attributes, and no relationship to anything else in the catalog — it's just a part number sitting in the database, structurally invisible to any rule the 2012 piece describes. And a self-serve buyer, who is doing more of this alone every year, never even reaches the rep to be rescued by his product knowledge.[Gartner's most recent sales survey](https://www.gartner.com/en/newsroom/press-releases/2026-03-09-gartner-sales-survey-finds-67-percent-of-b2b-buyers-prefer-a-rep-free-experience) puts the share of B2B buyers who prefer a rep-free purchase at 67%, consistent with the broader finding that buyers now complete the large majority of a purchase journey before a supplier rep is meaningfully involved. If your site lists the adjacent category as a bare SKU — no image, no attributes, no "works with" — that buyer doesn't call anyone to ask what fits. He searches, finds nothing usable, and orders the one line he already knew he needed from you, then buys the rest from whoever's site actually answered the question.


This is not a fringe problem. Industry surveys compiled by[Swell](https://www.swell.is/content/b2b-wholesale-ecommerce-statistics) put the share of B2B sellers admitting their product data is outdated or incomplete at 83%, with more than 30% of B2B online orders containing some kind of error traceable to bad data. That is the population DSG's advice has been trying to coach around for a decade — reps and algorithms, straining against a catalog that hasn't been asked to do its half of the job.


## The mechanism is measurable, not anecdotal


Here's the useful reframe: cross-sell isn't a behavior problem to manage, it's a data-completeness problem you can score. Take fill rate by category — the share of SKUs in a category with complete specs, imagery, and cross-references — and plot it against wallet-share by category, the share of a customer's spend in that category you actually capture versus what they buy elsewhere. Distributors rarely run this comparison, because fill rate lives in the PIM team's dashboard and wallet share lives in the sales VP's dashboard, and nobody sits in both chairs.


When we built the[Top Distributors 2026 index](https://anglera.com/blog/top-distributors-2026) — measuring over 200 distributors against a[Digital Readiness Index](https://anglera.com/blog/top-distributors-2026/methodology) across 14 signals scraped from their own live sites — this pattern showed up unprompted. The distributors that win in adjacent, non-core categories are consistently the ones whose weakest categories are still fully merchandised: complete specs, real imagery, cross-references intact, even in the long tail nobody prioritizes. The distributors bleeding share in adjacent categories tend to have one thing in common — their secondary lines look like an afterthought online, no matter how well-trained the reps selling them happen to be.


Lever the trade press reaches for What it assumes What it can't fix


Rep training / playbooks Rep already knows what's compatible A SKU with no attributes to reference


Comp plan redesign Motivation is the constraint A search that returns nothing


CRM prompts / AI recommendations The underlying data is scoreable A part number with no category, spec, or relationship


Catalog completion — This is the constraint the other three sit on top of


## What "complete" actually means here


Completing a SKU isn't a data-hygiene chore — it's the literal mechanism cross-sell runs on. The 2012 article's association rule ("customers who buy X also buy Y") can only become a live recommendation, a merchandised bundle, or a rep's talking point if X and Y are both specified well enough for the relationship to render — a "works with," a compatible fitting, a shared use-case tag. Strip the attributes and the rule has nothing to attach to. It stays a slide in a training deck.


This is squarely inside the enrichment work we do. Anglera doesn't replace the PIM a distributor already runs — your PIM stores the data, we do the work of filling it: specs, imagery, and the cross-SKU associations that make a category self-serve and rep-ready at the same time, starting from whatever's already on hand, including a flat export. Distributors typically see it live inside a month. Before the next round of cross-sell training gets scheduled, it's worth running the cheaper diagnostic first: pull fill rate by category, lay it against wallet share, and see how much of the "rep problem" turns out to live in the catalog instead.
