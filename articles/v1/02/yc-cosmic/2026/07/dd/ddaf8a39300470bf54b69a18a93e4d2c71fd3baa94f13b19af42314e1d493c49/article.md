---
schema_version: "1.0.0"
document_id: "ddaf8a39300470bf54b69a18a93e4d2c71fd3baa94f13b19af42314e1d493c49"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/ai-content-pipeline-claude-headless-cms"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-08-01T01:48:32.014309+00:00"
fetched_at: "2026-08-01T01:48:34.118909+00:00"
content_hash: "sha256:6d96bce0b33088082848b09f417e56e88dc5d1f53c44bda60d74b7dff5a75c18"
---

# How to Build an AI Content Pipeline with Claude and a Headless CMS

Most "AI content pipeline" tutorials hand you a second vendor. You install an AI provider's SDK, manage a second API key, absorb a second bill, and then write glue code to move the output into wherever your content actually lives.


Cosmic generates text, images, and video through the same API that stores your content. One SDK, one key, one bill. This guide builds a working pipeline on that API: a source document goes in, a reviewed draft comes out, and a human approves it before anything reaches production.


Everything below uses the official TypeScript SDK, . No second AI provider required.


## What we are building


A content pipeline has four stages, and the third one is the reason the other three are worth automating.


1. **Source.** The raw material: a brief, a transcript, a quarterly report, a spreadsheet of survey results.
2. **Generate.** Turn the source into a structured draft.
3. **Review.** A human reads it, edits it, and decides whether it ships.
4. **Publish.** The approved draft goes live through your existing front end.


Skip stage three and you are running a slop factory. The pipeline below treats the review gate as load-bearing infrastructure, not an afterthought.


## Prerequisites


