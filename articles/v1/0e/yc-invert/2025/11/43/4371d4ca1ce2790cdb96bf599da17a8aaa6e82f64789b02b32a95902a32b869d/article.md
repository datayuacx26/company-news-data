---
schema_version: "1.0.0"
document_id: "4371d4ca1ce2790cdb96bf599da17a8aaa6e82f64789b02b32a95902a32b869d"
company_key: "yc-invert"
company: "Invert"
source_id: "yc-invert-news-import-968576ef11e8"
canonical_url: "https://invertbio.com/blog/engineer-blog-series-invert-assist-with-simon-sotak-gregor"
published_at: "2025-11-11T00:00:00+00:00"
first_seen_at: "2026-07-24T00:25:27.297188+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:b09a49bb05251fe1279420ad6f51def13dd6c043ee25184f6be813299b3316ef"
---

# Engineer Blog Series: Invert Assist with Simon Sotak Gregor

## Engineer Blog Series: Invert Assist with Simon Sotak Gregor


*Welcome to Invert's Engineer Blog Series! This series is a behind-the-scenes look into the product and how it's built. Invert recently launched Invert Assist, our AI interface for bioprocess data analysis. We speak to senior software engineer Simon Sotak Gregor about Invert Assist to learn more about how it was built, what problems it solves, and how he hopes it’ll change the way bioprocess is done.*


‍


## What problem does Invert Assist solve?


Bioprocess experimental data is one of biopharma and biotech's most valuable assets. We think there's a huge gap in being able to use this data and a huge opportunity to use more of it much better.


We want to arm every single bioprocess engineer with their own on-demand data scientist that can help them analyze and model data they already have. There are so many bioprocessing teams sitting on large amounts of data, but analyzing it all would be too time-consuming.


Now, you have the capability to have sophisticated models and answers ready in a couple of minutes. Invert Assist can generate insights and design experiments faster, and ultimately improve time to milestone, such as how long it takes for a product to get to clinical trials.


## What makes applying AI to bioprocess data different from other kinds of data?


Even a small bioreactor generates large amounts of data from its online sensors — a single run can generate tens to hundreds of megabytes of data. You cannot just take all of this data and copy and paste it into a naïve general-purpose model like ChatGPT.


In bioprocess, companies usually have a large number of historical runs. Some of our customers that have all their data consolidated in Invert easily have tens of thousands of them. We need to be smart about how we structure the AI's workflow, as well as what tools we give the AI to pick and choose the the right pieces of the data it needs to answer your questions.


## How did the team make sure Invert Assist is suitable for enterprise biotech and biopharma use?


Biotech and especially biopharma are heavily regulated industries. One of the foundational requirements for any AI system is to ensure that it’s compliant with industry standards and that it doesn't drive decisions that might endanger anyone or lose a company millions of dollars.


To develop and deploy Invert Assist responsibly, we've adopted several compliance frameworks—the[EU AI Act](https://artificialintelligenceact.eu/) and the[NIST Risk Management Framework for AI](https://www.nist.gov/itl/ai-risk-management-framework) , which are both regulatory frameworks that outline best practices throughout the entire lifecycle of an AI model.


We are also SOC 2 Type 2 and ISO 27001-certified, which means we have to consider all risks when we develop AI features for our product. Given the value of bioprocess data, security is of utmost importance. We adhere to rigorous security standards to ensure no customer data leaks, and that it isn’t stored or used to train future AI models.


Another concern that customers may have is about the traceability of Invert Assist’s answers, or how to maintain audit-ready documentation of AI results. Most large language models like ChatGPT are “black box” models, which means you don’t have any insight into how they arrived at their results.


In contrast, Invert Assist provides answers that are fully traceable. It analyzes data by writing and executing Python code, and then reasons about it to give you conclusions and recommendations. Both the code and the reasoning are fully transparent and accessible to the user. This means that Invert Assist’s answers are reproducible, and users have the ability to review and verify answers for correctness.


## How did the team assess the reliability and accuracy of Invert Assist’s answers?


We use an industry-standard approach in AI called evaluations. They’re a set of tasks to test the performance of an AI model. For example, each new version of ChatGPT tells you how good it is at answering mathematical questions.


At Invert, we built our own bioprocess evaluations. We assembled datasets and sets of bioprocess-specific questions and tasks. They ranged from simple and objective, such as “Which run had the highest titer?”, to complex and subjective, such as, “Given data from these runs, how should I design my next experiments?”.


We run these evaluations daily, as well as after every change we make to Invert Assist. We assess its performance by testing how fast the system is, how well it performs, how sound its reasoning is, and how much it hallucinates.


We also evolve our evaluations as we as we get new insights about what kind of problems our customers are using Invert Assist for. We see new failure modes, new applications, and new capabilities every day. These let us add to our evaluations, which gives us insight into how the system is performing overall. I’d love it if Invert’s work in this space could become the industry standard for AI in bioprocess one day.


## What were some difficult technical hurdles and how did the team overcome them?


The hardest hurdle was how the AI should make decisions regarding the appropriate data to select. We overcame this in what I think was a pretty clever way. There's a term in AI called “human-in-the-loop”. The basic principle is that you shouldn’t try to solve the hardest problem, especially if your user can solve it really easily.


The bioprocess engineer ultimately knows their runs very well, so instead of having the AI hunt for the right data, you simply select the runs that you’re interested in. That way, you solve both the hardest problem and you get the most value out of out of the AI. The intersection gives us the best of both worlds.


## Which AI capabilities are you most excited to build in the future?


I'm excited about more proactive AI. If you have an experiment running, the AI would keep looking at the data and keep analyzing it. If anything unexpected or surprising showed up, or perhaps a piece of data confirmed or disproved your hypothesis, it would be able to proactively point it out to you. It’d be able to tell you, “This data is worth looking at because it’s unexpected or interesting,”— that’s something I’d be excited to make a reality.


‍
