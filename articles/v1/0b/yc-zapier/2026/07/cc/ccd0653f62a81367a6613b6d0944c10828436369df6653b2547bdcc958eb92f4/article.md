---
schema_version: "1.0.0"
document_id: "ccd0653f62a81367a6613b6d0944c10828436369df6653b2547bdcc958eb92f4"
company_key: "yc-zapier"
company: "Zapier"
source_id: "yc-zapier-rss-44f77ec59be6"
canonical_url: "https://zapier.com/blog/damian-guzman-fte-legal-mcp"
published_at: "2026-07-29T04:00:00+00:00"
first_seen_at: "2026-07-30T14:33:44.013319+00:00"
fetched_at: "2026-07-30T14:33:46.336294+00:00"
content_hash: "sha256:1acf452d7147cd3dd21714c72acacd159e58883c2aec8e934b4b828cbfdfdb97"
---

# What happens to a lawyer's business model when AI makes him 5x faster

Most automation stories are about saving time. This one is about what happens after you save it.


[Damian Guzman](https://zapier.com/blog/damian-guzman-fte-legal-mcp/linkedin.com/in/damianguzman) runs[FTE Legal](https://ftelegal.com/) , a Social Purpose Corporation, in Oakland, California, that represents small businesses and fintechs, often at rates below market and halved again for early-stage clients who can't yet pay full freight. He has about 30 hours of self-imposed billable capacity in a given week, which means every hour spent on admin is an hour he isn't serving a client.


Since last summer, he has been rebuilding that math with Claude, and since April with[Zapier MCP](https://zapier.com/mcp) reaching into his tools. The result is a solo practice that produces roughly five times as much output as it did just a few months ago. It also surfaced a problem big enough that he is now building a company around it: once AI can do the work of a legal team, how does anyone on the buying side know what that work is actually worth? That second question is where Zapier MCP stops being a personal-productivity trick and starts looking like infrastructure the legal profession can build on.


## The workflow that runs before he opens his inbox


The center of Damian's practice is a scan that runs every hour. Claude checks his[Gmail](https://zapier.com/apps/gmail/integrations) for anything actionable from clients, logs the task in[Clio](https://zapier.com/apps/clio/integrations) , his system of record, and then goes to work on it directly: drafting an NDA, doing a first-pass review of a contract, marking up language in track changes. By the time Damian opens the request, roughly half of it is already done.


That's the flagship workflow, but it's one piece of a much larger setup. A daily scan replaces Google Meet links with Zoom across his next two weeks of calendar, skipping anything with a physical address attached. A 48-hour buffer blocks time ahead of client work so he's never walking into a meeting cold. Regulatory alerts land daily, with a weekly rollup by email and Slack. A docket monitor tracks two matters that are personal to him. For one privacy-sensitive client, a separate automation strips party names out of Clio's billing narratives before AI ever touches the time entries.


None of this lives in the Zap editor. Damian connected Zapier MCP to Claude and let Claude decide what to do with it, the same pattern that shows up across the MCP builder cohort at Zapier, but taken further than most.


The account has 12,322 MCP tasks since he joined on April 3, active on 100 of the last roughly 104 days, and not one traditional Zap. Clio, Gmail, Google Calendar, Zoom, Docs, Drive, Sheets, Xero, and GitHub are all reached through the same protocol, called by an agent, never configured by hand in a builder UI. In the most recent 90 days alone, that's 10,972 tasks, up from 1,334 in the prior comparable stretch, a jump that lines up with how much of his practice he's handed to Claude since April.


## What Zapier MCP does


Claude is the one deciding what a client email means and what to draft in response. Zapier MCP is what lets those decisions land somewhere real: a task in Clio, a calendar block, a drafted document in the right folder.


Before MCP, connecting an AI agent to a specific tool meant either a native integration that may or may not exist, or building something custom. Damian's a case study in that gap: Claude's native Gmail connector couldn't handle scheduled sends, so he moved that piece to Zapier.


His GitHub connector broke on an OAuth conflict while a support ticket sat open with Anthropic, so Zapier substituted in without him waiting on anyone else's timeline.


The pattern he described is simple: start with what Claude connects to natively, and when that falls short, Zapier MCP closes the gap without a rebuild.


## The problem good automation creates


Damian estimates he's now producing the same work in 20% of the time it used to take, 5x faster. That's the kind of number a lot of automation content stops at. Damian doesn't stop there, because he bills by the hour.


If a task that used to take five hours now takes one, and he's still charging by the hour, his revenue on that piece of work just dropped by 80%. He's weighing the obvious responses: move to flat-rate pricing, take on more clients to fill the freed-up time, lean harder into the consulting and teaching work he's already started. Each of which has a cost. Flat-rate pricing means pricing risk he doesn't currently carry. Finding 5 times more clients isn't as easy as it sounds. And AI is making in-house teams, a significant proportion of his work, get more work done in-house, and they need his services less as a result.


He sees the legal industry splitting along this exact line: firms that have invested the time to make AI reliable for legal work, and firms that haven't, some of which have banned it outright. Damian thinks that ban is a mistake with a long tail: the firms sitting it out now will have the furthest to travel later, no matter how good the tools get.


He's running a free[8-week AI automation seminar](https://us06web.zoom.us/webinar/register/WN_KBy3ZWcGSHehWXncNzo-lA#/registration) primarily aimed at other lawyers, including direct competitors, on the bet that the market gets bigger for everyone who's competent at this before it gets smaller for the people who aren't. That free training is a rescue operation for lawyers who don't want to get left behind, a natural extension of his practice's social purpose mission.


The harder question he raised, and didn't pretend to have an answer to: if AI is doing the research memos, first-draft contracts, and NDA markups that used to be how junior lawyers learned the job, where does the next generation of lawyers get trained?


That's not a Zapier question. But it's the honest version of what "5x productivity" costs an industry that trains people by having them do the slow version first.


## The consultancy that came out of the problem


The freed-up time went somewhere. With most of the routine work done or in progress before he sits down, Damian now has room to help others fix the same problem from the other side. He launched Ergane, a legal efficiency consultancy that helps founders, CXOs, and other decision-makers make their legal operations work more efficiently. Where he rebuilt his own practice around Claude and Zapier MCP, he now advises teams on doing the same: which work to automate, which to keep human, and when to buy a legal-tech platform versus build the workflow in-house.


Ergane is kept separate from both the law firm and the product. The free 8-week seminar for lawyers stays under FTE Legal, part of the firm's social-purpose mission. Ergane is the paid advisory for the organizations on the buying side. The connective tissue is the same one running through the rest of this piece: the Zapier MCP workflows Damian built for himself are exactly what he now helps other teams stand up.


## The product that came out of the problem


The 5x number created a question Damian couldn't answer for his own clients, let alone the industry: once an AI-augmented lawyer produces in one hour what used to take five, how does the person paying the bill know whether they're getting a fair deal? He's building the answer as a separate company,[Ariadni LLC](https://www.ariadnilegal.ai/) , kept at arm's length from the law firm.


Ariadni is counsel-oversight software, and its pitch is blunt: know what your lawyer is worth. It lets a legal-services buyer monitor its outside counsel across a live engagement and produces a continuous value score for each lawyer, firm, and matter, built from four signals: responsiveness, who did the work, work quality, and value. It's in private pilot now.


The part that matters beyond one law firm is that Zapier MCP is wired into the product itself. Ariadni uses the same protocol Damian runs in his own practice, but as a feature its users switch on rather than something they build:


-


When Ariadni scores a lawyer, firm, or matter, it can send that event to the user's own Zapier webhook, so a change in score lands wherever the client already works.


-


It reads counsel responsiveness from the Slack and Notion activity a client routes in through Zapier, turning communication metadata into a service signal without reading message content.


-


When it catches a commitment from counsel, a line like "I'll have the revised draft to you by Friday," it uses Zapier MCP to turn that promise into a dated task in whatever tool the client runs.


That last one is the through-line of this whole piece. The promise-to-deadline move Damian built by hand for himself is now a button other people press, and Zapier MCP is what makes it portable. The same connective layer that lets one solo lawyer punch through a 30-hour workweek is what lets a legal-tech product reach into a buyer's stack without a custom integration per tool.


## What his practice looks like now


Damian isn't running a bigger firm. He's running the same solo practice with the same self-imposed 30-hour ceiling. Most of the work that used to eat into that ceiling now happens before he sits down. The time that frees up is going into things a busier version of him couldn't have made room for: teaching other lawyers how to do this, including those he technically competes with; helping founders and executives make their legal functions more efficient through Ergane, and building Ariadni.


---


*Damian Guzman is the founder of FTE Legal, a Social Purpose Corporation, in Oakland, of Ergane LLC, his consultancy helping founders and executives make their legal functions more efficient, and of Ariadni LLC, a separate counsel-oversight software company. He builds with Claude and connects both his practice and his product through Zapier MCP.*
