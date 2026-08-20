---
schema_version: "1.0.0"
document_id: "f3068c8a2f75acfa4586b304663f5440e8e3184c38c2e5cbb2e89294dcf6119d"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/seven-lines-of-code-from-rural-ireland-the-origin-story-of-stripe"
published_at: "2026-03-23T14:32:19+00:00"
first_seen_at: "2026-07-20T23:20:38.087450+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:6066cb3a65b27c981cfbcae8a677304ee8008867b46919005f4c95bec52f5fac"
---

# Seven Lines of Code From Rural Ireland: The Origin Story of Stripe

## The Demo Day That Started It All


The room was full of Y Combinator founders — twenty-something builders nursing laptops and rehearsed pitches, the usual controlled chaos of Demo Day season. Patrick Collison moved through that room differently. He didn't pitch. He didn't ask.


He just picked up your laptop.


"Right then," he'd say. And before you'd finished your sentence, he'd already opened your terminal. A few keystrokes. A paste. Seven lines of code. Then he'd hand it back, and your website could now accept credit cards.


Not in three weeks. Not after merchant account applications, gateway negotiations, PCI compliance consultants, or fax machines. Right now. This afternoon.


Paul Graham watched this happen enough times that he named it. The *Collison Installation* . It became legend in YC lore — not just as a growth tactic but as a statement about who Patrick and John were: people who solved the problem while you were still explaining it.


The thing they were solving? Everyone else had decided it was unsolvable. Or at least not worth the effort.


They were wrong.


---


## From a Village of a Hundred to a Million Dollars


### The Village


Dromineer is not a place that produces Silicon Valley founders. Population under a hundred. A small lake. The sound of wind and cattle. Located in County Tipperary, in the belly of rural Ireland, it has one main road and views of Lough Derg that are, depending on your temperament, either peaceful or suffocating.


Denis Collison, an electrical engineer, ran the Dromineer Bay Hotel. His wife Lily, a microbiologist turned entrepreneur, helped keep it alive. They raised three boys. The eldest, Patrick, was born September 9, 1988. John came three years later.


For most of their childhood, the internet at home was so unreliable it was nearly useless. The modem connected, then didn't, then connected again. So the brothers did something unusual for the era: they read books instead.


After school each day, Patrick and John would walk to the Nenagh library — the nearest town large enough to have one. They'd sit for hours. Patrick recalled years later, with the dry humor of a man who'd made a billion dollars since: "I suspect they were sick of us by the time we eventually left."


That library had a shelf of computing books that most borrowers ignored. Patrick found them first.


He was eight years old when he enrolled in computing courses at the University of Limerick. Not because his parents pushed him — they didn't. He just needed to know how programs worked. By ten, he'd bought a programming textbook at Eason's in Limerick city and taught himself to code.


By twelve, he was building things.


### The Prodigy


In January 2005, sixteen-year-old Patrick Collison stood at the RDS in Dublin in front of President Mary McAleese, Ireland's head of state, and received the trophy for winning the 41st Young Scientist and Technology Exhibition.


His project was called CROMA — a new dialect of LISP, the programming language invented in 1958 at MIT. Patrick had built his own variant. The judges said: *"CROMA is a powerful, general-purpose language in which web applications can be written more quickly."* He took home €3,000, a Waterford Crystal trophy, and a place representing Ireland at the EU Contest for Young Scientists in Moscow.


He was a fourth-year student at Castletroy College in Limerick. He'd started the project because he found existing web programming tools frustrating — too verbose, too rigid, too slow. So he built a better one.


His younger brother John was watching. And learning.


John would later achieve the highest Leaving Certificate score ever recorded in Irish history — ten As, including eight A1 grades. He was admitted to Harvard to study physics. Nobody in Dromineer had done anything like that. Nobody in most of Ireland had done anything like that.


The brothers were not ordinary children who became extraordinary adults. They were extraordinary children who'd simply found better problems to solve.


### Auctomatic: The First Million


