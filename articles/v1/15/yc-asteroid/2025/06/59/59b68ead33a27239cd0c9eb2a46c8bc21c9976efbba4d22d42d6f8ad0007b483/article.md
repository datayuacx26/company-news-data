---
schema_version: "1.0.0"
document_id: "59b68ead33a27239cd0c9eb2a46c8bc21c9976efbba4d22d42d6f8ad0007b483"
company_key: "yc-asteroid"
company: "Asteroid"
source_id: "yc-asteroid-news-import-1b74f0bf6416"
canonical_url: "https://asteroid.ai/blog/building-reliable-browser-automation/"
published_at: "2025-06-03T00:00:00+00:00"
first_seen_at: "2026-07-24T08:09:50.096820+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:54f27a2e51397aea52bb576fce73b7cd3494b6276e776a7012593d51e694ccb6"
---

# After browser agent version 47, you need an eval

At Asteroid, we were among the first to deploy OpenAI’s Computer Use models for mission-critical workflows.


Filling out customer data in insurance portals or healthcare forms demands significantly higher reliability than using browser agents for tasks like website testing, where occasional failures aren’t serious.


Here are a few lessons and principles around browser agent evaluations (evals) that help us put browser agents into production.


## Task Evaluation Priority Matrix


Building browser agent evals have time tradeoffs, so first just quick distinction on where evals are useful and where the time investment is not justified.


> **💡 TLDR:** Evals are needed when building **mission-critical automated workflows** .


**Best-effort tasks (“find me a restaurant in London”)** **Mission-critical tasks (“Update EHR system with this patient data: …”)**


**One-off tasks (runs once)** - ✅ Evals useful


**Automated workflows (runs 500 times a day)** ✅ Evals useful **🚨 Evals critical**


## Evaluations for Browser Agent Workflows


### Why we Bother with Evals at Asteroid?


With one of our customers, we built a browser agent that handles sensitive personal information daily through a complex booking system. Over many iterations—tweaking prompts, tools, guardrails, and models—we reached version 47 of the agent. It was becoming impossible to tell whether our updates were actually improving performance.


It was time to get serious about rigorously tracking agent performance and introducing specialized tools and safeguards. **Computer-use agents** , in particular, pose unique challenges for evaluation and guardrails due to their complexity and access to a wide range of systems.


Through this work, we’ve arrived at a few key principles for deploying reliable browser agents:


## Principle 1: It’s Better to Fail Safely Than to Succeed Incorrectly


> **⚠️ Issue:** Due to[automation bias](https://en.wikipedia.org/wiki/Automation_bias) people blindly trust agent results that look correct—even when they’re wrong. That’s what makes them dangerous.


If an agent enters the wrong patient information, it could cause real harm. But if it simply says, “I couldn’t complete this,” a human can step in and correct it. There are two kinds of mistakes:


- **False Positive** : The agent *thinks* it succeeded, but it actually got it wrong. (This is the dangerous one.)
- **False Negative** : The agent *thinks* it failed, even though it could have succeeded. (This is safer—someone can check and help.)


We prefer false negatives. It’s much easier to recover from a safe failure than to clean up after a silent mistake.


## Principle 2: Browser Agents Need Specialized Human Review Interfaces


> **⚠️ Issue:** Browser agents often take 30+ minutes; humans supervisors need efficient review tools.


We expect browser agent builders—especially non-technical users—to spend more time **reviewing** agent progress than watching agents in real time. That’s why we focused on building the most efficient review interface possible.


A well-designed review interface can **dramatically reduce the time** spent validating agent actions. Instead of watching a 30 minute video, analyse all the actions in 1 minute.


**Asteroid’s post-execution interface**


:::arcade src=“[https://app.arcade.software/share/oQsmkdcjsmExxu6x0OyM](https://app.arcade.software/share/oQsmkdcjsmExxu6x0OyM) ” :::


Videos, screenshots and agent progress updates blend together. It’s possible to find exact timestamp llm message and connect it to the video timestamp and screenshot.


## Principle 3: Implement offline evaluations with multiple LLM-as-a-judge models


> **⚠️ Issue:** Computer use model self-evaluating itself is not reliable enough.


Computer Use models (like OpenAI’s CUA) are trained to achieve goals using computer tools, but they aren’t reliable judges of their own performance.


We instead use multimodal LLMs, capable of analyzing screenshots, agent reasoning, and outcomes, providing robust baseline evaluations. Employing multiple LLM judges further improves detection of nuanced failure modes.


## Principle 4: Build agent guardrails and evaluate in production environments


> **⚠️ Issue:** Saying: *“Please don’t click on this.”* does not work. Prompts alone are insufficient guardrails for browser agents, yet creating simulated environments is impractical.


Complex and critical workflows cannot be reliably evaluated directly on live production systems—customers understandably object to incorrect data submissions or accidental record deletions “while the agent was training.” However, the most accurate evaluations happen when using the exact production workflow.


We address this by implementing **deterministic guardrails** at the browser interaction layer—enforcing restrictions through code rather than relying on prompts.


For example, when submitting an insurance quote, the agent can fill out the entire form, but we block the final submission by intercepting the selector associated with the submit button.


**Examples of deterministic guardrails:**


- **Selector-based Guardrails**


- Example: Prevent clicking on an element like` button.navigation-continue:has-text("Submit")` by explicitly detecting the elements in Playwright.


- **URL-based Guardrails**


- Example: Enforce navigation restrictions exclusively to approved URLs such as` company_domain*` by handling URL navigation checks in code.


## Conclusion


We imagine a future where millions of browser agents carry out complex tasks with **surgical precision** —learning, adapting, and scaling effortlessly.


But getting there isn’t easy. The **cold start problem** —turning early experiments into robust agents that run thousands of times a day—remains a serious challenge.


At Asteroid, we’re building the most **reliable browser agents on the planet** —designed to operate at enterprise scale, with human oversight where it matters.


Want to start building with us? We’re onboarding new teams to the Asteroid platform:


[Get in touch](https://form.typeform.com/to/UFNMztI0)
