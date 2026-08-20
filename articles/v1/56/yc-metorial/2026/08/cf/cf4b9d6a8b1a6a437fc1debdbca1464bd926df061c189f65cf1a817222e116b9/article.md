---
schema_version: "1.0.0"
document_id: "cf4b9d6a8b1a6a437fc1debdbca1464bd926df061c189f65cf1a817222e116b9"
company_key: "yc-metorial"
company: "Metorial"
source_id: "yc-metorial-news-import-9c457a3d81d4"
canonical_url: "https://metorial.com/blog/open-sourcing-the-yc-application"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-17T12:31:38.733405+00:00"
fetched_at: "2026-08-17T12:31:39.635745+00:00"
content_hash: "sha256:426d20691778f05ac8be90b8924f99a3afaa68bd6957aaca8eed8f8b7b4525e7"
---

# Open Sourcing the YC Application

TL;DR


We applied to Y Combinator twice with Metorial. Both applications received interviews; the first was rejected, and the second got us into F25. Between them, we made real progress, and we learned to describe it more clearly. Below are both applications and my best attempt to explain what changed.


Most successful companies exist because someone was willing to make a bet. Founders bet years of their lives, years they can never reclaim, against what often appear to be absurd odds. Pre-seed and seed investors bet on founders long before they can know whether the product will find a market, let alone return their fund.


To me, this creates something close to a moral imperative: to pay that faith forward. The faith cannot always be repaid to the person who first offered it, at least not in the same currency. It can be extended to early employees, to other founders, and to people who have not yet been given the chance to prove themselves, and who may eventually place bets of their own.


As a technical founder, the most natural ways I know to repay this trust are to open-source what I build and write down what I learn. YC encourages much the same instinct inside its community. I have reviewed other founders' applications and recommended people I genuinely believed YC should fund. Sharing this is another small attempt at doing the same.


We interviewed for S25 and were rejected. A few months later, we applied again and got into F25. I have redacted customer names; otherwise, the answers below appear as we submitted them, including the occasional typo. For completeness, I have included every written field. Where an answer was effectively unchanged, I show it once; where the difference seems meaningful, I show both.


After our first interview, we received this:


> Hi Karim and Tobias,
>
>
> Unfortunately, we've decided not to fund Metorial for this batch. You seem like a tight-knit team. We do like funding open-source tools, and the recent growth of your project is impressive. However, it wasn't clear to us how you'd differentiate among the many MCP projects that have come up. Having a single-line integration may be a good start, but we wish we had heard more about how this could become a business that is defensible.
>
>
> That said, we are far from perfect in our decision-making and are often wrong. We encourage you to keep pushing forward and consider applying again to the next YC batch.
>
>
> Thanks for taking the time to chat with us today. We wish you the very best and hope you'll stay in touch.


## A point and a slope


One way I have come to think about reapplying is as a point and a slope.


A first application gives YC a point. It shows where the company stood on a particular day: the product, the users, the founders, and whatever evidence they had accumulated. From that one point, the partners have to imagine the direction and speed of everything that might follow.


A second application gives them another point. Now they can calculate a slope. That second point contains more than another description of the company. It shows what the founders did with time, evidence, and criticism. Paul Graham's word for the quality investors seek in founders is "formidable." I do not think formidability is a tone of voice, or something that can be manufactured by writing with greater certainty. A reapplication can show it more honestly: an obstacle was identified, some months passed, and the result changed.


Of course, the metaphor cuts both ways. If little has changed, the second point makes that visible too. The useful question is not only whether to apply again, but whether there is now a slope worth showing.


## The founders


**How long have the founders known one another, and how did you meet?**


**S25**


> Tobias and Wen have known each other for 10 years now, starting as seatmates at a top technical high school in Austria. They worked on many projects together over those 5 years, and Tobias later joined Wen to co-lead the technical development of Valicit, a ticketing startup based in Abu Dhabi.


**F25**


> We have known each other for 10+ years. We met in a top technical high school in Austria where we worked on many projects together. Later, we co-lead the technical development at Valicit, a ticketing startup based in Abu Dhabi.


**Who writes code or does other technical work on the product?**


> We both write code and do technical work. Everything has been developed by us.


**Are you looking for a cofounder?**


> No.


## Company details


Field S25 F25


Company name Metorial Metorial