In 2007, Patrick was seventeen and John was fifteen. They had an idea about eBay — specifically about the chaos facing high-volume sellers who were drowning in listings, inventory, and manual tracking. They built a tool. They called it Shuppa (from *siopa* , the Irish word for "shop"). They applied for funding from Enterprise Ireland, the government agency that backed Irish startups.


Enterprise Ireland said no.


So they looked west.


Y Combinator, Paul Graham's accelerator in Mountain View, said yes. The brothers moved to California. They merged with two Oxford graduates — brothers Harjeet and Kulveer Taggar who were building something similar — and renamed the company Auctomatic.


It was 2007. Patrick was still a teenager. They designed it, built it, launched it, and sold it in ten months.


On Good Friday, March 2008, Auctomatic was acquired by Live Current Media, a Canadian company, for $5 million. Patrick Collison was nineteen years old. John was seventeen.


Millionaires. Both of them. From Dromineer, Tipperary, Ireland.


Patrick enrolled at MIT. John set his sights on Harvard. They had money. They had a network. By every reasonable measure, they could have coasted for years.


They lasted about eighteen months before the next problem became impossible to ignore.


---


## The Banking Hell Nobody Talks About


### The Wall


Patrick and John had been building software their entire lives. Websites, tools, experiments. Side projects and serious ones. After Auctomatic, they kept building.


And every time one of their projects needed to charge for something, they hit the same wall.


The wall had a name: the payment stack. And in 2009, it was a regulatory and technical nightmare dressed up as a business service.


Here is what you had to do to accept credit card payments online in 2009:


Apply for a merchant account. This meant submitting business documentation to a bank or payment processor, waiting weeks, often being denied, then waiting again. If approved, you needed a payment gateway — a separate service that sat between your code and the credit card networks. You needed to handle PCI DSS compliance, a security certification that required either an expensive consultant or a hundred-page self-assessment questionnaire. Your API documentation, if you could find it, was written in 2001 and hadn't been updated since.


If you were a small business or a first-time founder? You sent faxes. Literal faxes. You filled out paper forms and mailed them to banks.


Patrick Collison, reflecting on this later with characteristic Irish understatement, said: *"It seemed bizarrely anachronistic. What you wanted was a straightforward API for charging credit cards."*


He'd been able to build a programming language at sixteen. He'd already sold a company. He could spin up a server, deploy a web application, and buy a domain name faster than he could boil water. But getting paid? That required mailing a form.


The reason such an API didn't exist, he would later explain, was structural: *"You had to be really good at technology, but you also had to be quite sophisticated in navigating the financial industry, and the legal industry, and global expansion."* Nobody had sat at the center of all three. The banks didn't understand developers. The developers didn't understand banking law. The gateways understood neither.


And so the problem persisted — not because it was unsolvable, but because the people capable of solving it had never been frustrated enough to try.


### Buenos Aires


In October 2009, Patrick Collison had an idea. He described it later: *"Maybe it would be interesting to go Slice Host for payments."* Slice Host was a hosting provider famous for making server provisioning instantaneous and clean — you picked a plan, you clicked a button, you had a server. What if payments worked like that?


He called John.


They dropped out. Patrick from MIT, John from Harvard. Both parents were, by various accounts, supportive, or at least not destructively opposed. The brothers had proved enough by then that parental anxiety was hard to sustain.


They needed somewhere cheap, warm, and disconnected enough from Silicon Valley's noise to think. They chose Buenos Aires.


For roughly a month, the Collison brothers lived on about $10 a day each, writing code in cafes, arguing about API design, and building the first prototype of what they were calling */dev/payments* — a Unix-style name that announced its target audience without explanation. If you had to ask what it meant, you weren't the user.


The goal was radical simplicity. Not "easier than PayPal." Not "faster than banks." Something that felt like it had always existed and should have always existed: a few lines of code that made the problem disappear.


They came back to the US with a working prototype.


### /dev/payments


Y Combinator accepted them into the Spring 2010 batch. Paul Graham, who had first met Patrick through his work with Lisp — the programming language Patrick had built a dialect of at sixteen — was already a believer.


