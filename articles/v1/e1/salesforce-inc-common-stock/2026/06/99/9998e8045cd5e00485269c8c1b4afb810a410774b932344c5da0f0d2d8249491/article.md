---
schema_version: "1.0.0"
document_id: "9998e8045cd5e00485269c8c1b4afb810a410774b932344c5da0f0d2d8249491"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-news-import-e12c13bbd360"
canonical_url: "https://www.salesforce.com/blog/sales/salesforce-spiff-migration/"
published_at: "2026-06-11T13:57:16+00:00"
first_seen_at: "2026-07-22T12:38:45.747221+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:160ebda34bd3bff77cf639b716c4239dcb6d95624d11dc1f20202730b94a0e31"
---

# How Salesforce Migrated 30,000+ Sellers to Spiff, a Single Comp Management Tool

Any sales leader will tell you:[Sales compensation](https://www.salesforce.com/sales/incentive-compensation-management/sales-compensation-plans/) is one of the most sensitive parts of a business’s operations. Get it right, and sellers are motivated, focused, and confident. Get it wrong — or even leave room for doubt — and you quickly erode sellers’ trust.


Our recent[Trends in Sales Compensation report](https://www.salesforce.com/sales/resources/sales-compensation-trends-report/) spotlighted some of the current challenges that contribute to that erosion: 74% of sales reps say they wish there were more transparency in how their compensation is calculated, and 47% say their compensation doesn’t always seem fair. As Madeleine Gill, Senior Director of Product Management for[Spiff](https://www.salesforce.com/sales/incentive-compensation-management/) , puts it: “Reps live and die by their number. The minute they don’t have full visibility into that, a lot of trust is lost.”


We knew this firsthand at Salesforce. With 30,000+ commissionable employees, 120 unique[sales compensation plans](https://www.salesforce.com/sales/incentive-compensation-management/sales-compensation-plans/) , and 3.5 billion commission-based data points, our comp system had become one of the most complex in the world. Our legacy tools were struggling to keep up, and sellers were frustrated with the lack of visibility into how their compensation was calculated and why.


So we made a change. We migrated every single seller and all of our comp data to a single platform: Spiff. Here’s what it actually took, what we learned, and what it means for any enterprise considering a similar move.


**>>>[HEAR Salesforce CRO Adam Alfano, SVP Manish Menon, and other sales leaders share the Salesforce migration journey](https://www.salesforce.com/events/webinars/how-salesforce-scales-seller-transparency-webinar/)**


## How we knew our comp system couldn’t keep up


For more than 17 years, Salesforce managed sales compensation with third-party software. And for most that time, it worked. But as our company grew and our go-to-market motions grew more complex, the cracks started to show.


Our compensation data became fragmented across disconnected systems, requiring multiple error-prone integrations and significant manual intervention just to keep data current. And even then, sellers had no way to see how comp numbers were calculated or verify that they were right.


The impact on sellers was significant. With compensation data scattered across so many systems and processed manually at every step, errors were inevitable, and when something went wrong, it took weeks to resolve. Sellers who couldn’t get a straight answer about their commission payments stopped trusting the numbers entirely. There was a growing flood of support tickets and manual recalculations that put real pressure on the ops team and eroded seller confidence year over year. Something had to change.


> Salesforce has grown rapidly over the last 25 years. And as the company has grown, the systems have not kept pace. We needed to simplify the data flow, reduce manual processes, and drive automation.


Manish Menon, SVP, Global Incentive Compensation, Salesforce


-
-
-
-


## Finding a new solution


When we went to market to evaluate options for[incentive compensation management software](https://www.salesforce.com/sales/incentive-compensation-management/) , we weren’t just looking for a faster tool. We were looking for a platform that could grow with us. We evaluated several options, and Spiff stood out for a few key reasons.


First, connectivity. As a native Salesforce product, Spiff integrates directly with the ecosystem where our data already lives, the Customer 360 platform. For Chris Brooks, Senior Manager of Global Incentive Compensation Systems at Salesforce, that was the deciding factor: “Where Spiff really stood out was seamless connectivity with Salesforce.”


Second, visibility. The selling point for Spiff wasn’t just the integrations. It also offered better traceability and explainability across payees, reporting, and auditing needs. In other words, it directly addressed the transparency concerns our sellers had for years.


Third, the platform gave us real flexibility. Spiff’s intuitive, Excel-like configuration meant our team could build and modify comp plans without being fully dependent on a vendor for every change. And with a direct[Agentforce Revenue Management](https://www.salesforce.com/sales/revenue-lifecycle-management/) integration that could be stood up in 24 hours, sellers could gain visibility into their estimated commissions while building quotes, giving them the insight they need to prioritize the right deals at the right time.


## Step-by-step migration process


Before we get into the “how” of our migration, it’s worth noting that moving comp management to Spiff wasn’t just a technical lift. It was an opportunity to rethink the way we’d been doing things for two decades, to simplify processes, consolidate plans, and eliminate workarounds that had calcified over the years. That honest reckoning shaped every decision we made along the way.


Here’s how we approached it:


### **Step 1: Clean the data.**


Before a single seller could be onboarded to Spiff, we had to audit the data flowing through our source systems and ensure everything was accurate, structured, and ready to be ingested. Data hygiene isn’t glamorous, but it’s foundational. The questions Chris Brooks recommends every team ask before starting: “What data do you need? What do you not need? Where does it live? And is it represented in a clean and structured way?”


### **Step 2: Assemble the right teams early.**


To prepare and roll out Spiff, we assembled a large cross-functional team before any migration work began. The Incentive Compensation team owned plan design, configuration, and testing. Digital Technology managed the data infrastructure and integrations across our full pipeline. The Spiff product team co-developed the enterprise capabilities the implementation required, and even stepped in to help with testing when the product hit capacity constraints. Program Management kept the entire effort coordinated, and Accounting was critical in validating data integrity and building the SOX controls needed at go-live. Cross-functional alignment wasn’t a nice-to-have. It was a requirement.


### **Step 3: Establish executive steering from day one.**


A migration of this complexity needs air cover at the top, and that means engaging leaders who can remove blockers and make decisions quickly. For us, that meant leading regular steering meetings with EVPs and SVPs across sales, product, and ops so we could guide them and their teams throughout the entire implementation. These weren’t status updates. They were working sessions designed to keep the program moving when challenges arose. The cross-functional team handled the day-to-day work while executive sponsors ensured nothing got stuck.


### **Step 4: Build and test end-to-end, not just within Spiff.**


We ran five phases of testing across the full data journey: from our Salesforce instance where deal data originates, through Snowflake where it’s aggregated, into Spiff where calculations happen, and finally to Workday where payouts are executed, validating accuracy at every handoff. Throughout, Salesforce engineers and the Spiff product team worked in close partnership to build the capabilities an enterprise deployment required: performance improvements for calculations at unprecedented volume, automation for plan management and prior-period processing, and rigorous SOX controls. These were core requirements the two teams worked through together, and many are now available to Spiff’s broader enterprise customer base.


### **Step 5: Begin rollout with a small cohort.**


Rather than attempting a big-bang migration, we went live with approximately 6,000 solution engineers first. This gave us a real-world testing environment at a meaningful scale. We learned what worked, what needed adjustment, and what change management looked like in practice — then applied those learnings before expanding to the full organization of 30,000+ commissionable employees.


### **Step 6: Bring sellers along.**


Change management for 30k people is its own project, and the lessons from our initial cohort shaped how we approached it. We launched a “Spiff Insiders Network,” a group of sellers across different business segments who helped us think through training, surface questions early, and build peer credibility for the rollout. We also established a top-down sponsorship model, with sales leadership actively messaging the change. A dedicated Q&A Slack channel that included the Product team, along with a combination of async and live training, helped ensure no one was left behind.


**The lesson no one tells you:** Plan for what happens after the data leaves Spiff. We designed our processes carefully all the way through go-live, and then realized we needed to build new SOX controls to validate that data flowing from Spiff into our accounting systems was accurate. Think end-to-end — from every data source going in, to every downstream system it feeds.


> A lot of the performance-related work happened behind the scenes. We needed to ensure that compliance elements were fully automated to avoid any possible risk to the business, and to set a new standard for our broader customer base as well.


Madeleine Gill, Senior Director of Product Management, Spiff


-
-
-
-


## What Spiff delivers: The migration results


The migration is now complete, and the early results speak for themselves. Processing that once took days now takes hours, with at least a 75% improvement in comp calculation times and all crediting and payouts running in three to four hours. We’ve also been able to incorporate aspects of our comp design, like NNAOV (Net New Annual Order Value), to add more value to Spiff. And perhaps most importantly, sellers now have full visibility into how their compensation is calculated, line by line, in real time.


For other enterprises evaluating a comp transformation, the proof is in the scale: 120 unique compensation plans, 3.5 billion commission-based data points, and 30,000 sellers successfully migrated. Salesforce is Spiff’s largest and most complex customer, and the question of whether the platform can handle enterprise-level complexity has been answered.


Still, when we look back on this journey, what we’re most proud of isn’t the scale of what we accomplished. It’s how the teams came together. Across Incentive Compensation, Digital Technology, Product, Accounting, and Program Management, teams that don’t always work closely operated as one. When problems came up, people leaned in. There was no finger-pointing. Just people solving hard problems together.


And just as important as the hard metrics was the qualitative feedback from our own teams. Here’s some of what they shared post-migration:


*“Spiff changed the game for my teams. When reps have full visibility into their quota progress and pipeline performance, they stop guessing and start executing. That clarity doesn’t just motivate — it drives the right behaviors every single day. We’re not leaving money on the table anymore.” — Stephanie Seville, AVP Sales*


*“Before Spiff, comp was a black box. Reps would spend time questioning their statements instead of focusing on their next deal. Now they can see exactly what they’ve earned and why — and that trust changes everything. A rep who believes in their pay plan sells differently.” — Ryan Fox, AVP Sales*


*“We have some of the most complex comp plans on the planet and Spiff handles it without breaking a sweat. If it works here, it works anywhere.” — Lauren Larson, RVP Sales*


*“As an AE, I used to spend way too much time trying to untangle comp — spreadsheets, back-and-forth on commission cases, Sunday night math. Spiff gave me that time back. Now I can check my earnings in seconds and get back in front of my customers. That’s a huge win for me, and it’s hours every month.” — Patrick Llewellyn, Cloud AE*


## What’s ahead


As we look six months out and beyond, we’re most energized by what AI will mean for how sellers interact with their compensation data. Soon, sellers will be able to ask natural language questions about their comp and get real answers — not just “why did I get paid X?” but “where should I focus right now to maximize my payout?” Since our[sales incentive plans](https://www.salesforce.com/sales/incentive-compensation-management/sales-incentive-plan/) are designed to incentivize the business outcomes we want, helping sellers maximize their commissions directly aligns with maximizing revenue for the business. AI functionality is also coming to Spiff that will support case resolution, reducing the manual work that consumes comp and ops teams today.


Beyond AI, commission data will soon be available wherever sellers work — in Slack, in Lightning, or any surface — via MCP and a headless approach aligned with Salesforce’s broader platform strategy. And deeper integrations with Data 360, Tableau, and Agentforce will make the compensation lifecycle more seamless than it’s ever been.


Ready to see what Spiff can do for your team? Check out the demo below.


**>>>[For more on Salesforce Spiff, check out our product page](https://www.salesforce.com/sales/incentive-compensation-management/) .**
