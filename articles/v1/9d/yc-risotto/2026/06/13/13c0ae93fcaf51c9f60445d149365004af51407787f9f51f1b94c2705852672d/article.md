---
schema_version: "1.0.0"
document_id: "13c0ae93fcaf51c9f60445d149365004af51407787f9f51f1b94c2705852672d"
company_key: "yc-risotto"
company: "Risotto"
source_id: "yc-risotto-news-import-1fe94fa93287"
canonical_url: "https://www.tryrisotto.com/blog/jira-service-management-slack"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-22T12:02:46.365985+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:7a7c9d586910704aedeef807f48e6cec6ffe297f5bd3805080e309cc650aca26"
---

# Jira Service Management in Slack: Overview & Limitations

Teams connect Jira Service Management to Slack for a few different reasons, and the right setup depends on what you're trying to accomplish.


If your goal is **incident management and ChatOps for Slack** (e.g., receiving notifications for relevant alerts, taking actions on alerts or incidents, managing on-call work from Slack channels), then Atlassian’s guide on[connecting Jira Service Management with Slack](https://support.atlassian.com/jira-service-management-cloud/docs/integrate-with-slack/) covers this.


This article focuses on a different use case: **AI-powered ticket auto-resolution using Jira Service Management and Slack.** Employees submit requests through Slack, and an AI chatbot resolves them automatically before a human agent ever gets involved, syncing everything back to Jira Service Management.


Within JSM, you can try to do this by pairing[Atlassian Assist](https://www.tryrisotto.com/blog/atlassian-assist) (conversational ticketing in Slack or Microsoft Teams) and[Virtual Service Agents](https://www.tryrisotto.com/blog/jira-service-management-virtual-service-agent) (AI-driven automated ticket resolution). Atlassian also recently released[Rovo](https://www.atlassian.com/software/rovo) , which can be used for this purpose as well.


But there's a wide gap between configuring these tools and getting them to reliably auto-resolve tickets at a meaningful rate.


In this article, we’ll cover:


- **Atlassian’s native AI capabilities for automated ticket resolution,** and key limitations to be aware of.


- **Why enterprise IT teams augment Jira Service Management with Risotto,** our AI-native ITSM that auto-resolves 20-60%+ of tickets in Slack with seamless two-way JSM sync.


- **Hazel Health** replaced JIRA Assist and Atlassian Virtual Agent with Risotto and increased IT automation rates by 4X, from 3-5% to over 20%.[(Read full case study.)](https://www.tryrisotto.com/customers/hazel-health-increases-it-automation-by-4x-with-risotto)


- **Trust & Will** used Risotto as the orchestration layer on top of Jira Service Management and automated 35% of IT tickets.[(Read full case study.)](https://www.tryrisotto.com/customers/how-trust-will-built-it-automation-from-the-ground-up-with-risotto)


- **ThoughtSpot** integrated Risotto with Jira, Slack, and Okta in under a week, automating 48% of IT tickets and improving resolution time from 31 to 6.5 hours.[(Read full case study.)](https://www.tryrisotto.com/customers/from-50-manual-tickets-day-to-multi-department-automation-how-thoughtspot-transformed-it-support-with-ai)


- **Vidyard** integrated Risotto with Jira, Confluence, Google Drive, and Slack, automated 80% of tier-1 requests within 30 days, and achieved a 2-hour average AI resolution time.[(Read full case study.)](https://www.tryrisotto.com/customers/vidyard-automates-80-percent-tier-1-it-requests-with-risotto)


- **Gusto** integrated Risotto with Jira, Slack, Confluence, and Okta in 2 weeks, auto-solved 53% of IT tickets on the first day of implementation, and onboarded 11 departments with 157 access rules and 50+ runbooks. ([)(Read full case study.)](https://www.tryrisotto.com/customers/how-gusto-auto-resolves-55-of-tickets-with-risotto)


- **Jobber** integrated Risotto with Slack, Jira Service Management, AWS, Jamf, and Confluence in 2 days, automated 38% of IT tickets, achieved a 3-hour AI TTR, and saved employees 2,747 hours waiting for ticket resolution.[(Read full case study.](https://www.tryrisotto.com/customers/how-jobber-built-an-enterprise-wide-ai-support-funnel-with-risotto)


##
Using Atlassian’s Native AI Capabilities for Automated Ticket Resolution


If you're looking to automate ticket resolution using Atlassian's native AI capabilities, you have a couple of options.


- The first option is to use[Atlassian Assist](https://www.tryrisotto.com/blog/atlassian-assist) together with[Virtual Service Agents](https://www.tryrisotto.com/blog/jira-service-management-virtual-service-agent) . **‍**


- **Atlassian Assist** is the layer that connects Jira Service Management with your Slack workspace or Teams, ensuring seamless two-way sync between the platforms. Here’s the[Atlassian Assist Conversational Ticketing Guide](https://www.atlassian.com/software/jira/service-management/product-guide/tips-and-tricks/chat#overview) if you want to dig into setup and implementation.
‍
- **Virtual Service Agents** is the AI layer on top of that, which you can configure to resolve Jira tickets automatically without human intervention. For more information, here’s the[Jira Service Management Virtual Service Agent Guide](https://www.atlassian.com/software/jira/service-management/product-guide/tips-and-tricks/virtual-agent#overview) .


- The second option is to use[Rovo](https://www.atlassian.com/software/rovo) **,** Atlassian’s newest product, which excels at searching across Atlassian tools and offers some AI agent builder capabilities. Some teams have tried using it for automated ticket resolution, but that's not its core strength. It’s best for teams wanting company-wide search. For more information, see Atlassian’s guide on[What is Rovo](https://support.atlassian.com/rovo/docs/what-is-rovo/) or view this["Rovo in 5 minutes" video](https://www.youtube.com/watch?v=LbEFkTukBqc) .


##
Key Limitations to Consider & Why Choose Risotto to Augment Your Existing Jira Service Management Setup


Before diving into the differences, in a nutshell,[Risotto](https://www.tryrisotto.com/) (our Slack-native AI IT agent) lets you keep your existing Jira Service Management setup fully intact while adding a powerful AI ticket resolution layer on top:


**Risotto’s Slack integration is native by design.** Our co-founder Alex Confer spent years leading IT engineering at Dropbox and Gusto, and saw firsthand that employees already live in Slack. So we built Risotto to meet them there. Read our[origin story](https://www.tryrisotto.com/blog/our-story) to learn more.


Up next, we’ll break down how Risotto compares to Jira Service Management's native AI for automated ticket resolution, backed by customer quotes.


### Area #1: Automated Ticket Resolution Rates


**🚫 The bot tends to deflect employees to KB articles** rather than fully solving the issue, leaving the burden on the employee. When it does attempt to answer directly, the quality is inconsistent. From our conversations with customers who tried it, like Hazel Health ([see case study](https://www.tryrisotto.com/customers/hazel-health-increases-it-automation-by-4x-with-risotto) ), typical ticket resolution rates are around 3-5%.


**✅ Risotto’s north star metric is auto-solve rate,** meaning zero manual intervention from an IT agent. Most customers achieve 20%-60%+ auto-solve rates, which you’ll see through several[customer success stories](https://www.tryrisotto.com/customers) .


> *"We came to Risotto hoping to hold the line — but the results blew away our expectations. We doubled our resolution rate on day one, and it hasn't dropped since."*
>
>
> – Jose Izquierdo, Head of AIT Operations at Gusto


> *"Risotto's AI-powered support system had a massive impact straight out of the box and with very little setup. Employees now get fast, efficient answers with the same precision as one of our professionals."*
>
>
> – Mike Smith, IT Operations Manager at Jobber


> *“Our previous tool summarized whatever it pulled from wiki articles, it didn’t supplement responses with broader LLM knowledge or reasoning. This approach fell short because it lacked the capability to fully understand user intent or supplement answers with external knowledge… Risotto, however, synthesizes information from multiple inputs to provide context-aware, accurate, and actionable responses.”*
>
>
> – Phillip Rickett, VP of IT at Fundrise


> *"Things really clicked for us when we saw that Risotto actually reads and understands our knowledge instead of just guessing. Now, it's so ingrained in our daily work that our first instinct is to 'just ask Yardley."*
>
>
> – Vergil Smith, Head of IT and Security at Vidyard


> *"I'll see that six requests came in overnight and Risotto has resolved every general Tier-1 issue that popped up."*
>
>
> – Victoria DiRugeris, IT Operations Manager at Hazel Health


> *"Risotto was able to combine knowledge from different sources and give me an instant answer to a complex question, which was awesome to watch."*
>
>
> – Jason Huey, Senior IT Systems Administrator at ThoughtSpot


### Area #2: Setup & Time-to-Value ****


**🚫 Setup is complex and time-consuming,** requiring weeks to months, often with underwhelming ticket resolution rates to show for it.


**✅ Risotto setup takes hours to days.** Our platform was purpose-built from the ground up for automated ticket resolution, and the Jira integration is native and works seamlessly out of the box.


> *"The speed and simplicity of Risotto's setup was a great sign that we had made a good decision… We accomplished nearly the same configuration with Risotto in an hour that took us months with the other company."*
>
>
> – Phillip Rickett, VP of IT at Fundrise


> *"Ease of deployment was huge. We didn't need a consultant or months of configuration. Risotto just worked."*
>
>
> – Peter Hadjisavas, Head of IT at Hazel Health


> *"Risotto had the most thorough onboarding experience I've ever been a part of. Alex was great — he met with us weekly and made it very easy to quickly get up and running."*
>
>
> – Collin Clifford, Legal & Compliance Manager at Superhuman


> *"Working with the Risotto team was awesome from day one. They all have IT backgrounds and really cared about making us successful."*
>
>
> – Tom Grinberg, IT Manager at Trust & Will


> *"It's not just the tool that matters, it's the team behind it. Companies that work with Risotto are partnering with a responsive, collaborative group of people who are invested in your success."*
>
>
> – Vergil Smith, Head of IT and Security at Vidyard


### Area #3: Auto-Learning Capabilities


**🚫 Limited automatic learning from past answers.** You're stuck on a KB treadmill, constantly updating articles manually, which becomes its own major project.


✅ **Risotto automatically learns from past answers** in Slack and from past conversations in designated private channels. Our motto: you should never have to answer the same question twice.
‍


> *"The killer feature for us was that it could effortlessly learn and capture knowledge that our team creates every day in Slack… With Risotto, instead of constantly writing new documentation, our team can simply answer questions, and Risotto learns as we go."*
>
>
> – Phillip Rickett, VP of IT at Fundrise


> *"The more we use Risotto, the smarter it gets, that's what makes it different from every other tool we've tried."*
>
>
> – Peter Hadjisavas, Head of IT at Hazel Health


> *"One special thing about Risotto is it's constantly improving with every question. We see it getting better and better every day."*
>
>
> – Jason Huey, Senior IT Systems Administrator at ThoughtSpot


> *"Risotto began fielding tickets right away, and the setup across teams was incredibly low-lift. I could show colleagues how to configure their own workflows without any technical requirements."*
>
>
> – Victoria DiRugeris, IT Operations Manager at Hazel Health


### Area #4: IGA Automation in Slack


**🚫 Access management in Slack requires heavy custom configuration** rather than having workflows built in.


**✅ Risotto offers pre-built IGA automation capabilities embedded in the help desk,** including built-in approval workflows, automated provisioning, audit trails, and more. Every action is tracked and synced bi-directionally in real-time with Jira Service Management.


> *"When a team member asks, 'How do I get access to Hightouch?' they're not looking for a link or a document; they need immediate, actionable assistance. Risotto intelligently understands the intent behind the request. It confirms existing permissions, coordinates necessary approvals proactively, and automatically provisions access upon approval."*
>
>
> – Phillip Rickett, VP of IT at Fundrise


> *"Other IGA solutions felt clunky to set up and maintain. There was a lot of overhead to maintain them and they weren't embedded in the help desk, so you're solving one problem but creating another… we include Risotto in all our onboarding material now, and once people see how useful it is they keep coming back to use it more which is a really good sign.”*
>
>
> – Toby Stewart, IT Engineering at Ironclad


> *“The software access automations were a huge win for us. They were super easy to set up and we now have more than 30 applications with automated provisioning running 24/7.”*
>
>
> – Tom Grinberg, IT Manager at Trust & Will


> *“Automated software access saves us so much time. Within minutes people get the access they need with everything tracked, approved, and no additional overhead needed… For sensitive tools and resources Risotto’s automated time-based access has been a game-changer.”*
>
>
> – Collin Clifford, Legal & Compliance Manager at Superhuman


> *"Our control for SOC 2 is that access to privileged systems is approved by a manager... Risotto is able to automate the approval process and ticket tracking portion which is exactly what we needed."*
>
>
> – Vergil Smith, Head of IT and Security at Vidyard


### Area #5: Multi-Departmental Capabilities


**🚫 Multi-departmental ticket automation is inconsistent** and requires significant custom configuration.


**✅ Risotto offers multi-departmental capabilities out of the box.** Tickets route to the right person and department automatically, each team gets a separate knowledge base and siloed settings, and all departments are included under one license.


> *"Once you add in HR, our combined automation rate is even higher at 50.2%."*
>
>
> – Jason Huey, Senior IT Systems Administrator at ThoughtSpot


>
>
> – Victoria DiRugeris, IT Operations Manager at Hazel Health


> *"The multi-department capabilities are awesome. Our engineering and RevOps teams now also want to use Risotto."*
>
>
> – Collin Clifford, Legal & Compliance Manager at Superhuman


> *"Risotto started autosolving tickets immediately. Within the first few weeks, we saw departments that had never tracked ticket requests before, like our Cloud team, asking to get on the platform and streamline their support function."*
>
>
> – Erik Van Dijk, Senior IT Manager at Jobber


## Ready to Augment Your Jira Service Management Setup & Add a Powerful AI Ticket Resolution Layer in Slack?


Risotto auto-solves 20–60% of IT tickets, frees up IT agents for higher-value work, and improves key SLA metrics like time-to-first-response.


- [Book a demo call](https://www.tryrisotto.com/demo)
- [Explore customer success stories](https://www.tryrisotto.com/customers)
- [Read Risotto’s origin story](https://www.tryrisotto.com/blog/our-story)
