---
schema_version: "1.0.0"
document_id: "bfb18799b6078cc6a4106a4580e4c0f2305a78d04498cbfecf1640a30f2499e1"
company_key: "yc-skyways"
company: "Skyways"
source_id: "yc-skyways-news-import-8e093ee2ed4b"
canonical_url: "https://skyways.com/newsroom/skyways-first-bvlos-between-airports/"
published_at: "2025-10-14T21:16:00+00:00"
first_seen_at: "2026-07-24T01:08:05.997267+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:b365a3fbeee233b60bd3978bd89c9e7822a4623aa1a2c106cb6f9e18938d6005"
---

# Milestone Story: What It Takes to Be the 1st to Fly BVLOS Between Two US Airports

## Key Takeaways


- Skyways completed the first unmanned, fully autonomous BVLOS cargo flights between two U.S. airports—Grand Forks Air Force Base and Cavalier Space Force Station.
- Over the course of 12 days, Skyways completed 20 successful operational flights, consisting of 10 round trips with our V2 aircraft through the national airspace, showcasing the repeatability and reliability of the aircraft.
- We demonstrated safe, reliable BVLOS operations at 3,500 feet in controlled airspace — an altitude and scale no other cargo UAV operator has achieved.
- The event also marked the first customer demonstration of our V3 aircraft, flying BVLOS in controlled airspace, including an unplanned emergency landing that the aircraft and Skyways flight test team managed successfully.
- Emergency procedures proved operational maturity: the unplanned landing of V3 demonstrated the aircraft’s fault tolerance and generated critical data for future autonomy and diagnostics.
- Lessons learned highlight the need for automation in ATC coordination, diagnostic clarity, and minimal ground infrastructure for scalable UAV logistics.
- Regulatory evolution remains the biggest hurdle for autonomous cargo flight — not technology.


Andrew Fung was monitoring flight systems on our Skyways V3 aircraft from Grand Forks Air Force Base when the aircraft's performance data diverged from expectations. While climbing to 3,500 feet, operating in controlled airspace alongside commercial traffic and military aircraft—the telemetry looked normal at first. RPMs held steady around 4,000. The aircraft was still moving forward. Which, to be fair, is generally what you want an aircraft to do. But the speed was wrong. And in aviation, "moving but slow" is not a category you want to be in.


One cylinder on the V3 had failed. The other masked the problem by continuing to fire. By the time our team identified the issue, the aircraft had already transitioned to VTOL mode on its own—spooling up the VTOL motors autonomously before anyone touched anything. Which is fortunate, because a pure fixed-wing aircraft in this situation gets exactly one attempt at an unpleasant dead-stick landing. Even hybrid eVTOLs running propulsion off a generator would face the same delightful scenario. Our Separate Lift Cruise configuration? Cruise engine quits, VTOL motors take over. Simple. Fault-tolerant. Allowing our team enough time to take over for what came next, no easy feat in and of itself: a controlled descent from thousands of feet in the air, a vertical landing profile no cargo UAV had attempted. Seven minutes later, it touched down safely with no damage to the aircraft in a North Dakota wheat field with minimal battery reserves remaining, later earning the aircraft the name "Wheat Whacker."


The landing clearly wasn't in the flight plan. But the data it produced and the novel experience it provided would become as valuable as the rest of this successful mission.


## **Solving Problems That Actually Exist**


We build long-range unmanned aircraft systems designed to address logistics challenges that existing options can't solve economically or practically. While much of the UAS industry focuses on urban last-mile delivery—moving small payloads over short distances—we operate at a different scale: up to 100 lbs of cargo and just over 1000 miles in range.


"We're providing a solution to a problem that actually exists," says Andrew Fung, our lead flight test engineer, "not just shoehorning some expensive technology into something that isn't really a societal problem."


Our mission profile reflects this focus. Our aircraft combine vertical takeoff and landing capability with efficient fixed-wing cruise, requiring no prepared landing sites and minimal ground infrastructure at the destination. We operate in controlled airspace, coordinating with air traffic control at altitudes where meaningful logistics problems exist: resupplying offshore platforms, connecting remote military installations, serving island communities without reliable surface transportation.


