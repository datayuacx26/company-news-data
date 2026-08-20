---
schema_version: "1.0.0"
document_id: "6ce637ffbab6159f62e0ca7316d8172e5e46770dd36853991a3d2ecb24ff7971"
company_key: "yc-stardex"
company: "Stardex"
source_id: "yc-stardex-news-import-5aa4df39d83f"
canonical_url: "https://www.stardex.com/blog/using-claude-to-turn-an-intake-call-transcript-into-a-job-calibration-research-brief"
published_at: "2026-06-05T00:00:00+00:00"
first_seen_at: "2026-07-24T02:17:50.986437+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:b176f0ccdf45616063bebf4c811b0bc6b5eea6ec738c2c56364bd52a21d07f1f"
---

# Claude Skill for Recruiters: Turning an Intake Call Transcript into a Job Calibration Research Brief

[Job calibration](https://www.metaview.ai/resources/blog/talent-calibration) is the process of translating a messy client conversation into a structured definition of the role and then immediately stress-testing that definition against the real talent pool. A good calibration brief is what separates a search that closes in 8 weeks from one that drags on for 6 months because nobody noticed the must-haves were impossible to find together.


Traditionally, this took a senior associate 45 minutes to an hour to produce after every intake call. But today, firms using[modern executive search software](https://www.stardex.com/blog/why-stardex-is-the-right-fit-for-your-executive-search-firm) or AI recruitment software like[Stardex AI](https://www.stardex.com/) can complete this entire job calibration workflow in about 3 minutes with a purpose-built Claude skill.


That's the case for automating calibration, and here's exactly how to turn an intake call transcript into a calibration research brief using Claude Skill.


## **Step-by-Step Guide: How Claude Can Turn an Intake Call Transcript into a Calibration Research Brief in 3 Minutes**


###


### **Step 1: Connect Exa**


Start inside Claude. Go to **Customize → Connectors** and connect Exa.


Exa is an AI-native search engine built for machines. It has indexed a large portion of the public web and structures results so Claude can actually act on them. In this workflow, Exa runs the sourcing company research and the external candidate searches. It pulls from LinkedIn, GitHub, company websites, and other publicly available sources, then surfaces structured results that Claude can reason over.


You'll need an[Exa.ai](https://exa.ai/) account to connect it. There's a free tier to test with. Once you have it, add it as a connector directly in Claude's settings.


###


### **Step 2: Connect Your ATS**


If you also want the Skill to search your AI recruitment software’s database, connect your ATS software to Claude as a custom connector.


Go to **Customize → Connectors → Add a custom connector** . Most ATS platforms aren't yet in Claude's built-in connector list, so you'll need to add them manually. Your ATS should provide an MCP server URL. If you're using Stardex AI, reach out tosupport@stardex.ai , and the team will send you the setup doc with the exact URL and steps.


Once it's connected, Claude can now recognize when a query relates to your ATS software and pull from it without you needing to spell it out.


If you're not connecting an AI recruitment software right now, skip this step entirely. Remove the internal search section from the skill prompt, and the workflow runs cleanly without it.


###


### **Step 3: Create the Claude Skill**


Open a new chat in Claude, either regular chat or Cowork, it doesn't matter for this step.


**Option 1:** Upload a prebuilt skill (fastest)


You can use our ready-made Skill as a starting point.


Download it here:


[https://drive.google.com/uc?export=download&id=1nqx97qlHB9FPazvej5ssu5N6W93m5KX7](https://drive.google.com/uc?export=download&id=1nqx97qlHB9FPazvej5ssu5N6W93m5KX7)


Then:


Go to[claude.ai/customize/skills](http://claude.ai/customize/skills)


Click + Create Skill → Upload a Claude skill


Select the job calibration prep Claude skill


Once uploaded, open it and tweak it.


Adjust the meeting title format to mirror how your firm actually works.


For more prebuilt skills, here are[5 ready-to-use Claude Skills for recruiters](https://www.stardex.com/blog/claude-skills-for-recruiters-5-ready-to-use-workflows-skills) .


**Option 2:** Build your own Claude Skill


If you prefer, you can paste a prompt directly into Claude and ask it to create the Skill from scratch. You may also use and tweak our[prompts](https://briefs.stardex.com/External-Example-Prompts-to-create-skills-33d0d858de2d80f1902df5b9ab68d3a1) to generate the Skill.


Save it as /job-calibration-prep or whatever you prefer to name it.


Once Claude has generated the Skill, you'll see an option to save it at the end of the conversation. Click that. The Skill is automatically added to your Claude library.


Once saved, the Skill is available in any Claude chat. Type /job-calibration-research and you're ready to run it.


### **Step 5: Run It Against a Real Transcript**


Open a new chat, type /job-calibration-research, and paste in your transcript. This can be a Fireflies export, a Zoom auto-transcript, or rough notes. The Skill is built to read messy input.


If you use a notetaker like Metaview, you don't even need to paste the transcript. You can say: *"Review the transcript from my call with \[client name\] from Metaview and create a brief."* Claude will know to pull it from there. The same works for Google Docs if you have Google Drive connected.


When you hit send, Claude pulls the Skill, checks what tools it has available (Exa, your ATS tool if connected), and creates a task list you can follow along in real time. It runs several Exa calls in parallel, a broad search for the profile, then targeted searches at each sourcing company. You can watch the progress in the top-right corner, or leave it running in the background and come back to it later. It typically takes 2–3 minutes, sometimes a bit longer depending on how many API calls it's making.


The output lands as a structured brief in the chat. At that point, you can ask Claude to export it as a PDF and share it with a team member, or share the chat link directly.


## **Pro Tips for Recruiters Who Will Use These Claude Skills**


A few things to know before you set-up this in Claude Skill.


1.


**Use Sonnet for day-to-day tasks, Opus for heavier research.** Opus produces better reasoning for complex searches, but it's slower and burns through usage more quickly. For most calibration briefs, Sonnet is the right default.


2.


**Use Cowork to schedule recurring automations.** Daily briefings, weekly email triage, andautomated follow-up sequences all belong in Cowork. For one-off research tasks like this workflow, regular Claude chat is fine and faster to get started with.


3.


**You don't need Exa for everything.** If your use case is general reasoning or web research, Claude's built-in search handles it. Exa earns its place specifically for company and people research, which is what this workflow is built around. If you're not doing that type of research regularly, skip the connector.


4.


**Plan accordingly if you're on the free plan.** You'll hit usage limits quickly once you're running multi-step research workflows. Claude Pro ($20/month) is the minimum for regular use. If you're scheduling recurring tasks in Cowork, Claude Max ($100/month) is worth it.


## **Before vs. After Using the Job Calibration Research Claude Skill**


### **Before: Manual Calibration Workflow**


After an intake call, recruiters usually spend 45–90 minutes piecing everything together manually. They reread transcripts, figure out what the real must-haves are, hunt down comp details buried in the conversation, build sourcing lists from memory and LinkedIn, then manually search for candidates across LinkedIn Recruiter and candidate tracking softwares. The result depends heavily on who's doing the work, and important details often get missed.


### **After: Skill-Assisted Workflow**


You paste the transcript into Cowork and run /job-calibration-research.


In 2–3 minutes, you get a structured calibration brief with:


-


prioritized must-haves


-


sourcing company targets


-


ranked external candidates with fit assessments


-


internal ATS tool matches (if connected)


-


notes on whether the search is realistically executable


The workflow becomes faster and less dependent on manual synthesis. The goal isn't to replace recruiter judgment but to remove repetitive prep work so recruiters can focus on decision-making and search quality.


## **Explore More Claude Skills for Recruiters**


If you want more workflows like this, we've put together a full breakdown of the[most useful Claude skills recruiters should use today](https://www.stardex.com/blog/claude-skills-for-recruiters-5-ready-to-use-workflows-skills) — from client meeting prep to sourcing and follow-ups. Each one is ready to use or can be customized based on how your firm operates.


If you have ideas for workflows you'd want automated or want help setting any of this up, reach out to us atsupport@stardex.ai


This workflow works with any[ATS that connects to Claude](https://www.stardex.com/blog/ats-that-works-with-claude-how-to-use-stardex-with-claude) via MCP server or API. Here's the full tutorial showing how we demoed it with[Stardex AI](https://www.stardex.com/) if you want to set it up with your ATS.
