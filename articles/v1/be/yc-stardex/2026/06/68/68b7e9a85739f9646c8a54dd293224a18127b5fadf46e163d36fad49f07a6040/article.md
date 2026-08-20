---
schema_version: "1.0.0"
document_id: "68b7e9a85739f9646c8a54dd293224a18127b5fadf46e163d36fad49f07a6040"
company_key: "yc-stardex"
company: "Stardex"
source_id: "yc-stardex-news-import-5aa4df39d83f"
canonical_url: "https://www.stardex.com/blog/how-recruiters-write-client-ready-candidate-write-ups-using-claude-ai"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-24T02:17:50.986437+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:dc14e030c65036d3ca9e8a16fc93eb0b06c9144736b9ae8f7cdc7d93464caded"
---

# Claude Skill for Recruiters: Writing Client-Ready Candidate Write-Ups

Every recruiter knows the challenge of documenting candidate details after a thorough screening. Documenting a candidate's career history, previous companies, fit for the role, and compensation can take 15 to 20 minutes per candidate, and this time adds up quickly.


This task is hard to delegate and easy to delay, leaving candidates waiting while write-ups pile up. Fortunately, Claude AI can now assist by creating a first draft of the write-up using a purpose-built Claude Skill. Just provide the candidate's name or LinkedIn URL, the role they're considered for, and the client company. With this Claude Skill, Claude can gather information and produces a formatted write-up ready for review in as fast as 30 seconds. You can then make a few changes, and it's done.


Here's exactly how recruiters set it up using Claude AI.


## **Why Recruiters Need a Candidate Write-Up Claude Skill**


The /candidate-writeup Claude Skill takes three inputs:


-


The candidate's name or LinkedIn URL


-


The role they're being considered for


-


The client company


From there, it uses Exa to research the candidate across multiple sources: career history, recent activity, the funding stage and scale of their prior companies, any notable exits or milestones, and compensation benchmarks for the role and market. The output is a structured write-up formatted for client presentation.


## **Step-by-Step Guide: How to Create Candidate Write-Ups Using Claude AI**


###


### **Step 1: Connect Exa**


Go to **Customize → Connectors** and connect Exa.


Exa is what powers the candidate research. It searches across LinkedIn, company websites, news sources, and other public data to pull career history, prior company context, and compensation signals. Without it, Claude is working solely from its training data, which won't include current information on most candidates.


You'll need an[Exa.ai](https://exa.ai/) account to connect it. There's a free tier to test with. If you've already connected Exa with Claude, you're already set.


### **Step 2: Connect Your ATS or Notetaker (Optional)**


If you want the Skill to pull from your screen notes automatically rather than researching the candidate purely from public sources, connect your ATS or notetaker.


If you use Metaview, Fireflies, or Stardex notetaker, connect it via **Customize → Connectors** . Once connected, you can prompt the Skill to pull the transcript from a specific call and incorporate your notes into the write-up. This is exactly the workflow one attendee on our live workshop call had already built: screen notes from Metaview, piped into Claude, write-up delivered to Slack, and their ATS automatically.


If you're using an[ATS tool](https://www.stardex.com/) like Stardex that connects to Claude, the Skill can also pull candidate data and stage context directly from your pipeline. Reach out tosupport@stardex.ai for the setup doc.


Both connections are optional as the Skill runs on public research alone if you prefer to keep it simple.


### **Step 3: Create the Claude Skill**


Open a new regular Claude chat or Claude Cowork.


**Option 1: Upload the prebuilt Skill (fastest)**


