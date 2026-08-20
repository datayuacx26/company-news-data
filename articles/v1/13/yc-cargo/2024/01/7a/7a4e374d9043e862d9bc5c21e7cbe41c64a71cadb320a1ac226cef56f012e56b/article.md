---
schema_version: "1.0.0"
document_id: "7a4e374d9043e862d9bc5c21e7cbe41c64a71cadb320a1ac226cef56f012e56b"
company_key: "yc-cargo"
company: "Cargo"
source_id: "yc-cargo-news-import-f4a8864e899b"
canonical_url: "https://www.getcargo.ai/blog/ditch-the-leads-object"
published_at: "2024-01-01T00:00:00+00:00"
first_seen_at: "2026-07-21T12:34:39.888673+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:c32330cea2049fdfa71031a1d937cb4fa1c21c56dad2f1a0fdac078413bb49e1"
---

# Ditch the Leads Object

THE most divisive issue in RevOps that I’ve come across so far is the Leads vs Contacts debate.


The Lead object in Salesforce is a bit of a mixed bag. It’s like a temporary holding space for potential clients, but it’s not really clear what it is or why it’s there. It’s kind of stuck in limbo.


If you ask the people who’re in love with it, they’ll tell you they can’t live without it. For others, it is the messiest place in their whole CRM (customer relationship management) tool.


Salesforce Lead vs Contact comparison post


