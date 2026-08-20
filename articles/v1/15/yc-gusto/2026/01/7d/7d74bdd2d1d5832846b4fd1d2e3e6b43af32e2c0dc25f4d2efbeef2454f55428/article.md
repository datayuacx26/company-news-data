---
schema_version: "1.0.0"
document_id: "7d74bdd2d1d5832846b4fd1d2e3e6b43af32e2c0dc25f4d2efbeef2454f55428"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-engineering-rss"
canonical_url: "https://engineering.gusto.com/it-takes-a-village-building-gustos-first-ai-risk-agent-2a6aee2e6297"
published_at: "2026-01-26T20:24:46+00:00"
first_seen_at: "2026-07-19T22:15:27.842622+00:00"
fetched_at: "2026-07-28T20:54:41.720106+00:00"
content_hash: "sha256:45bf4d19ebc7802f31db01b94af79899e1b7ba38fcef40af9237f794cc188722"
---

# It Takes a Village: Building Gusto’s First AI Risk Agent

# It Takes a Village: Building Gusto’s First AI Risk Agent


[Xao Yang](https://medium.com/@xao.yang?source=post_page---byline--2a6aee2e6297---------------------------------------)


12 min read


·


Jan 26, 2026


--


Press enter or click to view image in full size


Myth vs reality of AI Work


## The Myth vs Reality of AI at Work


As an engineer, I’ve grown skeptical whenever I hear the word “AI” thrown around as a solution. There’s an unspoken belief that AI should just figure things out on its own. Feed it enough data, write a clever prompt, and something magical happens.


After spending months building Gusto’s Risk Onboarding Work Agent (GROW), Gusto’s first AI-powered risk agent, I can tell you: the common mental model of prompt + data = magic, is wrong. The missing variable is context but realizing how much we needed took more effort than we expected. We found ourselves debating things that felt absurdly small like: what does this field represent? How should it be interpreted? What does it mean if it is null? Should we send the field as null or omit it entirely? Those decisions materially changed the AI’s behavior in surprising ways.


As engineers, we could build the system. But we couldn’t answer those questions.


The hardest part wasn’t engineering. It was teaching the AI how to think about our data. And that required people who actually understood and used the data. Not engineers, but operations specialists who had spent years making these decisions themselves.


This is how we learned that lesson. Unlike traditional software development, building AI isn’t deterministic. You can’t spec it out, build it, and ship it. You iterate toward a quality bar, and that requires a different playbook: domain experts as co-creators, qualitative feedback over metrics, narrow scope, pragmatic trade-offs, and a tolerance for mess. What follows is how we discovered each of these the hard way.


Press enter or click to view image in full size


Transitioning from a proof of concept to product


## From POC to Product


### Why It Mattered


Companies onboard to Gusto to help them manage and simplify payroll. At onboarding, we use ML models to assess bad actor risk for such customers. While our ML models auto-approve much of the onboarding volume, companies whose ML score is higher than the approval threshold are required to go through manual review, whether due to compliance documentation or insufficient signals to auto-approve.


Our customers are small business owners, people more susceptible to economic uncertainty, and so are their employees. As we scaled, onboarding volume began outpacing what our operations team could review without increasing headcount non-linearly. Slow approvals meant lower conversion rates and a worse customer experience. A delayed approval doesn’t just mean a slow process. It can mean an employee waiting on rent money, or a business owner unable to pay their team on time. Delayed approvals mean delayed first paychecks.


But speed wasn’t the only concern. As the Risk team, we also needed to ensure bad actors weren’t getting onto the platform. After all, the companies reaching manual review were the ones that the ML models had determined to be higher risk. Our goal wasn’t just to move faster. It was to move faster without compromising quality.


### The Spark


The kickoff to innovate on AI was triggered by a confluence of things: one of our Risk Operations leaders was going through an introduction to prompting course and our Risk engineering manager was using AI to build risk summarization. What sparked this journey was a white-boarding session where the head of Payments and Risk, Risk operations leader and Risk engineering manager started drawing what it would take to get an AI agent for company approvals.


They tried Gusto’s enterprise versions of Gemini and ChatGPT asking things like “Give me a risk assessment on Company XYZ” using external and internal data. The initial tests were promising and the results were surprisingly good when data was easily accessible.


Every insight needs an action: what if we created a zero-to-one team — Gusto’s model for small, nimble teams that exist to rapidly validate new ideas. In August 2025, we decided to create one!


### The Team


To find out if the idea could work, we needed engineers who could build the infrastructure and operations experts who understood the domain. We called ourselves the “pirate crew.” This team was meant to be scrappy, with tight iteration loops and aggressive timelines. Critically, we had leadership buy-in that gave us space to move fast without getting blocked by competing priorities.


Early plans called for RAG (Retrieval-Augmented Generation) and fine-tuning. As the engineering lead, I was tasked with implementing that infrastructure. But as we started working closely, meeting multiple times a week, we discovered something simpler: a well-structured prompt with the right context and data could get us started with good results. The complex infrastructure could wait.


That discovery set the tone for everything that followed.


Press enter or click to view image in full size


A team collaborating on building a product


## It Takes a Village


### The Division of Labor


We quickly learned that engineers and operations subject matter experts (SMEs) needed to own different parts of the problem. Engineers owned the technical infrastructure: building the framework, setting up shadow testing and backtesting, making decisions about where and how to build. Operations SMEs owned everything the AI needed to get right: domain knowledge, edge case identification, translating standard operating procedures (SOPs) into structured prompts, spot-checking outputs for accuracy, and knowing when something “felt wrong” even if it technically passed.


### The Feedback Loop


From mid-August through early October, the team met multiple times every week. The cycle was tight: adjust the data payload, refine the prompts, test with real cases, get SME feedback, repeat.


Early on, we hit a milestone that gave us confidence: 14 out of 16 AI decisions matched what our human reviewers would have done. It wasn’t perfect, but it was enough to prove the approach could work.


Our operations specialists could look at an AI decision and immediately know if something was off. Not because they understood the technical implementation, but because they had years of context we didn’t. That kind of feedback was hard to replicate.


For example, they could look at a company’s bank account and immediately flag risk patterns that wouldn’t show up in any ruleset. Certain neobanks appeared frequently in fraudulent applications; seeing prepaid debit cards when other red flags were present as an example. These weren’t things we had documented in our systems. They were patterns operations had learned from years of reviewing cases. When the AI missed these signals, our operations specialists caught them. When we asked why something felt off, they could articulate the pattern so we could encode it.


Another example: the AI would sometimes claim that a signatory’s email “aligned with” their name when it clearly didn’t. To the AI, any plausible-sounding match was good enough. Our operations specialists knew that in fraud cases, subtle mismatches matter. We had to teach the AI to be skeptical in the way a human reviewer would be but not be so skeptical that it became totally conservative.


### Using AI to Write the AI’s Instructions


Here’s something we didn’t expect: we used AI to help us build the AI.


We had a long, narrative Standard Operating Procedure (SOP) for company approvals. We needed to convert it into structured, step-by-step instructions the AI could follow. So we fed the SOP into Gemini and asked it to restructure the content.


But the AI’s output was just a starting point. The operations team spot-checked, rewrote, and confirmed that the steps accurately reflected how Gusto actually evaluates companies, referencing the exact data fields in our systems. No amount of prompt engineering could have substituted for that institutional knowledge.


One insight that stuck with me came from the Head of Payments & Risk: “Write your prompts like code. Just in English.” That framing helped. We stopped writing prompts like we were chatting with the AI and started writing them like specifications.


### Why I Couldn’t Do This Alone


I want to be direct: as engineers, we could not have built GROW alone.


The institutional knowledge wasn’t something I could learn in a few weeks. Our operations specialists had years of context about edge cases, risk signals, and evaluation nuances that no documentation could fully capture. They needed us to translate that expertise into something technical. We needed them to tell us what “right” looked like.


Press enter or click to view image in full size


The messy reality of building an AI product


## The Mess


### We Optimized for Learning


One of our first technical decisions was where the agent should run. We had two options: build in Gusto’s dedicated AI platform service, cleanly separated from our monolith, or embed it within our existing monolith where ops workflows already lived.


We chose the monolith. It wasn’t the architecturally pure choice but it was the pragmatic one.


- **Organizational reality** : we didn’t own the general AI platform service. The team that owned that had their own critical path work. Building there meant getting blocked by another team’s priorities or potentially blocking their work if bugs arose from our feature.
- **Technical simplicity** : building within the monolith meant direct access to everything we needed. No cross-service plumbing, no complicated infrastructure setup.
- **Adoption friction** : a new service would mean new permissions, new tooling, new workflows for our operations team. By staying in the monolith, the AI became an enhancement, not an upheaval.


But the real reason was this: we were optimizing for learning, not architecture. Figuring out prompts, payloads, evaluation, and iteration mattered more than getting the architecture “right” from day one. Those learnings transfer even if we migrate later.


## Get Xao Yang’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


That same pragmatism shaped how we approached everything else.


### What Failed


The actual building process was messier than any technical blog usually admits.


We started with prompts that were too vague. The AI produced inconsistent results because we hadn’t defined our terms. We assumed the AI would infer things from context. It didn’t.


For example, we had a risk score where lower values indicated lower risk. We told the AI to consider “low scores” as a positive signal. But the AI interpreted “low” inconsistently. In some cases it treated a low score as good. In others, it flagged “low score” as concerning, using phrases like “the low score introduces some risk.” The same words meant different things depending on context and depending on the time when the agent was called. We had to define exact thresholds on how to read the data and spell out what each range meant. No room for inference.


We kept discovering edge cases. The ops team would look at a result and say “that’s not how we’d handle this situation,” and we’d realize there were unwritten rules and institutional knowledge we hadn’t incorporated in our context.


### The Unglamorous Work


The work that actually moved the needle wasn’t glamorous.


Deciding what context to include vs. leave out. Too little and the AI couldn’t decide. Too much and it got confused or fixated on irrelevant details.


Handling nulls. Do we send nil? Omit the field? Each choice changed how the AI interpreted the data.


Prompt wording. Small changes in phrasing produced different results. We couldn’t predict which phrasings would work until we tested them.


Operations specialists initially maintained a Google sheet tracking every prompt variation and manually recording results. When a prompt performed well, we became hesitant to touch it. Any change risked degrading something that worked. This led us to create shadow prompts: parallel versions we could experiment with without risking production. When a shadow prompt outperformed the active one, we’d promote it. It was scrappy, but it let us keep improving while protecting what already worked.


The approach paid off. In one test, we ran a new prompt in shadow mode where the production prompt had disagreed with our ops team. The shadow prompt got 90% right. That gave us confidence to promote it.


We deliberately kept scope narrow for v1. We didn’t obsess over model selection or cutting-edge architecture. We picked a model that worked and focused on the iteration loop.


Press enter or click to view image in full size


Seeing the impact


## Impact and What We Learned


### The Results


GROW now reviews companies and provides risk assessments that match what human analysts would produce.


In the first three months: we have reduced manual reviews by up to 70% and have 6x faster time to approval.


### When GROW Gets It Wrong


Because GROW is a back-office agent, no customer ever sees its decisions directly. That gave us room to build in safeguards.


If GROW doesn’t auto-approve a company, nothing changes. The company stays in the manual queue for our operations team, exactly where it would have been without GROW.


The riskier scenario: what if GROW approves a company that’s actually high-risk? Here we layered in defenses. When GROW approves a company, we signal to our other downstream ML models that this was an AI-approved decision. Those models then leverage this information along with other signals gathered in the subsequent customer journey to determine risk at appropriate points.


But we didn’t stop there. Our operations team became a human-in-the-loop QA team where they could review GROW’s decisions as needed. If they agree with the decision, that’s another signal to make the customer experience downstream smoother. If they disagree, we have the safeguards to establish trust in that company. The human override goes both ways.


### What Worked


Tight partnership, narrow scope, and leadership support. Meeting multiple times a week meant we could iterate in days, not weeks. A dedicated team and a purpose without competing priorities meant we could move fast. Side projects don’t ship this quickly.


### What Surprised Us


Qualitative feedback mattered more than metrics. We couldn’t automate our way to “good.” We needed human judgment on whether the AI was making decisions a human would make.


The unglamorous work was where value was created. Data curation, edge case handling, SOP refinement. None of it was exciting, but all of it was essential.


The AI didn’t figure things out. Every assumption we left implicit, it got wrong.


### Humans in the Loop as Our Initial Eval System


We didn’t initially build a formal evaluation system. We had something better: humans in the loop.


Our operations team reviewed AI outputs that did not line up with their own judgment. Would a human analyst have made this call? That qualitative check was our eval. It didn’t scale the way automated tests do, but it caught things no automated test could have.


None of us are AI experts. We didn’t set out to build an evaluation framework. But this is what naturally emerged from the work: SMEs as the quality bar, processes to test changes safely. We were doing evals without calling it that.


The lesson from this project: get the humans in the loop right first. The formal systems can come later.


**Advice for Teams Starting Similar Projects**


- Involve domain experts from day one. They’re co-creators, not consultants.
- Embrace qualitative feedback. Metrics alone won’t tell you if your AI is good.
- Start narrow. One use case, working, then expand.
- Make strategic trade-offs. Architectural purity can wait. Learning can’t.
- Document your SOPs. Clean and specific SOPs make clean prompts.
- Expect the mess. Prompts will fail. Data will be missing. Edge cases will surprise you. That’s not a sign something is wrong. That’s how this work actually goes.


**What’s Next**


We’ve seen real success with GROW for company approvals. On Risk, we have many processes that could benefit from this kind of agentic workflow. The playbook we built here (domain experts as co-creators, tight feedback loops, optimizing for learning) is what we’ll take with. In fact we have applied it to Account Take Over (ATO) Agent and have seen success where ATO AI agent accuracy is near human accuracy.


We’re also investing in more formal evaluation frameworks, both offline and online, to complement the human-in-the-loop approach that got us here. The scrappy Google sheets worked for v1. v2 and v3 are more automated and we continue to scale and build structure through iteration.


We’ve already started posting jobs for an AI Agent Manager: a role focused on the kind of work our operations specialists were doing, full time. With the job of managing agent accuracy and agreement/disagreement rates. That’s how much we believe in this approach.


**Acknowledgements:** Thanks to the pirate crew:[Samant Nagpal](https://www.linkedin.com/in/samant-nagpal-3240683/) ,[Achint Goel](https://www.linkedin.com/in/goelachint/) ,[Rochak Neupane](https://www.linkedin.com/in/rochakneupane/) ,[Brian Deane](https://www.linkedin.com/in/briandeane/) ,[Sia Tang](https://www.linkedin.com/in/shi-sia-tang-1549b5a5/) ,[Shubham Agarwal](https://www.linkedin.com/in/agarwalshubham2007/) ,[Drew Noolas](https://www.linkedin.com/in/drew-noolas-17b35168/) ,[Ainsleigh Mitchell](https://www.linkedin.com/in/ainsleighmitchell/) ,[Dale Robinson](https://www.linkedin.com/in/dalearobinson/) , and[Christian Hillson](https://www.linkedin.com/in/christianhillson/) . And to[Sarang Bhutada](https://www.linkedin.com/in/bhutadasarang/) and[Kim Nguyen](https://www.linkedin.com/in/kimnguyenkhn/) for their sharp feedback on this post.


**If you’re interested in solving complex problems that deliver real-world value, Gusto is hiring! Learn more at**[gusto.com/careers](http://gusto.com/careers) **.**
