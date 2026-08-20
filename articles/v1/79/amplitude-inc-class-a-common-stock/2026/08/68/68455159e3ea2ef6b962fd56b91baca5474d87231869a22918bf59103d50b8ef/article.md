---
schema_version: "1.0.0"
document_id: "68455159e3ea2ef6b962fd56b91baca5474d87231869a22918bf59103d50b8ef"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/ai-chat-box-tests"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T04:36:09.254434+00:00"
fetched_at: "2026-08-19T04:36:11.590828+00:00"
content_hash: "sha256:be680dff74a3a9d91d9f44be23563c0099dbebe00607c6610e7bf951c27de0a1"
---

# Your AI Chat Box Isn't a Strategy. Where You Put It Is.

It's never been easier to ship a chat box. But simply dropping one onto your site isn’t an AI strategy. The hard part (and what most teams skip) is figuring out where that box will actually benefit users the most. The only way to do that is to test it, monitor what happens, and ask why it worked or didn't.


When it came to determining where[Global Agent](https://amplitude.com/ai-agents) , Amplitude's AI chat experience, should live inside our product, we wanted to be as intentional as possible. We tested it, surface by surface, across both new and existing users to find out exactly how we can meet our users where they are.


Our tests uncovered two undeniable truths:


1. **Prompts only work when they're anchored to user intentions** . It doesn’t matter what your model can do. What matters is what a user wants and whether your chat box can get them there.
2. **Habits are hard to break** . Existing users default to the path that already works for them. The only way we got them to try something new was to make the new thing the *only* thing in front of them for a moment.


Here's an in-depth look at the tests we ran and what we learned from the results.


### **Onboarding: Get new users talking before they know what to ask**


For traditional SaaS products, the standard onboarding process is to show new users the entire product in a guided step-by-step tour. However, in the AI world, users expect more value with less effort. The fastest way to get them to their destination is to give them an empty chat box and let them drive, right?


My teammates[Chethana Kandula](https://www.linkedin.com/in/chethana-kandula/) and[Alex Yoon](https://www.linkedin.com/in/alexandersyoon/) put that theory to the test when they introduced Global Agent to new Amplitude users. Our first onboarding test surfaced the Global Agent chat box at the top of the page and suggested a range of clickable content underneath it.


*Control: Global Agent chat box with feature-focused prompts and suggested content*


They were curious to see if they could use Global Agent to showcase core Amplitude functionalities like charts and[session replay](https://amplitude.com/session-replay) . In the ideal outcome, the test would increase AI adoption by nudging new users to interact with Amplitude through the chat-first experience right away.


Chethana and Alex gave our users a choice: use the AI-powered chat to blaze their own onboarding trail or start with personalized content that showed them common paths similar users took. The key takeaway from that initial test is that new users weren’t ready to go headfirst into AI chat just yet. What helped them discover new features was simply providing a couple of default prompts to click on (i.e., "We have AI features and here are things you can do about it!") and limiting their choices to options we thought were most likely to help.


When it comes to the specific prompts that resonated with onboarding users the most, the familiar, clickable content outperformed both feature-focused prompts and the empty chat box. It turns out, new users trust their teammates (and their activity) to recommend valuable content more than they trust AI. Trust, like the habits I’ll discuss in the next section, is a muscle that builds up over time. It’s hard to break.


The test surfaced another trend worth chasing too: users who asked Global Agent three questions in their first seven days after signup were meaningfully more likely to retain than users who didn't.


In the next version, Chethana and Alex redesigned it to make it easier to start talking to the agent. They changed the default prompt to find out more about a new user’s persona and job-to-be-done. After a user entered their job title, Global Agent served up questions tailored to their specific role. ** By adding that customization to Global Agent, Amplitude converted new users at **2x** the rate of the original version.


*Treatment: customized prompts based on user persona and job-to-be-done*


**Takeaway #1:** Our lift in conversion didn't come from adding AI to onboarding. It came from getting to know the users better and tying the AI’s content suggestions to the user's job-to-be- done. A one-size-fits-all feature tour isn’t enough anymore. AI should help connect your product to something the user already pays attention to.


### **Home: Narrow the front door**


Home was the next test, and the stakes were different. It's high-traffic (the first thing every customer sees on login) but historically low-intent, functioning more like a navigation layer than a shortcut to insights.


Our first test added Global Agent as a small box at the top of the existing Home. Fewer than 10% of customers used it to ask a question. It got lost, buried among dashboards, shortcuts, and everything else users expect. We realized pretty quickly that adding anything new to the page would quickly be a victim of[banner blindness](https://blog.hubspot.com/marketing/banner-blindness) .


*Control: dashboard-first Home*


The next version had to be something drastically different to move the needle. I vibe-coded a new option that stripped almost everything else out of the page. Just the chat box, centered, with a single button for anyone who wanted to return to the old dashboard view. The result was positive and stat-sig: Global Agent engagement rose **55%** with fewer than 3% of users opting back to the old dashboard-focused Home.


*Treatment: chat-first Home*


The chat box wasn’t the problem. Users responded to the chat box. What they didn’t respond to was the way we originally surfaced it. By testing, we were able to increase adoption and change the way users behave when they first land in the product. Moreover, we nudged our users to go deeper into Amplitude. To wrap their head around the questions our product can answer for them. Amplitude has an existing nav for users who want to find the same things they are used to finding. Global Agent helps them find and make entirely new things.


If you want to read more, I wrote more about my Home experiments for the[GrowthDesigners.co](https://growth-designers.beehiiv.com/p/an-a-b-test-that-changed-how-customers-use-ai-part-1) Newsletter.


**Takeaway #2:** Habits are hard to build, and even harder to change. Existing users have strong muscle memory, so they’ll default to the same old usage patterns. To break those habits and develop a new behavior, sometimes it takes a drastic change to move the needle.


*\[Brief, related signal: one of the ways we're trying to reinforce that new habit is by letting users see what similar users are asking Global Agent on Home. The theory is that other users’ prompts are more welcoming than a blank box. The impact of this change is still too early to tell, but it's the next lever we're trying. Stay tuned!\]*


### **Chart and dashboard creation: Show up inside the workflow, not beside it**


Our chart and dashboard creation flows were the trickiest experiments of all because these are the pages where our most engaged users already have a well-worn routine. Research showed many of our users didn’t even notice the small Global Agent entry point hidden at the bottom-right edge of the experience they’re used to.


Global Agent can generate dashboards and charts on its own, often much faster than a user’s traditional process, but the users don’t know that. It was[Jingshu Zhao](https://www.linkedin.com/in/jingshu-zhao-29983822/) ’s job to add that agent into the existing workflows and highlight its capabilities in a way that would expand what our users could do, but not shut them out of the path that they already love.


First, she tested placement: an always-open AI sidebar beat a corner icon by **16.6%** , a statistically significant lift. After rolling it out more broadly, some users complained the sidebar kept reopening after they'd closed it. So she added a guardrail: once a user closes it, it stays closed on return visits.


Next came a harder question: Could Global Agent enter the chart-creation workflow without causing a disruption? Jingshu tested two treatments to find out. One embedded a Global Agent entry point directly into the existing workflow using a prompt that said, “Build this chart with AI.” The other placed a chat box in the page's unused "empty state" space, more visually prominent but off to the side.


*Control: manual chart creation*


Despite the prominent placement, the empty-state version we tested underperformed.


*Treatment 1: replace empty state with chat-first chart creation*


The embedded, in-workflow version reached stat-sig and was adopted.


*Treatment 2: add chat-first chart creation in the existing flow (event config on the left)*


The same pattern held for dashboard creation. Here's our manual dashboard creation screen.


*Control: manual dashboard creation*


Putting Global Agent at the start of the existing flow (above the traditional "start with" options rather than beside them) was the clear winner. Many users had Global Agent generate a full set of starter charts on the spot.


*Treatment: chat-first dashboard creation*


**Takeaway #2 (redux):** Chart/dashboard creation activity mirrors the same habit-formation lesson as the Home page activity, only with more entrenched users. The results say the same thing in two different ways: Global Agent will be adopted if we let it earn a place *in* the existing workflow, not next to it.


### **Experiment with your UX. Trust the data. Raise the bar.**


Since we started these experiments in December 2025, Global Agent usage across Amplitude has increased dramatically. The total number of messages sent is up 13x, and the number of users who have sent at least 1 message via Global Agent per week is up 6.52x. Not because we put a chat bar everywhere, but because we tested where it belonged. We went surface by surface, and considered both new users (who we needed to earn trust from) and existing users (with habits already worth respecting).


If you're deploying a chat interface anywhere in your product, the two questions worth asking first are: “Who would go through this flow?” and “How is the chat interface going to change their behavior in the product?”


It’s important to be clear about your goals and your intended outcome with chatboxes. That’s the only way to create a coherent data tracking plan that will collect the signals you need for your next big idea or rounds of iterations. Amplitude is ideal for teams testing new AI products. It gives you the tools to run experiments, surface behavioral signals that matter, and move from guessing to knowing.


[Try Amplitude Experiment](https://amplitude.com/amplitude-experiment)
