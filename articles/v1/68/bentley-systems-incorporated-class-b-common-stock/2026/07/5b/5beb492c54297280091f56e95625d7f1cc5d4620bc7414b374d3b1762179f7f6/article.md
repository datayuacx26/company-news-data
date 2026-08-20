---
schema_version: "1.0.0"
document_id: "5beb492c54297280091f56e95625d7f1cc5d4620bc7414b374d3b1762179f7f6"
company_key: "bentley-systems-incorporated-class-b-common-stock"
company: "Bentley Systems Incorporated"
source_id: "bentley-systems-incorporated-class-b-common-stock-news-import-dc7c98e657e9"
canonical_url: "https://blog.bentley.com/insights/quebec-bridge-rises-ai-recreates-engineering-wonder/"
published_at: "2026-07-16T16:00:43+00:00"
first_seen_at: "2026-07-24T19:29:36.034394+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:f4bc710b6af329eec059ee0ef242ef41df4a58f612bb0c1474247c054b9321c2"
---

# The Quebec Bridge Rises Again: AI Agent Recreates Engineering Wonder

Louis-Martin Losier has given this tour many times. When visitors come to Quebec City, he walks them from the historic Château Frontenac hotel to the Plains of Abraham battlefield, then heads down to the Quebec Bridge, which crosses the St. Lawrence River west of the city. “I tell them, this huge bridge was a great engineering achievement and people called it ‘The 8th Wonder of the World,’” he says.


The bridge is just a 13-minute drive from the Bentley Systems office, where Losier serves as a senior product manager for MicroStation, the trusted design platform engineers around the world have used to lay out roads, bridges, and railways for the better part of four decades.


The Quebec Bridge earned its wonder-of-the-world reputation the hard way. Nearly a kilometer long (0.62 miles), with a main span of 549 meters (1,800 feet), the steel structure took 17 years to complete. It remains the longest clear-span cantilever bridge in the world.


Seven years after construction began, the original bridge collapsed into the river after lead engineers lengthened the central span without redoing their structural calculations. The extra weight buckled the superstructure, taking about 75 workers down with it. And in 1916, with the rebuild almost complete, the new center span fell during hoisting, killing 13 more workers.


(The tragedies inspire some newly minted Canadian engineers to receive an Iron Ring, as a reminder “of the tragic cost of human error in engineering work.” The ring is worn on the pinky of the dominant hand, which means that every time they sign a document with a pen, the ring clinks against the table and calls attention to the significance of the act.)


Losier recently built the bridge again. Not with 66,000 tons of steel, but as a 3D geometric model inside MicroStation, the Bentley Systems software behind much of the world’s infrastructure. Nor did he build it with his own hands. Instead, he directed an AI agent in plain English, and it controlled the software to create the model.


The goal was not a rivet-by-rivet digital replica, but a geometrically faithful, engineering-scale reconstruction that could test how far an AI-driven workflow could go when given the right context, constraints, and tools.


A 3D model of the historic Quebec Bridge, which crosses the St. Lawrence River west of the city.


## An irresistible dare


The idea began as a dare. In March, at the 2026 AI in Architecture, Engineering, and Construction conference in Helsinki, Finland, Losier got into a conversation with another attendee about how far the technology could be pushed. The tool they had in mind was Bentley’s Python assistant, released in 2025, an AI aide inside MicroStation that turns plain-language requests into the Python scripts the software can run to build geometry on screen. “He said, ‘It would be cool to use the Python assistant with a complex engineering benchmark,'” Losier recalls. “Then he said, ‘What about the Eiffel Tower?’”


Back home in Quebec, Losier gave it a try. The result in MicroStation was crude, for sure, but unmistakably Eiffel-ish. It fired up his curiosity. A plausible tower conjured from a single sentence was a party trick. A giant local landmark that took three tries to build would be a real test. “We needed to find complex benchmarks for our new AI tools,” he says. “If we can achieve high accuracy, we can demonstrate for our users how they can do it, too.”


For the bridge, Losier turned to something newer and still limited to Bentley’s trusted testers: the MicroStation MCP server. MCP, short for Model Context Protocol, is an open standard that lets AI assistants operate other software. Bentley’s server is what gives an AI assistant access inside MicroStation. An engineer picks the assistant they already use, such as GitHub Copilot or Claude, and connects it to MicroStation through the server. Everything runs locally on the engineer’s machine, where the server passes instructions from the assistant to a live MicroStation session.


