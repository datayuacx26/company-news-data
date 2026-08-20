---
schema_version: "1.0.0"
document_id: "c6bf80a9327d3f2c59a055bd23bc3c67683a5bbd5be0174a351c177651b2e01f"
company_key: "yc-operator-labs"
company: "Operator Labs"
source_id: "yc-operator-labs-news-import-4e5219b8eb88"
canonical_url: "https://operator.io/blog/daily-news-digest-with-your-agent"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-27T00:56:34.740230+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:ef601d6ea7460c613e49dfdc72d337de176ac656d8da228a378db0caa6ecf25d"
---

# Get a daily news digest on your topics with OpenClaw

Staying current on a few subjects should not cost an hour a day, but that is what it turns into when the input arrives as an endless scroll. You open a reader, skim ten headlines to find the two that matter, and half the time you read the same story you already saw yesterday under a different headline. The information is out there; the problem is the shape it arrives in.


A digest is the better shape: one message a day, on the topics you chose, with the few items worth your time and a line on why each matters. Your[Operator.io](https://operator.io/) agent can produce that. You send it one prompt, tell it what to follow, and it handles the searching, the summarizing, and the deduping, then comes to you once a morning with the result.


## How it works


You send the digest prompt, and the agent sets up the log and the daily search behind it:


- It keeps a CSV in its workspace with the date, the topic, the headline, the source, the link, and a one line summary for each item.
- Once a day it searches the web for what is new on the topics you follow and writes a short summary of each piece in its own words.
- It checks each link against what is already in the file, so the same story never lands in your digest twice.
- It sends you one roundup in your channel, grouped by topic, with a line on why each item matters, or a quick note when it was a slow day.


The summary is the agent's own, so the digest reads like a person caught you up. You can change the topics or the timing on the Automations page in your dashboard.


## The prompt


This is the instruction the agent acts on:


```text
Be my daily news digest. Create a spreadsheet in your workspace, a CSV with
columns for the date, the topic, the headline, the source, the link, and a
one line summary. Once a day, search the web for what is new on the topics I
follow, write a short summary of each piece yourself, and add a row, checking
the links against what is already in the file so the same story never shows
up twice. Set up an automation that runs every morning and sends me one
roundup here, grouped by topic, with the few items actually worth my time and
a line on why each one matters, or a quick note when it was a slow day.
Before you start, ask me which topics and sources to follow, how many items
per topic is the right amount, and what time to send the digest.


```


The same prompt is saved in the[prompts library](https://operator.io/prompts) , so you can send it to your agent without retyping a word.


## What the agent is actually searching


When the prompt says "search the web," the agent is not opening a single app. It is querying the open web the same way a developer would wire a news product: topic searches, publisher homepages, and syndication feeds that still carry most of the world's breaking coverage.


[RSS](https://en.wikipedia.org/wiki/RSS) and its successor[Atom](https://en.wikipedia.org/wiki/Atom_(standard)) remain the format underneath most news sites. A feed is a simple XML file a publisher updates whenever they publish; readers like[FreshRSS](https://freshrss.org/) and[NewsBlur](https://www.newsblur.com/) subscribe to those files and pull new items on a schedule. Your agent can read the same feeds when you name specific outlets, or it can search more broadly when you care about a topic rather than a masthead.


[Google News](https://news.google.com/) aggregates headlines from thousands of publishers and exposes them through public RSS endpoints, which is why so many news tools still lean on it even though Google[retired the official News API years ago](https://en.wikipedia.org/wiki/Google_News) . Developers who build news products today typically use a third party API like[News API](https://newsapi.org/docs) , which returns JSON from over 150,000 sources, or publisher specific APIs such as[The Guardian Open Platform](https://open-platform.theguardian.com/documentation/) . Your agent on Operator has web search built in, so it can mix those approaches without you standing up API keys unless you want tighter control.


That flexibility matters because news breaks in different places. Regulatory filings show up on government sites before they hit the trade press. A niche forum will carry the first credible leak on a hardware rumor. A local paper will have the only detailed report on a zoning fight. Tell the agent which sources you trust on a given topic and it will weight them; tell it to skip a source that keeps surfacing repackaged press releases and it will stop wasting a slot in your morning message.


## Using it day to day


The digest lands at the time you picked, grouped the way you would group it: a few items under each topic, each with a sentence on why it is worth a look, and the link if you want to go deeper. You read it like a short briefing rather than a feed, and on a quiet day it says so instead of padding the list to look busy. Tell it how many items per topic you want and it holds to that, so ask for three and you get the three best.


The summary is there to tell you whether a story is worth your click, and because it is written in the agent's own words it can compress or round a detail in the process. For anything you plan to quote, act on, or make a decision with, open the link and read the source first, the way you would with any headline.


Because the digest is built from a CSV the agent keeps, you can shape it by talking to it. Add a topic, drop one that has gone stale, tell it to weight a source you trust or skip one that keeps surfacing noise, or ask for a longer weekend edition that goes deeper. When you want to look back, ask it what came up on a topic over the last month and it answers from the file.


The dedupe column is easy to underestimate until you have lived with it for a week. The same acquisition gets written up by Reuters, then Bloomberg, then three trade blogs, each with a slightly different headline.


An exact repeat of a link it already logged never gets through, since that is a clean URL match. The same event under three different publisher URLs is a judgment call rather than a clean match, so the agent leans on the headline and its own summary to fold those together, and now and then a near double still slips by. Tell it which outlets you would rather see cover a story and it weights toward your preferred source when the same news shows up in several places. Even with the occasional miss, that is the difference between a digest you can finish in two minutes and a pile of links that still feels like a feed.


If you follow fast moving topics, ask for a second pass on weekday afternoons. The morning run catches overnight news; a lighter check before you sign off catches the story that broke at lunch. Both runs write to the same file, so the dedupe still holds and you do not get the same link twice in one day.


## Why this beats another reader


Most reader apps put their best features behind a subscription.


Tool Free tier Paid


[Feedly](https://feedly.com/i/pro) up to 100 sources Pro at $6 a month, Leo AI on Pro+ at $8.25 a month billed annually


[Inoreader](https://www.inoreader.com/pricing) 150 feeds with ads Pro at $7.50 a month annually for rules, monitoring, and summaries


[Google News](https://news.google.com/) free, fine for browsing a feed to scroll rather than a digest delivered to you


All of them still expect you to come to the app and do the filtering.


Feedly's Leo and Inoreader's Intelligence summaries are useful once you are already inside the reader staring at fifty unread items. They help you triage faster, but they do not remove the step where you open the app, pick a folder, and decide what deserves attention today. A scheduled agent digest flips the order: the triage happens before the message arrives, and the output is sized to what you asked for, three items per topic, not three hundred.


The log being a CSV also changes what you can do with history. Export it to a spreadsheet, chart how often a company appears in your "competitors" topic, or paste a month of rows into ChatGPT, Gemini, or Claude when you are writing a quarterly recap and want a quick sense of what moved in your industry. Reader apps keep that history inside their own database; here it is a file in the agent workspace that you can copy, back up, or query in plain language.


## Setting topics that actually work


Broad topics like "AI" or "markets" will flood the digest unless you tighten them. "Open source agent frameworks," "FDA oncology approvals," or "Seattle commercial real estate" give the search step something to aim at. Pair each topic with two or three sources you already read so the agent has anchors when the web is noisy.


If you care about primary documents, say so explicitly. Ask the agent to prefer SEC filings, court dockets, or agency press releases over commentary when both exist on the same story. If you care about speed over depth on one topic and depth over speed on another, split them into separate rows in the setup conversation so the morning message can give each topic the right number of slots.


To set it up, open the[prompts library](https://operator.io/prompts) and send the daily topic digest to your agent. It asks which topics and sources to follow and how much you want per topic, then it does the reading and sends you the short version each morning.
