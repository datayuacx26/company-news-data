---
schema_version: "1.0.0"
document_id: "08a7c8424a29b4f426f2e8f837df702495ffeb1823a4b4d8cc001fa517a2acd6"
company_key: "yc-bits-2"
company: "Bits"
source_id: "yc-bits-2-news-import-8ae56e6aae1c"
canonical_url: "https://klausai.com/blog/openclaw-lead-generation-cold-outreach-that-actually-works/"
published_at: "2026-04-02T00:00:00+00:00"
first_seen_at: "2026-07-24T21:07:46.493176+00:00"
fetched_at: "2026-07-28T21:56:50.434513+00:00"
content_hash: "sha256:5bb29feae727602c82da3c4590d356fc857e47a8acf920c507a46b11a35e321e"
---

# OpenClaw for Lead Generation: How to Set Up Cold Outreach That Actually Works

Lead scraping and cold outreach is one of the thing our customers ask about most. Outbound. Finding prospects, enriching their contact info, writing personalized emails, and sending them without touching your personal inbox.


Every “AI lead generation” article I’ve found is written by a CRM company trying to sell you a platform. They describe what AI agents could do in theory. None of them show a working agent running the full pipeline on real tools.


Klaus ships with Apollo, Hunter.io, and AgentMail pre-configured. But this workflow can run on any OpenClaw instance, not just ours.


## What Does AI Lead Generation Actually Look Like?


AI lead generation is a four-stage pipeline where an agent finds prospects, enriches their contact information, drafts personalized emails, and sends them from a dedicated inbox.


That’s it. Not “autonomous prospecting powered by agentic AI.” Not “multi-signal intent-based targeting.” Four concrete steps.


