---
schema_version: "1.0.0"
document_id: "a2804f0958396fa5bf6ea2122a3286514c23064652646e4a7307a0deefd827cf"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/named-thirteen-years-before-the-technology-existed-the-origin-story-of-teradata"
published_at: "2026-03-23T14:32:35+00:00"
first_seen_at: "2026-07-20T23:20:38.087450+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:5bb30d7d0e432168cdd2f0d21f9dd280c0d04be975ff646765688acf687a71a2"
---

# Named Thirteen Years Before the Technology Existed: The Origin Story of Teradata

## 1984: A Machine the World Wasn't Ready For


1984.


A machine rolls into the back office of Wells Fargo Bank. It takes up most of the room.


The processor cabinet stands 60 inches tall and 27 inches wide. It weighs 450 pounds. The storage cabinet next to it — same dimensions — weighs 625 pounds. Together, over 1,000 pounds of steel, disk drives, and Intel microprocessors, just to store and query a database.


The machine is called the DBC/1012. It was built by a seven-person startup that started in a garage in Brentwood, California. The name on the door says Teradata.


The "1012" in the name is not random. It is a statement of ambition so audacious that most engineers in 1984 would have laughed. It stands for 10 to the power of 12 — one trillion bytes. One terabyte. The machine was theoretically designed to scale to that size, in an era when a terabyte of storage was science fiction.


Nobody laughed. Because the machine worked.


And in working, it launched a company that would quietly become the backbone of the world's largest databases — from Walmart's supply chain to American Airlines' ticketing system — for the next 40 years.


---


## Seven People in a Garage, a PhD Thesis, and Citibank


The story begins not in Silicon Valley, but in Pasadena.


California Institute of Technology, 1973. Philip Neches is an engineering undergraduate. He will go on to earn his MS and PhD in computer science — also from Caltech — studying under Carver Mead, one of the pioneers of modern chip design. By 1979, Neches has a PhD thesis, a radical idea, and a burning question:


What if a database wasn't run by a single processor?


What if you distributed the data and the computation across hundreds of small, cheap, parallel processors — each with its own dedicated disk — so that queries could run simultaneously, against their slice of data, and assemble the answer at the end?


This was massively parallel processing, MPP. And in 1979, the dominant view in the database world was that this was impractical. Oracle, IBM, and every other serious player was building databases around single, powerful central processors. The monolithic mainframe was king.


Neches and six co-founders — Jack Shemer, Walter Muir, Jerold Modes, William Worth, Carroll Reed, and David Hartke — thought the mainframe was a dead end.


They incorporated Teradata on July 13, 1979, in a garage in Brentwood, California.


The concept wasn't built in a vacuum. The parallel processing research had been developing since 1976 through a collaboration between Caltech researchers and an advanced technology group at Citibank — the New York megabank that was one of the first financial institutions to take database scale seriously. Citibank wasn't just a future customer. It was, in a sense, a co-creator.


Funding trickled in. $150,000 in seed money in March 1980. A $2.5 million venture round in July of that year. By the standards of the time, these were not large sums. The team was lean. The mission was enormous.


They named the company after what they intended to build: a system for managing terabytes of data. In 1979, no such system existed. No one had ever stored a terabyte commercially. The name was a bet on the future — placed before the future had arrived.


---


## Intel Chips, Winchester Drives, and a Network Named Ynet


For five years, the team built.


What emerged in 1984 was unlike anything the database industry had seen.


The DBC/1012 was not a traditional mainframe database. It was an ensemble of cheap, commodity processors — Intel 8086 microprocessors, the same chips being used in the original IBM PC — each paired with a dedicated 474-megabyte Winchester disk drive. These processor-disk units were interconnected by a custom switching network called the Ynet.


The Ynet was the secret weapon. It coordinated queries across all nodes simultaneously, routing data without bottlenecks, scaling the system as you added more nodes. The architecture was designed to support up to 1,024 interconnected processor-disk pairs. At maximum theoretical scale, that was over 400 gigabytes of storage — a number so large in 1984 that it barely needed to be stated.


Before the machine shipped to its first paying customer, three of the biggest names in American business had already been in the room.


Merrill Lynch. Standard Oil. Procter & Gamble.


They didn't just evaluate the DBC/1012. They lent their own people to Teradata to road test the machine and provide design input. The Procter & Gamble engineers sat with Teradata engineers and pushed the system until it broke, then pushed again. This wasn't a sales cycle. It was a partnership between the company trying to build the future and the companies that needed it.


The first beta system shipped to Wells Fargo Bank in 1983. Citibank became a paying customer in 1984. The machine that had spent five years being built in a garage was now running inside the most sophisticated financial institutions in the world.


Fortune magazine named it Product of the Year in 1986. The entire data warehousing industry — eventually a $128 billion market — can trace its origins to this machine.


---


## The Enterprise-Only Era


