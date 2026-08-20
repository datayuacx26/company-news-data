---
schema_version: "1.0.0"
document_id: "3921b4a9b2b58bb1b4870f178d28045375d738445c784a3f91049b658544e4b7"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/the-first-accelerator-ipo-the-origin-story-of-sendgrid"
published_at: "2026-03-23T14:31:45+00:00"
first_seen_at: "2026-07-20T23:20:38.087450+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:9e49539e4e91479931e66dbf4e147630d561f17c94e98366b9b716f8bc1f6d22"
---

# The First Accelerator IPO: The Origin Story of SendGrid

## The Whiteboard in Boulder


Boulder, Colorado. Summer of 2009. Three engineers from Southern California have relocated to the Front Range to spend three months in a converted warehouse with nine other startups, chasing a problem so unglamorous it barely had a name.


Isaac Saldana had a whiteboard. On it, he'd written a single statistic that had been haunting him for two years: **20%.**


One in five transactional emails — the password resets, the order confirmations, the shipping notifications, the sign-up verifications — never arrived. They vanished into spam folders or simply evaporated somewhere in the tubes between a developer's mail server and a user's inbox. Every company building anything on the internet was quietly hemorrhaging 20% of its most important communications, and almost none of them knew it.


Saldana knew it because he'd lived it. As CTO at two separate startups before this moment — ThemBid, a service marketplace, and NotPaul, a fan-event platform — he had personally wrestled with email infrastructure both times. He had configured SMTP servers. He had watched bounce rates spike. He had spent days he didn't have navigating the Byzantine politics of ISP spam filters, trying to figure out why his legitimate, user-requested emails were being silently discarded. He had fixed the problem. Then he had watched every other developer around him fix the same problem from scratch, wasting the same weeks, making the same mistakes.


The insight that would become a $3 billion company was not complicated. It was almost embarrassingly obvious once you said it out loud: *email deliverability is a solved problem — it just hasn't been solved as a service.*


Somewhere in that Boulder warehouse, with David Cohen and Brad Feld walking the halls and mentors cycling in and out, Saldana told his co-founders Tim Jenkins and Jose Lopez that he had decided to spend his life fixing email. They believed him. And then, with the methodical stubbornness of engineers who had already been burned by this problem before, they got to work.


---


## How Email Broke


To understand why SendGrid existed, you have to understand what email had become by 2009 — and why it was so broken.


Ray Tomlinson sent the first networked email in 1971. He was an engineer at BBN Technologies working on ARPANET, and his message — something like "QWERTYUIOP," a test typed without much ceremony — traveled between two machines sitting a few feet apart. Tomlinson also invented the @ symbol as the separator between user and host, choosing it partly because it would never appear in a person's name. He had no particular vision for what he'd started. He was just solving a small problem in front of him.


By 2009, the infrastructure Tomlinson had casually initiated had become the connective tissue of the entire internet economy. Billions of transactional emails flew daily. Every app that wanted to communicate with its users had to send them: account confirmations, receipts, alerts, password resets, fraud warnings. Email wasn't optional. It was the default channel for every important digital interaction.


But the infrastructure underneath it had curdled. The spam crisis of the early 2000s had turned email into a war zone. Spammers had flooded inboxes. ISPs had responded by building increasingly aggressive filters. The arms race had gone on for years — spammers getting more sophisticated, filters getting more paranoid — and the collateral damage was landing on legitimate senders. A developer at a legitimate SaaS company in 2008 who tried to configure their own mail server quickly discovered they were fighting a multi-front war: SPF records, DKIM signing, DMARC policies, IP warming schedules, feedback loops with individual ISPs, bounce management, complaint rate thresholds, reputation scoring systems — each one a mini-discipline unto itself, each one capable of routing your emails to spam if you got it wrong.


Most companies didn't get it right. Most companies didn't even know what "right" looked like. And so the 20% evaporation rate that Saldana had witnessed was, if anything, conservative. Companies were simply not tracking it. The emails went out; they assumed they arrived.


