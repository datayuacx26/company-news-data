---
schema_version: "1.0.0"
document_id: "b7410da5889e60aeba01d0480c914b483df10e65422fee5a7e3acbe33975ec4b"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-30112aa6d3bf"
canonical_url: "https://tiptap.dev/blog/release-notes/ai-toolkit-now-in-beta"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-22T16:45:40.643427+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:783731f66c6c8399f0aa7971ccc79a3cf56020e0b25b00979722c239fb3ab6b0"
---

# AI Toolkit, now in beta – plus 100 free licenses

## Connecting Documents and AI


If you’re building document-AI experiences, then you know how hard it is to get the plumbing right. Asking an AI to rewrite something is easy; doing it *inside the document* might look simple at first, but between precise edits, change review, rich-text complexities, tables, images, streaming, and all the other details it gets challenging fast. Especially when we’re talking about collaborative rich text.


At Tiptap, we’ve spent the past year figuring out how to offer this difficult infrastructure as a platform—so our customers can focus on building their unique functionality rather than the difficult-but-uninteresting bridge between AI and editor. Last month, we[wrote about how it works](https://tiptap.dev/product-guides/tiptap-ai-toolkit-enabling-in-document-ai) . Today, I’m thrilled to announce a new beta version of our AI Toolkit, simpler and ready for production use.


## What’s this about free licenses?


Intrigued? Want to take AI Toolkit for a spin?[Just sign up for a Tiptap account and join our AI License Giveaway](https://cloud.tiptap.dev/waitlist/ai-toolkit-launch) : we’re selecting 100 sign-ups to win a free lifetime AI Toolkit license good for 100k tool calls a month. In exchange, we want to hear from you: your feedback will help define the next version of AI Toolkit.


## What is AI Toolkit?


[AI Toolkit](https://tiptap.dev/product/ai-toolkit) bridges the gap between your AI agents and your documents. With it, you inform your agent about the content and structure of a document, and enable the agent to make targeted edits that you can display as precise tracked changes in a[Tiptap Editor](https://tiptap.dev/product/editor) . It integrates with our Document Server and Collaboration service, so AI changes can appear in your users’ editors in real time. But it works server-side and doesn’t *require* the presence of a Tiptap Editor, so you can build long-running AI workflows that won’t disappear when the user closes the browser, or that can run overnight while users sleep.


AI Toolkit doesn’t *do AI* for you: we know you want full control and ownership of your agentic loop. Nor does it *require* a particular document-storage or -sync system (though it becomes more powerful when paired with our Document Server). Rather, AI Toolkit extends the same principle of *headlessness* that we bring to our Editor: You own your codebase, your AI, your infrastructure; we plug in flexibly, solving problems you don’t need to solve on your own without getting in the way.


## Why do I need it?


Suppose you’re building a platform to create and edit legal contracts. Users can call upon AI to assemble NDAs, employment agreements, privacy policies, and so on, and then work together with each other and with AI to review, revise, and finalize.


What does that mean functionally? With a traditional chatbot-based experience your users can say, “Review Section 3.1 and ensure it’s consistent with the rest of the contract.” The chatbot might ask a few questions, then come back with a revised section that you paste into the doc.


That’s awkward! Why can’t your user select the section instead of referring to it in words? Why must they paste the results back into the doc? And once they do, if the section is long, why make them hunt around for the bits that have changed in order to review them?


Here’s a better approach: your user selects the section, asks, “Ensure this is consistent with the rest of the contract,” and sees three precise, two-word suggestions they can accept or reject. Maybe the AI even leaves a comment explaining itself.


That improved experience is incredibly difficult to build from scratch. We know, because we’ve spent the past year building it so you don’t have to. That’s AI Toolkit.
