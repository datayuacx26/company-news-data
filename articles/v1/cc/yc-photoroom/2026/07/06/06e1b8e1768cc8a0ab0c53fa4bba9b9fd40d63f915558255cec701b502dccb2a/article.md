---
schema_version: "1.0.0"
document_id: "06e1b8e1768cc8a0ab0c53fa4bba9b9fd40d63f915558255cec701b502dccb2a"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/background-remover-benchmark"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:33498377012dd00a8190d6fa93ebc9d1f09393668853b154e9b4aa3a69892e8d"
---

# What 9,000 community votes taught us about our background remover

Achieving flawless background removal is critical for enterprise-grade operations, from streamlining e‑commerce visual pipelines to improving creative production workflows. Enterprises demand not only precision but also solutions that work effectively at scale while addressing nuances like edge smoothness, subject isolation, and the preservation of intricate structures such as hair or fur.


Inspired by[Chatbot Arena (LMSYS),](https://lmarena.ai/) we built (and[open-sourced](https://huggingface.co/spaces/bgsys/background-removal-arena) ) a human-in-the-loop evaluation system that stacks multiple background-removal models side by side. We’ve gathered over **9,000 votes so far** and you can[cast your own votes](https://huggingface.co/spaces/bgsys/background-removal-arena) .


## A human-in-the-loop evaluation framework for background remover solutions


**Purpose-driven dataset selection**


The benchmark uses a carefully curated dataset reflecting a variety of operational challenges:


-


**Diversity in the object types:** Covering apparel, electronics, pets, and plants.


-


**Challenging scenarios:** Designed to capture different lighting conditions and intricate background complexities.


Hosted on Hugging Face Spaces, the evaluation platform is an open-source project that enables direct, side-by-side comparisons of model outputs.


### Who votes, and how?


**Community:** To date, we’ve gathered around 9,000 public votes coming from LinkedIn, X and Reddit.


**Side-by-side comparisons:** The system randomly presents two model outputs for a given image. Reviewers choose which mask looks better. If both masks are equally good (or equally flawed), a tie is recorded.


**Randomized model pairings:** Each model faces a range of others, reducing skew from repeated matchups.


Votes are then aggregated using an Elo rating system—commonly found in chess—to dynamically rank models based on performance.


### Why use Elo for background removal?


Traditional scoring systems can be too static for a continuously evolving benchmark. The **Elo** score lets us dynamically rank models based on ongoing human feedback and model improvements.


1.


**Pairwise matches:** Each vote is like a “chess game.” The winning model scores 1, and the losing model scores 0.


2.


**Expected score:** If Model A (rating R_A) faces Model B (R_B), we calculate E_A, the probability that A wins. A model will gain more points by beating a higher-rated opponent than by beating a lower-rated one.


Similarly, Model B's expected score is:


3.


**Update ratings:** Ratings are recalculated after every vote. A “K-factor” controls how drastically scores move, ensuring stable convergence over large numbers of votes.


Where:


-


R'_A: New rating for player A


-


S_A: Actual result for Player A (1=win, 0=loss)


-


E_A: Expected score for Model A.


-


K: A scaling factor controlling sensitivity


4.


**Handling ties:** If two outputs seem equally good (or equally flawed), we record a draw.


## Snapshot of background remover rankings


With the 9,000 votes collected so far (from December 2024 to January 2025), here’s how a few major players compare:


### **A few examples of good segmentations for each model**


We spent some time looking into the vote results to understand on which images each model performs the best.


**Examples of Photoroom as winner (ranked 1st)**


Check out two standout examples showcasing Photoroom’s algorithm. Notice how each strand stays intact in a young woman's portrait, while color spill from her backdrop is corrected. Meanwhile, that handbag image demonstrates a keen understanding of context, which removes a door knob effectively.


**Examples of Remove.bg as winner (ranked 2nd)**


**Examples of[BRIA](https://bria.ai/rmbg-for-commercial-use#image) as winner (ranked 3rd)**


**Examples of[Clipdrop](https://clipdrop.co/remove-background) as winner (ranked 4th)**


## **Why you should vote**


1.


**Statistical significance**


9,000 votes is a solid start, but for better rankings—especially across diverse categories and scenarios—we need additional data. More votes yield a clearer, more stable picture.


2.


**Edge cases**


Background-removal performance can vary widely (e.g., apparel vs. pets vs. electronics). Collecting more images and votes helps capture these subtleties.


3.


**Community validation**


A large number of diverse voters ensures that the benchmark reflects real-world preferences, not just a single viewpoint or use case.


➡ **[Get involved: cast your vote here](https://huggingface.co/spaces/bgsys/background-removal-arena)**


## Expanding the background remover benchmark


To make the benchmark even more inclusive, we’re pushing for:


-


**Targeted benchmarks** : Adding e-commerce-specific cases, including clothing, cars, furniture, and logos.


-


**Segment-specific metrics** : Zeroing in on known challenges such as transparency, color bleeding, and edge quality.


-


**Ongoing community collaboration:**


-


[Contribute images :](https://huggingface.co/spaces/bgsys/background-removal-arena/discussions) If your enterprise has specific visual challenges, consider contributing them and discover which model suits your needs.


-


**Add a model** : We welcome more models to our open-source platform so that this benchmark gives you a comprehensive view of the industry.


## Conclusion: elevating background removal standards


This background removal benchmark aims to provide an **open, transparent, and user-focused** perspective on the state of the art. We’ve already gathered **9,000 votes** , and each additional vote helps refine the rankings for a fairer, more representative result.


If your enterprise is seeking a[high-quality background removal platform](https://www.photoroom.com/tools/background-remover) that scales effortlessly and adapts to evolving user expectations, **schedule a call with our sales team** to learn about our Enterprise offerings. And if you're looking for more information, here's a great article on[what to look out for when choosing a background remover](https://www.photoroom.com/blog/image-background-removal-technology-comparison) .


### **[Get in touch with an expert.](https://www.photoroom.com/contact-sales)**


Get help with pricing and plans, schedule a demo, explore use-cases for your team, and more.


### **FAQs and additional details**


-


**Who can participate in the benchmark?**


Anyone can vote! Broader participation strengthens the results.


-


**Is everything really open-source?**


Yes![Visit the repository](https://huggingface.co/spaces/bgsys/background-removal-arena/tree/main) on Hugging Face to see the code that runs these comparisons, observe how votes are recorded, or fork the project yourself.


-


**Interested in adding your images or model?**


We welcome pull requests with additional data sets, and we’re happy to guide you through integrating your own model for evaluation.
