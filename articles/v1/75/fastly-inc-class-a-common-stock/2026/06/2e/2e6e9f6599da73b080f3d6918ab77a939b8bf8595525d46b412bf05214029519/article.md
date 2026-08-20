---
schema_version: "1.0.0"
document_id: "2e6e9f6599da73b080f3d6918ab77a939b8bf8595525d46b412bf05214029519"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/bot-defense-table-stakes-machine-traffic-requires-business-strategy/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:4229eb80fa1399ce275a23c217f40a43c473e1e9a9c22342b65e6e6a91692bbc"
---

# Bot Defense is Table Stakes. Machine Traffic Requires a Business Strategy

As my colleague Hossein Lotfi[recently shared in his deep dive](http://fastly.com/blog/ai-traffic-grew-6-5x-faster-than-human-traffic-this-year) into our latest network data, **AI requests on our platform have grown roughly 6.5x faster than human traffic.** With autonomous machine-to-machine traffic now approaching half of all internet requests, businesses face an entirely new landscape.


As AI traffic grows in importance, businesses need to decide **which AI interactions create value, which create risk, and how to act on both** . Stopping bad bots still matters but is no longer enough. Without a clear strategy, managing this machine traffic is a fundamental business challenge. Most companies can’t reliably tell the difference between what traffic should be stopped and what traffic adds value.


## **Blocking Is Only Half The Strategy**


Most companies are not ready for this. They can see automated traffic increasing. They can feel the pressure on applications, APIs, and infrastructure. They know AI systems are accessing their content, but they often do not know which ones, why they are there, what they are doing, or whether that activity creates value.


Is blocking every bot the answer? It’s tempting when faced with a flood of unknown requests. Some automated traffic should absolutely be stopped: malicious bots, credential stuffing attacks, aggressive scraping, inventory hoarding, policy-violating crawlers, and abusive automation create real risk.


But **blocking by default is not the same as having a complete strategy.** Treating every automated request as the same kind of threat collapses nuance into a single answer. It may reduce some risk, but it can also block future customers, reduce distribution, degrade user experiences, and prevent you from understanding how AI systems interact with your digital properties.


### **Two Companies, Two Different Responses to AI Traffic**


The debate around AI traffic is often framed as allow versus block. The decisions companies make can have a big impact on business.


Take the two large companies represented in the charts below as real-world examples: One saw a spike in fetcher traffic and instituted a policy to start blocking them, most likely to maintain content authority. The second has intentionally not blocked them and as a result saw increased fetcher volume over multiple months.


The companies that simply block AI traffic may feel safer in the short term. But they may also make themselves invisible in this new version of the internet. So what should you do?


### **The Edge Is Where Decisions Happen**


Human and machine traffic now share the same infrastructure, but they behave completely differently. While the edge was already the real-time decision layer for top-tier digital companies, this shift accelerates that reality for everyone else. Because it sits in the path of every request, **the edge is where you must inspect traffic, enforce policy, protect applications, accelerate delivery, manage origin access, and act** before requests create cost, risk, or latency.


To make this work, you need clarity. You have to see which AI systems are accessing your digital properties, understand their behavior patterns, and separate useful automation from unwanted automation. With a solid strategy, you don’t have to let every machine in or keep every machine out. The goal is to respond to each request based on its exact intent and impact.


## **Building a Machine Traffic Strategy**


Every business needs a strategy based on its goals and priorities. What works for a travel platform might not work for a publisher. The companies adapting most successfully focus on three key areas: **visibility, context, and precision.**


**Gain visibility** into exactly which machines are interacting with your sites, applications, APIs, and content.


**Build context** about what those systems are accessing, how often they return, whether they respect policies, and whether they create business value.


**Apply precision** in how you respond. Depending on the machine’s intent, you might choose to accelerate it, challenge it, redirect it elsewhere, or even rewrite the response on the fly to give that specific bot tailored data.


We help our customers execute that machine traffic strategy at the edge. In the path of every request, Fastly delivers the visibility, context, and precision required to balance[performance](https://www.fastly.com/products/cdn) ,[security](https://www.fastly.com/products/web-application-api-protection) ,[bot management](https://www.fastly.com/products/bot-management) , and origin access with[real-time](https://www.fastly.com/products/logging) intelligence. We help organizations distinguish beneficial automation from unwanted activity.


The internet is no longer just for humans. Some machine traffic creates risk and some creates value. The **future belongs to those who can see the difference and act on it.**


#### **Forward-looking statements**


These articles contain “forward-looking” statements that are based on Fastly’s beliefs and assumptions and on information currently available to Fastly on the date of these articles. Forward-looking statements may involve known and unknown risks, uncertainties, and other factors that may cause its actual results, performance, or achievements to be materially different from those expressed or implied by the forward-looking statements. These statements include, but are not limited to, those regarding expectations regarding the future growth, velocity, and composition of AI and automated traffic; the adoption rates and scale of agentic workloads and AI assistants; the behavioral patterns of AI crawlers and fetchers; the impacts of automated requests on web infrastructure; and the performance, capabilities, and expectations regarding customer experiences with Fastly’s products and services, including Fastly Bot Management. Except as required by law, Fastly assumes no obligation to update these forward-looking statements publicly, or to update the reasons actual results could differ materially from those anticipated in the forward-looking statements, even if new information becomes available in the future. Important factors that could cause our actual results to differ materially are detailed from time to time in the reports Fastly files with the Securities and Exchange Commission (“SEC”), including in our Annual Report on Form 10-K for the fiscal year ended December 31, 2025 and our Quarterly Reports on Form 10-Q. Copies of reports filed with the SEC are posted on Fastly’s website and are available from Fastly without charge.
