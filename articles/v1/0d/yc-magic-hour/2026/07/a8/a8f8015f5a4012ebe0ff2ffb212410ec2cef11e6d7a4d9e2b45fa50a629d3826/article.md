---
schema_version: "1.0.0"
document_id: "a8f8015f5a4012ebe0ff2ffb212410ec2cef11e6d7a4d9e2b45fa50a629d3826"
company_key: "yc-magic-hour"
company: "Magic Hour"
source_id: "yc-magic-hour-news-import-988efd6b5de7"
canonical_url: "https://magichour.ai/blog/nano-banana-2-vs-nano-banana-pro-we-benchmarked-both-on-the-same-5-prompts"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-25T03:27:13.496224+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:17cec9a5c576fef8e022cbed7329727adf8eceae0cb2d5aab8f621708f8b779f"
---

# Nano Banana 2 vs Nano Banana Pro: we benchmarked both on the same 5 prompts

Google shipped Nano Banana 2 on February 26, 2026, and the name reads like it replaces Nano Banana Pro.


It doesn't.


"2" marks the newer generation, Gemini 3.1 Flash Image, whereas "Pro" marks a tier, Gemini 3 Pro Image, and Google keeps both models live.


So if you are adding image generation to a product, you now pick between two current models, and the name tells you nothing about which one your job needs.


