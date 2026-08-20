---
schema_version: "1.0.0"
document_id: "c34b8501c8febf31ea1d6cf3e81b897f7ad985b01e3863237f0a3d699e7a5f0c"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/email-is-now-a-column-in-clay"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-24T05:21:53.618588+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:f5ff3685a1d072ef786026e3194756cf83d105d903998ef34bf7ab1335df19c4"
---

# Email is now a column in Clay

AgentMail is now a native source and action in Clay. Your tables can receive email, read it, and reply, without leaving the row.


If you haven't lived in it: Clay is the platform more than 500,000 GTM teams use to find companies and people, enrich them with data from hundreds of sources, and automate outreach.


Here's something Clay couldn't do until today: read an email that just landed, and reply to it, from inside a table.


Picture the loop.


```text
A prospect answers your outbound
↓
The reply shows up as a new row in Clay the moment it hits your inbox
↓
Claygent (Clay's AI research agent) reads the body, tags it "interested,"
drafts a response in your voice, and the row sends it back


```


No forwarding. No Zapier in the middle. No copy-paste into Gmail. The whole conversation lives in the same table you already use to enrich and route.


Email just became another column.


We built AgentMail because agents needed an inbox of their own, a real address that can receive, draft, and send over an API. Clay is where a huge amount of GTM work already happens. Putting the two together closes a loop that stayed open for a long time. Clay could always find people and send to them. It could never hear them answer. Now it can.


## The missing primitive


Most tools in this space only go one way. They push mail out and go quiet. The reply, the actual moment a human writes back, vanishes into a mailbox your workflow can't see.


That reply is the most valuable signal you get all day. Someone raised their hand. And until now it lived outside the system doing the work.


With AgentMail as a source, incoming mail flows into Clay as rows in real time: sender, subject, body, timestamps, all of it. From there it behaves like any other Clay data. Enrich it. Run it through a prompt. Branch on it. When you're ready to answer, two native actions (draft and send) close the loop from the same row.


## What you can build with it


Three flows that were painful yesterday and are three columns today.


### Reply-based outbound


A prospect answers your campaign. AgentMail catches it, Clay classifies it (interested, not now, out of office, wrong person, referral), enriches the account, assigns an owner, and drafts the next reply. You approve it or let it fly.


### CC an agent into a live deal


Drop an AgentMail inbox onto a real customer thread. It tracks context, remembers what was said, and nudges the follow-up you'd have forgotten.


### A command inbox for GTM


Forward it a messy list, an intro, an event export, a quick "can you enrich these." AgentMail turns the email into a trigger and Clay does the work downstream.


None of these need a new dashboard. They need an inbox your table can actually reach.


> "I haven't been this excited about an integration in a long time. This is going to open up a ton of really cool use cases for Clay."
>
>
> Alex, one of Clay's first GTM Engineers ([gtmengineering.ai](https://gtmengineering.ai/) )


He's not kidding about the use cases. Alex is dropping 11 of them in his newsletter this week, so keep an eye on[gtmengineering.ai](https://gtmengineering.ai/) if you want the deep cuts.


## Wiring it up


Most of the setup lives on the AgentMail side, and it's a one-time thing. Once your inbox and key exist, connecting to Clay takes about a minute.


**1. Point a domain at AgentMail.** Point a domain or subdomain at AgentMail to send and receive from. If your root domain already runs Gmail or Outlook, use a subdomain like` inbox.yourdomain.com` . AgentMail generates the DNS records; paste them into your provider and hit verify.


**2. Create an inbox.** Once the domain verifies, spin up an address like` sales@inbox.yourdomain.com` . That's the live inbox Clay listens to. Add as many as you want for different workflows.


**3. Generate an API key.** From your AgentMail dashboard.


**4. Add the source in Clay.**


```text
New table → Open Sources
↓
Search "AgentMail" → Pick "Import AgentMail message events"
↓
Paste your key
↓
Choose inboxes to listen to (up to ten per source)
Choose events (incoming mail, or the full set including sent and delivered)
↓
Clay creates the webhook on the AgentMail side automatically
↓
Send a test email to your inbox and watch a row appear, live


```


**5. Reply from the table.** Add a column, choose the AgentMail action.` Create draft` stages a reply in the inbox for a human to review.` Reply` sends it out, plain text or HTML, with the body built from other columns (a Claygent draft, for instance). Send runs on a Run button, so you fire it per row when you're ready, or set it to go automatically.


**Tech TLDR:** Bring your own AgentMail key. The import source subscribes to inbox webhook events and fans them into rows. Two native actions (draft, reply) send from the same inbox. Human-in-the-loop with draft, hands-off with reply.


## The loop, closed


Receive an email. Read it in Clay. Enrich, classify, decide. Draft the answer. Send it. Watch the delivered event land back in the table. One surface, start to finish.


We think agents should live where the work already is, not in one more tab. Your GTM brain already runs in Clay. Now it has ears and a voice.


Get started:[clay.com/integrations/data-provider/agentmail](https://www.clay.com/integrations/data-provider/agentmail)


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
