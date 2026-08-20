---
schema_version: "1.0.0"
document_id: "3540aed397d313ea56b98b35bb9c1ffea55ac1c6bad996d63b8a8f5d8dece292"
company_key: "yc-kashikoi"
company: "Kashikoi"
source_id: "yc-kashikoi-rss-651c469da509"
canonical_url: "https://blog.getkashikoi.com/2025/11/13/simulating-okta-logs/"
published_at: "2025-11-13T20:22:41+00:00"
first_seen_at: "2026-07-25T10:30:20.185118+00:00"
fetched_at: "2026-07-28T20:55:21.781978+00:00"
content_hash: "sha256:7950af8f53ef9ba09172f947dd5acb6134ac21ad8561e66254d93eb88e474d47"
---

# From Theory to Practice: How We Simulate Okta Logs with Domain Expertise

[We explored how simulation and verification are revolutionizing AI deployments, from Google’s data centers to nuclear fusion reactors](https://blog.getkashikoi.com/2025/11/12/beyond-the-bitter-lesson-why-ai-verification-matters/) . Now, we want to show you exactly how we’re applying this playbook to enterprise security.


Meet our first public demonstration:[the Okta Log Generator](https://oktalog.getkashikoi.com/) , a production-grade security event simulator that showcases how domain expertise transforms into scalable verification infrastructure.


## The Challenge: Testing Security in a Zero-Trust World


Enterprise security teams face an impossible task. They need to validate SOC detection rules, test security tools, and train AI analysts. But using production data is risky and regulated. Synthetic data is usually too simplistic to catch real threats. And hand-crafting realistic scenarios doesn’t scale.


Sound familiar? This is exactly the verification bottleneck we discussed in our[previous post](https://blog.getkashikoi.com/2025/11/12/beyond-the-bitter-lesson-why-ai-verification-matters/) , now manifesting in cybersecurity.


Traditional approaches fail because they try to hard-code security patterns. But real security events are complex, contextual, and interconnected. A user logging in from New York, triggering MFA, accessing resources, and logging out isn’t four random events. It’s a coherent session with shared context across device, location, and behavior.


## Our Solution: World Models Meet Security Expertise


Here’s where our playbook comes alive. At Kashikoi,[Tim](https://www.linkedin.com/in/timmichaud/) who has over a decade of security experience, didn’t try to write rules for every possible scenario. Instead, he used our proprietary technology to construct a world model of enterprise authentication.


Think of it as the security[equivalent of physics simulators](https://github.com/google-deepmind/torax) that enabled fusion control. Here instead of modeling plasma dynamics, we’re modeling the corporate user’s digital journey.


Our world model understands:


- Corporate users need business travel
- They authenticate across laptops and mobile devices
- They follow patterns: login → MFA → activity → logout
- Sessions have internal consistency but external variety


This isn’t random data generation. It’s structured simulation based on how authentication actually works in enterprise environments.


## What Makes Our Simulator Different


Our Okta Log Generator produces something unique: independent event sequences that mirror real user sessions.


**Each sequence contains:**


- ~4 related events forming a complete session
- Consistent context (actor, location, device, network)
- Production-grade fields with accurate IP geolocation
- Realistic user agents and valid Okta event structures
- Proper timestamps following natural activity patterns


**Here’s what 50 generated events might look like:**


- Sequence 1: Bob Johnson → New York office → Chrome/Mac → 4 events
- Sequence 2: Jane Doe → San Francisco VPN → Safari/iPhone → 4 events
- Sequence 3: Bob Johnson → Austin home → Firefox/Windows → 4 events
- … etc…


Notice how Bob appears in multiple sequences with different contexts. That’s real-world behavior. The same user has separate sessions from different locations and devices. Our simulator captures this complexity because expertise is embedded in the world model, not hard-coded in rules.


## From Simulation to Security Agent Testing


**Security teams are using our simulated environments to:**


- Validate SOC detection rules against realistic scenarios
- Test how security tools handle concurrent sessions and repeat users
- Create demo environments that feel production-real
- Generate training data for AI SOC analysts
- Build and verify security-oriented agentic workflows


[Remember the physical autonomous systems?](https://blog.getkashikoi.com/2025/11/12/beyond-the-bitter-lesson-why-ai-verification-matters/#domains)[They were trained via simulators first](https://blog.getkashikoi.com/2025/11/12/beyond-the-bitter-lesson-why-ai-verification-matters/#simulators) . Now we’re enabling the same approach for security AI. Test your agents against thousands of realistic scenarios before they touch production. Verify their decision-making across edge cases that would take years to encounter naturally.


## The Power of Domain Expertise at Scale


What makes this work isn’t just the technology. It’s how we’ve captured a decade of security experience in a scalable, reusable form. We didn’t write millions of rules. We defined the underlying model of how enterprise authentication behaves.


This is exactly what we saw with successful physical world deployments. Domain experts didn’t program every decision. They embedded their knowledge into simulators and reward functions. The AI learned the rest.


One expert’s knowledge, transformed into infinite realistic scenarios.


## Beyond Okta: The Platform Vision


The Okta Log Generator is just the beginning. We’re demonstrating that any domain where expertise exists can be transformed into simulation-based verification.


Imagine:


- Customer journey simulators for support agents
- Code simulators for coding assistants
- Data environment simulators for data analyst agents


Each powered by domain experts who understand not just the rules, but the underlying patterns of their field.


## Try It Yourself


Ready to see enterprise-grade simulation in action? Our Okta Log Generator is available now.


[Generate Okta Logs →](https://oktalog.getkashikoi.com/)


Start with our free tier to validate your detection rules. Need custom scenarios, bulk data, or malicious log patterns? Our pro and enterprise plans include consulting to build world models specific to your security environment.


## The Playbook in Practice


This is what moving beyond the Bitter Lesson looks like in practice. We didn’t throw more compute at security log generation. We didn’t train a massive model on billions of logs. We took one expert’s knowledge, encoded it into a world model, and created infinite verification capacity.


The same playbook that put AI in charge of data centers and fusion reactors is now available for your security operations. And security is just the beginning.


At Kashikoi, we’re not just building tools. We’re building the infrastructure that makes domain expertise scalable and AI verification possible.


Now let’s prove it in your domain!


---


*Want to build simulation-based verification for your enterprise? **Contact us at founders \[at\][getkashikoi.com](http://getkashikoi.com/)** to explore how world models can transform your AI deployment.*


*Already using our Okta Log Generator? We’d love to hear your feedback and use cases at **founders \[at\][getkashikoi.com](http://getkashikoi.com/) .***


### Like this:


Like


Loading…
