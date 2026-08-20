---
schema_version: "1.0.0"
document_id: "c9f19a93bc0035b719b282d5b1668dbc5b070865ee652628c9fecd23c8a6553d"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-88fd2d772bc9"
canonical_url: "https://hamming.ai/blog/cx-lessons-from-35-years-in-contact-centers-brian-jeppesen"
published_at: "2026-02-18T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:38.132930+00:00"
fetched_at: "2026-07-28T22:19:43.789522+00:00"
content_hash: "sha256:18152196620ba715bd3e9e2d792173f485f40bd64261d256627e0389f39295c3"
---

# CX Lessons from 35 Years in Contact Centers

# CX Lessons from 35 Years in Contact Centers


*This post was adapted from Hamming's podcast conversation with Brian Jeppesen, a veteran contact center leader with over 35 years of experience in customer experience, call center operations, and voice AI deployment.*


Most voice AI conversations focus on the technology. But the operators who actually deploy these systems day-to-day have a different perspective: what matters isn't the model or the platform. It's whether the customer's life got easier.


Brian Jeppesen has been in the contact center space long enough to see every wave: from manual QA and IVR trees to conversational AI and auto QA. His take is refreshingly practical and backed by a Forrester-validated 390% ROI on his voice AI deployment.


**Quick filter:** If you're deploying voice agents in production, struggling with QA at scale, or trying to build an ROI case for automation, this is directly relevant. If you're still evaluating whether voice AI is worth pursuing, Brian's framework for where to start will save you months of trial and error.


## Why Customer Effort Beats NPS


Every CX team tracks metrics. NPS, CSAT, handle time, the dashboards are full of numbers. But Brian argues most of these miss the point.


NPS scores are driven by extremes. Customers respond when they're either thrilled or furious. The vast middle (where most of your customer base lives) stays silent. CSAT has the same problem.


The metric that actually predicts business outcomes? Customer effort.


> Quit trying to delight your customers. Just make it easier for them. A customer is four times more likely to repurchase something from you if it was easy.


The implication for voice AI teams is clear: don't build agents that try to impress. Build agents that remove friction. Find where customers are struggling and eliminate those pain points.


With modern technology, you can now record and analyze 100% of calls to understand sentiment and effort across the entire journey, not just the 1% sample that manual QA covers.


## The Auto QA Problem: Checklists Don't Understand Context


Brian's team recently migrated from manual QA (supervisors listening to a small sample of calls) to automated QA. The experience revealed a fundamental limitation of most auto QA tools.


The problem: most auto QA solutions simply digitize an existing checklist. Did the agent greet the customer? Did they verify identity? Did they offer a resolution? Yes or no.


> Unless you're using a real good LLM to really understand the context of the whole conversation, an auto QA form is just answering yes or no (did they do something). But maybe they weren't supposed to do it on that call. It doesn't understand the context.


A checklist can tell you whether something happened. It can't tell you whether it *should* have happened. An agent who skips the upsell on a call where the customer is frustrated about a billing error isn't failing QA. They're showing good judgment. But a checklist-based system marks it as a miss.


Brian sees the future of QA as experience-focused rather than checklist-focused: understanding the whole interaction from both the agent's and the customer's perspective.


## QA Your Virtual Assistant Like You'd Coach a New Hire


One of Brian's strongest convictions: companies deploy voice agents, see their call volume drop, and assume everything is working. They don't listen to what's actually happening.


> A lot of companies just plug it in and turn it on and think, okay, we turned it on, it must be working, it's good. Look how many fewer calls we're getting. Well, they don't understand what's happening in the background, because they don't listen to it.


His analogy is pointed: you don't hire live agents, put them on the phones, and then never listen to them or coach them. You train them, monitor them, and improve them continuously. Voice AI requires the same discipline.


Brian's QA process for virtual assistants:


1. **Identify where customers are struggling** using AI-generated summaries and sentiment analysis
2. **Listen to the problem calls** to understand what's actually going wrong
3. **Fix the specific issues** :often it's wording that doesn't make sense to a customer, even if it's technically accurate
4. **Follow escalated calls to the human agent** to learn how they handle the same situation
5. **Feed those learnings back** into the virtual assistant's knowledge base


That last step is key. The best training data for your voice agent isn't synthetic but it's how your best human agents handle the exact same calls.


## The Terminology Trap


A recurring theme in Brian's experience: technically correct responses that confuse customers because the wording doesn't match how real people talk.


> We're saying this to the customer and it doesn't make sense. It's not "sold out," it's "your offer isn't available for the date you're looking for." Small wording changes like that make a huge difference.


Engineers build voice agents. But engineers don't always know the nuances of how customers expect to hear information. Brian's advice: work closely with operators and practitioners who understand the terminology customers actually use.


The people who've spent years on the phone know that "your reservation is not found" lands differently than "I'm not seeing a reservation under that name, can you help me look it up?" The words matter, and getting them right requires operational expertise, not just good prompting.


## Start Simple, Then Grow


When Brian first deployed voice AI, the context was dire: coming out of COVID, 50% of calls were being abandoned. Nobody wanted to come back to work in call centers. He didn't need analytics to tell him where the problem was.


