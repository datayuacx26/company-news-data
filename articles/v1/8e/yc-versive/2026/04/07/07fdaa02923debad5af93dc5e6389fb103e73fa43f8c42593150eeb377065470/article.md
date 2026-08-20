---
schema_version: "1.0.0"
document_id: "07fdaa02923debad5af93dc5e6389fb103e73fa43f8c42593150eeb377065470"
company_key: "yc-versive"
company: "Versive"
source_id: "yc-versive-news-import-a5070f4382ee"
canonical_url: "https://getversive.com/blog/april-2026-ai-test-improvements"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-22T18:46:56.164928+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:e485df6f0ca0254e78a85078b769fdeb8f253479e96ed847eae6b932441d4826"
---

# AI Test updates: synthetic users with SUS scoring and more

[Back to updates](https://www.getversive.com/blog)


April 28, 2026 •


Product Updates


### AI Test updates: synthetic users with SUS scoring and more


Synthetic users now run a built-in System Usability Scale survey, frame previews go full-screen during setup, and AI test runs are more reliable end-to-end.


---


A few updates to AI tests this month — most notably, you can now get a quantitative usability score out of every test run with synthetic users.


### Synthetic users with SUS scoring


You can now run a SUS (System Usability Scale) survey automatically on the synthetic users you generate for an AI test. SUS is the standard 10-question usability questionnaire that produces a 0–100 score — well-known, widely benchmarked, and useful for comparing designs against each other or against historical baselines.


- **Opt in to SUS** during AI test creation. The standard 10 SUS questions are pre-populated and editable
- **Auto-generated responses** at the end of every synthetic test run, so SUS becomes part of the same flow as the rest of the test
- **Aggregate scoring in the report** — the test results page surfaces the 0–100 score, per-question averages, and sentiment alongside everything else
- **Per-respondent detail** when you want to see how each synthetic user rated the experience


This is most useful when you want a quick directional signal on a design before putting it in front of real users. Run the AI test, get a SUS score, iterate, run it again, compare. The methodology matches a live SUS survey, so the numbers are comparable to the SUS scores you collect from real participants later.


### Full-screen frame previews


When setting up an AI test against Figma frames or uploaded images, you can now click any thumbnail to open it full-screen. Hover any frame to see the maximize button; press **Esc** to close.


Small but useful when you have several similar-looking frames in a test and need to confirm exactly which one you're attaching to a question.


### More reliable AI test runs


We've made AI test runs more resilient to upstream model issues. Runs now recover automatically from rate limits and transient errors instead of failing partway through. There's no UI for any of this — but if you've had test runs fail intermittently in the past, you should see fewer of those.


---


### Get in touch


We're continuing to build features that help you test and learn faster. If you have any questions or feature requests, reach out to us at[\[email protected\]](https://www.getversive.com/cdn-cgi/l/email-protection#6a02032a0d0f1e1c0f1819031c0f44090507) . Want to get started?[Sign up today](https://www.getversive.com/signup) and start running research in minutes.


---


Eric Li


, Co-Founder, Versive
