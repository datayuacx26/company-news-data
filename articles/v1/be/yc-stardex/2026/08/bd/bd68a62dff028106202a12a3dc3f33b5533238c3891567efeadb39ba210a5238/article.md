---
schema_version: "1.0.0"
document_id: "bd68a62dff028106202a12a3dc3f33b5533238c3891567efeadb39ba210a5238"
company_key: "yc-stardex"
company: "Stardex"
source_id: "yc-stardex-news-import-5aa4df39d83f"
canonical_url: "https://www.stardex.com/blog/how-claude-code-changed-recruiting"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T09:28:07.012011+00:00"
fetched_at: "2026-08-19T09:28:08.683167+00:00"
content_hash: "sha256:dea332505fae56ffc7480c025e9c00a2aa9b860480daa9bf5ee0c2f485263593"
---

# How Claude Code Changed Recruiting

AI has been in recruiting for a while now.


[Recruiters use ChatGPT](https://www.stardex.com/blog/ai-and-chatgpt-prompts-for-recruiters-practical-examples) to write outreach, summarize resumes, come up with interview questions. Helpful, but it doesn't change how the work gets done. You still do the task, you just have help writing it.


Claude Code is a different thing. Not because the model is smarter. Because it writes and runs code on your own files.


That sounds like a developer thing. It isn't. For a long time, if you wanted to pull every candidate you haven't contacted in six months and check them against your client list, you had two options. Find someone technical, or do it by hand in Excel over a weekend. There was no middle.


Now you type what you want in plain English and it writes the script. That's the change. Not AI doing recruiting. Just the coding requirement going away.


One thing worth saying before the examples. It's still code, and code does exactly what it was told, fast, across four thousand records. So work on an export instead of your live database, ask it to show you what it plans to change before it changes anything, and open a few records yourself to check. Takes five minutes and it's the difference between trying this and being scared of it.


Here's what we're seeing firms actually do with it.


## Cleaning up the database


Most firms know their database is dirty. Same person in there three times, titles all over the place, a lot of dead emails.


The numbers back it up. Contact data decays about 2.1% a month, or 22.5% a year, based on the MarketingSherpa research HubSpot uses. Recruiting is worse than that because your whole database is people who change jobs for a living. On duplicates, most databases run 10% to 30%, and under 5% is what a well-run one looks like. That usually takes tooling and someone who owns it. Most firms have neither.[Datamagnet](https://www.datamagnet.co/post/b2b-data-decay-accuracy-benchmarks-2026/)


So people are just writing the cleanup themselves now.


**Dedupe.** The thing to know is don't match on name, or you'll merge two different Michael Chens and lose a relationship. Go in order: LinkedIn URL first since it's the only unique ID in recruiting data, then email, then name plus company as a fallback. Sort by confidence so the obvious ones merge and the murky ones go in a file you look at.


The part people miss is what survives the merge. A guy who built a job tracking system with Claude Code ran into this and wrote up the fix. His rule keeps the row marked as applied when two records match, because just keeping the first row could erase the fact that he'd actually applied. Same thing in recruiting. If one duplicate has a screen note from 2024 and the other is a blank LinkedIn import, the blank one shouldn't win just because it's newer. You have to say that out loud or the script won't know.


**Titles and companies.** VP Eng, VP of Engineering, and V.P., Engineering are one role and your filters treat them as three. Meta and Facebook are one company. Have it group the variants, show you the groups, pick the right version for each. Then keep the mapping file, so next month's import runs through it on the way in.


## Moving data around


**Sheets into the system.** Everyone has longlists sitting in Google Sheets that never made it into the ATS. Built for a search, search closed, 300 qualified people stuck in a tab. Read the sheet, check each row against what's already in the system so you don't recreate duplicates, add what's new, tag it to the search.


**And back out.** Pull the pipeline for a search, format it the way that client likes it, drop it in a sheet. Monday morning, without you doing it.


**The report your ATS won't give you.** Time in stage by recruiter. Candidates sitting 30 days with nothing logged. Clients you haven't touched in 90 days. Every system has one report it can't do and you don't need your vendor to build it. Export the data, describe what you want, save it as a script, run it every Friday.


The first time is usually rough. Blank owner fields on hundreds of records, jobs with no candidates attached, a quarter with no client activity logged. Better to know.


## Sourcing and everything after


People who've been at this a few months aren't cleaning data anymore.


Sourcing is the clearest one. It used to mean writing search strings, testing title variations, fixing filters, then doing it again for the next role. One[recruiter](https://www.reddit.com/r/RecruitmentAgencies/comments/1s4imfh/claude_codeopenclaw_use_cases_in_recruiting/) described automating it:


> "I created a sourcing strategy, sub agent and then a sourcing subagent. It creates a sourcing strategy and then hands it off to the sourcing sub agent and brings back candidates for a search that I'm working on. This was a little more involved and I built it in OpenClaw using Claude Code."


Others[build around tools they already pay for](https://www.reddit.com/r/RecruitmentAgencies/comments/1s4imfh/claude_codeopenclaw_use_cases_in_recruiting/?utm_source=chatgpt.com) :


> "Basically, when you publish a job in your DB, you get an email with the most suitable candidates from your ATS."


Another[runs Claude Code across Google Workspace](https://www.reddit.com/r/RecruitmentAgencies/comments/1ryqkmd/comment/obok3ky/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) , enrichment tools, and a CRM:


> "Claude can directly make you the spreadsheets... I push it to HubSpot or Attio and take it from there."


Some went after research, which has always been limited by time not ideas. One[set up a daily brief using Claude Code](https://www.reddit.com/r/RecruitmentAgencies/s/TroqU4g2Hz) :


> "You wake up at 7 AM... important email you need to look at, your schedule/meetings... industry news, mergers and acquisitions in your industry, even new job openings at potential clients."


The same idea can be applied to candidate management. By connecting Claude to your ATS, you can receive a prioritized morning brief covering candidates who need follow-up, stalled searches, upcoming offers, and other pipeline risks.


We put together a[step-by-step guide to automating a daily candidate pipeline brief with Claude](https://www.stardex.com/blog/how-to-use-claude-ai-to-automate-a-daily-candidate-pipeline-brief) .


A[recruiting leader described automating the routine layer of the job](https://www.linkedin.com/posts/juanjo-moreno-2188274a_novarecruiter-ugcPost-7467476126630559745-Sddg/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAEOOg14BvNE4fJp7d13xtUxmXGqp3-4gXQ8) :


> "Sourcing. Pipelines. Outreach. Follow-ups. Scheduling. The work that quietly eats every recruiter's week."


They said it gave each recruiter back more than ten hours a week. A Claude Code workspace, reusable skills, tools connected through MCPs and APIs, routines on a schedule. That connection piece is why we built the Stardex MCP server, since most of this falls apart if the data has to leave the system by copy-paste.


## What it adds up to


Recruiters aren't turning into engineers. Nobody's learning data structures.


But they're not just users of software anymore either. The coding requirement went away. What's left is knowing what you want and checking the work.


The split isn't between recruiters who use AI and recruiters who don't. Everyone uses AI now. It's between recruiters whose knowledge sits in their head and recruiters whose knowledge sits in something that runs without them.


Start with duplicates. It's an afternoon, you're working on a copy so the risk is low, and it teaches you the rest.