Isaac Saldana had grown up far from the American tech scene. Born in the US but raised in a working-class family in Mexico, he came back to the United States at age fourteen — without knowing English — to live with relatives near Riverside, California, and pursue an education. He enrolled at UC Riverside studying electrical engineering and computer science, juggling school with two jobs and a young family he was supporting from the start. He married at nineteen. Sleep was a luxury. He graduated in 2003 and went to work.


He was, by every measure, someone who had earned his seat at the table the hard way. No venture capital connections. No famous engineering pedigree. No Y Combinator alumni network. What he had was a technical obsession, a precise understanding of a problem that everyone else was treating as background noise, and the particular kind of stubbornness that comes from having fought for everything before this.


Tim Jenkins and Jose Lopez had met Saldana at UC Riverside. The three of them were wired differently — Jenkins and Lopez brought complementary operational and business muscle — but they shared the same diagnostic clarity about email. When Saldana started sketching the solution in late 2008 and had his first paying customer by March 2009 — before he'd even pitched a co-founder — it wasn't a hobby project. It was the beginning of something real.


They applied to Techstars Boulder in the spring of 2009. The company at the time was called **SMTP API** — a technically accurate name that communicated the value proposition perfectly to anyone who already understood what SMTP was, which was a vanishingly small percentage of the people they needed to sell to. They got into Techstars in May. They packed up and moved to Colorado.


---


## Ninety Days in Techstars


The Techstars program ran from May to August 2009. David Cohen had co-founded the accelerator in 2006 with Brad Feld, building a model designed to compress years of founder learning into three months: mentor feedback, cohort peer pressure, and the looming pressure of Demo Day. The model was brutal in the best possible way.


During that summer, Foundry Group partner Ryan McIntyre came on as a mentor to Saldana and the team. McIntyre had co-founded Foundry Group with Feld and had a prior history in the email ecosystem — Foundry had backed Return Path, which operated in the email deliverability space. He understood what SendGrid was building, and he saw its potential faster than most.


By the time Demo Day arrived in August 2009, SendGrid had nearly **100 customers** and had delivered over **100 million emails** . This was not the result of any sales campaign. There was no sales campaign. Developers had simply found the product, tried it, and told other developers about it. The organic pull was immediate and striking.


One thing changed during the summer. Brad Feld looked at the name "SMTP API" and said it wasn't going to last. It worked for developers — but developers were only part of the audience. The CTOs signing contracts, the CMOs approving budgets, the investors writing checks — they didn't know what SMTP was and weren't going to pretend otherwise. The name was killing conversations before they started.


They renamed the company **SendGrid** . Direction (send). Network (grid). No technical literacy required.


Post-Demo Day, Highway 12 Ventures — a seed fund led by Mark Solon — offered a term sheet. They closed **$750,000 in November 2009** . David Cohen participated. SoftTech VC and FF Angel came in. Brad Feld wrote about the deal on his blog with genuine enthusiasm: "I love these guys." In April 2010, Foundry Group led the **$5 million Series A** . Ryan McIntyre joined the board. Individual investors included Dave McClure, David Cohen, Scott Petry, and Matt Mullenweg — the founder of WordPress. The fact that the man running the platform powering a significant fraction of the internet's websites personally backed the company that would handle those websites' transactional email says something about how obvious the need was to the people closest to it.


The Foundry Group investment thesis was explicit: while a mature ecosystem had developed for bulk marketing email (ExactTarget, Constant Contact, MailChimp), "no such ecosystem had developed around transactional emails despite their greater challenges and volume demands." SendGrid was not competing with email marketing platforms. It was inventing a category that had been hiding in plain sight.


What made the early growth remarkable was its structure. By early 2010 — just months after the seed close — SendGrid had thousands of customers and had delivered **1.2 billion messages** . Through word of mouth, without a sales team, without a marketing budget. The product was spreading through the developer community the way useful tools spread: somebody needed email for their side project, found SendGrid via a Stack Overflow thread or a Hacker News comment, integrated it in an afternoon, and mentioned it to a colleague.