The aircraft are fully autonomous. Operators set the mission parameters and then simply monitor systems while maintaining the authority to intervene. The autonomy stack and computer vision onboard every aircraft handles navigation, traffic deconfliction, and landing site selection without continuous human input.


## **The Engineer Behind the Mission**


Andrew grew up in Iowa and studied aerospace engineering at Iowa State, where he developed a passion for experimental aircraft—building, testing, and flying new designs from the ground up. After completing his master's degree, he joined us at 24 because we were doing something rare: designing innovative aircrafts and flying real missions at a small, hands-on company where engineers work across multiple aspects of development.


So yes, we're trusting complex autonomous flight operations to someone who's barely allowed to rent a car. But when you're that good, you're that good.


As our lead flight test engineer, Andrew helps manage the entire flight test program—from production testing to experimental development and operator training. He's responsible for ensuring that every engineering change, every system modification, and every new capability is safely integrated and thoroughly tested before operational deployment.


That experience would prove critical in North Dakota.


## **A First-of-Its-Kind Demonstration**


In July 2025, we conducted a twelve-day operation demonstrating Beyond Visual Line of Sight (BVLOS) logistics capability between Grand Forks Air Force Base and Cavalier Space Force Station in North Dakota. The effort, funded through a Department of Defense initiative, represented the first sustained UAS logistics operation of its kind in the United States, and showcased the capabilities of both our V2 and V3 aircraft flying BVLOS in the national airspace.


The mission parameters were straightforward but unprecedented: prove that autonomous aircraft can reliably deliver operationally relevant cargo between remote locations, through controlled airspace, under FAA oversight, with active air traffic control coordination.


Over twelve days, we completed 20 operational flights with our V2 aircraft—10 complete logistics cycles covering more than 60 miles per round trip, with no flight-blocking issues. Our aircraft carried various cargo loads including equipment, supplies, and, on one flight, 23 pounds of Chick-fil-A. Look, we know how it sounds. But that delivery mattered operationally: personnel at Cavalier currently face a three-plus-hour round trip by car for off-base supplies. We completed the delivery in 45 minutes, demonstrating practical mission utility beyond abstract capability demonstrations. Plus, if you can successfully deliver hot chicken sandwiches and waffle fries in the time it takes most delivery apps to assign a driver, you can deliver actual mission-critical cargo when seconds count.


"There's really nothing like this that's ever been done," Andrew explains. "Actual repeated logistics between remote sites through controlled airspace in the U.S. No one is doing this at the distances we're doing, through the types of airspace we're flying."


And this mission demonstrated just a fraction of the range our V2 aircraft can reach.


The operational difference is altitude and integration. Most commercial UAS operations stay below 400 feet, effectively operating below conventional aviation. Our aircraft flew at 3,500 feet, requiring full integration with existing air traffic management infrastructure—the same environment where scaled operations will eventually need to function.


Jessica Hogan, our CMO, emphasized the significance of our selection for the mission: "We were the only ones selected for the task order because we're the only ones that could have done this. It was monumental."


At the post-operation debrief, program leadership specifically recognized our team's performance, singling out Andrew after the conclusion of the task order, making special mention of his extraordinary piloting capabilities and managing the unplanned landing of the V3 aircraft with grit and a steady hand. The kind of recognition that's worth more than any press release—especially coming from people who've seen enough emergency landings to know the difference between "handled it" and "nailed it." The validation came from officials who understood exactly how difficult the execution had been.


## **When Best Laid Plans Meet Reality**


The engine failure occurred during a first-ever customer demonstration of our V3 aircraft flying BVLOS in the national airspace, worth noting since our V2 platform completed 20 successful operational flights without issue. One cylinder on the V3 failed while the other continued operating—a failure mode that doesn't present clear signatures. The aircraft autonomously transitioned to vertical flight mode and began descending from several thousand feet in the air, an altitude four times higher than most cargo UAS ever reach. The descent took seven minutes and nearly drained battery reserves by the time it touched down, giving the team real-world insight into power limits under emergency conditions.


