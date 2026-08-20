---
schema_version: "1.0.0"
document_id: "0f9748c085a18e5f912442d50c04696102ada912a88fa3b4db992f82a60a5b76"
company_key: "yc-whalesync"
company: "Whalesync"
source_id: "yc-whalesync-news-import-4fcd9b7a082a"
canonical_url: "https://www.whalesync.com/blog/9-lessons-weve-learned-from-171-million-sync-operations-per-year"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-22T19:45:33.835255+00:00"
fetched_at: "2026-07-28T22:12:48.521638+00:00"
content_hash: "sha256:840653e714eabfd12ab80aa7af52bb1108b869fcdbee113eb9e46c8e313bfcb7"
---

# 9 Lessons We've Learned from 171 Million Sync Operations per Year

I'm Curtis, CEO of Whalesync.


We started Whalesync and went through YC in the summer of 2021. Since then, we've worked with many companies on connecting their tools together, understanding their data, etc.


Over the last five years, we've formed strong opinions about:


- APIs
- SaaS data and how it's structured
- Best practices for connecting tools
- Structuring your data in a CMS, CRM, E-commerce, internal wikis, and productivity tools


Why am I saying all of this? Well, we've been working on some great new stuff, but before going into all of that, I wanted to give some context and share a few lessons we've learned over the years.


Whalesync is a key component to our customers' automation tool stack. On the surface, we're a tool that's simple to understand: sync data between two systems. Examples include[Airtable and Webflow](https://www.whalesync.com/connect/airtable-webflow) ,[HubSpot and Notion](https://www.whalesync.com/connect/hubspot-notion) ,[Supabase and Google Sheets](https://www.whalesync.com/connect/supabase-google-sheets) . It usually pairs well with a workflow automation tool like Zapier or n8n. While there is some overlap, Whalesync is only concerned with syncing data, while workflow tools are best at "if this then that" logic-based automation.


In working closely with our customers, we've formed some strong points of view.


# 9 lessons we've learned


## Most people don't understand their data


Most people don't understand their data across their tools. They don't know what's in it, how it's structured, and what will happen when they change it. Most people have no idea what you're talking about when you say "foreign key", "cascade delete", or "nested JSON".


## Some data is more complex than others


There are three annoying types of data to sync: rich text, foreign keys, and binary files. Rich text is rendered differently in every tool and requires careful transformations to convert it from one tool and saved in another. Foreign keys require lookup tables which can get stale, such as when a source record is deleted. Binary files (images, PDFs, etc.) need their own upload/download process, proper headers, and their source URLs often have short expiration times.


## Most SaaS tools have poor data restore options


Most SaaS tools can't restore customer data cleanly. If data gets messed up via an automation, a bad import, or AI, it's hard to roll back. Even when a tool offers a trash folder or version history, it usually doesn't have a way to quickly restore your data in bulk to a point in the past.


## A source of truth leads to the best results


Most data needs to have a source of truth. This is common among large enterprise systems, but uncommon in smaller companies. Even if it's just informally determined on a team, any piece of data that people care about should have **one** spot where that data is declared the "truth". This could be a customer first name/last name, an email address, a product price, a blog post, etc.


## Visibility matters


The more visibility into what a tool is doing, the better. You don't need to show users all information at all times, but when someone needs to dig into why something isn't working as expected, visibility is key.


## Raw is best


Raw API data is the best thing to store, which is usually JSON. Transformation or filtering tends to be lossy and has downstream effects. When setting up automations, you don't always know ahead of time what data you'll need. If you have all of it in its original form, you're good to go.


## SaaS data is basically a collection of documents


SaaS APIs return "records", but it's often more useful to think of these bundles of information as **documents** . They often contain all kinds of unusual nested data and they don't always fit an expected schema.


## Data hub


Pulling, editing, transforming, and syncing data is much easier when it's stored together in one spot. There are several names for this, but we like the term "data hub". This is different from a data warehouse because the purpose is operations, not analytics, but it's a similar concept. We started to build Whalesync this way, but we didn't fully lean into the concept.


## Deletes are scary


Deletes need a human-in-the-loop approval. When you delete a record via an API (especially in a CRM), it often causes a "cascade delete". This causes other records that were tied to the deleted record (e.g. notes, emails) get deleted as well. There's usually no easy way to restore it.


# AI hypotheses


In the last six months, AI tools like Claude Code have started to fundamentally change the way we work. A system that can effectively extend our brains and perform the fanciest auto-complete we've ever seen has made us rethink how to best handle data and where it's stored. As with most startups, we've been using Claude Code on a daily basis and trying to use these newfound[mech suits](https://www.youtube.com/shorts/IeOZtj08zAA) to the best of our ability. Here's what I believe so far.


## The CLI is AI's favorite form factor


If you've used Claude Code and you have a bit of a background in the command line, you've noticed what Claude is doing. It's weaving together command line tools in ways that I've only seen done by the most grizzled Linux gurus, like it's trying to make Richard Stallman proud. MCP servers are good but they often have limited functionality and power, plus they struggle with keeping authentication alive via OAuth.


## AI likes combining discrete actions


Calling back to the old days of[Unix philosophy](https://en.wikipedia.org/wiki/Unix_philosophy) , AI works best when it works with tools that do one thing well. Each high-quality, single-purpose tool it has access to multiplies its capabilities. If we want to design software that AI can use, we need to keep this top of mind.


## Local files are best


Local files are the easiest and fastest data format for AI to work with. There are no rate limits, no transient API issues, and countless CLI tools can work with the files immediately.


## AI likes raw API data


Raw API data is the easiest for AI to understand because it's already been trained on a million similar examples. If Claude sees a Stripe or HubSpot JSON record, it already knows how to read it. This is a huge time (and token) saver.


## AI needs safety


The more we've been connecting AI directly to our own SaaS tools, the scarier it is. Can you imagine running a production software service with no version control, no database backups, and no testing or deployment pipeline? That's what it feels like when you connect Claude to Webflow/HubSpot/Shopify.


# What's coming


We've been working on a product that's designed to complement Whalesync and we're calling it Scratch. It takes all of these strong opinions we formed over the years and creates a central data hub that's perfect for managing your SaaS data in this crazy AI-driven world.


These are some use cases that Scratch supports (or could easily support soon):


- Safe bulk editing and reviewing of data in your CMS, CRM, E-commerce store, or spreadsheets
- AI workflows that monitor, improve, and iterate on SEO performance of your CMS
- Reviewing what's changed in your CRM over the past week
- Regular backups of your SaaS tools in case of data corruption (e.g. from AI or automation tools)
- Style guide checking of all content being published in your CMS


There's a lot more to come, but I wanted to share the context for this new offering. If you're curious, you can sign up for the wait list at[https://app.scratch.md](https://app.scratch.md/) . Stay tuned!
