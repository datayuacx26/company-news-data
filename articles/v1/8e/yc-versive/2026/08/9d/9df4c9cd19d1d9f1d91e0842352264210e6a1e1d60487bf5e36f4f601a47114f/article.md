---
schema_version: "1.0.0"
document_id: "9df4c9cd19d1d9f1d91e0842352264210e6a1e1d60487bf5e36f4f601a47114f"
company_key: "yc-versive"
company: "Versive"
source_id: "yc-versive-news-import-a5070f4382ee"
canonical_url: "https://getversive.com/blog/august-2026-product-updates"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-19T04:23:58.049058+00:00"
fetched_at: "2026-08-19T04:23:59.284627+00:00"
content_hash: "sha256:8f3cfe7041e3e5ea7151d0532e03d3f43c554c16ee7b3606c4b91779782c4ed0"
---

# What's new: conjoint analysis, default languages, and a faster Versive

[Back to updates](https://www.getversive.com/blog)


August 14, 2026 •


Product Updates


### What's new: conjoint analysis, default languages, and a faster Versive


A roundup of recent improvements: a new choice-based conjoint question type, default study languages, higher-accuracy re-transcription, smarter AI tests, big performance gains, and a new documentation site.


---


A roundup of improvements we've shipped across the platform over the past few weeks, headlined by a new question type for measuring what actually drives choice.


### Conjoint analysis


Ask people what they care about and they'll tell you everything matters. Ask them to *choose* between real options with real trade-offs, and you learn what actually drives their decisions. That's conjoint analysis, and it's now a native Versive question type.


Setup is simple: define your attributes and levels (say, price, support tier, and contract length), and Versive generates balanced choice tasks automatically, with no design expertise or external tools required. Participants compare concepts side by side, task by task, with optional randomization and a "None of these" option.


When responses come in, Versive estimates preferences for you:


- **Attribute importance** : which attributes drive choices most
- **Level utilities** : preference scores for every level, so you can see exactly how much appeal drops between price points
- **Choice shares** : how often each concept (and "None") was chosen, with sample-size guidance so you know when the results are solid


Everything exports to CSV and PDF. And because a conjoint question lives inside a Versive study, you can follow it with an AI question that asks participants about the trade-offs they just made: the numbers and the why, from the same session. This first release focuses on aggregate-level analysis; respondent-level utilities and market simulations are on our radar.


### Pick the language participants see first


For multilingual studies, you can now choose which language participants see by default, useful when your study is written in English but most of your audience isn't. Per-language links still work exactly as before, so you can send your French list a French link while everyone else starts in your chosen default.


### Re-run transcription when accuracy matters


For voice interviews, you can now re-transcribe a single message, or a whole interview, using our highest-accuracy transcription model. When a key quote is headed for a report, you can make sure it's exact.


### AI tests now handle Exploratory questions


Simulated participants in AI tests now hold full multi-turn conversations with[Exploratory questions](https://www.getversive.com/blog/july-2026-exploratory-questions) , the same way real participants do. Your dry runs now show you the actual back-and-forth the AI moderator will run, so you can tune the goal and turn budget before launch.


### Faster everywhere


We shipped a major round of performance work across the app: navigation, sign-in, and study pages make far fewer network round trips, results and transcripts load significantly faster, and pages show loading UI immediately instead of blocking. The gains are biggest for teams far from our servers, but everyone should feel the difference.


### A new home for documentation


Versive's documentation now lives at[docs.getversive.com](https://docs.getversive.com/) , including the full API reference and guides for studies, logic, and integrations. It'll keep growing alongside the product.


---


### Get in touch


We're always building. If you have any questions or feature requests, reach out to us at[\[email protected\]](https://www.getversive.com/cdn-cgi/l/email-protection#d5bdbc95b2b0a1a3b0a7a6bca3b0fbb6bab8) . Want to get started?[Sign up today](https://www.getversive.com/signup) and start running research in minutes.


---


Eric Li


, Co-Founder, Versive
