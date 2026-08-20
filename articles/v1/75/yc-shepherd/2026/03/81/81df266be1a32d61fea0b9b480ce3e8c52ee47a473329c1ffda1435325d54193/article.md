---
schema_version: "1.0.0"
document_id: "81df266be1a32d61fea0b9b480ce3e8c52ee47a473329c1ffda1435325d54193"
company_key: "yc-shepherd"
company: "Shepherd"
source_id: "yc-shepherd-news-import-bb872756570a"
canonical_url: "https://shepherdinsurance.com/blog/designing-nova-shepherds-ai-engine"
published_at: "2026-03-18T00:00:00+00:00"
first_seen_at: "2026-07-24T01:19:36.558879+00:00"
fetched_at: "2026-07-28T22:18:37.089432+00:00"
content_hash: "sha256:73a95098c659d0bc73f4d6c45a8d5b5ea55072a5de0834913d7f750908d53e31"
---

# Designing Nova: Shepherd's AI Engine

We built an AI that filled in every field correctly. Users still didn't trust it.


That failure taught us more about design than the AI did. But before we get into it, a quick overview of the domain we're working in.


Brokers represent businesses that need coverage.


Underwriters evaluate the risk, set a price, and negotiate toward a deal.


A submission is the package a broker sends to kick off that process, and it usually arrives as a mix of documents: financial data, property schedules, loss histories, incomplete forms. The underwriter's job is to extract what's relevant, price the risk accurately, and respond fast enough to win the deal.


The strategic part, the actual risk evaluation and negotiation, is where underwriters shine. The part before it, the data extraction and manual entry, was not a great use of anyone's time.


That's where Nova comes in. Nova is Shepherd's internal AI engine. It started as a chat assistant, but today it works throughout the entire underwriting platform, reading documents, extracting data, and preparing records before an underwriter even opens a submission.


Here's how we got there.


### **Accuracy isn't enough. You have to show your work.**


Our first target was structured data buried in unstructured files. Three inputs are critical to pricing a commercial insurance policy: payroll, subcontractor costs, and fleet schedules. They almost always arrive as Excel files with inconsistent headers, merged cells, hidden tabs, and formatting that varies wildly from broker to broker. Underwriters were spending hours on these alone, every day.


We built Nova to parse them automatically. In the first version, Nova extracted the data and populated the fields. It was correct roughly 80% of the time. Users still preferred to manually enter everything.


The problem wasn't accuracy. It was that the data appeared without explanation. No trail, no reasoning, no way to verify. So we redesigned around transparency: Nova now surfaces which files were processed, how columns were mapped, and where each value came from. The underwriter sees the reasoning, not just the result.


Trust followed. And once it did, we could move Nova's suggestions earlier in the flow until it became the default starting point instead of something to work around.


### **Earning our right to be the default.**


With spreadsheets in a good place, we expanded into quote inputs that directly affect pricing. Higher stakes, so we moved carefully.


The first version kept suggestions invisible until a user focused on a specific field. We tracked accuracy and adoption in the background without changing anything.


As both improved, we made suggestions more visible. Lighter placeholder text first. Then inline previews. Each increase in presence was tied to real performance data, not instinct. That discipline kept us from shipping something that looked confident before it had earned it.


### **When trust compounds, the scope of what's possible expands.**


Some fields eventually reached accuracy levels high enough to unlock something bigger.


We introduced Email Triage: a unified inbox built directly into the Shepherd platform. Underwriters select their assigned submissions, and by the time they open one, Nova has already parsed every attachment and prepared the relevant records. Policyholders and policies can be created right there, pre-filled and sourced, ready to review.


What used to be a scattered, multi-tool process became a single, fast, legible workflow. The underwriter stays in control. Nova just makes sure they're not starting from zero.


### **The best part of building here**


Our users are real experts with real consequences attached to their decisions. The feedback loop is tight. The domain complexity keeps you honest.


And because our users are internal, you get something rare: direct access to the people you're designing for. No research ops bureaucracy, no struggle for user feedback.


We're building AI tools that earn trust incrementally, explain themselves, and make hard work genuinely easier.


If it sounds like your kind of thing, we'd love to talk.[We're hiring across the board.](https://www.shepherdinsurance.com/careers)


‍
