---
schema_version: "1.0.0"
document_id: "599b6afed79e19a9fa147bb0abaa7681fcd75ab57bb0779fa3be0d2b85a73ee2"
company_key: "yc-encord"
company: "Encord"
source_id: "yc-encord-news-import-59af355da1b0"
canonical_url: "https://encord.com/blog/not-a-data-problem-its-a-curation-problem/"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-21T18:06:14.355617+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:8aa4f038d51e14c9a7421ba6aae5512f67d086585d4bac475153a79728ef1dca"
---

# You Don't Have a Data Problem. You Have a Curation Problem

# You Don't Have a Data Problem. You Have a Curation Problem


[Saniya Patwardhan](https://encord.com/author/saniya-patwardhan/)


Encord Ambassador at Encord


July 10, 2026


|


5 min read


Summarize with AI


-
-
-


*"Do you think robots will be able to do my laundry?"*


It's a question I get asked all the time by people who are curious about my work in robotics.


Ironically, I had just spent weeks collecting more than 500 demonstrations to teach a robot a task that sounds almost trivial: picking up an object from a table.


That experience was a reality check.


If teaching a robot something this simple requires hundreds of carefully recorded demonstrations, what would it take for a robot to reliably do your laundry? Fold clothes? Sort colours? Handle socks tangled inside shirts? Recover when something unexpected happens?


My first instinct was the same one almost every machine learning team has at some point:


*"We probably just need more data."*


It sounds reasonable. Modern AI has been built on the idea that larger datasets create better models. For years, we've celebrated bigger benchmarks, larger web-scale datasets, and ever-growing training corpora.


But after working on[computer vision](https://encord.com/blog/what-is-computer-vision/) and[robotics](https://encord.com/robotics-and-humanoids/) projects, I've realised that this instinct is often wrong.


Most teams don't have a **data problem** .


They have a **[data curation](https://encord.com/blog/what-is-data-curation/) problem** .


**Read More:[Encord's Ultimate Guide to Data Curation](https://encord.com/blog/what-is-data-curation/)**


####
**Are you collecting more data or more information?**


There's a saying: *A lie repeated a million times does not become the truth.*


The same applies to machine learning. A demonstration repeated a thousand times doesn't necessarily make a model smarter. Once the model has learned the underlying pattern, additional near-identical examples add very little new information.


The question is no longer "How much data do we have?"


It's "What hasn't the model learned yet?"


That shift in thinking is where[data curation](https://encord.com/blog/what-is-data-curation/) begins.


Teaching a robot isn't like teaching a human. You can't show it one successful grasp and expect it to generalise. Every demonstration captures a slightly different reality: the object is rotated differently, lighting changes, the robot approaches from another angle, the gripper slips, or the object is partially occluded. Together, these examples help the model learn what matters and what doesn't.


But somewhere along the way, something interesting happens. The 450th demonstration rarely teaches the model as much as the 50th did. If those last 50 demonstrations all look nearly identical to what the model has already seen, they contribute very little new information. Yet they still cost time to collect, annotate, store, and train on.


If collecting more data isn't the answer, the obvious question becomes: **how do you decide which data is actually worth keeping?**


Imagine you're working with hundreds of thousands of images or thousands of hours of video. Before you think about annotation, you first need to understand what you already have. Which scenarios are common? Which are missing? Which examples are nearly identical?


This exploration stage is where semantic search becomes valuable. Rather than manually scrolling through a dataset, natural language and similarity search let you surface examples that match a concept or resemble a particular failure case. Instead of asking, *"Where is that one clip where the robot dropped the mug?"* you can actually find it.


[Encord](https://encord.com/) makes this practical through natural language and similarity search, allowing teams to explore massive multimodal datasets without manually scrubbing through every sample.


#### **How do you know what to prioritize?**


Finding interesting examples is only the beginning. The harder question is deciding which ones deserve annotation time.


Good curation should be driven by evidence, not intuition. Dataset metrics, class distributions, image quality statistics, and custom metadata can reveal patterns that aren't obvious from browsing samples alone. Rather than relying on a hunch, teams can quantitatively identify where coverage is weak or quality is poor.


Encord supports this through more than 40 built-in data quality metrics and metadata filtering, making it easier to prioritize samples that are likely to improve model performance.


#### **Where are your coverage gaps?**


One of my favourite ways to think about a dataset is as a map.


If every point on that map represents one sample, dense clusters often indicate redundancy, while isolated regions reveal rare behaviours the model rarely encounters.


Custom embedding visualizations make these patterns visible. Instead of asking whether your dataset is "large enough," you can ask a much more useful question: *Which parts of the problem space are underrepresented?*


Encord's custom embedding plots help surface these outliers and sparse regions, making it easier to identify the examples that will contribute the most new information.


#### **How do you close the loop?**


Once you've identified the valuable samples, the next step is surprisingly simple: separate them from everything else.


High-value edge cases, rare scenarios, and representative examples should move forward for annotation, while duplicates and low-quality samples should be left behind. Organizing these curated subsets before labeling ensures annotation effort is spent where it matters most.


Encord's Collections feature supports this final step by grouping high-value samples and removing redundant examples in bulk, closing the loop between data exploration and annotation.


So when people ask me whether robots will one day do our laundry, I still believe the answer is yes.


But after spending weeks collecting demonstrations for something as simple as grasping an object, I've realised the challenge isn't simply gathering more data.


It's making sure every new example teaches the model something it doesn't already know.


Bigger datasets don't necessarily build smarter robots.


Better[curated datasets](https://encord.com/blog/what-is-data-curation/) do.


[< Previous How to annotate video for machine learning: A Step-by-Step workflow](https://encord.com/blog/video-annotation-workflow/)[Next > AI Data Curation for LLM and Multimodal Teams: A Practical Framework](https://encord.com/blog/ai-data-curation-practical-framework/)


## Get the data right.


300+ of the best AI teams in the world use Encord.


[Take a tour](https://encord.com/explore-product/)[Book a demo](https://encord.com/book-demo/)
