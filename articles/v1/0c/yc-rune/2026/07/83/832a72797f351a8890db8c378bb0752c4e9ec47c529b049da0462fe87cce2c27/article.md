---
schema_version: "1.0.0"
document_id: "832a72797f351a8890db8c378bb0752c4e9ec47c529b049da0462fe87cce2c27"
company_key: "yc-rune"
company: "Rune"
source_id: "yc-rune-news-import-dca8148f0685"
canonical_url: "https://www.runetech.co/news/saga-launch"
published_at: null
first_seen_at: "2026-07-23T23:32:11.319893+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:96fe97966ab03d747a6c4ff132188784b46c1812704246f153937a1379e76375"
---

# The next war will be won on supply lines. Rune's AI agent is already there

The order comes down at 0200. Nine brigades are battered across weeks of sustained combat operations. Some are at half their combat power. A few are barely operational. The mission: regroup and reorganize the forces to maximize their collective operational capability once again... to get back into the fight, because the battle is not yet won.


A dedicated staff of two dozen logisticians with maneuver unit subject matter experts sits down to work the problem. They will still be working it out days from now.


This is reconstitution: one of the most complex and manpower-consuming tasks in military logistics. To do it right, logistics officers must simultaneously account for personnel, equipment, maintenance parts, and movement across a degraded and dynamic operational picture. Moving a tank requires the right crew. Move the crew, and now maintenance support and the parts needed to make it ready must be reassigned. Every decision cascades.


At its core, reconstitution is a multi-dimensional optimization problem. The exact kind that computers were invented to help us solve in less time. Yet today, our warfighters make this a slow, stubby pencil, manual process, informed through working radios, scouring multiple disconnected data systems, and whiteboards in the middle of the night just to scope out the full picture to make the right decisions.


Moments like this stayed with us because the pattern was always the same. The doctrine and procedures are well defined. The logisticians are savvy and experienced at what they do. But in large-scale combat operations, the time to pull the right information together, reason through options, and produce the right courses of action under pressure sometimes simply does not exist.


**The Whiteboard That Runs the Fight**


Step into a makeshift supply yard in a contested environment, and you will find the same tools that have defined military logistics for generations. Supply counts are scrawled in marker. Unit designations crossed out and rewritten. Phone numbers for points of contact in the supply chain taped to the corner. People who are doing the best they can with what they have.


Out in the field are helicopters that can evade radar and autonomous drones that need no pilot. Even so, we track the fuel that keeps them flying on a whiteboard with a dry-erase marker. The contrast is stark.


Logistics has always been the unglamorous thing that is taken for granted, yet determines whether everything else works. We have spent decades perfecting weapons. But the supply chains that feed them still run on phone trees and hand-written plans.


A senior logistics officer carries decades of hard-earned judgment to know when the doctrinal rate for fuel consumption doesn’t match actual conditions or when a recommended route looks clean on paper but won’t hold under real terrain and potential threat. The technology to match that type of operational know-how has been hard to develop. Until now.


The disconnect has become impossible to ignore. And using advances in AI, it’s the exact problem we set out to solve.


**Building Solid Ground for Intelligence to Stand On**


When we started Rune, there was already enormous pressure in defense to develop new applications for AI. To task: build an LLM chatbot with a military skin on it and call it ready for operations. We made a different choice.


LLMs are extraordinarily competent for synthesizing large volumes of information or reasoning through ambiguity. But they are not built for the precise math and hard computational logic that military logistics actually demands. We shouldn’t rely on LLMs to estimate fuel consumption from memory or to guess at available parts. It needs to be grounded in systems that can handle the hard math correctly.


