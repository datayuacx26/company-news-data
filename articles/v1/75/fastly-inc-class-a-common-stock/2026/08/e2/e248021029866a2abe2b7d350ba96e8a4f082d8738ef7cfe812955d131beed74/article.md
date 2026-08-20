---
schema_version: "1.0.0"
document_id: "e248021029866a2abe2b7d350ba96e8a4f082d8738ef7cfe812955d131beed74"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/online-sports-shadow-audience/"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-07T18:51:39.622940+00:00"
fetched_at: "2026-08-07T18:51:40.979794+00:00"
content_hash: "sha256:d7a28e148ad6f8b9c889cd3a52596274f42f9ae434c8d4f90367435620f3a7b6"
---

# Online Sports’ Shadow Audience

FIFA projected that approximately 6 billion people would engage with some part of the FIFA World Cup this year, across group stage, knockout drama, and everything in between (source). We've spent this tournament digging into what that audience looked like:[halftime dips](https://www.fastly.com/blog/world-cup-viewership-trends) ,[close-match surges](https://www.fastly.com/blog/what-the-world-cups-knockout-rounds-reveal-about-viewership) ,[how the World Cup Reshaped U.S. Internet Traffic](https://www.fastly.com/blog/how-the-world-cup-reshapes-us-internet-traffic) , as well as a[summary of our insights on World Cup traffic](https://www.fastly.com/blog/delivering-a-resilient-world-cup-final) .


Every legitimate stream very likely had company: unauthorized rebroadcasts pulling from the same matches, bots probing the same login pages and ticket queues, and automated traffic trying to blend in with real fans. None of that is unique to Fastly's platform or our customers. It's a byproduct of popularity. The unauthorized traffic isn't noise. It's data. And tracing it across Fastly's platform gives you a different lens on what 6 billion people actually wanted to watch.


## **Counting the Unauthorized**


We're always being thoughtful about what we share from our customers' traffic, and we wouldn't want to overstate our own role in a fight that involves law enforcement, rights holders, and dozens of other organizations. But the public record on World Cup piracy this year is unusually detailed - detailed enough to show what unauthorized demand looked like at global scale, and to draw some revealing comparisons.