We ran benchmarks and measured which model you should use. We sent the same five prompts to both models, three times each at 1K, through[Magic Hour's image API](https://docs.magichour.ai/introduction) : 34 generations in all, with wall-clock time and credits logged on every request.


The prompts and harness are in a[public repo](https://github.com/maniculehq/image-benchmark-mh) so you can rerun them. The rest of the piece maps those numbers to real jobs, down to the one-string change that switches between the models.


> Rerunning this benchmark costs about 4,250 credits. New accounts get 400 free credits plus 100 a day, and the code **FIRSTAPI** takes 10% off your first API credit purchase.[Create an account](https://manicule.link/mh-nanobanana) to try both models.


## How did we measure?


Five prompts, spanning the jobs an image API gets in practice: a photoreal street scene, a typography-heavy poster, a five-subject jazz-club composition, a watercolor children's-book illustration, and a detail-dense labeled cutaway diagram.


Each prompt ran three times per model at 1K resolution, 1:1 aspect ratio, one image per request, through[POST /v1/ai-image-generator](https://docs.magichour.ai/api-reference/image-projects/ai-image-generator) , with the models interleaved on every repeat and all requests run serially on one day. We also ran three Nano Banana 2 generations at 640px and one request with` model` left at` "default"` , bringing the total to 34. All 34 completed, with no failures and no refunds.


Generation on Magic Hour is asynchronous for every model: the create call returns a project ID, and the client polls[GET /v1/image-projects/{id}](https://docs.magichour.ai/api-reference/image-projects/get-image-details) until the status is terminal. Our harness polls every 2 seconds and measures wall-clock from just before the create POST to the first poll that returns` complete` , so every figure has roughly 2-second granularity and measures the Magic Hour path, queue plus render plus polling, not raw Gemini inference.


The create response carries` credits_charged` , deducted when the request is made and refunded if generation fails, so every cost figure was known before the image finished rendering.


The per-request log, 1K runs paired by prompt and repeat:


Prompt


Repeat


` nano-banana-2` (s)


` nano-banana-pro` (s)


photoreal


1


15.2


29.6


photoreal


2


27.3


25.6


photoreal


3


13.7


26.9


typography


1


15.1


31.3


typography


2


23.5


24.6


typography


3


12.4


41.6


five-subject


1


11.5


48.2


five-subject


2


15.8


25.0


five-subject


3


13.6


26.1


illustration


1


12.9


28.5


illustration


2


13.7


31.2


illustration


3


13.9


27.1


diagram


1


13.8


37.8


diagram


2


16.0


29.1


diagram


3


13.9


29.1


Every` nano-banana-2` row charged 100 credits and every` nano-banana-pro` row charged 150. The harness, the five prompts, the 34 output images, the raw response JSON, and the full log are in the[public benchmark repo](https://github.com/maniculehq/image-benchmark-mh) .


## How much faster is Nano Banana 2 really?


About twice as fast on identical prompts: Nano Banana 2 averaged 15.5 seconds per 1K image and Nano Banana Pro averaged 30.8 seconds, and Pro was slower in 14 of the 15 paired runs.


Model (1K, n=15)


Mean


Median


Min


Max


` nano-banana-2`


15.5s


13.9s


11.5s


27.3s


` nano-banana-pro`


30.8s


29.1s


24.6s


48.2s


The gap is consistent, not one slow outlier. The two distributions barely overlap: Pro's fastest run, 24.6 seconds, sits next to Nano Banana 2's slowest, 27.3 seconds. The single pair Pro won was that slow run, a photoreal repeat where Nano Banana 2 took 27.3 seconds against Pro's 25.6.


The ratio moves with the prompt:


Prompt


` nano-banana-2` mean (s)


` nano-banana-pro` mean (s)


Pro / NB2


photoreal


18.7


27.4


1.46x


typography


17.0


32.5


1.91x


five-subject


13.6


33.1


2.43x


illustration


13.5


28.9


2.14x


diagram


14.6


32.0


2.20x


The gap is narrowest on the photoreal scene (1.46x) and widest on the five-subject composition (2.43x), so a pipeline heavy on multi-subject or layout-dense prompts will see a wider gap than the 1.99x average.


Dropping Nano Banana 2 to 640px did not make it faster: the three 640px runs averaged 17.3 seconds, indistinguishable from its 15.5-second mean at 1K at this sample size.


## What does each image cost?


Through Magic Hour, Nano Banana Pro costs 150 credits per image and Nano Banana 2 costs 100, which at the Creator tier's $1.20 per 1,000 credits is $0.18 and $0.12. The per-image cost is flat: across all 33 Nano Banana generations in our benchmark,` credits_charged` was exactly 100 for Nano Banana 2 (at 1K and at 640px alike) and 150 for Pro, with zero variance.


Google's direct-API pricing changes with resolution. Its[pricing page](https://ai.google.dev/gemini-api/docs/pricing) puts Nano Banana 2 at $0.045 at 0.5K, $0.067 at 1K, $0.101 at 2K, and $0.151 at 4K, and Pro at $0.134 at both 1K and 2K, then $0.24 at 4K.


So "Nano Banana 2 is half the cost" is true only at 1K. At 2K the ratio falls to 1.33x, and Nano Banana 2's 4K image ($0.151) costs more than Pro's 1K image ($0.134). Pro's 1K and 2K images cost the same because both render as 1,120 output tokens.


Three practical limits change the arithmetic:


- Pro refuses anything below 1K. Our live probe got HTTP 422: "Model nano-banana-pro does not support 640px resolution. Supported: 1k, 2k, 4k". There is no cheap draft mode for Pro.
- Nano Banana 2 at 640px still charges the full 100 credits; each of our three 640px generations did. Below 1K you pay the same and wait the same.
- Subscription tier caps resolution independently of the model: Creator tops out at 1024px and 4096px needs the Business tier, so "supports 4K" in the model list also depends on your plan.


Our own measurements stop at 1K and 640px. Magic Hour's 2K and 4K credit costs depend on subscription tier, and our benchmark key's tier caps at 1024px, so the 2K and 4K figures above are Google's list prices, not our measurements. For Pro's pricing in more depth, see our[Nano Banana Pro pricing breakdown](https://magichour.ai/blog/nano-banana-pro-pricing) .


## Which model makes better images?


Neither model wins this cleanly. The two models differ in character more than in rank, with one exception: editing localization got worse in Nano Banana 2.


The character difference first.[fofr](https://www.fofr.ai/nano-banana-2-vs-pro) , a prompt engineer at Google DeepMind who published roughly 20 side-by-side comparisons, describes two aesthetics rather than a ranking: Pro renders muted, earthy, realistic images that avoid the AI look, while Nano Banana 2 is more vibrant and contrasty, "a little bit more AI-like, and less realistic".


His testing also found Nano Banana 2 better at one job: it returns clean design assets where Pro tends to return a photo of the design, with paper texture and borders.


The editing regression comes from[GenAI Showdown](https://news.ycombinator.com/item?id=47167858) , which scores image models on editing tasks: the quality leap from the original Nano Banana to Pro did not repeat for Nano Banana 2, in several cases it was substantially harder to stop the new model from changing the rest of the image, and localization of edits "seems to have changed and not necessarily for the better".


In the same thread, minimaxir found prompt adherence on trickier compositional prompts "much worse" than Pro; asked for a 5x2 grid built from 10 inputs, the model kept producing 4x3 grids with duplicates.


Google's own[Nano Banana 2 page](https://deepmind.google/models/gemini-image/flash/) lists the first-party limitations: it "can still struggle with small faces, accurate spelling, and fine details in images", and blending multiple images or making major lighting changes "may sometimes produce unnatural results".


Our benchmark's 15 prompt pairs, including the typography poster and the five-subject composition, are published unscored in the[benchmark repo](https://github.com/maniculehq/image-benchmark-mh) , one Nano Banana 2 image next to one Pro image per run, for your own judgment.


> **TODO (editor):** Any quality or prompt-adherence claim about our own 34 bench outputs needs human visual review of live-api/bench/images/ first; until that review happens this section publishes the pairs unscored and makes no claims about them.


## How do you choose for a real workload?


Three questions decide it, in order: what resolution do you ship, does the job need an exclusive, and can it wait twice as long?


Resolution comes first because it can end the decision by itself. Below 1K, only Nano Banana 2 exists, and Pro's 1K floor is enforced server-side. At 1K, Nano Banana 2 costs half of Pro on Google's prices and two-thirds through Magic Hour credits. At 2K the gap narrows to 1.33x on Google's prices, so price stops being a strong reason to pick Nano Banana 2.


Capability comes second. The job needs Pro if it uses interleaved text-and-image output, five character references, or style-reference images. It needs Nano Banana 2 if it uses image-search grounding, PDF input, or configurable thinking levels.


Latency and cost come last, as the tiebreaker. In our 1K runs, choosing Pro cost about 15 extra seconds and 50 extra credits per image, and in a batch pipeline those per-image costs add up with volume.


Three common workloads, run through those questions:


- A thumbnail pipeline at sub-1K resolution stops at the first question: Pro cannot render it. At 100 images a day, Pro would also have added about 25 minutes of wall-clock and 5,000 credits ($6 at Creator).
- A 2K hero-asset job favors Pro more often: the cost gap there is 1.33x, the volume is small enough that latency is irrelevant, and composition work is where Pro's character references and style references apply.
- An iterate-then-finalize loop uses both: iterate on Nano Banana 2 at 15 seconds a round, then re-run the final prompt on Pro only if the deliverable needs a Pro exclusive.


## What is the best way to use Nano Banana Pro and Nano Banana 2?


Through a unified API.[Magic Hour's image API](https://docs.magichour.ai/api-reference/image-projects/ai-image-generator) serves Nano Banana 2, Nano Banana Pro, and the rest of its model catalog through one endpoint, one request shape, and one credit balance, so using both models is not two integrations. Changing models is editing one line: the request body's` model` field.


The rest of the request body is identical for both models:` image_count` and` style.prompt` are required;` model` ,` resolution` , and` aspect_ratio` are optional. This is the benchmark's actual Nano Banana 2 request:


` POST /v1/ai-image-generator { "name": "bench-p1-r1-nb2", "image_count": 1, "aspect_ratio": "1:1", "resolution": "1k", "model": "nano-banana-2", "style": { "prompt": "A photorealistic street scene in Lisbon at golden hour: a yellow Tram 28 rounding a cobblestone corner, laundry hanging from wrought-iron balconies, long shadows, shot on a 35mm lens." } }`


The paired Pro request changed` "nano-banana-2"` to` "nano-banana-pro"` and nothing else.


Routing between Nano Banana variants is already how Magic Hour runs part of its own product. The Body Swap endpoint picks the model by resolution inside one endpoint: Nano Banana 2 Lite serves 640px and 1K requests, Nano Banana 2 serves 2K and 4K.


The pattern we recommend: default to` nano-banana-2` , and escalate to` nano-banana-pro` on an explicit rule, either the resolution you ship or a Pro exclusive the job needs. Each request routed away from Pro saves 50 credits and about 15 seconds. Both models are callable side by side on[Magic Hour's image generator](https://magichour.ai/models/nano-banana) .


> The code **FIRSTAPI** takes 10% off your first Magic Hour API credit purchase.[Sign up](https://manicule.link/mh-nanobanana) and both models are one` model` string away.


## Which model should you call?


The decision is one string in the request body's` model` field. Send` nano-banana-2` unless the job needs one of Pro's exclusive capabilities or accuracy matters more to you than waiting twice as long. Pro charges 150 credits per image to Nano Banana 2's 100, and the speed gap held across all five prompt types.


Your job


Call


Volume generation, iteration loops, tight latency budgets


` nano-banana-2`


Output below 1K


` nano-banana-2` (Pro's floor is 1K)


Image-search grounding, PDF input, or configurable thinking


` nano-banana-2`


Interleaved text-and-image output


` nano-banana-pro`


Five character references or style references


` nano-banana-pro`


Accuracy-critical composition that can wait twice as long


` nano-banana-pro`


## Does Nano Banana 2 replace Nano Banana Pro?


No. Nano Banana 2 (Gemini 3.1 Flash Image, launched February 26, 2026) is the fast model of the newer Gemini 3.1 generation; Nano Banana Pro (Gemini 3 Pro Image, built on Gemini 3) is the premium tier of the generation before it. Google keeps both live, retaining Pro for work where accuracy matters more than speed.


For API callers, nothing was replaced. The[Gemini API docs](https://ai.google.dev/gemini-api/docs/image-generation) list four coexisting Nano Banana models:


- Nano Banana 2 (` gemini-3.1-flash-image` ), the "generalist workhorse"
- Nano Banana Pro (` gemini-3-pro-image` ), the "premium choice"
- Nano Banana 2 Lite (` gemini-3.1-flash-lite-image` ), the fastest and cheapest
- Nano Banana (` gemini-2.5-flash-image` ), the 2025 original and the only one marked legacy


Our own API treats them the same way. The model enum in` src/types/v1-ai-image-generator-create-body-model-enum.ts` of our Node SDK lists all four as siblings:


` export type V1AiImageGeneratorCreateBodyModelEnum = // ... | "nano-banana" | "nano-banana-2" | "nano-banana-2-lite" | "nano-banana-pro" // ...`


## Appendix


The harness, the five prompts, the full per-request log, and all 34 raw API responses are in the[public benchmark repo](https://github.com/maniculehq/image-benchmark-mh) .
