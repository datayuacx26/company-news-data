---
schema_version: "1.0.0"
document_id: "179ae69c720c85daca6266fcaae34d51aab54208b2ac733c66aa878d9d703802"
company_key: "yc-motion"
company: "Motion"
source_id: "yc-motion-rss-f628c3046538"
canonical_url: "https://engineering.usemotion.com/why-i-never-want-another-standup-f858238b4bed"
published_at: "2025-10-28T15:02:21+00:00"
first_seen_at: "2026-07-20T23:21:05.244128+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:beb2a5d8a3c9fb7966bf63b1d9b27f8a4e64dde9592f41d270c48add1496e04b"
---

# Why I Never Want Another Standup

# Why I Never Want Another Standup


[Scott Walters](https://medium.com/@scowalt?source=post_page---byline--f858238b4bed---------------------------------------)


6 min read


·


Oct 28, 2025


--


> After you finish the first 90% of a project, you have to finish the other 90%.


— Michael Abrash


When Motion first started, we were an execution machine. Surveying other YC startups, we would routinely trounce them in terms of pull request velocity, feature launches, and virtually every DORA metric.


Over time, things started to change. Releases became quite complicated, and new hires would often forget critical steps — “Make sure to turn on early adopters at least one day before 5%!” or “Did you forget the datadog dashboard?”


When we built[Project Workflow Templates](https://www.usemotion.com/blog/project-workflow-templates) , our very first internal use case was our release process. Motion would automatically assign the correct tasks to the stakeholders, remember blockers, and only prompt you to do specific tasks when ready. This gave us a nice, repeatable standard operating procedure. As long as everyone followed the tasks they were assigned, even a new hire on day one could launch a feature successfully to production!


Press enter or click to view image in full size


Our old Waterfall SOP


Immediately, our correctness in executing these long, complicated launches improved. But something curious happened. Over time, launches became even *slower* . Despite our best intentions, we had fallen into a very 1970’s waterfall style of company. Sure, launches were technically ‘on time,’ but the intent behind product’s requests was lost. Users didn’t really love what we were shipping. And engineers felt like they had less ownership than ever before. They had become glorified factory workers: handed a spec and told to implement. Worst of all, Product and Engineering never truly met in an ongoing basis to hash out the problems, so implicit trade-offs were constantly being made.


At the time, the solution seemed simple enough; just have the product and engineering leads meet up on a weekly or daily basis. If only that worked — it would have been the first time in history that a truly substantive problem was solved with a meeting.


Instead, we were now in a zombified waterfall-agile hybrid.


Agile had its time and place in history. If we rewind back to the year 2000, it was a reaction against many of the legitimate problems brought up by Waterfall. Why were stakeholders only getting status updates in between each stage? Can’t we coordinate and cooperate faster?


But agile is a product of its time. In 2001, when the Agile Manifesto was first released, there was no concept of Continuous Integration or Continuous Delivery. The dominant version control system was CVS, and the widely held wisdom at the time was to simply have developers work on different files to avoid merge conflicts. Agile promised to shorten delivery cycles from quarters to weeks! And at the time, it was an extremely appealing promise.


But two and a half decades have passed, and as an industry, we can do better.


## Enter ShapeUp


After growing frustrated with the state of Motion’s product development, and in May of 2025, we introduced Basecamp’s[Shape Up](https://basecamp.com/shapeup/shape-up.pdf) to Motion.


Developed and released in 2019, Shape Up is a refreshingly modern take on how software teams can ship high quality software that users love without useless ceremony (“how many points is that task again?”). As Basecamp themselves mention, it’s meant to be a starting template from which you pick and choose the parts that work for your organization. I transformed our old Waterfall development process to the following:


Press enter or click to view image in full size


Our new, more flexible Shape Up SOP


Instead of a single, massive project template, we have 4 discrete phases. The first is the shaping. The most important aspect is to **get engineering into the room in the earliest stages of product development — even before there is a design** . Engineering can inform product the rough constraints of the current system and can help steer the conversation away from technical tar pits. On the flip side, the product team can help impart onto the engineers the actual problem they’re trying to solve, the motivations of the users, and the design philosophies.


## Get Scott Walters’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


While nearly every product engineer worth her salt will want to attend, we found that keeping shaping sessions to the bare minimum number of people was ideal. Too many people ultimately leaves no oxygen in the room for creative problem solving. For us, this meant having the designer, the engineering manager, the product manager, and the tech lead.


We found this single change to have the single largest impact on our organization. In the first three weeks of implementing Shape Up, we actually *canceled* two full features! This is a feature, not a bug. In the old world, we would have spent weeks implementing and launching these features, only to wonder later why users didn’t get any value.


Shaping sessions happen *before* design for a reason: they should start with the user problem, not the solution. It’s tempting to fill the blank page right away, but resist that urge. The first few sessions might feel awkwardly quiet. That negative space is where your best ideas can emerge.


In many parts of our stack, one or two engineers held deep, domain-specific knowledge that could reshape a project. To capture that early, we will occasionally do a short technical design review right after shaping. Sometimes, insights from the technical design document (TDD) would send us back for another shaping round. And that’s a good thing! It’s far better to uncover ambiguity upfront than in the middle of development.


After the TDD is approved and the team aligns on a rough design, implementation starts. We don’t over-specify the details. We trust our engineers and designers handle things like border radius, shading, or icon choice. Once the rough Figma is aligned, we move fast.


The key when choosing implementations milestones is to make them full-stack functionality for the user; this way, your product is getting better with every block. It does not matter how small the slice is, as long as it’s functional.


Press enter or click to view image in full size


An incorrect slice — only frontend changes Press enter or click to view image in full size


Another incorrect slice — the frontend and backend didn’t implement the same part Press enter or click to view image in full size


A correct slice


We were surprised how much value even the smallest slice could deliver. Suddenly, the urgent pressures of future milestones would lessen or even disappear entirely.


We’re a small startup, and shifting priorities is just the name of the game. Having a development process that leans into it instead of pretending it can’t happen has allowed us to minimize chaos, address tech debt, and deliver value in a much more predictable cadence.


## Motion’s ShapeUp


After several months, we adapted Shape Up to fit Motion’s unique rhythm. There were some aspects of Basecamp’s methodologies that we didn’t find particularly useful.


1. For Basecamp, the entire company is on the same 8 week cadence (6 week cycle + 2 week cooldown). At Motion, we just have too many features with different levels of difficulties, priorities, etc. Some feature teams desperately need a 2 week urgent high priority feature while some infrastructure teams are perpetually on 6 week long term build outs.
2. Speaking of infra teams, we’ve decided that shape up doesn’t really work for non-feature teams at Motion. The feature teams were feeling the pain of shifting deadlines, delayed releases, and mounting tech debt the hardest, so it makes sense that they have the most to gain. Infra teams are mostly shielded from the pressures of the business, so they operate with more autonomy. The formality of shaping sessions was adding more overhead than benefit.
3. Lastly, we haven’t abandoned our project workflow templates! We still follow the standard release process (starting from Launch Readiness onwards) to make sure we correctly release. Even projects still in shaping or in early development benefit from following our standard operating procedures.


Shape Up didn’t just help us ship faster. It helped us ship *better* , and rediscover the joy of building.
