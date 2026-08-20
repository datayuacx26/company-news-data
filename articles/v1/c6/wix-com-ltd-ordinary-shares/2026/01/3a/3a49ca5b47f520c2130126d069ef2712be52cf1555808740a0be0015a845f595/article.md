---
schema_version: "1.0.0"
document_id: "3a49ca5b47f520c2130126d069ef2712be52cf1555808740a0be0015a845f595"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/from-syntax-checker-to-critical-reviewer-how-we-forced-ai-to-catch-real-api-quality-issues"
published_at: "2026-01-07T10:09:21+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T22:24:01.443100+00:00"
content_hash: "sha256:ed5bc72b0b945dfa09bddbcb972a7148f62d98243f9bac5d2888c18a633cfe52"
---

# From Syntax Checker to Critical Reviewer: How We Convinced AI to Catch Real API Quality Issues

Building an AI tool to review API design taught us something fundamental about how AI approaches quality: it confidently optimizes for the least important parts.


Instead of flagging the quality issues that actually hurt developers, like confusing workflows, hidden rules, or inconsistent mental models, it focused on the safe, measurable stuff: naming patterns, formatting mistakes, even typos. Developer experience issues just didn’t get caught, even though in our case, we specifically asked for them.


The scary part? This wasn't a bug or a mistake in our setup. It was a built-in bias.


Every test we ran produced the same result: technically “correct” reviews that completely ignored the real usability problems that slow developers down and kill adoption.


Below I break down how we uncovered this gap, why AI naturally gravitates toward easy technical fixes instead of real quality, and what we had to change to force the model to behave like a critical reviewer instead of a syntax checker.


##


**The Technical Trap: Why AI Chooses Easy Over Important**


We asked an AI to review an API design. What do you think it caught?


-


TODO notes in field descriptions ✓


-


Inconsistencies between message names and descriptive texts in proto files ✓


-


FQDN formatting errors ✓


-


A service type design that confuses every developer who touches it...


*crickets*


From our very first tests, the pattern became crystal clear: AI gravitates toward technical compliance. This makes sense; it's measurable, objective, and safe. Grammatical errors and pattern inconsistencies have clear right and wrong answers. Developer experience issues, though, require understanding workflows, mental models, and user context. This is computationally expensive work that AI avoids unless you force it to


.


##


**AI Was Missing the Forest for the Trees**


This pattern played out clearly in our Services API review. The AI tool easily caught technical issues like:


-


Field descriptions that said only "Description of value" instead of explaining the field's purpose.


-


