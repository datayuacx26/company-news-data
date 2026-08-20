---
schema_version: "1.0.0"
document_id: "0e6f9b04b868aa789bd373120c6f74888d5989a16fc7b051e7280418093d8845"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/text-fund-twilio-to-this-number-the-origin-story-of-twilio"
published_at: "2026-03-23T14:32:55+00:00"
first_seen_at: "2026-07-20T23:20:38.087450+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:0056a737dc5c24ff8baf82e2820a1549bfe40908fa7c87854c7306c48e8bcd54"
---

# Text 'Fund Twilio' to This Number: The Origin Story of Twilio

## The Demo That Changed Everything


November 2008. San Francisco. Jeff Lawson has a new company with no customers, no revenue, and a product the telecommunications industry said couldn't exist.


He builds a prank app on his own API. He uses it to rickroll Michael Arrington, the founder and editor of TechCrunch — calling his phone and playing Rick Astley's "Never Gonna Give You Up." Arrington gets pranked. The room laughs. The next day, TechCrunch writes about the actual product launch.


That's how Twilio announced itself to the world: not with a press release, not with a keynote deck — with a phone call made in minutes from a few lines of code.


The message was the prank: *anybody* could do this now.


---


## Amazon's First Day, and the Lesson Jeff Carried Out


Jeff Lawson joined Amazon in the fall of 2004, hired as one of the first product managers for a small, mostly-secret internal team. They were building something called Amazon Web Services.


On the morning of his very first day, the S3 team had a design review with Jeff Bezos. It was supposed to be the final review before launching that week. Bezos sent them back to the drawing board. The team had assumed they'd just re-architect the service to scale linearly if it got traction. Bezos didn't accept that. They rebuilt from scratch. It took over two years.


Lawson spent 15 months at AWS — long enough to watch S3 and EC2 go from internal experiments to publicly available services. Long enough to understand what happened when you gave developers direct access to infrastructure they previously couldn't touch.


The lesson wasn't just technical. It was philosophical: developers don't need products handed to them. They need building blocks and the freedom to build.


He left Amazon when the company decided not to build what he wanted to build. The problem he had identified was sitting in plain sight across every development team he'd ever worked with: **communications were nearly impossible to program.**


Not complex. Nearly *impossible.*


He had experienced this at three different companies before — Versity (online lecture notes), StubHub (joined as founding CTO, went from first line of code to launch in six weeks), and a brick-and-mortar skate and surf shop in Southern California where he found himself writing point-of-sale software and realizing he belonged back in technology.


At every company, the same moment had happened: we need to call or text a customer, and the only way to do it was to call AT&T, who would tell you it would take two years, cost millions of dollars, and begin with rolling out copper wire.


On March 13, 2008, Lawson co-founded Twilio with Evan Cooke and John Wolthuis.


---


## Building Primitives When Everyone Wanted Products


The original pitch to VCs was straightforward: make telecommunications programmable with a simple API and pay-as-you-go pricing.


One VC literally laughed Lawson out of his office. The idea of targeting developers — not enterprises, not telecom resellers — as a primary customer was considered strategically naive. Most investors at the time viewed developers as an audience to "win over," not a revenue-generating channel.


Lawson bet differently. He believed if you built something developers genuinely loved, they would bring it inside their organizations and the organizations would follow.


**The validation method was unusual.** Before building, Lawson talked to developers and described the solution. He watched for a specific reaction: if people connected with the problem, within 30 seconds they'd articulate their own use case unprompted — "Could I notify a customer when a package ships?" — and propose something Lawson hadn't thought of. That organic response was the signal. When it didn't happen, conversations went awkward and pivoted to other topics. He got the signal repeatedly.


**The core architectural decision:** Twilio would build primitives — SMS, voice, video, email — not end products. The reasoning was deliberate. End products lock you into specific use cases. Primitives allow developers to invent use cases you never imagined. A telecom incumbent building an enterprise communication suite would never allow a developer to build a rickroll app in three lines of code, send a one-time password for two-factor authentication, or power a rideshare dispatch system. Twilio made all of that the same product.


**The pricing was designed to kill the incumbent model.** Traditional telecom demanded million-dollar contracts and two-year timelines. Twilio's original pricing was $0.03 per minute for voice calls and $0.03 per SMS — pure pay-as-you-go. No contracts, no minimums, no enterprise negotiations. You put a credit card in, you built something, you paid for what you used.


**The first major social proof moment:** In 2010, GroupMe was built at TechCrunch Disrupt's hackathon. It used Twilio's SMS API as its entire backend for group text messaging. Skype acquired GroupMe for $85 million. GroupMe had been built on Twilio. For developers who watched that happen, it wasn't abstract infrastructure anymore — it was a viable business in a weekend.


