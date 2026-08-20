---
schema_version: "1.0.0"
document_id: "79eb3e36e12a8e5cb5ddcb8dbea89f5849e6929904866c495d911ebd83e51b51"
company_key: "yc-stardex"
company: "Stardex"
source_id: "yc-stardex-news-import-5aa4df39d83f"
canonical_url: "https://www.stardex.com/blog/how-to-use-claude-ai-to-automate-a-daily-candidate-pipeline-brief"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-24T02:17:50.986437+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:7c298337f84ef8ddaf0866cd912d31a6f96bb050c06838c5c417e7db78e82e69"
---

# Claude Skill for Recruiters: Automating a Daily Candidate Pipeline Brief

Pipeline visibility sounds simple until you're the one piecing it together every morning from check-ins, ATS notes, and team status updates. By the time you have the full picture, the day is already half gone.


The fix is a scheduled Claude Skill that runs every morning in Claude Cowork. It connects to your Applicant Tracking System, checks for candidates with interviews or updates in the last 48 hours, and delivers a prioritized action list to Slack before your day starts: who needs follow-up, who's gone dark, where there's a competing offer or timeline risk.


This requires ATS connectivity, so the setup depends on your system. We demoed it with[Stardex AI](https://www.stardex.com/) , but it applies to any[ATS tool that works with Claude](https://www.stardex.com/blog/ats-that-works-with-claude-how-to-use-stardex-with-claude) via the MCP server or API.


Here's exactly how to set it up in your AI recruitment software.


## **What Is a Daily Candidate Pipeline Brief And Why It Matters for Search Firms**


A pipeline brief is a structured daily summary that answers the questions a good manager would ask in the morning, such as:


1.


**What needs to move today?** Candidates in the final stages, offers pending, clients waiting on feedback.


2.


**What's stalled?** Searches with no activity in 5+ days, candidates who haven't been advanced or declined, and intake calls that never turned into a shortlist.


3.


**What's closing this week?** Offers out, placements expected, retainer milestones due.


Once you automate this, you won't have to ask your team for updates anymore; you'll already have them. You come into every conversation knowing exactly where each search stands, which means your interventions are faster and your team spends less time on status reports in your AI recruitment tool.


Traditionally, this required either a dedicated ops person pulling reports or a weekly pipeline review that was already out of date by Monday morning. With a modern[AI executive search software](https://www.stardex.com/blog/why-stardex-is-the-right-fit-for-your-executive-search-firm) and a scheduled workflow through Claude, it runs every night and lands in Slack before anyone clocks in.


## **Step-by-Step Guide: How to Automate Daily Candidate Pipeline Brief, Using Claude Skill**


###


### **Step 1: Connect Your ATS**


This workflow is built around AI recruitment software data, so the connection is the foundation. Without it, Claude has nothing to pull from.


Go to **Customize → Connectors → Add a custom connector** . Most ATS tools aren't yet in Claude's built-in connector list, so you'll add them manually using an MCP server URL. If you're using Stardex AI, reach out tosupport@stardex.ai , and the team will send you the setup doc with the exact URL and steps. If you're on Bullhorn, Crelate, or Loxo, the same concept applies as long as your recruitment tracking software has an API or MCP server.


Once connected, Claude can query your AI recruitment software data directly. It knows which searches are active, which candidates are at which stage, and when each was last updated, without you having to export anything manually.


### **Step 2: Connect Slack (or Email)**


The brief needs somewhere to land. Slack is the most common delivery method, and you get it in a dedicated channel, a DM, or both. Email works just as well if that's how your team operates.


Go to **Customize → Connectors** and connect Slack. If your team uses Microsoft Teams or prefers email delivery, connect those instead. The brief will be formatted and sent to whichever Slack channel or inbox you specify in the Skill.


### **Step 3: Create the Claude Skill**


Open a new chat in Claude; either regular chat or Cowork will work for this step.


**Option 1: Upload the prebuilt Skill (fastest)**


Download our ready-made Claude Skill here:[Daily Pipeline Brief Claude Skill](https://drive.google.com/uc?export=download&id=1nqx97qlHB9FPazvej5ssu5N6W93m5KX7) .


Then:


-


Go to[claude.ai/customize/skills](https://claude.ai/customize/skills)


-


Click **+ Create Skill → Upload a Claude Skill**


-


Select the daily past candidate interview brief Skill


Once uploaded, open it and adjust the prompt as needed to reflect how your team actually ranks urgency. The Skill is a template; you want it to mirror how your firm works.


For more prebuilt Skills, here are[5 ready-to-use Claude Skills for recruiters](https://www.stardex.com/blog/claude-skills-for-recruiters-5-ready-to-use-workflows-skills) .


**Option 2: Build your own Claude Skill**


Paste a prompt into Claude and ask it to create the Skill from scratch. You can also use and tweak our[example Claude prompts](https://briefs.stardex.com/External-Example-Prompts-to-create-skills-33d0d858de2d80f1902df5b9ab68d3a1) as a starting point. The core instruction is:


*Pull active searches from the ATS → identify what needs action today → flag stalled candidates → surface closing activity → format into a prioritized brief → deliver to Slack.*


Save it as /daily-pipeline-brief or whatever name your team prefers.


Once Claude AI generates the Skill, you'll see an option to save it at the end of the conversation. Click that. The Skill is automatically added to your Claude library and available in any chat via typing /.


### **Step 4: Test It Manually First**


Before scheduling, run the Skill once manually to check the output.


Type /daily-pipeline-brief in a new chat and review what comes back. Check that:


-


Claude is correctly pulling from your ATS tool and seeing the right searches


-


The priority logic reflects how you actually think about urgency


-


The brief format is something your team would read, not skip


If anything feels off, such as formatting not matching your team's workflow, or ATS data is pulling incorrectly, you may fix it in the Skill before you schedule it.


### **Step 5: Schedule It to Run Automatically**


This is where the workflow becomes truly hands-off. Scheduling lives in Claude Cowork, which is Claude's desktop app for automated tasks. Download the Claude desktop app if you haven't already. Cowork only works on the desktop version, not in the browser version.


Once you're in Cowork, go to **Scheduled → New Task** . In the Prompt field, reference the Skill you created: /daily-pipeline-brief. Set it to run every morning at whatever time makes sense for you. Set the delivery destination to your Slack channel or email.


Now the workflow runs on its own every night:


-


ATS data triggers it


-


Claude pulls and prioritizes the pipeline


-


The brief is formatted and delivered to Slack


One scheduled task. No one has to remember to run it, pull a report, or compile a status update. The brief is just there.


## **Pro Tips for Recruiters Who Will Use These Claude Skills**


1.


**Use Cowork for recurring automations, Claude chat for one-off tasks.** The scheduled brief belongs in Cowork. Ad hoc pipeline questions are fine in regular chat with your ATS connected.


2.


**Start with one brief format, then iterate.** Run the default output for a week before customizing. You'll quickly know which sections your team reads and which they skip, which tells you where to put the detail and what to cut.


3.


**Plan accordingly if you're on the free plan.** Scheduled tasks in Cowork require Claude Max ($100/month). Claude Pro ($20/month) covers manual runs and one-off tasks but won't support overnight automation.


##


## **Before vs. After Setting Up the Automated Daily Pipeline Brief**


### **Before: Reactive Pipeline Oversight**


Most managers piece together pipeline status reactively. Monday morning standup surfaces what happened last week. A deal stalls for four days before anyone mentions it. Clients follow up on candidates before the recruiter has a chance. You're managing from behind.


The information exists. It's all your AI recruitment software, but extracting it takes time nobody has.


### **After: Brief-First Mornings**


A manager at a 6-person search firm tests this workflow during a week with 14 active searches. Before, their morning started with a scan of the ATS and a mental assembly of where things stood, about 20 minutes most days, longer when something had moved over the weekend.


After setting up the scheduled brief, they open Slack at 8 AM and find a prioritized list waiting for them. That morning, it flags two searches with no activity in 6+ days and one offer letter that still hasn't been sent. None of those would have surfaced in their usual morning scan routine. Instead, they would have come up later, reactively, after a client or candidate followed up first.


## **Explore More Claude Skills for Recruiters**


If you want more workflows like this, we've put together a full breakdown of the[most useful Claude skills recruiters should use today](https://www.stardex.com/blog/claude-skills-for-recruiters-5-ready-to-use-workflows-skills) , from client meeting prep to sourcing and follow-ups. Each one is ready to use or can be customized based on how your firm operates.


If you have ideas for workflows you'd like automated or need help setting up any of this, reach out to us atsupport@stardex.ai .


This automation applies to any[ATS tool that works with Claude](https://www.stardex.com/blog/ats-that-works-with-claude-how-to-use-stardex-with-claude) via the MCP server or API so here’s the full tutorial video on how we demoed it with[Stardex AI](https://www.stardex.com/) .
