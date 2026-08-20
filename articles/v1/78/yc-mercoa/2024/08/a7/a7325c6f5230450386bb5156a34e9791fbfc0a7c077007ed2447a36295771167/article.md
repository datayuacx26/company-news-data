---
schema_version: "1.0.0"
document_id: "a7325c6f5230450386bb5156a34e9791fbfc0a7c077007ed2447a36295771167"
company_key: "yc-mercoa"
company: "Mercoa"
source_id: "yc-mercoa-news-import-107a1845dd35"
canonical_url: "https://mercoa.com/blog/building-invoice-ocr-in-three-days-with-type-safe-llm-calls"
published_at: "2024-08-09T00:00:00+00:00"
first_seen_at: "2026-07-22T04:05:05.455716+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:a4a60fad9c50cef9fc0f94dbb0bd965c88387fb1ccb4bf25df00229f798486af"
---

# Launching invoice OCR in three days with type-safe LLM calls

# Launching invoice OCR in three days with type-safe LLM calls


How I deleted over a thousand lines of code to make a better product


Ashwin Kumar - August 9, 2024


When I started my internship at Mercoa, one of my first questions for the CTO was how invoice OCR worked. I was impressed by how well I’d seen it work in demos, and customers gave it good reviews – which made the Rube Goldberg machine-esque backend code he showed me especially jarring to find.


Invoice OCR needs to pull a bunch of data from pictures of invoices, some of which needs to be defined at runtime. It turns out that multimodal LLMs are actually smart enough to do this by receiving an invoice image and a text list of the fields to pull – but this makes for some truly unmaintainable, hard-to-read code like what you see above.


In fact, I was surprised that this worked at all. The LLM’s response to this is necessarily a blob of text that the rest of invoice OCR assumes is a correctly formatted JSON object. However, most LLMs aren’t that smart yet, which is why custom handlers fixing the LLM’s mistakes littered the business logic after it received the LLM’s response. For example, the LLM sometimes randomly chose to use different field names (“description” instead of “des”, or “quantity” instead of “qty”, etc. – seePrompt engineering with types for more details), which would break the OCR pipeline.


I expected this to come with severe costs in consistency and accuracy.


But at the end of the day, the invoice OCR did work, and rather well too! That is, only after 6 months of fixing production issues to get the prompts and parsing logic right.


During my internship, I had the opportunity to build and deploy several AI features to customers. To build them, I did some research and found a tool called BAML, a domain-specific language for writing LLM functions. After having amazing success using it for the invoice line item classifier and an email invoice detector, I suggested using BAML to rewrite our invoice OCR pipeline to the team.


In less than 3 days, I was able to replace 6 months of custom parsing logic with a better, faster, and more maintainable invoice OCR pipeline.


## What is BAML?


