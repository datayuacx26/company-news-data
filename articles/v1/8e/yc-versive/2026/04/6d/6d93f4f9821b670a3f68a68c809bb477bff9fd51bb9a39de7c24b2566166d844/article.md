---
schema_version: "1.0.0"
document_id: "6d93f4f9821b670a3f68a68c809bb477bff9fd51bb9a39de7c24b2566166d844"
company_key: "yc-versive"
company: "Versive"
source_id: "yc-versive-news-import-a5070f4382ee"
canonical_url: "https://getversive.com/blog/april-2026-study-improvements"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-22T18:46:56.164928+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:7c702842abbd78d2db943f8c002a4895974e1be59e1a4858bdedfefd5dd666b4"
---

# Study updates: bulk transcript exports, device checks, and wildcard embeds

[Back to updates](https://www.getversive.com/blog)


April 30, 2026 •


Product Updates


### Study updates: bulk transcript exports, device checks, and wildcard embeds


A bulk transcript download from the results page, a pre-interview device check for participants, multi-select question templates, wildcard embed domains, and a stack of prototype task fixes.


---


A roundup of improvements to live studies — from how participants enter an interview, to what they can embed it in, to how you get the data back out at the end.


### Bulk transcript download


The results page now has a one-click bulk transcript export. Previously, getting transcripts out meant downloading them one interview at a time.


- **Zip download button** sits next to the CSV export on the results toolbar
- **Markdown or Word** as the export format
- **Respects your active filters** , so the zip contains exactly the set of interviews currently visible in the table
- **Each file** has the same content as the single-transcript download: metadata, AI summary, durations, and the full Q&A transcript


This is the fastest path from a finished study to a folder of writeups you can share with a stakeholder, archive in your research repository, or feed into a separate analysis tool.


### Pre-interview device check


Voice and video interviews now start with a device check screen so participants can confirm their microphone and camera before the interview begins. This catches one of the most common causes of bad recordings — a wrong default device on the participant's machine — before they've spent time answering questions.


The screen lets participants:


- **Pick a microphone and camera** from the available options on their device
- **See live audio levels** with real-time warnings if input is too low to be usable
- **Recover gracefully** when the browser blocks access or no devices are available
- **See it in their language** — the device check is translated into every language Versive supports


For voice interviews especially, this should noticeably improve the share of interviews that produce a clean, transcribable recording.


### Multi-select question templates


Building studies from the Question Library is now multi-select. Pick the templates you want — selecting and deselecting cards as you go — and add them all in one click instead of opening the dialog once per question.


The library's empty states have also been improved, with clearer messages when a category, search, or your library itself has no templates yet.


### Wildcard embed domains


You can now allow an entire subdomain pattern to embed your studies. In **Embed Settings** , add an origin like` https://*.example.com` and Versive will accept any matching subdomain.


This is useful for teams using tools that serve content from many subdomains under one parent — Notion, Monday, Confluence, internal portals, multi-tenant SaaS apps. One wildcard entry covers them all.


---


### Get in touch


We're continuing to build features that help you test and learn faster. If you have any questions or feature requests, reach out to us at[\[email protected\]](https://www.getversive.com/cdn-cgi/l/email-protection#61090821060415170413120817044f020e0c) . Want to get started?[Sign up today](https://www.getversive.com/signup) and start running research in minutes.


---


Eric Li


, Co-Founder, Versive
