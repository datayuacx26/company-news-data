---
schema_version: "1.0.0"
document_id: "46d2ec1b898a3e95dae5c4681392774850c5d3fed65c50196bdfb46ed94cdd29"
company_key: "yc-alai"
company: "Alai"
source_id: "yc-alai-news-import-161422870eb7"
canonical_url: "https://getalai.com/blog/growth-at-alai"
published_at: "2026-08-19T20:17:55.942+00:00"
first_seen_at: "2026-07-24T15:02:58.240858+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:807a785ad7def9717f5e7402228cfea6b33f561af364c6ca24a9531d7b25f2c2"
---

# Building Alai's growth from scratch: 88,000 signups as the first marketing hire

I joined Alai as the marketing lead back in November (2025). Eight months in, here’s a quick guide on the channels that helped me move the needle, the growth I saw and the strategy I used.


We measured success via the following three metrics:


-


**Traffic.** Getting the right people to the site without paying for every click.


-


**Signups.** Getting that traffic to actually try the product.


-


**Signup-to-paid conversion.** Getting those people to pay.


I worked on three channels: Reddit, SEO, and branding (socials, product newsletters, Product Hunt launch). Our primary focus was to build strong organic levers of engagement through content.


**The growth, at a glance**


**Metric I owned**


**Start**


**Finish**


**Growth**


Monthly signups


4,062 (Nov)


21,680


over 5x


Organic traffic / mo


around 700 (Jan)


27,800


roughly 40x


Measured search clicks / mo


7,020 (Jan)


21,200


roughly 3x


Signup to paid conversion*


around 1% (Nov)


Between 2 to 2.5%


roughly 2x


**Conversion is worked out from the signup and paid ratios.*


Paid users grew too, close to 12x from November to the April peak.


Paying customers came from Reddit and Google Search. Quality improved as volume grew.


## Pillar 1: Reddit


**Goal**


While exploring tools, most people usually search for some version of "what's the best app for X" or "has anyone actually used Y." A lot of that asking happens on Reddit, right at the moment someone is about to decide. The purpose was to show up for all the queries that mattered to us, in front of the people who were one good answer away from signing up.


There's a second payoff too. LLMs lean heavily on Reddit, so the threads we engaged in reached two audiences at once: the people reading them, and LLMs that include Reddit citations.


**Strategy / What was done**


A few systems made this work:


Finding the right threads before they went cold -


-


We set up an automation that scanned Reddit for threads mentioning keywords related to what we do, and dropped the relevant ones into a Slack channel every morning, so we could engage while they were still active.


-


I kept a running list of the subreddits that drove the most engagement for us, and rotated posts across them so we weren't leaning on the same community every time.


The posting itself ran across three personas, each with its own voice:


-


**Founder:** posting from the building-it side, as the person actually making the product.


-


**Brand:** the official Alai account, holding the company line.


-


**Marketing:** talking shop with other marketers, peer to peer.


The accounts were also at different stages of maturity, so the established ones took the higher-stakes threads while newer ones spent time warming up and building a real posting history first.


Across all three, the rule held: be useful before anything else. Every post had to teach or inform on its own merits, and Alai came up only when it genuinely answered the question, shown as one of several options someone might pick.


The posts spanned formats and funnel stages:


-


**How-to guides and automation walkthroughs:** top of funnel, pulling people in with something worth learning.


-


**Comparisons:** for people weighing one tool against another.


-


**Listicles:** for people close to choosing and scanning their options.


To make all of this repeatable, I built a Claude skill with its own readme that drafts a post for each persona automatically, in the right voice. It turned a slow manual write into something I could spin up quickly and keep consistent across every account.


**Results**


By April, Reddit was sending around 5.3K unique visitors to the site a month, and one post pulled 460k views on its own. Those visitors became signups: Reddit-sourced signups went from under 100 a month in November to around 4,100 in April, our second-largest source by self-reported attribution, behind only Google Search.


The bigger result was paying customers. Reddit became our single biggest source of them, ahead of search. So Reddit sat second for signups and first for paying customers, which means it converted better than any other channel.


## Pillar 2: SEO


This is the channel with the clearest before-and-after, so it's worth walking through properly.


**Goal**


When I joined in November, under 1% of our search clicks came from non-branded queries. In plain terms, almost everyone who found us on Google had already typed our name. Anyone out there searching "best ai presentation maker" with no idea Alai existed simply wasn't running into us.


So search was barely pulling its weight. It mostly worked as a login shortcut for people who already knew us. From January, the goal was to change that: make search a way for total strangers to discover Alai, across the whole funnel, and get Alai showing up inside AI answers while we were at it.


**Strategy / What was done**


**Starting with the content closest to a signup.** Signups were the primary goal, so I started at the middle and bottom of the funnel, where the search intent is closest to a decision. The first wave was:


