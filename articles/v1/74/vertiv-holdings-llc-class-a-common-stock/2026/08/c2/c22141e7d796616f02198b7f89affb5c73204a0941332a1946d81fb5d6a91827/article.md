---
schema_version: "1.0.0"
document_id: "c22141e7d796616f02198b7f89affb5c73204a0941332a1946d81fb5d6a91827"
company_key: "vertiv-holdings-llc-class-a-common-stock"
company: "Vertiv Holdings LLC"
source_id: "vertiv-holdings-llc-class-a-common-stock-news-import-f0997059a52b"
canonical_url: "https://www.vertiv.com/en-us/insights/articles/educational-articles/vertiv-cooling-innovation-day-2026-lessons-from-a-live-colocation-deployment/"
published_at: null
first_seen_at: "2026-08-03T22:57:26.735359+00:00"
fetched_at: "2026-08-04T00:20:40.433623+00:00"
content_hash: "sha256:b13e7cee3a8fe47ff366e9f92ea3b1dd4e676ffc9b3f24871992b966b4950d00"
---

# Vertiv Cooling Innovation Day 2026: Lessons from a live colocation deployment

Retrofitting a live colocation facility for AI means working around real constraints: tight timelines, active customers, and hardware specifications that shift mid-project. In the third episode of[Vertiv Cooling Innovation Day 2026](https://www.vertiv.com/en-us/about/news-and-events/events/2026/vertiv-cooling-innovation-day-2026/) ,[Steve Madara](https://www.datacenterdynamics.com/en/profile/steve-madara/) , Distinguished Engineer and VP of Thermal Architecture & Application at Vertiv, and[Tom DeJonge](https://www.datacenterdynamics.com/en/profile/tom-dejonge/) , VP of Facility Engineering at US Signal, join[Zoe Turner](https://www.datacenterdynamics.com/en/profile/zoe-turner/) of DatacenterDynamics (DCD) to discuss what that looks like in practice. They cover the move to liquid cooling, modular[coolant distribution units (CDUs)](https://www.vertiv.com/en-us/insights/articles/educational-articles/understanding-coolant-distribution-units-cdus-for-liquid-cooling/) , real-world deployments, and AI readiness for regional colocation facilities.


#### Zoe Turner, Channel Manager at DCD: Can you describe the facility? What state was it like when US Signal acquired it?


##### Tom DeJonge, VP of Facility Engineering at US Signal:


Around August 2024, we closed the acquisition. It was a purpose-built enterprise data center, renovated a couple of times, running at 5 kW per cabinet with perimeter cooling and a 24-inch raised floor. It was about 6 megawatts (MW) of utility, 4 MW of[uninterruptible power supplies (UPSs)](https://www.vertiv.com/en-us/about/news-and-events/articles/educational-articles/what-is-a-ups/) , with Vertiv[computer room air conditioner (CRAC)](https://www.vertiv.com/en-us/insights/articles/educational-articles/what-is-dx-cooling--direct-expansion-cooling-system/) units throughout. The original customer stayed on, and we had auxiliary rooms for new customers.


#### Zoe: What drove the decision to move toward liquid cooling for the new customer requirements?


##### Tom:


The big problem: we had no chilled water and no capacity to support rear-door heat exchangers or direct-to-chip cooling. We went to Vertiv, asked what our quickest path would be, and they came back with the[Vertiv™ CoolPhase CDU 300](https://www.vertiv.com/en-us/products-catalog/thermal-management/high-density-solutions/vertiv-coolphase-cdu/) . That gave us the ability to quickly implement liquid cooling with a modular approach. The customer’s requirements were 415V, about 30 kW per cabinet, with a next phase heading into a direct-to-chip application.


##### Steve Madara, Distinguished Engineer and VP of Thermal Architecture & Application at Vertiv:


The Vertiv™ CoolPhase CDU 300 was there to give Tom the additional fluid capacity for the[rear door heat exchangers (RDHxs)](https://www.vertiv.com/en-us/insights/articles/educational-articles/how-rear-door-heat-exchangers-rdhx-support-high-density-rack-cooling/) , and room to expand as customers come in with higher density racks. When you go to 100 kW and beyond, those racks are going to need to be liquid cooled. The challenge is: how do you build those solutions into a live data center, and what’s the right technology to make those moves?


#### Zoe: Is AI inferencing driving new demand into regional colocation facilities like yours?


##### Tom:


Absolutely. Right now I’m probably getting two or three calls a week: do you have 2 MW available, how quickly can you support 30 to 40 kW cabinets, even 60 kW air-cooled with the ability to transition to direct liquid cooling? Our 17 data centers are all very traditional, around 10 to 15 kW designs. The big question is how we transition these builds to support GPU and AI applications, and how do you prepare sites to move from a 5-kW customer to 30 or 60 kW for the future?


##### Steve:


When GPU manufacturers and others started talking about AI, the big press was around gigawatt training facilities. But it’s inferencing that generates revenue for businesses. We’re going to see more and more AI applications at the enterprise level: on-prem, at a local or regional colocation facility, or in the cloud. It’s going to be very diverse.


#### Zoe Turner: How did the Vertiv™ CoolPhase CDU perform in practice, and what flexibility did it give you?


##### Tom:


We installed seven units but are only running two or three as the customer adds load, so we have a lot of room to grow. It solved our two big requirements: future growth within the space and a tight timeline, getting the customer up and running within about six months. With the economizer, it is a very efficient unit. We’re probably down to a power usage effectiveness (PUE) of 1.2 in the winter and maybe 1.4 in the summer.


The key is flexibility: the[Vertiv™ CoolPhase CDU](https://www.youtube.com/watch?v=7pEZgFUdXmU) can feed RDHxs for air-cooled racks, or a[Vertiv™ CoolChip CDU](https://www.vertiv.com/en-us/products-catalog/thermal-management/high-density-solutions/vertiv-coolchip-cdu/) if we need to go direct to chip. Everybody I talk to says they’re going to deploy higher-density racks, but they don’t always know what the load looks like.


##### Steve:


Designing from the chip out: that’s the goal. But a lot of times customers don’t know what the chip or the usage is going to be, so you end up designing from the outside in. The Vertiv™ CoolPhase CDU gives you that modularity: if customers don’t have their cooling requirements defined yet, you can build into that space over time. We’re talking differences of 10 to 20 kW as you go — you need to adjust to what the customer actually rolls in.


#### Zoe: How did you approach deploying the solution in a live environment where uptime was critical?


##### Tom:


We were fairly fortunate. It was a 2N infrastructure, so we could shut down pieces of the UPS based on how they were built out. All the new cooling was brand new, so we left the traditional perimeter cooling in the main data hall alone and focused on the new build-to-suit suite. It came down to a great partnership; we worked through the cut-overs, and so far we have not had any outages.


##### Steve:


If you’re deploying higher density racks that are still air-cooled, it’s more straightforward: you’re introducing liquid loops only at the row level. The harder part is adding liquid to the chip or secondary fluid loops, because that means a lot of additional piping and construction.


The biggest thing we’ve seen: modular solutions, including modular piping systems and smart runs for hot aisle containment, take a lot of construction activity off the site. In some cases, if the solution is very different from what you have, it may be better to build a pod adjacent to your existing building.


#### Zoe: How important was it to have a single integrated approach across power, cooling, and infrastructure?


##### Steve:


Speed of deployment is important, and the fewer partners and pieces that have to interconnect, the fewer delays. Teams who repeat the same designs, the same supply base, and the same startup teams learn from that. There’s not a new learning curve every time, and that contributes to speed.


##### Tom:


We were able to go to a Vertiv partner in Detroit. They had the full portfolio to support this project from top to bottom: electrical, mechanical, all aspects of it. I didn’t have to go to three or four different vendors. That advisory role helped us design and implement a quick solution that was also the right solution for the customer.


#### Zoe: What were the biggest lessons from this deployment?


##### Tom:


Time. Everything is speed to market, but taking the time upfront to design well and remove assumptions leads to a faster deployment. On the flip side, you need the flexibility to quickly pivot, because the market is changing fast. You may think you’re designing to A and it’s going to go to C before you know it. Having the right partners to work through those changes is what’s going to be most valuable.


##### Steve:


This market of AI inferencing, which we’re calling the second wave, is going to impact the enterprise and small colocation market. There’s going to be a lot of different applications, from single racks to 10 or 20 racks, and it’s all around flexibility.


The Vertiv™ CoolPhase CDU gives you that modularity: build as you need, add on as you need, rather than putting in one large block of infrastructure and guessing at the right size. The advantage we have at Vertiv is our direct connection with most server and chip manufacturers, understanding what’s coming so we can build the right solutions. It’s our responsibility to work with partners like US Signal, understand their real needs, and find the right solution to support them.


### Watch the full discussion:[Lessons from a live colocation deployment](https://www.vertiv.com/en-us/about/news-and-events/events/2026/vertiv-cooling-innovation-day-2026/)
