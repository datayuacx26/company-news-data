---
schema_version: "1.0.0"
document_id: "533827fea08e360667b615cb1033ee2da24d64269f788208ed45b0fbc6645c41"
company_key: "yc-loops"
company: "Loops"
source_id: "yc-loops-news-import-d3d77458967f"
canonical_url: "https://loops.so/changelog/cli-skills-array-support"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-25T13:13:35.252230+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:8244bd8e4ecf5d579e68a8266406a4f96fe67ca65533c6f225c151f6e7a045be"
---

# CLI, Skills and Array support

In this release, we’re introducing our CLI, Arrays, and Skills, and sharing what we’re working on next. If you missed it, we also launched a new[home page](https://loops.so/) .


### **CLI**


We just released the first version of our CLI.


It is currently[open source](https://github.com/Loops-so/cli) and has parity with our current API. You (or your agent) can now send transactional emails, update contacts and more from your terminal.


This is the first step in making Loops drivable via API. You can dive right in through the docs or send your agent to this[markdown variant](https://loops.so/docs/cli.md) of the CLI docs.


### **Arrays**


Our new[Arrays feature](https://loops.so/docs/creating-emails/editor/arrays) allows you to send multiple items in a single payload. This is perfect for invoices, order updates, or any email that requires a varying number of items per order.


### **Skills**


We have just released[Skills for Loops](https://loops.so/agents/skills) . Currently, you may install all applicable skills or install them individually, you can also use the best practices skill independently from Loops.


You can now tell your agent to wire up Loops in an app it’s building, trigger a welcome email when a user signs up, or manage a mailing list from a scheduled job and it will use the SDK, the HTTP API, or the CLI depending on where the code lives.


**API**


```text


```


Work with contacts, lists, events, and transactional email through the Loops API and SDKs.


**CLI**


```text


```


Install, authenticate, and run Loops workflows from the terminal.


**Best practices**


```text


```


Improve email quality, deliverability, and message type decisions.


### **Shipping soon**


Coming soon, we’ll be launching **Goals** in alpha to help track conversion data associated with email sending, along with a **Content API** for Campaigns.


This will let you work with Campaigns over the API. You will be able to create them from scratch, update content and styles, and use Claude or Codex to update your emails.


Soon after, we’ll be rolling out API support for our Loop Builder as well as Transactional emails. If you’d like early access, please reply to this email.


### **Everything else**


We also released new[subscription events for Stripe](https://loops.so/docs/integrations/stripe#supported-events) , a[suppression list endpoint](https://loops.so/docs/contacts/suppression) , dozens of bug fixes, zoom in the Loop builder, fully refactored the existing Components feature, a revised design for sending preview emails and a new[/agents](https://loops.so/agents) page to showcase new and upcoming releases.