**Discovery.** Find companies and people matching your ideal customer profile. Your agent searches Apollo’s database of 275 million contacts, or crawls platforms like X, GitHub, and Product Hunt using the[lead-hunter skill](https://playbooks.com/skills/openclaw/skills/lead-hunter) .


**Enrichment.** Get verified emails, job titles, company data. Apollo provides the contact data. Hunter.io verifies the email addresses actually work before you send to them.


**Drafting.** Write personalized outreach based on what the enrichment found. The agent reads the prospect’s company details, recent funding, tech stack, and writes an email that references something specific. Not a mail merge. An actual personalized email.


**Sending.** Deliver from a dedicated agent inbox, track replies, send follow-ups. AgentMail gives your agent its own email address, separate from yours.


Why does this work better than traditional cold email?[Hunter.io’s 2026 State of Cold Email report](https://hunter.io/the-state-of-cold-email) analyzed 31 million emails and found that personalized emails (just two custom attributes) get a 56% higher reply rate: 5.6% vs 3.6% for generic blasts. The problem is that manual personalization doesn’t scale. An agent does both.


The broader trend confirms the direction.[McKinsey research](https://www.mckinsey.com/capabilities/growth-marketing-and-sales/our-insights/unlocking-profitable-b2b-growth-through-gen-ai) shows B2B sales teams using AI see 13-15% revenue increases.[Gartner predicts](https://www.digitalcommerce360.com/2025/11/28/gartner-ai-agents-15-trillion-in-b2b-purchases-by-2028/) AI agents will intermediate $15 trillion in B2B purchasing by 2028. The tools exist. The question is how to wire them together.


## The Tools You Need (and How They Fit Together)


Each tool handles one part of the pipeline. Here’s what does what.


Tool What it does Pipeline stage


**Apollo** People and company search, domain enrichment Discovery + Enrichment


**Hunter.io** Email verification and email finder Enrichment (verify before sending)


**Lead-hunter skill** Three-stage pipeline: discover, enrich, score Full pipeline orchestration


**AgentMail** Dedicated email inbox for the agent Sending


**Exa** AI-native search for finding companies by description Discovery (niche searches)


The[lead-hunter skill](https://playbooks.com/skills/openclaw/skills/lead-hunter) orchestrates the full pipeline. It crawls discovery sources, enriches each prospect with contact data and company profiles, scores them against your ICP, and exports CRM-ready lists. Apollo provides the prospect data. Hunter.io verifies emails before you send.[AgentMail](https://www.agentmail.to/blog/openclaw-agent-email-inbox) provides the dedicated sending inbox.


On Klaus, Apollo and Hunter.io are available via Orthogonal with pre-loaded credits. No API keys to configure. On self-hosted OpenClaw, you bring your own keys.


One thing worth noting about AgentMail: your agent’s email inbox contains sensitive data. Verification codes, password resets, private links. If your agent uses your personal inbox and a malformed email comes in, it could trigger the wrong behavior ([AgentMail](https://www.agentmail.to/blog/openclaw-agent-email-inbox) ). A dedicated inbox keeps everything isolated.[AgentMail raised $6M](https://techcrunch.com/2026/03/10/agentmail-raises-6m-to-build-an-email-service-for-ai-agents/) to build this exact infrastructure because the problem is common enough.


## How to Set Up the Full Outreach Pipeline


### Step 1: Install the Lead Generation Stack


On any OpenClaw instance, install the skills:


```text
clawhub   install   apollo
clawhub   install   lead-hunter
clawhub   install   agentmail
```


Configure your Apollo API key in` config/apollo.env` :


```text
APOLLO_BASE_URL=https://api.apollo.io
APOLLO_API_KEY=your_key_here
```


The[Apollo skill](https://github.com/openclaw/skills/blob/main/skills/jhumanj/apollo/SKILL.md) loads these credentials automatically. Rate limiting applies at roughly 600 requests per hour, so the skill handles pacing for you.


On Klaus, skip this step. Apollo and Hunter.io are pre-configured via Orthogonal, and AgentMail credentials come with your instance.


### Step 2: Find Prospects


Define your ideal customer profile in conversation. Tell your agent something specific:


> “Find me 50 marketing VPs at Series A SaaS companies in the US with 20-100 employees.”


The agent uses Apollo’s people search to pull matching contacts. Behind the scenes, that’s:


```text
apollo-people-search.sh   "vp marketing"   1   50
```


For company-level research, domain enrichment fills in the context:


```text
apollo-enrich-website.sh   "example.com"
```


This returns company size, industry, tech stack, and funding information ([Apollo skill docs](https://github.com/openclaw/skills/blob/main/skills/jhumanj/apollo/SKILL.md) ).


If you’re targeting a niche that Apollo doesn’t cover well, the[lead-hunter skill](https://playbooks.com/skills/openclaw/skills/lead-hunter) also crawls X, GitHub, Product Hunt, and custom RSS feeds. Useful for finding founders in specific communities.


### Step 3: Enrich and Verify


Raw prospect lists need cleaning before you send to them. The lead-hunter skill handles enrichment in bulk: email patterns, social links, company profiles, tech stack, and funding data ([lead-hunter docs](https://playbooks.com/skills/openclaw/skills/lead-hunter) ).


Hunter.io then verifies each email address. On Klaus, Hunter.io is accessed through the` orthogonal/hunter` skill — your Orthogonal credits cover usage, so there’s no separate Hunter.io API key to manage. On self-hosted OpenClaw, install it with` orth skills add orthogonal/hunter` and bring your own Orthogonal or Hunter.io key.


This step matters more than most people realize.[Hunter.io’s data](https://hunter.io/the-state-of-cold-email) shows an average bounce rate of 3.6% across verified lists. Unverified lists bounce at much higher rates, and high bounce rates damage your sender reputation fast.


The scoring step ranks prospects by ICP fit and intent signals. Not every match is worth emailing. The output is a prioritized, CRM-ready list with verified emails and enriched profiles.


### Step 4: Draft and Send Personalized Outreach


This is where the agent earns its keep. It drafts emails using the enrichment data: the prospect’s role, their company’s recent funding round, their tech stack, a blog post they published.


The data backs this approach. From[Hunter.io’s 2026 report](https://hunter.io/the-state-of-cold-email) :


Tactic Impact on reply rate


Two custom attributes in email body +56% (5.6% vs 3.6%)


Three-message follow-up sequence +106% more total replies


Custom domain (not freemail) +108% higher reply rate


21-50 recipients per campaign +158% vs 500+ recipients


20-49 emails per day per account +27% vs higher volumes


But there’s a catch. The same report found that 69% of decision makers say it bothers them if AI was used, unless the output feels human. The agent needs to write like a person, not a template. That means referencing something genuinely specific about the prospect, not just inserting` {{first_name}}` into a formula.


Send via AgentMail from a dedicated inbox. The free tier includes 3 inboxes, which is enough to get started. Custom domains are available on paid plans for branded sending ([AgentMail](https://www.agentmail.to/blog/openclaw-agent-email-inbox) ).


Follow-up sequences matter. Three messages instead of one doubles your total replies ([Hunter.io](https://hunter.io/the-state-of-cold-email) ). Space them out. Don’t blast three emails in three days.


## What We’ve Seen Work (and What Doesn’t)


### What Works


**Smaller segments, better results.** Hunter.io’s data is clear: campaigns targeting 21-50 recipients get 158% higher reply rates than those blasting 500+. Quality targeting beats volume every time ([Hunter.io](https://hunter.io/the-state-of-cold-email) ).


**Custom sending domains.** Sending from` yourname@yourdomain.com` instead of a freemail address nearly triples reply rates. AgentMail supports custom domains on paid plans, so your agent sends from a professional address.


**Real context in every email.** The agent pulls the prospect’s recent funding round, a job posting they have open, or a blog post they published. Two specific details are enough. More than that and the email reads like a dossier, not a message.


**Dedicated agent inbox.** Your agent’s outreach stays separate from your personal email. No risk of it replying to an internal thread by accident. No risk of your personal domain getting flagged if deliverability drops.


**Review the first batch.** Before you let the agent send autonomously, review 10-20 drafts. Catch tone issues early. Once you’re confident it writes like you, enable auto-send for follow-ups.


### What Doesn’t Work


**Mass blasts with the same template.** The data confirms what everyone suspects: high-volume, low-personalization outreach underperforms dramatically. If you’re sending the same email to 500 people, you’d get better results sending a personalized email to 50 ([Hunter.io](https://hunter.io/the-state-of-cold-email) ).


**Auto-sending without review.** If the agent’s tone is off and you’ve already sent 200 emails, you’ve burned through a prospect list and potentially damaged your domain reputation. Start in draft mode.


**Using your personal Gmail.** Your agent might accidentally reply to the wrong thread. Email providers will rate-limit you. And if prospects mark your emails as spam, your personal email deliverability suffers too.


**Over-enriching prospects.** You don’t need 100 data points per person. You need 2-3 relevant signals that inform the email. Company size, recent funding, and one specific detail about the person’s role is enough for most outreach.


## Frequently Asked Questions


### How much does the lead generation stack cost?


On Klaus Starter ($19/mo), you get a one-time $20 Orthogonal credit that covers Apollo and Hunter.io usage, plus a dedicated AgentMail account. Self-hosted OpenClaw: bring your own API keys ([Klaus pricing](https://klausai.com/pricing) ).


### Can the agent send emails without my approval?


Yes, but start with review mode. Have the agent draft emails and present them for approval. Once you trust the tone and targeting, enable auto-send for routine follow-ups. If you skip review and the tone misses, you’ve burned a prospect list with no way to undo it.


### How many emails should I send per day?


[Hunter.io’s data](https://hunter.io/the-state-of-cold-email) shows 20-49 emails per day per email account is the sweet spot, with a 27% higher reply rate than higher volumes. More is not better. AgentMail supports multiple inboxes if you need to scale across campaigns.


### Does this work for B2C businesses?


The workflow described here is B2B-focused because Apollo and Hunter.io are B2B data providers. For B2C, you’d swap the discovery sources but the enrichment and outreach pattern is the same. The[lead-hunter skill](https://playbooks.com/skills/openclaw/skills/lead-hunter) supports custom discovery feeds if you need to target different audiences.


## Key Takeaways


- The full pipeline: discover prospects, enrich with verified contact data, draft personalized emails, send via a dedicated inbox.
- Personalization drives results: two custom attributes in an email body produce a 56% higher reply rate ([Hunter.io](https://hunter.io/the-state-of-cold-email) ).
- Start small. 21-50 targeted prospects outperform 500+ generic contacts by 158%.
- Use a custom domain and a dedicated agent inbox. Never send cold outreach from your personal email.
- Review the first batch of drafts before enabling auto-send. Catch tone issues early.
- On Klaus, the entire stack (Apollo, Hunter.io, AgentMail) comes pre-configured with Orthogonal credits included.
- This is lead generation, not spam. Smaller segments, real personalization, and dedicated sending infrastructure.


---


Want to try this workflow?[Sign up at klausai.com](https://klausai.com/) and Apollo, Hunter.io, and AgentMail are ready on your instance. For the full deep-dive on scraping tools, see our[web scraping guide](https://klausai.com/blog/openclaw-web-scraping-tools-skills-what-actually-works) . For more customer workflows, see[how our customers actually use OpenClaw](https://klausai.com/blog/openclaw-use-cases-10-ways-our-customers-actually-use-it) .


## Sources


- [Hunter.io](https://hunter.io/the-state-of-cold-email) . “The State of Cold Email 2026.” Analysis of 31 million emails sent in 2025.
- [McKinsey](https://www.mckinsey.com/capabilities/growth-marketing-and-sales/our-insights/unlocking-profitable-b2b-growth-through-gen-ai) . “Unlocking Profitable B2B Growth Through Gen AI.”
- [Gartner via Digital Commerce 360](https://www.digitalcommerce360.com/2025/11/28/gartner-ai-agents-15-trillion-in-b2b-purchases-by-2028/) . “AI Agents Will Command $15 Trillion in B2B Purchases by 2028.”
- [TechCrunch](https://techcrunch.com/2026/03/10/agentmail-raises-6m-to-build-an-email-service-for-ai-agents/) . “AgentMail Raises $6M to Build an Email Service for AI Agents.” March 2026.
- [AgentMail](https://www.agentmail.to/blog/openclaw-agent-email-inbox) . “How to Give Your OpenClaw Agent Its Own Email Inbox.”
- [AgentMail](https://www.agentmail.to/blog/openclaw-email-automation-use-cases) . “OpenClaw Email Automation: 7 Real-World Use Cases.”
- [Apollo skill](https://github.com/openclaw/skills/blob/main/skills/jhumanj/apollo/SKILL.md) . OpenClaw Apollo.io REST API integration.
- [Lead-hunter skill](https://playbooks.com/skills/openclaw/skills/lead-hunter) . Automated lead generation and enrichment for AI agents.
- [Klaus pricing](https://klausai.com/pricing) . Self-reported pricing data. Accessed March 2026.
