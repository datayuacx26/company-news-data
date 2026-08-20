---
schema_version: "1.0.0"
document_id: "91d9e22a8fe25b63732894be60638e81eb5cd54b75c4b00327cff5956fdb2dc8"
company_key: "yc-middleware"
company: "Middleware"
source_id: "yc-middleware-news-import-fe54c9028845"
canonical_url: "https://middleware.io/blog/sentry-pricing/"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T00:45:27.941478+00:00"
fetched_at: "2026-07-30T00:45:30.238731+00:00"
content_hash: "sha256:fa6910d12c5800f154b04bb91d9324b7d185778827aec976da2462279b1e4ae4"
---

# Sentry Pricing 2026: Full Cost Breakdown by Plan & Team Size

Sentry bills differently than most observability platforms: instead of metering data volume, it meters events, errors, spans, replays, logs, and attachments each with its own quota and its own overage rate. Overages on the Team plan run from $0.0003625 per error in the 50k–100k tier down to $0.0001500 per error at 20M+ events, while Business tier overage rates run higher, from $0.0011125 per error down to $0.0003000 at the top tier. At low volume this is genuinely predictable. Past a[certain scale](https://checkthat.ai/brands/sentry/pricing) , the number of separately-metered categories starts to matter; a single noisy deploy can spike one category without warning.


### No per-event billing. No overage cliffs by category. One rate per GB.


Compare your usage free for 14 days


#### Key takeaways


- Sentry bills across five separately-metered categories errors, spans, replays, logs, and attachments each with its own included quota and overage rate.
- A team generating ~300,000 errors/month on the Business plan pays roughly $138/month in errors alone, before spans, replays, or logs.
- The median Sentry buyer pays about $950/year, per Vendr’s transaction data but that figure covers mostly small teams; volume and category count push costs up fast from there.
- Sentry’s Seer AI debugging feature costs $40/month per active code contributor, billed separately from base plans.
- Middleware prices on data ingestion only (a flat per-GB rate across metrics, logs, and traces) with no per-event category structure and no per-seat charge.


Table of Contents


### The math worth running


Model a mid-sized team on Sentry’s Business plan ($80/month, needed for SSO and advanced quota management): 300,000 errors/month.


- Base: $80/month
- Error overage on Business tier at published PAYG rates: 200,000 errors billable beyond the base quota, at roughly $0.000290/error ≈ $58/month
- Total (errors only): ~$138/month


That figure doesn’t include the other metered categories. Team and Business plans both include 50K errors, 5M spans, 50 replays, 5GB logs, and 5GB application metrics as their base quota; a team running session replay broadly or shipping verbose logs adds cost on top of the error total above. Logs beyond the included 5GB run $0.50/GB, and session replays beyond the included 50 run about $0.00375 each.


At enterprise scale, the picture is directionally similar but wider: mid-sized organizations processing 500K – 2M events/month typically see $6,000 – $24,000 annually, while larger enterprises with 10M+ events/month and Business tier requirements commonly negotiate contracts in the $50,000–$150,000+ range. Real Enterprise deals are individually negotiated, so treat the top end as directional rather than a quote.


### What Sentry actually is


Sentry is a developer-first error tracking and[performance monitoring](https://middleware.io/blog/what-is-application-performance-monitoring/) platform that has expanded from pure error tracking into a broader toolset covering distributed tracing, session replay, structured logs, and continuous profiling, all tied back to the specific commit that caused an issue. It supports over 100 languages and frameworks with SDKs for web, mobile, and even game consoles.


### What you actually get at each tier


**Tier** **Cost** **Included Quota** **Users** **Best For**


Developer $0/month 5,000 errors/month 1 user Solo developers, side projects


Team $26/month (annual) / $29 (monthly) 50K errors, 5M spans, 50 replays, 5GB logs, 5GB metrics Unlimited Small teams, predictable low-to-moderate volume


Business $89/month (monthly) / $80 (annual) Same base quota as Team, higher overage rates Unlimited Teams needing SSO, audit logs, cross-team workflows


Enterprise Custom Custom, negotiated Custom Compliance-heavy, high-volume organizations


### Where the pricing model gets complicated


- **Five categories, five meters** – Errors, transactions, session replays, and performance events are all billed separately, and different event types carry different per-unit prices; errors are typically the most expensive per unit.
- **Replay costs scale faster than teams expect** – Sentry counts a new browser tab as a new session, which can push actual replay consumption 2 – 3x higher than initial estimates.
- **Sampling controls aren’t fully reliable** – Multiple users report SDK-level rate limiting and sampling were ineffective at controlling event volume, leading to higher-than-expected bills.
- **A single bad deploy can spike the bill** – One team reported a 3.2x bill spike after hitting 1.4 million events in a single day.
- **Seer AI is metered per contributor, not per team** – It costs $40/month per active code contributor, defined as anyone making two or more pull requests to a Seer-enabled repo in a billing cycle.


### Why teams evaluate alternatives


None of this makes Sentry a weak product; it holds a 4.5/5 rating on G2 from 212 reviews and a 4.6/5 on Capterra, and its developer-workflow integrations are genuinely well-liked. The issue is scope and predictability: Sentry is built around code-level error tracking first, with infrastructure and full-stack visibility bolted on more recently, and its per-category event billing means cost doesn’t move in one predictable direction as usage grows. A few signals it’s worth comparing options:


- Engineering time is going into tuning sample rates and quota management across five separate meters, rather than debugging.
- Teams report spending real time adjusting sample rates per environment and managing per-project quotas work that has nothing to do with debugging applications.
- You need infrastructure and log correlation alongside errors, and are running Sentry next to a second tool to get it.


### Sentry vs. Middleware


**Sentry** **Middleware**


Pricing axes Five separate meters (errors, spans, replays, logs, attachments) One data ingested (GB)


Free tier 5,000 errors/month, 1 user, permanent 14-day trial, unlimited ingestion


Per-seat cost $0 (unlimited users on paid plans) $0 (unlimited users, all plans)


Core rate $0.0001500 – $0.0011125 per error, plus separate rates per category $0.30/GB flat for logs, metrics, and traces


Scope Error tracking, tracing, replay, logs code-focused Full-stack: APM, infrastructure, logs, metrics, traces, RUM in one OTel-native agent.


Best fit Teams whose core workflow is code-level debugging tied to commits, working in GitHub/Jira/Slack Teams that want errors correlated with infrastructure, logs, and traces in one bill


Also read about[New Relic Vs Sentry](https://middleware.io/blog/newrelic-vs-sentry/)


### Where Middleware differs


Sentry and Middleware meter usage on fundamentally different units events vs. gigabytes so a direct like-for-like recompute of the scenario above isn’t apples-to-apples, and it would be misleading to force one. What’s fair to say instead: a 10 – 15 person team running full-stack observability (not just error tracking) on Middleware pays a single flat rate against total data volume, with no separate meter for replays, spans, or attachments to manage.


[Middleware’s pricing](https://middleware.io/pricing/) includes a free tier with a 14-day free trial and unlimited data ingestion.


> “Middleware isn’t just another company’s profit machine; it’s a platform that actually works with us”
>
>
> Elijah Smith, Software Engineer III, Generation Esports.


Read how Middleware helped[Generation Esports](https://middleware.io/customers/generationesports/) slash observability costs and improve MTTR by 75%


## Wrapping up


Sentry’s pricing model isn’t complicated by accident, it’s a byproduct of metering five different things (errors, spans, replays, logs, attachments) instead of one. That gives fine-grained cost attribution, but it also means bill variance is baked in: a noisy deploy, a broader replay rollout, or a verbose logging change can each move the number independently, and someone ends up owning quota tuning across five dashboards instead of debugging. Middleware collapses that to a single ingestion-based rate, so the tradeoff comes down to a real question: does your team want per-category precision, or one predictable number to plan against?


## FAQs


### What's the real difference between Team and Business?


Both include the same base event quotas; the price difference buys SSO, audit logs, longer data retention, and cross-team workflows Business also carries higher overage rates on some categories.


### Why does my Sentry bill fluctuate month to month?


Because you’re billed across five independently variable meters at once a noisy release can spike errors and span in the same month a broader replay rollout spikes that category too.


### Does Middleware track errors the same way Sentry does?


Middleware surfaces errors as part of its unified APM and log timeline rather than as a dedicated, commit-linked error-tracking workflow team whose primary daily tool is deep, code-level error triage tied to GitHub/Jira should weigh that difference.


### How do I estimate what switching would actually save?


Pull your current monthly totals across all five Sentry meters (errors, spans, replays, logs, attachments) and convert to an approximate data volume Middleware’s team can help model this during a trial, since it’s not a 1:1 unit conversion.