The product was still rough. The company was still called /dev/payments. (Delaware later forced a name change: the state does not permit leading slashes in corporation names. The brothers considered various alternatives and settled on *Stripe* — clean, geometric, chosen partly because it would appeal to the traditional bankers they needed to work with as much as to developers.)


The early team was tiny. Four people. Then five. The hiring bar was punishing. The Collisons took nearly two years to hire their first five employees. Candidates worked with the team for a full trial week before receiving an offer. Technical founders couldn't afford to train anyone. Everyone had to ship from day one.


They gave those first ten employees over 10% of the company's equity — a generosity rare enough to be noticed. The signal was intentional: *we are building something worth owning* .


### The Collison Installation


Word spread through the Y Combinator network the way it always does: one founder told another. But the Collisons didn't wait for word of mouth. They accelerated it manually.


When another founder mentioned they were struggling with payments, Patrick or John would appear, laptop already open. They'd say: *"Give me your machine."* And then they'd set it up. On the spot. No ticket. No sales pitch. No follow-up call.


The founder would be accepting payments before the coffee cooled.


Paul Graham formalized what he was watching into an essay titled *"Do Things That Don't Scale."* He coined the specific term for what the Collisons were doing: the *Collison Installation* . He wrote: *"When anyone agreed to try Stripe they'd say 'Right then, give me your laptop' and set them up on the spot."* Graham held it up as the exemplary case of founder hustle — the kind of thing that looked inefficient and was actually the only thing that worked.


Their first user — their very first — was Ross Boucher, founder of 280 North, a web development company. Boucher was so impressed with the product that he eventually joined as one of Stripe's first employees.


Their first twenty to thirty users all came through YC connections. These weren't random developers. They were technically sophisticated founders who gave real feedback and had real stakes. The premium the Collisons charged during beta — 5% plus $0.50 per transaction, roughly double competitors — was deliberate. It filtered for users who valued the product, not the price. It forced Stripe to be better, not just cheaper.


### The Banking Hell Nobody Talks About


What startup histories understate, because the founders usually downplay it, is how brutally hard the regulatory and banking side of this was.


Building the API was the easy part. Getting actual payment infrastructure was another matter. Stripe had to build relationships with banks, negotiate with card networks, acquire money transmission licenses, and navigate a financial regulatory apparatus that was not designed for two Irish brothers in their twenties with laptops and a GitHub repo.


Patrick's explanation was blunt: the reason the problem hadn't been solved before was that solving it required mastery across three domains that never overlapped — technology, financial regulation, and global legal compliance. The banks didn't trust scrappy startups. The card networks had their own rules. The compliance requirements were designed for enterprises.


The Collisons did it anyway. One relationship at a time. One license at a time. Slowly, stubbornly, without announcing it.


This was the invisible work — the part that looked like nothing from the outside, that produced no blog posts, that generated no press coverage, and that was the actual hard part of what they were building.


---


## Seven Lines of Code, and the Flywheel


### September 2011


Stripe launched publicly in September 2011. It had been in private beta for over a year. The product was polished — not because the founders were perfectionists in the academic sense, but because they'd been obsessively watching how developers actually used it and fixing every friction point they found.


By then they'd raised $2 million in seed funding. The investor list read like a PayPal reunion: Elon Musk. Peter Thiel. Max Levchin. All three had co-founded PayPal. All three looked at what Stripe was doing and understood immediately — they'd tried to solve this problem from the consumer side. Stripe was solving it from the developer side, which nobody had done.


Sequoia Capital. Andreessen Horowitz. SV Angel. Everyone wanted in.


The valuation at seed: $20 million. For a company with almost no revenue, barely any employees, and a product that had been live for less than two years.


### What Developers Found


When developers actually used Stripe, something happened that doesn't happen very often with developer tools.


They told their friends. Unsolicited.


Tech bloggers wrote about it without being asked. Hacker News threads filled with developers saying things like: *finally* and *this is what I've been waiting for* and *I've been building workarounds for three years and I just deleted all of them.* A YC partner posted on Hacker News inviting developers who needed payments to try the beta. Three hundred signups came in immediately.


