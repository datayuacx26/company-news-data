---
schema_version: "1.0.0"
document_id: "93f10aeaa098d376cd03537e1e974de1641dcf79a173e609369997ec87a605ec"
company_key: "yc-common-paper"
company: "Common Paper"
source_id: "yc-common-paper-rss-3b76f5ee637c"
canonical_url: "https://commonpaper.com/blog/ai-chat-in-contract-review-process/"
published_at: "2026-06-10T11:44:06+00:00"
first_seen_at: "2026-07-20T23:20:15.555416+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:3a04446d8aa4aff834bd33acd100c986e5b6027ce6d1d35a78a1b7a8f5c731c1"
---

# What AI chat assistants can and cannot do in the contract review process

People considering our[AI contract review product](https://commonpaper.com/product/ai-agent/) , Gerri, are AI-forward. Many use Claude daily and they actively look for new AI solutions to old problems. Nearly everyone I talk to has already used AI chat in the contract review process (mostly Claude or ChatGPT). This article consolidates what people are experiencing when using chat assistants in the contract process: what chat assistants do well, and why the core problems of contract review are still unsolved by AI chat alone.


## What chat assistants do well in the contract review process


Large language models are prediction machines, which makes them genuinely good at spotting odd things in contracts. A few things chat assistants on their own handle well:


- **First-read triage:** surfaces clauses worth paying attention to
- **General legal questions:** decent at “what does this clause typically mean?”
- **Comparison:** flag the meaningful changes between two versions of a document (Word will catch every minor edit, which can be overwhelming)


This is legitimately useful. One operations leader I spoke with recently described her current workflow as: run the contract through Claude to pull the key items, then review with her CEO, then escalate the hard stuff to outside counsel. That’s dramatically better than their contract review process from five years ago! But it is not without its issues.


## What chat assistants do not do well in the contract review process


The examples below all came up in recent conversations with CEOs and operations leaders:


**Output volume:** Raw AI gives you everything it knows, not what you need to act on. For a 1.5-page document, you might get 1.5 pages of output. A 40-page contract will give a “summary” that is a wall of text. The experience is clunky.


**Lack of memory persistence:** Individual users on paid Claude plans can now carry context across sessions through Chat Projects. That’s a real improvement for someone working alone! But[contract review is a team sport](https://commonpaper.com/blog/contract-review-is-a-team-sport/) . When institutional knowledge lives in one person’s head (or Claude account), that memory doesn’t transfer. That person goes on vacation or leaves the company, and the memory goes with them.


**Contract review involves multiple people** : an ops person, a CEO, a CISO for the security clauses, a sales rep who negotiated the commercial terms. The problem isn’t that you can’t remember what you agreed to last quarter. It’s that your team can’t.


One ops leader described her process as going back to a contract from two years ago, taking a screenshot, downloading a copy — “copy number six on your downloads at that point” — and comparing it with the customer’s language. That screenshot collection was the playbook.


**Hallucinations in playbook-building:** When buyers try to systematize their AI usage by building a custom GPT or feeding it Slack channels and past emails to create a playbook, the output often contains hallucinations. Wrong references, invented positions, clauses attributed to the wrong party.


This was a specific issue with a contracts operator at a SaaS company I met with: “We used Claude to create our playbook based on our Slack channel where we discussed red lines and emails from legal. There was some typical hallucination type stuff in there where it referenced the wrong thing for a specific line item in the contract.”


Contract review requires more precision than this.


**Context poisoning appears in long-running chats:** A long-running chat maintains context, but if it covers too many issues, you run the risk of context poisoning. This is when the assistant starts to lose track of what action belongs to what document, or whether a certain position was a one-time exception or a general policy. In contract review, this matters: confusing the customer’s redlines with your own template means accepting terms you meant to reject. AI results improve when there is greater structure.


**Lack of resolution workflow:** As I said before, contract review is a team sport. Indemnity language goes to your lawyer. Customer audit requests need engineering review. Security issues routed to CISO. General purpose AI can flag the issues, but someone still needs to project manage the contract review.


## How we approach AI contract review


Like AI chatbots, our contract review software is built on a large language model. But Gerri also has specific functionality that puts structure and guardrails around the model to prevent the issues outlined above.


### Built for multi-player


I don’t want to sound like a broken record, but contract review really is a team sport. This is the foundational belief that Gerri is built on.


Contract review creates significant friction in deal cycles, and the industry keeps making better tools for lawyers. Speed comes from better coordination across the full group.


Most AI contract tools are single-player: you, a document, and a chat window. Gerri is built around the multi-person reality. Every team member sees the contracts and decisions relevant to them. Every decision is logged and visible across the team.


With Gerri, anytime the AI surfaces a term that isn’t covered in your existing playbook, it will automatically route that term to the right party. So AI is handling the entire contract review process which involves getting every redline and issue in the negotiation resolved.


### Self-contained context


The hallucination problem in AI comes largely from context bleed: prior conversations, unrelated threads, and multiple documents create interference. Gerri runs each review in a clean context along with your approved playbook and the instructions you gave it on how to behave, like overall risk tolerance that may not be explicitly defined in your playbook. These features significantly reduce the risk of context poisoning and hallucinations.


### Every decision is cited and visible to the whole team


Gerri doesn’t just flag a clause. It tells you which playbook rule triggered the decision, and why. Every recommendation is traceable to a specific rule. When you disagree with a call, you know exactly which rule to update. In a single click you can establish the rule as one-time or a permanent position moving forward.


This traceability also makes coordination possible. The person who ran the review, the CEO who needs to approve an exception, the outside counsel who gets escalated items — they all see the same reasoning. It’s not a screenshot in someone’s Downloads folder and it’s not institutional knowledge that walks out the door.


### Imperfect inputs don’t break the system


Several of our customers attempted to build a playbook using only Claude: pulling from Slack channels, email threads, and legal feedback. The result had hallucinations in it. That’s not a problem if you’re using Gerri. Uploading an imperfect playbook to Gerri won’t corrupt the system. You find errors by watching how Gerri applies rules. You fix them by updating the rule. The playbook gets more accurate as your team uses it, and every correction is visible to everyone working the contracts.


The system is designed to learn from your team’s decisions.


### Available in Claude


Finally, our contract review software plays very nicely with Claude.[Gerri has an API & MCP](https://help.commonpaper.com/en/articles/15264267-using-gerri-in-claude-cowork) , so if you like Claude (and I do too!), you can use Gerri there, getting access to all of the capabilities listed above without leaving your existing Claude workflows.


## What about Claude for Legal?


Anthropic released a set of skills and plugins called “Claude for Legal” to support the legal profession. If you have a Westlaw subscription and an enterprise Microsoft 365 license, then it’s worth checking out. Anthropic is building integration and capabilities that will ease some of the friction lawyers feel with the current ecosystem of tools.


For everyone else, the challenges above remain largely the same whether you’re using Claude, ChatGPT, or Claude for Legal: you need to solve multiple people in the contract review process, you need to have an audit trail on shared decisions and positions, you need institutional memory that your whole team can see and act on.


That gap is exactly what Gerri is built to close.