The solution was targeted: identify the simplest, highest-volume call types and automate those first.


> I had 40,000 calls that were a very simple call type. Day one, the virtual assistant handled over 80% of those calls. It dropped my abandonment rate to under 10%. My customers who were waiting 45 minutes just to ask "can you transfer me to the restaurant?" Now they could get answered immediately.


The playbook Brian recommends:


1. **Map your call types by complexity and volume.** Find the intersection of high volume and low complexity.
2. **Automate those first.** FAQs, balance inquiries, password resets, transfers. Calls that don't require judgment.
3. **Measure the downstream effects.** Your remaining agents now handle more complex calls. Your abandonment rate drops. Your attrition may drop too.
4. **Gradually expand.** Add more use cases as you learn what works. Don't try to solve complex workflows on day one.


The key insight: don't start with the hardest problem. Start with the most obvious one. Quick wins build confidence and free up resources for harder challenges.


## The ROI That Surprised Everyone


Brian's deployment with Poly AI produced results that even surprised him. When Forrester conducted an independent ROI study, they came back with a number: **390% ROI.**


Brian's initial reaction: "That can't be right." He dug into the data. It was right.


The ROI came from sources he didn't fully anticipate:


- **Direct call avoidance:** ~30% of call volume now handled by the virtual assistant, equivalent to 24 FTEs. But he didn't lay off 24 people. Those were positions he couldn't fill during a labor shortage.
- **Attrition reduction:** Call center attrition dropped by more than 50%. In an industry where 100% annualized attrition is common, this was massive. The simple calls that churned through the bottom 25% of staff were now automated.
- **Training cost savings:** Less turnover means less money spent recruiting and training replacements.
- **Better customer experience:** Remaining agents were the more skilled, knowledgeable staff, giving better experiences on the complex calls that actually needed human judgment.


The critical framing: none of this happened because Brian set out to cut costs. It happened because he set out to make the customer experience better.


> The people who go into it saying "I'm going to do this to cut costs" end up giving bad experiences. And I think it hurts them in the end. If you do it right (to remove friction) you'll get the ROI.


## Why 60% of Callers Still Want a Human


Despite all the progress, Brian estimates that 60% or more of callers immediately try to bypass virtual assistants. The reason isn't that the technology is bad. It's that callers remember when it was.


> Everybody wants AI, but nobody wants a bad experience. They've tried the "agent, agent, agent, zero, zero, how do I get out of this?" because it was bad.


The trust deficit is real, and it's earned. Years of terrible IVRs trained customers to fight automation. Rebuilding that trust requires consistently good experiences over time, possibly a generational shift.


But the technology has improved dramatically:


- Voice quality is now natural enough that callers sometimes don't realize they're speaking to an AI
- Response latency has dropped significantly
- Context understanding with modern LLMs is far better than intent-based systems


Brian's principle: **automate everything you can, but never force people to use automation.** Give customers the option to reach a human at any point. Some calls genuinely need a person. Someone planning a special family event needs personalized help, not a decision tree.


## The Feedback Loop: Learning from Escalations


Brian's most forward-looking idea: use escalated calls as training data for the virtual assistant.


When a customer talks to the virtual assistant and then asks for a human, that's a signal. But the real gold is in what happens next: how does the human agent handle the call the virtual assistant couldn't?


> Follow the escalation to the human agent. Listen to how they handle it. Then take what works and teach it back to the virtual assistant.


This creates a continuous improvement loop:


1. Virtual assistant handles a call
2. Customer escalates to a human
3. Human agent resolves the issue
4. The resolution pattern gets fed back into the virtual assistant
5. Next time, the virtual assistant handles it without escalation


The approach mirrors RLHF (reinforcement learning from human feedback) in practice, even if the implementation is more manual. Your best agents are the model. The question is whether you have the tooling to capture and systematize what they do.


## Advice for CX Leaders Considering Voice AI


Brian's framework for teams evaluating voice AI:


1.


**Start with "why," not "how."** Don't ask "how do we deploy AI?" Ask "why are our customers struggling?" The technology is a means, not an end.


2.


**Follow the customer journey.** Identify where it's hard for customers to do business with you. Those friction points are your deployment targets.


3.


**Pick the low-hanging fruit first.** Simple, high-volume call types that don't require complex reasoning. Prove value before tackling harder problems.


4.


**QA continuously.** Don't deploy and forget. Listen to your virtual assistant's calls. Fix what's broken. Iterate like you would with a new hire.


5.


**Keep humans available.** Automate everything you can, but never force automation on customers who don't want it. The best experiences come from giving people choices.


6.


**Measure experience, not just efficiency.** ROI follows great experiences. Cost-cutting without experience improvement creates a death spiral.


> Don't try to build things really complex and involved when you know that 60% of the people aren't going to use it in the first place. Start with simple stuff, find what works, and then gradually grow and increase from there.


The contact center space is undergoing its biggest transformation in decades. The operators who get it right (the ones who use AI to make things easier rather than cheaper) will pull ahead. Brian's 35 years of pattern recognition says so, and the data backs him up.


[Listen to the full conversation on The Voice Loop](https://www.youtube.com/watch?v=KOFKa-Rc28k)