Teradata's model in the 1980s and 1990s was singular in its exclusivity.


The DBC/1012 was not for small companies. It was not for the curious. It was for organizations with massive data problems, massive IT budgets, and the organizational patience to operate a system that weighed half a ton and required specialized expertise.


A typical 22-processor configuration in 1988 cost $1.8 million. By the time on-premise Teradata appliances reached their mature form, the company was publishing a starting price of $34,000 per terabyte of uncompressed user data space. A full cabinet configuration — 83 terabytes — would cost roughly $2.8 million at list price. And that was before annual maintenance.


The U.S. Navy signed a single-year maintenance and support contract in 2014 — covering just three Teradata systems — for $4.6 million. One year. Three machines.


This pricing wasn't a mistake. It was a feature. Teradata built the most reliable, most powerful, most capable large-scale database systems on earth, and it charged accordingly. Its customers — the world's largest banks, retailers, telecoms, and government agencies — accepted this because they had no alternative. The cost of not having Teradata was greater than the cost of having it.


The company went public on the NYSE in August 1987, trading as TDC. It was a technology company unlike most technology companies: unglamorous, expensive, deeply technical, and essential. Not essential the way a consumer app is essential. Essential the way a bank vault is essential.


---


## Walmart and the Invention of Retail Analytics


In 1992, Teradata did something that changed retail forever.


It built the first database system to exceed one terabyte — for Walmart.


The number barely registers now. One terabyte is what you can buy on a USB drive at an airport gift shop. In 1992, it was an event serious enough to be reported as a milestone in the history of computing.


What Walmart did with that terabyte is more interesting than the terabyte itself.


By 1991, Walmart had launched Retail Link — a system that collected point-of-sale data from stores across the country, consolidated it, and shared forecasting information with suppliers. By 1995, when they formalized their decision-support system partnership with AT&T (which then owned Teradata), their database had grown to 7.5 terabytes. By 1997, it had grown to 24 terabytes. By 1999, a Teradata customer was running the world's largest database at 130 terabytes.


That customer was Walmart.


The database gave Walmart something its competitors didn't have: near-perfect visibility. Not just what sold, but where, when, at what price, and how it interacted with the 65 weeks of historical data the system maintained on every item, in every store, every day. Thirty or more applications ran against the warehouse simultaneously, handling up to 50,000 queries per week.


Walmart used this to achieve what no retailer before them had achieved at scale: the right item, at the right store, at the right time, at the right price — everywhere, at once. When Warner-Lambert connected to the system via the internet in 1998 to streamline supply chain forecasting, the two companies reduced supply chain time by two and a half weeks and generated millions of dollars in reduced inventory costs. From a single data connection.


By 2013, Walmart's Teradata database had grown to 30 petabytes. Thirty million gigabytes. The 1992 system — one terabyte — fit inside a modern iPhone seven hundred times over.


Teradata didn't just build databases. It built the information infrastructure for the most successful retailer in human history.


In the mid-1990s, every serious enterprise was asking the same question: what is Walmart doing that we're not? The answer, consistently, came back to data. And the data ran on Teradata.


Banks, airlines, telecoms, insurance companies — they all came. By the early 2000s, Teradata was running the analytical backbone of institutions that collectively accounted for trillions of dollars in economic activity. Verizon. American Airlines. Apple. T-Mobile. Southwest Airlines.


The machine that started in a garage in Brentwood now ran the world's nervous system.


---


## Garage to AT&T in Twelve Years


Teradata's ownership history is a lesson in how the technology industry consolidates around power.


1979: Seven people in a garage in Brentwood.


1989: Teradata enters a joint venture with NCR Corporation — National Cash Register, the 105-year-old industrial company that had been digitizing commerce since the 1880s.


1991: AT&T acquires NCR Corporation. In the process, AT&T acquires Teradata for approximately $250 million. A database startup founded on a PhD thesis, twelve years old, selling machines that cost $1.8 million each, is now a division of one of the largest corporations in America.


1997: AT&T spins off NCR (including Teradata) as an independent public company.


2007: NCR announces it will spin off Teradata as a fully independent public company. On October 1, 2007, Teradata begins trading independently on the NYSE under the ticker TDC, with Michael Koehler as its first CEO.


From garage to AT&T subsidiary to independent public company: 28 years, three owners, and a market capitalization that at its 2012 peak would have seemed inconceivable to the seven people who started it.


---


See real-time two-way sync in action


Book a demo with real engineers, no sales script.


