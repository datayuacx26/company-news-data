---
schema_version: "1.0.0"
document_id: "3c95af64620e24392bf84a4893106314a3af8db8f0716105caa9087e2de56f56"
company_key: "yc-kombo"
company: "Kombo"
source_id: "yc-kombo-news-import-d70eadf76d3a"
canonical_url: "https://www.kombo.dev/blog/kombo-vs-merge"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-22T01:38:35.972465+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:ef4156265bcc052c6c540d095ec12fbd0ac2a607af01b9f0cb02ba1a39e5e3b7"
---

# Kombo vs. Merge: A fact-based comparison

Merge published a comparison page positioning themselves against Kombo. We don’t have a comparison page of our own, but we’ve had enough customers and prospects addressing the page, or its content, in conversations that it felt worth addressing directly.


## **TL;DR we follow different strategies**


The fundamental difference between Kombo and Merge isn’t about feature checkboxes, but about product strategy:


1. Merge started with HRIS and ATS integrations, but has been diversifying away for years - first into ticketing, CRM, and chat, and most recently beyond customer-facing integrations entirely with Merge Gateway, an LLM routing product for AI infrastructure.
2. Kombo has always been, and will remain, focused exclusively on people data. This is what we do best.


## **Respect where it's due**


Before we get into details, we’d like to stress the deep respect we have for Merge. It’s a great company with a very capable team, and they deserve all the credit for popularizing the unified API category as one of the first movers. None of what follows is intended to be a takedown.


That said, their comparison page contains several claims about Kombo that are either factually incorrect or outdated. For technical and product leaders evaluating the integration space, these inaccuracies matter.


## **Why comparison pages exist — and why this one exists**


Competitor pages exist for SEO and LLM search. When you’re using Google or an LLM to search for a solution or to compare competitors, companies naturally want to own the answer and narrative. That is a very rational marketing move, and we’re not criticizing it.


What’s worth noting, however, is that the page was published two years ago and had inaccuracies from the start. We reached out to Merge last year to request an update and to correct the inaccuracies, but they haven’t followed through on their promise to do so.


## **What sets us apart**


Here’s what technical and product leaders in people tech typically evaluate when selecting an integration vendor:


### **API coverage**


A unified API provider should grow with your needs. Merge diversifies across over eight verticals, while Kombo focuses exclusively on workforce tech. That means our entire engineering team works on HRIS, ATS, Payroll, and LMS integration challenges, rather than spreading thin across categories.


### **Integration depth and data coverage**


Not all integrations are built equally, and depth matters more than category counts. Because we build exclusively in people data, we are able to provide more depth per integration, and extend into adjacent people-tech categories like LMS and Assessment rather than chat, CRM, or ticketing.


