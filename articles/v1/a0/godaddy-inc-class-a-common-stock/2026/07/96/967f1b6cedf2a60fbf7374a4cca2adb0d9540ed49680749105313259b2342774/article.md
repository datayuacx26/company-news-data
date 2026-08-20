---
schema_version: "1.0.0"
document_id: "967f1b6cedf2a60fbf7374a4cca2adb0d9540ed49680749105313259b2342774"
company_key: "godaddy-inc-class-a-common-stock"
company: "GoDaddy Inc."
source_id: "godaddy-inc-class-a-common-stock-news-import-cf537cccbea7"
canonical_url: "https://www.godaddy.com/resources/news/ai-adoption-in-applied-ml-a-human-centric-report"
published_at: "2026-07-03T05:01:00+00:00"
first_seen_at: "2026-07-21T21:50:09.149998+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:31d4058e7a84e9341cb58f5b24e06bc2aa9fcc34795fb10d3ed8688dfa2b0133"
---

# AI Adoption in Applied ML – A Human-centric Report

## Key takeaways


- Increased velocity shifts the bottleneck from writing code to reviewing it, and teams that don't build judgment to match their output speed will drown in what they generate.
- Professional identity resistance, not tool skepticism, is the hardest barrier to AI adoption on technical teams.
- The real unlock is self-sufficiency, but only the 20% of power users willing to stay skeptical of AI output will actually develop it.


I manage a team of Machine Learning Scientists and Engineers at GoDaddy, building deep learning models as well as LLM-powered solutions. Though our team has been using AI tools for the last couple of years, we really doubled down on fundamentally changing our ways of working since January 2026. This report shares learnings from our journey, covering the human side of the lessons rather than the technical ones, which there are plenty of on the internet. By reading this human-written, AI-assisted report, I hope other leaders managing similar technical teams will find actionable insights for their own AI adoption journey.


## Barriers to adoption


The following sections discuss the two biggest barriers my team faced when adopting AI tools: outdated thinking and fear of loss of professional identity.


### Outdated mental models


Before I laid down a strict AI usage policy for the team, I did some casual probing on usage. Most of it was based on early Copilot models from early-to-mid 2024, and the team were using them to generate documentation and test cases. As I nudged the team towards more advanced tools like Claude - which we now use as the default - I quickly found that team members were still reporting concerns that have largely been fixed. The first step was to get the team to use Claude everyday and challenge them to use it for all parts of their regular work including modelling.


The tool migration itself revealed how sticky mental models can be. Several engineers who had been productive in one tool resisted switching, not because the new tool was worse, but because they had already formed habits around the old one's limitations. One engineer described the shift as "less like an incremental tooling upgrade and more like working with a new programming interface - one that comes with a fundamentally different set of rules." The team had to unlearn patterns like writing detailed step-by-step prompts (useful for earlier models) and instead learn to provide rich context through project files and let the model figure out the steps.


### Identity and professional pride


Within a couple of months, the team self-reported their usage and it showed a remarkable shift in the variety of tasks done using AI, except for one conspicuous gap - they still weren't using it to do their core work of modelling. Their mental model was updated enough that they trusted Claude to do much more than the unit tests, but they were unwilling to let it do the work that was most meaningful to them.


This resistance ran deeper than skepticism about tool quality. For ML scientists, modelling is the craft they trained for years to master. Letting AI do it felt like ceding the part of the job that made them specialists. One team member described the tension: "AI can help you build projects fast, but at the end you are still in control of WHAT to build. That's where all the expertise is needed." Another framed their role shift as moving from implementer to "orchestrator and reviewer - defining the what and why, while delegating the how." These reframings didn't come naturally. They required honest conversations about the difference between the value of a task and the value of the person performing it.


We overcame this through real-life examples, where AI-assisted modelling produced results that the scientist then improved with their domain judgment - demonstrating that the human contribution had shifted, not diminished, and reassurances that any one task does not define the entire value someone brings to the team. But these had to be backed by visible proof that their judgment still mattered in the final output.


## New bottlenecks


After addressing the team's biggest barriers to fully adopting AI, we were off to the races. Output increased dramatically, but the increased velocity led to new bottlenecks. Fortunately we were able to provide solutions to most of these new problems, which I'll discuss in the following sections.


### Unmanageable reviews


With self-assurances and external motivations, most of the team fully embraced use of Claude. They were now using it for data analysis, modelling, design documentation, testing and naturally for scaffolding work. With generation of code and text becoming easy, a new challenge emerged - how to review it all. One engineer captured the fatigue: "When you're constantly reviewing so many lines for small mistakes, it's easy to get tired and just accept what Claude wrote verbatim." Another described the sheer scale: "I've never been in a position before where I go to clean up a PR and find thousands of lines I didn't really know about, and 100% of them are soft-spots."


What helped here is to clarify two things:


