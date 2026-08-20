---
schema_version: "1.0.0"
document_id: "ad0e4d19ed030625c2ea49bf8710703db89b98746cca1165ed2f324590e3557a"
company_key: "yc-operator-labs"
company: "Operator Labs"
source_id: "yc-operator-labs-news-import-4e5219b8eb88"
canonical_url: "https://operator.io/blog/cross-app-briefing-with-your-agent"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-24T07:49:23.882568+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:35f93d7fc4f95b74e3e00a6d6458ec27ffa2ce230ce50e4fd7c4fcc524709368"
---

# A morning briefing that reads across all your apps

The first twenty minutes of a workday usually go to reconstructing what is on it. You open the calendar to see the meetings, the inbox to find what came in overnight, Slack to catch the threads you were tagged in, and whatever holds your tasks to remember what was already due. Each of those lives in a different app, and opening them one after another is how the workday starts, before the work itself does.


A single agent that can read all of them folds those four checks into one message.[Composio](https://composio.dev/) is what gives your[Operator.io](https://operator.io/) agent that reach: you connect it once and the agent can read your Google Calendar, your Gmail, your Slack, and your project tool through the same connection, with no separate setup per app. From there, one prompt turns it into a briefing that arrives before you do.


## How it works


You send the briefing prompt, and the agent assembles the same roundup every morning from your connected apps.


- It reads your calendar for the day and lists the meetings with their times and who is on each one.
- It scans your inbox for the few overnight messages that need a reply, and skips the noise.
- It checks Slack for the messages and mentions waiting on you.
- It pulls the tasks that are due or overdue from your project tool.
- It groups all of that into one message you can read in under a minute, with links straight to anything you will want to open.


The briefing is read only by design. The agent looks across your apps and reports back, and it does not move a meeting, archive a message, or close a task unless you ask it to in a later message.


## Connect Composio first


This prompt needs Composio connected to your agent, since that is what gives it access to the apps it reads. Open the MCPs page in your Operator dashboard, add Composio, and sign in to your own Composio account through the window that opens. The full walkthrough is in[the Composio setup guide](https://operator.io/blog/automate-anything-with-composio) , and the[prompts library](https://operator.io/prompts) will offer to connect it inline the first time you send a prompt that needs it.


Composio's[free tier](https://composio.dev/pricing) gives you twenty thousand tool calls a month with no card on file, which covers a daily briefing with plenty of room to spare. Some toolkits count as[premium calls](https://docs.composio.dev/toolkits/premium-tools) at roughly three times the standard rate, but a morning read across Gmail, Calendar, Slack, and a task app stays well inside the free allowance.


Each app inside Composio authorizes the first time the agent reaches for it, so the first morning you may get a link or two to approve your Google and Slack accounts. After that the agent just reads them. Composio publishes a[toolkit directory](https://composio.dev/toolkits) if you want to see what else you can fold in later, from Linear and Asana to HubSpot and Notion.


Each connection is a normal OAuth grant to the official APIs those products expose.[Google Calendar](https://developers.google.com/calendar/api/guides/overview) returns events, attendees, and conference links.[Gmail](https://developers.google.com/gmail/api/guides) exposes message metadata, labels, and thread IDs so the agent can surface unread mail without opening every newsletter.[Slack's Web API](https://api.slack.com/start/overview) covers channels, direct messages, and` @` mentions. Your task tool, whether that is Todoist, Asana, Linear, or Monday, has its own REST surface that Composio wraps into the same tool call pattern.


You approve scoped access through OAuth the same way you would for any other integration, with tokens stored under your Composio account. The brief only ever reflects what those scopes return, so it reads the accounts you connected and nothing past them. Connect one Slack workspace and it sees that one; leave an inbox off and it never shows up in the morning message.


## The prompt


This is the instruction the agent acts on:


```text
Be my morning briefing, working across the apps I connected through
Composio. Each morning before I start, pull one short brief and send it to
me here: today's calendar with the times and who is in each meeting, the few
overnight emails that actually need a reply, any messages or mentions
waiting for me in Slack, and the tasks that are due or overdue in my project
tool. Group it so I can read it in under a minute, and link straight to
anything I will want to open. Keep it read only, so do not change anything
in those apps unless I ask. Before you start, ask me which apps to include,
what time to send the brief and on which days, and whose email and messages
matter most so you can put those at the top.


```


The same prompt is saved in the[prompts library](https://operator.io/prompts) , so you can send it to your agent without retyping a word.


## Using it day to day


The brief lands at the time you picked, and you read it the way you would read a note from an assistant who already went through everything. Three meetings, the second one moved, two emails worth answering, a thread in Slack where someone is waiting, and the report that was due yesterday still sitting open. Because it comes to you in the channel you already have open, you can answer in the same place: reply to draft the email, ask it to move the meeting it flagged, or tell it to nudge you again about the report at noon.


### Where the brief lands


The brief gathers your calendar, the mail worth a reply, the Slack threads waiting on you, and your open tasks into one place, which is a fair amount of your day sitting in a single message. If the channel you use with the agent is a one to one chat, that is the natural home for it. If you share a channel with anyone else, point the brief at a direct message so nobody is reading your inbox over your shoulder.


### How it decides what needs a reply


The agent reads the sender, the subject, and enough of the body to judge whether something is actually waiting on you, then leans on the priority senders you named to order the list. It gets this right most mornings and wrong on some. A newsletter with an urgent subject line can slip into the list, and an important note from someone you never flagged can sit lower than it should.


So read the brief as a fast pass over the morning. For anything that cannot wait, open the thread in Gmail or Slack before you act on the one line summary, and tell the agent when it misjudges something so the next brief sorts closer to the way you would.


### Tuning what leads


What you put at the top is yours to set. Tell it your manager's email and your two biggest clients always lead, and the brief sorts itself so the things that cannot slip are the first lines you read. Over a week you can also ask it to look back, which meetings you keep rescheduling or which threads you keep meaning to answer, since it has been reading the same sources every day and can see the pattern.


Weekends and travel days reward a little setup too. Ask for a lighter brief on Saturdays that skips Slack entirely, or a Monday version that adds a glance at the full week ahead instead of only today. The prompt's setup questions cover timing and priority senders, so add one sentence about which days need which shape and the automation follows it.


## What else is trying to solve this


Morning briefings have become a product category of their own. Slack's[Today tab](https://slack.com/blog/news/today-daily-briefing) pulls calendar, tasks, and notifications into one view inside Slack for Business Plus and Enterprise customers, with Pro plans listed as coming later. Dedicated apps like Fazm and Slime connect calendar, email, and chat into a single notification. Automation marketplaces such as Apify ship no code actors that fetch Gmail, Calendar, and Slack, then post a formatted summary to a channel.


Those tools each solve one delivery surface. Slack Today keeps you inside Slack. Mobile briefing apps keep you inside their notification. The Operator version keeps you inside whatever channel you already use with OpenClaw, Telegram or the web chat, while Composio supplies the cross app reads.


You can also extend the same prompt later: add GitHub pull requests for an engineering standup, Stripe revenue for a founder check in, or a weather line when you commute on foot.


## Why route it through Composio


You could wire each of these reads up on its own, a calendar connection here, an email integration there, a Slack token after that, and keep four sets of credentials current. Composio is the reason you do not have to.


One connection covers Gmail, Google Calendar, Slack, Linear, Notion, HubSpot, and the rest of the toolkits in its directory, it holds the OAuth for each one and refreshes the tokens itself, and the agent only loads the tools a given request needs, so adding a fifth source to the briefing later is a sentence rather than another integration project. The apps you connect and the tokens behind them sit under your own Composio account, not a shared one.


Composio also markets this exact workflow for everyday users, the brief, log, and follow up loop, so a morning roundup is squarely what it is built to do. To set it up, connect Composio on the MCPs page, then open the[prompts library](https://operator.io/prompts) and send the morning briefing prompt. It will ask which apps to read, when to send the brief, and whose messages come first, and then it starts showing up before you do.
