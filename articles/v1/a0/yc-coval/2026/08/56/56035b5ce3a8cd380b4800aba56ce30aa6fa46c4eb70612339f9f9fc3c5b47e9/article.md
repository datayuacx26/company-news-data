---
schema_version: "1.0.0"
document_id: "56035b5ce3a8cd380b4800aba56ce30aa6fa46c4eb70612339f9f9fc3c5b47e9"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/voice-ai-first-inning-mike-droesch-bessemer/"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-17T12:22:39.426921+00:00"
fetched_at: "2026-08-17T12:22:40.644010+00:00"
content_hash: "sha256:e7692144d8b091a79e9379873ea23a6bcec9be2dd767ae9f52b44488510a3aff"
---

# Voice AI Is Still in Its First Inning: Bessemer's Mike Droesch on What Comes Next

# Voice AI Is Still in Its First Inning: Bessemer's Mike Droesch on What Comes Next


[Henry Finkelstein](https://www.linkedin.com/in/henryfinkelstein) ✓ Expert verified


Founding Growth Engineer · August 17, 2026 · 14 min read


Bessemer partner Mike Droesch and Coval CEO Brooke Hopkins unpack where voice AI is working now, what enterprise adoption actually takes, and where the next wave of opportunity will come from.


Voice AI has moved from a speculative category to a board-level priority in less than two years. But according to Mike Droesch, a partner at Bessemer Venture Partners who has led the firm’s voice AI thesis since early 2024, the market is still only scratching the surface.


Mike joined Coval co-founder and CEO Brooke Hopkins for a wide-ranging conversation about what is working in production, why enterprise deployments still require hands-on engineering, which industries are adopting voice faster than expected, and where the next generation of companies will be built.


## The short version


- **The best early use cases are high-volume, structured, and verifiable.** Scheduling, intake, reservations, and other transactional calls remain the most reliable entry points.
- **Enterprise voice AI is not an out-of-the-box deployment.** Teams need to map policies, connect systems of record, define what a successful call looks like, and earn permission for agents to take real action.
- **Forward-deployed engineering is part of the product—for now.** Templates and reusable primitives will make each launch easier, but every enterprise still has its own long tail of real-world edge cases.
- **Voice wins wherever a screen is unnatural.** Healthcare, home services, logistics, construction, field sales, and other deskless workflows are adopting voice because it fits how people already work.
- **The next wave is multimodal.** Voice agents that can see a screen, interpret a camera feed, use a browser, or direct a physical system will unlock workflows that did not previously happen over the phone.
- **Outcome-based pricing is directionally right but operationally early.** The model works when success is easy to define. Most vendors will use hybrid subscription and usage pricing until attribution and infrastructure costs improve.


Podcast


## Watch or listen to the conversation


[Open the full recording](https://drive.google.com/file/d/1uvt8JcPAS_lr3cwFyWoBJ-b66jJ2ljm_/view)Read the transcript


## Voice AI started with a missed call


Bessemer’s thesis began with a simple home-services problem. Contractors and business owners spend their days in the field, yet much of their revenue still arrives by phone. Calls after hours or while a technician is on a job often go unanswered. A capable voice agent can turn those missed calls into booked appointments.


The use case was compelling because the value was immediate: the technology was not asking a business to invent a new workflow. It was recovering demand that already existed.


That early example pushed Mike and the Bessemer team to study the full stack. In early 2024, the models were only beginning to become fast enough for real-time conversation. Tool calling was immature. Teams that had built voice agents needed dedicated infrastructure engineers and a great deal of custom work.


The investment question became: which hard-won capabilities should become reusable infrastructure for everyone else?


Bessemer later invested in Vapi and, across the firm, in companies including Abridge, EliseAI, and Rilla. The portfolio spans infrastructure and applications, but the thesis is consistent: cost and latency will continue to fall, voice will become a natural interface to software, and applications will expand well beyond the call-center workflows most people associate with the category today.


## The deployment gap is organizational, not only technical


The lowest-hanging fruit in voice AI shares three properties:


1. The call happens frequently.
2. The flow is relatively structured.
3. The result can be verified.


Appointment scheduling, basic intake, and reservations fit that pattern. The agent has a defined job, a limited set of systems to use, and an outcome that both the vendor and the customer can measure.


Large-enterprise deployments are different. An agent cannot create a good customer experience by reading an FAQ more naturally. It needs permission to take action: update a system of record, change a reservation, unblock a card, schedule a patient, or hand the call to the right person with the right context.


That requires work beyond model selection. The vendor and customer must map internal policies, identify the tools the agent can use, define what a successful call looks like, and agree on how to evaluate the system before it reaches customers.


> “There’s no shortcut at the moment.” — Mike Droesch


Brooke framed evaluation as product definition rather than a testing task at the end of development. If a team cannot specify what success means for a pilot, the promise that an agent can “do everything” quickly turns into a failure against every possible workflow.


The strongest deployments start with one bounded use case, one clear hypothesis, and an explicit success metric. From there, teams can expand based on evidence.


## Forward-deployed engineering is not going away yet


Voice agents meet the physical world in ways that are difficult to predict in a staging environment. A caller may be driving with the radio on, standing in a noisy restaurant, switching languages, spelling an unfamiliar name, or changing an order three times mid-sentence.


Vendors will build better templates, industry-specific integrations, and libraries of known failure modes. The hundredth healthcare deployment should be easier than the first. But at large enterprises, Mike expects engineers to remain deeply involved in initial deployments for the foreseeable future.


The reusable layer gets stronger with every customer. The customer-specific layer does not disappear.


That has consequences for company building. A services-heavy implementation motion is not necessarily a temporary embarrassment on the way to pure software margins. In AI, it can be the mechanism that turns ambiguous business logic into a reliable product. The advantage compounds when each engagement contributes reusable primitives, evals, and domain knowledge.


## Healthcare moved faster than expected


Healthcare has historically been treated as a slow adopter of new technology. Voice AI has challenged that assumption.


Mike pointed to Abridge, where clinicians trust an AI system to transcribe highly sensitive patient conversations, generate notes, and increasingly support workflows connected to billing. Scheduling and intake agents are also gaining adoption across health systems.


The urgency matters. Healthcare organizations face severe staffing pressure and large volumes of manual work. When the existing pain is acute enough—and the product is designed for the industry’s vocabulary, workflows, and risk tolerance—the value of automation can outweigh institutional inertia.


Financial services and insurance are moving too, but trust has to be made concrete. Deterministic checks such as verifying a patient’s birthday before discussing health information help enterprises put boundaries around an otherwise probabilistic system.


The counterexample is drive-thru ordering. On paper, it looks easy: a known menu, a short interaction, and a structured output. In reality, background noise is constant, customers are impatient, and orders are revised nonlinearly. Humans effortlessly keep track of “go back to the burger and remove the pickles.” Voice agents can lose the state of the order.


The contrast is useful. Regulation does not automatically make an industry slow, and a narrow vocabulary does not automatically make a voice task easy. User behavior, environment, and tolerance for friction matter just as much.


## Voice becomes most valuable when screens get in the way


Many of the strongest voice use cases occur where the user is not sitting at a computer.


A technician on a job site, a truck driver on the road, a clinician moving between patients, or a construction worker documenting a site may find a form or tablet unnatural. Speaking is already how they coordinate with other people.


That creates opportunities beyond replacing existing phone calls. A worker can narrate a site inspection while a system combines the audio with images. A field salesperson can receive feedback without a manager attending every meeting. A traveler can rebook a disrupted trip without navigating a dense mobile interface.


Voice also creates elastic capacity. An airline contact center cannot staff perfectly for a storm that cancels thousands of flights at once. AI agents can scale with demand, handle the portions of calls they are equipped to resolve, and bring in a human when judgment or authority is required.


The most useful architecture may not be full automation. Like remote assistance for autonomous vehicles, a human can supervise many agents and intervene only at a high-leverage decision point.


## The next voice agent will be able to see


Mike is especially interested in agents that combine voice with another modality.


Imagine a customer sharing their screen while an agent explains where to click. A product demo that responds to a prospect’s questions while operating the browser in real time. A construction agent that interprets a camera feed while the worker narrates the site. A robot or autonomous vehicle that can accept natural verbal direction without forcing the user into an app.


These are not simply call-center automations with a video feed added. They are new interfaces to software and physical systems.


Brooke described the missed opportunity in today’s product design: early websites copied paper documents onto a screen before the web developed its own interaction patterns. Voice AI is at a similar stage. The first generation automates standard operating procedures that already happen by phone. The next generation will treat conversation as a native interface available alongside web, mobile, and chat.


## Conversation data moves from cost control to growth


The value of voice AI is not limited to reducing labor costs.


Rilla, a Bessemer portfolio company, records and analyzes field sales conversations. Before systems like this, a sales manager might ride along for one meeting each month—a tiny sample of a rep’s work. Now every conversation can produce coaching feedback, and patterns across thousands of calls can be correlated with sales outcomes.


The same shift applies to customer research, investment research, support, and product development. The historical bottleneck was the amount of conversation a human could conduct or review. Voice agents can run many interviews in parallel, while analysis systems can turn the resulting data into product insights.


For enterprises, this changes the objective. The first phase of observability asks, “Is the agent going wrong?” A more mature phase asks, “What are our customers telling us, where are they getting stuck, and what should we build or offer next?”


That is the path from a cost center to a revenue-generating system.


## Outcome-based pricing needs better measurement


Charging for a resolved ticket, a booked appointment, or another completed outcome aligns the vendor with the customer’s value. The model is already emerging in workflows where the universe of requests and the definition of success are clear.


It becomes harder when call types vary widely. Two “resolved” calls may differ by an order of magnitude in business value. A call can also fail for reasons outside the agent’s control, and the cost of voice infrastructure remains material.


Mike expects subscription and usage-based components to remain common in the near term, with pricing negotiated against expected outcomes. More precise outcome pricing will follow better attribution, clearer evals, and lower inference costs.


Brooke noted that an agent call can quickly become expensive when speech, language, and synthesis models compound to 20 or 30 cents per minute. Vendors taking full outcome risk can end up underwater if many calls consume infrastructure without converting.


Open-source models, better routing, and voice-specific inference infrastructure should improve that equation.


## Trust will become a product surface


As voice agents proliferate, bad actors will use them too. Mike expects more disclosure requirements around when a person is speaking with an AI system.


That does not weaken the core value proposition. Responsible voice AI companies are not trying to trick customers into believing a human answered the call. They want the system to be natural, fast, and capable enough that the caller can solve the problem and move on.


Transparency can help separate trustworthy operators from abuse. The companies that win will make identity, permission, action boundaries, evidence, and human escalation visible parts of the product.


## The open opportunities


Mike closed with three areas he wants to see founders push:


- **Multimodal agents** that combine voice with video, screen context, browser use, or embodied systems.
- **Speech-native models in production** that unlock interaction patterns unavailable to cascaded systems.
- **Voice-specific inference infrastructure** optimized for real-time workloads, where cost and latency behave differently from batch or text inference.


The category began with an obvious promise: answer more calls. Its next chapter is larger. Voice is becoming the interface through which people direct software, physical systems, and increasingly autonomous work.


## Edited transcript


*This transcript has been edited for clarity and length. Timestamps mark the start of each topic in the recording.*


### 00:00 | Meet Mike Droesch


**Brooke Hopkins:** Mike, thanks so much for joining. I’m excited to dive into what you’re seeing across the voice AI landscape. Mike is a partner at Bessemer. We’d love to hear an introduction.


**Mike Droesch:** Great to be on. I’ve been at Bessemer for nine years and spent the last two leading our voice AI thesis. It has been an exciting time to invest on both the application and infrastructure sides, and I think we’re still scratching the surface of what’s possible.


### 01:10 | The call that started the thesis


**Mike:** Like many great theses, it started with a founder doing something interesting. In early 2024, I met one of the handful of companies building scheduling voice AI for home-services businesses. Most of their business came over the phone, the owners were in the field, and calls after hours were being missed. A high-quality voice agent available around the clock could pick dollars up off the floor.


That pushed us to understand the model and infrastructure layers. We spent the first six months of 2024 talking to teams that had built their own agents before the tooling existed. It required dedicated engineering teams, models were only becoming fast enough, and tool calling barely existed. We wanted to identify what leading teams had built that should become reusable middleware.


We believed cost would come down, latency would improve, and the application surface would expand. Bessemer later invested in Vapi, and across the firm we have investments including Abridge, EliseAI, and Rilla.


### 04:00 | Voice as the interface to AI


**Brooke:** Coval started with general evals. At Waymo I built evaluation infrastructure and simulation tools for a non-deterministic system navigating the world. Voice agents have similar properties: they perform many tasks autonomously and have to do them reliably.


Our first voice customer also came from self-driving and recognized the same problem. Early investors sometimes said voice sounded niche—who talks on the phone? It turns out every enterprise in the world talks on the phone. Beyond that, conversation is the most natural interface to an autonomous system.


**Mike:** Consumer perception is still shaped by bad IVR experiences. That will take time to change. As people have more experiences with high-quality agents, many will prefer a fast, helpful agent over waiting on hold for a person when they simply want a problem solved.


**Brooke:** It reminds me of the early internet, when people were reluctant to enter a credit card online. Voice agents can already take complex actions, but users do not always trust that the capability exists. Product quality across repeated interactions is how the industry earns that trust.


### 07:58 | What works in production today


**Mike:** The clearest successes are high-volume, structured calls with verifiable outcomes: scheduling, basic intake, and reservations. When the agent connects to a common system of record, those can be deployed with limited effort and high quality.


The opportunity—and the gap—is moving from those repeatable calls to complex enterprise use cases. An out-of-the-box agent will miss internal policies and systems. You need to understand the actions the agent must take. Reciting an FAQ only frustrates people. The enterprise has to trust the agent to make changes in core systems.


You also need to understand what a great call looks like and use historical data to build the right evals. We see false starts when vendors do not spend enough time understanding the use case and earning the trust required to put the system into production.


Today, strong infrastructure plus forward-deployed engineering is the combination that works. There is no shortcut at the moment.


**Brooke:** A clear pilot matters even more in AI than it did in traditional SaaS. The technology promises to do everything, so the sales process can drift into an expectation that every call flow should work. Evals are another way to say product definition: align on what the product should do, define success, and then build the agent against that definition.


### 12:40 | Why deployments stay high touch


**Mike:** It is both. Companies will build primitives and templates that make every deployment easier, but every call is an edge case. Voice agents interface with the real world in ways that are impossible to predict.


A vendor deploying repeatedly in an industry will map more of the potential surface area, so the next delivery gets easier. But large enterprises will still require hands-on tuning. I do not see engineers disappearing from the initial deployment any time soon.


Self-improving agents should accelerate tuning and maintenance, but the first launch remains high touch.


**Brooke:** The business understanding is the hardest part. Customers will mark an interaction as an obvious failure, but an outsider cannot see why until they explain a hidden rule: a phrase that must be exact, a step that cannot be skipped, or an exception to the written procedure. Evals force teams to codify the real product definition.


### 16:33 | Healthcare surprises—and drive-thrus humble


**Mike:** I continue to be amazed by adoption in healthcare. Historically it was considered a technology laggard. Abridge handles incredibly sensitive doctor-patient conversations, generates notes, and supports workflows tied to billing. Large health systems becoming comfortable with and dependent on that technology is remarkable.


Scheduling and intake are moving quickly too. The pain was so severe and the manual workflows so extensive that the opportunity was too good to ignore.


Financial services and insurance are adopting voice as well. Deterministic gates help: confirm the patient’s birthday before discussing health information, for example.


The use case that always surprises me in the other direction is drive-thru ordering. It sounds simple, but background noise is extreme and there are endless ways to revise an order. Humans are very good at keeping track of those instructions.


**Brooke:** Drive-thrus also have impatient users. In healthcare, people have learned to speak slowly to an IVR. User expectations and environment change the difficulty of the same underlying technology.


### 24:06 | Voice beyond the phone call


**Brooke:** Silicon Valley assumes everyone wants a great computer interface. But people in the field do not want to pull out a tablet on a job site. Trucking, logistics, healthcare, home services, and other industries already use the phone to coordinate. Voice is the natural medium.


**Mike:** Exactly—and it does not have to replace a phone call. A construction worker can document a site through a combination of images and transcription. Conversation produces digital exhaust that was never captured before; now we can collect it and do useful things with it.


Another opportunity is infinite scaling. Airline contact centers are either overstaffed or understaffed when a storm causes a spike in demand. AI agents can fluctuate capacity at very little marginal cost.


**Brooke:** Full automation is not the only model. A human can provide remote assistance at the moment an agent gets stuck, similar to rider support for an autonomous vehicle. The agent handles the rest of the call.


### 28:58 | The multimodal frontier


**Mike:** I am increasingly excited about mixed modalities: a voice agent with a live screen share, a camera feed, or browser automation. An agent could walk a user through a website, run an interactive product demo, or help document a physical site.


Connect voice to robotics, a doorbell camera, or another system in the physical world and you unlock capabilities that did not exist before.


**Brooke:** Embodied robotics will be huge. In a Waymo, I want to say, “Pull over here,” or, “Hold on while I put on my seatbelt.” Directing a physical system through buttons is less natural than speaking to it.


Voice is still in the stage where early websites copied paper onto HTML. Today’s products execute an existing standard operating procedure. The next generation will make voice a native interface to nearly any product.


### 32:41 | Turning conversations into revenue


**Mike:** Rilla is a good example. Before voice products could monitor field sales calls, a manager’s alternative was one physical ride-along per month—maybe one meeting out of a hundred. Now every call can be transcribed and produce coaching. The improvement in sales performance is substantial and directly revenue-generating.


The historical bottleneck was how much data a human could consume. Investors could use voice agents to conduct hundreds of expert calls in parallel. Companies can identify upsell opportunities and improve the customer experience across every interaction.


**Brooke:** At first, teams use observability to make sure the agent is not going horribly wrong. The next stage is product insight: where customers get stuck, what they ask for, which products to build, and how to retain someone who has had repeated support problems.


### 35:49 | Outcome-based pricing


**Mike:** We see outcome-based pricing where the interaction is well defined and success is clear. In ecommerce support, for example, you can pay per resolved ticket.


It becomes harder when the range of calls is broad, the end state is ambiguous, and one resolution can be worth 10 or 20 times another. Outcome-based pricing is probably the best long-term alignment of customer and vendor value, but it will take better data and attribution.


For the next few years, many companies will use hybrid pricing: a subscription plus usage, negotiated against expected value.


**Brooke:** Infrastructure cost also matters. A voice call can reach 20 or 30 cents per minute. If many calls do not convert, a vendor taking full outcome risk can end up underwater. As costs fall, pricing can focus more on the value of the work rather than the cost of the models.


### 40:10 | Disclosure, trust, and regulation


**Mike:** As voice agents proliferate, bad actors will abuse them. I would not be surprised to see more legislation requiring disclosure that a caller is speaking with an AI.


That does not take away from the power of the product. Good companies are not trying to fool users into thinking an agent is human. They want the conversation to be natural and low-latency so the user can solve a problem. Disclosure can help distinguish responsible companies from bad actors.


### 42:01 | What Mike wants to see next


**Mike:** I want to meet people combining voice with video, screen capture, computer use, or embodied AI. I am also interested in speech-native models reaching production and unlocking new use cases.


The third area is voice-specific inference infrastructure. Voice workloads have a unique profile. There is a lot of room to optimize cost and latency and tailor infrastructure to real-time speech.


**Brooke:** Spinning up an AWS instance 30 minutes after the call is not very useful to the person calling you.


**Mike:** Exactly. There is a lot of opportunity there.