In 2011, SendGrid did something that proved the distribution insight was real: they partnered with Heroku. Heroku was the dominant Platform-as-a-Service for developers deploying web applications, and they had an add-on marketplace. Adding SendGrid to a Heroku app took one CLI command. The integration generated over **10,000 free accounts** . The direct revenue was modest — $8,400 per month in MRR, with 30% going back to Heroku. The economics looked terrible on paper. But those 10,000 developers went on to build companies. SendGrid was in their default stack. By the time those startups raised their Series A and needed to send hundreds of millions of emails per month, SendGrid was already there. The Heroku bet was a distribution play disguised as a mediocre revenue play.


Underneath all of this, the real competitive moat was being built in a place no one could see.


What SendGrid was actually doing was building something that functions like a **diplomatic corps for email** . ISPs — Gmail, Yahoo, Hotmail, the big carriers — are essentially sovereign entities with their own rules about who gets to send mail into their networks at scale. They maintain real-time reputation scores for IP addresses and sending domains. They publish feedback loop data — reports on how many recipients marked your mail as spam — and share it with senders who register for it. They impose throttling limits on new or unrecognized IPs. They have whitelisting programs that require audited applications and ongoing compliance.


Navigating this ecosystem at scale requires relationships, processes, and technical infrastructure that no single company building a product could justify building for themselves. SendGrid built it once and rented it to everyone.


The specific technical work was deeply unsexy but critically important. **IP warming** — the process of slowly ramping up email volume from a new IP address so ISPs can observe sending patterns before granting full throughput — is an art form as much as a science. Do it too fast and you get throttled. Do it too slow and you waste time. SendGrid maintained entire pools of IPs at different warmup stages, routing customer traffic appropriately. They built **suppression systems** that automatically stopped sending to addresses that had bounced or complained, because continued attempts to reach bad addresses were one of the fastest ways to destroy a sender's reputation. They built **feedback loop integrations** with every major ISP that offered them, processing complaint data in real time. They built **shared IP pool management** that segmented customers by reputation — so a low-volume startup didn't drag down the deliverability of a high-volume enterprise sharing the same IP pool.


All of this was invisible to the developer who integrated SendGrid in an afternoon. That invisibility was the whole point.


---


## Scale Itself Was the Moat


SendGrid didn't have a single cinematic breakthrough moment. The breakthrough was structural, and more durable than any one event: it was the realization that **scale itself was the moat** .


The more email SendGrid sent, the better their deliverability got. Each customer added contributed to the aggregate reputation signal they carried in ISP eyes. Large, consistent, well-managed volumes of mail from diverse senders — as long as quality controls held — actually strengthened their IP pool reputations. They were building a flywheel: deliverability attracted customers, customers created volume, volume improved deliverability, improved deliverability attracted more customers.


The Bessemer Venture Partners investment memo — prepared for the **$21.6 million Series B in January 2012** at a $60 million pre-money valuation — captured the architecture of this moat precisely. The numbers were remarkable:


-


Customer acquisition cost payback periods **under 6 months**


-


Net dollar retention: cohort revenue at month 12 was **136%** of month one; at month 18 it was **186%**


-


Gross margins of ~63%, tracking toward 70%


-


Only 10% of customers ever contacted the inside sales team — the rest self-served entirely


Bessemer had previously avoided the email deliverability space, assuming commoditization risk was too high. When they finally ran reference calls on SendGrid, what they found surprised them: customers displayed "unusually deep adoration" for the product. They weren't buying it because it was cheap. They were buying it because it worked, and because working meant something profound when the alternative was a 20% evaporation rate on your most critical communications. Bessemer noted they had "never lost a customer from the highest spending tier" except for spam violations. The product, at the high end, was essentially churn-proof.


By 2013 — just four years after three engineers showed up in Boulder with a whiteboard — SendGrid was processing **8 billion emails every month** for more than **130,000 customers** . Spotify. Airbnb. Uber. Pinterest. The logos that would define the consumer internet's first decade were all routing their transactional email through SendGrid's infrastructure. The company had become utility infrastructure for the startup economy.


In 2011, Saldana made a decision that founders almost never make voluntarily: he hired someone to be his boss. Jim Franklin — a former Oracle executive — came in as CEO. Saldana stepped down to President and Head of Product. His philosophy, stated plainly: "hire your boss." He recognized he was an exceptional product builder and infrastructure thinker, not a professional CEO. Rather than cling to the title, he removed himself from a role he wasn't built for and put the company above his ego. Most technical founders either never give up the CEO role, or give it up under investor pressure and spend years in low-grade conflict with their replacement. Saldana did it proactively, before it became a crisis.