Losier supplied the AI assistant with context materials – historical drawings and engineering schematics, dimensions, geolocation data, and modern-day photographs – and told it to create a geometrically accurate model of the bridge. The agent interpreted that intent, extracted what it needed from the materials, and got to work inside MicroStation.


His earliest experiments relied on a single, carefully structured prompt that specified the bridge’s proportions, coordinate system, segmentation, and modeling sequence. The results were promising, but they also revealed the limits of trying to capture such a complex engineering process in a single instruction.


Nearly a kilometer long (0.62 miles), with a main span of 549 meters (1,800 feet), the Quebec Bridge took 17 years to complete. It remains the longest clear-span cantilever bridge in the world.


## If at first you don’t succeed


“The agent’s first attempts were a bit of a mess,” Losier admits. “The agent confused the profile of the cantilever bridge with Quebec’s Pierre-Laporte suspension bridge, which runs parallel to it nearby, and began blending the two bridge styles.” The trusses also came out wrong. The Quebec Bridge is defined by its eye-grabbing K-truss configuration – a century later, it still gives the bridge an otherworldly look fit for a Ridley Scott movie – but the agent repeatedly reached for more conventional patterns instead.


Then there was the ground itself. The early model wasn’t properly georeferenced, floating free of any real-world coordinate system. In subtler forms, that problem is one of the industry’s most common and costly mistakes, says Losier, who has a master’s degree in geomatics, the science of gathering, processing, and analyzing geospatial data. “It was clear the agent had creative capability,” says Losier. “What it lacked was engineering discipline.”


So Losier stopped treating the agent like a genie and started treating it like a talented drafting assistant that needed a detailed plan and human oversight. (He had proved the approach once already, on a reconstruction of the Antikythera mechanism, the 2,000-year-old Greek astronomical calculator.) Everything would rest on creating a detailed, structured plan. Losier had an AI agent build it around a set of written specifications that fixed the bridge’s geometry and locked key measurements, forbidding the model from inventing them, which significantly reduced hallucinations. The plan also defined the coordinate system, structural layout, proportions, and construction sequence. Just as importantly, it told the AI how to work.


## Building a Bridge, One Step at a time


Rather than building the entire bridge at once, the plan broke the task into small stages. Losier reviewed and approved each step before the next began. “I told the AI assistant, I don’t want you to create one script that does everything in two seconds,” he says. “I want it built in small chunks of work that can be validated, that we can examine visually on screen, so we catch any errors early instead of letting them compound.”


With the detailed plan locked down, he gave it to an AI agent connected to MicroStation through Bentley’s MCP server. Sure enough, the bridge rose section by section, following the sequence laid out in the plan. Constrained this way, the agent behaved beautifully, Losier says. “As long as you define the limits, give clear instructions – use exactly this information, do not hallucinate – you’re giving guardrails, and the agent can do a pretty good job,” he says.


The finished model closely matched the real-world geometry of the bridge. Losier was pleasantly surprised by the accuracy of the geometry and geopositioning. “I’m no bridge expert, but I was able to recreate that bridge, with just a few days of effort,” he says.


## Geometry? Check. Gravity? Check.


An impressive drawing and a sound structure are not always the same thing, as the history of the Quebec Bridge demonstrates. Structural validation is the job of Bentley’s analysis software, STAAD, and wiring it into the AI workflow was the obvious next step for Losier. In the future, he says, “I see engineers creating drafts faster with our MicroStation MCP, then using our new STAAD MCP to validate the structural part, before sending the design back to MicroStation for completion.”


At this stage, however, his bridge model had the required geometry, but it didn’t contain all the information needed for structural analysis. So Losier went back to the workflow. To sharpen the agent’s reasoning about the bridge’s structural behaviour, he worked with two frontier AI models – Claude’s Fable and OpenAI’s GPT 5.6 Sol – to build a set of specialized skills that drew on photographs, the bridge’s profile, and other characteristics, along with lessons from the previous failed attempts. This enabled him to refine the build plan and tighten the constraints the bridge-building agent was allowed to operate within. This helped the AI better distinguish between geometry that merely looked right and geometry that could support meaningful structural analysis – and ultimately a bridge that could serve people.


