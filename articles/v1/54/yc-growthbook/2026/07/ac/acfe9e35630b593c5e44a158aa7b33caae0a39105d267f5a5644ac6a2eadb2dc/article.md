---
schema_version: "1.0.0"
document_id: "acfe9e35630b593c5e44a158aa7b33caae0a39105d267f5a5644ac6a2eadb2dc"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/fin-fixed-the-fake-refund-promises-without-losing-the-upside"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T19:33:57.948442+00:00"
fetched_at: "2026-07-29T19:34:00.096178+00:00"
content_hash: "sha256:f9ca12d23a975212342078ff033c9d9d10a84f6bbfea4c64bb08c392aedae443"
---

# Fin fixed the fake refund promises without losing the upside

A customer opens a support chat with a simple question about a refund. The AI agent, trying to be helpful, replies: "Of course, we're going to process your refund." It sounds perfect. It is also a problem, because no human ever authorized that promise, and the agent essentially made it up.


That moment is at the heart of how Fin, the AI support agent formerly known as Intercom, actually gets better. Pedro Tabacof, Principal Machine Learning Scientist at Fin, has spent three years running the[experiments](https://www.growthbook.io/products/experimentation) that turn a promising demo into a product companies trust. Fin now crosses $100M in ARR, serves more than 10,000 customers, and is Anthropic's own first line of customer support. None of that came from shipping features that looked good. It came from testing everything, especially the things that failed.


[🎧 Listen to the full episode →](https://www.youtube.com/watch?v=c7_SmvRDjI8&list=PLVvetio-SXKJG-zaNdhhrXVTw8vH9G662&index=2)


### Why you cannot unit test an AI agent


Most software teams have a comforting ritual: write the test cases, watch them go green, ship with confidence. Tabacof's first argument is that this ritual quietly breaks the moment you put a large language model in the loop.


"Unit tests, like traditional software[unit tests don't work with AI](https://www.growthbook.io/blog/how-to-a-b-test-ai-a-practical-guide) ," he says. The reason is that the system is non-deterministic and the inputs are effectively infinite. "Even if you try to come up with all sorts of use cases, your end user is always gonna surprise you, and there's always gonna be something new, like a change in the product or the world that's gonna make users ask different questions."


If you cannot enumerate the cases, you cannot pin the behavior down with examples. What you can do is measure it at scale. That is why[A/B testing](https://www.growthbook.io/blog/why-a-b-test-your-ai) , not offline evaluation, is Fin's gold standard for every decision. Offline evals run on maybe one thousand to ten thousand examples. Fin's live experiments pull millions of samples in a few days, which gives them the statistical power to detect very small effects with real confidence, sometimes in a single day.


The result is a culture where testing is the default reflex, not a special occasion. Fin runs one to two dozen experiments concurrently, has run thousands over three years, and A/B tests changes most teams would never bother to check, down to a single comma or period in a prompt. "Whenever we have any kind of question dilemma, we just put it to the test," Tabacof says. They even hired a full-time AI analyst whose only job is improving the experimentation framework itself.


### The experiment that made Fin slower and better


The clearest example of why this matters is an experiment nobody expected to run. Leadership wanted to reduce Fin's latency. The instinct was universal: faster is better, and the literature on e-commerce and search backs it, where even 100 milliseconds moves revenue. But no one at Fin actually knew what latency cost their business.


There was a catch. You cannot easily test reducing latency, because if you could make the agent faster, you would have already done it. So the team did the counterintuitive thing and tested increasing it. They[designed the experiment carefully](https://www.growthbook.io/blog/7-steps-to-better-experiment-design) , raising latency roughly in proportion to the natural distribution so that no single user would feel something was obviously wrong and contaminate the result.


Then the data came back inverted. "Higher latency only saw good stuff happening," Tabacof says. Some users bounced, which was expected. What was not expected was that positive feedback went up. The result was so counterintuitive that his manager forced him to run a confirmatory experiment, check for selection bias, and rule out any metric miscalculation before anyone would believe it.


The best explanation is psychological. When an AI answers a hard question instantly, it reads as a canned macro rather than real thought. A human never replies that fast. A small delay makes the agent look like it is doing the work, more human-like, and people reward that with more positive feedback. Fin still reduced latency in the end, because a slow agent looks bad in a sales demo. But they learned something durable: latency is a confounder. Now, when they run experiments that reduce it, they add a third arm that holds latency constant, so they can isolate the effect they actually care about.


### Turning a hallucinating loser into a shipped winner


The article's title comes from the experiment that best captures Fin's whole philosophy. The team wanted to give the agent more conversation history when it generated answers. It is one of the most obvious ideas in the book: more context, better understanding, better replies.


The first result was genuinely mixed. Positive feedback improved massively. But hallucinations, which Fin measures as a hard guardrail metric using LLM judges, went up too. Worse, some of that new positive feedback appeared to be caused by the hallucinations, like the fake refund promise. As Tabacof puts it, "Fin could say things such as, yeah, of course we're gonna process your refund. This can be a very serious fake promise." It was not common, but it was common enough to register in the metric, and that made the first experiment a failure despite the upside.


Here is where most teams choose between two bad options: ship the feature and accept the hallucinations, or scrap it and lose the gain. Tabacof's team refused both. They went back to the drawing board, used offline analysis to map every new pattern of hallucination that the added context was creating, and addressed those specifically in the prompt. Then they reran the experiment to measure the real effect of the context with the new instructions in place.


The fix cut the positive-feedback gain roughly in half. But that remaining gain was real, and it now came with no increase in hallucinations at all. That was shippable, so they shipped it. Fin has carried a much stronger conversation-history context ever since, without the trust cost.


The lesson generalizes. "For most changes which fail, almost always with the right prompting you can overcome most of the challenges, not all," Tabacof says. A losing experiment is frequently a winning one with a single broken component. The skill is diagnosing which component, then fixing only that.


### The uncomfortable math of a real experimentation program


If all of this sounds like a lot of work to ship one feature, that is the point. Tabacof estimates Fin's experiment win rate at roughly 20 to 30 percent, and he is quick to note they do not even track it formally because defining the denominator is genuinely hard. Most people, he observes, implicitly assume that 90 to 100 percent of their new features are winners. Learning the real number is humbling, and it is exactly what pushes a serious team to test more and claim less.


What makes it work at Fin is that leadership treats experimentation as the cost of[quality](https://www.growthbook.io/blog/why-a-b-testing-is-good-for-your-company-culture) , not a tax on speed. The team's manager, now Chief AI Officer, came from an experimentation background and once sold a company to Optimizely. That protection means the scientists can run losing experiments without fear, because everyone understands that "AI development is inherently very uncertain, very experimental, because you never know how much ground you have covered."


Looking forward, Tabacof expects AI to drive more of the experimentation itself. Claude already launches and analyzes many of Fin's experiments, dredging through data and flagging problems. But he draws a sharp line about where judgment still lives: "We humans this year, we own the strategy but Claude's owning the tactics." The tools will keep getting better at writing the prompt and running the test. Knowing which question is worth asking remains the human's job, and the way you answer it does not change. As he puts it, "decision-making can only essentially be done through experiments."


For product, data, and engineering leaders building on top of non-deterministic models, that is the whole playbook in one line. Ship less on intuition, measure more at scale, and treat every loser as a diagnosis waiting to happen. To see how teams run experiments like these end to end, learn more at growthbook.io.


‍
