---
schema_version: "1.0.0"
document_id: "5e4f2623b3bdb96510e6de6fd667f44d8a13a525d607d76c302e376166ac00f9"
company_key: "yc-velos"
company: "Velos"
source_id: "yc-velos-news-import-0294ff98d3e5"
canonical_url: "https://tryvelos.com/articles/not-every-insurance-process-is-a-good-fit-for-ai"
published_at: null
first_seen_at: "2026-07-22T18:33:16.886113+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:7827efa91933d7a558d4fcd21797623edff21524892a52fd1c703875ba7c9dd9"
---

# Not every insurance process is a good fit for AI

A few weeks ago, I gave a webinar in partnership with the[Insurance Accounting & Systems Association](https://www.iasa.org/IASA/Events/Event-Display.aspx?EventKey=WBNR040826&Source=Social&WebsiteKey=ebda8b10-95bf-48b4-a3da-059f940dc7ba) ( *IASA* ) where I shared a practical framework that leaders can use to identify AI initiatives worth pursuing, and how to avoid costly missteps. I also went over how to evaluate automation opportunities using real-world case studies of AI wins and failures in insurance operations.


View the full slide deck: https://www.figma.com/deck/mLq3f539AztyPl2i7p8Lky


—


##
“We’re Using AI” Doesn’t Mean Much


A lot of conversations about AI start with some version of, “Yeah, we’re using AI.”


That statement is about as useful as saying, “Yeah, we’re using the internet.”


I don’t mean that as a cheap shot. For anyone who lived through the shift from on-prem systems and mainframes to cloud software, “using the internet” was not a trivial milestone. Similarly, getting an organization onto an approved enterprise AI tool is not nothing. It can reduce shadow IT risk, give employees a flexible sandbox, and create a safer alternative to people pasting sensitive information into their personal ChatGPT accounts.


But it is still too vague to be useful.


Instead, I think it helps to look at AI deployment as a spectrum.


At one end, you have company-wide access to tools like ChatGPT, Copilot, Gemini, or Claude. This is the lowest-risk form of deployment. It is flexible, easy to roll out, and can create real value in the hands of creative employees. It is a little like giving everyone Excel. Some people will do incredible things with it. Others will barely touch it. The return is real, but variable.


The next step is targeted use-case enhancement. This might look like shared prompts for an underwriting team, internal playbooks for how to use AI in a specific workflow, or AI features embedded in tools employees already use. It also includes what I think of as “vertical chatbots”: tools positioned as ChatGPT for underwriting, claims, compliance, or some other specific function.


These tools can be valuable because they reduce the amount of imagination required from the user. They prescribe where and how AI should be applied. But they still usually assume there is a person sitting in the driver’s seat, moving information into and around the system. The form factor is often still a chatbot or autocomplete interface, just with more context and better packaging.


Then there is the third act: fully autonomous workflows.


This is the cutting edge. These are AI-driven systems that execute long-horizon workflows repeatedly, often across hours of runtime and hundreds or thousands of individual AI steps. If you tried to recreate the process manually by messaging ChatGPT step by step, it would take hundreds of back-and-forths.


The reward can be much higher. Entire processes, often the ones nobody wants to do, can be handled with the speed, scalability, and auditability of software.


But the cost is also higher. You have to answer hard questions. How do you guarantee sufficient accuracy? How do you detect when something has fallen out of the happy path and needs to be kicked to a human? How do you debug a system that made 100 different judgment calls along the way? How do you manage security, compute, and recovery?


The throughline is autonomy. As autonomy increases, so does potential ROI. But so does cost and risk.


The playbook is about determining where a use case belongs on that spectrum.


Spending six figures on implementation only to realize the problem could have been handled by telling your team to use ChatGPT is an expensive mistake. On the other hand, giving people a chatbot for a process that really needs a repeatable autonomous workflow is also a mistake. People will conclude “AI doesn’t work,” when really the wrong delivery mechanism was chosen.


##
The First Failure Pattern: The Process Is a Mess


The first and most common failure mode is when the process itself is a mess.


Simply put, if the human team doing or overseeing the work today cannot explain and agree on how it is done, AI is not going to be your salvation.


A painful example: a commercial auto MGA was revamping part of its month-end close process. One key task involved recording and sending exposure information for all in-force units to a capacity provider. The raw exposure data came in variable formats, and there was no clean central system that normalized it. What they did have were historical spreadsheets showing what the human team had extracted and reported in prior months.


Messy, but seemingly manageable.


The implementation team started building an AI-enhanced process and began backtesting against historical results. Some rows matched perfectly. Eureka.


But other rows were inexplicably off.


So they went back to the process owners and asked what happened. The answer was some version of: “Oh, that one is special. For that case, you need to do this.”


So they implemented the special rule. Then they ran it again. That case matched, but others still failed.


They went back again. “Right, for those cases we actually do this other thing.”


That pattern repeated until it became clear that for every rule, there was an exception, except when the exception could be ignored, except when it could not.


You get the idea.


This is the most insidious failure pattern because the technology often appears to be handling the “hard” part. The AI can read the messy spreadsheet. It can structure the data. It can pull out the right fields. But it never quite matches the human process because the human process is not actually defined.


That creates implementation drag. It is the kind of project where the vendor seems to say “almost there” every week, until six months pass and everyone starts realizing the thing may never converge.


Everyone loses.


My sense is that this happens because people’s mental model of AI is still too close to “Skynet” or the recommendation engine that keeps them scrolling TikTok. It feels powerful, predictive, and mysterious.


In practice, AI is often closer to a very smart intern. It can process a lot of information, do a lot of grunt work, and follow instructions well. But it will not magically bring order to a process your VP of Finance was supposed to own.


If you would not expect an intern to figure it out, you should not expect an AI implementation team to figure it out either.


##
The Second Failure Pattern: Good Is Subjective


The next failure mode is what I like to call the taste problem.


This shows up when the definition of a “good” output is subjective. One of my colleagues calls it the “I’ll know it when I see it” problem. Another warning sign is when process experts say, “It’s more art than science.”


At the core of this problem, the AI system can produce something valid. It can generate an output in the right shape. It can look impressive. But it does not quite pass the sniff test without human editing.


One common area where this shows up is specialized underwriting research.


I saw a team evaluate whether AI could replace underwriting assistants in compiling research reports on submissions. The work involved researching a company, its executives, and other publicly available information. At first, the results were impressive. The system could produce a lot of useful information. It summarized submissions, highlighted sensible areas, and looked similar to what a junior analyst might produce.


But it was missing something.


The experienced underwriters knew it. They could point to examples. They could sometimes explain what they would have done differently. But a lot of it came down to feel, context, and judgment.


That does not mean AI cannot help. In fact, this can be a great use case for AI.


But it may not be a great use case for full autonomy.


In this example, a less specialized system paired with a human driver outperformed a more ambitious autonomous system. The human could own the final judgment while using AI to move much faster through the grunt work.


That distinction matters. A lot of underwriting falls into this bucket. The right answer is not “AI cannot do it.” The right answer is that the problem may be better suited to a copilot than a fully autonomous workflow.


##
The Third Failure Pattern: Thinking About Accuracy Wrong


The third failure pattern is accuracy theater.


There is an old way of thinking about AI implementations where the buyer wants a single top-line accuracy number from the vendor on the first call. This leads to vendors saying things like, “Our model is 98.3% accurate,” usually with enough asterisks to make the number meaningless.


That model made more sense when we were talking about simpler classifiers. But for complex operational workflows, accuracy is not a static number someone should be able to give you off the top of their head.


Accuracy is a trajectory.


It usually starts lower than you want. Then, as the implementation proceeds, the team uncovers edge cases, undocumented workarounds, unclear rules, and parts of the process that were never fully written down. As those get captured, accuracy improves.


The key is to separate two things. The parts of the process that are clearly documented should be highly accurate and stay that way. The overall accuracy may still be dragged down by undocumented edge cases. That is not necessarily a reason to pull the plug. It may just be the implementation doing its job by surfacing the mess.


So the right question is not, “What is your accuracy?”


The better question is, “How will accuracy be managed over time?”


A good implementation team should be able to explain how errors will be categorized, how edge cases will be resolved, how the system will improve, and what controls will determine whether it is ready for production.


Accuracy should be treated as an ongoing control, not a point-in-time marketing claim.


##
What the Successful Implementations Have in Common


The success stories tend to be the other side of the same coin.


One example involved high-volume invoicing at a program administrator. Every month, they had to process exposure data from thousands of insured locations across hundreds of different formats and then invoice based on rules that varied by insured.


Before automation, the work was handled by a growing offshore team.


This had the classic pain profile: enough volume to justify investment, enough variability to make ordinary automation difficult, and enough operational burden that the pain scaled directly with growth.


It was not a glamorous use case. It was not the kind of thing people put in a keynote to look futuristic. But it was a great use case.


There was clear pain. There was a documentable process. There was a strong definition of correct because invoices are a natural source of truth: if the invoice is wrong, someone will complain.


That does not mean it was easy. Early in the process, the company lacked a single source of truth for how certain cases should be handled. The offshore team had internalized inconsistent rules. Blindly trying to match that inconsistent process would have turned into a never-ending chase.


The breakthrough came when the process owners agreed to define the process clearly enough for the system to execute it. Once that happened, edge cases could be brought under a documented process, accuracy could be monitored, and the implementation could move forward.


That is the real lesson.


The win was not just that software could read messy PDFs or spreadsheets. The win was that the process could be wrangled into something documentable, measurable, and improvable.


Another example involved financial reconciliation for a fractional finance team serving large regulated organizations. The team had to reconcile data across customer systems, legacy platforms, ERPs, and PDFs from financial institutions.


On the surface, it checked a lot of boxes. The process was fairly systematic. There was a clear definition of correct for many transactions. Volume was growing, and headcount would have needed to grow with it.


But there was a caveat.


The process had two phases. Phase one involved obvious matches: identical amounts, similar dates, and corroborating details. Those were highly automatable. Phase two involved more judgment. Sometimes two transactions from one system needed to be combined to match another transaction elsewhere. Some of that was objective, but some of it involved the judgment of a senior finance person deciding whether the match really made sense.


The solution was not to force full automation everywhere.


Instead, the system automated phase one and routed phase two to humans for review. Over time, the human decisions could inform the system, allowing more transactions to be auto-reconciled later.


That still created strong ROI because the team no longer had to spend time on the obvious work. Their capacity increased because they could focus on the ambiguous cases rather than basic processing.


Again, the lesson is not “AI can read PDFs.” The lesson is that success comes from understanding which parts of the workflow are ready for autonomy and which parts still need human judgment.


##
The Market Is Moving This Way


A lot of what we are seeing in the market is a reaction to these dynamics.


The best firms are treating enterprise-wide AI tools as discovery mechanisms. If a finance team is consistently using the same three prompts to reduce month-end close work by 20%, that may be a signal that a deeper autonomous workflow is worth exploring.


At the same time, many companies are on their second marriage with AI vendors. They got burned the first time. Maybe the vendor overpromised. Maybe the buyer picked the wrong problem. Usually, the issue was some version of poor problem-solution fit. A core dimension of the process was never resolved, and the pilot drifted for months without converging.


Buyers are also becoming less willing to pay for “ChatGPT with a fresh coat of paint.” They want more than a tool. They want outcomes. In response, vendors are increasingly moving toward managed workflow takeovers with clearer guarantees around accuracy, speed, and timelines.


In other words, the market is starting to realize that the hard part is not access to a model.


The hard part is operationalizing the model against a real business process.


##
The Scorecard


So what is the playbook? It starts with scoring the use case.


The dimensions I would look at are:


**Volume.** Is there enough work, frequency, or operational burden to make this matter?


**Stability.** Is the process stable enough to document?


**Definability.** Can we define what “correct” means?


**Exception rate.** Are the exceptions manageable and finite, or is the process basically all exceptions?


**Economic value.** Is this painful enough to justify the effort?


That last point matters more than people think. A use case can be technically interesting and economically irrelevant. Those are dangerous because they look like innovation but do not actually change the business.


The exception rate also deserves attention. The question is not whether exceptions exist. They always do. The question is whether exceptions can be categorized, routed, and managed. If they can, autonomy may still work. If every case is its own bespoke snowflake, you probably want a human-AI copilot instead.


##
Start with an SOP


The most underrated tool in AI implementation is the humble standard operating procedure.


We talk a lot about “documentable” processes, but that can sound abstract. The fastest way to test whether a process is documentable is to try to write it down at a level of detail that someone with minimal business context could follow.


This can be painful.


It may require multiple rewrites. It will surface holes. It will create debates. People will realize that different team members are doing the same task in different ways.


Good.


You want to find that out before you spend six months and a large implementation budget.


A good SOP should capture the process, the definition of correct, the known edge cases, and the escalation paths. It becomes the baseline for implementation, accuracy management, and ongoing improvement.


If you can produce a strong SOP and the economic case is real, the implementation becomes dramatically de-risked.


If you cannot produce one, that is not a reason to give up on AI. But it is a reason to be honest about where the use case belongs on the autonomy spectrum.


##
The Real Lesson


Successful AI implementation does not mean every project becomes a fancy autonomous agent. It means matching the problem to the right solution.


Sometimes that means giving employees secure access to AI tools and letting them work faster. Sometimes it means a structured copilot embedded in a specific process. Sometimes it means a fully autonomous workflow that takes over a painful recurring operation.


The mistake is treating all of those as the same thing.


The organizations that win with AI will not be the ones that chase the most futuristic demos. They will be the ones that get very good at selecting problems, documenting processes, defining correctness, and managing accuracy over time.


Most teams obsess over the technology. The best teams obsess over the problem.
