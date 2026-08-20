---
schema_version: "1.0.0"
document_id: "2ba4770740edb9ce7f08de9c1fe0d488ae07fe4b34041a7893f7ea266bf43ff5"
company_key: "yc-rune"
company: "Rune"
source_id: "yc-rune-news-import-dca8148f0685"
canonical_url: "https://www.runetech.co/news/adk-launch"
published_at: null
first_seen_at: "2026-07-23T23:32:11.319893+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:a24e8c9cb6e3f183e3cd28b57af2be4b04f3e7bbbfcd44bd9143409f07f13ae4"
---

# The Joint Force is fielding the platforms. Rune is building the platform that orchestrates them.

In a Pacific archipelago at first light, a small unmanned surface vessel idles a few hundred meters offshore. Inland, a ground robot waits at a logistics release point with a pallet of small arms ammunition. Two Group 3 drones loiter overhead. Each platform is running its own autonomy stack, its own mission software, its own control plane. They are all in the same fight. None of them are aware of the others.


This is the part of the autonomy story we don't talk about enough. The Department of War has poured years of investment into Replicator and into purpose-built unmanned air, surface, subsurface, and ground platforms. The wave is real. Thousands of autonomous systems are fielded today, with thousands more on contract. What we don't have is anything to make them work as a coordinated logistics fleet. When a commander asks how an unmanned vessel could pick up cargo from a forward node and hand it off to an aerial drone, the answer today involves a whiteboard, four different chat threads, and a logistician staying up past midnight trying to reconcile fuel readings across systems that were never built to compare them.


We started Rune to fix that.


**The Hard Work Only Starts With The Drone**


The companies that have built our autonomous platforms have done extraordinary work. They've produced surface vessels capable of running mobile logistics hubs for aerial drones, ground systems that follow lead vehicles through complex terrain, and unmanned aircraft that evacuate casualties.


What none of them have built, and none of them should have to build, is a predictive sustainment orchestration layer that spans every domain. Today every platform handles its own dispatch. The vendor runs its own console. The operator works from a slice of the picture. When the time comes to coordinate a multi-modal, multi-nodal resupply across surface, air, and ground, there is no shared scheduler. The fleet exists. Today, the fleet doesn't fly in formation without a human and a whiteboard.


**TyrOS Already Schedules Anything That Moves**


We built[TyrOS](https://www.runetech.co/tyros) as the operating system for military logistics. It fuses supply and transportation data, real-time geospatial information, maintenance state, and operational intelligence into a living sustainment picture. It's fielded today across U.S. Army and Marine Corps formations. It runs at the edge and in the cloud, in denied and degraded conditions, where logistics decisions actually get made.


One detail about TyrOS shaped what we built next. TyrOS is agnostic to whether a vehicle has a crew. The scheduling and optimization logic that figures out which asset to allocate is built on the same building blocks for manned and unmanned platforms. If anything, removing the constraint of matching crew availability to asset availability makes the math simpler, because there's one fewer constraint to satisfy. The software was always ready to orchestrate autonomous systems. What we needed was a standardized way for those systems to participate.


**Introducing the TyrOS Autonomy Development Kit**


Today we're announcing the TyrOS Autonomy Development Kit (ADK). The ADK is the way any autonomous vehicle manufacturer integrates with TyrOS.


Mechanically, the ADK is a well-defined set of Application Programming Interfaces (APIs), documentation, sample code, and optional software libraries. A vendor implements the ADK APIs to register an asset type and its characteristics (cargo capacity, patient capacity, cargo class), publish telemetry (fuel, battery, position-location information, faults), and receive tasking from TyrOS. Once that work is done, the vehicle becomes schedulable inside the TyrOS Task Catalog. A logistician using TyrOS can then plan a mission using the new platform the same way they can with their manned platforms.


That's the whole thing. We deliberately scoped the ADK so an engineering team can pick it up in days, not quarters. The complexity of cross-domain scheduling lives inside TyrOS, where it belongs. The vendor side is light. We did the hard part so they don't have to.


**The Logistics Network Effect**


The Army’s Operation Jailbreak is putting this principle into practice right now, breaking open walled gardens and pushing the entire defense industrial base towards an open systems architecture. The right to integrate is a right we live by. TyrOS was built with open APIs and an open architecture from day one. And as new autonomous platforms get fielded, modified, or updated, the orchestration layer needs to receive those changes and update its scheduling in real time. That’s what ADK is built to do.


This is where the story gets interesting for two specific groups.


The first is the program offices driving autonomy work inside the services. DIU, the COCOMs, and the service-level autonomy programs are writing the next round of requirements. If you're standing up an autonomy program, you need a vehicle and you need software to coordinate it. The vehicle market is filling out fast. The orchestration market is one product wide. TyrOS is the software side of the answer for any logistics autonomy program a service runs.


The second is the autonomous vehicle companies themselves. For the last two years, every customer we talked to asked us the same question: are you integrated with Maven? We answered yes, and the conversation moved forward. The inverse should be true for the autonomy side of the field. If a company builds autonomous platforms and wants to be real about military logistics, it should be integrated with TyrOS, because customers will expect it. The ADK is how a vendor gets there.


This is not hypothetical. Vendors marketing their Maven integration on their product pages is a category we now take for granted. Within a year, the same thing happened with Model Context Protocol (MCP) for agentic AI. The pattern is consistent. When a platform becomes the way work gets coordinated, the companies that plug into it earlier come out ahead. We expect the same pattern to play out for TyrOS in logistics autonomy.


**Available to Exclusive Partners**


Today, the ADK is available to our autonomy partners across the ecosystem, as well as our integration partners on NGC2 and Project Dynamis. If you're an autonomy company that wants to integrate, or a program office that wants to understand what your prospective vendors are getting access to, reach out. We'll get you the documentation and an engineer to walk you through it.


The platforms are fielded. TyrOS is fielded. The ADK is in the market. The work ahead is bringing more of the autonomous fleet under one orchestration layer, so that the next time a commander asks for a multi-modal resupply across a contested archipelago, the answer takes seconds instead of a long night.


The Joint Force is fielding the platforms. We're building the platform that orchestrates them. If you're building autonomy, build with TyrOS.
