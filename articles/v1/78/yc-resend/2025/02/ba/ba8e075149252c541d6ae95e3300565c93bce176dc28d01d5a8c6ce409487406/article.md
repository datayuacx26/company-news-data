---
schema_version: "1.0.0"
document_id: "ba8e075149252c541d6ae95e3300565c93bce176dc28d01d5a8c6ce409487406"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/agent-experience"
published_at: "2025-02-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:4ce6d917fe0d99b67365ab5d730563a1dc3700168aee3e467667cdcc1d6cd6e8"
---

# What is AX (Agent Experience) and how to improve it

The world is changing.


Humans are not the only ones performing tasks; AI agents are doing so as well.


Soon, AI agents will be an integral part of our lives. Agents can now use their own browser to perform tasks for you. Agents can write code and even deploy it to the cloud.


For the past couple of years, I've been obsessed with DX (Developer Experience). In fact, the reason why Resend exists is because of how poor the developer experience was for sending emails.


Now, we're entering a new era, an era in which AX (Agent Experience) is just as important. AX doesn't replace DX; it extends it.


*P.S.: If you're developing AI agents today and want to allow them to send emails, we would love to hear from you.*


## What is DX (Developer Experience)?


People define DX (Developer Experience) in a lot of different ways. Here's my own definition:


> DX is the sum of all the details. It's the art of making a product feel amazing for a specific group of people — developers.


Here's how it works in practice:


-


**Poor DX:** You try to use a product, but the docs are outdated. You call the API, but the error messages are unclear. You reach out to support, but they don't respond, and you feel blocked.


-


**Great DX:** You download an SDK, and *it just works* . You share with your team, and they can integrate easily. Once you move to production, you have clear metrics and alerts to monitor your usage. If you run into issues, you can get a timely answer from someone knowledgeable.


## What is AX (Agent Experience)?


A couple of weeks ago, Mathias Biilmann coined the term AX (Agent Experience) in this[blog post](https://biilmann.blog/articles/introducing-ax/) . Here's how he defines it:


> Agent Experience is the holistic experience AI agents will have as the user of a product or platform.


To make this more concrete, let's look at the difference:


-


**Poor AX:** You give an agent a task, and it fails. The agent tries multiple paths to complete the task but spends way too many credits/tokens to do so. You have to repeat yourself over and over again, and eventually take over as a human operator.


-


**Great AX:** It performs the task, and it's exactly what you wanted. The agent is able to perform each action in a single run. The process is cost-effective, and there's no human intervention needed. It makes you want to do it again.


## Building great products


The fact is that building great products is hard.


It requires a deep understanding of your users, their needs, and their pain points.


When it comes to UX, DX and AX, the challenge is even greater because you're not just building for one type of user - you're building for both humans and AI agents.


Let's explore some key areas where DX and AX intersect and diverge, as well as how to optimize for both. Note that this is not an exhaustive list.


UX, AX, and DX


## Challenge #1: Onboarding


Onboarding is the art of getting a new user to a point where they can start using the tool.


-


**DX:** Humans should experience the "wow" factor of a product as soon as they sign up. To make this happen, a great product needs to instruct the user with a few clear steps and reduce friction at all costs.


-


**AX:** Agents will pick whatever tool is easiest to perform a task. If you ask an agent to send an email, they won't be able to wait 2 days for a service to manually review and authorize their sending. Agents are simply not going to "contact sales" or "book a demo". Agents should be able to get up and running in milliseconds.


## Challenge #2: Documentation


When it comes to developer tools, we've always believed that documentation is not adjacent to the product — documentation is the product.


-


**DX:** Humans must be able to easily scan and understand. Great documentation is concise, up-to-date, and complete. Developers can easily spot bad documentation, so even the smallest typo matters.


-


**AX:** LLMs rely on website information to fetch up-to-date details, but they face some limitations: context windows are too small to handle hundreds of web pages with heavy HTML markup and JavaScript. Agents need an LLM-readable format like[llms.txt](https://llmstxt.org/) to operate efficiently.


## Challenge #3: SDKs


SDKs provide developers with pre-built libraries to integrate with a service more easily.


-


**DX:** People don't want to learn a new language for your product. That's why having SDKs written in the developer's favorite programming language is crucial. Great SDKs feel native to the language they are written in and have additional features for that platform — they are not just an HTTP client wrapper.


-


**AX:** Agents have the power to write their own SDKs if needed, so SDKs are less important. Instead, a well-structured REST API is the best "SDK" for an AI Agent. It should use the best HTTP verbs, be predictable and consistent, and include all necessary endpoints. In a world where[MCP servers](https://www.anthropic.com/news/model-context-protocol) are fully supported, having an OpenAPI spec is more important than ever.


## Challenge #4: Activities/Audit Logs


Audit Logs provide a chronological record of activities and events within a system.


-


**DX:** Logs are essential for security, compliance, and debugging. For example, if an engineer takes the production system down, you can check the audit logs to see what they did. Being able to quickly filter for specific actors, and event types is key. Streaming audit logs to your own data warehouse is also important.


-


**AX:** Agents will perform tasks on your behalf, but, as we all know, they can hallucinate. When they make mistakes, you'll want to triage and **differentiate** between human and agent actions. You will also want to know **when** and **where** they made the mistake. Every action should be logged to give you full visibility.


## Challenge #5: API Keys


API keys are the most common way to authenticate. They are simple to use and understand, but they also come with their own set of challenges.


-


**DX:** A human creates an API key, then shares with their team via a password/secret manager. This token is used to interact with an API, and is not shared with the public. Once in production, setting and monitoring rate limits becomes very important.


-


**AX:** Agents are already able to use API keys, but they will also need to **create** them. Having API keys that have permission to create other, less permissive API keys is a must-have. Agents should also be able to delete and rotate keys.


## Challenge #6: RBAC (Role-based access control)


Instead of assigning permissions directly to users, RBAC assigns permissions to roles and helps enforce the principle of least privilege.


-


**DX:** Different team members can have different permissions. For example, senior developers in a team can have an` admin` role for full access, while junior developers may have a` viewer` role for read-only access.


-


**AX:** Agents will require much more granular permissions than human users. Agents might need multiple permissions to perform a specific task instead of a single role. Both granularity and specificity are key. High-risk actions, however, like destructive APIs, may need confirmation from a human operator.


## Further reading


If you're interested in learning more about AX, here are some great resources:


- [Introducing AX (Agent Experience)](https://biilmann.blog/articles/introducing-ax/)
- [The rise of AI-first Developer Experience (DX 2.0)](https://kenneth.io/post/the-rise-of-ai-first-developer-experience-dx-20)
- [Designing APIs for AI: The Need for New Standards](https://medium.com/aimonks/designing-apis-for-ai-the-need-for-new-standards-700a606e9a10)
- [Great agent experience starts with great collaboration](https://liveblocks.io/blog/great-agent-experience-great-collaboration)


## Conclusion


It's important to understand that DX and AX are not mutually exclusive.


In fact, many of the great DX decisions we made at Resend are now paying off on the AX front.


If you're building AI agents, and would like to enable your users to send emails, please get in touch with us.


We want to make Resend not only the *"email for developers"* , but the *"email for agents"* too.