The simplicity was the thing. You opened a terminal. You ran a curl snippet. You saw a successful charge object returned. The whole pipeline — card tokenization, charge creation, response — worked on the first try, in an afternoon, with documentation that was actually readable.


Patrick's vision had been precise: *"Taking something as complex as credit card processing and reducing the integration to only a few lines of code is really quite magical."* The product delivered on that exactly.


What once required a compliance team, two lawyers, three weeks, and a fax machine now required an afternoon.


Developers didn't just use Stripe. They became missionaries for it.


### The Flywheel


YC companies used Stripe. Those founders went on to build more companies — and used Stripe at those too. The investors in those companies recommended Stripe to their other portfolio companies. The developers who integrated Stripe at one job carried the preference to their next job.


Silicon Valley's network, for once, wasn't just amplifying hype. It was amplifying a product that genuinely deserved amplification.


Within three years of public launch, Stripe was worth over $1 billion.


In February 2012 — five months after going public — they raised an $18 million Series A at a $100 million valuation. The company that had been two brothers in Buenos Aires with $10 a day and a prototype was now a unicorn in everything but the official designation.


---


## The Youngest Self-Made Billionaire in the World


### The Youngest Billionaire


In 2016, John Collison was named the youngest self-made billionaire in the world. He was twenty-five years old.


He'd dropped out of Harvard six years earlier to write code with his brother in Palo Alto. He'd grown up in a village of a hundred people in rural Ireland, walking to a library in a nearby town to read computer science books because their home internet was too unreliable to use.


The headline was written a thousand times: *Harvard dropout becomes world's youngest billionaire.* The writers meant it as irony. The Collisons experienced it as logic.


### What Stripe Became


The seven lines of code became a financial operating system.


Stripe Atlas: incorporate a company in Delaware from anywhere in the world, in days. A founder in Lagos or Bangalore could now access US banking and legal infrastructure without ever stepping on a plane.


Stripe Radar: machine learning fraud detection that got better with every transaction Stripe processed — a network effect applied to trust.


Stripe Treasury: banking-as-a-service APIs that let platforms offer their users FDIC-insured accounts, debit cards, and money management — without becoming a bank themselves.


Stripe Issuing: create and distribute virtual and physical cards programmatically.


Stripe Climate: let customers direct a fraction of their purchase toward carbon removal at the moment of checkout.


Each product a layer. Each layer expanding what it meant to "accept a payment." By 2021, Stripe wasn't a payment processor. It was the economic infrastructure of the internet — the layer between ideas and revenue for millions of businesses.


### The Valuation


At peak in 2021, Stripe was valued at $95 billion. The most valuable private startup in the United States, built by two brothers from a village of a hundred people whose childhood library happened to stock programming books.


By early 2026, that number had climbed to $159 billion.


The company had processed hundreds of billions of dollars in transactions. Millions of businesses — from one-person Shopify stores to Amazon, Google, and Salesforce — ran on Stripe. The GDP of the internet, as Patrick had called it, was measurably larger because of what they'd built.


### The Men Themselves


Patrick, now in his mid-thirties, maintains a personal website with hundreds of book recommendations and essays about scientific progress, ambition, and the nature of hard problems. He reads voraciously. He thinks publicly. He is, by all accounts, exactly the same person he was at sixteen — someone who finds the friction in a system and can't stop thinking about removing it.


John focuses on operations, international expansion, and building the institutional Stripe that will outlast its founding moment. His instinct — the paranoia against complacency he described — keeps the company honest. Biweekly customer feedback sessions. Direct access to what's breaking. No buffering between leadership and reality.


Neither brother appears particularly interested in the theater of being a billionaire. No yachts. No tabloid drama. No tweets about Mars or politics. Just the work.


They went back to the Nenagh library once, years after making their billions. Patrick told the librarian: *"I suspect they were sick of us by the time we eventually left."*


They were not, in fact, sick of them.


---
