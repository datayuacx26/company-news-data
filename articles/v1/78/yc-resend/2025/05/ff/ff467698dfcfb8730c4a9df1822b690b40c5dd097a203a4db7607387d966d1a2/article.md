---
schema_version: "1.0.0"
document_id: "ff467698dfcfb8730c4a9df1822b690b40c5dd097a203a4db7607387d966d1a2"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/the-fixer"
published_at: "2025-05-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:83453baca16f25ab6f38d4581ad4666065f68a68da85203aba104a3646adc7d4"
---

# The Fixer

The quality of any product is in the details.


And yet we noticed a pattern. Our support team would bring up a small bug and it would get lost in the flood of new features.


Even when Support contacted Engineering, no one would own the request, and so it would often go untracked.


Prioritizing fixing small bugs, improving UX, or polishing details can often get lost in the desire to ship new features.


## The Problems


We noticed at least two problems occurring regularly.


Either genuine needs would go unaddressed because we were busy developing new features or our engineers lost the ability to tackle more focus-intensive projects due to requests to address bugs.


Each problem has its own set of short-term and long-term consequences.


## The Principles


As with many problems, it's important to work from your principles to the proper solution.


We identified three key principles:


1. **Quality over features** : Our commitment to quality is our top priority and is a key differentiator.
2. **Collaboration and communication** : Effective communication and collaboration within our team and across departments are crucial for resolving issues efficiently.
3. **Customer-centric approach** : Our primary focus is on delivering a delightful experience to customers.


## The Solution


While we encourage all our engineers to stay in touch with the needs of Resend users, our Support team lives the day-to-day with our customers.


We found the solution to small bug fixes and improvements by leveraging the relationship between Support and Engineering. Internally strengthening that team relationship led to an improved customer experience.


We developed a new rotating role called " **The Fixer** ." The Fixer plays a critical role within Resend, ensuring that our products maintain high quality and reliability.


## The Implementation


Each week, we assign an engineering team member the role of "Fixer". The rotation ensures every engineer empathizes with support and can effectively contribute to resolving issues.


We use[Incident.io](https://incident.io/) to create our rotation schedule and automatically update` @the-fixer` tag in Slack with the assigned engineer.


We track Fixer tasks using a few tools:


- **Slack** : all bugs and improvements are posted on our` #all-triage` channel and tagging` @the-fixer` contacts the assigned engineer
- **Linear** : we track each ticket on our Customer Engineering board and use` the-fixer` label to auto-apply a 14-day SLA to ensure we fix the bug within two weeks.


While the Fixer is ultimately responsible for addressing the bugs raised, they'll often need more context on an issue or help finding a fix, so others are brought in as needed.


Care and attention to user needs is a core value at Resend. We even celebrated the role by creating a custom enamel pin for the Fixer to give to our engineers.


## The Benefits


We immediately noticed several benefits both internally and externally.


1. **Cross-team collaboration:** this structure has improved the collaboration between Success (or whoever is reporting the bug) and Engineering
2. **Bug ownership** : assigning an owner for reported bugs provides clarity for each issue
3. **Deep focus** : while other engineers do help address bugs regularly, not being the Fixer allows for deeper focus on more complex problems and roadmap features
4. **Timely response** : the system lets us track report-to-fix and enables us to circle back around to customers with confidence
5. **Shared responsibility** : it's easy to build features in a vacuum, but the Fixer's close connection to customers ensures engineering stays in contact with real customers and that each engineer retains context on the whole product


We're excited to continue to improve our processes and better serve our customers.
