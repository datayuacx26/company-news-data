---
schema_version: "1.0.0"
document_id: "16638f13b81d25ba3e668926e6ffdcc1b3af5cb0a073fc94f9961a18cefa9c74"
company_key: "yc-great-question"
company: "Great Question"
source_id: "yc-great-question-news-import-f3444748def0"
canonical_url: "https://greatquestion.co/blog/introducing-great-question-ai"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-10T16:22:12.446331+00:00"
fetched_at: "2026-08-10T16:22:13.182783+00:00"
content_hash: "sha256:91f54ebfc910f6f60f94f908acac3ae82795b767824004d81915c4e39d50e788"
---

# Great Question AI: the research platform your AI can actually run

Plenty of tools will let an AI read your research. Great Question lets it go and do some.


Here is the short version. The[Great Question MCP](https://greatquestion.co/features/mcp-integration) connects Claude, ChatGPT, Cursor, Gemini, and Copilot to your research operation, with 112+ actions they can take: designing a study, recruiting your own customers, writing the screener, reading the transcripts, and pulling the findings into whatever you are working on. Inside the platform, Great Question AI moderates interviews on its own, clusters themes in your reports, answers questions across a study or your whole repository, and links every quote to the exact moment someone said it.


## An AI that can read your research still cannot do any research


The last two years produced a lot of tools that summarise transcripts. Useful, up to a point. The limit shows up the moment you ask about something nobody has put to your customers yet.


Ask an AI what users think of your new settings flow. If the research does not exist, the best case is that it tells you so. The worse case is that it generalises from three tangentially related interviews and sounds confident doing it. What you needed was for someone to go and ask 30 people this week.


So we built the other half. Your AI can go and get the data.


## Give Claude, ChatGPT, or Cursor the keys


MCP, the Model Context Protocol, is an open standard that lets AI tools securely connect to and operate other software. Most product teams now start their day in one of those tools rather than in a research platform, so we made Great Question work inside them.


Read access is table stakes. What follows is what your AI can change.


### 112+ actions, and most of them write


- **Design studies.** Create interviews, surveys, and prototype tests, with incentives, participation caps, screeners, skip logic, and language settings.
- **Manage your panel.** Create and update candidate profiles, custom attributes, and segments, with consent records and GDPR deletion built in.
- **Recruit.** Shortlist candidates from your panel onto any study and send screener invitations, with ineligible candidates skipped automatically.
- **Schedule.** Set availability, booking limits, notice periods, and timezones, and add moderators and observers to sessions.
- **Analyze.** Read speaker-attributed transcripts, highlights, screener match scores, survey answers, and prototype results including completion rate, top paths, and per-screen drop-off.
- **Synthesize.** Pull findings, stories, and curated highlight reels, and export insights as a PDF.


### One prompt, one fielded study


Which means this is now a single request:


> Recruit 30 weekly-active PMs for an interview about our new settings flow. Screen out anyone at a company under 100 people.


Claude designs the study, writes the screener, shortlists candidates from your panel, sends the invitations, and sets the booking window. You review it before anything goes out. Two days of setup becomes about ten minutes of reviewing what your AI proposed.


It works at the other end of a project too. Ask it to find your interviews about payment friction and help you write the problem statement for a PRD, and you get real customer language in the doc you already have open, instead of hunting for a quote you half-remember from March.


### Your team's method travels with it


The MCP also serves skills, which are written instructions for how research gets done at your company. The AI loads them before it acts. Two are available today, covering screener creation and participant recruiting, so the AI structures qualification questions properly, filters out fraudulent respondents, and does not message the same candidates three times in a fortnight.


That is what makes this safe to open up to more people. A PM running their own study through Claude inherits your screener standards and your messaging rules by default, instead of inventing both from scratch at 11pm.


### Setup is three steps


Add the server, authorize, start working:


` claude mcp add --transport http great-question https://greatquestion.co/api/mcp/v1`


Your browser opens to Great Question, you review the permissions, and you click authorize. Full instructions live in the[MCP setup docs](https://greatquestion.co/support/integrations/mcp) .


### Your permissions come with you


The connection uses OAuth 2.0, so your credentials never touch the AI tool directly. Your existing Great Question role permissions apply automatically, which means the AI sees exactly what you are cleared to see. PII is redacted by default, and Great Question is SOC 2 Type II certified with complete data separation between accounts. More on the[security and governance](https://greatquestion.co/features/security) pages.


This is the part worth being fussy about. If an AI tool can reach every customer conversation your company has ever recorded, regardless of who is asking, you have handed your team a compliance incident waiting to happen.


## The AI inside the platform


All of the above runs on the AI already working inside Great Question. It is there when you are working in the platform directly, too.


### AI runs the interview


You write the study, and AI Moderation runs the session. It asks your questions, follows up when an answer is worth pushing on, probes for the reason behind an opinion, and keeps the conversation on track.


The practical effect is reach. You get closer to the depth of an interview at closer to the sample size of a survey, without booking 60 slots on anyone's calendar. And because the same moderator ran every session, you are not quietly correcting for who happened to be in the room. It sits alongside the[moderated and unmoderated methods](https://greatquestion.co/features/methods) already in the platform, and the sessions land in the same repository as everything else.


### Themes get clustered in your report


Great Question AI groups responses into themes inside your report, ranked by how many people expressed each one, with the response count shown against every theme. Click a theme and you get the evidence behind it.


So you skip the part where you export quotes into a sticky-note tool and rebuild the affinity map by hand. You start from a clustered view and do the work that needs you: renaming themes, splitting the ones that got lumped together, and binning the cluster that is really three people using the same word for different problems. You still decide what any of it means.


### Ask across one study or all of them


Ask AI queries a full study, up to 50 hours of interview data, in seconds. Ask for a specific quote, a summary, an answer to your research question, or a report in whatever shape you need it.


You can also ask across the whole[research repository](https://greatquestion.co/features/research-repository) , which covers the question researchers get asked most: has anyone here already looked into this? Answering that in a minute rather than a week is usually what decides whether old research gets reused or quietly repeated.


### The report is drafted before you sit down to write it


After every interview, AI-Assisted Synthesis generates a summary, chapters, highlights, and tags. Use it as the first draft of your report, or as a fast way to get up to speed on a study a colleague ran six months ago.


> The automated summaries and chapters perfectly complement the already excellent transcripts. They save me time when checking how an interview went, or synthesizing my own studies. Olivier Thereaux, Director of Product Research, Dashlane


### Every quote traceable in one click


Every insight and quote Great Question AI surfaces links to the exact turn in the transcript it came from. Click the citation and you land on the moment someone said it.


Anyone can generate a confident-sounding theme from a pile of transcripts. The question your stakeholders will ask, and should ask, is who said that. Citations mean you answer it in two seconds instead of defending a summary you cannot trace back.


## Why this only works on one platform


Most teams come to Great Question to stop running research across a dozen disconnected tools. ServiceNow consolidated from 15 tools to 7 after moving its research onto one platform.


That consolidation is what makes any of the AI worth having. Theme clustering, Ask AI, cited insights, and every MCP action work across everything your team has ever run, because the recruitment, the sessions, the surveys, and the repository already sit in one place. An AI pointed at research spread across six tools and a shared drive will give you a confident answer built on whichever fraction of it happened to be reachable.


## What we do not do with your data


PII is masked before anything reaches a model, and your data is never used by third parties for training. When the AI produces a quote, we locate the matching section of the transcript and link it, so you can confirm it is verbatim.


We are also straight about the limits. LLMs make mistakes, which is why every response is cited and every quote is traceable. Check the work before you act on it. For more on how we think about this, see our take on[AI in research](https://greatquestion.co/blog/democratizing-ai-research) .


## Frequently asked questions


### What can Great Question's AI do?


Two things. Through MCP, it gives Claude, ChatGPT, Cursor, Gemini, and Copilot 112+ actions across your research operation, from designing and fielding a study to reading transcripts and exporting findings. Inside the platform, it moderates interviews, clusters themes in reports, answers questions across a study or your entire repository, generates summaries, chapters, highlights, and tags after every session, and links every quote to its source transcript.


### What is MCP, and why does it matter for research?


MCP is an open standard that lets AI tools securely operate other software. For research, it means the AI tool your team already uses can design a study, recruit your own customers, read the transcripts, and pull the findings into a PRD, without anyone switching tools or copying quotes by hand.


### Which AI tools does Great Question connect to?


Claude and Claude Code, ChatGPT, Cursor, Gemini, and Microsoft Copilot.


### Can AI run my user interviews?


Yes. AI Moderation runs the session, asks your questions, and follows up in real time based on what the participant says. It is built for studies where you want conversational depth at a sample size that would be impractical to moderate yourself.


### Can I trust what the AI tells me?


Every quote and insight links to the exact turn in the transcript it came from, so you can verify it in one click. We instruct the AI to work from the transcripts we supply rather than its own training knowledge, which makes invention rare. It is still worth checking before you act on it.


### Is my research data used to train AI models?


No. PII is masked before data reaches any model, and we have prohibited the use of data sent to third-party providers for model training.


## Put it to work


[Connect Great Question to your AI tools](https://greatquestion.co/features/mcp-integration) , or[see what the AI does inside the platform](https://greatquestion.co/features/ai) , and run it on your next study.


‍