Losier used an AI agent to run a structural analysis of the Quebec Bridge using STAAD MCP server.


With these steps completed, Losier brought in Bentley’s new STAAD MCP server, and he used the agent to run a structural analysis. “What impressed me was that the agent was able to connect the geometric model to a structural analysis workflow,” Losier says. “Engineers will interpret and review the results and adjust the model based on their judgement. These adjustments could be dictated by the engineers to the agent to complete. That’s the kind of iterative loop engineers go through every day. Seeing an agent participate in that process was a glimpse of the future.”


Christos Aloupis, a STAAD expert in Bentley’s structural engineering group says that “structural analysis would show the behavior of the structure under different loading conditions, and would help engineers identify potential flaws of the model that might not be easily visible on the MicroStation model. This is also the reason why having the model transferred to STAAD is important.”


## What Engineers Do Next


Losier is careful not to oversell the result. The model is not a perfect digital replica of the Québec Bridge. It captures the bridge’s overall geometry, proportions, location, and primary structural systems, but it does not reproduce every detail of the original structure. Bridge features such as individual connections, hollow multi-layer box chords, and the dense lattice bracing would require a much higher level of detail.


“In engineering terms, this is closer to a conceptual or LOD2-style reconstruction than a fabrication model,” says Losier. “The goal wasn’t to reproduce every rivet. The goal was to create and validate a draft model of the bridge’s essential geometry, carry it into structural analysis, and do it in a way that engineers could review, discuss, and improve the design.”


And the engineers remain paramount, says Losier: “This initiative is not about replacing engineers. Our goal is to empower them by automating repetitive, low-value tasks so they can spend more time on engineering, innovation, and problem solving.”


This workflow is early and experimental, but when it matures, Losier reckons it will change the shape of engineering design. “I see this as a disruptive technology,” he says. “For example, if the agent shows you a draft of the bridge that you asked for, but you’re not happy with it, you can say, ‘let’s change that part,’ and a few minutes later you have a revised design that can be checked again.” That process shifts what an engineer does with their day: less time manipulating complex software, more time exploring alternatives, testing ideas, and deciding which solution is best.


The final call, though, is always the engineer’s. “AI can help generate options and accelerate workflows, but accountability and final approval remain with qualified engineers,” Losier says. “The technology will multiply what they can achieve.”


Combining AI agents with capable engineering software will have another knock-on effect, says Losier. “A junior engineer who doesn’t yet know the power of MicroStation – the workflows, the tools, and where they sit in the software’s menu ribbon – could start using MicroStation with Claude Desktop or GitHub Copilot, just give their intent, and see results very quickly.”


The Quebec Bridge itself is a useful measure of how much has changed. When the last rivets were driven in 1917, some 30 years after the bridge was first proposed, the London journal


*The Engineer* hailed it as “one of the greatest, if not the greatest, feats of bridge engineering the world has ever seen.” More than a century later, one curious product manager, working with modern engineering software and AI, was able to reconstruct its geometry, prepare it for structural analysis, and explore design alternatives in a matter of days.


The bridge’s first designers sent 75 men into the river because, when the span was lengthened, nobody redid the calculations. When a change to a complex design can be modeled and re-checked in an afternoon, such corner-cutting will be banished to the past.


## FAQ:


**


**


How did the MicroStation MCP server help recreate the Quebec Bridge model?


The Model Context Protocol server acted as an open-standard bridge that allowed an external AI assistant to operate MicroStation software locally. By connecting the AI directly to a live session, the engineer could use natural language to direct the agent, which then extracted data from historical schematics and photos to build the bridge’s geometry section by section.


**


**


What role does AI play in the structural analysis workflow for infrastructure design?


AI agents can connect geometric models to structural analysis programs like STAAD to check engineering viability and interpret data loops. While the AI automates repetitive drafting tasks and accelerates alternative design exploration, accountability remains entirely with human engineers who must validate the results to prevent historical structural errors.


**


**


Why did the AI agent fail its first attempts to model the bridge accurately?


The agent initially lacked engineering discipline and hallucinated details, confusing the Quebec Bridge with a nearby suspension bridge and using standard trusses instead of the specific K-truss pattern. It also failed to georeference the model, leaving it floating without real-world coordinates until the engineer implemented a structured plan with strict geometric guardrails.
