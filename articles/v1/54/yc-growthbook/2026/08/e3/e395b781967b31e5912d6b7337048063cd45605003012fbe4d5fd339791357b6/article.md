---
schema_version: "1.0.0"
document_id: "e395b781967b31e5912d6b7337048063cd45605003012fbe4d5fd339791357b6"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/farfetchs-case-for-building-your-own-experimentation-platform"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T08:03:14.163536+00:00"
fetched_at: "2026-08-11T08:03:15.925708+00:00"
content_hash: "sha256:d66a99628aada24f43773ee970c06bbbc436ce520c0888cb455c7efdabbd244e"
---

# Farfetch's case for building your own experimentation platform

Buying an experimentation tool is the easy part. The hard part is admitting when that tool costs more than it teaches you.


[Luis Trindade](https://www.linkedin.com/in/ltrindade/) has watched that math play out from the inside. He joined[Farfetch](https://www.farfetch.com/) twelve years ago as employee 700, just as the luxury marketplace opened its second tech hub in Lisbon. The company grew to 7,000 people worldwide, was acquired by Coupang, and refocused on the marketplace that connects luxury boutiques and brands to customers in markets Amazon has tried and failed to crack. Through all of it, Luis built the experimentation program that now runs a couple hundred experiments a month in low season, on a platform Farfetch built entirely in house.


On[The Experimentation Edge](https://www.growthbook.io/podcast) , Ashley Stirrup asked Luis how that happened and what it cost. The answer is a case study in[build versus buy](https://www.growthbook.io/blog/a-b-testing-platforms-build-vs-buy) , and in the difference between owning a testing tool and running an experimentation program.


[Listen to the full episode](https://www.youtube.com/watch?v=noYsU4RzaFY&t=36s)


## A hybrid setup that stopped making sense


Early Farfetch looked like most companies at hyper growth. Engineering ran tests on a tool it had implemented itself. Marketing used an external vendor because that was the tool it could get access to. Some teams did not know[experimentation](https://www.growthbook.io/products/experimentation) capabilities existed at all.


"How could we make sure that we talked the same language all as a company?" Luis asked. His answer was organizational before it was technical. Farfetch stood up an experimentation center of excellence, but rejected the standard version of one from day one. "Let's wait for somebody to ask us to execute for them and deliver just the results. I never believed on that," he said. The center would enable teams to run their own experiments, not run experiments for them.


On the engineering side, that meant Fabs, the Farfetch A/B testing system. Split engine, setup engine, stats engine, all built internally, layered on top of an omni-tracking system designed to follow a customer across web, app, and even into physical boutiques. For a while, Fabs ran alongside the external vendor that marketing used, a hybrid arrangement many companies would recognize.


Then the bill for the hybrid came due. The injected JavaScript hurt page performance. The results were inconsistent. And the team was already doing rework to pull data back internally for the deep dives the vendor could not provide. "Especially with tools that are based on JavaScript and code injection, they break a lot," Luis said. "They create inconsistent results." For a tech heavy company at Farfetch's maturity level, the external tool had become pure overhead. They phased it out and consolidated everything on Fabs 2.0.


## One door for every experiment


The most distinctive choice in Fabs 2.0 is architectural. Farfetch's feature toggling system is the only entry point for running an experiment at the company. Every test, from any team, goes through it.


This is not a typical[feature flag](https://www.growthbook.io/products/feature-flags) setup with on and off key values. The toggling system is deeply interconnected with Farfetch's segmentation service, benefits service, and user systems, which means teams can define exactly who sees what and when, then connect that definition to a randomized split. It also plugs into the CMS, the recommendation system, and the messaging systems, so marketing and content teams run their own experiments with full control and zero code injection.


The single door did more than clean up the architecture. It gave every team the same language. A single hypothesis template is used across the company. Weekly experimentation clinics, which Luis describes as group therapy sessions, put hypotheses in front of peers to be challenged before they ship. A monthly test and learn session opens the results to everyone from junior developers to C level executives. New joiners upskill just by sitting in the room.


That is where the center of excellence spends its effort now. The central team has shrunk over time while experiment volume keeps growing, and Luis considers that the point. "Practices, for you to be good, you need to practice, practice and repeat," he said. Roughly 80 percent of his time goes to enablement: ceremonies, coaching, and the shared knowledge base of learnings.


## The metric Farfetch refuses to manage by


Ask most experimentation leaders about their win rate and you will get a number. Ask Luis and you get a shrug. "Oh, what is your winning rate? We don't even track it," he said. It exists on a dashboard somewhere. Nobody manages by it.


Instead, Farfetch rebranded the outcome of every experiment, in the tooling itself. The question is not whether a test won or lost. It is whether the team was able to learn from it, yes or no. "A failure is actually a test that was badly set up, wrong metrics, created many biases like sampling biases," Luis said. "That was a failure test. All the other tests are opportunities to learn."


The target that follows is unusual: a learning rate of 100 percent, meaning zero failed tests. A disproven hypothesis is not a failure. It is money the company did not spend on an idea that was not worth pursuing, and teams are encouraged to announce that proudly.


The reframe matters because it changes what people are willing to test. When losing counts against you, teams protect safe hypotheses. When learning is the metric, they bring their riskiest, most interesting ideas to the clinic.


## Two years to beat the market leader


The hardest test of that philosophy came from Luis's own area. Farfetch was paying the world's leading recommendation engine vendor while sitting on a lake of behavioral data the vendor could never fully use. Strategically, the company wanted the dependency gone. So the team built its own engine, called Inspire.


"Surprise, at the beginning, it was completely losing against the world leader of recommendations," Luis said.


A strict fail fast reading says kill it. Farfetch did not, because the vision was strategic rather than tactical. What the team refused to do was run the bet blind. Every additional dollar spent on Inspire had to be justified while the vendor contract was still being paid, so the team ran hundreds of small, quick, directional iterations: A/B tests where volume allowed, quasi experiments where it did not, qualitative insight wherever it sharpened the picture.


"We all have a tool belt of experimental tools that you can use. All of them are valid," Luis said. "We just need to understand the different capabilities of them and their limitations."


Around the halfway mark of what became a two year effort, the tide shifted. Inspire started winning. Farfetch increased its investment, phased out the vendor, and today the engine powers recommendations across the business. Even accounting for full maintenance costs, the economics compensated.


"Strategy is key when we are doing experimentation," Luis said. "But at the same time, we need to do it in multiple and small, quick learning iterations." Keep the vision fixed. Let the iterations decide the path.


## When building is the right call


Luis is not dogmatic about building. His own caveat: "I would not recommend to do this for a company that is not core for them to have a tech team." Farfetch is a product and technology company by DNA, with the maturity to maintain a statistical engine, a tracking layer, and a toggling system as first class products. For a company without that core, the same decision could be a costly mistake.


The honest version of build versus buy is not about features. It is about whether the platform is strategic to your business, whether you have a data advantage to exploit, and whether the[hidden costs of an external tool](https://www.growthbook.io/compare) , in performance, control, and rework, exceed the visible cost of engineering time.


## Steps to take from Farfetch's playbook


1. Audit what your external testing tools actually cost. Count page performance, inconsistent results, and every hour spent pulling your own data back for analysis.
2. Route every experiment through[one entry point](https://docs.growthbook.io/features/basics) . A single door creates a single language, and a single language is what makes learnings transferable between teams.
3. Measure learning rate, not win rate. Define failure narrowly as bad experimental design, and celebrate disproven hypotheses as avoided spend.
4. Give strategic bets a longer clock. Use quick directional iterations to decide whether to keep investing, and reserve fail fast for tactics, not vision.
5. Spend your central team on ceremonies, not execution. Clinics, shared templates, and open review sessions scale further than a service desk ever will.


Ready to take control of your experimentation program?[Start for free](https://www.growthbook.io/get-started) or[book a demo.](https://www.growthbook.io/demo)
