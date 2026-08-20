---
schema_version: "1.0.0"
document_id: "930f4c7ee8deaf61deb518df712d6cb002b4e459d041bf4054eff68dadb04a4a"
company_key: "yc-risotto"
company: "Risotto"
source_id: "yc-risotto-news-import-1fe94fa93287"
canonical_url: "https://www.tryrisotto.com/blog/jira-conversational-ticketing"
published_at: "2026-03-02T00:00:00+00:00"
first_seen_at: "2026-07-22T12:02:46.365985+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:b7d08fb0b89bfa460b16b7e88f820a579354868d86e276d18b4a7d88d19d4b7f"
---

# Jira Conversational Ticketing: In-Depth Look & Case Studies

The benefits of having an AI agent handle requests directly in Slack or Teams and automatically sync everything back to Jira are obvious: centralized help desk, less duplicated work, fewer missed tickets, and a better experience for both employees who get answers faster and IT teams who have more time for higher-value work.


**The harder question is how to actually implement Jira conversational ticketing without months of configuration,** a brittle setup that breaks every time something changes, and automation rates that don't justify the investment.


In this article, we'll cover:


- **The downsides of implementing conversational ticketing with JSM's built-in AI capabilities,** and why complex configuration, slow deployment, and inconsistent AI responses leave many teams frustrated.


- **The benefits of implementing Jira conversational ticketing through Risotto.** Risotto is our Slack-native AI IT agent that’s purpose-built to integrate with Jira Service Management. It deploys in hours (not weeks or months), auto-resolves 20–60%+ of tier-1 tickets out of the box, and onboarding is guided by IT practitioners who've been in your shoes — including co-founder Alex Confer, who previously led IT engineering at Dropbox and Gusto.[Explore our origin story.](https://www.tryrisotto.com/blog/our-story)


- **Case studies & results from companies who integrated Risotto with Jira Service Management:** These span companies of all sizes, from large enterprises like Gusto handling ~3,000 requests/month to midsize teams like ThoughtSpot managing ~1,500 requests/month, and smaller teams as well.


