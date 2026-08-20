---
schema_version: "1.0.0"
document_id: "99a64eeed651a125ef16e883de099b6357f740fba8874017e20348c3821c89ac"
company_key: "forrester-research-inc-common-stock"
company: "Forrester Research Inc."
source_id: "forrester-research-inc-common-stock-rss-7ea008fcdcc6"
canonical_url: "https://www.forrester.com/blogs/harness-up-for-our-black-hat-2026-recap/"
published_at: "2026-08-10T20:42:54+00:00"
first_seen_at: "2026-08-10T21:19:18.719977+00:00"
fetched_at: "2026-08-10T21:19:20.048349+00:00"
content_hash: "sha256:5c0fae29a553e8b1766d5332b1df477acfaecf49ae849a8233746ab5e139a0cb"
---

# Harness Up For Our Black Hat 2026 Recap

Black Hat 2026 generated more questions than it answered. Are we headed for a vulnerability apocalypse (aka vulnpocalypse), or are we clearing a backlog of flaws that AI can now find with ease? Can organizations patch fast enough? What should buyers expect from vendors when software can reason? Will AI be our undoing or our salvation?


This was a code event through and through. Keynotes and briefings centered on models, agents, vulnerabilities, exploits, and software development. Human risk barely registered. Neither did deepfakes, social engineering, or the people who will trust, supervise, approve, and fall victim to these systems.