[Book a demo →](https://www.stacksync.com/book-a-demo)


## When the Dinosaur Refused to Evolve


Snowflake was founded in 2012.


Teradata's stock hit its all-time high that same year.


The timing is not coincidental. It is, in retrospect, a perfect diagram of the moment the data warehousing world began to fracture.


Snowflake's founders — two Oracle architects and a database researcher — bet on something that Teradata's entire architecture made difficult: the separation of storage from compute. In Teradata's model, you bought a box. The storage and the compute were bundled together in that box. You paid for the whole thing, whether you were using 10% of its capacity or 100%.


Snowflake said: what if you only paid for what you used? What if the storage lived in the cloud — cheap, elastic, infinite — and the compute spun up when you needed it and disappeared when you didn't? What if a startup with $50,000 could access the same data warehouse capability that Walmart was paying millions to maintain?


Teradata heard this argument. They were not deaf to the cloud. But they were slow. The organizational inertia of selling $1.8 million machines to Fortune 500 companies — the sales cycles, the enterprise relationships, the professional services revenue — made it genuinely difficult to walk away from a model that was still generating billions in revenue.


Between 2018 and 2020, Teradata's revenue shrank from roughly $2.2 billion to $1.8 billion. The stock fell 75% from its 2012 peak by mid-2020.


In October 2018, Teradata launched Vantage — its cloud analytics platform. By 2022, it had evolved into VantageCloud, a full hybrid and cloud offering deployable across AWS, Azure, and Google Cloud. The product was real. The strategy was real. But the timing was contested.


Today, Teradata is running. Cloud ARR grew 15% in 2025. Total ARR sits at $1.49 billion. Revenue is $1.66 billion — declining, but generating $260-280 million in free cash flow. The company has 90% institutional ownership and an active $500 million share buyback program.


But Snowflake's IPO in September 2020 — the largest software IPO in history at the time — told a different story about where the market's imagination had gone.


Snowflake went from zero to a $70 billion market cap in eight years. Teradata's current market cap: approximately $3 billion.


The dinosaur didn't die. But the meteor has already landed.


---


## Forty-Five Years Old, Still Running the World


Here is the thing about Teradata that almost nobody talks about.


The companies that run on Teradata today — Verizon, American Airlines, Apple, major global banks — are not running on it because they forgot to migrate. They are running on it because migrating is genuinely, structurally difficult.


Teradata databases are not just databases. They are decades of business logic, query optimization, schema design, and process integration baked into a system that has been running reliably, at scale, for longer than most of its administrators have been alive. The institutional knowledge required to migrate a 30-petabyte Teradata installation is not a project. It is a multi-year transformation program with significant risk of operational disruption.


When organizations have tried to migrate from Teradata to Snowflake, the results have sometimes been surprising — not because Snowflake is inferior, but because the differences in architecture produce unexpected cost structures. Migration teams arrive with projected budgets; the first quarterly cloud bill arrives at double. Dashboards that queued in 2 minutes on Teradata take 20 minutes on Snowflake. Data loading pipelines that ran in 45 minutes now take 5 hours. The architectures are not equivalent. They are different solutions to related problems, and the translation is imperfect.


Teradata's storage costs $34,000 per terabyte at list price. Snowflake's storage costs roughly $1 per day per terabyte. The math looks obvious — until you account for compute, concurrency, migration complexity, retraining, and the organizational risk of moving the database that runs everything.


For a subset of the world's largest organizations, Teradata is not a legacy system. It is the least risky option.


That is not nothing. That is, in fact, the entire business model.


---


## Five Things Most People Don't Know About Teradata


**1. The company was named for a scale no computer had ever reached.**
In 1979, when seven people named their startup "Teradata" — after "terabyte" — no computer had ever stored a terabyte of data commercially. The first commercially available terabyte database wouldn't exist until 1992. They named the company after a future that wouldn't arrive for 13 years.


**2. The DBC/1012 used the same chip as the original IBM PC.**
The revolutionary massively parallel database machine of 1984 ran on Intel 8086 microprocessors — the same chips inside the original IBM PC, introduced three years earlier. Teradata's genius wasn't in using exotic hardware. It was in connecting hundreds of commodity chips together in a way nobody had done before.


**3. Merrill Lynch, Standard Oil, and P&G helped design the machine — for free.**
Before the DBC/1012 shipped, Teradata ran a "Partners programme" in which engineers from Merrill Lynch, Standard Oil, and Procter & Gamble were lent to Teradata to road test and provide design input. These companies shaped the product before they bought it. This is a customer development model that Silicon Valley would rediscover 30 years later and call "co-creation."


**4. Walmart's Teradata database grew 30 million times in 30 years.**
In 1992, Walmart's Teradata system stored 1 terabyte. Today, the Walmart data warehouse runs at 30 petabytes — 30,000 terabytes, or 30 million gigabytes. If you had started filling that 1992 system one terabyte at a time, adding a terabyte a day, you would need 82 years to fill the current warehouse.


**5. Teradata stock hit its all-time high the same year Snowflake was founded.**
The exact year — 2012 — that Teradata peaked as a public market story was the same year Benoît Dageville and Thierry Cruanes founded Snowflake in San Mateo, California. Teradata didn't see Snowflake coming. The market did. It was already pricing in the replacement.


---
