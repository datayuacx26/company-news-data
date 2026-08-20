---
schema_version: "1.0.0"
document_id: "dc7b87172150be8c72a1dd7472a8b209d46b8322f1f77ff6312aefc4e568780f"
company_key: "yc-openphone"
company: "Quo (fka OpenPhone)"
source_id: "yc-openphone-rss-7c9664a9f202"
canonical_url: "https://www.quo.com/blog/how-quo-built-voc-program/"
published_at: "2026-08-04T15:30:17+00:00"
first_seen_at: "2026-08-04T15:47:11.792243+00:00"
fetched_at: "2026-08-04T17:31:37.256081+00:00"
content_hash: "sha256:aa1e26c6e1a92cb129b33937ab302f558c8bc8d19f4329e6d7f3436a8a9ba709"
---

# What Quo’s Voice of the Customer program can teach any small business

Every small business already runs some version of a


[Voice of the Customer](https://www.quo.com/blog/voice-of-customer-analysis/) program. It’s just usually informal: noticing when three customers complain about the same thing within a week or keeping a mental list of what people keep asking for.


At Quo, we’ve built a more formal system. About 25,000 support tickets come through every month across every channel we support. Getting our program to work at that scale means staying disciplined about who’s watching for what and how much feedback justifies action. AI does a lot of the reading but makes none of the judgment calls.


Here’s what that looks like day to day and how it could work for a small business.


##


**The mandate**


My goal for our VoC program is simple: get customer pain points in front of the people who can act on them. That means three audiences, not one. What matters to the product team isn’t always what matters to engineering, and what the whole company needs to know about is often different from both.


What sits underneath that goal is straightforward. Our ticketing system, Zendesk, includes a feature that automatically


[flags customer sentiment](https://www.quo.com/blog/customer-sentiment/) , and the AI tool doing most of the day-to-day work is Claude.


A key distinction I draw is between CSAT and sentiment. CSAT, the satisfaction score customers give after a ticket closes, only tells you about a problem after it’s already happened. Sentiment shows up earlier, in the tone of a first message or how a chat with our AI bot is going. It can flag a problem before it turns into a bad survey score. So, we pay attention to both.


For a smaller business without our infrastructure, the mandate is what’s worth copying, not the mechanism. Identify pain points. Don’t let them die in an inbox. The specific tools you use will vary and evolve, and that’s fine.


##


**The flashlight rule**


I describe AI’s role here at Quo as a flashlight. It can show you where to look, but somebody still has to walk over, confirm what’s there, and decide what it means.


It starts with dashboards that show the types of tickets we receive and the monthly volume. When the data shifts, ‌whether it’s a spike in one category or a dip in another, that’s the signal to point Claude at the underlying tickets and ask why.


Claude works best when I give it small batches of 50 to 100 tickets at a time. Feed it 500 or more at once, and it starts inventing patterns that aren’t actually there. After Claude flags a potential trend, someone on our team reviews at least 20% of the relevant tickets before I treat it as a real trend and act on it. Working in small batches and validating the results manually keeps the analysis accurate.


One month, technical support tickets increased by 40%. This is a clear example of the flashlight landing on something real. The next step was to break down that increase further. Were the increased tickets about phone calls or integrations? It turned out integration tickets were up by about 10%. So I had Claude scan roughly 100 of those tickets to figure out what was going on.


The same process applies to slower-moving patterns. Our CSAT scores dip in months with service incidents. It’s not surprising on its own, but it’s exactly the kind of pattern the flashlight is supposed to catch early, rather than something that only becomes obvious after a full quarter of low scores.


I can afford to be patient about this, partly because our baseline is strong. Industry average CSAT sits at 70% to 80%. We’re usually around 90%. That headroom is what lets us treat individual complaints as data points worth investigating rather than fires that need putting out immediately.


This lesson works for businesses of any size. Decide in advance how many complaints justify a change. Write the number down if you have to. It’s a lot easier to set that number before an angry email shows up than after.


##


**Delegating the light**


None of this works if one person is reading every ticket. Two of my teammates own pieces of the monthly Voice of the Customer report directly. Nisha Dhiman owns the reliability category. When engineering tells her something broke, she figures out what that meant for our customers and how badly it hit them.


Every month, she creates a report that covers how many incidents occurred, how many tickets each one drove, and what customers said. Every write-up includes the actual language customers used, not just the ticket count.


Drew Schuffenhauer owns bug reporting, and Claude does a first pass on incoming reports before he reviews them. Much of that first pass is sorting real bugs from user err‌or. If the app doesn’t work because a customer’s own internet ‌is down, that’s not a product issue. Catching that earlier means the reports that reach the product team are worth their time.


Beyond Nisha and Drew, we also have subject matter experts dedicated to our messiest categories: carrier registration and porting rejections. Carrier registration is the paperwork a business has to file with phone companies before the business can text customers. Registrations get rejected for very specific reasons. An invalid business website alone drives 45.7% of those rejections. The next biggest reason is not having proof on file that customers opted in to receive texts in the first place.


Porting, or moving a customer’s existing phone number over to Quo, is another common driver for tickets. An incorrect transfer PIN drives the most rejections. Having someone who specializes in porting means we catch the pattern early and fix the real problem, not just the ticket in front of us.


Ticket volume grew a lot faster than our headcount did. We kept up by giving colleagues ownership of categories.


It doesn’t take an org chart to make this work, even at our size. It just takes someone deciding, even informally, who’s responsible for noticing what, so nothing disappears into one unsupervised inbox.


##


**Getting people to actually read it**


A report nobody reads doesn’t do much good. Internal readership of the monthly digest quadrupled across the company this year. A few things drove that. My manager, Quo’s VP of Customer Experience, Justina Altiere, brings it to her own leadership meetings. New hires are introduced to the report on day one as part of learning who our customers are and what frustrates them.


Probably the simplest factor is that the report comes out the same week every month, which builds a habit of expecting it. We also moved distribution out of smaller, more private Slack channels into a company-wide one. That change alone made a real difference to who saw it.


A small team may not be able to compel everyone to read something or even have a company-wide


*anything* yet. But the consistency lesson still stands. Review feedback on the same day every week, somewhere everyone can see it, and it becomes routine.


##


**What we’d tell a small business about listening to their customers**


If someone asked me how to build a version of this on a tight budget, my advice would be simple. Get a $20-a-month Claude subscription, connect Claude directly to your business email through an MCP integration, and let it read and categorize incoming messages automatically. You don’t need a data team or an expensive support platform to start noticing patterns.


You don’t even need a ticketing tool yet. Gmail works fine since Claude connects to that directly. What you need is a place where your customers’ messages already live and a tool that can work through them in manageable batches so you don’t have to read every message yourself. Platforms like Zendesk charge by the user, and those costs add up fast if you’re just starting out, so there’s no shame in sticking with what you’ve got.


The specific tool matters less than how well it connects to where your messages already live. If you’ve got Claude or ChatGPT and can find something inexpensive that connects cleanly to your messages, you’re most of the way there. If you happen to use Quo,


[Claude can connect to it directly](https://support.quo.com/core-concepts/integrations/mcp) .


The same 20% rule applies here too, whether you’re a five-person shop or a large company. Decide, ahead of time, how many complaints it takes before you‌ make a change.


##


**Where our VoC program goes next**


It comes back to the same mandate. We still need to get pain points in front of the people who can act on them, no matter which tool is doing the listening.


What’s next for us is making the ticket data easier for people inside Quo to search for themselves. Instead of asking Claude a broad question and risking a made-up answer, they’ll be able to search within specific categories, like carrier registration. Narrower instructions should produce results that only come from the tickets that matter.


My more immediate goal is simpler than any of that. It’s to publish the digest every month, without exception, for the rest of the year. I’m about halfway there already, and I don’t see a reason to stop now.
