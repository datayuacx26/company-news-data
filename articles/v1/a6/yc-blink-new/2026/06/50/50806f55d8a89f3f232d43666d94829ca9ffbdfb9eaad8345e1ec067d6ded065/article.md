---
schema_version: "1.0.0"
document_id: "50806f55d8a89f3f232d43666d94829ca9ffbdfb9eaad8345e1ec067d6ded065"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-ai-chatbot-website"
published_at: "2026-06-12T02:34:23+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:89d99199d8138ecc8d8fcfb644aef57c529dfb65a31d6fa951e68b0ead40a8e7"
---

# How to Build an AI Chatbot for Your Website (Custom Context, No Generic Answers)

## Step 1 — Build the knowledge base


This is the most important step. Spend more time here than anywhere else.


The knowledge base is a table of articles: title, content, category, and a last-updated timestamp. You also need a search function so you can test what the bot would find for any given query before you go live.


Here's the exact prompt to use in[Blink](https://blink.new/) :


```text
Build the knowledge base for a customer support AI chatbot. Create:
- A database table for knowledge articles: title, content, category, last_updated
- An admin interface where I can add, edit, and delete knowledge articles
- Categories: Product Info, Pricing, FAQ, Returns & Refunds, Technical Support
- A search function to test what the bot would find for a given query


```


Start by copying in your existing FAQ content. Add your pricing page, your return policy, and any product documentation you already have. One afternoon of data entry here prevents weeks of bad chatbot answers.


**One practical tip:** for each answer in the knowledge base, write it the way a customer would ask the question, not the way your legal team wrote the policy. "Can I get a refund?" performs better than "Refund Eligibility Policy — Section 4.2."


## Step 2 — Build the chat interface


Once the knowledge base has content, build the chat widget. Here's the prompt:


```text
Add an AI chatbot to my website. The chatbot should:
- Appear as a floating chat button in the bottom-right corner
- Know my business context from the knowledge base (use vector search to find relevant articles)
- If it can't answer with high confidence, say "Let me connect you with our team" and show a contact form
- Store every conversation (question, answer, timestamp, confidence score) in a conversations table
- Show a live conversation dashboard in the admin panel
- The chat uses OpenAI or Claude API (I'll provide my API key in the settings)


```


A few things to note here:


**You'll need your own AI API key.** Blink builds the app and wires it up, but the model calls go through your OpenAI or Anthropic account. You pay for the tokens directly. For a small to medium site, this is $5-$15/month — far below what Intercom charges. For a high-traffic site, factor in your expected conversation volume.


**Vector search matters.** The chatbot shouldn't keyword-match against your knowledge base. It should semantically understand that "do you do returns?" and "what's your refund policy?" mean the same thing. Blink handles this automatically with the right prompt.


**The conversation dashboard is not optional.** After your first 50 conversations, you'll know exactly what your customers actually ask — and it's almost never what you expected. Build the dashboard from day one.


## Step 3 — Add the escalation layer


Low confidence needs a real destination. "I'm not sure about that" is unhelpful. "Let me connect you with our team" followed by a real contact form is useful.


The escalation threshold is a setting you tune over time. Start at 75-80% confidence. Below that threshold, the chatbot:


