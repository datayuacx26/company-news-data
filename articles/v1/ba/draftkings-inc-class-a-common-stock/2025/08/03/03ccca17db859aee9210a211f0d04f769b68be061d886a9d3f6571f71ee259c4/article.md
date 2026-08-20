---
schema_version: "1.0.0"
document_id: "03ccca17db859aee9210a211f0d04f769b68be061d886a9d3f6571f71ee259c4"
company_key: "draftkings-inc-class-a-common-stock"
company: "DraftKings Inc."
source_id: "draftkings-inc-class-a-common-stock-rss-016c40719db2"
canonical_url: "https://medium.com/draftkings-engineering/ai-meets-engineering-draftkings-real-world-code-review-accelerator-cd35a256c064"
published_at: "2025-08-05T10:00:36+00:00"
first_seen_at: "2026-07-20T04:35:13.112015+00:00"
fetched_at: "2026-08-20T00:36:59.605120+00:00"
content_hash: "sha256:42d6377b8ab710b854a8a2ce19f2bd090fd41207e8ca8efe60c3691f2791653d"
---

# AI Meets Engineering: DraftKings Real-World Code Review Accelerator

### DraftKing’s Internal AI Tool


Ever wonder how you can significantly reduce code review time? Say by 40%? AI tools and experimentation are at the core of DraftKings culture. In this article, we are going to explore DraftCode — the tool that makes this efficiency gain possible.


### DraftCode for a more efficient and smoother review experience


At DraftKings, we care about strong code reviews, but there’s always room for improvement. Pull requests need quick, helpful feedback, so teams don’t waste time going back and forth.


Having a short review time helps us ship new features faster by reducing feedback lag.


DraftCode helps by speeding up the review process. It looks at code changes, adds context, for example, from Jira issues, and uses Large Language Model (LLM) to write suggestions, comments, and summaries. It finds areas of interest early, so reviews stay focused and fast.


### Technical Overview


DraftCode is fully connected to DraftKings’ source control system. It listens for pull request (PR) events, such as when someone opens, updates, or comments on a PR. Afterwards, it uses AI to help with code review.


DraftCode’s high-level flow **:**


- When a **PR is created or changed** , the source control platform **sends a message** to DraftCode to start its review
- DraftCode **builds its context** by collecting code changes, repo settings, and related Jira tickets. Including as much useful information as possible, such as the description of Jira tickets, is crucial for high-quality feedback.
- When the context is gathered, DraftCode builds its custom prompt and sends it to LLM. The model sends back review suggestions and a summary in JSON format.
- After DraftCode parses the LLM response, it does some post-processing of the feedback and posts the most important comments in the Pull Request as comments.


### A review buddy, not a review authority


DraftCode is fast, consistent, and very good at finding certain problems in code, especially when we use custom prompts and settings for each project. No AI fully understands the product or the business; often, the difference between a mediocre and great review is the extra context. You can’t get all the context from the code.


That’s why DraftCode can be used like a second pair of eyes and not the final decision maker. It can help find issues and ask questions, but people still should make the final call. Developers can focus on things that AI cannot; for example, deadlines, business needs, big design choices, flow of data and state, and code history not stored in Git.


**DraftCode** is a tool, not a shortcut.


### DraftCode Code Review Wins


#### Detecting and Fixing Logging Format with DraftCode


**Problem:** Using string interpolation ($”…”) inside a logger.Warning call.


**Why it matters:** This breaks structured logging and makes it hard to search logs in tools like Datadog.


**Fix:** Instead of string interpolation, use message templates with parameters. This keeps logs clean and easy to search.


#### Risky Async Code with DraftCode


**Problem:** Calling .GetResult() on an async method without exception handling.


**Why it matters:** This can block threads or crash the app, especially in high-traffic services.


**Fix:** Use await instead, and check for null or invalid results like DraftCode suggested.


#### YAML Formatting Mistake Found by DraftCode


**Problem:** Extra trailing whitespace in a helmrelease.yaml file.
**Why it matters:** It looks fine to people but can break CI/CD pipelines and stop deployments.
**Fix:** DraftCode finds these hidden issues early, so you can fix them before they cause downtime.


### From Assistant to Strategic Enabler


Beyond merely assisting with code reviews, DraftCode now proactively supports **:**


- **Automated Release Notes Generation:** Summarizing essential changes clearly and concisely:


> Summary of changes


> The pull request introduces support for validating multiple Jira tickets in a single pull request within the service. This enhancement is primarily targeted at the repository to handle multiple ticket validation as per the user story requirements. The changes involve modifications to existing services and interfaces to accommodate multi-ticket validation.


> **JiraContextBuilder.cs** : Added a new method GetJiraContextsAsync to retrieve multiple Jira contexts based on potential ticket IDs extracted from the pull request title. This method logs an error if no valid Jira ticket is found and returns a list of Jira context.


- **Prompt-driven Enhancements:** Using custom prompts and DraftCode’s configuration, engineers can modify DraftCode’s behavior, for example, to emphasise performance or maintainability with:


```text
{    "review_scorer_weights": {    "maintainability_weight": 9,    "performance_weight": 6,    },    "user_prompt": "Pls be sure that it used feature of C# 12, be sure that initialization of array like this\"[]\" are correct for c# 12."  }
```


- **Advanced Observability:** Integrated analytics and metrics to monitor effectiveness and provide continuous feedback for improvement.


Lines of code reviewed per language


### Pitfalls and Limitations


DraftCode provides helpful tips for many technologies, but it doesn’t work the same for all tech stacks. For example, when reviewing SQL pull requests-especially Snowflake SQL-it might give false warnings or basic advice. This happens because DraftCode can’t clearly see schema connections and stored procedures. That is why its SQL suggestions are sometimes general or incorrect.


To fix this issue, we have built in repo-specific prompts that can be set and maintained by engineers, which can provide the extra context to DraftCode. Projects with custom prompts have significantly higher accepted comments.


### The Power of Prompting


Prompting capabilities enable DraftCode to handle dynamic queries from engineers, adapt to specific contexts, and continuously improve its accuracy. Metrics and analytics are used to refine prompting strategies and ensure continuous enhancement of the system’s efficacy and user satisfaction. For LLMs, the prompt is one of the important things, and each engineer should keep that in mind. That means you need to maintain and keep up-to-date repo-specific prompts by yourself or with automation.


### Conclusion


DraftCode reflects DraftKings’ commitment to responsible innovation in AI, streamlining the code review process while keeping engineers at the center. By handling routine checks and surfacing potential issues early, it allows human reviewers to focus on the critical decisions that require expertise, context, and judgment.


By integrating AI into the development lifecycle, DraftCode doesn’t replace human reviews — it enhances them. Engineers remain fully responsible for code quality, safety, and compliance, ensuring every change meets DraftKings’ high standards before it ships.


---


[AI Meets Engineering: DraftKings Real-World Code Review Accelerator](https://medium.com/draftkings-engineering/ai-meets-engineering-draftkings-real-world-code-review-accelerator-cd35a256c064) was originally published in[DraftKings Engineering](https://medium.com/draftkings-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