Boolean fields using inconsistent naming patterns (\`require_manual_approval\` vs \`manual_approval_required\`).


But it completely missed major usability problems. This API has three service types within the same primary object:


-


APPOINTMENT (like a doctor visit),


-


CLASS (like a yoga class),


-


COURSE (like a multi-week cooking series).


Each type has completely different rules: appointments require staff members but classes don't, appointments are limited to 1 person but classes can have higher capacity, different types have different location restrictions and session duration rules.


A developer trying to create their first appointment service would need to navigate conditional field requirements, understand booking domain concepts, and decipher which rules apply when. The cognitive load was enormous, and these issues aren’t clearly documented for the user, but because each individual rule was technically correct, the AI never flagged it as a problem.


Classic case: technically correct, but completely confusing.


AI was just doing what it was trained to do - pattern match against explicit rules. But the biggest developer experience issues often live in the spaces between the rules, in the cognitive load, in the "wait, what?" moments that slow developers down.


Whenever I asked the AI how to help it catch these issues, it suggested elaborate solutions including workflow simulation phases, cognitive load budgeting, mandatory continuation phrases.


We considered these recommendations during our research but decided against them. So we had to rethink our approach entirely. The fixes we went with in the end were simpler and more direct.


##


**Rebalancing the Prompt**


We heavily weighted developer usability throughout the entire prompt so that it’s the primary focus throughout. Instead of treating quality as one item among many technical checks, we restructured the instructions to emphasize developer experience as the primary focus. The prompt now explicitly states that the review "must be an independent and critical assessment based on established API design principles and developer experience best practices" and instructs the AI to "put yourself in the developers' shoes" and "flag everything that likely negatively affects the external developer's experience."


We also added a validation step directly in the prompt. The AI now must explicitly verify each issue using this checklist:


-


"I have verified the actual implementation,"


-


"This affects external developer experience,"


-


"The current approach is genuinely problematic,"


-


"The recommendation would be a real improvement,"


-


"The recommendation resolves the issue. "


The prompt also requires that the tool add a verification summary to the report. It can't proceed to the final report without completing this verification for every single issue.


##


**Rebalancing the Technical Guidelines**


I also realized that the very fact that I provided a hard-coded list of technical guidelines was skewing the AI toward finding technical issues, even though these were far from the only things we expected it to catch. So we heavily weighted developer usability throughout the entire list of guidelines, as well:


-


Clearly defined developer experience as the foundational principle underlying all of the technical guidelines.


-


Added a “developer impact” sentence to every section of the guidelines to link the technical issues to the foundational developer experience principle. For example:


*“Developer Impact: Developers expect APIs to match how they think about business resources, not internal database structures. Incomplete APIs force developers to work around missing functionality they naturally expect.”*


-


Added similar content to individual guidelines that might otherwise be taken as “for technical compliance purposes”. For example:


*“One main resource per service (keeps APIs focused and predictable for developers)”.*


##


**The Results: Quality Issues AI can Actually Catch**


Once we implemented these changes, our AI started catching some of the quality issues it'd been skipping:


-


Hidden cognitive complexity: Required fields that vary by context.


-


Misleading field names: For example, a field called \`submissionState\` that actually contains form values, not the submission status.


-


Workflow friction: Multi-step processes requiring domain expertise.


-


Entity cognitive overload: APIs with single entities containing 35+ fields, overwhelming developers trying to understand the basics.


-


Method naming inconsistencies: Similar operations with completely different naming patterns across the same API.


-


Service naming confusion: Services that aren’t named based on the value they provide. For example, a service named “Form Completions Service” whose value to the user is in providing AI form assistance.


##


**The Bigger Lesson: AI Wants to Help, But Needs Structure**


Here's what surprised me most: AI is actually quite good at quality assessment. It's just quite bad at prioritizing quality over technical correctness, even with explicit direction.


The problem isn't capability, it's priority. Left to its own devices, AI will always choose the easier technical path. But when you can force it to focus on developer experience, it can catch these issues relatively well.


For anyone building AI quality tools, remember: you need to make skipping quality checks computationally harder than doing them.


##


**Why This Matters**


The difference between catching technical issues and quality issues isn't just a nice-to-have; it directly impacts developer success. Technical compliance issues are annoying but rarely stop integration work. Quality issues are adoption killers.


When a developer encounters a confusing service name or hidden field dependency, they don't just fix it and move on. They lose confidence in the API, spend extra time validating their assumptions, and often even abandon integration attempts entirely. These quality issues compound into support tickets, delayed launches, and ultimately, fewer developers successfully building on our platform.


But there's a bigger issue at play here, as AI is increasingly being used not just to review APIs, but to design and build them.


If AI can't evaluate developer experience in its own work, we're going to see a proliferation of technically perfect but practically unusable APIs. The same bias that makes AI default to checking technical compliance over usability will make it design APIs that follow every technical rule while creating terrible developer experiences.


Teaching AI to prioritize developer experience isn't just about better code review, it's about ensuring that as AI takes on more API design tasks, it builds systems that developers can actually succeed with. We need AI that can self-critique on the dimensions that matter, not just the ones that are easy to measure.


##


**What’s Next**


The issue isn’t solved yet. We're still finding new ways the AI defaults back to technical compliance instead of quality assessment. The challenge is ongoing. Every time we solve one bias, we discover another. Current experiments include:


-


Testing different ways to structure the validation step to make it harder to phone-in.


-


Exploring whether separating quality checks from technical checks will improve the results.


-


Experimenting with using multiple personas.


-


Finding the right balance between being explicit about quality priorities without making the prompts so long that they become unwieldy.


The goal is to keep nudging AI toward caring about what actually matters to developers. Each iteration teaches us more about how AI approaches problem-solving and where we might be able to intervene.


This post was written by


**Aliza Solomon**


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/) |


[TikTok](https://www.tiktok.com/@wix_engineering)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/subscribe)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)


-


Listen to our podcast on


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) ,


[Spotify](https://open.spotify.com/show/5CmjtjpdcKkHDnr0601uYS?si=PcOf7Rx_RUmGojFj5n7CEA) or


[Google](https://podcasts.google.com/?feed=aHR0cHM6Ly9yYW5sZXZpLmNvbS9mZWVkL3dpeF9wb2Qv&ved=0CAAQ4aUDahcKEwjY3bLcy7_oAhUAAAAAHQAAAAAQAQ)
