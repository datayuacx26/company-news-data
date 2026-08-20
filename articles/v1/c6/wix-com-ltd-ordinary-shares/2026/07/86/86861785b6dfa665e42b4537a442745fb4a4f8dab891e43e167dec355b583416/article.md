---
schema_version: "1.0.0"
document_id: "86861785b6dfa665e42b4537a442745fb4a4f8dab891e43e167dec355b583416"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/how-to-build-ai-agents-that-fix-themselves-without-bigger-prompts-more-context-or-another-llm"
published_at: "2026-07-16T06:17:34+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:c859b048c497250d9cd4487745148aa179d8ac809d1cc47b1f3603bf3a577c65"
---

# How to Build AI Agents That Fix Themselves - Without Bigger Prompts, More Context, or Another LLM

Every developer building with AI agents eventually hits the same wall. You write a masterpiece of a system prompt, loaded with rules, edge cases, and brand guidelines. It works perfectly... until you add one more rule.


Suddenly, the agent gets "context fatigue," ignores half your instructions, and hallucinates a creative but entirely broken solution.


We need to stop trying to make agents perfect on the first try. Instead, we suggest a new architectural approach: organizing agentic workflows around a structured


**Feedback-Driven Agent (FDA)** pattern. Rather than relying solely on giant upfront instruction sets, we propose a clear separation between


**Structural Integrity** and


**Goal Assessment** .


##


**Stage 1: The Standalone FDA Loop (Enforcing Structural Integrity)**


At its core, the FDA loop is a self-contained, self-healing loop designed to enforce non-negotiable structural requirements (like code syntax, design system tokens, API schemas, or legal compliance contracts). It does not require another LLM to act as a judge, and it operates beautifully entirely on its own:


The core of the


**FDA loop** is a continuous, self-healing circle:


1.


**AI Agent Drafting:** The agent takes an initial run at a task based on lightweight, minimal instructions, operating with virtually zero upfront context window bloat.


2.


**Task Output:** The agent produces an actionable artifact—such as generated


**Code** or a structural


**API Call** .


3.


**Deterministic Validation Engine:** Instead of using another unreliable LLM to guess if the output is correct, a strict programmatic engine tests the artifact against deterministic constraints (like Contracts, Design Tokens, Accessibility rules, or API Schemas).


4.


**Smart Error Reporting:** If the validation fails, the system generates a structured report. Crucially, it doesn’t just say "failed"; it includes explicit links to specific guides, skills, and instructions needed for immediate self-repair, routing the agent back to Step 1.


This loop runs continuously until the output is structurally pristine. Because it relies on deterministic code checks, it runs in milliseconds and costs fractions of a cent—providing an ironclad foundation for agent output without the need for high-level reasoning.


##


**Stage 2: The Hybrid Pattern (FDA + Goal-Based Loops)**


While the standalone FDA loop is perfect for deterministic structural tasks, we hit a different challenge when dealing with creative, non-deterministic problems. If you ask an agent to


*"optimize this layout to increase conversion rates,"* there is no single "correct" code output. The agent needs room to explore, experiment, and try different variations.


This is where the mainstream approach usually fails. Most architectures rely entirely on "multi-agent loops"—having a second "critic" agent review the work of the "writer" agent. While easy to set up, we suggest it introduces significant engineering risks if used as the sole gatekeeper:


-


**Structural Blind Spots:** A goal-based reviewer agent is highly focused on completion. It might happily approve a layout that technically works on the surface, completely ignoring spaghetti code, security vulnerabilities, invalid design tokens, or broken accessibility guidelines underneath.


-


**The Student-Grading-Student Dilemma:** Expecting a probabilistic model to thoroughly review code quality without an answer key is unrealistic. The reviewer agent can easily miss subtle bugs, hallucinate errors that don't exist, or simply "agree" with bad work to resolve the loop.


###


**The Suggestion: An "Integrity-First" Hybrid Pipeline**


To solve this, we suggest combining both approaches into a highly optimized, two-tier hybrid system. By running the creative explorer


*inside* the guardrails of the deterministic FDA loop, you ensure that only standard-compliant, production-grade candidates ever make it to the desk of the high-level Goal Judge.


Placing the FDA loop before the goal-based loop yields major advantages:


##


**Why "Compiler Feedback" Isn't Enough**


Skeptics might argue: "How is this different from letting an LLM read raw terminal compiler errors until it compiles?"


We suggest it is completely different. Raw, noisy tracebacks often cause models to enter endless, circular hallucination loops. They try a bad fix, get a different error, revert, and repeat.


FDA is built on


