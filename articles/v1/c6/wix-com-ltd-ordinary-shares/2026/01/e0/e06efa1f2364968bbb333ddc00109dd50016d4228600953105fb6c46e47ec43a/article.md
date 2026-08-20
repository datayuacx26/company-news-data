---
schema_version: "1.0.0"
document_id: "e06efa1f2364968bbb333ddc00109dd50016d4228600953105fb6c46e47ec43a"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/why-i-stop-prompting-and-start-logging-the-design-log-methodology"
published_at: "2026-01-05T07:29:38+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T22:24:14.621256+00:00"
content_hash: "sha256:d23d6d8268f6a27bbd4b143db7cf934b87fbf97523dd1b915a892748fa7f9404"
---

# Why I Stop Prompting and Start Logging: The Design-Log Methodology

If you use AI to code, you’ve felt the "Context Wall." You start a project, things go great, but as the codebase grows, the AI starts making suggestions that conflict with previous decisions. You spend more time correcting the model than writing code


To solve this, I developed the


**Design-Log Methodology** . It started as a way for me to remember my own decisions in large projects, but it has become the single most important tool in my AI-coding workflow. The AI is no longer just a "coder"; it is a partner in the architectural process.


##


What is a Design Log?


A design log is a version-controlled folder


**./design-log/** in your Git repository. It contains markdown documents that serve as a "snapshot in time" of a specific design, decision or feature.


Unlike a README or traditional documentation that tries to stay "current" (and often fails), a design log is


**true to the time it was written** . It’s the "brain" of your project that the AI can read before it touches your code.


##


The Methodology in Action: Adding Server Actions


To see why this works, let’s look at a real-world example from the


[Jay Framework](https://github.com/jay-framework/jay) . I wanted to add a way for the client to call the server—a standard "Server Actions" feature.


###


1. The Simple Prompt


I didn't give the AI a 10-page spec. I gave it a high-level intent:


*"In Jay, we do not have a way for the client to call the server. Let's add this capability. For example, for a product page 'add to cart' button."*


###


2. The AI Becomes the Architect


Because of my project rules, the AI didn't start coding. It created


[Design Log #63](https://github.com/jay-framework/jay/blob/main/design-log/63%20-%20jay-stack%20server%20actions.md) and began a discovery process. It identified related project patterns (like


JayContract


) and asked:


-


*How should we handle type-safe arguments?*


-


*How do we manage the "Loading" state on the client?*


We answered these in the


**Questions and Answers** section of the log. We were "coding" the logic in English before writing it in TypeScript.


###


3. Reviewing the Blueprint


The AI then proposed a specific design:


-


**The Server API:**


```text
export const     searchProducts = makeJayQuery('   products.search    ')
.withServices(PRODUCTS_DATABASE_SERVICE)
.withCaching({ maxAge:    60     })
.withHandler(async (
input: { query:    string    ; page?:    number     },
productsDb,
) => {}
```


-


**The Client API:**


```text
import     { searchProducts }    from        '../../actions/search.actions'    ;
const     response = await searchProducts({ query: query() });
```


I reviewed this markdown file. We agreed on the APIs and the implementation plan. No surprises.


###


4. Zero-Drift Implementation


During the build, any deviation—like a specific way to handle serialization—was appended to the


**"Implementation Results"** section. The AI even suggested creating secondary logs when a change was too large for the original scope.


###


5. From Idea to Production in 48 Hours


The most impressive part? We completed the entire feature—from that first vague prompt to a polished, tested Pull Request—in


**just two days** .


##


The Secret Sauce: Why This Works


The reason


[Design Log #63](https://github.com/jay-framework/jay/blob/main/design-log/63%20-%20jay-stack%20server%20actions.md) was so successful isn't just because the AI is smart—it’s because the project rules force a specific behavior. This creates three massive shifts in the workflow:


-


**The "Magic" Simple Prompt:** Because the AI


*must* read the logs before writing code, I can give a one-sentence prompt. The model finds the context it needs and builds on top of it automatically. No more re-explaining my architecture every 10 minutes.


-


**Socratic Collaboration:** Instead of guessing and hallucinating, the AI identifies gaps in my logic. It uses the


**Questions & Answers** section to clarify edge cases


*before* they become bugs. We finalize the design in Markdown where changes are "cheap."


-


**Traceable Implementation:** If the AI needs to deviate from the plan during coding, it must document


**why** in the "Implementation Results." This prevents "context drift" and ensures the codebase doesn't move faster than the documentation.


##


The Rules of the Game


To make this work, I add these four pillars to my AI's system instructions:


1.


**Read Before You Write:** Check


./design-log/


for existing decisions and modules for reuse before any change.


2.


**Design Before You Implement:** A log must be created and approved before a single line of production code is written.


3.


**Immutable History:** Once implementation starts, the "Design" section is frozen. All changes are appended as results.


4.


**The Socratic Method:** Missing info? The AI must ask questions in the log, and the answers become a permanent part of the record.


**The result: Building is faster, higher quality, and—most importantly—predictable.**


##


Want to try it?


Here is the rule I use in my projects. You can drop this into your AI's instructions to start your own design log:


```text
---
description: "This rule provides standards for design log files"
alwaysApply: true
---


# Jay Framework Project Rules


## Design Log Methodology


The project follows a rigorous design log methodology for all significant features and architectural changes.


### Before Making Changes
1. Check design logs in `./design-log/` for existing designs and implementation notes
2. For new features: Create design log first, get approval, then implement
3. Read related design logs to understand context and constraints


### When Creating Design Logs
1. Structure: Background → Problem → Questions and Answers → Design → Implementation Plan → Examples → Trade-offs
2. Be specific: Include file paths, type signatures, validation rules
3. Show examples: Use ✅\❌ for good/bad patterns, include realistic code
4. Explain why: Don't just describe what, explain rationale and trade-offs
5. Ask Questions (in the file): For anything that is not clear, or missing information
6. When answering question: keep the questions, just add answers
7. Be brief: write short explanations and only what most relevant
8. Draw Diagrams: Use mermain inline diagrams when it makes sense


### When Implementing
1. Follow the implementation plan phases from the design log
2. Write tests first or update existing tests to match new behavior
3. Do not Update design log initial section once implementation started
4. Append design log with "Implementation Results" section as you go
5. Document deviations: Explain why implementation differs from design
6. Run tests: Include test results (X/Y passing) in implementation notes
7. After Implementation add a summary of deviations from original design


### When Answering Questions
1. Reference design logs by number when relevant (e.g., "See Design Log   #50   ")
2. Use codebase terminology: ViewState, Contract, JayContract, phase annotations
3. Show type signatures: This is a TypeScript project with heavy type usage
4. Consider backward compatibility: Default to non-breaking changes
```


##


Conclusion


The end result is a development process that is


**faster, higher quality, and much more predictable.** I no longer hope the AI understands my project’s "vibe"—I know it understands the architecture because we wrote the history together.


##


Resources


1.


Yoav’s GitHub repository, where he posts updates on new versions of the rules and methodology:


[yoavaa/design-log-methodology](https://github.com/yoavaa/design-log-methodology/tree/main?tab=readme-ov-file)


2.


Deep Dive Presentation:


[Download the Presentation PDF](https://github.com/yoavaa/design-log-methodology/blob/main/Why%20I%20Stop%20Prompting%20and%20Start%20Logging_%20The%20Design-Log%20Methodology.pdf)


This post was written by


**Yoav Abrahami**


You can


[follow him on X](https://x.com/yoavabrahami)


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/)


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


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) or


[Spotify](https://open.spotify.com/show/5CmjtjpdcKkHDnr0601uYS?si=PcOf7Rx_RUmGojFj5n7CEA)
