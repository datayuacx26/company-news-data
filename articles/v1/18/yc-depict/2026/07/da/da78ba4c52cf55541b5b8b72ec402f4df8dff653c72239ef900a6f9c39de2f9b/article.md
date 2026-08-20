---
schema_version: "1.0.0"
document_id: "da78ba4c52cf55541b5b8b72ec402f4df8dff653c72239ef900a6f9c39de2f9b"
company_key: "yc-depict"
company: "Depict"
source_id: "yc-depict-news-import-c8e412319241"
canonical_url: "https://depict.ai/resources/guides/shopify-bot-traffic"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-24T04:11:03.956705+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:d669b65d7ba8f129777e2473d4539b856ab821465ff3da36ffa65b081690905a"
---

# How to actually block Shopify bot traffic | Depict

*Last verified July 2026. Cloudflare and Shopify both change their dashboards and policies often; where this guide describes a setting, we describe what it does, not where the button sits this month.*


One merchant we work with looked at their traffic report and found that around 70% of it came from China. They do not sell there, do not ship there, and do not advertise there. Orders from that traffic: zero. Another merchant counted 250,000 sessions in a month and concluded that roughly 215,000 of them were bots.


Neither store is unusual. During the 2024 holiday season, bots accounted for 57% of ecommerce website traffic, the first time automated traffic outweighed human shoppers, according to[Radware's 2025 E-commerce Bot Threat Report](https://www.globenewswire.com/news-release/2025/04/23/3066593/0/en/Radware-Finds-57-of-Online-Shopping-Traffic-Now-Bots-Not-Buyers.html) . That figure counts all bots, good and bad. Counting only the bad ones,[Imperva's 2025 Bad Bot Report](https://www.imperva.com/resources/wp-content/uploads/sites/6/reports/2025-Bad-Bot-Report.pdf) puts malicious bots at 33% of retail site traffic in 2024, up from 26% the year before. By June 2026, Cloudflare's CEO[reported](https://www.tomshardware.com/tech-industry/artificial-intelligence/bots-have-now-passed-human-traffic-online-cloudflare-boss-laments-says-agentic-traffic-wasnt-expected-to-eclipse-real-people-until-next-year) that automated systems had passed humans in HTML requests across Cloudflare's network, at 57.5%.


If your store is on Shopify, this traffic is not just noise in a chart. It inflates every tool you pay for by the session. It fills your analytics with visits that will never buy, which quietly corrupts every decision you make from those numbers. It feeds junk signals to your ad pixels, so Meta and Google optimize toward more junk. And in late 2025, a wave of exactly this hit thousands of Shopify stores at once, filling the[Shopify community forums](https://community.shopify.com/t/massive-chinese-bot-traffic-is-corrupting-shopify-analytics-ad-attribution-and-paid-tools-plus-merchants-affected/581112) with merchants describing traffic that was 50 to 90% bots, from countries they had never sold to.


The fix is real, and it is not an app. It is putting a free Cloudflare account in front of your store and filtering traffic at the network edge, before it ever reaches Shopify. The setup involves one genuinely nerve-racking step, moving your DNS, and a handful of decisions that are easy to get wrong, including one that can silently make you invisible to the AI assistants that are starting to send real, paying shoppers.


This guide walks through all of it: how to measure your bot share, which bots you should deliberately keep, which strategy fits your store, the DNS move with a pre-flight checklist and a rollback plan, and how to confirm afterward that nothing broke. Nothing here requires a developer, though if you have one, the DNS chapter is the part to hand them.


## Your dashboard is lying to you, and it costs you money


Start with why this is worth an afternoon of your time, because "my sessions are inflated" undersells it.


**Tools billed by traffic.** A chunk of the Shopify app ecosystem prices by sessions or pageviews.[Lucky Orange](https://www.luckyorange.com/pricing) , a popular heatmap and session recording app, runs from $32 per month for 3,500 sessions to $839 per month for 300,000.[OptiMonk](https://www.optimonk.com/pricing/) prices by pageviews. Analytics tools like Matomo bill by hits. These vendors filter known bots, but the recent waves use residential proxies and human-like browsing precisely to evade that filtering. If 60% of your sessions are bots, you may be paying tier prices for an audience that is 60% nobody. In the community threads from the 2025 wave, merchants reported exactly this: usage-based bills climbing on traffic that could never buy.


**Poisoned ad signals.** Your Meta and Google pixels learn from the traffic they see. Bot sessions with 100% bounce and zero purchase intent teach the algorithm the wrong lessons, and if bots click your paid ads, the budget burns directly. In the community threads from the 2025 wave, corrupted Meta Pixel learning was the damage merchants named most often, ahead of the analytics themselves.


**Decisions made on fiction.** Conversion rate is orders divided by sessions. Flood the denominator and your conversion rate craters, your geography reports point at markets that do not exist, and your channel attribution drifts. Imperva's retail report literally names "analytics skewing" as a standard bad-bot activity. If you merchandise from your data, bury what does not move, boost what does, this is the quiet cost: the data itself goes bad.


**Load nobody pays for gladly.** Every bot pageview is served by real infrastructure. On standard Shopify plans bandwidth is Shopify's problem, but every tool in your stack that meters usage feels it, and for headless stores the bandwidth line is direct and immediate.


How much does this add up to for your store? Later in this guide there isa calculator : pick which of your tools bill by traffic, plug in your session count and bot share, and see the annual number.


## Find out how bad it is


Before changing anything, measure. You may find your bot share is 8%, in which case close this tab and go do something more useful. The merchants who need this guide usually find something closer to half.


**In Shopify admin:**


- Open the sessions-by-location report and put it next to orders by location. A country that produces thousands of sessions and zero orders is the classic signature. Shopify's own guidance on[identifying bot activity](https://help.shopify.com/en/manual/intro-to-shopify/bots/identifying-bot-activity) says to watch for sessions from known datacenter locations, and names Ashburn, Virginia and Los Angeles; merchants in the community add Council Bluffs, Iowa, home of a Google Cloud region.
- Since October 2025, Shopify reports include a "Human or bot session" dimension you can filter by ([Shopify docs](https://help.shopify.com/en/manual/intro-to-shopify/bots/bot-filtering) ). Use it, but know its limits: it only covers data from October 7, 2025 onward, it is deliberately conservative (Shopify says it would rather miss bots than mislabel a real customer), and it does not exist for headless storefronts. Treat its number as your floor, not your total.
- Check your store search terms report for queries that look like machine-generated strings, and your customer list for garbage names. Shopify lists both as bot tells.
- Look at 30 to 90 day windows. Short windows swing too much to read.


**In GA4:** Google already filters known bots automatically, so anything suspicious you can still see has already slipped past that first filter. The composite signature that survives is engagement rate near zero, plus source showing Direct or "(not set)", plus a country outside your markets. Any one of those alone is weak evidence. All three together, at volume, is close to certain.


**One structural caveat.** Shopify analytics, GA4, and your pixels are all JavaScript counters running in the visitor's browser. A bot that never executes JavaScript is invisible to all of them while still loading your pages. Whatever number your dashboards show you, the network edge sees more. You will get your first honest count after the Cloudflare setup below, from its traffic analytics, and it is normal for that count to be a shock.


Write your numbers down: total sessions, suspected bot share, and which countries or patterns dominate. The strategy choice below depends on them, and the before-and-after comparison is how you will know the work paid off.


## The bots you want to keep


Here is the part every "block the bots" tutorial skips, and the reason you should not simply turn every defense to maximum.


Some bots are the plumbing of your discoverability. Googlebot and Bingbot decide whether you exist in search. Meta's link-preview crawler decides whether your products unfurl properly when someone shares them. And a newer class matters more every quarter: the crawlers and fetchers behind AI assistants.


They are not one thing. AI bots come in three kinds, and confusing them is how stores go dark by accident:


- **Training crawlers** (OpenAI's GPTBot, Anthropic's ClaudeBot, Meta's meta-externalagent) collect content to train future models. Blocking them is a legitimate business choice with no effect on your visibility today.
- **Search indexers** (OAI-SearchBot, Claude-SearchBot, PerplexityBot) build the indexes AI assistants answer from. OpenAI's own documentation is blunt: sites opted out of OAI-SearchBot "will not be shown in ChatGPT search answers" ([OpenAI bot docs](https://developers.openai.com/api/docs/bots) ).
- **User-triggered fetchers** (ChatGPT-User, Claude-User, Perplexity-User) retrieve a page because a human asked a question right now. Block these and the assistant cannot read your product page at the exact moment a shopper asks "is this jacket true to size and in stock?"


Does that traffic matter yet?[Adobe's analytics of US retail sites](https://business.adobe.com/assets/pdfs/resources/reports/ai-traffic-report/ai-sourced-traffic-insights-2025.pdf) found AI-referred traffic up 693% year over year during holiday 2025, and, more interesting, those visitors converted 31% better than the average visitor. That is a reversal: a year earlier the same measurement showed AI referrals converting worse. The channel went from curiosity to quality in about twelve months. It is still small in absolute terms, but it is the only referral channel growing at that rate, and the shoppers it sends arrive pre-sold by an assistant that read your pages.


There is a real asymmetry to weigh honestly:[Cloudflare's measurements](https://blog.cloudflare.com/ai-crawler-traffic-by-purpose-and-industry/) show AI companies crawl far more than they refer back (in August 2025, Anthropic crawled roughly 50,000 pages per referral visit sent, OpenAI under 900, Perplexity around 118). Crawl load is a genuine cost and mostly serves training. Which is exactly why the right posture is per-bot, not all-or-nothing: keep the search indexers and fetchers that produce shoppers, decide consciously about the training crawlers, and block the impostors that respect nothing.


One more reason to do this at the network edge rather than with a robots.txt file: robots.txt is a polite request, and Shopify itself notes crawl rules are "directional and advisory." Well-run crawlers honor it; the bots flooding your store do not (even the honest operators publicly dispute who crawled what, as the 2025 Cloudflare-Perplexity spat showed). Preferences need enforcement, and enforcement lives at the edge.


The condensed allowlist, the bots that should survive any configuration you choose:


Keep Why


Googlebot, Bingbot Search. Never block these


facebookexternalhit Link previews on Facebook, Instagram, Messenger


OAI-SearchBot, Claude-SearchBot, PerplexityBot AI search indexes: being findable by assistants


ChatGPT-User, Claude-User, Perplexity-User Live fetches when a shopper asks an assistant about you


GPTBot, ClaudeBot, meta-externalagent Training crawlers. Optional: your call, not a security issue


## What Shopify already handles


Shopify is not asleep here, and it is worth knowing exactly what the platform does so you also know what it leaves to you.


Every Shopify storefront already sits behind Shopify's own edge infrastructure (which itself runs on Cloudflare) with platform-level DDoS protection and a web application firewall, plus captchas on forms by default ([Shopify docs](https://help.shopify.com/en/manual/intro-to-shopify/bots/dealing-with-bots) ). Shopify Plus merchants can additionally arm a checkout bot protection feature built for product drops: it guards checkout for scheduled events of up to 60 minutes ([docs](https://help.shopify.com/en/manual/checkout-settings/bot-protection) ). That is drop-day armor, not everyday traffic hygiene.


What merchants do not get is any control over that edge. There is no setting in Shopify admin to block an IP range, challenge a country, or rate-limit a scraper. The October 2025 "Human or bot session" filter cleans up your reports, which is genuinely useful, but read the fine print of what filtering means: the bots still visit, still fire your pixels, still count in every session-billed app, still occupy infrastructure. The report looks better while the meter keeps running.


This gap is why the App Store is full of country-blocker and bot-blocker apps, and why they disappoint. An app cannot stand in front of Shopify's edge; the page has already been served by the time an app runs. So these apps work by loading JavaScript in your theme that detects and then hides or redirects. Two consequences follow. The session has already been counted by the time the script fires, so your analytics and metered apps are polluted anyway. And a bot that does not execute JavaScript never runs the blocking script at all. The same is true of removing a country in Shopify Markets: that stops checkout from a region, not browsing, not bots.


If your measured bot share was small, Shopify's built-ins plus the analytics filter are a perfectly sane place to stop. If it was 30, 50, 70%, the only structural fix is to filter traffic before it reaches Shopify. That means putting your own edge in front, which brings us to Cloudflare.


## Pick your strategy before you touch DNS


The Cloudflare setup is the same regardless; what differs is which rules you turn on once traffic flows through your zone. Decide now, calmly, instead of improvising in a dashboard at midnight.


A quick vocabulary note. Cloudflare rules can block a request outright, or serve a managed challenge: a quick automated check that most humans pass without noticing (at most once per half hour or so), while most bots fail. Cloudflare itself recommends the challenge over the block for most rules, and so does this guide: it converts "I hope that rule had no false positives" into "a rare human sees a two-second interstitial."


### Baseline bot defense


Cloudflare's free plan includes Bot Fight Mode, a single toggle that challenges traffic matching known simple-bot patterns. It is blunt: it applies to your whole domain, cannot be scoped or given exceptions, and can catch legitimate API or app traffic. On the $20 to 25 per month Pro plan this becomes Super Bot Fight Mode, which is configurable: you choose the action for definitely-automated traffic, you get a switch that allowlists all verified good bots in one move, and you get bot analytics. If the budget question is "is Pro worth it," for a store with a real bot problem the answer is usually yes, for that verified-bots switch and the visibility alone. ($200+ Business and Enterprise tiers add finer control, including scoring every request; most merchants reading this do not need them.)


*Fits:* every store doing this setup. This is the floor.
*Risks:* on Free, the bluntness, and you cannot create exceptions for it: if Bot Fight Mode interferes with a theme app or a custom storefront call, your options are turning it off (and filtering with WAF rules instead) or upgrading to Pro, where exceptions exist. So after enabling it, click through your store like a customer and watch for anything that stopped working.


### Geo-fence a country you do not sell to


If your diagnosis showed one country producing massive sessions and zero orders, one WAF custom rule handles it. The pattern, straight from[Cloudflare's own documentation](https://developers.cloudflare.com/waf/custom-rules/use-cases/allow-traffic-from-verified-bots/) , is a rule matching the country with an exemption for verified crawlers. In the rule editor, the expression is:


```text
ip.src.country eq "CN" and not cf.client.bot
```


and the action, chosen separately in the rule's action dropdown, is managed challenge. (The expression and the action are two fields; the expression editor will reject anything else pasted into it.) The "not cf.client.bot" clause matters: it exempts every crawler on Cloudflare's verified good-bot list from the challenge, so you are not accidentally fighting search engines. Free plans include 5 custom rules, which is plenty for this.


Be honest with yourself about the trade-offs before choosing a hard block instead of the challenge. Geolocation resolves the network exit point, not the person: VPN users, travelers, and expat customers shipping gifts home will hit your rule, which is exactly why the challenge (which humans pass) beats the hard block. Blocking a country also ends your visibility in that country's local search engines (if you block China, that is Baidu). And one legal note for European merchants: the EU's geo-blocking regulation (2018/302) prohibits blocking shoppers from other EU and EEA countries, so geo-fencing is a tool for countries you genuinely do not serve, outside that zone.


*Fits:* the "70% from one country, zero orders" pattern.
*Risks:* the edge cases above; mitigated by challenge-not-block and the verified-bot exemption.


### A deliberate AI crawler policy


As of July 2026, Cloudflare classifies AI bots into three behaviors, Search, Agent (user-triggered fetching), and Training, and lets you set a policy per behavior, or per individual crawler in its AI Crawl Control panel, on every plan including Free. This is where you implement the keep-list from earlier: allow Search and Agent categories, decide consciously about Training, and let Cloudflare's verified-bot system separate the honest crawlers from the impostors that fake user agents.


Two warnings here. First, check what is already set: Cloudflare has blocked AI crawlers by default for zones created after mid-2025, so your store may already be invisible to AI search without anyone having chosen that. Second, defaults change again on September 15, 2026 (Search allowed, Training and Agent blocked on ad-carrying pages, for new zones and Free customers who have not chosen). Do not inherit a default either way. Set it yourself.


*Fits:* everyone; this is five minutes in one panel.
*Risks:* none if set consciously. The risk is not setting it.


### Filter the reports and stop there


Turn on Shopify's bot filter, add GA4 annotations, accept the costs. Zero risk, zero setup, and the honest choice if your bot share is minor or you cannot stomach a DNS change this quarter. You can return to this guide when the share grows. It usually grows.


Most stores with a real problem want the baseline defense plus the AI crawler policy, adding the geo-fence if the geography is lopsided. All three ride on the same setup below.


## Moving your DNS without losing sleep


Everything above happens inside a Cloudflare account. What makes it apply to your store is pointing your domain's DNS at Cloudflare so traffic passes through Cloudflare's network on the way to Shopify. This is the step merchants fear, reasonably: DNS is the address system that makes your store exist on the internet, and yes, a botched change can take your site offline. The fear shrinks when you know three things: the failure modes are few and known, every one of them has a fast exit, and nothing about the move is permanent.


Before any checklist, get one piece of vocabulary into your hands, because the entire move hinges on it: the little cloud next to each DNS record in Cloudflare, grey or orange. Flip it yourself:


The one toggle that matters


Every record in Cloudflare's DNS panel has a little cloud next to it, grey or orange. The whole setup hinges on knowing the difference. Flip it.


CNAME


www


shops.myshopify.com


Your www record in Cloudflare, roughly as the panel shows it


Click the cloud ↑


Shoppers and bots


↓


→


Cloudflare


gives directions, then steps aside


↓


→


Your store


Grey cloud: Cloudflare is an address book


Someone asks "where does www.yourstore.com live?", Cloudflare answers, and steps out of the way. Every visitor, human or bot, then connects straight to your store. Nothing is filtered, nothing changes. This is the safe starting state: you can move your DNS to Cloudflare and leave everything grey, and your traffic flows exactly as before.


The flip works both ways, in seconds. Orange misbehaving? Click it back to grey and traffic flows like Cloudflare was never there. That is the whole rollback plan.


Next, the honest disclosure that most guides bury or omit.


**Cloudflare supports this setup. Shopify does not.** Cloudflare documents the configuration officially (it is called Orange-to-Orange or O2O, because both your zone and Shopify's are Cloudflare zones), has supported it on every plan including Free since June 2025, and detects it automatically: no ticket, no Enterprise contract, despite what older forum posts say ([Cloudflare's Shopify guide](https://developers.cloudflare.com/cloudflare-for-platforms/cloudflare-for-saas/saas-customers/provider-guides/shopify/) ). Shopify's help center, meanwhile,[states plainly](https://help.shopify.com/en/manual/domains/troubleshoot-issues-with-domains) that Cloudflare proxy setups including O2O are not supported, that they can affect certificate provisioning and reduce the accuracy of Shopify's own bot detection, and that problems arising from them are outside Shopify Support's scope. In practice this means: it works, a lot of stores run it, Cloudflare built explicit machinery for it, and if you ever contact Shopify support about an unrelated domain issue, their first instruction will be to turn the proxy off. You should also know what you trade away: Shopify's own bot defenses see less of your traffic's original characteristics when it arrives through another proxy, so you are substituting Cloudflare's bot layer for part of Shopify's.


### How bad can it get, really


Sizing the risk honestly, for whoever signs off on touching DNS:


- **The process is designed so the store never goes down.** Step one moves only who answers DNS lookups, with identical records; traffic flow does not change until you flip one toggle later, and that toggle reverts in seconds. There is no moment where the site is "moved" and unreachable by design.
- **Worst realistic case #1: a missed record.** Email or a verification breaks a few days later because a record did not make the move. Prevention is the record-by-record comparison in the checklist below; the fix is adding the missing record, minutes once noticed.
- **Worst realistic case #2: a certificate or proxy conflict.** Shopify admin shows "SSL pending" or visitors see a security error or redirect loop. Exit: flip the proxied record back to "DNS only" and the condition clears while you investigate. Degradation is minutes if you are watching, which is why the checklist says to do this in a quiet window and verify immediately.
- **Worst delayed case: certificate renewal fails weeks later** because "Always Use HTTPS" was enabled. Prevention is one instruction below; the fix is the same toggle-back.
- **The lasting cost is organizational, not technical:** Shopify treats this setup as unsupported, so on any future domain-adjacent support ticket, expect "please disable your Cloudflare proxy" as their first triage step. That is a permanent tax on your support conversations, and you should weigh it like one.
- **On performance:** both zones run on Cloudflare's network, so the added hop stays inside Cloudflare rather than crossing the public internet. If milliseconds matter to you, measure your storefront's time to first byte before and after; that is the honest test.


If that trade reads as acceptable, and for a store drowning in junk traffic it usually is, here is the process. Set aside a quiet evening, not the night before a campaign.


### Before you touch anything


1. **Inventory your DNS.** Log in to wherever your DNS currently lives (often your domain registrar). Export or screenshot every record: the A record for your bare domain, the www CNAME, every other subdomain (shop, blog, link-tracking CNAMEs your email platform like Klaviyo created, anything an app added), and critically your email records (MX, plus the TXT records for SPF, DKIM, DMARC) and any verification TXT records (Google Search Console, Meta domain verification, and so on). Missed email records are the most common self-inflicted wound in any DNS migration; Cloudflare's importer is good but[Cloudflare's own docs](https://developers.cloudflare.com/dns/zone-setups/full-setup/setup/) warn it is not guaranteed to find everything.
2. **Lower your TTLs.** If your current DNS host lets you, set the time-to-live on your records down to 300 seconds a day or two before the move. TTL is how long the internet caches your records; short TTLs mean any mistake you make propagates away in minutes instead of hours.
3. **Know your logins.** Registrar, current DNS host, Shopify admin. The rollback path runs through the registrar, so test that login now.
4. **Confirm your Shopify records match the current standard.** As of July 2026, a third-party domain connects to Shopify with an A record pointing to 23.227.38.65 and a www CNAME pointing to shops.myshopify.com ([Shopify docs](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-domain-manual) ). If yours differ, understand why before migrating.


### The move itself


1. **Create a free Cloudflare account and add your domain.** Cloudflare scans and imports your existing records. Now do the single most important verification of the whole process: compare the imported list against your step-1 inventory, record by record, and add anything missing. This is the step that prevents the "my email died three days later" story.
2. **Leave every record on "DNS only" (grey cloud) for now.** Grey-cloud everything first, so the move changes nothing about how traffic flows. You are doing this in two safe steps: move the DNS, then turn on the proxy.
3. **Change your nameservers at the registrar** to the two Cloudflare assigns you. This is the actual move. Propagation is officially up to 24 hours; in practice it often completes within hours, though some registrars and domain endings are slower. Your store keeps working throughout either way, because the records themselves are identical, only the server answering them changed.
4. **Wait for Cloudflare to mark the zone Active** (you get an email), then check your store loads, your admin works, and a test email arrives. Nothing has functionally changed yet, which is the point.


### Turning the filter on


1. **Flip the www CNAME to proxied (orange cloud), and only the www CNAME.** Every other record, email-sending CNAMEs, blog and app subdomains, all of it, stays on "DNS only" unless you know exactly why you are proxying it; a proxied email-tracking record is a classic way to break something unrelated days later. Because the www target is shops.myshopify.com, Cloudflare recognizes the Shopify relationship automatically and routes traffic zone-to-zone; you will see a Shopify logo appear next to the record in the dashboard. Your WAF rules, bot settings, and analytics now apply to storefront traffic.
2. **Leave the bare-domain A record grey-clouded, and let the bare domain redirect to www.** The zone-to-zone handoff engages on CNAME records, not on proxied A records; a proxied A record in front of Shopify is exactly the unsupported configuration that produces certificate errors. The clean pattern: keep the apex unproxied and make sure www is your primary domain in Shopify (check Settings, then Domains; it is the default for most stores). Shopify redirects the bare domain to whichever domain is primary, so with www primary, apex visitors arrive at the filtered www anyway. If your bare domain is currently primary, switch the primary to www before this step or your filtering will only ever see the redirect.
3. **Do not enable "Always Use HTTPS," ever, on this zone.** This is the trap that bites weeks later. Shopify renews your TLS certificate through a validation file it serves over plain HTTP at "/.well-known/acme-challenge/". Cloudflare's blanket HTTPS redirect intercepts that path, validation fails, and your certificate quietly fails to renew ([Cloudflare's Shopify guide](https://developers.cloudflare.com/cloudflare-for-platforms/cloudflare-for-saas/saas-customers/provider-guides/shopify/) documents this exact caveat). If you want an HTTPS redirect at the edge, create a redirect rule that excludes that path, per Cloudflare's guide. Set the SSL/TLS encryption mode to Full (strict), never Flexible; Flexible against Shopify produces an infinite redirect loop.
4. **Now apply the strategy you picked earlier:** enable Bot Fight Mode (or Super Bot Fight Mode with verified bots allowed, on Pro), create the geo rule if you chose one, and set your AI crawler policy in AI Crawl Control.


### Prove it worked


Run through this list the same evening, then again after a day:


- Storefront loads over HTTPS on www and on the bare domain, from a normal browser and from your phone off wifi.
- **Place a real test order through checkout,** including payment. Checkout is the one flow you never assume.
- Shopify admin, Settings, Domains: status green, SSL active, no proxy warnings.
- Send yourself an email at your custom domain, and send one out.
- Confirm Google Search Console and any other domain verifications (Meta, Pinterest) still show verified; their TXT records made the move with everything else, this is the check that proves it.
- In Cloudflare, watch Security analytics for a day: you should see challenges being served, with solve rates near zero for the junk traffic (bots do not solve challenges; humans do).
- Compare your Shopify sessions report week-over-week after seven days. This is your before-and-after.


### If something breaks


The exits, fastest first:


- **Undo the filter, keep the DNS: flip the www record back to grey cloud.** Takes effect in seconds to minutes, and you are back to traffic flowing exactly as it did before Cloudflare touched anything. Certificate errors, redirect loops, "domain uses an unsupported proxy" warnings in Shopify admin: this one move clears the condition that causes all of them while you investigate. This is why the two-step approach matters; the scary-sounding full rollback is almost never the one you need.
- **Full rollback: change nameservers back** at your registrar to the old DNS host. Keep your old DNS zone untouched at the old host for at least a couple of weeks precisely so this remains a five-minute operation. With the lowered TTLs from the pre-flight list, propagation is quick.
- **Certificate stuck on "SSL pending" in Shopify admin:** grey-cloud the records and wait for Shopify to finish provisioning; then re-enable the proxy. This is the known failure mode of proxying too early or with the wrong settings, and it resolves the same way every time.


### The week after


Your charts are about to change shape on purpose, and it helps to know what to expect before your boss asks.


- **Sessions fall off a cliff. That is the fix working.** Every trend chart that counts sessions breaks its history on cutover day. Annotate the date, in GA4 and wherever your team keeps notes, so nobody in three months reads the drop as a traffic problem. The line for the Monday meeting: sessions dropped because we stopped serving robots; every one of those sessions was inflating our metered app bills and feeding junk to the ad pixels.
- **Conversion rate jumps for the same reason.** The denominator got honest. Resist the urge to celebrate it as growth; it is the same orders over real sessions.
- **Know which number is the truth now.** Cloudflare's analytics count everything that arrives at the edge, including what gets challenged away; Shopify's reports count what got through. Expect Cloudflare's number to be much larger, and treat the gap as your filter's work. Since Shopify's own bot detection sees proxied traffic less clearly (their warning, and a fair one), Cloudflare's dashboard is your traffic source of truth from now on; Shopify remains the truth for orders and everything that matters more than traffic.
- **Watch for the rare real person caught in a rule.** Cloudflare's security events log shows exactly which rule matched every challenged or blocked request. If a real customer or a partner's tool reports being blocked, find the event, then either relax that rule from a block to a managed challenge (humans pass those) or add a skip rule for their IP above it. You are never choosing between "the rule stays" and "the customer stays."


## The bill for traffic that never buys


You now know how to stop the flood. Here is a frame for what it was costing, so the afternoon of setup has a number attached.


For a standard Shopify store the cost is not bandwidth (Shopify includes it) but everything metered around the store:


- **Session and pageview billed tools.** Count yours: heatmaps, personalization, popups, analytics, some review and support tools. Each has a tier ladder, and bot sessions climb it for you. One real ladder as reference: Lucky Orange's public pricing runs $32 to $839 per month depending on session volume.
- **Ad spend and pixel damage.** Bot ad clicks are direct loss; polluted pixel learning is indirect but compounding, because the algorithm optimizes toward whatever converts, and bots teach it that nothing converts.
- **Decisions.** Conversion rate, market prioritization, campaign readouts, merchandising signals: every one degrades with the denominator. This cost is unmeasurable, which is exactly why it is the worst one.
- **Everyone's infrastructure.** For headless stores the bandwidth line is real and immediate ([Vercel](https://vercel.com/docs/pricing) , for example, charges $0.15 per GB past included transfer). When the docs platform Read the Docs[blocked AI crawlers in 2024](https://about.readthedocs.com/blog/2024/07/ai-crawlers-abuse/) , its bandwidth dropped 75%, about $1,500 a month; not a store, but a clean before-and-after of what serving bots costs whoever serves them.


Plug in your own numbers, from your diagnosis earlier:


The bot bill


Pick which of your tools bill by traffic, plug in your sessions and bot share, and see what a year of serving robots costs you.


$ ··,···


## Why is a merchandising company writing this?


Fair question. Depict builds AI-assisted merchandising for Shopify stores: visual merchandising for collection pages. Bot traffic is not our product, and this guide sells nothing.


We wrote it because our merchants kept asking, and because we have skin in the game three ways. Our own pricing counts sessions the way analytics tools do, without separating bots, so bot floods push our customers toward limits for traffic that will never buy (our stance: generous limits, tracked but not automatically enforced, and honest conversations). Bot sessions also pollute the behavioral data merchandising runs on. Bots do not buy, so your sales figures survive; what gets corrupted is everything based on views and clicks. Junk sessions inflate the apparent popularity of whatever pages they hammer, so the products a merchandiser deliberately placed can read as underperformers next to whatever the bots happened to hit, and popularity-based sorting quietly learns from fiction. And bot load burns compute, ours and every vendor's, for nobody.


So: cleaner traffic makes your costs saner and your data honest, and honest data is the raw material of good merchandising. If the hours you reclaim from fighting junk traffic are hours you would rather spend curating the collections that carry your story,[that is the job Depict does](https://depict.ai/integrations/shopify) .


## FAQ


### Does Cloudflare work with Shopify?


Yes, with an asterisk. Cloudflare officially documents and automatically enables the setup (O2O) on every plan. Shopify officially does not support proxy setups and will ask you to disable the proxy if you contact their support about domain issues. Thousands of stores run it; run it with the rollback plan from the DNS chapter and the trade-off is yours to make.


### Will this break my checkout?


Checkout runs on Shopify's infrastructure and Cloudflare disables its own page-modifying features on the checkout path in this setup. The verification list in the DNS chapter includes a real test order precisely so you confirm it on your store, the same evening.


### Can I just block all of China (or any country)?


You can, with one WAF rule. Prefer a managed challenge over a hard block: VPN users, travelers, and expats are humans who will pass the challenge, and the verified-bot exemption keeps search crawlers unaffected. EU merchants: EU law prohibits geo-blocking shoppers in other EU/EEA countries.


### Will I disappear from Google or ChatGPT?


Not if you configure deliberately. Cloudflare's verified-bot system exempts honest crawlers from your rules, and its AI crawler controls let you allow search indexers and user-triggered fetchers (the ones that make you visible in AI assistants) while blocking or allowing training crawlers as you choose. What actually makes stores disappear is inheriting a block-everything default without noticing; check yours, especially if your zone is new or after the September 2026 default change.


### Do I need a paid Cloudflare plan?


No. The DNS setup, the zone-to-zone Shopify routing, Bot Fight Mode, 5 WAF custom rules, and the AI crawler controls are all on the free plan. The $20 to 25 Pro plan buys you configurable bot actions, a one-switch verified-bots allowlist, and bot analytics; worth it for stores with a serious problem, not required to start.


*Facts in this guide were verified in July 2026 against vendor documentation and primary reports; sources are linked inline. Cloudflare and Shopify change quickly here. If you spot something that has drifted, tell us and we will fix it.*


Depict's visual merchandising runs on the behavioral data this guide just helped you clean: curated first rows with rules underneath, on collection pages, learning from real shoppers instead of robots. If you run on Shopify, there is a free tier: install it and see your own collections in it in minutes.


[See Depict on Shopify →](https://depict.ai/integrations/shopify)


Want this run against your actual store?


Leave your email and your store's web address and we'll send you a short personalised read within 48 hours: what's visibly working from the outside, what's quietly leaking, and which of the practices in this guide would move the needle first for you.


About Depict.


Depict is visual merchandising software for e-commerce teams: curated collections with boost, bury, and pin rules underneath, and scheduled publishing for collection pages, on top of your existing storefront. Learn more at[depict.ai](https://depict.ai/)
