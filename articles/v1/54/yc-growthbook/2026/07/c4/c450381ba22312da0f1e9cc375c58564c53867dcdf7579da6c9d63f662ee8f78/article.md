---
schema_version: "1.0.0"
document_id: "c450381ba22312da0f1e9cc375c58564c53867dcdf7579da6c9d63f662ee8f78"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/kargo-shows-how-to-shift-the-mindset-on-losing-experiments"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-23T22:17:24.982400+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:d1de6728cbf3de345928a8d302958a2cd7bd4fe1b3dbff138b3fe0f7662d1ea4"
---

# Kargo shows how to shift the mindset on losing experiments

Most companies say they value experimentation. Far fewer have built a culture where a failed test is treated as a win.[James Falzone](https://www.linkedin.com/in/jamesafalzone/) , Director of Product Management at[Kargo](https://www.kargo.com/) , leads a team that does, and on The Experimentation Edge, he made a case for why that distinction is the whole game.


Kargo engineers technology that helps brands connect with consumers and grow their businesses. Every day, its teams build products across agentic AI, CTV, eCommerce, social, mobile, and its OpenAI integration, giving advertisers new ways to reach audiences across premium media. When you tap a link and a page loads, the ad you see didn't get there by accident. In the split second before the content renders, a real-time auction fires. The publisher signals an opportunity, companies like Kargo bid on behalf of advertisers, and the winner places the ad, all inside roughly a thousand milliseconds. Multiply that by up to 10 billion ad requests a day, across tens of thousands of advertisers and publishers, and you start to see why Falzone says[experimentation](https://www.growthbook.io/blog/what-is-a-b-testing) at Kargo isn't a team you visit. It's embedded in the culture because the scale and constant change of the industry require it.


## A bad result is not a bad experiment


The line that anchors Falzone's philosophy is simple: "There's a difference between a bad result and a bad experiment." A bad result is the market telling you an assumption was wrong. A bad experiment is the one you were too cautious to run.


"If you're not getting those bad results, if you're not failing, are you really trying anything new?" he asked. It's a reframe worth sitting with. Most teams instinctively measure themselves by[hit rate](https://www.growthbook.io/blog/experimentation-program-mistakes-to-avoid) , the percentage of experiments that succeed. But a high hit rate often means a team is only testing the safe, obvious bets. The genuinely new ideas, the ones that could move the business, are exactly the ones most likely to fail at first. A perfect record isn't a sign of great experimentation. It's a sign you stopped experimenting.


Falzone is candid that his team probably learns more when it fails than when it wins. That isn't a consolation prize. It's the operating model.


## The experiment that failed, and why


The clearest example came from Kargo's own bidding strategy. The team built a click optimization model: predict the likelihood of a click within five milliseconds, then adjust the real-time bid accordingly. For Kargo's direct demand, the advertisers who come straight to the company, it worked beautifully. The improvements were real and substantial.


So the team did what looked like an obvious next step. They took the same model, the same tech, the same rationale, and applied it to their third-party demand, where another party taps Kargo's inventory on an advertiser's behalf. "And we failed," Falzone said. "It did not work. Bad results across the board."


The instructive part is the diagnosis. "The bad results didn't come from technical implementation," he explained. "They came from a lack of contextual implementation." The model was sound. The context was wrong. With first-party demand, Kargo has rich visibility: the geographies a customer wants to serve, the audiences they're targeting, the dates, the budget. Third-party demand is full of idiosyncrasies, and crucially, it isn't just customers reacting; it's their models interacting with Kargo's models. Different segments react to different data and different inventory. What a customer says they want and what their models actually optimize for are not always the same thing.


The fix wasn't a better algorithm in the abstract. It was going back to the drawing board to customize the model around the verticalization of those customers, and to make sure the metrics and signals being tested were business-driven rather than borrowed. Eventually, the team recovered the 25 to 40% performance gains they'd been chasing. But the takeaway outlived the project: a result that works in one context is a hypothesis everywhere else, not a conclusion.


## Putting failure on the agenda


Knowing that failure is valuable is one thing. Building a team that acts on it is another. Kargo's mechanism is refreshingly concrete. In every biweekly retro, there's a dedicated section: "Where did you fail?" Everyone takes a turn. "I tried this, it didn't work." Out loud, on the agenda, by design.


The effect is cultural, not procedural. When failure is a scheduled topic instead of a quiet embarrassment, three things happen at once. Losses get normalized, which is the precondition for taking real swings. One person's dead end becomes the whole team's shortcut. And the psychological safety that every leadership book talks about stops being aspirational and becomes a recurring calendar item.


Falzone is also honest about a tension he hasn't fully resolved. He believes deeply that a single person should be able to run an experiment without needing a committee, that autonomy and speed and a startup mentality matter. But he equally believes the best ideas come from bouncing ideas off other people. "You're a person first," he said, not a job title, and the best ideas can come from outside your function. Holding both individual speed and collective creativity is an ongoing balancing act rather than a solved problem. That he names it openly is part of the same culture that schedules failure into the retro.


## Better, not bigger


Asked where experimentation goes next, Falzone did what almost everyone does now and brought up AI. But his framing was specific. AI's real unlock, he argued, is access. People who previously couldn't read the code or understand the system can now form opinions and run experiments, whether that's generating a SQL query or interrogating data they couldn't reach before. More people at the table mean more ideas and more experimentation opportunities.


He's also clear-eyed about the limits. In ad tech's latency world, where everything has to resolve inside a second, large language models simply aren't fast enough to sit in the live auction path. But they can operate at an orchestration layer, and Kargo is already thinking about[agents that run tests on the team's behalf](https://www.growthbook.io/insights/run-experiments-ai-agents-workflows) . The caution underneath the optimism is the part worth keeping: AI can't be treated as a magic bullet, because everything still has to be built on solid ML and infrastructure engineering.


His closing phrase captured the whole conversation. "Better, not bigger." More compute, more data, and more agents don't matter if they're pointed at the wrong thing. What compounds is a team that runs new experiments, welcomes the bad results, talks about them openly, and rebuilds with context. In an industry where a single mistake can burn an entire campaign budget in under a minute, that discipline isn't a nicety. It's the edge.


‍
