---
schema_version: "1.0.0"
document_id: "f3e83b8f782f06bf9a8e35f6aa115463d2e1e28931c09a67b44c5a7b8f7f42d8"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/introducing-basedash-public-sharing/"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T05:03:18.489996+00:00"
fetched_at: "2026-08-19T05:03:23.435354+00:00"
content_hash: "sha256:ec6f6110e3905faea777c9a29d36e2dc6ea4f80cb0553c197f882cec660f8334"
---

# Introducing public sharing: live dashboards for anyone

Today we’re launching **public sharing** — a way to send a live Basedash dashboard or chart to anyone with a link, even if they don’t have a Basedash account.


Open **Share** , turn on public sharing, and copy the link. Send it to a client, an investor, a member of your leadership team, or a contractor. They open the dashboard in their browser and see the latest data, with the filters and table controls they need to explore it.


No invite. No new seat. No export to regenerate before the next meeting.


## One link instead of another account


The usual way to get a dashboard in front of someone outside your company is to make them part of your analytics setup. You invite them, explain where the dashboard lives, and hope they remember another password. Or you export a PDF and send a snapshot that starts going stale immediately.


Public sharing takes the shorter path. The URL carries a token that opens a standalone view of the dashboard. The viewer doesn’t enter your Basedash workspace, and they don’t need to create an account. They get the live result you chose to share.


## How it works


Sharing a dashboard takes three steps:


1. Open the dashboard and click **Share**
2. Turn on **Public link** and copy the tokenized URL
3. Send the link — the viewer opens it in any browser


You can also share at the chart level. If one number or table is all someone needs, open that chart’s share menu and send the chart instead of the full dashboard. Chart-level filters stay with the public view, so you don’t have to build a separate report just to share a focused slice.


The link keeps working as the underlying dashboard changes and its data refreshes. When you’re done sharing, turn public access off.


## The view stays interactive


A public link is not a screenshot. Viewers can use the filter dropdowns you added to the dashboard, including dropdowns backed by SQL or record data. They can switch a region, period, account, or any other visible variable and see the shared view update.


Public table charts can be sorted and rearranged too. A client can sort accounts by revenue, an investor can bring the fastest-growing segment to the top, or a leader can organize a long operating table for the question they’re answering. Those choices don’t rewrite the chart you built.


## Who public sharing is for


Public sharing works best when the audience is known but doesn’t belong in your Basedash workspace:


- **Clients** who need a live performance dashboard before a review
- **Investors or board members** who want to explore the numbers behind an update
- **Leadership** who needs a direct link to a focused operating view
- **Contractors and partners** who need one chart or dashboard for a project


Instead of copying numbers into a deck or onboarding every viewer, you send the view itself.


## Public sharing is not Embedding


Public sharing and[Basedash Embedding](https://www.basedash.com/blog/introducing-basedash-embedding) both put analytics in front of people without a Basedash account, but they solve different jobs.


**Public sharing** gives you a standalone URL. Use it when the action is “email this dashboard to this person.” It takes no code, lives in the viewer’s browser, and is ideal for a client report, investor update, board view, or one-off collaboration.


**Embedding** puts Basedash inside your own product. Use it when the action is “make analytics part of our app.” It uses an iframe and, for the full app experience, JWT-based sign-on and row-level security so you can serve many customers inside the product they already use.


If you’re sending a link, use public sharing. If you’re shipping analytics as a product feature, use Embedding.


## A note on what “public” means


Anyone who has the public URL can open it. The token makes the link difficult to guess, but it does not identify the viewer or replace permission rules for sensitive data. Filters on the shared view are exploration controls, not authorization controls.


Share only the dashboard or chart you intend that audience to see, and turn the link off when it is no longer needed. For a deeper look at choosing between public links, guest access, embedding, and static exports, read our[guide to sharing dashboards outside your company securely](https://www.basedash.com/blog/how-to-share-dashboards-outside-your-company-securely) .


## Getting started


1. [Sign up for Basedash](https://charts.basedash.com/signup) or log in
2. Open the dashboard or chart you want to share
3. Click **Share** , turn on **Public link** , and copy the URL
4. Send it to anyone who needs the live view


**Share a live dashboard today — no export, invite, or Basedash account required.**
