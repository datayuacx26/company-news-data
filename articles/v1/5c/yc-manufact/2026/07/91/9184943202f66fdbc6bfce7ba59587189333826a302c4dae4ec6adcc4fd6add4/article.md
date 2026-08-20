---
schema_version: "1.0.0"
document_id: "9184943202f66fdbc6bfce7ba59587189333826a302c4dae4ec6adcc4fd6add4"
company_key: "yc-manufact"
company: "Manufact (formerly mcp-use)"
source_id: "yc-manufact-news-import-914a697bc101"
canonical_url: "https://manufact.com/blog/predictleads-mcp-marketplace-review"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-08-04T07:33:32.289519+00:00"
fetched_at: "2026-08-05T03:48:28.288743+00:00"
content_hash: "sha256:4d0b27655ff630d43a3a05336226d4d3d9d59efdd33894a8d56a1074c8dcbca7"
---

# How PredictLeads Avoided Weeks of MCP Marketplace Review

## Background


[PredictLeads](https://predictleads.com/) sells company intelligence with structured data on 120M+ companies: news, hiring signals, firmographics, and more. They were building an MCP server so agents in Claude and ChatGPT could pull that data directly.


OpenAI and Anthropic review submissions by hand, and neither tells you much when it rejects your MCP submission. A rejection lands in a queue with little detail, you guess at the cause, fix, and wait again. For teams, each blind round trip consumes weeks of time.


PredictLeads needed a way to know if their server was actually ready before submitting. Every tool worked: call it, get data back, no errors. But for a product built on rich, detailed data, it hid a real risk. Responses that looked fine in testing could be more than a model could actually handle in production.


## What the checks found


Using Manufact's[pre-submission checks](https://manufact.com/platform/publishing-checks) , the team found two problems that a normal test never did.


First, many tools declared no output schema. This meant clients like ChatGPT had nothing to validate results against, and nothing to reason about follow-up calls with. The tools passed every call and were still not ready.


Second, several tools returned responses too large for ChatGPT to actually use. If you call the tool and a valid response comes back, a normal check passes. Manufact also runs an end-to-end test, where a model actually consumes the tool's response and judges whether it can do anything with it. That's the test that caught this: the call worked, but the model couldn't read what came back, and in the app it simply fails.


The gap showed up before a reviewer ever saw it. Without it, this would've been a server that looked healthy on every call but broke the moment an agent tried to use it! Weeks of blind resubmission and waiting time avoided.


## The Result


PredictLeads is approved and their MCP is now live!


## The Takeaway?


Working tools are not a readiness signal. A tool call succeeding tells you nothing about whether a model can use the result once in the app store. And on marketplaces that review by hand, every defect you find before submitting can be weeks of time you gain back to focus more on your core product.


Congrats and thanks to the Predictleads team for building with us!


Thinking of shipping your own MCP server? Deploy your first server in under 60 seconds at[manufact.com](https://manufact.com/) , or[book a demo](https://manufact.com/book-call) and we'll help you map the fastest path to production agent traffic!
