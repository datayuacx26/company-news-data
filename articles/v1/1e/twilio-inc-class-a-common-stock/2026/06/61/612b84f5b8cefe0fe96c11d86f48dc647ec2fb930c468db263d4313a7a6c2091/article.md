---
schema_version: "1.0.0"
document_id: "612b84f5b8cefe0fe96c11d86f48dc647ec2fb930c468db263d4313a7a6c2091"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/events/matt-cortland-guinness-price-index-signal-berlin-2026"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:32dde751b0b41b53128013ab108533f48e977dd7d5fcabd288c46feba965965c"
---

# From Idea to Index: How Twilio Voice and AI Helped Bring the Guinness Price Index to Life

Time to read:


-
-
-
-
-


June 11, 2026


**Written by**[Twilio](https://www.twilio.com/en-us/blog/authors/author.twilio) Twilion


**Reviewed by**[Ina Perkins](https://www.twilio.com/en-us/blog/authors/author.iperkins) Twilion


---


## From Idea to Index: How Twilio Voice and AI Helped Bring the Guinness Price Index to Life


You may have seen the[Guinness Price Index](https://guinndex.co.uk/) making headlines lately. Created by Matt Cortland, the project used AI-powered phone calls to help map the price of a pint across Ireland and the UK - uncovering everything from regional pricing trends to surprising insights into how people respond to Voice AI.


Powered in part by[Twilio’s programmable voice technology](https://twilio.com/voice) , the project has quickly become a standout example of how developers are using AI and communications APIs to build creative, real-world experiences at scale.


Matt is joining us at[Twilio’s SIGNAL Berlin in June](https://signal.twilio.com/berlin26?i=) to share more about the project and the technology behind it. Ahead of the event, we caught up with him to talk about building the Guinness Price Index, what he learned from making thousands of AI-powered calls, and why he believes voice will become one of AI’s most important interfaces.


## Matt Cortland talks to Twilio


### Q: Where did the idea for the Guinness Price Index actually come from?


**Matt:** Just like people talk about the weather, there’s this long-standing cultural habit -


especially across the UK and Ireland - of talking about the price of a pint. We’re always comparing where you can still get one for under a certain price, or reacting when somewhere suddenly feels expensive.


What fascinated me was that Guinness had almost become this informal economic benchmark. In the same way people talk about the cost of eggs, milk, or petrol, people use the price of a pint to illustrate how things are changing around them. So the idea was really to build something that could make that information visible, searchable, and useful at scale.


### Q: Did you always see this as something with broader potential?


**Matt:** I designed it as a consumer transparency tool. The initial data collection was there to kickstart the index, but the bigger vision was that over time it could become something much more comprehensive; a living dataset people could contribute to and use.


What surprised me was the level of resonance it had. I knew I was tapping into something that was relevant but you can never fully predict what will take off culturally, and resonate with the wider public.


### Q: Twilio played a central role in the project. How did the technology help bring it to life, and how much scale were you able to achieve?


**Matt:**


Twilio,[ElevenLabs](https://elevenlabs.io/) , and Anthropic's[Claude Code](https://www.anthropic.com/product/claude-code) were really the three foundational technologies behind the project. Twilio gave me the programmable communications layer I needed, and the API documentation made experimentation incredibly accessible. As ElevenLabs is a Twilio partner, it was a one-stop shop and a really easy integration. And since Twilio works with any LLM, I leveraged Claude.


One thing I found especially valuable was being able to use APIs almost as a collaborative development environment - testing capabilities, understanding workflows, iterating quickly. That accessibility made a huge difference. The whole process became very iterative. I’d send out batches of calls, analyse the responses, adjust prompts or workflows, then repeat. What would traditionally have taken months of manual research became something we could execute extremely quickly.


### Q: Were there any unexpected lessons from running voice AI calls at that scale?


Matt: One surprisingly important factor that I hadn’t anticipated was background noise.


Calls tended to perform better when venues were slightly busy. If there was some ambient sound - glasses, music, people talking - the interaction felt more natural. In completely silent environments, the AI voice could feel a little too perfect or unnatural.


At this scale, you start discovering all kinds of behavioural and environmental patterns you wouldn’t necessarily predict upfront.


### Q: What were some of the technical challenges that you faced along the way?


Matt: IVR systems - “press one for this, press two for that” menus - were one of the biggest blockers - they create genuine friction for AI agents trying to navigate calls autonomously. I ended up building different call-routing paths to work around that.


It was an interesting learning to understand how legacy communications systems are still shaping what AI can and can’t do in the real world. This is a massive hurdle across the board right now. This also aligns with[Twilio’s survey](https://www.twilio.com/en-us/report/Inside-the-Conversational-AI-Revolution?tab=1_3)[which found](https://www.twilio.com/en-us/report/Inside-the-Conversational-AI-Revolution?tab=1_3) that EMEA organisations are nearly twice as likely as those in the US or APAC to name existing infrastructure compatibility as their number one challenge when setting their AI deployment strategy.


### Q: What do you think projects like this show about where Voice AI is heading next?


Matt: Voice will become one of the key interfaces for AI agents.


Right now, AI systems are already very good at researching information online. The next step is enabling them to act in the real world - calling businesses, checking availability, making reservations, and gathering information that isn’t accessible on the web.


That’s where voice becomes incredibly powerful, and where developers have a huge opportunity to build entirely new kinds of experiences around conversational AI and programmable communications.[Twilio’s recent report](https://www.twilio.com/en-us/report/Inside-the-Conversational-AI-Revolution?tab=1_3) suggests that the market is already moving in this direction as while 33% of EMEA organisations are using voice interactions for AI, as many as 50% of EMEA organisations are actually planning for their next-generation AI rollout to deploy on voice channels.


For me, my Guinndex project was an example of what can happen when those tools become accessible. It combined voice AI, APIs, automation, and data analysis to solve a very human, very relatable problem - and I think that’s why people connected with it. From my perspective, Twilio was a core part of making that possible.


## See Matt and Twilio in person


*If you want to see Matt talk about Guinndex in person on June 11, 2026, you can still[register for SIGNAL Berlin here](https://signal.twilio.com/berlin26) .*