In September 2014, Franklin was replaced by **Sameer Dholakia** , a Group VP from Citrix. Growth had been decelerating. The company was around $30 million in trailing revenue and losing money. Dholakia's mandate was clear: build this into an IPO-ready business. He expanded the product surface — adding marketing email capabilities so customers could consolidate their entire email operation onto one platform — built out a proper enterprise sales motion, and quadrupled revenue over four years. By 2017, SendGrid was processing **36 billion emails per month** and approaching **$100 million in annual revenue** .


---


## The First Accelerator IPO


**November 16, 2017. New York Stock Exchange. Ticker: SEND.**


SendGrid priced at $16 per share — above its target range of $13.50 to $15.50. It raised **$131 million** . Morgan Stanley and JP Morgan ran the books. The company had 58,000 customers and 408 employees.


The milestone that barely registered in the tech press: SendGrid was **the first company to go public that had come out of any accelerator program** . Not just the first Techstars company. The first from YC, 500 Startups, Techstars, all of them combined. The entire accelerator model as a category had been producing private companies, acquired companies, and dead companies for over a decade. None had produced a public company. SendGrid was the first.


David Cohen and Brad Feld — who had been running the program since 2006 and had mentored Saldana through that summer of 2009 — had built the thing that was supposed to produce outcomes like this, and now one finally had. It was, for the Techstars ecosystem, an eleven-year proof of concept.


Less than a year later — in October 2018 — Twilio announced it would acquire SendGrid for approximately **$2 billion in stock** . The deal closed February 1, 2019. By the time it completed, stock appreciation had pushed the effective value to approximately **$3 billion** .


The strategic logic was clean. Twilio had built the dominant platform for programmable voice, SMS, and video — every channel developers used to communicate with users programmatically, except one. Jeff Lawson, Twilio's co-founder and CEO, had long understood that email would eventually need to join the platform. SendGrid was the obvious target: same developer-first distribution model, same API architecture, same obsession with reliability. The combined entity served more than 140,000 active customer accounts and powered more than 600 billion annualized interactions.


Lawson's framing at the announcement captured exactly the thesis that had animated Saldana's whiteboard nine years earlier: *"Effective customer engagement is a strategic imperative for every company. With SendGrid now a part of Twilio, our goal is to provide a complete platform for every form of customer engagement."*


The acquisition was, in a sense, the logical endpoint of the API economy's internal logic. Twilio had asked "why is every company building their own telephony infrastructure?" Stripe had asked it about payments. SendGrid had asked it about email. Each company found the same answer — the complexity was real, the domain knowledge was real, the ISP relationships and the bounce management and the reputation scoring were real — and none of it needed to be rebuilt by every developer who ever needed to send a notification.


The oldest communication protocol on the internet. Born in 1971. And it took until 2009 for someone to build the company that would finally make it work reliably at scale, as a service, for everyone.


---


## Five Things Nobody Knows About SendGrid


**1. The company had its first paying customer before it had co-founders.**
Saldana started building the product in November 2008 and got his first customer in March 2009 — before Jenkins and Lopez formally joined, and months before the Techstars application. By the time the three of them walked into the Boulder program together, there was already a product with real users. The co-founders joined a company that was already working, not an idea on a napkin.


**2. The name "SMTP API" was killed by a mentor in one conversation — and the rebrand changed the company's trajectory.**
The original name was honest: it was literally an SMTP API. But Feld and others pointed out during Techstars that "SMTP" was a conversation-stopper with anyone who wasn't a developer. CEOs, CMOs, investors — they stumbled on it. "SendGrid" required no technical literacy. The rebrand happened inside the accelerator, before they had institutional funding, and it changed who they could sell to. The name change was not cosmetic. It was a business strategy.


