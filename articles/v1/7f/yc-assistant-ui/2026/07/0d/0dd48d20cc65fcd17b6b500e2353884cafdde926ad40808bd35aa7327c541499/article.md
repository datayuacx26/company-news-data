---
schema_version: "1.0.0"
document_id: "0dd48d20cc65fcd17b6b500e2353884cafdde926ad40808bd35aa7327c541499"
company_key: "yc-assistant-ui"
company: "assistant-ui"
source_id: "yc-assistant-ui-news-import-09747b4cbd06"
canonical_url: "https://www.assistant-ui.com/blog/2024-07-29-hello"
published_at: "2024-07-29T12:00:00+00:00"
first_seen_at: "2026-07-21T08:04:45.708370+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:c19b601a085750d8664cd4a7a6f8ded0d24ba9a5f223d67890cdc85436aa0368"
---

# Hello, assistant-ui

After spending the last two years building Gen-AI prototypes and products, I realized that all software will soon have a natural language interface. OpenAI's ChatGPT sets the quality bar high and I wanted to have a similar UX in my own apps. No solution on the market came anywhere close, so I built assistant-ui.


assistant-ui is an open-source, embeddable, customizable AI chat component for React web apps. You can use it to build GPT wrappers, in-app copilots, or agentic systems. It supports rich content (markdown, code highlighting, charts, tables), generative UI, message editing, ..., across all the major model providers.


Earlier last month, I released the project to the world. The feedback from early adopters has been overwhelmingly positive:


[@raw_works i needed this 7 months ago!](https://x.com/raw_works/status/1797111840188809472)[@eliasdevs I was able to get it into production in 2 hours](https://x.com/eliasdevs/status/1800691268194013219)[@manuuonly was just thinking of researching a tool that could help me do this. awesome, great product!](https://x.com/manuuonly/status/1797511225523454243)


It didn't take long for developers to start building with assistant-ui. Here are some of my favorite products from the community:


- [Helicone](https://helicone.ai/) - open-source LLM observability platform
- [screenpipe](https://screenpi.pe/) - AI to remember everything you see, say or hear
- [myresume.guru](http://myresume.guru/) - AI resume optimizer to land your dream job


With 250+ stars on[GitHub](https://github.com/assistant-ui/assistant-ui) and over 1k[npm](https://www.npmjs.com/package/@assistant-ui/react) weekly downloads, we got a vibrant growing community of developers who care about the user experience.


I want to thank the following contributors who got the project to where it is today:


- [@m13v](https://github.com/m13v) for contributing two examples and several how-to videos
- [@okisdev](https://github.com/okisdev) for multiple bugfixes
- [@Rajaniraiyn](https://github.com/Rajaniraiyn) for improving streaming support for REST APIs
- @ccbkai for integrating Chrome's` window.ai` with assistant-ui
- [@stingfeng](https://github.com/stingfeng/dify-extensions) for building a[Dify](https://dify.ai/) integration
- [@Ephibbs](https://github.com/Ephibbs/flowtoken) for building a Perplexity-style text streaming library ([Demo](https://assistant-ui-flowtoken-demo.vercel.app/) )
- … and so many others


assistant-ui builds on top of[Radix UI](https://www.radix-ui.com/) ,[Tailwind](https://tailwindcss.com/) ,[shadcn/ui](https://ui.shadcn.com/) ,[Vercel AI SDK](https://sdk.vercel.ai/docs/introduction) and other amazing open-source projects.


Over the coming month, I'll be rolling out multimodal support, so you can use your voice and upload images. There's so much more in the pipeline that I can't wait to unveil!


Today, several companies like[Helicone (YC W23)](https://x.com/justinstorre/status/1816849882612904156) are using assistant-ui to power a core part of their product. If your company wants to do the same, please[get in touch](https://cal.com/simon-farshid/assistant-ui) .


If you're building conversational AI, join us on[Discord](https://discord.gg/S9dwgCNEFs) . If you're technical and want to shape how we interact with AIs of the future,[send me an email](https://www.assistant-ui.com/cdn-cgi/l/email-protection#4e3d272321200e2f3d3d273d3a2f203a633b27602d2123) .