For HRIS, Kombo supports workflows like[creating employees in enterprise HRIS](https://docs.kombo.dev/hris/implementation-guide/create-employee) and specialized data models like[performance data](https://docs.kombo.dev/hris/features/performance-data) ,[staffing entities](https://docs.kombo.dev/hris/features/staffing-entities) , and[skills](https://docs.kombo.dev/hris/v1/get-skills) that Merge doesn’t support.


For ATS, Kombo supports updating application stages for 45 systems and posting screening-question answers for 91. Merge supports updating application stages for 14 systems and posting screening-question answers for 11. See the table below for the full breakdown across the two categories.


### **Support quality**


Integration challenges need both deep system knowledge and fast resolution. Kombo gives you direct Slack access to the engineers who built your integrations on every plan. Merge routes tickets through general support and reserves Slack and onboarding support for its enterprise tier.


## **Correcting the comparison table**


This is Merge’s own feature comparison, with the numbers brought up to date.


Feature comparison Kombo Merge


Unified API categories Exclusive focus on HRIS, ATS including assessments, and LMS 8 categories


API-based integrations for HRIS and ATS 184 integrations 142 integrations


Normalized data models across HRIS and ATS 31 common models 26 common models


Authenticate and authorize integrations Embedded link and URL-based link Embedded link and URL-based link


SDKs: Node, Java, Python, Go, Ruby, etc. 3 languages 6 languages


Battle-tested by 10,000+ organizations ✅ ✅


Customizable Link ✅ ✅


Transparent integration coverage ✅ ✅


Audit trail in dashboard and via API ✅ ✅


Read and write custom fields ✅ ✅


Supports passthrough data ✅ ✅


Rate-limit management for passthrough requests ✅ ✅


Automatic issue detection and fully searchable logs ✅ ✅


Top-tier support with ability to request new integrations ✅ ✅


Delete individual data via API ✅ ✅


Supports normalized CSV uploads ❌ ✅


Supports normalized SFTP uploads ✅ ✅


Access to raw data for all integrations ✅ ✅


Respects data redaction for raw data ✅ ✅


Sync status for common models ✅ ✅


GTM guidance for bringing integrations to market ✅ ✅


Support in multiple time zones: Berlin, NYC, SF ✅ ✅


Office in Berlin for support ✅ ✅


Choose where your data is hosted US and EU multi-tenant US, EU, and APAC multi-tenant, single-tenant


Security assessments SOC 2 Type II, ISO 27001, HIPAA SOC 2 Type II, ISO 27001, HIPAA


Complies with GDPR ✅ ✅


## **Going deeper across HRIS and ATS**


Here’s the breakdown of how Kombo and Merge compare at the level of actual data models and operations supported across systems.


Feature comparison Kombo Merge


**HRIS integration capabilities**


[API-based integrations for HRIS](https://docs.kombo.dev/hris/connectors) 85 integrations 80 integrations


Normalized data models for HRIS integrations 11 common models 9 common models


[”Employee”-level filtering](https://docs.kombo.dev/hris/implementation-guide/filtering-employees/using-the-filtering-ui#using-the-filtering-ui) ✅ ✅


[Creating employee records](https://docs.kombo.dev/hris/implementation-guide/create-employee) 36 integrations Not supported


[Getting absences](https://docs.kombo.dev/hris/v1/get-absences#get-absences) 34 integrations 35 integrations


[Skills data model](https://docs.kombo.dev/hris/v1/get-skills) ✅ ❌


[Performance data model](https://docs.kombo.dev/hris/v1/get-performance-review-cycles) ✅ ❌


[Staffing entities data model](https://docs.kombo.dev/hris/v1/get-staffing-entities) ✅ ❌


[Time and attendance data model](https://docs.kombo.dev/hris/v1/get-timesheets#get-timesheets) ✅ ✅


**ATS integration capabilities**


[API-based integrations for ATS](https://docs.kombo.dev/ats/connectors) 99 integrations 62 integrations


[API-based integrations for ATS assessments and background checks](https://docs.kombo.dev/assessment/connectors) 16 integrations None


Normalized data models for ATS and ATS assessment integrations 20 common models 17 common models


[Posting applications](https://docs.kombo.dev/ats/v1/post-jobs-job-id-applications#supported-integrations) All integrations 25 integrations


[Posting result link to application](https://docs.kombo.dev/ats/v1/post-applications-application-id-result-links) 64 integrations None


[Posting attachment to application](https://docs.kombo.dev/ats/v1/post-applications-application-id-attachments) 56 integrations 19 integrations


[Patch application stage](https://docs.kombo.dev/ats/v1/put-applications-application-id-stage) 47 integrations 14 integrations


Posting screening question answers 78 integrations 11 integrations


[Reject application with provided note](https://docs.kombo.dev/ats/v1/post-applications-application-id-reject) 24 integrations ❌


Background check module ✅ ❌


Webhook support for real-time and high-volume use cases ✅ ✅


Auto-answering screening questions ✅ ❌


**Developer experience**


[Raw data field discovery for easier field mapping](https://docs.kombo.dev/hris/features/data-explorer#data-explorer) ✅ ❌


[Customize what data is being synced](https://docs.kombo.dev/hris/features/scopes) ✅ Only professional and enterprise plan


[Test and troubleshoot with built-in API client](https://docs.kombo.dev/hris/features/komboman#komboman) ✅ ❌


[Detailed implementation guides for use cases](https://docs.kombo.dev/hris/implementation-guide/context) ✅ ❌


Permission testing ✅ ✅ with limitations


Send customers reminders for unfinished integrations ✅ ❌


**Support & partnership**


Support during onboarding ✅ Enterprise-tier only


Direct communication on Slack All tiers Enterprise-tier only


Built-in sandboxes ✅ ❌


Implementation support Included on Scale and Enterprise plan ❌


## **On security and compliance**


Merge’s page implies Kombo doesn’t offer equivalent protections. It does. Point for point:


- **DPAs and SCCs:** In place with all customers, with standard contractual clauses for cross-border transfers.[DPA](https://www.kombo.dev/data-processing-agreement)
- **Data residency:** US and EU tenancy, and your customer data stays where you choose.
- **Data minimization via scopes:** Configure exactly which fields, objects, and data points sync per integration; anonymize fields at ingestion.[Scopes](https://docs.kombo.dev/hris/features/scopes)
- **Selective sync:** Supported.[Filtering](https://docs.kombo.dev/hris/implementation-guide/filtering-employees/using-the-filtering-ui#using-the-filtering-ui)
- **Data deletion:** Deleting a linked account purges associated data; selective exclusion is supported.[Data policy](https://docs.kombo.dev/hris/features/filtering/data-policy#data-deletion)
- **Audit trail:** All user actions logged and searchable in-dashboard.[Audit logs](https://docs.kombo.dev/hris/features/audit-logs)
- **SSO / SAML and RBAC:** Both supported.[Roles](https://docs.kombo.dev/hris/guides/environments#roles)
- **Certifications:** SOC 2 Type II, HIPAA, GDPR, and ISO 27001.[Security](https://www.kombo.dev/security)


## **The framing problem**


The deeper issue with Merge’s page is that the framing presents Merge as a superset of Kombo - a bigger version of the same thing. That’s not an accurate description of either product, and the gap has widened over the past year.


When you build your product on a unified API, you’re betting that your vendor will still be investing in your category’s connectors, write operations, and enterprise edge cases five years from now.


Merge is a multi-product company. Alongside eight integration categories, their newest flagship product, Merge Gateway, is an LLM router for AI infrastructure - a different market with a different buyer. That’s a rational strategy, and it may be a great product. But engineering attention is finite, and every new market dilutes what’s left for any single category - including the HRIS and ATS integrations your product depends on.


Kombo is a vertical specialist built for HR and people tech. Every engineer we hire works on HRIS, ATS, LMS, and assessment integrations, because that’s the entire company. That’s where 4+ years of building exclusively in this space compounds, and why the gap grows rather than closes.


If your product spans many unrelated categories and you want one vendor for all of them, Merge is built for that. If you’re building in people tech, ask both of us the same question: what share of your roadmap is going into my category next year?


## **You don’t have to take our word for it.**


Merge’s newest flagship product, Merge Gateway, isn’t solving for customer-facing integrations. It’s an LLM router and “control plane for production AI.” It’s what Merge promotes on its homepage today, it’s where their case studies and launch promotions are going.


This is a rational bet for Merge. But if you’re choosing an integration vendor for the next four years, it tells you where their engineering attention is going.


## **What customers are saying about Merge**


Over the past few months, we’ve collected feedback during prospect and customer calls. Three themes come up consistently, as provided by customers in their own words.


### **Merge is moving away from people tech**


Merge has signaled, both publicly and directly to customers, that its focus is shifting toward newer categories like CRM, ticketing, and chat. One prospect shared the note they received from Merge:


> *We have decided to deprioritize bringing in new customers on our professional plan for our ATS category, as we're currently heavily investing in other categories and products.*


A long-time partner of theirs put it more plainly:


> *They care a lot more about the CRM category than they do about HRIS. The HRIS category is languishing.*


### **Support is slow outside the enterprise tier**


Customers on Merge’s Launch and Professional plans repeatedly tell us resolution is slow:


> *Frankly, I'm less than impressed with how Merge engages with customers. We've had to wait a long time to get answers on technical details.*


### **Syncing large instances is unreliable**


This came from one of Merge’s own featured case-study customers, who’s now migrating 300+ integrations to Kombo:


> *The first sync with you was extremely smooth. We've not had that with Merge. In a lot of cases the sync is always a little bit off. Sometimes it syncs all the data, sometimes it doesn't, sometimes it takes ages. Workday is a particularly hard one, so getting through the entire sync cleanly with you is already a very good sign.*


## **One last thing**


If you’re evaluating integration providers and building in people tech, pressure-test both platforms on the specific integrations and data models that matter for your use case, not on category counts or checkbox comparisons.


Ask:


- Where is the vendor investing next - deeper into your category, or into new markets and product lines?
- How many integrations actually support writing the specific fields you need?
- How do they handle the initial sync of very large enterprise instances, where you’ll hit rate limits?
- How many systems do they provide sandboxes for?
- Can they fast-track partnerships for hard-to-crack vendors?
- How well does the support team understand the quirks of the specific systems your customers use?


Those questions will tell you more than any comparison page.


## **Frequently asked questions**


How is Kombo different from Merge and other unified APIs?


+


Kombo focuses exclusively on people tech — HRIS, ATS, payroll, assessments, and LMS — while most unified API providers spread across many unrelated categories like CRM, ticketing, and file storage. That focus translates into deeper bi-directional coverage, more write-back operations per system, and a support team made up of the engineers who actually built each integration.


If you need integrations across many unrelated software categories, a horizontal platform may fit better. If you're building in people tech, depth in this vertical is where Kombo is strongest.


How does Kombo handle real-time data and webhooks?


+


Kombo is webhook-first. When an underlying system supports webhooks, Kombo validates the change and forwards a data-change webhook to you in real time.


For systems without native webhooks, Kombo syncs on a schedule — every three hours by default, configurable down to every five minutes on higher plans — and you can always trigger a forced sync. API rate limits start at 300 requests per minute and can be raised on Scale and Enterprise plans.


Does Kombo support enterprise systems like Workday, SAP SuccessFactors, Oracle, and UKG?


+


Yes. Kombo maintains direct partnerships and sandbox access for closed-ecosystem enterprise systems — including Workday, SAP SuccessFactors, Oracle, and UKG — that are often difficult to integrate with on your own. Initial large-instance syncs are actively monitored by Kombo engineers to prevent timeouts, which is a common pain point when syncing very large enterprise tenants.


How customizable is Kombo? Can I access custom fields and raw data?


+


Yes. Beyond the normalized data models, Kombo gives you a Custom Field Explorer to search the actual raw data stored in each connected system and map non-standard fields — including custom fields — without engineering support. You can also use passthrough requests to reach system-specific endpoints that aren't part of the unified model, and configure scopes to control exactly which fields and objects sync per integration.


Where is my data stored, and how long does Kombo keep it?


+


Kombo offers both US and EU data residency, with EU customer data hosted inside the European Union. Kombo persists synced data — rather than operating as pure passthrough — to power change tracking, deletion tracking, and resilience during upstream outages.


Data is encrypted in transit and at rest, and is automatically deleted when no longer needed within 14 days, supporting GDPR-aligned data minimization.


Is Kombo secure and compliant?


+


Kombo is SOC 2 Type II and ISO 27001 certified, and HIPAA- and GDPR-compliant, and has passed security reviews from more than 20,000 organizations. Authentication uses per-environment API keys with optional expiry and IP allowlisting, webhooks are signed with HMAC-SHA256, and the platform provides audit logs, role-based access control, and configurable data scopes.


See the security and compliance section above for a point-by-point breakdown.


How hard is it to migrate from Merge to Kombo?


+


Most teams find the switch lighter than expected: you're replacing one unified API with another, so the integration model is conceptually familiar, and you gain coverage rather than lose it. Kombo provides a sandbox to validate every data flow end-to-end before going live, and the support engineers help map your existing flows during onboarding. Customers have migrated hundreds of live integrations to Kombo this way.


How does Kombo's pricing work?


+


Pricing is based on an annual platform fee plus the number of connected integrations or customers, with volume-friendly scaling as you grow. Because the model rewards volume, Kombo tends to be most cost-effective for higher-volume, long-term use cases.


For exact pricing tailored to your systems and volume, request a quote.


What kind of support does Kombo offer?


+


Every plan includes direct Slack access to the engineers who built your integrations — not a general support queue. Kombo operates across Berlin, NYC, and SF time zones, provides built-in sandboxes for testing, and includes implementation support on Scale and Enterprise plans. Many providers, by contrast, reserve Slack and onboarding support for their enterprise tier only.