-


**"Best \[X\]" listicles** ranking the top tools in a category, which is exactly what people search right before they pick one.


-


**Detailed reviews and alternatives pages** for the biggest tools in the space, to catch people already looking at a competitor: Gamma alternatives, Kimi alternatives, and so on.


-


**Trending topics** as they broke, like creating and editing slides from NotebookLM, where a sudden spike in searches had almost no good content answering it yet.


Top-of-funnel how-tos and utility tool pages (link-to-PPT, PDF-to-PPT, a deck prettifier) came in on top of that foundation once the commercial pages were ranking.


**Building authority with backlinks.** None of my content would rank without domain authority, and authority comes mostly from backlinks. The usual routes are expensive agencies or pricey tools. I built my own lightweight system instead, and it ran on about $20:


-


**Agent 1** scrapes relevant blogs for my target keywords automatically.


-


**Agent 2** reads each blog and writes a personalized opening line for the outreach email.


-


Everything flows into a **Google Sheet** with the email copy, follow-up sequences, and recipient info.


-


An **Apps Script** sends the approved emails, tracks replies, and handles the follow-ups on its own.


That system took our Domain Rating from 21 to 36 and our backlinks from 132 to 396. I wrote the whole thing up in a[LinkedIn post](https://www.linkedin.com/posts/nandini-jain-marketer_i-spent-100-on-backlink-tools-and-got-15-share-7428265181148979200-5B-k/) , which then became its own backlink engine: people who read it reached out wanting to use it, and those were exactly the people actively participating in exchanges.


**Turning the first three months into a machine.** By month three I had enough working patterns to systematize the whole content process into a chain of claude skills, each one handing off to the next:


-


**Keyword research:** takes a topic and uses the Ahrefs MCP to run detailed keyword research, then passes it down the line.


-


**Brief generator:** turns that into a full brief with outline, depth, keyword volumes, LLM snippet strategy, headline structure, and the backlink and interlink plan.


-


**SEO writer:** a different writer for each content type, each trained on our existing blogs for tone and on our product files for accuracy.


-


**Humanisation pass:** checks the draft for over-exaggeration and anything that reads as AI before it ships.


-


**Medium repurpose:** spins each finished blog into a Medium version to widen reach.


What used to take days of manual work became a pipeline I could run end to end. While these skills were my go-to for a strong first draft, I did always make it a point to do a heavy revision on all articles and each review was based on actual testing.


**Results**


The clearest way to see what changed is to watch how the branded and non-branded split moved.


**Search demand we captured**


**Nov 2025**


**Jan 2026**


**Apr 2026**


Share of clicks from non-branded queries


under 1%


14%


41%


Share of impressions from non-branded queries


5%


69%


92%


Read it left to right. In November, search only worked if you knew the name. By January, with the first content live, we were already appearing for the big commercial searches even though we weren't winning many clicks yet, which is what 69% of impressions at 14% of clicks tells you. By April, 41% of our search clicks came from people who had never heard of Alai before they searched.


One page did more of this than anything else. The "best AI presentation makers" listicle went from 115 clicks in January to 9,496 in April, and it now pulls close to 891,000 impressions a month. It's the biggest organic asset we have, and it ranks for the exact thing people search right before they choose a tool.


The size of that move is easier to feel with the before-and-after. The term "ai presentation maker" sat at position 90 in November and 30 in January. By April the "best" versions of those terms were in the top 3. Cracking the top 3 for "best ai presentation maker," "best presentation maker ai," and "best ai for presentations," and landing right at the edge of it for "best ai ppt maker," were the biggest wins of the whole channel. Those are the searches a buyer makes right before they pick.


When combined with our other blogs, the top-line numbers caught up. Measured search traffic, January to now:


**Metric**


**Jan 2026**


**May 2026**


**Change**


Total clicks


7.02K


21.2K


+14.2K (roughly 3x)


Total impressions


458K


2.17M


+1.71M (roughly 4.7x)


Average position


9.7


7.1


improved by 2.6


Average CTR


1.5%


1.0%


down 0.5pp


The CTR dip is easy to misread. Impressions grew much faster than clicks because the top-of-funnel guides started ranking for a huge spread of new, browsy queries. More people seeing us, a smaller share clicking any single term, and a lot more clicks in total.


The rankings and traffic that authority unlocked, from Ahrefs:


**Metric**


**Jan 2026**


**Now**


**Change**


Organic keywords


86


318


+232


Top-3 rankings


3


81


+78


Organic traffic / mo


around 700


27.8K


+27.1K


Traffic value / mo


around $200


$2.8K


+$2.6K


And the AI bet, starting from almost nothing:


**AI surface**


**Before**


**Now**


Google AI Mode


0


212


Google AI Overviews


0


180


Grok


0


755


Gemini


0


132


Perplexity


0


40


ChatGPT citations


9


15


The authority from the backlink work is what let the listicles climb from 3 top-3 rankings to 81.


All of this lands where it counts, in signups. By April, Google Search was our single biggest source of signups, at roughly 6,800 a month, up from 1176 in Jan when we just started out on SEO. The AI-visibility work showed up in the same place: signups crediting AI chatbots climbed from around 40 to nearly 1,700. (Both come from the self-reported "how did you hear about us" field.)


## Pillar 3: Social and branding (socials and newsletter)


**Goal**


Of the three pillars, this was the one I kept lightest on purpose. Reddit and SEO were where the growth came from, so that's where most of my time went. The job here was narrower: keep a basic, consistent brand presence across the channels that mattered, and communicate well with the customers we already had. Same voice and look wherever someone ran into Alai, and a newsletter that kept existing users in the loop.


**Strategy / What was done**


The work split into three pieces.


**Product newsletter.** We had a user base of over 100,000 people, and the newsletter kept them warm. On a bi-weekly to weekly cadence, each issue walked through new launches, how to actually use them, and ways to get more out of the product. The job was retention: bringing people who had already signed up back into the habit of using Alai.


**Socials.** Every launch and update went out across our brand pages and our personal LinkedIn accounts, in one consistent voice. The clearest example is the most recent one. When we opened public access to Alai’s Modern Slides, I made the launch video using Claude Code, and every design in the campaign was made with Alai or Claude itself. You can see[the launch post here](https://www.linkedin.com/posts/krishnagupta21_today-were-opening-public-access-to-alai-ugcPost-7471322949510168576-LFIT/) .


**Product Hunt launch.** My first ever Product Hunt launch! We finished the day in 4th place, with 314 upvotes and 58 comments. My biggest takeaway - spend a month engaging on other launches and PH in general and ask team members to do so as well!


**Results**


Two numbers track the branding side from November to April.


Brand searches climbed as the name got known. People specifically Googling Alai went from around 2,100 branded clicks a month in November to over 10,500 by April, with the number of brand searches itself roughly doubling on top of that. More people knew the name well enough to type it.


That recognition fed signups. Across LinkedIn, Instagram, and Twitter/X, social-driven signups went from around 550 a month in November to about 3,900 by April.


The traffic mix backs it up. By April, "Direct / None" was the second-largest source of visitors behind Google, ahead of every referral. That's people typing us in or coming back with no tracked link.


## Wrapping Up: A few moments I’m proud of


Taking the end of the blog to boast a little:


-


**88,238 signups** from November to June, off the back of three channels built from nothing.


-


**A single Reddit post hit 460k views** , and Reddit became our number one source of paying customers.


-


**Organic search traffic grew roughly 40x** , from around 700 visits a month to 27,800, and we cracked the **top 3 for "best ai presentation maker"** and the cluster around it. The flagship listicle alone pulled 9,496 clicks and close to 891,000 impressions in a single month.


-


**A $20 backlink system** I built with a couple of agents took our Domain Rating from 21 to 36 and our backlinks from 132 to 396.


-


**From invisible to cited inside AI** : 755 mentions in Grok, 212 in Google's AI Mode, 180 in AI Overviews, plus Gemini and Perplexity.


-


**Brand searches grew around 5x** , and we finished **4th on Product Hunt** with 314 upvotes.


-


**I raised my first PR using Claude code!** A huge milestone for me since I have often had to push/drop ideas due to lack of tech bandwidth.


## My Most Important Learnings


It wasn't all upward, and the back half of this period is where I learned the most.


**SEO.** At the end of May we lost our ranking on "best ai presentation maker," one of our biggest keywords and the blog behind it, and June dropped off hard because of it. When that much of your traffic sits on one or two listicles, a single ranking slip takes the whole month with it. Listicles are also the most contested and most volatile pages you can try to rank for, so leaning the channel on them was the real risk. The fix, which I'd already started, was to spread the weight across detailed guides and tool pages that don't rise and fall with one blog.


**Reddit.** In May, posting got inconsistent because we were heads-down on a big June launch, and Reddit fell off as a result. When a channel is actively driving growth, you can't go quiet on it for a few weeks without paying for it. That's what pushed me to build the proper Claude skill for Reddit, so the posting could stay consistent even when my attention was elsewhere.


**Backlinks.** The moment I eased off the outreach, our Domain Rating stalled. Authority needs steady upkeep; it flattens out the week you stop pushing on it.


There are far more channel-by-channel learnings than I can reasonably fit into one blog. If you want to get into any of it, or just compare notes, reach out:[LinkedIn](https://www.linkedin.com/in/nandini-jain-marketer/) .