1. Says it doesn't have a confident answer
2. Shows a contact form (name, email, message — pre-filled with the user's question)
3. Sends an email notification to your support inbox
4. Logs the escalation in the dashboard so you can identify gaps in the knowledge base


If you have a team, you can add human takeover — an admin can see live conversations and jump in. Blink can build this in the same session with a prompt like "Add a live takeover mode where admins can see active conversations and respond directly."


## Step 4 — Add lead capture


This takes 10 minutes and can meaningfully improve your conversion rate.


When a user asks about pricing, plans, or discounts — buying-intent signals — the chatbot can ask for an email before answering. Something like: "I can help with that — would you like me to send you our full pricing breakdown?"


Captured emails go into a leads table with the question context. You can export this weekly and follow up personally.


The prompt to add this to your existing chatbot:


```text
If the user asks about pricing, plans, or discounts, ask for their email before answering
in full. Store captured emails in a leads table with the question and timestamp. Show
leads in the admin dashboard with a CSV export button.


```


Don't overdo this. Only trigger on clear buying-intent signals. Asking for an email when someone asks "what are your business hours?" is annoying and unhelpful.


## Step 5 — Embed on your website


Once your chatbot app is live in Blink, embedding it is a single JavaScript snippet.


Paste this before` </body>` on any page:


```text
<  script   src  =  "https://your-chatbot.blink.app/widget.js"   defer  ></  script  >
```


This works with any website: WordPress, Squarespace, Webflow, custom HTML, or any JavaScript framework. You don't touch the chatbot's code — it runs in Blink's infrastructure. Database, auth, and hosting are all included. No Vercel config, no Supabase account needed.


If you want the widget on every page, drop it into your layout file or` <head>` . If you want it only on specific pages — your pricing page or documentation — add it there.


Chatbot widget going live — the chat icon appears in the bottom-right corner of a website after clicking deploy


Blink


## Comparison: custom chatbot vs off-the-shelf tools


Intercom Drift Custom Blink Chatbot


**Monthly cost** $74-$499/mo $400-$800/mo $0-$29/mo + API costs


**Knowledge base** Yes (structured) Yes (structured) Custom (you control the schema)


**Escalation path** Yes Yes Yes (you design the flow)


**Data ownership** Intercom's Drift's Yours


**AI customization** Limited Limited Fully custom


**AI API key** Theirs (opaque cost) Theirs (opaque cost) Yours (pay-as-you-go)


**Lead capture** Yes (CRM integration) Yes (CRM integration) Custom (direct to your DB)


Intercom and Drift are good products built for enterprise teams that need shared inboxes, SLA management, and integrations with Salesforce. If you're at that scale, use them.


If you're a small team — fewer than 10 support conversations a day — you're overpaying by $4,500-$9,600/year for features you'll never use. A custom chatbot built with[Blink](https://blink.new/) gives you full data ownership, complete control over the AI behavior, and costs a fraction of the monthly fee.


The API costs are transparent: OpenAI's GPT-4o costs about $0.0025 per 1K tokens. A typical customer support exchange (question + knowledge base context + answer) is roughly 800 tokens. At 50 conversations/day, that's $3/month in API costs.


Custom chatbot: small cost compared to crumbling expensive SaaS subscriptions


Blink


## Where to start


Write a 5-line knowledge base article about your most-asked customer question. Something like:


**Title:** Do you offer refunds? **Category:** Returns & Refunds **Content:** We offer full refunds within 30 days of purchase. After 30 days, we offer store credit. To request a refund, emailsupport@yourcompany.com with your order number. Processing takes 3-5 business days.


That's your first knowledge base article. Build from there. Add five more this week. After your first 20, you'll have covered 80% of the questions your support team answers by hand every day.


Build this with Blink — database, auth, and hosting included. No config needed →[blink.new](https://blink.new/)


---


No. Blink builds the full-stack app from a description. You write the prompt (like the examples in this article), and Blink generates the database schema, the admin interface, the AI integration, and the embeddable widget. You'll need to provide your own OpenAI or Anthropic API key and add a few knowledge base articles — that's it.


If you want to customize the behavior later (change the escalation threshold, add a new lead capture trigger, modify the UI), you describe the change and Blink updates the code. No coding required.


Two costs: Blink's hosting fee ($0 on the free plan, $29/month on Starter) and your AI API costs. OpenAI and Anthropic charge per token. A typical support conversation costs about $0.002-$0.005. At 100 conversations/day, that's roughly $6-$15/month in API costs.


For context: Intercom starts at $74/month before any AI features. Drift starts at $400/month. A custom chatbot on Blink typically costs 70-90% less for the same functionality, with full data ownership and no per-seat pricing.


That depends on how you build the escalation layer. The typical setup is: if the chatbot's confidence score is below 75-80%, it says it doesn't have a confident answer and presents a contact form. The user's question is pre-filled in the form, and a notification goes to your support inbox.


You can also add a live takeover mode — an admin can see active conversations in the dashboard and jump in directly. For most small teams, the contact form escalation is sufficient.


Not automatically, but you can build a feedback loop. The conversations table stores every exchange — question, answer, confidence score, timestamp. Review the low-confidence conversations weekly and turn the good human answers into knowledge base articles. After 4-6 weeks of this, the chatbot handles significantly more questions without escalating.


If you want automatic learning, you can extend the system with a prompt like "Add a feature where support staff can mark any escalated conversation as 'resolved and learned' — this adds the answer to the knowledge base automatically." Blink can build that workflow in the same app.
