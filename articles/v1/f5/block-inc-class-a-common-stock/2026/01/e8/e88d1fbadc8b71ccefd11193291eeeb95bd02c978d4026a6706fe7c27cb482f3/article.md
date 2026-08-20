---
schema_version: "1.0.0"
document_id: "e88d1fbadc8b71ccefd11193291eeeb95bd02c978d4026a6706fe7c27cb482f3"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/how-we-red-teamed-our-own-ai-agent-"
published_at: "2026-01-15T12:00:00+00:00"
first_seen_at: "2026-07-20T03:32:06.876214+00:00"
fetched_at: "2026-07-28T20:54:46.350230+00:00"
content_hash: "sha256:3a83962b8b95126c99e3c83a8f591ec2fcab2f3bdda01ba242d5a4a889109508"
---

# How We Red-Teamed Our Own AI Agent

# Introduction


At Block, we believe that the best defense is a good offense, especially when it comes to securing AI systems. When we released[goose](https://github.com/block/goose) , a general-purpose, open source AI agent, we recognized the need to proactively identify how attackers will attempt to abuse it.


Operation Pale Fire was our internal red team engagement designed to answer the critical question: In what ways could attackers leverage goose to compromise Block employees?


The results were extremely valuable to improving the security posture of goose. Our Offensive Security team successfully compromised a Block employee's laptop using a prompt injection attack hidden in invisible Unicode characters. Our Detection and Response Team (DART) quickly identified and contained the simulated threat, using the engagement to further strengthen our detection coverage and incident response processes.


## Background: What is goose?


goose, developed by Block and now under Agentic AI Foundation (AAIF), is an open source AI agent that helps empower users with coding tasks, debugging, and workflow automation;spanning engineering and general purpose use cases. goose is able to take meaningful real-world action by interacting with external entities through Model Context Protocol (MCP) extensions, to perform complex multi-step operations.


# Our Initial Approach


## Discovering the MCPs


Our main goal was to figure out how goose can be used as an initial access vector. We started by specifically surveying the commonly used Model Context Protocol (MCP) extensions utilized by internal users of goose. We were looking for extensions that would pull in untrusted content into the context window. In our search we stumbled upon a Google Calendar MCP. Functionality wise it enables users to ask goose questions like "What's on my calendar today?" or "Please re-prioritize my calendar for today".


## Testing Google Calendar


When testing Google Calendar functionality we found that it was relatively trivial to send calendar invitations from external domains, and have those invites appear on users' primary work calendar. We utilized the Google Calendar API to opt out of sending an invitation email, so instead the invite would stealthily pop up on the victims calendar. We also customized the calendar name by creating a shared calendar and giving it a label that would blend in well to the environment.


Loading image...


## Crafting the Prompt Injection


Next we embedded a prompt injection in the description of a calendar event. When a user asks goose, "What's on my calendar today?" goose will query the user's calendar and ingest our payload. In that prompt injection payload we encourage goose to invoke its built-in developer shell tool to check for "updates" to the Calendar MCP. In actuality, this contacts our C2 server to demonstrate how goose could be manipulated to run untrusted code.


## ASCII Smuggling


Here we had all the elements needed to run our attack, but faced one major challenge. Current models have greatly improved task alignment forcing us to craft a lengthy prompt injection. If anyone checked the calendar invite it would be extremely obvious that a prompt injection was in the description. With this in mind we started hunting for ways to hide our prompt injection within the calendar invite.


With great timing[Embrace The Red](https://embracethered.com/blog/ascii-smuggler.html) had just released a[blog](https://embracethered.com/blog/posts/2024/hiding-and-finding-text-with-unicode-tags/) (and tool) on how LLMs could decode zero-width unicode characters, effectively making our attack invisible to the end user. At that time goose had no protections against zero-width characters, and would happily interpret them when processing the invite. Using this we encoded our massive prompt injection within zero-width characters and placed them in the description of the invite unbeknownst to our victims.


Loading image...


## The Payload


Once the prompt injection invoked the developer shell tool, we needed a payload for it to subsequently download and run. We reached out to our Threat Intelligence team here at Block, and inquired about the recent threats they were seeing in the wild. We then modeled this phase after a common real-world infostealer to test our detections and response capabilities.


The payload emulated the data theft behaviors observed in the wild for that particular malware, allowing us to validate how effectively our monitoring and containment controls would perform against such threats.


# The First Campaign


## Putting the Pieces Together


We started our campaign by sending out calendar invites for 8 AM PST for a meeting at 9 AM PST that same day with our pretext urging users to ask goose "What's on my calendar today"?


After sending roughly 50 calendar invitations, we encountered Google Calendar API rate limits. This issue didn't surface during our initial testing because invites within an organization aren't subject to those limits.


We continued the campaign over the following days, sending 50 invitations daily before running into rate limits. We were able to track successful runs by watching to see if a request reached our webserver hosting the infostealer. After approximately five days with no successful requests we decided to continue sending out invites, alongside pivoting to a new vector in hopes for a better result.


## What Went Wrong?


Lots can go wrong! Both the development of the underlying models and the development of the goose agent has been moving at a rapid pace. goose specifically has some interesting challenges, as we allow internal users to pick from a large list of available models from many different vendors. In turn this means that a payload developed and tuned with a particular model might not work with another, even those by the same vendor. Additionally, the non-deterministic nature of LLMs means that there is always a chance that our special "update" instructions might not actually run. Lastly, users can have wildly varying content in their context windows based both on what tools/MCPs they have enabled along with how dense their calendar itself is.


During this time the Google Calendar MCP received a full update which severely hampered the efficacy of the attack. It became common that the tool would grab invalid date ranges of calendar events and miss our injection.


We realized that while an injection attack through the user or agent prompt (using this tool-call response approach) was technically possible, it wasn't the most consistent path forward. Instead, we decided to target the system prompt directly. This method offered a more consistent and reliable way to execute the attack across different versions of the model, MCPs, and goose. This is when we decided to pivot.


# The Actual Attack


## goose Shareable Recipes


goose has the concept of[recipes](https://block.github.io/goose/docs/guides/recipes/session-recipes) , "reusable workflows that package extensions, prompts, and settings together. To share proven workflows with your team and reproduce successful results consistently."


We identified the portable nature of recipes as being a risk. Internally goose will decode base64 blobs then load a JSON string from the URL to automatically populate the agent instructions and prompt in the UI. Unfortunately this was being done without much transparency, making it trivial to poison a recipe sent to an unsuspecting user.


Using this feature we theorized we could attempt to slip in a prompt injection that would trigger goose to download and run our infostealer via the developer shell tool-call. To our benefit the recipe appended the prompt content to the system prompt of the goose session meaning that our prompt would have significantly more control over the underlying LLM.


Loading image...


## A Compelling Fish


We decided to take a deeper look into our calendar pretext in hopes to find a more efficient way to send out invites. When reviewing the APIs offered by Google Calendar we noticed that you can include a Google Meet link. We then tested Google Meet settings and found that it was fairly hard to distinguish that a meeting link was external to the company.


If you host a meeting, you'll be notified when attendees from outside your organization join. Curiously, if you join a meeting hosted by another organization you won't see that notification. Lastly we created a slide deck that explains our recipe and shows users how to run it.


## The Second Campaign


Using the shared recipe technique, we embedded a URL pointing to the recipe in the description of the calendar invite. We reused the ASCII smuggling technique within the recipe itself so if the base64 was decoded by a curious user it would not show our hidden instructions / prompt injection. Handicapped by the Google Calendar rate limit of 50 invites a day we began a second round of our campaign.


We had hoped that the included Google Meet link would add legitimacy to the calendar invite. When the scheduled time arrived we joined the meeting and began screen-sharing our slide deck. We ran the meeting like an internal learning session, lastly reaching the demo portion where we walked attendees through running the poisoned recipe. On our server we saw multiple requests come through, and it looked like we had finally caught something!


We began monitoring our controlled environment for any indication that the payload had executed, but never saw anything show up….. What happened?


Turns out a small typo in the prompt injection stopped the infostealer itself from being pulled down and run. While we had a successful phish, we did not see the full execution flow we wanted and opted to continue the operation. At this point given the difficulties we were encountering with the rate limits we opted to pivot to a new vector while continuing to send out calendar invites each day after fixing the bug / typo.


Loading image...


## The Third Campaign


We pivoted for a third time with a more targeted plan. We knew for this third option we wanted a system that would guarantee a hit and we would not have to fight rate limiting so we decided to reach out directly to the goose development team via public channels. We worked under the guise of finding a "bug" in the goose recipe system


We worked under the pretext of encountering a RTL bug within recipes when using Arabic text. Luckily one of our operators is experienced with RTL text, and was able to make the bug look very legitimate.


As part of the debugging workflow the employee clicked and executed the recipe which proceeded to kick off the infostealer.


Loading image...


# The Detection


The detection phase provided valuable insights into how AI-driven attacks manifest across existing security controls. Shortly after the red team initiated their final campaign, our monitoring systems surfaced behavior that warranted deeper investigation. The Detection and Response team (DART) quickly engaged, isolating the affected environment and coordinating with the red team to analyze the event.


This collaboration allowed both teams to trace the complete sequence – from the agent's prompt interaction to the resulting local activity – and assess how well existing detections surfaced the behavior. The findings confirmed that current controls can effectively identify atypical activity but also highlighted opportunities to expand visibility into how AI agents interact with host systems and developer workflows.


The lessons learned during this phase directly influenced subsequent improvements to our monitoring and playbooks.


## Added Detections & Mitigations


### Google Calendar Policy


We changed the Google Calendar policy to only show calendar invites of known external senders, and ones users have interacted with before. Now calendar invites that are external have to be acknowledged in some way by the user for it to show on their primary calendar.


### Transparent Recipes


As part of the remediation process we worked with the goose development team to implement a few new features. Firstly, more transparency was added to recipes when loading in a new recipe by actually[visualising the instructions being loaded](https://github.com/block/goose/pull/3537) .[Zero width character attacks](https://github.com/block/goose/pull/4080) have also been[mitigated by stripping](https://github.com/block/goose/pull/4047) when loaded into the app. Finally we have been developing new ways to detect prompt injection, with our initial version of that work being[merged into goose](https://github.com/block/goose/pull/4237) already.


Loading image...


### Detection Opportunities


Operation Pale Fire helped refine our approach to monitoring and responding to AI-borne threats. We used the findings to strengthen our telemetry, improve correlation between agent behavior and system events, and update our response runbooks to include AI-specific investigation paths.


This collaboration between our Offensive and Defensive teams underscored the value of offensive testing in accelerating defensive maturity. The lessons learned directly informed new detection strategies and response workflows for future engagements.


## Wrapping it up


This operation was a great learning experience for both our developers and cybersecurity security personnel. We were able to perform a handful of attacks specific to coding agents that were relatively novel at the time and effectively stressed the importance of keeping LLM context clean from untrusted information.


goose along with the other LLM coding tools can not effectively isolate untrusted context. We expect this to improve in the future with isolation, better detectors, and underlying model improvements. Coding agents in specific pose a unique risk because of their open nature, and singular context window allowing a context poisoning attack to be particularly powerful. Lastly, being mindful of stripping zero width characters from being digested by the LLMs as these can be completely missed by human review.


# Lessons Learned: The Bigger Picture


Operation Pale Fire reinforced several critical principles for AI security:


## AI Agents Are Different


AI coding agents break a lot of the traditional security model and expectations. Unlike conventional software, AI agents:


- Mix instructions and data in unpredictable ways
- Can be manipulated through natural language anywhere in their context
- Operate as "mystery boxes" with opaque decision-making processes
- Have failure modes that are difficult to predict or test comprehensively


## Defense in Depth Still Matters


No single control would have prevented this attack, but multiple layers of defense can significantly reduce risk:


- Input sanitization catches obvious attacks
- User confirmation adds a human touch point
- Behavioral monitoring detects successful compromises
- Rapid response limits damage


## The Human Element


Like with other areas in security, social engineering is a leading vector of risk. Security awareness training must evolve to address AI-specific threats, helping users recognize when they might be interacting with compromised or malicious AI systems.


## Looking Forward


As AI agents become more powerful and widespread, the security challenges will only grow. The techniques demonstrated in Operation Pale Fire—Unicode smuggling, prompt injection, and AI-targeted social engineering—represent just the beginning of what we expect to see in the wild.


At Block, where possible, we default to an open position of knowledge sharing. By being transparent about both our vulnerabilities and our defensive successes, we hope to help the entire industry build more secure AI systems.


AI security's future lies in hardening the systems around our models as much as improving the models themselves. Operation Pale Fire showed us that with the right combination of technical controls, monitoring, and rapid response, we can harness the power of AI while managing its risks.


## Key Takeaways


For organizations building or deploying AI agents strive to follow[our Gen AI Security Principles](https://engineering.block.xyz/blog/genai-security-principles) :


- Treat AI output like user input: Apply the same validation and sanitization you would to any untrusted data
- Never rely on the AI for access control: Implement authorization checks outside the AI system
- Sanitize all input sources: Any data the AI processes such as documents, emails, and calendar entries can all be sources of prompt injection
- Implement behavioral monitoring: Traditional signature-based detection may miss AI-specific attacks
- Plan for compromise: Have incident response procedures specifically designed for AI system compromises
- Test proactively: Regular red team exercises should include AI-specific attack scenarios


The security landscape is evolving as rapidly as AI technology itself. By staying ahead of threats through proactive testing and transparent sharing of lessons learned, we can build AI systems that are both powerful and secure.


**Contributors:** Josiah Peedikayil, Wes Ring, Justin Engler, Michael Rand, Timothy Bourke, Tomasz Tchorz, Daniel Cook, HS