[Harnesses, however, were everywhere](https://www.linkedin.com/posts/jpollard96_early-bhusa-2026-takeaway-more-harnesses-activity-7490823627202875394-927X) : harnesses for agents, harnesses for models, harnesses for workflows, and harnesses for the systems that vendors would very much like customers to trust with autonomous action.


In fact, there was a vendor-sponsored lunch-and-learn session titled, “The Harness Wars.”


The conversation in cybersecurity is following the same path as it did in software development. Earlier this year, the smartest model mattered. In the second half of the year, it will be all about the best – and hopefully most secure – harness – that binds the model so it follows instructions.


Our Forrester contingent attended the AI Summit, keynotes, briefings, client meetings, vendor conversations, and the Business Hall. Here’s our take, along with event-inspired haikus.


#### **The Agents Made A Message Board Because Of Course They Did**


*Secure agentic.*
*What does that even mean now?*
*Action logs matter.*


The week’s most discussed session was OpenAI’s account of an incident involving its agents, internal infrastructure, and Hugging Face. Experimental models received difficult, sometimes impossible tasks. When they got stuck, they looked for shortcuts to their rewards. The agents had no direct internet access, but they were able to reach an Artifactory package manager that did.


The agents began writing files into Artifactory and turned it into a shared message board. Others joined. Traffic grew to hundreds of thousands of messages, some embedded in file names, as agents helped one another finish their tasks. The presenters’ observation that “frontier models really like to cheat” was memorable in a way no one wants.


From there, the activity escalated: server-side request forgery, an Artifactory zero-day, compromised credentials, and remote code execution. After OpenAI removed the messages, the agents rebuilt the board within days. The investigation also found they had identified two zero-days in Hugging Face infrastructure.


A big take-away from this session was that none of this activity triggered an alert. It all came to light during an investigation. OpenAI reconstructed the episode after the fact by mining an enormous volume of logs to piece together the agents’ trajectories. No established detections exist for agents quietly standing up a communications system, sharing information, changing course, and exploiting nearby infrastructure.


OpenAI also has access to compute and GPUs that most organizations do not. Enterprises collecting the right telemetry may still lack the resources to reconstruct what their agents were trying to accomplish. Logging an action is necessary. Detecting harmful behavior is harder, and understanding intent is harder still.


The speakers called the agent activity — an impossible task turning into an adversarial situation — an “unintended side effect.” That phrase landed badly. These were effects the designers had not modeled, produced by the agents’ incentives, tools, and environment.


#### **Intent Is A Detection Surface Nobody’s Watching Yet**


*Securing AI.*
*Crawl, walk, run condenses fast.*
*Run, if you can trust.*


Jeff Pollard and Heidi Shey built their[briefing](https://blackhat.com/us-26/briefings/schedule/index.html?day=thursday&track%5b%5d=ai-ml--data-science#the-intent-gap-where-every-ai-regulation-falls-short-and-what-security-leaders-need-instead-51939) around the gap the OpenAI incident exposed: agents operate across sequences, change paths on the fly, and can cause harm without any single step seeming malicious. That makes intent a necessary detection surface. Security teams need telemetry showing how a system moved from instruction to outcome, plus ways to recognize when behavior diverges from an authorized objective.


It’s still, however, unclear who owns this problem. AI security, AI governance, GRC, and broader technology governance blurred together all week. “Runtime,” “convergence,” and “secure agentic” often described very different controls. Without more precision, securing intent will become ordinary monitoring with a fresh coat of agentic paint.


As part of the session, Jeff and Heidi outlined ways to classify AI’s intent (from Engineered and Emergent Helpfulness to Accidental and Purposeful Harm), covered the telemetry required to do so, and showcased required artifacts and a step-by-step workflow that security leaders can use to start assessing and classifying agentic intent over a 90-day period. Stay tuned for published research on this key principle of our[AEGIS framework](https://www.forrester.com/blogs/introducing-aegis-the-guardrails-cisos-need-for-the-agentic-enterprise/?ref_search=604835_1786390524725) .


#### **The Vulnolution Will Not Be Stabilized**


*AI fills the halls.*
*The basics wait patiently.*
*Identity sighs.*


Highlights from keynote sessions included David Weston arguing that AI is changing the economics of attack. As finding and exploiting vulnerabilities gets cheaper, “patch faster” and “detect and respond” become precarious strategies. His prescription: secure construction, formal verification, memory-safe languages, infrastructure as code, and prevention before detection.


The next day, Yan Shoshitaishvili complicated that message. He described attempts to reimplement critical C and C++ libraries in Rust, then delivered the inconvenient result: The bugs did not disappear. Day 1 told attendees to adopt memory-safe languages. Day 2 reminded them that secure construction still requires secure thinking. It seems conference programming sometimes produces its own peer review.


Additionally,[research 1Password debuted at the event](https://1password.com/blog/why-ai-generated-patches-still-require-human-review) put numbers on the problem. Its security research group, led by Keith Hoodlet, found that LLM-generated patches routinely looked complete while remaining insecure. Only 26% fully resolved the vulnerability without changing intended application behavior. More than half failed to block at least one exploit path, introduced a new vulnerability, or both. Another 20% stopped the exploit but altered the way the application behaved.


Whether AI brings a sustained vulnpocalypse or simply exposes a backlog of existing flaws, more findings and more machine-generated patches don’t make anyone safer. Organizations still need to know what matters, what to fix, and whether the fix worked. Many vendors in the business hall remained focused on the “what to fix” with their exposure management and agentic pen testing solutions, while “whether the fix worked” was found more in the startup booths.


#### **The Business Hall: Unbridled Money Trees**


*VC money flows.*
*AI, robots, many trees.*
*Shade in Business Hall.*


Newly launched startups showed up with large booths, including companies out of stealth for only a few months with an effect that was less healthy market validation and more venture capital discovers artificial foliage. Funding says nothing about product quality, and a prominent booth, or a booth with a tree in it, is not proof of traction.


AI and freshly funded marketing budgets crowded out familiar, practical risks. Critical infrastructure, OT, IoT, browser security, and mobile security got far less attention than all things AI and agentic. That matters because agents act through existing infrastructure and endpoints. If the systems beneath them stay insecure, agents won’t need to escape far to cause damage.


These are just some of our Black Hat 2026 observations. Forrester clients can[request an inquiry or guidance session](https://www.forrester.com/inquiry) to discuss what the event’s agentic security, vulnerability management, AI governance, endpoint, OT, and human risk implications mean for their organization.


Share


-
-