- A Cosmic account. The[Free plan](https://www.cosmicjs.com/pricing) includes 1 Bucket, 2 team members, 1,000 Objects, and a monthly AI token allocation, which is enough to build and test this end to end.
- Your Bucket slug, read key, and write key, from **Settings → API Access** in your Bucket.
- Node.js and a package manager.


A write key is required for every AI operation. Keep it server side only.


## Step 1: Install the SDK and create a client


```text


```


```text


```


That is the entire dependency list. The same client reads content, writes content, and generates it.


## Step 2: Generate your first draft


takes a prompt and returns the generated text plus token usage.


```text


```


The object matters more than it looks. It is how you attach a real cost figure to every piece of content the pipeline produces, which we come back to below.


If you prefer HTTP directly, the same call over REST:


```text


```


## Step 3: Feed it a real source document


This is the step that turns a prompt toy into a pipeline. The parameter points at any file in your Bucket and the model analyzes it as part of the request. Images, PDFs, Excel spreadsheets, Word documents.


Upload a quarterly report to your Bucket, then generate from it:


```text


```


The grounding this gives you is the whole point. A model working from a prompt alone invents plausible numbers. A model working from your PDF is constrained by a document you control, and the instruction to use only stated figures becomes checkable: a reviewer can open the same PDF and verify every claim.


The same parameter works with the chat format when you need multi-turn refinement over one source:


```text


```


Useful source documents for a content team: customer interview transcripts, support ticket exports, release notes, analytics CSVs, competitor pricing pages saved as PDFs.


## Step 4: Write the result into the CMS as a draft


Generation and storage are the same client, so this is one call and no glue code.


```text


```


The line is the pipeline's safety property. Every generated object lands in a state that is invisible to your production front end until a person changes it.


## Step 5: Make the review gate real


A review gate that depends on someone remembering to check a folder will fail. Two things make it hold.


**Query the queue.** Anything awaiting review is one request away:


```text


```


Wire that to a Slack message on a schedule and the queue comes to your team instead of waiting to be discovered.


**Publishing stays a human action.** The approval step is a deliberate status change, made by a person:


```text


```


Keep that call out of your generation script entirely. Put it behind an editor's click in the dashboard, or behind an internal tool that requires a named approver.


## Step 6: Pick a model, and keep the ability to change it


The parameter defaults to . It also accepts Gemini models such as , OpenAI models such as , and Moonshot Kimi models such as .


```text


```


Switching providers is a one-line change with no new dependency, no new key, and no new billing relationship. That matters because the model you pick today will be superseded. A pipeline that pins itself to a single vendor's SDK has to be rebuilt every time the frontier moves. See[why your AI stack should be model-agnostic](https://www.cosmicjs.com/blog/why-your-ai-stack-should-be-model-agnostic) for the longer argument.


A practical split for a content pipeline: a cheaper Budget-tier model for mechanical work like extracting bullet points or drafting meta descriptions, and a Standard-tier model for prose that a reader will actually see.


## Step 7: Generate the featured image in the same pipeline


The AI API also generates images and video, so the featured image does not have to be a manual handoff to a designer or a trip to a stock library. Image generation is billed as a fixed token cost per image: a DALL-E 3 image is 4,800 output tokens, a Gemini 1K or 2K image is 32,160, and a Gemini 4K image is 57,600. Videos through Veo cost considerably more, from 144,000 tokens for a 4-second fast render.


See the[AI API reference](https://www.cosmicjs.com/docs/api/ai) for the image and video request formats. Generated media lands in your Bucket's media library, so you can attach it to the draft object in the same run that created the draft.


## Step 8: Stream when a human is watching


Batch jobs do not need streaming. Editor-facing tools do, because a blank screen for twenty seconds reads as a broken feature.


```text


```


## Step 9: Give the pipeline memory


Generation is half of a useful pipeline. Retrieval is the other half. Before drafting a new post, check whether you already published one on the same topic, and feed the existing coverage into the prompt as context.


Cosmic includes semantic search over your Bucket content, which finds objects by meaning rather than keyword match. That is how you stop a pipeline from producing four posts that compete with each other for the same search query. See the[semantic search docs](https://www.cosmicjs.com/docs/api/content-rag) for the query format.


## The no-code path: connect Claude directly to your CMS


If you want an AI assistant operating on your content interactively instead of a scripted job, use the Cosmic MCP server. It exposes your Bucket as tools that Claude Code, Claude Desktop, or Cursor can call directly: read objects, create drafts, upload media, generate content.


The read-key-only configuration is worth knowing about here. Point the MCP server at a read key and every create, update, and delete tool is blocked, which gives you an assistant that can analyze your content library and propose changes without the ability to write anything. Full setup is in[connect Claude Code to your CMS with MCP](https://www.cosmicjs.com/blog/connect-claude-code-to-cms-with-mcp) and the[MCP server docs](https://www.cosmicjs.com/docs/mcp-server) .


## Guardrails that actually matter


Six rules, learned the practical way.


1. **Never publish from an automated step.** Generation writes drafts. Only a person changes status to published.
2. **Ground every factual claim in a source document.** Use and instruct the model to use only stated figures. Claims a reviewer cannot trace to a source get cut, not softened.
3. **Cap per job.** It is your circuit breaker against a runaway loop consuming a month of allocation.
4. **Log on every call.** Store input and output tokens alongside the object so you can attribute cost per piece.
5. **Keep write keys server side.** Every AI operation needs one. A write key in client code is a public write key.
6. **Deduplicate before you generate.** Run a semantic search first. Two posts targeting one query split their own rankings.


## What this costs


AI usage draws tokens from your plan allocation, and text generation applies a tier multiplier to the actual tokens used.


- **Budget, 1.0x:** GPT-5 Nano, GPT-5 Mini, Claude Haiku 4.5. 1,000 actual tokens deducts 1,000.
- **Standard, 2.0x:** GPT-5, GPT-5.2, GPT-5.2 Codex, GPT-5.5, Claude Sonnet 4.6, Claude Sonnet 5, Claude Opus 4.7, Claude Opus 4.8, Gemini 3.1 Pro, Kimi K3. 1,000 actual tokens deducts 2,000.
- **Premium, 4.0x:** Claude Fable 5. 1,000 actual tokens deducts 4,000.


Media is a fixed cost per asset, billed as output tokens, with the image and video figures listed in Step 7.


Plan pricing, current at time of writing: Free at $0/month, Builder at $49/month, Team at $299/month, Business at $499/month, and Enterprise on custom pricing. Additional team members are $29/user/month. Token packs are available if you exceed your monthly allocation. Check the[pricing page](https://www.cosmicjs.com/pricing) for current allocations and pack sizes.


## Measuring whether the pipeline is working


Volume is the wrong metric. A pipeline that triples output and halves engagement has made things worse. Track these instead:


- **Approval rate.** What share of generated drafts a human ships without a rewrite. A low rate points at your prompts and source documents, not the model.
- **Editing distance.** How much reviewers change before publishing. Rising distance means quality is drifting.
- **Cost per published piece.** Logged divided by pieces that actually shipped, counting the drafts you threw away.
- **Engagement on generated pieces versus hand-written ones.** The honest test.


## FAQ


**Do I need my own Anthropic or OpenAI API key?**


No. Text, image, and video generation run through the Cosmic API using your Bucket write key. Model access is included in your plan allocation.


**Which models are available?**


Claude, Gemini, OpenAI, and Moonshot Kimi models, selected with the parameter. The default is . The full list is in the[AI API docs](https://www.cosmicjs.com/docs/api/ai) .


**Can the AI read my existing files?**


Yes. Pass pointing at any file in your Bucket: images, PDFs, Excel spreadsheets, Word documents, and more.


**Does Cosmic have a GraphQL API?**


No. Cosmic provides a REST API and the JavaScript/TypeScript SDK, .


**Can I run this without writing code?**


Yes, through the MCP server or the AI features in the dashboard. The code path gives you scheduling and repeatability.


**Will generated content hurt my SEO?**


Unreviewed content will. The review gate in Step 5 exists for that reason. Publish what a human has verified and cut what nobody can source.


## Ship the boring version first


The smallest useful pipeline is one source document, one prompt, one draft object, one reviewer. Build that, run it for a week, then add scheduling and images and retrieval once you trust the output.


This is the outcome teams are after when they automate content operations. As Maximilian Wuhr, Co-Founder at FINN, put it: "Cosmic is: us never having to ask a developer to change anything on the backend of our website."


[Start building for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=conclusion-signup-cta) with AI generation included on every plan, or[book 20 minutes with our CEO](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=conclusion-demo) to talk through your content workflow. If you want the interactive version instead, wire up[Cosmic AI agents](https://www.cosmicjs.com/ai/agents) or the[MCP server](https://www.cosmicjs.com/docs/mcp-server) and start from a conversation.
