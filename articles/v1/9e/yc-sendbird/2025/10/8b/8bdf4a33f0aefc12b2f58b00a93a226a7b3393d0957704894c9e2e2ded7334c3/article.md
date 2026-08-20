---
schema_version: "1.0.0"
document_id: "8bdf4a33f0aefc12b2f58b00a93a226a7b3393d0957704894c9e2e2ded7334c3"
company_key: "yc-sendbird"
company: "Sendbird"
source_id: "yc-sendbird-news-import-8f7056c4fa29"
canonical_url: "https://sendbird.com/blog/security/hacking-our-own-ai-agent-security"
published_at: "2025-10-10T07:00:00+00:00"
first_seen_at: "2026-07-22T13:04:56.787986+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:d54a7d873d02d308164f586e872685441cfd593cfb9988f5d60eab7c10bd903e"
---

# Offense Is The Best Defense: Why We Hacked Our Own AI Agent Security to Safeguard Customers

The world of AI is often compared to the Wild West—a new frontier brimming with opportunity, but also rife with uncharted risks around AI agent security. As agents become more integrated into our daily digital lives, securing them becomes all the more critical.


Sendbird has built a world-class security program over the years, establishing a mature, enterprise-grade security posture. To maintain this high bar and secure the next generation of features, we remain laser-focused on protecting our new[AI agents](https://delight.ai/) . Our Security program has always provided hands-on security training for our engineers—but we wanted to take it to the next level.


This resulted in HackTheBird, an internal Capture the Flag (CTF) competition designed to enhance our AI agents' security by thinking like the malicious actors who might target them. More than a training exercise, this was a hands-on journey into the mindset of a hacker. We sought to give our team firsthand experience of the kinds of threats AI systems can face—and how to defend against them—to further advance our already robust security posture.


Following a week of more than 1,700 agent interactions, participation from 40% of our employees across departments, and many late nights of hacking— *using* *a specially configured and intentionally vulnerable AI agent—* here’s what we learned about AI agent security from a hacker’s POV.


**5 key questions to vet an AI agent platform**


[Download the ebook](https://sendbird.com/resources/key-questions-to-vet-an-ai-agent-platform)


## The goal: From abstract threats to concrete lessons in AI agent security


AI agent security is a rapidly evolving discipline, and awareness of its unique challenges and **** emerging threats **** is still growing. It’s one thing to read about theoretical


[AI security vulnerabilities](https://sendbird.com/blog/ai-security) ; it’s another to try and exploit them yourself.


To bridge this gap, we created realistic, hands-on scenarios that challenged our employees to break things purposefully. By encouraging a "think like a hacker" mindset, we turned abstract security concepts into tangible, memorable learning experiences.


The ultimate goal? To leverage these insights to build the safest, most resilient AI agents for our customers.


## The proving ground: Designing realistic **** AI agent security challenges


To create a realistic experience, we built the "Sendbird Shop," a mock ecommerce site powered by a Sendbird[AI agent for retail](https://sendbird.com/ai-agent/industries/retail) . This wasn't just any agent. *We intentionally embedded it with 12 distinct vulnerabilities mirroring real-world implementation challenges.*


*We built an ecommerce store and retail AI agent with 12 unique security vulnerabilities to learn from*


Here are a few of the scenarios our teams tackled:


### 1. AI knowledge base overpopulation


What happens when an agent knows too much? This challenge demonstrated how an overly detailed or unfiltered[AI knowledge base](https://sendbird.com/blog/ai-knowledge-base) can become a goldmine for attackers. Participants learned to[craft AI prompts](https://sendbird.com/blog/prompt-engineering-guide) that tricked the AI into leaking sensitive company information it was never supposed to share


### 2. Function call response with excessive data exposure


An API response should be precise, not a data dump. In this challenge, our teams discovered how agent function calls could return far more data than necessary—including sensitive details like authentication tokens or internal system information. Attackers **** might use this extra data to map out and exploit a system.


### 3. Function call authentication flaws


This challenge focused on a classic security oversight: improper authentication in function calls. If a function that retrieves a user's order history doesn't properly verify who is asking, anyone could potentially access it.


Beyond this sample of challenges, Sendbird teams also grappled with prompt leaks, bypassing content restrictions, manipulating function call parameters, and exploiting incorrectly implemented function calls—a full gamut of[AI security](https://sendbird.com/blog/ai-security) risks.


Sendbird awarded Best in Communications APIs


[Read blog post](https://sendbird.com/blog/best-in-communications-api-award)


## Lessons in AI agent security from the field


More than a learning opportunity, HackTheBird was a strategic initiative to harden our products for an evolving threat landscape. For example, we had a stressful moment when a code patch unexpectedly broke during one of the challenges. It was a powerful reminder that AI agents operate in live, dynamic environments and reinforced a key lesson: for events like this, extensive testing followed by a short, high-impact competition window is the best approach to producing actionable insights.


Here’s how these lessons have translated to security protections for our AI agents:


## AI agent **** security guardrails


Sendbird provides robust defenses against the most **** common AI attacks right out of the box, and we’re more glad for this than ever. Our built-in[AI agent guardrails](https://sendbird.com/blog/how-to-safeguard-ai-agents) with API and webhook support are designed to mitigate the security risks inherent to large language models (LLMs), automatically detecting jailbreaking attempts, prompt injections, policy violations, and more.


*Sendbird AI agent dashboard is a centralized hub of AI agent security threats, hallucinations, and policy violations*


## Secure implementation guides


To further protect and empower our customers, the Sendbird security team is launching more documentation about the common security pitfalls and best practices for implementing agent security features, in support of our existing comprehensive Security guides. This helps ensure that enterprises avoid common errors and[build responsible AI agents](https://sendbird.com/ai-agent/builder) **** on a secure foundation.


## AI observability and monitoring


Our platform provides a suite of[observability tools](https://sendbird.com/blog/ai-agent-observability) to monitor and manage AI behavior in real-time, allowing you to flag potential attacks, suspicious messages, and AI hallucinations.


As we experienced first-hand, this combination of automated threat detection and response **** with human-in-the-loop review was critical **** to efficiency and effectively[refining AI prompts](https://sendbird.com/blog/prompt-engineering-guide) , strengthening knowledge bases, safeguarding user trust **,** and[improving agent performance without risk](https://sendbird.com/ai-agent/performance) .


*View detailed reasons for flagged security threats and more in the dashboard*


## Sendbird Trust OS


The features mentioned above are all part of a larger **** AI agent governance framework we call[Trust OS](https://sendbird.com/blog/introducing-trust-os-for-ai-agents) . It’s our comprehensive framework that outlines[how to build AI agents](https://sendbird.com/blog/how-to-build-an-ai-agent) that are not just powerful, but also controllable, secure, and reliable.


Trust OS provides the tools to manage everything from the AI's personality and[knowledge base](https://sendbird.com/blog/ai-knowledge-base) to robust security guardrails that protect against misuse and data exposure. It ensures that the AI agents you build are safe, trustworthy, and aligned with your business needs.


You can learn more about it in our[introductory blog post on Trust OS](https://sendbird.com/blog/introducing-trust-os-for-ai-agents) .


**How to choose an AI agent platform that works**


[Watch now](https://sendbird.com/resources/how-to-choose-an-ai-agent-that-works)


## Words from our AI agent security hackers


But don't just take my word for it. The energy during the competition was palpable, and the feedback we received highlighted how impactful the event was for everyone involved, regardless of their role. Here’s what some of the participants had to say:


-


"Kyah! That was really fun, haha. The best security gamification training lol. Thanks for the great event!"


-


"It was rather fun, and I think the competitive nature really drove people to spend a lot more time than they maybe would have."


-


"I hadn't targeted our agents hard until now, but when I tried it, I realized that we should improve some things…… and it helped me a lot."


## From competition to customer protection: Further fortifying our AI agents


Organizing a company-wide CTF event was an intensive yet rewarding experience. There's a unique joy in seeing colleagues passionately engaged in AI agent security, proving that learning and adopting security best practices can be fun.


HackTheBird was an invaluable investment in our security culture and our AI agent platform. By encouraging our own team to find and exploit vulnerabilities, we've sharpened our defenses and turned takeaways into better agentic[AI security features](https://sendbird.com/docs/security/documentation/overview) .


Our commitment to providing secure enterprise AI solutions is unwavering. Initiatives like HackTheBird are a testament to our proactive approach to[AI security](https://sendbird.com/blog/ai-security) , one that anticipates threats, fosters expertise, and delivers more resilient, trustworthy AI agents to our valued customers. We will continue to push the boundaries, adapt to emerging threats, and build the future of[secure enterprise-grade AI communications](https://sendbird.com/ai-agent/enterprise-grade) .


To learn more about Sendbird's enterprise-grade security posture, you can read about:


1. [Sendbird's security guides: Strengthening application security](https://sendbird.com/blog/security-guides)
2. [Sendbird’s risk-aware detection & response program](https://sendbird.com/blog/security-detection-response-program)
3. [6 ways Sendbird is embracing AI securely](https://sendbird.com/blog/using-ai-securely)