- **How Hazel Health switched from Jira Assist & Atlassian Virtual Agent to Risotto** and increased automation rates from 3–5% to 20%+. *(*[Read full case study](https://www.tryrisotto.com/customers/how-hazel-health-increased-it-automation-by-4x-and-lifted-employee-experience-with-risotto) *)* .


- **How Trust & Will used Risotto as the AI orchestration layer on top of Jira Service Management** and automated 35% of IT tickets. *(*[Read full case study](https://www.tryrisotto.com/customers/how-trust-will-built-it-automation-from-the-ground-up-with-risotto) *).*


- **How ThoughtSpot integrated Risotto with Jira, Slack, and Okta in under a week,** automating 48% of IT tickets and cutting resolution time from 31 to 6.5 hours. *(*[Read full case study](https://www.tryrisotto.com/customers/from-50-manual-tickets-day-to-multi-department-automation-how-thoughtspot-transformed-it-support-with-ai) *).*


- **How Gusto integrated Risotto with Jira, Slack, Confluence, and Okta in 2 weeks,** auto-solved 53% of IT tickets on day one, and saved 114,000 hours of support wait time. *(*[Read full case study](https://www.tryrisotto.com/customers/how-gusto-auto-resolves-55-of-tickets-with-risotto) *)* .


- **How Jobber integrated Risotto with Slack, Jira Service Management, AWS, Jamf, and Confluence in two days,** achieving 38% IT ticket automation and saving 2,747 hours of support wait time. *(*[Read full case study](https://www.tryrisotto.com/customers/how-jobber-built-an-enterprise-wide-ai-support-funnel-with-risotto) *)* .


## **The Downsides of Jira's Built-in AI Capabilities for Conversational Ticketing (& How Risotto Solves Them)**


Before getting into the nitty-gritty details, it’s worth understanding exactly what Risotto is and how it works alongside Jira.


**Risotto is a purpose-built AI IT agent that lives in Slack and integrates seamlessly with Jira Service Management.** You don’t rip and replace anything — Jira stays exactly where it is. You just add Risotto on top, and it becomes the AI layer that handles employee requests conversationally in Slack — auto-resolving tier-1 tickets, managing access provisioning, and escalating to the right IT agent when needed. Every Slack interaction is bi-directionally synced with Jira behind the scenes automatically.


[Risotto was built by IT for IT.](https://www.tryrisotto.com/blog/our-story) Our co-founder Alex Confer previously led IT engineering at Gusto and Dropbox, and after personally running into the limitations of existing AI automation tools — months of configuration, inconsistent answer quality, and the constant maintenance burden — we built the tool we wished existed.


That context shapes everything about how Risotto is designed: pre-built Slack and Jira integrations that work out of the box, an AI agent focused on actually *solving* tickets (not just deflecting to KB articles), and deployment that happens in hours rather than weeks or months.


With that context in mind, here’s how Risotto addresses core issues IT teams run into when trying to get real conversational ticketing value out of JSM’s built-in AI capabilities:


### **1. Setup & Ongoing Maintenance**


**🚫 With JSM:** Configuring AI workflows in Jira Service Management from scratch is a significant undertaking. Custom workflows need to be built, integrations configured, and rules mapped out — a process that commonly spans weeks or months. And that's just getting started. Ongoing maintenance is a heavy ongoing burden, with a real risk of things breaking when your team makes changes. Many organizations end up hiring consultants just to manage the system that was supposed to save them time and money.


**❇️ With Risotto:** Risotto integrates seamlessly with Jira Service Management and Slack from day one. Most teams are up and running in hours — not weeks or months. Our integrations come pre-built, with workflows that work out of the box immediately.


That’s not by accident. As we shared earlier, our co-founder Alex Confer previously led IT engineering at Gusto and Dropbox. He built Risotto to solve the problems he experienced firsthand — including the pain of deploying and maintaining clunky, legacy enterprise IT automation tools. That context shapes everything about how Risotto is designed and deployed.


> *"Getting Risotto integrated with Jira, Slack, and Okta was so seamless and fast. Risotto is one of the easiest tools I've implemented. We were up and running in less than a week and already seeing our first autosolves."*
>
>
> – Jason Huey, Senior IT Systems Administrator at ThoughtSpot


> *"We’ve completely replaced JIRA Assist with Risotto, it was a seamless transition… Ease of deployment was huge. We didn't need a consultant or months of configuration. Risotto just worked."*
>
>
> – Peter Hadjisavas, Head of IT at Hazel Health


> *“The speed and simplicity of Risotto’s setup was a great sign that we had made a good decision… We accomplished nearly the same configuration with Risotto in an hour that took us months with the other company.”*
>
>
> – Phillip Rickett, VP of IT at Fundrise


> *“Risotto’s AI-powered support system had a massive impact straight out of the box and with very little setup. Employees now get fast, efficient answers with the same precision as one of our professionals.”*
>
>
> – Mike Smith, IT Operations Manager at Jobber


> *“Risotto had the most thorough onboarding experience I’ve ever been a part of. Alex was great — he met with us weekly and made it very easy to quickly get up and running.”*
>
>
> – Collin Clifford, Legal & Compliance Manager at Superhuman


> *“Working with the Risotto team was awesome from day one. They all have IT backgrounds and really cared about making us successful.”*
>
>
> – Tom Grinberg, IT Manager at Trust & Will


> *“We include Risotto in all our onboarding material now, and once people see how useful it is they keep coming back to use it more which is a really good sign.”*
>
>
> – Toby Stewart, IT Engineering Manager at Ironclad


### **2. Answer Quality & Auto-Solve Rates**


**🚫 With JSM:** JSM's AI agent tends to deflect employees to knowledge base articles rather than actually solving their problems. When it does provide an answer, the quality is often inconsistent — your IT team still ends up reviewing and resolving a large portion of tickets manually.


**❇️ With Risotto:** Our north star metric is **auto-solve rate** — meaning zero manual IT involvement. That's the entire focus of the product. Risotto's AI IT agent doesn't just deflect to KB articles; it resolves issues conversationally in Slack, the way an actual IT agent would. It asks clarifying questions, troubleshoots step by step, and executes actions like granting software access, resetting passwords, and running MDM workflows.


**Most teams achieve 20–60%+ tier-1 ticket automation rates:**


- [Fundrise:](https://tryrisotto.com/blog/from-frustration-to-automation-how-fundrise-transformed-it-support-with-risotto) Automated nearly 60% of IT support tickets
- [Gusto:](https://www.tryrisotto.com/customers/how-gusto-auto-resolves-55-of-tickets-with-risotto) Automated nearly 55% of IT support tickets
- [Superhuman:](https://tryrisotto.com/blog/from-manual-to-magical-how-superhuman-transformed-it-support-with-risotto) Automated nearly 20% of IT support tickets
- [Ironclad:](https://www.tryrisotto.com/blog/from-100-weekly-manual-tickets-to-90-automation-how-ironclad-transformed-it-support-with-ai) Automated nearly 90% of access-related IT requests
- [ThoughtSpot:](https://www.tryrisotto.com/customers/from-50-manual-tickets-day-to-multi-department-automation-how-thoughtspot-transformed-it-support-with-ai) Automated nearly 48% of IT support tickets
- [Trust & Will:](https://tryrisotto.com/blog/how-trust-will-built-it-automation-from-the-ground-up-with-risotto) Automated nearly 35% of IT support tickets
- [Jobber:](https://www.tryrisotto.com/customers/how-jobber-built-an-enterprise-wide-ai-support-funnel-with-risotto) Automated nearly 38% of IT support tickets
- [Hazel Health:](https://tryrisotto.com/blog/how-hazel-health-increased-it-automation-by-4x-and-lifted-employee-experience-with-risotto) Improved deflection rates from 3–5% to over 20%
- [Shakepay:](https://tryrisotto.com/blog/shakepay-customer-story) Automated nearly 40% of IT support tickets
- [Retool:](https://tryrisotto.com/blog/retool-sla-customer-story) Reduced SLA resolution time from an average of 2 days to under 1 day
- [Vidyard:](https://tryrisotto.com/blog/vidyard-customer-story) Automated nearly 56% of IT support tickets


### **3. Auto-Learning Capabilities from Resolved Tickets**


**🚫 With JSM:** JSM's AI struggles to learn from resolved tickets, which means your IT team has to create and maintain knowledge base articles manually to compensate. This defeats much of the purpose: if your agents are constantly writing documentation to make the AI smarter, you haven't actually freed up their time.


**❇️ With Risotto:** When Risotto can't resolve an issue, it intelligently escalates the ticket in Slack to the appropriate IT agent — with full context intact. When the agent resolves it, Risotto learns from that interaction and applies that knowledge the next time a similar question comes up. The goal is for you to never answer the same question twice.


> *“The killer feature for us was that it could effortlessly learn and capture knowledge that our team creates every day in Slack… with Risotto, instead of constantly writing new documentation, our team can simply answer questions, and Risotto learns as we go.”*
>
>
> – Phillip Rickett, VP of IT at Fundrise


> *"The more we use Risotto, the smarter it gets — that's what makes it different from every other tool we've tried."*
>
>
> – Peter Hadjisavas, Head of IT at Hazel Health


> *"Risotto started answering complex product questions even I didn't know off the top of my head. It was pulling insights from our own past Slack conversations, surfacing knowledge that would have otherwise been buried."*
>
>
> – Peter Hadjisavas, Head of IT at Hazel Health


### **4. Software Access Automation**


**🚫 With JSM:** Automating software access provisioning in JSM within Slack requires building complicated AI workflows from scratch — a process that can take weeks or months, and demands ongoing maintenance to keep up with org changes, new applications, and shifting access policies. It's one of the most painful parts of managing IT at scale.


**❇️ With Risotto:** IGA automation in Slack works from day one. Employees request access in natural language — "Can I get access to Figma?" — and Risotto handles everything: gathering business justification, routing to the appropriate approver(s), and automatically provisioning or deprovisioning access for Just-In-Time (JIT) access requests.


Risotto bi-directionally syncs with Jira Service Management in real time, and manages the entire ticket lifecycle: automatically generating a title, categorizing the issue, applying relevant tags, routing, triage, and more.


Risotto also documents every exchange, ensuring a complete audit trail.


> *"Risotto became the orchestration layer for Jira Service Management and gives us instant automation with AI. We always wanted to require a reason for application access, but it was really difficult to integrate that into JSM. Risotto adds that functionality to JSM and so much more."*
>
>
> – Tom Grinberg, IT Manager at Trust & Will


> *"When a team member asks, 'How do I get access to Hightouch?' they're not looking for a link or a document; they need immediate, actionable assistance. Risotto intelligently understands the intent behind the request. It confirms existing permissions, coordinates necessary approvals proactively, and automatically provisions access upon approval."*
>
>
> – Phillip Rickett, VP of IT at Fundrise


> *“The software access automations were a huge win for us. They were super easy to set up and we now have more than 30 applications with automated provisioning running 24/7.”*
>
>
> – Tom Grinberg, IT Manager at Trust & Will


> *“Automated software access saves us so much time. Within minutes people get the access they need with everything tracked, approved, and no additional overhead needed… For sensitive tools and resources Risotto’s automated time-based access has been a game-changer.”*
>
>
> – Collin Clifford, Legal & Compliance Manager at Superhuman


> *“Sending a compliance report from Risotto with all of the tickets tagged correctly — that’s so much easier, it’s definitely made audits a far less stressful experience.”*
>
>
> – Vergil Smith, IT Manager at Vidyard


### **5. Multi-Departmental Support**


**🚫 With JSM:** Extending conversational ticketing to non-IT departments is possible, but it requires significant additional configuration. JSM's complexity and cost can be overkill for teams like HR, Legal, Sales, and others who just need a simple way to automate repetitive requests in Slack.


**❇️ With Risotto:** Multi-department conversational ticketing is built in from the start. Once IT is up and running, other departments consistently want in — because they’re also overwhelmed with repetitive tickets and want to free up time for higher value work.


**Key benefits for multi-department adoption:**


- **All departments included at no extra cost:** No per-team pricing fees
- **Built-in ticketing system:** Simpler and more intuitive than Jira for non-technical teams like HR, Sales, Legal, and others
- **Intelligent ticket routing:** Risotto automatically routes requests to the right team and person based on their previous ticket history
- **Department-level customization:** Separate knowledge bases, permissions, and workflows per team


> *"The multi-department capabilities are awesome. Our engineering and RevOps teams now also want to use Risotto as they also get lots of the same questions over and over again."*
>
>
> – Collin Clifford, Legal & Compliance Manager at Superhuman


> *"Once you add in HR, our combined automation rate is even higher at 50.2%."*
>
>
> – Jason Huey, Senior IT Systems Administrator at ThoughtSpot


> *"Risotto started autosolving tickets immediately. Within the first few weeks, we saw departments that had never tracked ticket requests before — like our Cloud team — asking to get on the platform and streamline their support function."*
>
>
> – Erik Van Dijk, Senior IT Manager at Jobber


**Note:** For more context on how Risotto compares to Jira Service Management for conversational ticketing in Slack, see our article on the[Best Jira Service Management Alternatives](https://www.tryrisotto.com/blog/jira-service-management-alternatives) .


## **Case Studies: Risotto + Jira Service Management + Slack**


The[customer success stories](https://www.tryrisotto.com/customers) below show what happened when IT teams added Risotto on top of their existing Jira setup — how fast they got up and running, what their ticket automation rates looked like, and how it changed day-to-day operations.


They range from large enterprises like Gusto (~3,000 requests/month) to midsize teams like ThoughtSpot (~1,500 requests/month), and smaller teams as well.


### **How Hazel Health Increased IT Automation by 4x After Switching from Jira Assist & Atlassian Virtual Agent to Risotto**


‍ **The challenge:**


Hazel Health is a telehealth company supporting over 5 million students. Their IT team was managing high volumes of tickets across multiple departments, products, and time zones. They needed to maintain quality support without expanding headcount.


**Before Risotto — using Jira Assist + Atlassian Virtual Agent:**


Hazel Health was already using Jira as their ticketing backbone, with Jira Assist and Atlassian Virtual Agent layered on top for AI automation. Despite investing significant time tweaking keyword triggers and maintaining workflows, their deflection rates remained low at just 3–5%.


**What changed with Risotto:**


Risotto became the AI ticket resolution layer — while Jira Service Management stayed in place as the ticketing backend. The transition was seamless, and the impact was immediate. As Peter Hadjisavas, Head of IT at Hazel Health, noted:


- *"We started seeing deflection rates jump from 3–5% to over 20%, which is huge."*
- *"Ease of deployment was huge. We didn't need a consultant or months of configuration. Risotto just worked."*
- *"The more we use Risotto, the smarter it gets — that's what makes it different from every other tool we've tried."*
- *"Risotto is providing immediate answers 24/7, even when IT isn't online. That's a game changer."*


For Hazel Health's healthcare providers — who often work outside standard business hours — the 24/7 availability in Slack proved especially valuable. Risotto is also HIPAA compliant, which was a critical requirement for their team.


### **How Trust & Will Integrated Risotto with Jira Service Management (& Automated 35% of IT Tickets)**


**The challenge:**


When Tom Grinberg joined Trust & Will as IT Manager, he needed to build their IT operations from scratch. Over 50% of incoming requests were access-related, and the volume threatened to make the support model unsustainable.


**Before Risotto — the JSM configuration problem:**


Tom initially implemented Jira Service Management as a foundational ticketing system. But configuring it for meaningful automation proved cumbersome and time-consuming.


Rather than enabling efficiency, JSM's complexity created additional overhead that slowed down the very processes it was meant to streamline.


**What changed with Risotto:**


Trust & Will layered Risotto on top of their existing Jira Service Management setup. Risotto became the conversational AI layer — handling employee interactions in Slack, automating routine tasks, and escalating complex issues to the right IT agent — while every interaction was synced bi-directionally with Jira behind the scenes. As Tom Grinberg shared:


- *"Risotto became the orchestration layer for Jira Service Management and gives us instant automation with AI.”*
- *“We always wanted to require a reason for application access, but it was really difficult to integrate that into JSM. Risotto adds that functionality to JSM and so much more."*
- *“Risotto has been super popular internally, it’s a much improved experience for employees to get answers and problems solved immediately”.*
- *“Working with the Risotto team was awesome from day one. They all have IT backgrounds and really cared about making us successful”.*


**Key results:**


- Risotto automated 35% of IT tickets
- Over 30 applications with automated provisioning running 24/7
- Compliance requirements that previously required manual tracking are now seamlessly integrated into automated workflows
‍


### **How ThoughtSpot Integrated Risotto with Jira, Slack & Okta in Under a Week (& Automated 48% of IT Tickets)**


**The challenge:**


ThoughtSpot's lean IT team was supporting 1,000+ employees with a ticket volume of around 1,500 requests per month. Employees were frustrated.


As Jason Huey, Senior IT Systems Administrator at ThoughtSpot, shared:


> *“The biggest gripe that our users had was that they would put in a ticket and it would just be lost into the ether for a couple days, and they would have to actually be the ones to bump it or get our attention on it* ”.


**Before Risotto — evaluating other options:**


ThoughtSpot evaluated several other IT automation platforms. Each required significant configuration overhead and delivered limited immediate value. For a small team already overloaded, those tools would have created more work than they saved.


**What changed with Risotto:**


Risotto integrated with ThoughtSpot's existing Jira setup, Slack, and Okta in under a week — and saw their first autosolves almost immediately. Risotto connected directly to Confluence documentation and past Slack conversation history, creating an intelligent support system capable of handling complex, multi-faceted questions with minimal IT involvement.


As Jason shared:


- *“Getting Risotto integrated with Jira, Slack, and Okta was so seamless and fast.”*
- *“Risotto is one of the easiest tools I’ve implemented. We were up and running in less than a week and already seeing our first autosolves.”*
- *“Risotto takes away the telephone tag we had to play so now employees get what they need faster and more efficiently.”*
- *“Before Risotto compliance reports could take days to figure out but now I can pull everything needed in seconds.”*
- *“One special thing about Risotto is it’s constantly improving with every question. We see it getting better and better every day.”*


**Key results for IT:**


- 48% of IT tickets automated
- Resolution time dropped from 31 hours to 6.5 hours
- Multi-step access workflows, which comprised the majority of ThoughtSpot's tickets, now complete within 24 hours without manual follow-ups


**Multi-department expansion:**


After expanding to HR, ThoughtSpot’s combined automation rate with IT reached 50.2%.In addition, ThoughtSpot's accounting team achieved an outstanding 80% auto-solve rate for transactional queries with Risotto.


### **How Gusto Integrated Risotto with Jira, Slack, Confluence, & Okta in 2 Weeks (& Auto-Solved 53% of IT Tickets on Day One)**


**The challenge:**


Gusto's IT team was handling roughly 3,000 support requests per month. They lost hours to troubleshooting and access provisioning, leaving little time for the strategic AI initiatives that would define Gusto's direction. Their existing chatbot couldn't keep up — it lacked the continuous learning, integration depth, and flexibility they needed to scale automation across a growing enterprise company.


**The Risotto implementation:**


Risotto worked side by side with Gusto's team to integrate with their full tech stack — Jira, Slack, Confluence, and Okta. The platform was fully deployed in two weeks with zero downtime. Within hours of going live, the impact was clear. As Jose Izquierdo, Head of AI Operations at Gusto, shared:


- *"We came to Risotto hoping to hold the line — but the results blew away our expectations. We doubled our resolution rate on day one, and it hasn't dropped since."*
- *"Employees can get help directly within Slack. They can kick off workflows for getting access to any internal tool, ask any HR-related question, or resolve questions about GitHub repository access — all without leaving Slack." ‍*


**Key results & benefits:**


- 53% resolution rate on day one of implementation
- 55% of tickets auto-resolved on average
- 114,000 hours of support wait time saved
- 5-hour AI time-to-resolution (vs. 35-hour human TTR)
- 11 departments onboarded, with 157 access rules and 50+ runbooks
- Risotto documents every exchange for complete audit trails, ensuring full SOC 2 compliance


### **How Jobber Integrated Risotto with Jira Service Management, Slack, AWS, Jamf, & Confluence in Two Days (Achieving 38% IT Automation)**


**The challenge:**


Jobber was scaling fast, adding significant headcount, and their five-person IT team was struggling to keep up. Employees were waiting an average of 35 hours for ticket resolution.


**As Mike Smith, IT Operations Manager at Jobber, shared:**


> *“Historically, ticket work overshadowed strategic project work. We were spending our days trying to keep the wheels on."*


**Before Risotto:**


Jobber's IT team had previously piloted another platform designed to streamline ticketing directly within Jira Service Management. However, the tool wasn’t a fit, and the complex setup effort outweighed the gains.


**What changed with Risotto:**


After a collaborative onboarding process that involved Jobber's stakeholders directly (earning strong buy-in from senior leadership before full rollout), Risotto integrated with Slack, Jira Service Management, and core apps like AWS, Jamf, and Confluence within two days. Jobber's fragmented help desk became a single, centralized support funnel overnight.


**As Mike Smith, IT Operations Manager at Jobber, shared:**


> *"Risotto's AI-powered support system had a massive impact straight out of the box and with very little setup. Employees now get fast, efficient answers with the same precision as one of our professionals."*


**As Erik Van Dijk, Senior IT Manager at Jobber, shared:**


> *"Risotto's support is fully embedded in the way we operate — from day-one employee onboarding to providing technical support for seasoned team members."*


**Key results & benefits:**


- 38% IT ticket automation rate
- 41% Revenue Technology ticket automation rate
- 3-hour AI TTR (down from 35-hour human TTR)
- 2,747 hours saved waiting for ticket resolution
- 30 apps and 12 departments integrated with Risotto


## **Interested in Implementing Jira Conversational Ticketing Through Risotto?**


Risotto is purpose-built to do one thing exceptionally well: auto-resolve tier-1 IT tickets in Slack or MS Teams. We integrate with your existing Jira Service Management setup — you don't replace anything, and most teams get set up in hours, not weeks or months.


Most of our customers achieve 20–60%+ tier-1 ticket automation rates. Whether you're a midsize team or an enterprise company with thousands of employees, our ITSM platform scales with you.


In short, we help internal support teams maximize the benefits of[conversational ticketing](https://www.tryrisotto.com/blog/conversational-ticketing) , free up time for higher-value work, and improve customer satisfaction.


We invite you to:


- [Schedule a demo](https://www.tryrisotto.com/demo)
- [Read our origin story](https://www.tryrisotto.com/blog/our-story)
- [Explore customer success stories](https://www.tryrisotto.com/customers)
