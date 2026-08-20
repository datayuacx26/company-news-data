---
schema_version: "1.0.0"
document_id: "ee9dde168bfa7ebf7518871a09fc0aa055d34304009eca89ad156698285513c3"
company_key: "yc-stardex"
company: "Stardex"
source_id: "yc-stardex-news-import-5aa4df39d83f"
canonical_url: "https://www.stardex.com/blog/how-can-recruiters-use-claude-ai-to-map-a-competitive-landscape-for-any-company"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-24T02:17:50.986437+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:127cd638a8862772bb813fcbe8417b378cf57fef190bd3141dbbc35ca6d4b64d"
---

# Claude Skill for Recruiters: How to Build a Competitive Landscape Map

Before every intake call, you know the company but you don't know their market well enough to ask the right questions. You spend 30–45 minutes Googling competitors, checking Crunchbase, and piecing together a picture that's still incomplete by the time the call starts.


Today, there's a Claude Skill that does this in 2 minutes for recruiters. Give it a company name, and it maps out the competitive landscape from direct competitors to companies at a similar funding stage, category alternatives, and adjacent players, and produces a clean table of 8–15 companies you can paste straight into Google Sheets.


It's one of the most useful skills in the[Claude Skill for recruiters](https://www.stardex.com/blog/claude-skills-for-recruiters-5-ready-to-use-workflows-skills) toolkit, and it works for intake prep, BD targeting, and showing up smart on client calls.


## **Why Do Recruiters Need Competitive Landscape Claude Skill**


A competitive landscape is a structured map of the companies competing in the same space as your client. For recruiters, it has three immediate uses.


1.


**Intake prep.** Before a kickoff call, knowing who your client competes with tells you where their candidates are likely sitting right now, which companies they benchmark against for comp, and what they mean when they say "we're not like the big players."


2.


**Sourcing.** The competitive landscape *is* your list of sourcing companies. The 8–15 companies Claude surfaces are exactly the places you'd start looking for candidates.


3.


**Business development targeting (BD targeting).** If you're targeting a new vertical or trying to get warm on a prospect, understanding their competitive environment before the first call is the difference between a generic intro and a conversation that actually lands.


##


## **Step-by-Step Guide: How to Map a Competitive Landscape Using Claude AI**


### **Step 1: Connect Exa**


Go to **Customize → Connectors** and connect Exa.


Exa is an AI-native search engine built for machines. It indexes the public web, including company sites, LinkedIn, funding databases, and news sources, and structures the results so Claude can reason over them. In this workflow, Exa runs parallel searches across multiple angles simultaneously: direct competitors, companies at a similar funding stage, category alternatives, and broader market players.


You'll need an[Exa.ai](https://exa.ai/) account to connect it. There's a free tier to test with.


### **Step 2: Create the Claude Skill**


Open a new chat in Claude or Claude Cowork, both work.


**Option 1: Upload the prebuilt Skill (fastest)**


Download our ready-made Skill here:[Competitive Landscape Skill](https://drive.google.com/uc?export=download&id=1nqx97qlHB9FPazvej5ssu5N6W93m5KX7)


Then:


-


Go to[claude.ai/customize/skills](https://claude.ai/customize/skills)


-


Click **+ Create Skill → Upload a Claude skill**


-


Select the competitive landscape Skill


Once uploaded, open it and adjust the number of companies to return, the funding stage filters, or the output format. The default is 8–15 companies, which is enough to be useful without being overwhelming.


For more prebuilt Skills, here are[5 ready-to-use Claude Skills for recruiters](https://www.stardex.com/blog/claude-skills-for-recruiters-5-ready-to-use-workflows-skills) .


**Option 2: Build your own Claude Skill**


Paste a prompt into Claude and ask it to create the Skill from scratch. You can also tweak our[example prompts](https://briefs.stardex.com/External-Example-Prompts-to-create-skills-33d0d858de2d80f1902df5b9ab68d3a1) as a starting point. The core instruction is:


*Given a company name, run parallel searches across direct competitors, companies at a similar funding stage, category alternatives, and adjacent players. Return a curated table of 8–15 companies with name, description, estimated size, funding stage, and HQ. Export as a tab-separated file.*


Save it as /competitive-landscape or whatever name your team prefers.


Once Claude generates the Skill, save it at the end of the conversation. It's automatically added to your Claude Library and should be available in any chat.


### **Step 3: Run It Through Claude**


Type /competitive-landscape in any Claude chat, then give it a company name.


That's the entire prompt. Claude then searches for information about competitors, companies at a similar stage, related categories, and alternatives. It combines results, removes duplicates, and filters for the most relevant content. You can observe the process in real time or leave and check back later. It usually completes in under 2 minutes.


The results appear in a well-organized table in the chat. They are also available as a tab-separated file that you can easily copy into Google Sheets, with no extra formatting required.


## **Pro Tips for Recruiters Using This Skill**


1.


**Run it before every intake call, not after.** The competitive landscape shapes the questions you ask on the call, from sourcing targets to what the client means by "similar stage." Having it beforehand changes the quality of the conversation.


2.


**Use it as your sourcing company list.** The output maps directly onto Step 2 of the calibration research workflow. If you're running both skills, the landscape feeds straight into the sourcing section.


3.


**Paste the TSV output into Google Sheets.** The tab-separated export is formatted to drop straight in. No cleanup needed.


## **Before vs. After Using the Competitive Landscape Claude Skill**


### **Before: Manual Competitive Research**


A recruiter gets a brief for a VP of Product search at a Series B payments company. Before the intake call, they spend 30 minutes: Googling the company, checking Crunchbase for competitors, searching LinkedIn for companies in the same category, and building a rough list in a Google Doc. The list has 6 companies, two of which the client mentions on the call as "not really competitors." They leave the call, not sure they have the right sourcing targets.


### **After: Claude-Assisted Research**


The same recruiter runs /competitive-landscape before the call. In 2 minutes, they have a 12-company table with funding stage, size, and HQ. They bring it to the call, and the client immediately confirms 8 of the 12 as relevant sourcing targets and adds two names that the Skill didn't surface. The sourcing list is built before the call ends.


**Before**


**After**


**Time to competitive map**


30–45 min


~2 min


**Number of companies surfaced**


5–8, from memory


8–15, from parallel web research


**Funding stage and size data**


Manually looked up


Included in output


**Usable as sourcing list**


Needs reformatting


Table ready to use


**Export to Google Sheets**


Manual copy-paste


Tab-separated file, paste directly


**Ready before the intake call**


Rarely


Every time


##


## **Explore More Claude Skills for Recruiters**


If you want more workflows like this, we've put together a full breakdown of the[most useful Claude skills recruiters should use today](https://www.stardex.com/blog/claude-skills-for-recruiters-5-ready-to-use-workflows-skills) , from client meeting prep to sourcing and follow-ups. Each one is ready to use or can be customized based on how your firm operates.


If you have ideas for workflows you'd like automated or need help setting up any of this, reach out to us atsupport@stardex.ai .


This workflow works with any[ATS that connects to Claude](https://www.stardex.com/blog/ats-that-works-with-claude-how-to-use-stardex-with-claude) via the MCP server or API. Here's the full tutorial video showing how we demoed it with[Stardex AI](https://www.stardex.com/) .