1. Humans are still responsible for the final output
2. Not all code needs to be inspected by hand


With this clarity, the team approached the review process with a different lens than before. They leveraged AI to review peer work, but focusing on the core idea and the execution output. And where long-term maintainability was important, they still reviewed the generated code. They used` plan` mode more often to break-down a large task into manageable chunks, carefully reviewing the code before merging, which further reduced downstream review overhead.


For our geographically distributed team, the bottleneck shifted further - no longer writing code, but getting a human to review it across timezones. As one put it: "If I finish a PR at 11am, I won't get feedback for at least 24 hours just due to time differences." The constraint moved from authoring to waiting.


### Overwhelming verbosity


Despite the many attempts to manage the firehose of AI generated content, an underlying feature of the models still proves challenging - verbosity. As code and documentation were no longer typed by-hand, the limit for what is considered to be verbose blew up. Engineers and Scientists were passing around multiple page markdown files to each other for review, complaining each time about the review overhead! We managed to circumvent the verbosity challenge through better context engineering and instructions in` CLAUDE.md` files as well as creating custom skills, like the` /rewrite-for-humans` which takes any content and rewrites it in a "what, why, how" structure.


### Borrowed cognition


The verbose output and the review burden were further exacerbated by a deeper issue: the team hadn't generated any of it themselves. When you review code you wrote, you know where the soft-spots are. When AI writes it, everything is a soft-spot. This surfaced as something more than review fatigue - it became a question of cognitive ownership. One engineer noticed: "My ability to articulate the context of a task is getting worse." Another reflected: "Even a small type check has been delegated. This is going to impact human cognition." A third raised the generational dimension: "For those of us early in our careers, how do we develop the skill to call out good responses from bad?"


The concern isn't hypothetical. When AI handles the mechanical work - the part that used to build intuition through repetition - the feedback loop that develops judgment gets shorter. Senior engineers can fall back on years of accumulated pattern recognition. Junior engineers don't yet have that reservoir. This is, as yet, our most significant unresolved challenge. We have gotten far enough to realize two things: first, the time freed up by AI productivity can be deliberately redirected toward keeping core skills sharp. Second, how we assess proficiency must change - if AI writes the code, the skill being tested is no longer "can you write it?" but "can you judge it, improve it, and know when it's wrong?"


Some promising practices have emerged. One team member appends "why I'm wrong" to every AI response, forcing himself to remain skeptical of its output. Another cross-checks Claude with a second model to reduce confirmation bias. Neither is a complete answer, but both signal that the team is actively experimenting rather than passively accepting.


The open questions remain: If AI writes and AI reviews, what is left for us? And how do we build the next generation of engineers when the scaffolding they used to climb on has been automated away?


## Expectations and reality


With the bottlenecks identified and partially addressed, I stepped back to look at how adoption was actually playing out across the team. The picture was more uneven than I expected.


### Uneven usage


This should not surprise anyone who has managed teams through technology adoption. Despite the clear and obvious utility of AI, the usage has been anything but uniformly deep. Even after five months of continuous usage, our team is roughly made up of 20% power users, 40% normal users and a persistent 40% that are still beginners. The Pareto Principle strikes harder when it comes to token usage. By my estimate, the power users account for more than 90% of all token usage within my team, and they are pulling away from others in not only being more productive but also in their ability to manage AI induced chaos.


### Blurred boundaries and effort estimates


Within this group of power users, a new pattern is emerging. They no longer see themselves as just ML scientists or Engineers. They leverage AI to do tasks that are adjacent to their role. We now routinely see a UI mockup or a simple web app representing an idea or an analysis to be reviewed. Tasks that would have been perceived as dependencies are now just an MCP server in their Claude configuration. But because this advancement is not uniform, task assignment and estimates to complete those tasks vary wildly. It is not apparent if an estimate is uncalibrated for AI usage or it includes additional items.


## Path forward


After five months of relentless focus and sustained usage, it has become apparent to me that the biggest unlock from AI tools is increased self-sufficiency. With the right taste and purposeful agency, a small team can go beyond what their job title says. We now routinely see an engineer design and run a full statistical experiment that previously would have been a dependency on another team, or a scientist build a complete data pipeline end-to-end without waiting on infrastructure support. But this reach comes with a cost that we are still learning to pay. It requires decoupling self-worth from any singular task. It requires honest conversations about what "expertise" means when the mechanical execution is handled for you. And it requires acknowledging that the borrowed cognition problem doesn't disappear with adoption - it deepens. The teams that thrive won't be the ones that generate the most code. They'll be the ones that maintain judgment while moving fast; who can tell the difference between velocity and progress. We aren't there yet, but we're asking the right questions.


Photo by[Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on[Unsplash](https://unsplash.com/photos/brown-wooden-puzzle-game-board-D44kHt8Ex14?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
