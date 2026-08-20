---
schema_version: "1.0.0"
document_id: "0192b66fdb953504a6cace0ab9995173548f5aad05f12197cbbc8fe28ca4becb"
company_key: "yc-readme"
company: "ReadMe"
source_id: "yc-readme-news-import-ecdc4511c006"
canonical_url: "https://readme.com/blog/linter-style-guide"
published_at: null
first_seen_at: "2026-07-22T11:00:35.158559+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:4250199df4599abcc0b319c15f019ba42f65e0107bf4c7b974dd382b368b9f22"
---

# Keep Your Docs Consistent With the AI Linter

One key element of good documentation is consistency. Docs with a single, unified voice and uniform structure increase trust in your product and help users get moving quickly.


But most documentation isn't written by a single person. In fact, collaboration is one of the most celebrated features of ReadMe — anyone on your team can contribute to your docs in the way that works best for them, whether they're a software engineer working in the ReadMe CLI or a product manager making edits in the IDE.


So how do teams maintain one voice with many contributors? A style guide. Style guides can include nearly anything, from grammar rules to code style for examples to guide structure and everything in between.


Enter the[ReadMe AI Linter](https://docs.readme.com/main/docs/linter) . Think of the Linter as an AI-powered style guide enforcer. Set your style rules right in your ReadMe project, then run the Linter on any Guides or API Reference page to get a Style Guide Score and a list of style guide violations and suggested changes. You can also run the Linter on every page in your documentation from the Linter page.


Warnings


2 Issues


Jargon


The content uses unexplained internal jargon or acronyms that aren't expanded or clarified for general readers. (near "API Reference / ReadMe Basics")


Uncertain Language


The content includes uncertain wording or editorial suggestions that present options rather than definitive guidance. (near "/* Note to editors: Consider")


## Successful Linting = Your Style Guide + Effective AI Prompts


The Linter uses your style guide as an AI prompt. When you run it, the Linter sends your documentation page and your prompt to an AI model, which evaluates the content against your rules and surfaces violations with suggested fixes.


A great Linter setup combines two things: a style guide that reflects your team's specific decisions, and prompts written clearly enough for an AI model to act on them. Here are some suggestions for setting your Linter up for success.


- **Write specific, detailed rules.** The more specific you can be, the better. Use specific numbers wherever possible. "Keep sentences under 20 words" will be more effective than "write shorter sentences." The more important the rules are, the more specific you should be.
- **Add examples.** Models respond better when shown specific format requirements. Wherever possible, include a bad/good example pair.
- **Favor rules that explain what to do over rules that explain what not to do.** This gives the model a better understanding of your intentions and helps it generate better suggestions.
- **Keep your style guide current.** A small set of fresh, accurate rules is more effective than a large collection in various states of disrepair. As your product and team evolve, your rules should too.
- **Pull your most specific, checkable rules into the Errors and Warnings sections so the Linter can flag them precisely.** These rules get pulled out into individual lists and flag individual sections for revision. Enterprise customers can also prevent changes from being merged if the Linter detects any Errors.


To learn more about configuring the Linter, check out[the Linter documentation](https://docs.readme.com/main/docs/linter) .
