---
schema_version: "1.0.0"
document_id: "fb3a629735c396519f2afc52b51c8ad0420f90386db025864f552d78b4976e3c"
company_key: "yc-stardex"
company: "Stardex"
source_id: "yc-stardex-news-import-5aa4df39d83f"
canonical_url: "https://www.stardex.com/blog/how-to-source-candidates-on-claude-(and-add-them-to-your-ats-in-seconds)"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-24T02:17:50.986437+00:00"
fetched_at: "2026-07-28T22:15:53.113233+00:00"
content_hash: "sha256:43056a36debbfd4bf5538c6eaaf4859d6aacafe344fb3aebc84d8f2c4b3e8384"
---

# How to Source Candidates on Claude (And Add Them to Your ATS in Seconds)

Most AI sourcing tools haven't actually changed the workflow. You still open a search tool, fiddle with filters, export a list, switch to your ATS, and manually create records. The AI part is usually just a slightly smarter search bar sitting on top of the same fragmented process.


Here's what actually changes things: connecting a search engine and your ATS directly to Claude through MCP connectors, so you can find people, review them, and add them to your database without ever leaving the conversation. No CSV exports. No tab switching. No copy-pasting LinkedIn URLs one at a time.


It takes about 60 seconds to set up. And it works right now.


## What is Exa (And Why Recruiters Should Know About It)


Exa is an AI-native search engine built for machines to use. It indexes over a billion profiles across the web. What makes it different from LinkedIn Recruiter or any Boolean search tool is simple: you describe who you're looking for in plain English, and it finds them. That's the[power of semantic search in recruiting](https://www.stardex.com/blog/what-is-semantic-search) . Will it be as accurate as Linkedin? Probably not as Linkedin has the best data source. But Exa's vector database can get you pretty far.


No Boolean strings. No nested AND/OR logic. No dropdown filters. You type something like "VP of Sales at PE-backed software companies in healthcare," and Exa returns relevant people with their LinkedIn profiles and available contact information.


Exa recently launched a dedicated "people" search category that goes beyond just LinkedIn. It pulls from personal websites, company pages, and other professional profiles across the web. That matters because the best candidates for executive and senior roles often have a digital footprint that extends well beyond their LinkedIn headline.


For recruiters who have spent years mastering Boolean search, this might feel like cheating. It's not. It's just that the search engine finally understands what you mean instead of making you translate your intent into a query language, and here's[why Boolean search might be going extinct soon](https://www.stardex.com/blog/how-to-use-boolean-search-on-linkedin-recruiter-(and-why-it-s-going-to-go-extinct-soon-)) .


## How it Works: Connect, Search, Add


Before you begin, you’ll need to set up your own Exa account then follow this step-by-step guide:


### Step 1: Connect Exa to Claude


Open Claude, click the connectors menu (the "+" icon at the bottom left of your chat), search for Exa, and add it. Authenticate with your Exa account. That's it. If Exa doesn't show up in your connector list, click "Add connector" and search for it there.


### Step 2: Connect your ATS


Same process. If your ATS has an MCP server ([Stardex ATS](https://www.stardex.com/) does), you add it as a connector and authenticate. If you want to see how this works in practice, here's a breakdown of[how an ATS that works with Claude actually looks](https://www.stardex.com/blog/ats-that-works-with-claude-how-to-use-stardex-with-claude) . Now Claude can talk to both your search engine and your candidate database in the same conversation.


### Step 3: Search


Type what you're looking for. Be as specific as you want. "Find me VP of Sales candidates at PE-backed software companies in healthcare" works. So does "senior product managers who've worked at Series B fintech companies in New York." Claude uses Exa to run the search and returns a list of people, typically with names, titles, companies, and LinkedIn URLs.


### Step 4: Review and Add


This is where it gets interesting. You can look at the list, ask Claude to filter or rank them, and then say something like "add all of these to Stardex" or "add the top five to the VP Sales search." Claude creates the records in your ATS. If you switch over to your ATS, they're already there.


The whole thing, from typing a query to having candidates in your database, takes less than a minute.


## What Difference Does an ATS + Claude Integration Make


On the surface, this looks like a faster way to do the same thing. But the difference is more fundamental than speed.


First, it's composable. You're not locked into a single tool's workflow. You can chain actions together in ways that no single product anticipated. Search for candidates, ask Claude to review their backgrounds against the job spec, filter out anyone who's been at their current company for less than 2 years, and then add the remaining candidates to a specific search in your ATS. All in one conversation.


Second, the recruiter stays in the loop on every judgment call. Claude isn't blasting candidates into your database on its own. You see the list, you decide what to do with it, and you tell Claude how to proceed. The mechanical work disappears, but the decision-making stays with you. That matters a lot when you're running retained searches, and your clients expect a curated shortlist, not a bulk data dump.


Third, you can iterate in real time. If the first search is too broad, you don't start over. You refine. "Actually, narrow that to people who've sold into hospital systems specifically." Claude adjusts and gives you a new list. It's the most natural version of search refinement because you're just having a conversation.


This is what we think about a lot at Stardex. The real unlock in recruiting technology isn't just AI features bolted onto an old database. It's making the entire workflow fluid enough that the tool disappears and you're just doing the work.


## This Works With Any ATS That Has an MCP Server


One thing worth noting: this workflow isn't exclusive to any single ATS. Stardex has an MCP server, and we built it specifically so recruiters could connect Claude (or ChatGPT, or other AI tools) directly to their candidate database. But the pattern works with any ATS that supports MCP.


MCP is an open standard spearheaded by Anthropic. It's a universal plug that lets AI tools connect to software. More ATS and CRM platforms are adding MCP support, and the trend is accelerating. If your current system doesn't have an MCP server yet, it's worth asking your vendor when they plan to add one. Because this is the direction everything is heading: your tools talking to each other through AI, with you orchestrating the workflow in plain language.


The recruiters or talent engineers who figure this out early aren't just saving time; they're also saving money. They're building a fundamentally different kind of practice, one where the technology compounds their judgment instead of just storing their data.


## Ready to Connect Your ATS to Exa AI?


If you want to try this yourself, connect Exa and Stardex (or your ATS of choice) to Claude and run your first search. It takes a minute.


Curious what this looks like inside Stardex?[Book a demo](https://www.stardex.com/book-demo) , and we'll show you how recruiters actually use it every day.