**The community play:** In 2010, investor Dave McClure's 500 Startups created the Twilio Fund — $250,000 in seed funding dedicated to startups building on Twilio's APIs. This wasn't charity. It was compound interest on developer mindshare: every startup that built on Twilio became a case study, a validator, and a recruiter for the platform.


**The culture was the product.** Every new employee at Twilio — regardless of role, regardless of technical background — was required to build a Twilio app and present it company-wide. Non-engineers got coding boot camps. Complete it and you received a branded track jacket and $30 monthly Amazon Kindle credit. The ritual kept the entire company close to the developer experience. They called it "Hatch."


---


## COVID Made Twilio Essential Overnight


In 2019, Twilio went public at $15 per share. The stock opened at $23.99 — up nearly 60% — and closed at $28.79. Almost 92% gain on the first day. It was the first US venture-backed tech IPO of 2016 (the correction: Twilio IPO'd on June 23, 2016 — becoming the first major US tech IPO of that year).


At the IPO, Lawson didn't just ring the opening bell. He had three developers — David Huerta, Leah Culver, and Rob Spectre — writing live Twilio apps on the floor of the New York Stock Exchange, streamed live on Twitch, taking audience suggestions in real time. It was a statement: this company belongs to developers.


By 2017, things got complicated. Uber — which had grown to represent 12% of Twilio's total revenue at peak — began pulling back. They had built some communications infrastructure in-house and started diversifying to competitors. The stock plummeted in after-hours trading. Lawson had to explain to analysts that a single customer reducing usage could move the company's quarterly numbers. It exposed a structural concentration problem: in 2015, just 10 accounts represented 32% of Twilio's revenue, with WhatsApp and Uber alone accounting for 26%.


Then COVID arrived.


In March 2020, everything that had been moving to digital communication in years suddenly moved in weeks. Telehealth platforms needed appointment reminders and video calls. Schools needed mass SMS notifications. Contact tracers needed automated outreach at scale. Vaccine distribution would eventually require text-message coordination for a billion people.


Twilio's own research found that COVID-19 accelerated companies' digital communications strategy by an average of **six years.** 97% of enterprise decision makers said the pandemic sped up their digital transformation. 53% of companies added new communication channels during the pandemic. 1 in 3 companies used live chat or IVR for the first time because of COVID.


Twilio announced that within 12–24 months, one billion people would receive COVID-related communications powered by its platform.


The stock that had struggled after the Uber problem went from roughly $70 at the start of 2020 to over $400 by early 2021, as Twilio crossed $1 billion in ARR while still growing at over 80%.


---


## Building an Empire on Top of the Primitives


**SendGrid — October 2018 (closed February 2019) — $2–3 billion in stock**


When Twilio bought SendGrid, the tech press framed it as a voice/SMS company buying an email company. The real logic was simpler: developers needed all the communication channels in one place. SendGrid processed over 45 billion emails per month. The combined platform could now offer developers a single API layer for voice, SMS, WhatsApp, video, and email. CEO Jeff Lawson was direct about the motive: "A huge part of the calculus for both sides was the cross-sell opportunity."


**Segment — October 2020 — $3.2 billion in stock**


The Segment acquisition was Lawson's most ambitious move. Segment was a customer data platform (CDP) — infrastructure for collecting, unifying, and routing customer behavioral data from any source to any destination. Lawson's argument: "Data silos destroy great customer experiences. Segment lets developers and companies break down those silos and build a complete picture of their customer." The combined total addressable market they cited: $79 billion.


The strategic vision was clear — Twilio wanted to own the entire customer engagement stack: know who your customer is (Segment), and reach them on any channel (Twilio). At peak valuation, this positioning made Twilio worth over $60 billion.


---


## The Boardroom Battle


By 2022, the macro environment had turned against high-growth, high-valuation SaaS companies. Twilio's stock, which had peaked above $400, fell below $80. The Segment and SendGrid acquisitions, which were all-stock deals made at peak valuations, now looked expensive. Profitability had been subordinated to growth for too long.


In 2023, two activist investors arrived.


**Legion Partners** and **Anson Funds** built stakes in Twilio and began applying pressure. Legion Partners held at least six meetings with Twilio's leadership, pushing for board changes and divestitures — specifically, they wanted Twilio to spin off or sell its data and applications business. Anson Funds sent a letter to the board pushing for the sale of the entire company.


On January 8, 2024, Jeff Lawson stepped down as CEO and board member. He was replaced by Khozema Shipchandler, a longtime Twilio executive.


Lawson, who had built the company from a rickroll app in 2008 to a $60B peak market cap in 2021, was out.


Even after his departure, Anson Funds stated it was "a step in the right direction" but that more actions were needed to maximize shareholder value.


The man who had argued that developers were the most important workers in the digital economy had been removed by investors who primarily wanted cost cuts and margin expansion.


**Postscript:** In April 2024, Lawson used some of his Twilio proceeds to buy The Onion. The satirical news outlet. He bought the thing that makes fun of everything.


---


## Ask Your Developer (2021)


Published in January 2021, Lawson's "Ask Your Developer: How to Harness the Power of Software Developers and Win in the 21st Century" laid out the philosophy behind Twilio in explicit terms.


The central argument: most companies treat software developers like "digital factory workers" assigned to rote tasks, when in fact developers are the creative workforce capable of solving the company's most fundamental business problems.


The provocation: the strategic question has shifted from "build vs. buy" to **"build vs. die."** Companies that fail to integrate developers as strategic partners — not implementation resources — will lose to companies that do.


The contrarian claim: developers are the primary audience for the most important software products, and developers make purchasing decisions that flow upward into organizations. The right go-to-market for a technical product isn't a top-down enterprise sales cycle; it's winning developer conviction first.


The book was coauthored with Eric Ries, the author of The Lean Startup.


---


See real-time two-way sync in action


Book a demo with real engineers, no sales script.


[Book a demo →](https://www.stacksync.com/book-a-demo)


## Twilio.org and the 1% Commitment


In 2013, Twilio launched Twilio.org, providing nonprofits with free and discounted access to its communications APIs.


In September 2015, at Salesforce's Dreamforce conference, Twilio joined Pledge 1% — committing 1% of its equity to fund Twilio.org for 10 years, then worth over $10 million.


The stated goal, in Lawson's words: "enable them to send a billion messages for good."


By 2023, Twilio.org supported 80+ organizations estimated to positively impact 48 million people. By 2024, it had awarded $4.8 million in grants to 40+ organizations. Partners have included the American Red Cross, Doctors Without Borders, and the Polaris Project (anti-human trafficking).


---


## Five Things Most People Don't Know About Twilio


**1. Jeff Lawson was in the room when S3 almost launched — and got sent back.**
His first day at AWS, he watched Jeff Bezos reject the S3 team's final design review because they hadn't built it to scale from scratch. That meeting — on day one of his Amazon career — shaped everything. When Lawson built Twilio, he built it to scale from the beginning.


**2. The rickroll was the product launch.**
Twilio didn't announce with press releases. Lawson built a prank app on his own API, used it to rickroll TechCrunch's Michael Arrington, and let the resulting coverage announce the product. The demo proved the point: complex telephony in a few lines of code.


**3. One VC literally laughed him out of his office for targeting developers.**
The developer-first go-to-market was considered strategically unsophisticated. Enterprise SaaS was the default playbook. Lawson bet the whole company on developers as primary customers — not a marketing channel, not an audience, but the actual buyers. The VC community called it naive.


**4. Twilio's entire customer base almost became a single company.**
At peak, Uber represented 12% of total revenue. WhatsApp and Uber together represented 26% of revenue from just 10 accounts. When Uber pulled back, the stock crashed on a single customer decision. The diversification Twilio spent years engineering was the quiet, unglamorous work behind the "platform" story.


**5. The man who built the $60B communications platform bought The Onion.**
After being pushed out by activist investors in January 2024, Jeff Lawson didn't retire quietly. He used his Twilio wealth to buy America's most famous satirical news outlet. The developer-evangelist who spent 16 years arguing that code is the new literacy is now publishing jokes about the people who will never understand it.


---


## Twilio, From First Call to Public Company


Year Event


2004 Lawson joins AWS as one of first product managers


2006 AWS officially launches (S3, EC2); Lawson exits ~15 months after joining


Mar 2008 Twilio founded (Lawson, Cooke, Wolthuis)


Nov 2008 Twilio Voice API launches; rickroll prank gets TechCrunch coverage


Feb 2010 Twilio SMS API launches at $0.03/message


May 2010 GroupMe built on Twilio at TechCrunch Disrupt hackathon


2010 500 Startups creates $250K Twilio Fund for API-based startups


2011 GroupMe acquired by Skype for $85M — first major Twilio success story


2013 Twilio.org launched


Sep 2015 Twilio joins Pledge 1%


Jun 2016 IPO on NYSE at $15/share; opens at $23.99; live coding on Twitch from trading floor


May 2017 Uber pulls back; stock drops; customer concentration risk exposed


Oct 2018 SendGrid acquisition announced (~$2B in stock)


Feb 2019 SendGrid acquisition closes


Jan 2021 "Ask Your Developer" published


Oct 2020 Segment acquired ($3.2B in stock)


2021 Stock peaks above $400; market cap exceeds $60B


2023 Legion Partners and Anson Funds pressure board; stock below $80


Jan 2024 Lawson steps down as CEO; replaced by Khozema Shipchandler


Apr 2024 Lawson buys The Onion


---