**3. Bessemer Ventures admitted they had "over-thought" the email deliverability space for years.**
The Series B memo — now publicly available — includes the explicit statement that email infrastructure "appears to be a market that we over-thought." They had avoided the category due to perceived commoditization risk: if email was a commodity, why would anyone pay a premium? The reference calls answered the question. Customers didn't describe SendGrid as a commodity. They described it with what Bessemer called "unusually deep adoration." The product had solved a problem that felt impossible to solve, and customers remembered that feeling every time an email arrived.


**4. The flywheel was invisible — but it was the actual moat.**
The more email SendGrid sent, the better their deliverability. Better deliverability attracted more customers. More customers meant more sending volume. More sending volume meant stronger ISP reputation signals. Stronger reputation meant better deliverability. The loop ran silently underneath everything visible about the company. By the time a competitor could replicate SendGrid's API, they couldn't replicate the years of relationship capital SendGrid had built with the postmaster teams at Google, Yahoo, and Microsoft. The technical product was reproducible. The trust was not.


**5. Isaac Saldana voluntarily hired two different CEOs above himself — and stayed.**
Most founders cling to the CEO title or get pushed out. Saldana brought in Jim Franklin in 2011, then Sameer Dholakia in 2014, both times proactively, before external pressure made it inevitable. He remained as President and Head of Product through both transitions and through the Twilio acquisition. This is extraordinarily rare. The normal arc is ego conflict, investor-forced transition, founder marginalization. Saldana's arc was: recognize what you're good at, build the company around that, hire people better than you for the things you're not. The outcome — a $3 billion exit where the technical founder was still present and respected at close — is almost without precedent.


---


See real-time two-way sync in action


Book a demo with real engineers, no sales script.


[Book a demo →](https://www.stacksync.com/book-a-demo)


## SendGrid by the Numbers


Year Metric


March 2009 First customer — before co-founders, before Techstars


Aug 2009 100 customers, 100M emails sent — end of Techstars


Nov 2009 $750K seed round closed


Apr 2010 $5M Series A, Foundry Group leads


2010 $1.4M ARR


2011 $10.2M ARR, 57,000 signups, 7.5B emails/month


Jan 2012 $21.6M Series B, Bessemer leads


2013 8B emails/month, 130,000 customers


2016 30B emails/month, 120,000 paying customers


Nov 2017 IPO at $16/share — $131M raised, first accelerator company to go public


Feb 2019 Acquired by Twilio — deal closed at ~$3B


---


## Infrastructure for Infrastructure


SendGrid's story is not really about email. It's about a category of company that almost never gets built: **infrastructure for infrastructure** .


Email wasn't a new problem in 2009. SMTP had existed since 1982. Every developer knew the problem. Hundreds of companies had tried to build around it. What Saldana, Lopez, and Jenkins understood — and most of their predecessors didn't — was that the solution wasn't better email. It was making email invisible.


The best infrastructure is the kind you never think about. SendGrid's ambition wasn't to be interesting. It was to be forgotten — in the best possible way. You add the API key, email works, and you never open a spam report or check an IP blacklist again.


They built the boring thing. The boring thing sent 36 billion emails a month and sold for $3 billion.


The API economy has a pattern. Someone looks at a complex, domain-specific problem that every company is solving independently. They build the solution once, wrap it in an API, and sell it to everyone. Twilio did it with telephony. Stripe did it with payments. Plaid did it with banking data. Stacksync does it with CRM and database sync.


SendGrid did it with email in 2009 — eight years before the category had a name, and fifteen years before most people could articulate what the API economy even was. They just saw a whiteboard with 20% written on it, and decided that number was unacceptable.


---


*Sources consulted: WhoAPI Isaac Saldana interview; Foundry Group Series A announcement (April 2010); Bessemer Venture Partners SendGrid investment memo (Series B, 2012); Brad Feld / Feld Thoughts (December 2009); Network World Boulder Startup Spotlight; Timescall.com Boulder SendGrid feature (2015); CanvasRebel Isaac Saldana profile; TechCrunch IPO coverage (November 2017); Twilio press releases — acquisition announcement and closing (2018–2019); CanvasBusinessModel SendGrid history; Techstars Sameer Dholakia podcast; Built In Colorado IPO coverage; Startupik SendGrid infrastructure explainer.*