Download our ready-made Skill here:[Candidate Write-Up Skill](https://drive.google.com/uc?export=download&id=1nqx97qlHB9FPazvej5ssu5N6W93m5KX7)


Then:


-


Go to[claude.ai/customize/skills](https://claude.ai/customize/skills)


-


Click **+ Create Skill → Upload a Claude Skill**


-


Select the candidate write-up Skill


Once uploaded, open it and adjust the output format to match your firm's template. If you have a specific structure for client presentations such as sections you always include, language your clients expect, a preferred length, you may customize the Skill to reflect that. The more it mirrors your actual format, the less editing you'll need to do on each draft.


For more prebuilt Skills, here are[5 ready-to-use Claude Skills for recruiters](https://www.stardex.com/blog/claude-skills-for-recruiters-5-ready-to-use-workflows-skills) .


**Option 2: Build your own Claude Skill**


Paste a prompt into Claude and ask it to create the Skill from scratch. You can also tweak our[example prompts](https://briefs.stardex.com/External-Example-Prompts-to-create-skills-33d0d858de2d80f1902df5b9ab68d3a1) as a starting point. The core instruction is:


*Given a candidate name or LinkedIn URL, the role, and the client company, research the candidate using Exa across career history, prior company context (funding, scale, exits), recent activity, and comp benchmarks. Produce a formatted client-ready write-up.*


Save it as /candidate-writeup or whatever name your team prefers.


Once Claude generates the Skill, save it at the end of the conversation. It's automatically added to your Claude library and it should be available via/in any chat.


### **Step 4: Run It**


Type /candidate-writeup and give Claude three things: the candidate's name or LinkedIn URL, the role they're being considered for, and the client company.


Claude runs multiple Exa searches in parallel, career history, prior employers, company context, comp signals, and assembles the write-up in about 30 seconds. Review it, add your call notes and qualitative read, adjust anything that needs adjusting, and send.


## **Pro Tips for Recruiters Using This Claude Skill**


1.


**Train it on your firm's format.** The default output is clean and usable, but if your clients expect a specific structure or you always use a letterhead template, customize the Skill to match it. You'll do less editing on every draft.


2.


**Add your call notes before you send.** The Claude Skill covers the research. You cover the qualitative read: how the candidate came across, what they said about their search, and why they're genuinely interested in this client. That's the part clients actually use to make decisions.


3.


**Connect your notetaker for a fully automated draft.** If you use Metaview or Fireflies, connecting it means the Skill can pull your screen notes directly and incorporate them into the write-up, no copy-pasting required.


4.


**Use Sonnet for speed, Opus for senior or complex roles.** Sonnet produces solid drafts fast. For C-suite or board-level write-ups where the nuance matters more, Opus is worth the extra time.


## **Before vs. After Using the Candidate Write-Up Skill**


### **Before: 15–20 Minutes Per Candidate**


A recruiter finishes three screens in a day. Each write-up takes 15–20 minutes: pulling up the candidate's LinkedIn profile, cross-referencing their prior companies, researching funding context, finding a comp benchmark, writing a narrative, and formatting it for the client. An hour of administrative work on top of the screens themselves. And if the screens run long, the write-ups get pushed to the next morning, which means candidates sit without feedback, and client presentations get delayed.


### **After: 30-Second draft, a Few Minutes to Finalize**


The same recruiter runs /candidate write-up after each screen. The first draft is ready before they've finished their notes. They add their qualitative read, adjust one section, and it's done, three write-ups in the time it used to take to do one.


**Before**


**After**


**Time per write-up**


15–20 min


30 sec draft, 3–5 min to finalize


**Career history research**


Manual — LinkedIn, Google


Exa research, pulled automatically


**Prior company context**


Looked up separately


Included in output


**Comp benchmarking**


Ad hoc, often skipped


Included by default


**Format consistency**


Varies by recruiter


Consistent every time


**Turnaround to client**


Next morning, if you're fast


Same day, often same hour


##


## **Explore More Claude Skills for Recruiters**


If you want more workflows like this, we've put together a full breakdown of the[most useful Claude skills recruiters should use today](https://www.stardex.com/blog/claude-skills-for-recruiters-5-ready-to-use-workflows-skills) — from client meeting prep to sourcing and follow-ups. Each one is ready to use or can be customized based on how your firm operates.


If you have ideas for workflows you'd like automated or need help setting up any of this, reach out to us atsupport@stardex.ai .


This workflow works with any[ATS that connects to Claude](https://www.stardex.com/blog/ats-that-works-with-claude-how-to-use-stardex-with-claude) via the MCP server or API.


Here's the full tutorial video showing how we demoed it with[Stardex AI](https://www.stardex.com/) .
