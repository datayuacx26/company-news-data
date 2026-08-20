---
schema_version: "1.0.0"
document_id: "6ab8e1c2cbcfe19eee943b65e010855febc79f8548a542e882853ea04623da6d"
company_key: "wayfair-inc-class-a-common-stock"
company: "Wayfair Inc."
source_id: "wayfair-inc-class-a-common-stock-news-import-42cff1750757"
canonical_url: "https://www.aboutwayfair.com/careers/tech-blog/how-we-used-ai-as-a-lab-assistant-to-run-110-experiments-in-three-days-and-cut-projected-tag-validation-cost-by-94"
published_at: "2026-06-15T14:30:00+00:00"
first_seen_at: "2026-07-22T19:30:59.923738+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:735fe40fc672d6c6a52a7ca20fec68948929c20277e59453e31c6abd1fa43280"
---

# How We Used AI as a Lab Assistant to Run 110 Experiments in Three Days and Cut Projected Tag-Validation Cost by 94%

Every Wayfair product is described by a large set of structured tags: material, shape, room, style, color, storage features, assembly details and 47,000 more. Those tags drive search, filtering, recommendations and merchandising. When a tag is wrong or missing, the customer experience degrades quickly. Products become harder to find, filters become less trustworthy and downstream systems make worse decisions.


Tag validation is the layer that checks structured catalog data against the richer evidence on a product page, including images, descriptions and other metadata.


We validate tags with a multimodal model. For each product, the model sees the evidence — images, descriptions and other metadata — alongside the tags being checked and their definitions. For every tag, it returns one of three verdicts: keep the current value, correct it to a specific new value or flag it as unsure, plus the evidence supporting that decision.


Our goal is to make this model cheap enough to run across a Wayfair-scale catalog while still finding corrections that are worth applying.


By late 2025, we had solved the problem of scaling accurate tag validation models across the Wayfair catalog, but rising costs had become the bottleneck. A 1M-product pilot using our existing setup was on track to cost roughly **$400K** . We needed a path to cheaper inference.


# Our December Hackathon Ran 110 Variants and Cut Projected Pilot Cost by 94%


Our December hackathon treated cost reduction as a systems problem. The team changed model choice, batching strategy, prompt structure, response schema, image selection and much more. That let us search for savings in the full architecture and compare whole-system designs instead of isolated tweaks.


Over three days, **five machine learning scientists** built and tested **110 different model variations,** exploring 20+ ideas including majority voting with cheap models, different base models across multiple vendors and different image selection strategies. The biggest unlock was in-prompt batching: evaluating multiple tags for the same product in a single call instead of repeating the same product context over and over. Combined with simpler prompts, a leaner output schema and tighter image controls, in-prompt batching moved us onto a very different cost curve.


The December timeline shows **110** systems through December 7, with the quality bar defined in terms of model F1, suggestion accuracy and deployment risk.


Our winning framework used **GPT-5-mini** . On the large test set, the comparison looked like this:


**Precision**


**Recall**


**Suggestion Accuracy**


**F1**


Production baseline
79%
67%
37%
73%


Winning model
80%
60%
46%
69%


Shipping the winning model helped us land a **94%** reduction in inference costs versus our original projection.


December changed the tag validation work stream in two durable ways:


- We found the lower-cost architecture we needed.
- We established a shared experimentation setup: standardized inputs and outputs, repeatable evaluation and a way to compare full-system variants quickly while code and results were being shared live across the team.


Our new framework made it easy to plug in a new validation approach, measure its quality and cost, and compare it quickly against the rest of the field.


Scientists could clone a system, and prompt their coding assistant to try a novel idea, rerun the evaluation loop and publish results to a shared dashboard. Coding assistants handled all the implementation work needed to spin up those variants, while the framework handled the data sampling, evaluation and metric reporting that made the comparisons trustworthy.


By the end of the hackathon, the workflow had settled into a clear pattern. Scientists spent most of their time brainstorming what to try next, reviewing results and deciding which ideas were worth another turn. Coding assistants and the framework took care of much of the mechanical work around implementation and measurement.


# In March, We Reused the Setup on Newer Models and a Better Objective


In March 2026, we ran a new hackathon, where we used a new version of the December framework to test newer models and new ideas.


This time, five scientists and one engineer tried 35+ novel ideas, including using generative AI (GenAI) to compress long definitions into shorter decision rules, changing in-prompt batching to include all product variants of a given product and creating stitched image grids from Wayfair videos.


A concrete March lever: compressing long tag definitions into concise decision rules that the model can execute with fewer tokens.


A stitched video grid can surface useful evidence quickly. In this example, the final frames show the product packed into a box, which suggests that assembly is required.


This chart shows **142** March runs through March 15, with the quality bar again defined in terms of model FR, suggestion accuracy and deployment risk.


The cheapest qualifying system came in at 10.6% of the March production baseline — a further 89.4% cost reduction on top of the December savings.


# What We Learned


The first lesson is that the biggest gains usually come from harnessing the power of AI in a controlled way — what we could call ‘evaluation discipline’. We were able to dramatically scale our experimentation capacity because we did the unglamorous work first: we scoped the problem, built a labeled eval set, locked a data contract and automated metrics reporting. That infrastructure made it safe to let AI agents explore the design space and measure every result. The AI was just one component. The science was everything else.


The second lesson is that reusable experimentation compounds. Once the team had a reliable way to compare variants, later hackathons could spend more time on genuine search and less time on setup.


The third lesson is that the right metric changes as a system matures. In December, cost reduction at reasonable quality was the urgent constraint. In March, the more useful question was how cheaply we could buy meaningful catalog corrections.


# Closing Thoughts & Next Steps


We’ve already taken steps to use the results of the March hackathon, which we will cover in a separate post.


We are now extending the same framework to explore open-source large language models (LLMs), including models such as Qwen, Gemma 4 and Llama models.


This new way to do science, building a harness that lets AI run hard while people steer the search, is what we want to keep pushing.


We're hiring! If this kind of work excites you, check out our[open roles](https://www.wayfair.com/careers/jobs?teamCategoryIds=6&teamIds=1) .


In alphabetical order, we’d also like to give special thanks to Ameya Jain, Ashequl Qadir, Brian Seaman, Deeksha Tiwari, Elizabeth Bui, Graham Ganssle, Jeff Arena, Trevor Truog and Vipul Dalsukrai for their contributions.
