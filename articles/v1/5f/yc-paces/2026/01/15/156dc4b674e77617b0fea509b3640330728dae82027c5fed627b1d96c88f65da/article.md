---
schema_version: "1.0.0"
document_id: "156dc4b674e77617b0fea509b3640330728dae82027c5fed627b1d96c88f65da"
company_key: "yc-paces"
company: "Paces"
source_id: "yc-paces-news-import-cb0c795e26d8"
canonical_url: "https://www.paces.com/post/the-one-person-billion-dollar-power-development-company"
published_at: "2026-01-28T00:00:00+00:00"
first_seen_at: "2026-07-25T18:30:06.754267+00:00"
fetched_at: "2026-07-28T22:23:03.686122+00:00"
content_hash: "sha256:6f2d01b7b4a1535e46469283d2306a0fed5d8a2de346b3e82f7e35811ce9a315"
---

# The one-person, billion-dollar power development company

Foreword:


[I wrote the original version of this essay in September 2025](https://www.jamesmcwalter.com/post/the-one-person-billion-dollar-power-development-company) . I have been amazed by the response from developers, utilities, and VCs who are seeing these principles being applied to their projects and workflows. Even more striking is the speed at which the thesis play out in real time. Within the Paces ecosystem, we’ve watched teams use our platform to submit for interconnection in less than 48 hours after site control; compressing months of work to days.


This speed matters more than ever given what’s happening in the market.


On the demand side, the AI buildout remains massively power-constrained. Global electricity consumption for data centres will roughly double to ~945 TWh by[2030](https://www.iea.org/reports/energy-and-ai/energy-demand-from-ai?utm_source=chatgpt.com) . Grid operators are already reacting; PJM just[unveiled](https://www.reuters.com/legal/litigation/pjm-unveils-plan-tackle-ai-driven-power-demand-surge-2026-01-16/?utm_source=chatgpt.com) a plan explicitly targeting AI-driven load growth (including requirements for large loads to bring supply or curtail).


On the supply side, you can see an “AI-native infrastructure builder” category forming. Paces continues to lead the category with our ability to automate project development siting, permitting, and power workflows. We have also seen excellent companies like Unlimited Industries (who are automating design and EPC) emerge.


An interesting theme we’re seeing is that multiple large developers have asked us to help re-wire their org with AI. More notable is the intent: not “add copilots,” but redesign the whole development engine to be higher leverage per person, and radically faster cycles. These are very exciting projects as we apply our technology and transform teams to accelerate their development rate.


The question isn’t whether the one-person power development company can exist; Paces works with many. Rather, the race is on for which will reach 9-10 figures in value creation. The forcing function is speed: the teams that can repeatedly get more steel in ground will compound into the largest portfolios.


I would love to receive more feedback on the essay. Where are the hottest companies, and how are people seeing new and novel ways of applying AI and automation? You can read the full, original essay below.


# The one-person, billion-dollar power development company


> “A flywheel of compounding infrastructure buildout to run these increasingly powerful AI systems… data centers that can build other data centers.” *Sam Altman in*[Gentle Singularity](https://blog.samaltman.com/the-gentle-singularity)


‍


It’s 2030. You open your laptop and check on your power company. You see that your Land Management Agent has approved five Letters of Intent (LOIs) that the Siting Agent drafted overnight. You watch in real time as two interconnection filings are submitted, with high-fidelity design attachments, by the Power Agent. There’s a request to approve the deployment of in-field robots to build a power plant, which you push back because your Permitting Agent has flagged that the necessary clearances are not yet in place. At a different site, a drone completes its site circuit and sends data to the Environmental Agent, which adds it to the Phase I environmental report. You go for lunch.


This reality may only be five years away; we are more than halfway there with current AI tools and data platforms. The remaining work is orchestration, audit trails, and a value chain that can accept machine-generated submittals across the board.


## **One-person AI companies**


> “We’re going to see 10-person companies with billion-dollar valuations pretty soon… in my little group chat with my tech CEO friends there’s this betting pool for the first year there is a one-person billion-dollar company, which would’ve been unimaginable without AI.”[Sam Altman](https://x.com/alexisohanian/status/1752753792058294725?s=46&t=zHQCBTtfz3hXdhbfUsgEAg)


‍


Evan Armstrong’s “[The One-Person Billion-Dollar Company](https://every.to/napkin-math/the-one-person-billion-dollar-company) ” kicks off with the above Altman quote and walks through how it might be possible, only to end on a note of doubt. I disagree with Evan. In the 18 months since he wrote that article, we’ve seen extraordinary new software companies leveraging AI to build product quickly and grow revenue faster than ever believed possible. What looked speculative in 2024 now looks like simple throughput math when you replace weeks of handoffs with minutes-long agent loops.


I see the same thing happening in power development. Power development I define as any real estate project where power is an important constraint. This includes everything from data centers and EV charging stations to solar and battery development, to manufacturing, office and homebuilding. The pressure to build this infrastructure is driving the emergence of an industry-specific power development tech stack. This spans the pre-construction “project development” soft-cost phases as well as the construction and asset-management hard-cost phases. Think of a project-development OS that spans:


**Siting → Interconnection → Permitting → Finance → Procurement → Construction → Operations**


Stitch those pieces together with an AI agent and we’re at the beginning of a one-person AI company that actually builds real-world power projects. The impact this would have is profound as it would enable many more development companies, speed up the rate of power developer and enable the US to meet AI deployment and energy targets. Specifically, I am stating we will see by 2030:


A one-person company that reliably moves **500 MW/year** of project pipeline and generates **~$100M** of development fees at **$0.20/W** commanding a **~$1B** valuation.


At[Paces](https://www.paces.com/) , we’re already working with a couple of people attempting to do just this.


## **Why it has not happened already**


Power development is hard. Really, really hard. At every step, there are bottlenecks, opaque decisions, and uncertainty. The work is procedural but interdependent; fragmentation and variance kill throughput.


1. Your life is three steps forward, two steps back every single day for 2–5 years. You look for sites, leveraging GIS point solutions. You hand them back and forth to your land agents to sign up landowners. Why does this take months, you ask? There is always just one more thing to check, and without a single system of record, teams re-learn the same constraints in every jurisdiction. Your desktop work is only as good as your data, it’s getting harder, not easier to obtain reliable sources as seen by recent deletions of federal power data.
2. Then you need to understand the power: can you get power from or onto the grid? You hire a consultant who claims some special sauce on grid modeling as they use[PSSE](https://www.siemens.com/global/en/products/energy/grid-software/planning/pss-software/pss-e.html) /[Tara](https://power-gem.co/software/tara-software/) like everyone else. They take a couple of months to send you a report. It looks good. Now let’s submit for grid interconnection. Now you need to understand the requirements, put together the single-line drawings and other information, and pay a big deposit.
3. Then on the site itself: are there endangered species on it? What about soil and wetlands? Also, local permitting: every county interprets rules differently. Each project in each new jurisdiction is a nightmare of permitting and environmental review. Small delays stack into months.
4. For finance, you lean on your Excel gurus, iterate on pro formas with dozens of variable inputs, keep it all up to date, and track **Battery_Project_Atlanta_realversion2.34.xls** ..


Pull it all together and the result is sequential and low outcome derisking, minimal transparency, and ever-slower projects.


## **Why now?**


Suddenly, multiple curves bend positive at once on both the demand and supply side. Demand, deregulation, software, and hardware are compounding at the same time; a once-in-a-100-year alignment.


- **Demand:** AI load, electrification, and re-shoring shifted growth from linear to step-wise; development backlog is now a macro constraint.
- **Rules changes:**[FERC Order 2023](https://www.ferc.gov/explainer-interconnection-final-rule?utm_source=chatgpt.com) and related regulations enforced a basic level of streamlining that opened up the market to smaller companies.
- **Supply (software):** We finally have AI agents with the capability and fidelity that can match or exceed human work. At Paces, we have automated multiple roles, including Power Engineering, Permitting Analysts, GIS Analysts, and Environmental Consultants, end-to-end. Point-solution SaaS tools like Enverus did a solid job but still need large teams to operate; now a set of agents can operate internal and external tools at scale. Our internal benchmarks show 1,000x speed and 130x cost reductions for better than human accuracy.
- **Supply (hardware):** Robotics & power components has never been more exciting. On the component side, companies like[Heron Power](https://www.heronpower.com/) produce solid-state transformers: compact, fast-responding equipment that could make grid interconnection faster to deliver. Robotics includes[Planted Solar](https://www.plantedsolar.com/) , actively building a MW scale solar farm for data center and distributed generation customers. Others like[Terabase](https://www.terabase.energy/) ’s Terafab have installed similar-sized projects recently. Standardization at the interconnect node plus power-electronics-heavy kits shrinks footprint and schedule risk.


This all adds up to a unique time where a single person or small team can achieve an amount of power development that previously needed hundreds of people.


## **The building blocks**


The way to build this is to be obsessed, frankly, to a weird degree, with the minutiae of every single step a power development team does. A major mistake in building agentic AI is trying to get the model to do all the work. Rather, you need to map things down to the second, down to the click. Once you do that, you start to see that things that seemed impossible to automate are fairly straightforward.


You need three things:


1. **Data:** A system of record that includes power, permitting, etc.
2. **Tools:** Use case-specific software and robotics/hardware that leverage the underlying data.
3. **Orchestration:** A series of agents that use those tools and executes, with human-in-the-loop escalation as needed.


For this to scale to a level a single person can manage, there also needs to be:


- Continual cross-referencing of compliance. These are real steel-in-ground projects that require rigorous approvals, so every filing and report is generated with line-referenced citations. Every claim in a filing must trace to the right statute or tariff, with submittals reproducible from code.
- Appropriate handoffs and APIs between the various systems. Agent outputs align to utility/AHJ-required formats and be obsessive with interoperability.
- For robotics, there is a real safety & cybersecurity concern. There needs to be things like geofenced robots with certified stop conditions and least-privilege plant controls.


With that backbone, a single person can set the parameters, approve exceptions, and sign off as needed. Everything else runs autonomously. Specifically, the emerging workflow will look like:


### **1) Market selection & capital allocation**


Decide the type of development company you want to start: powered land for Texas data centers? Transmission buildout for utilities? Battery projects in cities? Once you have a market thesis, raise the capital to deploy. AI is great at helping research markets, but the capital raise is down to you. You can definitely write out your thesis and then let agents run over inputs like load growth, tariffs, interconnection status, and land use to score markets. I really want to emphasize though, that as you remove friction somewhere, it will naturally arise elsewhere. In development, this means that as friction drops, the premium shifts to capital structure, personal relationships with counterparties, and trust.


### **2) Siting**


Desktop analysis is already available in systems like Paces and its Geospatial AI search engine, which collapses all the sites in a state into a scored short list in minutes (soils, floodplain, setbacks, ownership graph, substation headroom). The remaining pain is title and outreach, with the best land agents being world-class salespeople. But 95% of that work is still on desktop and phone… and I’ve seen a pretty good internal AI phone demo that cold-called and booked a meeting for a real-estate deal…


### **3) Permitting**


Early attempts for power permitting included tools like[SolarAPP+](https://www.gosolarapp.org/) , which issues residential permits. Large-scale power projects are a lot more difficult with permitting needing dozens of documents and thousands of pages. Despite this, the principle stands: by automating standardized checks and packages with citations, you can massively reduce the time it takes to permit a project. Paces’ LLM-based permitting tools do this for tens of thousands of jurisdictions today. One remaining area that will be hard to speed up is the need for local lobbying and personal relationships at the community level, back to where friction will lie with relationships and handshakes.


### **4) Site visits**


This is a bit speculative but a combination of drones and new <$40k robots means that fully robotic site visits for power projects will be not only possible, but cheaper and more reliable. Drones equipped with LiDAR, thermal, and multispectral cameras can map hundreds of acres in under an hour, while quadruped robots handle on-the-ground inspections, VOC sensing, and utility locating with ground-penetrating radar. Autonomous sensor pods can be dropped to monitor noise, air, and dust over time, and all data is streamed back to centralized environmental professionals who review and sign off. Compared to the current model where crews of specialists travel for days at a time at a cost of +$100k per site, robotics can slash field costs. Geotechnical drilling will still require human intervention; I would love to see some startups work on this.


### **5) Interconnection**


Many tools try to better model the grid before and after. We are seeing the rise of software tools generating “heatmaps” that show out of date capacity and give more noise than signal. The key is to build agents that operate the existing tools the utility already uses to do full studies. This is the only way to move fast. With clusters and readiness screens in place, you win by submitting serious studies, being surgically honest about network impacts, and presenting a plan to reduce them. Do not fight the utility’s approach; screen for capacity and potential upgrades early and present mitigation options alongside impacts.


Paces 5-day Power Flow Studies


### **6) Finance**


There are a lot of good startups working on this, though it's not that hard technically. At Paces, we left this until recently to automate, as it’s so straightforward. Most of the value in the finance side is not the data room or pro-forma construction but getting the actual inputs. This comes from deep site-specific information as well as by linking to various marketplaces or buyers and sellers e.g.[LevelTen Energy](https://www.leveltenenergy.com/) and its PPA prices. Crux has also streamlined the transferability of[IRA credits](https://www.irs.gov/irb/2025-36_IRB) .


### **7) Procurement**


Overly complex supply chains, procurement datasets and the lack of a simple buy button on most hardware websites is a long-running problem. Many hardware founder friends have spent months just trying to price up their MVP. Happily, we are seeing the emergence of some vertical AI companies like[Parter](https://parter.ai/) . These and others will help the engagement and ordering of suppliers into a query with price, spec, delivery windows, and domestic content in one view. Further, as part of using robotics, we will see standardization and pre-assembly which will make project level procurement decisions easier.


The nightmare of procurement requests


‍


### **8) Construction**


I love seeing the robots my founder friends have built to deploy power infrastructure. Companies like[Planted](https://www.plantedsolar.com/our-platform) are doing an incredible job automating the construction of solar farms. There are literally not enough trained workers in the US to deploy all the power infrastructure we need. Labor scarcity is structural; the durable answer is mechanization plus better planning. Speed at construction comes from robotics that can work across many sites.


Planted solar panel robot


### **9) Operations**


Really impressed by companies like[Zeitview](https://www.zeitview.com/) , which use remote imagery & AI to visualize and understand the condition of deployed assets.


None of this implies that fully autonomous, end-to-end development and construction will happen tomorrow. But the blend we have today is enough to cut literal years and millions from every project. And, I am (of course) biased, but Paces is, and will continue to be, the orchestration layer to enable this reality.


## **Friction we must battle**


The entire above essay I wrote on a flight to Vegas for a conference. I then sat with it for a couple of days and the realization struck me that this vision is inevitable from a technological perspective. But there *are* major risks from the understandable concern of job losses. The demand to put human driving “observers” into Waymos is a warning sign; we need to advocate and be vigilant in our industry so that regulation does not prevent this vision. Some areas to work on:


- **Governance lag:** The tech will be ready before the governance is so we need to plan for that gap. Pilot with agencies to raise understanding early and often **.**
- **Regulatory acceptance:** In both interconnection and permitting, a human will sign off on the final approval. To leverage automation, we need to work within the status quo while pushing for adoption of tools that are as accurate as humans but many multiples faster. This means humans in the loop on the client side.
- **Labor:** Concerns over job losses are understandable but in our industry,[we do not have enough people to develop and deploy the power assets](https://www.energy.gov/sites/default/files/2025-07/EWAB_Special_Report_Opportunities_for_Amercian_Workers_in_Energy.pdf?utm_source=chatgpt.com) needed. We need to restate the threat to local and state economic targets if we do not deploy these projects and how total economic and job growth is dependent on cheap, ubiquitous power.
- **Community:** Stakeholder mapping and early outreach get automated, but consent remains human. Publish plain-language summaries alongside filings. And always use the word “automation” not AI; there continues to be nerves about “AI”.


## **It's inevitable**


The data, tools, and orchestration layers needed for the one-person power developer are mostly in place, we are deploying this at Paces now. What is needed now is continued fast iteration on the remaining gaps and preventing regulatory hurdles that block essential assets from being built. I also would love to see capital to get more comfortable with tech-forward project development models. Paces is already working with the first of these new types of firms. I fully expect to see hundreds emerge over the next couple of years.


*(Thanks to Duncan Campbell, Eric Brown, Kyle Baranko, Shanu Mathew and many others for their thoughts and earlier comments on this article.)*


‍


‍