Post-flight inspection revealed the battery pack was visibly swollen from the extended high-draw operation. Our team calculated approximately ten seconds of remaining capacity at landing. (Not a margin we'd recommend.)


The immediate response demonstrated operational maturity. Two of our technicians were en route to Grand Forks before the recovery team reached the aircraft. The engine manufacturer dispatched a representative with a replacement engine the same day.


Despite the V3 setback, we successfully completed the mission. "The reliability of the V2 platform impressed us throughout the operation," Andrew notes. "When missions didn't complete, it was due to weather or external factors. The aircraft performed."


## **Operational Insights No One Else Has**


The engine failure and emergency landing generated data that doesn't exist anywhere else in the industry. No other cargo UAS operator conducts missions at comparable altitudes, distances, or integration levels. That makes our operational experience—including failures—uniquely valuable.


"By flying real missions that few others can, we gain critical knowledge that exponentially fuels our progress...the real world is the ultimate educator," said CEO, Charles Acknin.


Above: Skyways engineering and flight team (Blake Bequette, Nick Pombuena, Mark Sims, Andrew Fung, Steven Piston) with CEO, Charles Acknin


**Things we learned on our mission:**


**Air traffic control integration at scale** requires automation. Scaled operations with multiple aircraft operating simultaneously need automated systems that handle routine ATC interactions. We're developing these capabilities around existing infrastructure rather than waiting for future ATC modernization.


**Automated emergency response** is critical. Autonomy stacks must have the ability to detect failure modes, select landing sites, execute procedures, and notify operators of actions taken—converting high-workload emergencies into low-workload monitoring tasks—safely and effectively.


**Diagnostic systems** need to provide actionable information, not just raw data. The engine failure demonstrated how much interpretation operators must perform when systems degrade. Development priorities include diagnostic automation that presents clear status and recommended actions.


**Multi-party coordination** complexity scales non-linearly. The North Dakota operations involved nine organizations. Communication and coordination challenges were primarily organizational and procedural, not technical. Scaling autonomous operations requires standardized coordination protocols.


**Minimal ground infrastructure** at the destination unlocks new mission profiles. Our aircraft air-dropped cargo to remote locations without prepared landing sites, personnel, or ground equipment—capability that expands the operational envelope beyond traditional aviation.


**Regulatory frameworks** remain the critical path. The technology performed. The mission succeeded. But Beyond Visual Line of Sight (BVLOS) operations remain extremely limited in the United States for routine logistics. Regulatory evolution—not technical capability—is now the primary constraint on scaling in the US.


## **What Happens Next**


The North Dakota operation proved the capability: autonomous aircraft can deliver operationally relevant cargo within the existing aviation system.


Near-term, we're focused on distributed logistics networks for environments where current solutions are economically or practically unfeasible—offshore energy operations, forward military installations, remote communities where helicopter operations are prohibitively expensive and surface transportation is too slow.


The longer-term trajectory extends well beyond cargo. We're working toward operational missions for paying customers, accumulating flight hours in real-world conditions, and building the experience required to scale this capability into persistent logistics infrastructure. The work happening in North Dakota, offshore in the Baltic Sea, and across Japanese island chains isn't experimental. It's operational. And the lessons we're learning—including from unplanned events like "Wheat Whacker"—are accelerating our development in ways that prototype testing simply can't replicate.


FAQs:


1. What does BVLOS mean and why is it important?


1. BVLOS stands for Beyond Visual Line of Sight. It allows UAVs to operate far beyond the pilot’s direct view — essential for scaling cargo drone operations.


2. Why was the North Dakota mission significant?


1. It validated that autonomous cargo delivery in controlled airspace at operational scale can be achieved safely and effectively.


3. How does Skyways’ approach differ from other cargo UAV companies?


1. Skyways focuses on long-range, heavy-payload operations for innumerable defense and commercial use cases. Skyways aircraft enable logistics in remote areas where roads or runways aren’t practical and current options are costly and put human life at risk. Skyways is currently flying with customers and generating revenue.


4. What’s the biggest challenge for autonomous cargo aircraft today?


1. Regulatory frameworks. The technology works — scaling will depend on regulations evolving to allow routine BVLOS operations.
