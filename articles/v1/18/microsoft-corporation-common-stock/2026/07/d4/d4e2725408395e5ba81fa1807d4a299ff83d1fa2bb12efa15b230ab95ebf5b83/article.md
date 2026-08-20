---
schema_version: "1.0.0"
document_id: "d4e2725408395e5ba81fa1807d4a299ff83d1fa2bb12efa15b230ab95ebf5b83"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0524baf4a828"
canonical_url: "https://microsoft.ai/news/optimizing-the-frontier-performance-curve/"
published_at: "2026-07-30T17:56:46+00:00"
first_seen_at: "2026-07-31T17:02:19.653982+00:00"
fetched_at: "2026-07-31T17:02:21.050165+00:00"
content_hash: "sha256:49548510f9fcf8ffc665572b7888ad7f286752f1b575bafd33ee9262b3dd8b04"
---

# Optimizing the frontier performance curve

Tokenmaxxing has been the story of the last few months, but token efficiency is the next big focus across the industry. How do we get the best possible performance per token invested, and the best real customer outcome per dollar invested?


To build a frontier firm, you have to optimize frontier performance against cost. Choosing where you want to sit on that curve is critical. By co-optimizing your models, harnesses, and RLEs you can pick a point on the curve that suits your firm.


In most cases, frontier generalist models aren’t necessary for every task. By tuning models for a specific product, you can maintain or even exceed frontier performance, while reducing token costs dramatically.


This is where we have focused our MAI hill-climbing machine over the last quarter, and the results are pretty cool. This week we released MAI-Cyber-1-Flash optimized for our MDASH harness.


Together, the system landed at **No.1** **on the leading CyberGym benchmark – beating Mythos by 12ppts – at 50% of the cost .** And remarkably, we serve it on H100s too.


It was designed to handle up to 90% of tasks efficiently, so that MDASH can reserve the largest and most expensive models in our fleet (in this case GPT 5.4) for the 10% of exceptionally hard problems that truly need them.


As Satya mentioned today in our Q4 Earnings call, since last quarter, we’ve shipped more than a dozen new models across image, voice, transcription, coding and security, and they’re already powering many of Microsoft’s most widely used products to maintain or improve quality while using significantly fewer tokens, in many cases saving 50-90% of GPU costs:


- We built MAI-Code-1-Flash hand-in-hand with our colleagues at GitHub, where **since June millions of developers have used it in their daily work. 10% higher code accept rate and 10% lower median token usage** than GPT-5.4 Mini and Claude Haiku 4.5 in VS Code, and already showing improved retention.
- We then trained that same checkpoint inside an Excel RL environment to achieve comparable performance to GPT-5.6 for the most common tasks while being more cost-efficient, and **small enough to serve on an A100 or H100 vs only the latest and most expensive accelerators.**
- MAI-Image-2.5-Flash, is now the end-to-end default in Bing Image Creator, in **production in PowerPoint where it is reducing GPU costs up to 84% compared with GPT-Image-2** , and is the default for key OneDrive editing scenarios, where it has increased save rates by 26% and delivers up to 2.5x greater token efficiency.
- **MAI-Voice-2-Flash now powers Dynamics 365 Contact Center, where customers like T-Mobile and EasyJet build their call center agents** , reducing GPU costs by up to 89%.
- MAI-Transcribe-1.5 now serves Dragon Copilot’s multilingual workflow across 58 languages — a solution used by 170,000 medical providers that processed 28 million patient encounters last quarter, where **our tests show a 50% relative in reduction transcription and language-identification error rates.**


And what’s more, by co-designing our models with our own silicon, we are seeing **40% better performance per watt running MAI models on Maia 200.**


But the benefit is not only cost. It’s resilience. Every business now must assume that any one model it depends on could disappear, through a security incident, a business or policy misalignment, or a geopolitical shift.


Every model in a product or agentic system should be substitutable, and that’s only possible when you build the harness, context, memory and action space independently of a single model family. That’s the hill-climbing machine we’ve built.


We think this is the beginning of a genuinely new performance curve. Its shape represents a system rather than a model, and traversing this curve delivers better quality, lower cost, and more choice.


This has been a summer of hard but wonderful work by the team. We are keenly aware of how early this is, and of how much we still have to learn. But the direction is clear, we are hill-climbing to move the frontier on the cost-to-outcome curve, and we will keep sharing what we learn along the way. There is much more to come.
