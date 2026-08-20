---
schema_version: "1.0.0"
document_id: "20f209f38c6ae26c03a503230f048e052e60617857046dd7e6bb7cf393a8d540"
company_key: "yc-promise"
company: "Promise"
source_id: "yc-promise-news-import-0c9e56a05050"
canonical_url: "https://www.promise-pay.com/resources/inside-the-ai-agent-that-gets-benefits-to-people-faster"
published_at: "2026-04-07T00:00:00+00:00"
first_seen_at: "2026-07-25T19:58:41.550404+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:22de61f12bf27afad59849b40e572ffece4919731a037efa1ed6b95763210907"
---

# Inside the AI agent that gets benefits to people faster

Every winter, low-income households apply for programs that help them keep their lights and heat on. Traditionally, the review of these applications is slow and manual: a single case can take a trained agency reviewer 5-10 minutes to process, even in simple cases. We built an AI agent that handles that same case in under 2 minutes. Reviewers refocus: instead of manually reviewing applications themselves, they are re-tasked with QAing the results of the AI agent, focusing human attention on complex cases where it matters most rather than on the repetitive work of cross-referencing policy and documents. And instead of waiting weeks, families are approved for aid in just a single day.


‍


## The problem with benefits administration at scale


Eligibility determination requires reviewing a wide range of document types across many income categories, applying rules that vary by household size and geography consistently across thousands of applications. The operative word is *consistently* . When a human reviewer processes 40 cases a day, the same documentation can produce different outcomes depending on who reviews it, when they review it, and how they interpret ambiguous rules.


‍


That inconsistency makes the human review process both a quality problem and a fiscal one. Errors in eligibility determination go in both directions: families lose benefits they qualified for, and agencies approve payments that do not survive audit. Under new federal rules, states with error rates above 6% face direct cost-sharing penalties — and the national average is nearly 11%. At the same time, the administrative funding that pays for human reviewers was just cut in half. Agencies need more verification capacity with less money, and the cost of getting it wrong has never been higher. As part of the PromiseVerified platform, we’ve built an AI-assisted eligibility agent to close that bottleneck in the review process.


‍


## How the agent works


The agent reviews applications end-to-end, reading income documents, verifying information, applying program rules, and producing a structured determination using a program-specific toolset. Straightforward cases are processed immediately, with human reviewer validation. Complex and borderline cases are routed for full human review instead. In designing the agent, we were deliberate about dividing tasks between AI and deterministic logic. This architecture leverages each technology's strengths: the LLM handles what it's proven to be good at, such as reading documents, interpreting context, and applying nuanced program rules. We then use a set of non-AI hardcoded tools to handle the math: income calculations and threshold lookups. This eliminates the risk of the model hallucinating an arithmetic result that could change whether a family qualifies.
‍


## Results


Measuring accuracy was a central part of how we built confidence in the system. Before deployment, we ran the agent against historical cases with known outcomes and found that in a number of cases, the agent's determination was more accurate than the original human review. This wasn't because the agent was smarter, but because it was more consistent. Human reviewers apply the same rules differently depending on the case, the day, and who's doing the review. The agent applies them the same way every time.


‍


In ongoing quality assurance reviews, the agent has maintained a precision above 96%, consistently higher than the human review baseline closer to 90%. The cost picture reinforces the importance of this: a human review costs four times what the agent costs. Agencies are currently paying more per case for less consistent results. The hybrid model flips this, so routine cases are handled by the agent at a fraction of the cost, and human reviewers are reserved for the complex cases where their judgment adds more value.


‍


Over the first month of operation, our agent has accelerated approvals for nearly 14,000 applicants. This includes recommending approvals for qualified cases, flagging incomplete documents, and routing complex and borderline cases for full human review. This represents approximately $3.3 million in benefits to be disbursed to families who qualified for aid.


‍


## Why this matters


There are few places where well-deployed AI can do more good than manual benefits administration. Today, human reviewers are doing high-stakes work under conditions that make consistency structurally impossible at scale. The volume is high, the rules are complex, and the cost of errors falls on people already in difficult situations. AI doesn't replace human judgment in this work. It handles the routine cases with speed and consistency, freeing up human review time for the cases that genuinely need it, getting aid to people faster and reducing the administrative burden on agencies. PromiseVerified is how we’re bringing this capability to agencies at scale: we're focused on scaling this hybrid LLM-and-deterministic-logic architecture to new benefits programs, continually driving down processing times and improving the consistency of eligibility decisions across the public sector.


‍


‍


##### **About Promise**


Promise is an AI company deployed inside government benefits programs and partners with agencies and utilities to deliver scalable affordability and assistance programs. Promise has reached more than 5 million households across 20+ states and builds tools that verify eligibility, deliver relief, enforce compliance, and produce audit-ready records for every dollar, operating across SNAP, LIHEAP, WIC, Medicaid, and energy affordability programs. Promise has raised over $50 million in venture capital from leading investors, including First Round Capital, Y Combinator, Kapor Capital, XYZ Ventures, The General Partnership, and Howard Schultz. To learn more, visit[joinpromise.com](http://joinpromise.com/) and join Promise on[LinkedIn](https://www.linkedin.com/company/joinpromise/) .