So we built[TyrOS](https://www.runetech.co/tyros) : the underlying platform, algorithms, and edge-first architecture that embeds intelligence right where decisions get made. We deployed it across multiple large-scale units in the U.S. Army and Marine Corps and proved it works in environments where traditional systems fail. Logistics operations today break down under network degradation or when units are disconnected, because they rely on a distant server to tell a unit where its assets are or what it needs. With TyrOS, the intelligence lives at the edge, where these decisions actually happen, and converges over the mesh of logistics nodes.


That foundation was essential for what we knew we wanted to create next.


**Introducing Saga**


Saga is Rune’s newest AI agent. It’s built on top of TyrOS’ operational and sustainment know-how and represents the future of logistics intelligence for the military.


Saga functions like a twenty-year veteran logistician who has absorbed every doctrine, after-action report, and field manual, and it learns from real-time and historic trends by unit and across the force. It provides logistics officers with real-time visibility into current assets and supplies, forecasts consumption, and reasons over the full operational picture. This combination of experiential depth and live situational awareness is what allows Saga to make the kinds of operational recommendations that previously required a large staff and days or weeks of planning.


When an officer asks Saga for options to resupply an armored brigade with three days of supply (DOS) of fuel, it doesn’t just cite the standard doctrine. Saga looks at how many systems are actually available, accounts for the terrain and conditions dictated by the scheme of maneuver, reviews past consumption and produces a broad selection of courses of action. Saga develops experience-based options that may even go beyond the experience of the officer asking the question. The interface looks like a familiar conversational chat window, but the outputs are operationally specific. Rich visual courses of action, detailed routes, and decision support come out on the other side of a plain-language question.


What separates Saga from being just a chatbot for military operations is what happens beyond the response.


An agent reasons and acts. It continuously monitors consumption, changes in the operational environment and with the latest commander’s intent. When the problem is complex and the information is incomplete, Saga asks clarifying questions. It generates recommendations and invites the officer to explore them to find the right course of action. When a decision is made, Saga can take the next step to execute it with a human in the loop: scheduling distribution missions, ordering troop movements, and taking actions that previously required a human working through a series of manual steps. The end goal is for a logistics officer to condense days of work into a simple conversation with Saga, all the while staying in control and making the final judgment call based on the information presented to them.


This brings us back to reconstitution. The same problem that consumes two dozen personnel for days, Saga can do in a matter of seconds.


Saga is the kind of leap forward that transforms what’s operationally possible.


**Designed for the Battlefield**


There are two things we were uncompromising about in building Saga.


The first is that it had to operate where the problem actually lives. Saga is built for edge deployment, meaning it can operate in environments with denied, degraded, intermittent or latent (DDIL) network conditions – where logistics decisions for the warfighter are often made. This matters deeply: a system that works only in perfect conditions and goes dark the moment communications are contested is a liability. In a world where adversaries increasingly target lines of communication, the ability to keep operating in a decentralized fashion, with the best information available, is a necessity.


The second is placing clear guardrails on what Saga can and can’t do. Agents that can act can also act incorrectly. Unintended consequences in a logistics context can have a serious operational impact. So we have been deliberate from the start about which actions Saga can take on its own and which require human judgment to make the final call. This allows the technology to work as it should, keeping the logistics officer in command while removing the burden of process and manual calculations that have always slowed them down.


Saga is also designed to operate inside a broader ecosystem of agents. Through the model context protocol (MCP), Saga can serve as the logistics expert in a multi-agent warfighting cell. It fields questions from other specialized military AI systems that need to understand sustainment before they can make their own judgments. This kind of agent-to-agent collaboration is already taking shape within military branches and points to where battlefield AI is heading: specialized agents that know their domains deeply and communicate with each other fluidly.


Saga is poised to be the logistics node that every other agent turns to for sustainment questions.


**The Future of Logistics Speed and Resilience**


Reconstitution is just one place we see Saga changing how military operations work.


Predictive maintenance is next on the horizon. A maintainer in the field dealing with a fault on an Apache helicopter should be able to interrogate Saga on their end user device, describe what they're seeing, and get an immediate answer: the parts needed, the repair procedure, and the personnel to fix it. For planning and wargaming environments, Saga’s potential is also significant. It can provide scenario refinement, iterative stress-testing of plans before execution, and the ability to ask critical planning and sustainment questions and get a set of realistic scenarios instead of a guess.


The through-line across all of it is the same. Saga unlocks faster decisions with less toil and better outcomes. Logisticians are able to conserve their effort and judgment for tasks that actually require human judgment.


Military logistics has always been a crucial determinant for whether operations succeed or fail. Until now, logisticians have never had software that truly answers that call. A tool that doesn’t just track supplies but reasons about them. That doesn’t just surface data but has the intelligence to reason about it. That doesn’t just assist the logistics personnel but works alongside them as a trusted peer.


Back in the war room, the whiteboard marker runs dry sometime around 0400. Someone finds another one. The count gets updated, and the phones keep ringing. Outside, tank platoons are waiting on a decision that twenty people are only beginning to sort through. The future of warfighting won't be determined just by who has the best weapons or the most experienced operators. It will be decided by who can turn a two-day problem into a thirty-second decision. We built Saga to be that answer.
