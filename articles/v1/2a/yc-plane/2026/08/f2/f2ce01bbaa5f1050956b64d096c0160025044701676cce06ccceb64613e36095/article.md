---
schema_version: "1.0.0"
document_id: "f2ce01bbaa5f1050956b64d096c0160025044701676cce06ccceb64613e36095"
company_key: "yc-plane"
company: "Plane"
source_id: "yc-plane-news-import-c2c9290ea736"
canonical_url: "https://plane.com/blog/spreadsheet-free-way-to-check-pto"
published_at: "2026-08-19T22:16:45.862+00:00"
first_seen_at: "2026-07-31T23:58:24.462445+00:00"
fetched_at: "2026-07-31T23:58:25.366995+00:00"
content_hash: "sha256:c2ee663d0dbee0b208e7eb2545dc4f2d53fbeb85c82663047091430245b01c1d"
---

# The spreadsheet-free way to check your team’s PTO

Checking on PTO usually means opening a tracker, pulling a report, or building one from scratch. If you’re a founder or Head of People, you’ve got better things to do.


Let Claude or ChatGPT handle it for you with Plane's MCP server. Ask a plain language question like, "Who has barely taken any vacation this year, and who is nearly out?" and get an instant answer with:


-


Days taken per person


-


Vacation balance: entitled, used, and available


-


How the picture breaks down by team


The data comes from the same place as payroll. Nothing to export, nothing to reconcile.


Dig deeper by breaking the numbers down by department. Then draft a Slack message to anyone who's overdue, without leaving the conversation. Here are the steps to follow:


#### Setting up your MCP connection


Start by connecting your AI app to Plane. For MCP-compatible clients, add Plane as a remote connector using the link below. For more detailed instructions, check out our[MCP guide](https://docs.plane.com/guides/mcp) .


```text


```


```text


```


```text


```


```text


```


#### Step 1: Pull the company-wide time-off view


Ask Plane to list everyone currently on your team along with their vacation balance.


```text


```


```text


```


```text


```


```text


```


> 💡 **Tip:** "Who currently works here" matters more than it sounds. Plane keeps the numbers for people who've already left, so leaving that out can put a departed employee on your overdue list. If you track "paid time off" instead of, or in addition to, "vacation" time, specify which one you want analyzed.


#### Step 2: Break it down by department


See which teams are carrying the most unused time off, and which are keeping pace.


```text


```


```text


```


```text


```


```text


```


#### Step 3: Turn the read into action


Once you know who's overdue, draft the nudge without leaving the conversation.


```text


```


```text


```


```text


```


```text


```


#### Step 4: Review and send


If your AI app is connected to Slack, you can ask it to send the note individually to each person, once you've confirmed who's on the list.


```text


```


```text


```


```text


```


```text


```


> 💡 **Tip:** Confirming the send list first means nothing goes out that you haven't reviewed. You approve, Plane executes.


## What you'll need


-


A Plane workspace with your people, their departments, and recorded time off


-


Admin access in Plane


-


An MCP-compatible AI client (e.g., Claude or ChatGPT) connected to Plane