*Taken from[Max’s post](https://www.linkedin.com/posts/tshaped-marketer_crmadmin-salesops-salesforce-activity-7094262373196161024-PLjY) a few weeks ago*


To be fair to Marc Benioff, there’s a smart reason behind creating it.


This Contact-Leads dichotomy lets the marketing team add their potential clients to the database without messing up the main sales areas: Accounts, Contacts, and Opportunities.


Sales and Marketing have always sat together uneasily, and this was an ok way to manage that turf war.


Sales teams often see these early-stage entries as uncertain and speculative. So, the Lead object acts as a tracker for these early steps in the sales process. When the lead can be linked to a company and has **a high likelihood to convert to a potential sale,** often determined via a scoring model (fit-based or behavioral), then it gets upgraded into a contact.


It’s a practical, not entirely elegant approach. My take though, is that it’s time to get rid of it. Hear me out.


Starting with basics first.


## What is the Contacts object in your CRM?#


The Contacts object in your CRM stores info about the people your sales team talks to.


Contacts have lots of information because they are linked to Accounts (i.e. the companies these contacts work for). We save many details under Accounts that we wouldn’t ask for when we first meet someone.


In Salesforce, Contacts are also connected to Opportunities. This shows us their role in possible deals. This becomes more useful as the average size of your prospects increases, if you’ve been prospecting John Deere as an example, where you might have 10,000 Contacts but only 5 are really helping with the deal.


However, there are a few situations where managing Contacts can be tricky:


**Cleaning up bad Info** : Every Contact should be linked to an Account, but sometimes forms don’t ask for the Company Name.


**Avoiding overcrowding accounts** : In the past, we would only link Contacts to Accounts if they were key people we needed to talk to.


**How salesforce works** : Salesforce was originally built to focus more on Leads for follow-ups and creating Opportunities.


When Salesforce was being setup, it tried to solve this dilemma with a technical solution. By introducing another object, called Leads.


Neat solution, but it introduced some problems of its own.


## Why do we need a Leads object?#


Leads are like a temporary space for people that may become contacts at some point. By keeping these apart, it helps keep our main contact list uncluttered.


Basically, Leads are people we are not sure about yet:


- They might work at a company (aka Account) we already know, be a new chance to make a sale, or they might not be useful for us.
- Usually, when someone seems interested, we add them as a lead. Then, we check if they are really a good fit for us. If yes, we move them to our main contact list. If not, we forget about them being there.
- Leads usually don’t have a direct connection to Accounts in our system.


When someone is a Lead, we usually know only a few things about them:


- What they told us on a website form.
- Information from a list we bought: lists bought on ZoomInfo or Lusha, usually show up in our system as leads
- Or, what was on their business card.


This way of doing things often misses important details. Are they already a customer? Are they involved in a current deal? Do they work at an account we’re targeting?


## The Downsides of Using a Leads Object#


1. **Leads are like a Messy Drawer** : Even though it’s good to have a place to put information quickly, it often ends up being too cluttered to find anything later on. Like a junk drawer.


As Leads are built to be these free-form records inside your CRM, they hardly follow any norms. Most of them will be missing important fields of data, each in their own way.


For example, among many leads from the same company, they might have slightly different information - like different ways of writing the company’s name or different industries listed.


Of course, data stored by these Leads doesn’t remain static in the real world. Ideally, you could consistently enrich and standardise this mess with 3rd party databases. But these are expensive and it doesn’t necessarily make economic sense at that scale, enrichment is typically paid per record.


Given what most companies’ Leads objects look like, it’s generally a nightmare to try to integrate it to a marketing tool to manage your interactions with your leads.


1. **Lead to Account Matching is a Nightmare** : The problem stems from having two objects with fundamentally different characteristics.


While both objects represent individuals, Leads are unorganized and unqualified, whereas Contacts are organized under an Account record that they’re linked to.


The simplest way to put it: Leads are “non-enriched” Contacts. Qualifying them through data enrichment or touchpoints costs time and money, and a clean method to automate this still eludes most teams, often due to persistent duplication issues.


1. **It confuses handover and ownership between marketing & sales** : Ironically, the Lead object, designed to keep peace between teams, often makes the turf war worse.


Consider these questions:


Under what conditions should a Lead become a Contact?


Deciding when to turn a lead into a contact can be tricky and lead to disagreements within the team. Should a mere touchpoint be the trigger, or does it take multiple touchpoints and passing scoring thresholds for a Lead to become a contact?


Who should make that call, an SDR or an account executive?


Discrepancies in opinions arise: one team member might see potential in a prospect, while another disagrees. And then there’s the age-old tug-of-war between marketing and sales. Does marketing handle Leads while sales focuses on Contacts? But what if sales bypasses marketing and creates Contacts directly?


1. **Reporting is Tougher** : When you want to make reports, having leads and contacts separate can make it complicated.


Imagine you wanted something simple like: a list of all managers or directors at software companies in California that aren’t already customers.


To get these numbers, you’d need to pull multiple reports, download spreadsheets, match up the data, remove duplicates, and then send it out.


It adds a lot of extra steps.


1. **Training and Daily Work Get Complicated** : Even though in theory sales reps should only need to follow up with Leads, in reality, they end up having to check both Lead and Contact objects in the CRM before taking actions.


This makes everything more complicated, from daily tasks to training new team members.


## What if we used only the Contacts object#


After reading this article, you decide you’re using only the straightforward Contacts object. Anything resembling “Leads” can just be a stage attribute if necessary.


Easy sell? Not yet.


So far, the GTM world has lacked an efficient automation environment to execute workarounds for the drawbacks of a Contacts-only approach that we discussed at the beginning of this article.


But that’s changing. Modern revenue orchestration platforms now provide the infrastructure needed to make this work, and this is exactly what we built Cargo to solve.


Here’s how it works.


Yes, you need a separate space to work on unqualified contacts (the function Leads served), to avoid polluting your main Contacts object.


My take: that separate space should not be in your CRM at all. Your CRM wasn’t built to be a junk drawer.


In an era of automation and[waterfall enrichment](https://www.getcargo.ai/blog/waterfall-enrichment-the-secret-sauce-to-maximize-enrichment-coverage) , pushing incomplete records to your CRM is retrograde.


What if there was an intermediary system designed to transfer only enriched, validated, and scored contacts into your CRM, whether they’re linked to an existing or a newly established account?


You can call it a middleware. Max likes to refer to it as a[Prospect Relationship Management Solution](https://www.getcargo.ai/blog/what-is-a-prospect-relationship-management) (it makes for better initials, admittedly)


I call it, to keep it simple, a pre-CRM solution.


What does it imply? It’s a system where you store your entire Total Addressable Market, pre-qualified prospects, until they’re ready to become real Contacts in your CRM.


It comes with a workflow builder (that salespeople can actually use) that integrates with best-in-class sales tools, allowing you to systematically perform segmentation, enrichment, scoring, and account assignment operations to decide which records to promote to your primary Contacts and Accounts objects in your CRM.


The outcome? CRM hygiene.


## A Pre-CRM solution renders the Lead object largely redundant#


This new hierarchy separates the function of the CRM and pre-CRM as follows:


A CRM is responsible for enriching and nurturing the existing relationship between the company and its customers (i.e., loyalty, satisfaction, and expansion).


A pre-CRM orchestrates a set of workflows and best-of-breed tool integrations that companies use to store and manage interactions with prospects, such as lead nurturing, prospecting sequences, or tracking and analyzing prospect interactions to understand their maturity level.


Why does it create better hygiene?


It’s for the following simple reasons:


1.


**Bypass lead conversion nightmares:** With a pre-CRM, prospects are nurtured until they’re qualified, and only then are they transferred into the CRM as opportunities or direct contacts. This bypasses the need to convert ‘Leads’ in Salesforce.


2.


**Best-in-class toolkit:** Because a pre-CRM is plugged to the best data collection and validation tools (unlike a CRM’s app exchange store), by the time these contacts enter Salesforce, they are well-vetted, reducing the clutter of unqualified or poorly qualified leads that can accumulate in the traditional Leads object.


3.


**Focused sales efforts:** Because there is only one object (Contacts) to track and reach out to, sales teams spend less time juggling contexts and focus only on high-potential opportunities.


4.


**Marketing - Sales discipline:** Because the handover between pre-CRM and CRM is clean and automated, there’s fewer reasons to cause confusion on what transitions over the CRM and when. What’s even better is since these rules are written in a no-code interface, there’s a strong foundation for argument and iteration instead of mental heuristics.


5.


**Easier reporting:** Simple analytical questions have simple answers in your CRM reports due to reduced divergences of definitions between multiple objects (there’s only Contacts and Accounts left after all)


6.


**Better economics:** Pre-CRMs are typically built on top of data warehouses, which are 10-20x more cost-effective per record than your average Salesforce or HubSpot storage instances, making it economical to store millions of prospects instead of thousands.


## Key Takeaways#


- **The Lead object creates a “junk drawer” problem** : Leads accumulate inconsistent, incomplete data that’s expensive to enrich at scale, most companies end up with thousands of messy records they can’t effectively use or integrate with marketing tools
- **Lead-to-Contact conversion is the #1 RevOps pain point** : Matching unstructured Leads to organized Accounts causes duplication nightmares, handover confusion between marketing and sales, and endless debates about conversion criteria and ownership
- **Contacts-only approach requires pre-CRM infrastructure** : You can’t just delete Leads without a system to handle unqualified prospects, you need a “middleware” layer (Pre-CRM/PRM) where TAM lives, gets enriched, scored, and validated before entering CRM
- **Pre-CRM architecture enables true CRM hygiene** : By orchestrating enrichment, scoring, and qualification workflows outside Salesforce, only vetted Contacts tied to proper Accounts enter your CRM, eliminating the 5 core problems Leads create
- **The separation clarifies system purposes** : CRM manages existing customer relationships (loyalty, expansion, satisfaction); Pre-CRM orchestrates prospect workflows (nurturing, sequencing, qualification), each system does what it’s built for


## Frequently Asked Questions#