Company URL[https://metorial.com](https://metorial.com/)[https://metorial.com](https://metorial.com/)


Product URL[https://metorial.com](https://metorial.com/)[https://app.metorial.com](https://app.metorial.com/)


Current location / location after YC Abu Dhabi, UAE / Linz, Austria / San Fransisco, USA Linz, Austria / San Francisco, USA


**Explain your decision regarding location.**


> San Francisco is the place to be for connecting with and understanding AI developers and founders. Having lived in New York and Abu Dhabi and spent time in San Francisco, Wen embraces Paul Graham's insight that "in a hundred subtle ways, the city sends you a message: you could do more; you should try harder."


**What changed since the previous application?**


**S25**


> n/a


**F25**


> Last batch we applied with the same idea and received an interview. One piece of feedback we received was that our moat was not clear. We have since shifted our focus from the ease of integration to being a full-blown integration platform with full developer tooling that can expand beyond just MCP integrations. This led to enterprise customers like the Head of AI at \[a pharmaceutical company\] to reach out to us personally for an enterprise plan. Since our last interview, we have also grown from 700 to 2.6k GitHub stars. We improved our MCP runtime and added comprehensive docs as well as TypeScript and Python SDKs. Currently, we are rolling out the product to our 350 early access users to receive feedback and iterate on it.


Looking back, what I like about this answer is that we did not argue that YC had misunderstood us. We repeated the concern plainly and described what had changed: the product had expanded beyond ease of integration, open-source adoption had continued, a serious enterprise buyer had appeared, and hundreds of developers were waiting to use the product.


Had we simply insisted that our moat was strong, I doubt it would have helped. The more useful answer was what we had built after someone questioned it, and what had happened because we built it.


## How far along are you, and is anyone using it?


**S25: How far along are you?**


> We built a no-code drag-and-drop AI workflow builder and had 100 early access signups within a week of opening up our beta launch. We then pivoted to the current MCP product. We open-sourced our MCP container project, landing on the front page of Hacker News and getting over 700 GitHub stars in a week. Now we are about 1-2 weeks away from releasing our MCP product and getting our first paying customers.


**F25: How far along are you?**


> We currently have 350-developers signed up for our early access. Our open-source repos have organically earned 2.6K stars, and we're now piloting an enterprise plan with \[a pharmaceutical company\]. Next up: a full public launch and closing more enterprise contracts.


The contrast here is almost embarrassingly simple. The first answer spends much of its limited space on a product we no longer pursued. What remains is a forecast: we were close to releasing the new product and expected customers shortly afterward.


The second answer is mostly about the present. There were 350 developers, 2,600 stars, and an enterprise buyer. It still contains plans, but only after establishing what already existed.


**How long have each of you been working on this? How much of that has been full-time?**


**S25**


> Tobias has been working on Metorial for over a year. He has been working full-time on Metorial for the past 2 months. Wen joined him half a year ago. He was working part-time on Metorial while finishing up his Bachelor's degree and will be working full-time starting next week.


**F25**


> We have both been working on Metorial full-time for around half a year.


**What tech stack are you using, or planning to use, to build this product? Include AI models and AI coding tools you use.**


**S25**


> We have built a service oriented architecture mostly written in Typescript and run using Bun. We have built our own Docker-powered MCP server runner/orchestrator, we basically built our own small serverless cloud provider. All of this makes our service super scalable yet cheap to run at scale. We're using OpenAI models for some convenience features and for automatically inferring information about open source MCP servers. We're big believers in AI-based coding tools and are using GitHub Copilot on a daily basis.


**F25**


> We use a service-oriented architecture written in Go and TypeScript, running on Bun. We built our own Docker-powered MCP server orchestrator that acts as a lightweight, serverless cloud provider. This design makes our service highly scalable and cost-efficient. We leverage OpenAI models for convenience features and to infer metadata about open-source MCP servers. We rely on GitHub Copilot daily for code productivity.


**S25: Are people using your product?**


> No.


**When will you have a version people can use?**


> June 1st.


**F25: Are people using your product?**


> Yes.


**How many active users or customers do you have?**


> We are currently in the process rolling out access to the 350+ developers signed up on our early access list so we can iterate on their feedback to improve our product. We are also piloting with the developer teams of enterprise customers, like \[a pharmaceutical company\]. Update: We rolled out access to a few people and already got our first paying customer.


We added the final update after submitting the application. YC applications remain editable, so when something consequential happened while ours was still being considered, we added it.


**Do you have revenue?**


> No.


I then forgot to update this answer after the first buyer. Evidently, the contradiction did not kill the application.


## What do you make?


**S25, in 50 characters**


> AI integrations in a single line of code


**F25, in 50 characters**


> The MCP integration platform for agentic AI.


The first answer advertises convenience without making the product easy to place. It could describe a library, a feature, or a code snippet. More importantly, it frames the entire company around "a single line of code", almost exactly the language YC returned to when questioning whether the product had a moat.


In retrospect, we had supplied the frame through which they rejected us.


The second answer names the category first. It is still not perfect. But a reader familiar with MCP could at least identify which part of the stack we intended to own.


The longer descriptions show the same difference.


**S25**


> Metorial is a developer-first AI integration platform that lets you embed external data and tools into your applications with a single SDK call. Under the hood, we spin up a fully managed, serverless MCP runtime that handles authentication, caching, monitoring, security, and prompt-level audit logs, so you never write or maintain connector boilerplate. We bridge any open-source or proprietary MCP server under one unified API. At scale, we optimize for sub-second cold starts and thousands of concurrent agent calls.


**F25**


> Metorial is a developer-first integration platform that connects AI agents to external APIs and data sources through MCP. We offer one-click deployments for 400+ MCP servers. Our proprietary, serverless MCP runtime is optimized for sub-second cold starts and enterprise-grade concurrency. JavaScript and Python SDKs allow for integration in minutes, and built-in developer tooling like monitoring, logging, security, and analytics comes out of the box. We are open source.


The first version moves quickly into the details: SDK calls, caching, runtimes, audit logs, and cold starts. All of those details were true, but the reader had to assemble them into a product before they could decide whether the product mattered.


The second version establishes the job first: connecting AI agents to external APIs and data. It makes the scope tangible, then explains the infrastructure underneath it. After one reading, someone has a better chance of repeating what we sell.


I used to think technical precision meant including the implementation details as early as possible. Here it had the opposite effect: the components obscured the company. The order I now aim for is simpler. What is the product? Who is it for? What does it make possible? Only then: why is the implementation difficult or unusual?


The writing improved, but the product had also become broader. The second description did not merely present the same company more persuasively. It described more company.


## Why did you pick this idea?


**S25**


> We kicked off building a no-code AI agent builder and soon ran headlong into the same integration nightmare every AI developer hits, weeks spent wiring auth flows, retries, and context handling. It became clear that MCP was built for end-users to hook AI tools into external platforms on their own machines, not for product teams embedding integrations in their apps. We then open-sourced our MCP container product and received great feedback from the community with landing on the Hacker News front page and getting over 700 GitHub stars in a week. Moreover, we had a partner at \[venture capital\] reach out to us personally and telling us that he believes that we are currently building the best MCP solution on the market.


**F25**


> We first kicked off developing a no-code AI workflow builder but soon ran into the same integration hurdles many other AI developers hit with their MCP integrations: wasting weeks wiring auth flows, handling retries, and wrestling with unreliable MCP servers. That pain drove us to build and open-source our own MCP solution, which landed on Hacker News and earned 700 GitHub stars in a week. A partner at \[venture capital\] called to say we are building the best MCP solution on the market. We know people need what we are making because they find us through organic search, join our 350-person waitlist, and reach out for enterprise plans, like \[a pharmaceutical company's\] Head of AI.


The first answer explains why we believed the problem existed. The second tells the same origin story, but adds evidence that other people had encountered the problem too. Our conviction no longer had to stand alone.


## Who are your competitors?


**S25**


> Zapier MCP, MCP.so, and others give end-users a hosted gateway to 7,000 apps but are built for desktop/chat clients, not for embedding in your code. Smithery.ai offers a registry with a basic SDK, but they focus on enabling end users to integrate MCP servers into apps which already have MCP support, like Cursor. Metorial focuses on developers who build AI products wanting to integrate tools and actions without having to dive deep into adding their own MCP layers. There's no DevOps, no manual plumbing, and no extra configuration. The most direct competitor targeting developers is \[yc company\], but they only offer support for 32 MCP servers and offer no SDK, logging, or monitoring.


**F25**


> MCP platforms like Zapier MCP focus on spinning up servers for chat clients rather than embedding AI directly in code, and registries such as Smithery cater to end-user apps. We understand that bringing AI agents to production demands a developer-first integration platform with full observability built in from day one. Our closest competitor, \[yc company\], also offers MCP integrations for AI applications, but they support only 45 servers, provide rudimentary SDKs, and lack comprehensive developer tooling.


Here again, the second answer is shorter without saying less. The dividing line is easier to see: other products served chat clients or registries; we intended to serve developers putting agents into production.


## How large can this become?


**How do or will you make money? How much could you make?**


> Standard usage-based billing model: customers pay $4 per 1,000 tool calls. OpenAI alone currently has 3 million developers signed up, of which roughly 1-2 million use the API actively each month. Assuming an average of 50,000 tokens per user per day, mix of light chat and heavy embeddings/fine-tuning, that equates to about 1.5-3 trillion tokens processed last month, roughly 7.5-15 billion calls at 200 tokens per call. If 80% of those calls require external service integrations, that's 6-12 billion tool calls per month, implying a $288-576 million ARR opportunity at $4 per 1,000 calls. Capturing just 5% of this market would yield about $21.6 million annually. These numbers will grow as we expand our market presence.


This is the answer I am least satisfied with now. We began with the number of OpenAI developers, estimated their token usage, converted tokens into calls, assumed that 80 percent of those calls would require tools, and then gave ourselves five percent of the resulting market. The arithmetic is precise; the assumptions are not.


During YC group office hours, we were encouraged to reason in the opposite direction: from the customer upward. At $50,000 per year, $100 million in annual revenue requires 2,000 customers. At $5,000, it requires 20,000 customers and an entirely different company. The questions become less elegant but more useful: who are those customers, who controls the budget, and how long will each sale take?


If I rewrote this answer today, I would keep a short version of the top-down calculation to show the available room, then add the bottom-up path to test whether we could plausibly reach it.


## Ownership and fundraise


**Planned equity ownership**


**S25**


> Tobias Herber, CTO, 50%. Karim Rahme, CEO, 50%.


**F25**


> Karim Rahme, CEO, 50%. Tobias Herber, CTO, 50%.


> Keep it as close to an equal split as possible.


**Relevant details about the fundraise**


**S25**


> We had a call with \[partner at VC\]. He reached out to us after finding our open-source MCP container project which had hit over 700 stars on GitHub. The partner said we are a bit too early for him since they are a seed fund, but he would pass along our demo to pre-seed funds he knows, and that we should get back to him once we have more traction.


**F25**


> \[Partner at VC\] personally reached out to us after finding our open-source project on GitHub. They are interested in discussing a seed round once we have more traction.


## Why YC?


**S25**


> We were convinced to apply to Y Combinator through word of mouth from our friends. Our close friends X and Y work for Company X (YC W24) and Company Y (YC S24), where they attended YC mixers and events around San Francisco, telling us about the value of the community and the resources it offers to builders.


**F25**


> We have friends working at YC companies. X worked at Company X and is now full-time with Company Z while Y is at company Y. Their word-of-mouth convinced us that Y Combinator is the right place to build.


**How did you hear about Y Combinator?**


> See above.


## Remaining answers


Field S25 F25


Category Developer Tools Developer Tools


Previous incubator, accelerator, or pre-accelerator n/a n/a


Founder citizenship - Karim Rahme, Austrian Citizen; Tobias Herber, Austrian Citizen


Noncompetes or overlapping intellectual-property agreements No No


Anything else YC should know No No


## What actually changed


I cannot know precisely why YC made a different decision the second time. The partners may have weighed details differently from how I reconstruct them now. But the observable differences were not subtle. Both founders became full-time. People began using the product. Our open-source projects grew from 700 to 2,600 stars. A large enterprise buyer reached out. The product grew from a convenient integration into a more complete platform.


Those were months of work. By comparison, writing the application took an hour or less.


None of this makes prose more important than the company. A cleaner application would not have rescued our first attempt if nothing else had changed. Still, the writing mattered. In the first application, we made a broad technical system sound perilously close to a thin feature. In the second, we described a business.


I do not think there is a phrase that gets a company into YC. If I had to reduce our experience to anything, it would be this: reapplying was valuable because the interval between the applications contained something worth showing. We answered the concern beneath the rejection, separated what existed from what we merely expected, and described the product in a way someone outside our heads could repeat.


YC eventually took a bet on us. Publishing both applications is one small way to pay that faith forward.