**Dynamic Skill-Mapping** . When a test fails in the FDA model, the validator matches the exact error to its corresponding "how-to" documentation or specific execution skill, serving the agent a tailored recipe for repair. It's not just an error log; it's an inline injection of the specific knowledge needed to solve the problem.


##


**Three Real-World Blueprints of FDA in Action**


This isn't just theory. Here is how we are actively putting the FDA model to work across different layers of software development.


###


**1. The Jay Framework: Guardrailing the Frontend**


The


[Jay Framework](https://github.com/jay-framework/jay) is a JS framework built from the ground up specifically for AI agents, completely designed around this feedback loop. Rather than asking an agent to write complex Javascript, the agent codes in


**Jay HTML** —a declarative, highly structured hybrid of HTML, CSS, and minimal Jay tags.


Because the code is declarative and deterministic, we can run it through an incredibly wide, robust suite of validators:


-


**Contract Files:** Verifying that data tags, interactive components, and variants match headless component definitions.


-


**Design Tokens:** Checking colors, spacing, and typography against a


[design.md](http://design.md/) specification.


-


**Production Standards:** Running automated SEO, accessibility (a11y), and Wix Media CDN optimization checks (ensuring images are uploaded to the CDN, sized correctly, and use proper


srcset


parameters).


If the agent makes a mistake, the framework instantly hands it a highly focused report:


```text
📦   design-system-validator/design-tokens
⚠ src/pages/page.jay-html
<a class="home-logo" > "onsko" — font-size value "32px" not in typography tokens
[(max-width: 600px)] <a class="home-logo" > "onsko" — font-size value "32px" not in typography tokens


Suggestions:
Use a src/pages/DESIGN.md typography token
See design-system-validator agent-kit/designer/design-system.md for usage guide
```


Instead of guessing, the agent immediately knows what to do and converges on a perfect website within a few rapid cycles.


###


**2. The Jay WebMCP Plugin: Defeating Context Bloat**


Exposing interactive elements to a WebMCP agent can easily amount to dozens of tools—from picking product variants, to adding items to a cart, to cycling through galleries or applying product filters.


If you feed the full schemas, valid values, and parameter formats of all these tools to the agent upfront, you


**blow the context window** , slow down response times, and pay a heavy token tax.


**The FDA Solution (Lazy-Loading Tools):** We register all tools with the WebMCP agent


*without* any parameter descriptions. When the agent tries to call a tool, it naturally makes a naive attempt. The validator intercepts the call, blocks it, and returns a rich "smart error" containing the exact schema, expected values, and documentation for


*only that specific tool* . The agent reads the error, gets the precise footprint, and calls it correctly on the second try.


###


**3. Self-Validating APIs: The "Heal on HTTP 400"**


My colleague Kfir Bloch proposed a brilliant concept that reshapes how agents interact with APIs:


**Why should an invalid API call just return a generic** **HTTP 400 Bad Request** **?**


Instead of a generic error, the API returns a self-validating payload designed specifically for machine consumption. It treats the agent like a self-repairing client:


```text
// HTTP 400 Bad Request (Designed for Agents)
{
"status": "error",
"error_type": "INVALID_PARAMETERS",
"message": "Missing 'size' variant for product ID 9921",
"context": {
"expected_schema": {
"product_id": "integer",
"variant": "string (valid: 'S', 'M', 'L')"
},
"remediation_instruction_url": "    https://api.mydomain.dev/docs/errors/err-9921-variants     "
}
}
```


The agent catches the structured response, reads the inline schema and documentation link, updates its payload, and fires a successful second request.


##


**The Takeaway: Build Systems, Not Prompts**


Developing for AI agents is no longer about writing the perfect instruction manual. It’s about building


**resilient infrastructure** that catches errors and points the agent to the right page of the manual at the exact moment they trip.


Stop trying to build a smarter agent. Build a better loop.


This post was written by


**Yoav Abrahami**


You can


[follow him on X](https://x.com/yoavabrahami)


**More of Wix Engineering's updates and insights:**


-


**Follow us on:**[Twitter](https://twitter.com/WixEng) **|**[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/) **** **|** ****[Instagram](https://www.instagram.com/wix_eng/) **** **|** ****[Facebook](https://www.facebook.com/WixEngineering/)


-


**Join our**[Telegram channel](https://t.me/wixeng) ****


-


**Visit us on**[GitHub](https://github.com/wix) ****


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/) ****


-


**Subscribe to our**[YouTube channel](https://www.youtube.com/WixTechTalks) ****


-


[Follow our Medium publication](https://medium.com/wix-engineering) ****