In late June, the Department of Justice[announced it had seized nearly 400 domains](https://www.justice.gov/opa/pr/united-states-seizes-hundreds-internet-domains-used-illegally-stream-world-cup-matches) used to illegally stream World Cup matches in real time, in a coordinated action that reached servers and domains in Peru and Bulgaria — both described by investigators as established hubs for streaming piracy — with related disruptions in Croatia, Romania, Poland, and Colombia. The operation, nicknamed Operation Offsides, was built on leads from FIFA as well as several major rights holders and broadcasters. That was only the opening move: by the time the tournament wrapped, DOJ said the cumulative Operation Offsides effort, carried out across three separate actions, had seized more than 1,000 domains in total.


That DOJ number was only part of the cleanup effort. Separately, the Trustworthy Accountability Group ([TAG](https://www.tagtoday.net/) ), an industry group focused on ad-fraud and piracy, ran a parallel and distinct effort: rather than seizing domains, TAG used its network of advertising industry members to cut off ad revenue to nearly 1,400 sites (1,376, to be precise) found to be streaming or hosting stolen World Cup content, adding them to its Pirate Domain Exclusion List so member companies would stop placing ads on them. TAG's list and the DOJ's seized domains are separate efforts targeting different (if surely overlapping in spirit) parts of the same piracy ecosystem — one choking off the money, the other pulling the sites themselves offline.


The scale of individual piracy operations is its own story. One ring known as PirloTV, dismantled in a joint action by the Alliance for Creativity and Entertainment ([ACE](https://www.alliance4creativity.com/) ), UEFA, and Mexican authorities just before the tournament, was reportedly[generating upward of 950 million visits a year](https://torrentfreak.com/ace-uefa-and-mexico-chase-pirlotvs-950-million-visit-piracy-network/) on its own, with roughly a quarter of that coming from Mexico alone. Its popularity wasn't really about price — it reportedly built its audience on mobile devices, where fragmented broadcast rights and platform restrictions make legitimate access genuinely harder to navigate for casual fans. That's less a story about criminality and more one about friction: when legitimate access is complicated, a well-designed illegal alternative doesn't need to try very hard.


And that's before accounting for the enforcement that happened before a ball was kicked. Bulgarian authorities and Europol ran a separate pre-tournament sweep, Operation KRATOS 2, which dismantled nine organized groups and[took down more than 27,000 illegal streaming URLs](https://www.laliga.com/en-GB/news/laliga-participates-alongside-europol-and-international-authorities-in-a-new-offensive-against-audiovisual-piracy) tied to sports piracy generally, not just the World Cup.


Taken together, the honest read isn't "piracy was defeated."[One outlet covering the DOJ action](https://thespun.com/soccer/major-crackdown-on-sites-illegally-streaming-world-cup-games) put it well: "The 2026 FIFA World Cup probably ranks as the most illegally-streamed piece of media on record. \[...\] Unfortunately, trying to put a wholesale stop to illegal streaming is very much like playing whack-a-mole — the minute one is shut down, another quickly takes its place."


The interesting part for anyone running infrastructure isn't the takedown count. It's that the piracy footprint scales in lockstep with legitimate audience size, in ways that are now genuinely trackable across multiple independent sources.


## **It Wasn't Just Piracy**


Piracy is the most visible shadow traffic around an event like this, but it wasn't the only kind. From where we sit, the underlying trend shows up directly in our own traffic:[Fastly's Q3 2025 Threat Insights Report](https://learn.fastly.com/rs/025-XKO-469/images/FY25Q3%20Threat%20Insights%20Report.pdf?version=0) found that bots accounted for roughly 29% of all observed web traffic that quarter, with about 25 percentage points of that made up of traffic classified as unwanted — a baseline rate of automated noise that a fixed-date, high-demand event like the World Cup only adds pressure to.


That matches what outside researchers found looking specifically at the tournament. Security researchers tracking the event separately reported a sharp rise in bot traffic and automated attacks against sports and betting platforms through June, including one European sports-betting operator that logged close to 19 million blocked malicious requests over a three-week window, and an 87-second flash DDoS attack that generated more than 780,000 requests,[peaking at nearly 18,000 requests per second](https://datadome.co/threat-research/attacks-on-sports-betting-platforms-ramp-up-amid-2026-fifa-world-cup/) . Thales's 2026 Bad Bot Report similarly found that a meaningful share of attacks on sports platforms specifically - roughly a quarter, by its count — were attributed to more advanced, AI-assisted automation designed to mimic real users in ticket queues and login flows.


None of that traffic looks like piracy on the surface. It's a different problem — account takeover, ticket scalping, ad fraud, credential stuffing — but it shares a root cause: a fixed, high-value event creates a short window where a lot of automated and adversarial traffic tries to look exactly like a real and excited fan.


## **Traffic Doesn't Announce What It Is**


We're not going to claim credit for the DOJ seizures, TAG's exclusion list, or the ACE/UEFA and Europol takedowns above — that work belongs to law enforcement, rights holders, and the investigators who tracked it. But we're not just commentating from the sidelines, either. In April 2026,[Fastly and LaLiga announced a joint anti-piracy effort](https://www.google.com/url?source=gmail&sa=E&q=https://www.fastly.com/press/press-releases/fastly-and-laliga-team-up-on-joint-innovation-to-combat-piracy) that uses AI and proprietary content signals to identify illegal LaLiga streams in real time and disable them with far more precision than broad regional blocking allows — a direct response to piracy problems. LaLiga estimates that piracy costs its clubs $700–800 million a year, a cost it had already cut by 60% in Spain during the 2024/25 season through a combination of legal, technological, and educational measures. It's the same category of leeching traffic described above, just addressed at the source instead of after the fact.


Caching and edge logic aren't just about shaving milliseconds off a legitimate viewer's join time. The same systems that decide how to route and serve a real fan's request are also the first place you can tell that a request isn't one — a manifest fetch from an unexpected origin, a login attempt with the wrong shape, a traffic pattern that spikes from a region with no historical demand for the event at all. Load balancing exists to absorb legitimate surges gracefully; the same instincts and tooling are what let a platform notice when a "surge" is actually a botnet or an unauthorized restream pulling from the same feed.


That's really the throughline of everything we've covered about this tournament, whether we were writing about halftime dips or knockout-stage traffic spikes: the internet doesn't distinguish between a legitimate fan and an illegitimate one until you build something that does. The scale of an event like the World Cup doesn't just test whether a platform can handle a lot of traffic. It tests whether that platform actually understands the traffic it's handling.


## **A Problem That (likely) Doesn't Have a Final Whistle**


Piracy operations will keep rotating domains, and bot traffic will keep getting better at looking human. That's not a problem this tournament solved, and it's not one any single company solves either. But the public record from this World Cup — DOJ takedowns, industry group actions, Europol operations, independent bot-traffic research — gives a rare, detailed look at just how large the shadow side of a major event has become, and how much coordinated effort across law enforcement, rights holders, and infrastructure providers it takes to keep up with it.