BAML is a domain-specific language for prompting LLMs created by[Boundary](https://www.boundaryml.com/) . Rather than manually importing and calling OpenAI’s SDK (or Anthropic’s SDK, or Gemini’s SDK, etc…), BAML enables developers to define LLM-powered functions with customizable prompts and return types. These functions can be called as if they were regular TypeScript functions – that is, with the confidence that the LLM will never break my return type.


We chose to use BAML for two reasons:


-


Good developer experience


-


Type safety


These are two big reasons I’ve been able to ship AI as fast I have. At first glance, this might look like the typical pitch for why TypeScript is better than JavaScript or why Rust is a great programming language – but it’s more than that. (To be clear, I also believe both of these things, but that’s a different discussion.)


AI hallucinates, is sensitive to prompts, and provides many ever-changing options for models to use. In this environment, not having a good DX or strong typing is especially painful. At Mercoa, we learned that the hard way by spending 6 months building and perfecting our old invoice OCR pipeline.


Ultimately, rewriting OCR with BAML eliminated that tech debt for future AI integrations, and actually resulted in a better pipeline! My main takeaways from rebuilding with BAML fall under the following categories:


-


Vendor portability


-


Prompt engineering with types


-


Unpredictable failure modes


-


Response quality + latency optimization


## Vendor portability


When I first looked at the old invoice OCR pipeline running on GPT4o, I found commented-out lines of code referencing an \`ANTHROPIC_KEY\` near our OCR business logic. Turns out, this was leftover code from an experiment the CTO ran a few months ago between OpenAI and Anthropic models for OCR – he hadn’t touched it in a while, but left the code in so it would be easier to experiment again.


Every API is structured slightly differently, which makes using a model slightly sticky for users who have to invest engineering effort to switch models. It may not be a lot of engineering effort, especially if you have well-modularized and clean code – but setting that up in the first place still takes time.


Instead, I offloaded model selection to BAML’s[LLM Clients](https://docs.boundaryml.com/docs/snippets/clients/overview) . After completing the pipeline, it took 15 minutes of testing followed by a two-line PR to migrate from GPT4o to Claude 3.5 Sonnet. Now that GPT4o cut their prices in half, it will take another two-line PR to switch back to GPT4o if I decide to. Mercoa now requires zero engineering effort to switch models, which is valuable in a world where models improve weekly (and where I have better things to do).


## Prompt engineering with types


BAML’s guarantee that LLMs never break the return type enables it to strongly couple types and prompts. This is nice to have for writing prompts, but I found it was really helpful for advanced prompt engineering – in my case, using[descriptions and field name aliases](https://www.promptfiddle.com/Aliases-and-descriptions-Au7_s) .


Tooling matters a lot for prompt engineering: just look at the old type versus the BAML type for representing OCRed invoice line items!


Aside from removing redundant field name spellings, I used the @description decorator to “nudge” the LLM to format fields correctly, and used @alias to shorten the text the LLM needs to respond with by shortening the field names.


Try out BAML’s prompt engineering yourself at[promptfiddle.com](https://www.promptfiddle.com/) !


## Unpredictable failure modes


Reasoning about an LLM function’s edge cases feels very different from a traditional function, even with BAML’s return type guarantees. As dystopian as it sounds, the best mental model I could construct while debugging the LLM calls was to treat the LLM like a human. I would ask myself: What mistakes would a human make if I gave them this task?


One surprising edge case that BAML abstracted away was parsing floating point numbers. With the old pipeline, most invoices we processed were from the US and gave us no trouble parsing numbers. Then, one day we received a European invoice that swapped the meaning of the decimal point and the comma for some numbers.


So, GPT4o dutifully returned all our floating point numbers with the European convention, immediately breaking our entire OCR pipeline.


Unfortunately, even BAML can’t catch every failure mode. In my MVP, I hoped that a single LLM call would be enough to parse an invoice of any length, but quickly found that this wasn’t the case because the LLM got lazy, and gave up on parsing the entire document. In human fashion, giving it progressively more stern instructions to parse the whole document actually made the LLM parse 20-30 more line items, though it never parsed the entire thing.


(Disclaimer: we kept our threats tame for reasons of self-preservation during the singularity.)


Think of BAML as autocorrect for LLMs – BAML can fix the low-level mistakes, but LLMs are still tricky to coerce into doing what you want.


## Response quality + latency optimization


My top priority past getting the OCR pipeline to work was making sure I didn’t degrade performance by switching to BAML, especially on my constrained timeline. While I sadly didn’t have a better way to test for response quality besides manually uploading a bunch of invoices, here’s what I found:


-


Every model does poorly with too much input information


-


Descriptions were consistently effective over every model


-


Aliases only worked well with smarter models (GPT4o mini was much worse)


-


Poorly formatted invoices were only consistently parsed well by smarter models


-


Prompt micro-optimizations for GPT4o mini did not work


-


GPT4o and GPT4o mini surprisingly took the same amount of time to run


-


Claude 3.5 Sonnet was significantly faster than GPT4o


-


~35 seconds for a long invoice instead of ~45-60 seconds


Switching our line item parsing to individually parse each page of long invoices in separate LLM calls fixed the laziness issue completely – in fact, making multiple LLM calls simultaneously for each page was actually faster than making a single LLM call!


After this, I observed that model intelligence had a huge impact on OCR quality that prompt engineering couldn’t mitigate. Not only did GPT4o mini fail on poorly formatted invoices, it couldn’t use field name aliases without putting information under the wrong fields if the alias itself wasn’t descriptive of the field’s meaning.


Finally, my initial reasoning behind using shorter field name aliases was that the LLM would run faster by having less output characters to generate. However, this is simply not how LLMs work – both “q” and “quantity” are internally represented by one token, so there’s no difference to the LLM. You can check this yourself with[OpenAI’s tokenizer](https://platform.openai.com/tokenizer) – while OpenAI hasn’t released the tokenizer for 4o and 4o mini, it’s safe to assume it isn’t drastically different.


This is a great example of why understanding how AI works under the hood isn’t just a theoretical exercise, but has practical implications in day-to-day ML engineering. I suspect we still see latency improvements with field name aliases because the actual response payload is smaller and faster to send over the network, but I’m not sure – I’d love to see someone test this!


## How can I get started?


Glad you asked – if you want to build this yourself, check out Boundary’s[website](https://www.boundaryml.com/) to get a feel for using BAML! With multimodal LLMs and tech like BAML readily available to anyone, OCR is easier than it has ever been.
