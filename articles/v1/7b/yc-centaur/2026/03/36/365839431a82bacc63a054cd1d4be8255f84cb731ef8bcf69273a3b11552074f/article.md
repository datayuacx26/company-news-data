---
schema_version: "1.0.0"
document_id: "365839431a82bacc63a054cd1d4be8255f84cb731ef8bcf69273a3b11552074f"
company_key: "yc-centaur"
company: "Centaur"
source_id: "yc-centaur-news-import-7ab94b327191"
canonical_url: "https://centaur.ai/post/from-prediction-markets-to-production-ready-data"
published_at: "2026-03-06T02:14:47.897+00:00"
first_seen_at: "2026-07-27T00:36:30.031492+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:ecfffaa43bf09489b377103beb6b45cfac7831799c608b155c7d594cda36e658"
---

# From Prediction Markets to Production-Ready Data

In a recent episode of SegMed’s[Bytes of Innovation](https://www.youtube.com/watch?v=_LPE-4biL3c&t=86s) webinar series, Centaur.ai CEO Erik Duhaime discussed one of the least visible yet most consequential factors in Healthcare AI: how training data is actually labeled. Much of the industry still thinks about annotation in simple terms. Assign a case to a human, collect the label, and move on to the next image.
‍


At a small scale, that approach appears reasonable. At the production scale, however, it breaks down quickly. The problem is not just the volume of data. It is the variability in how difficult individual cases can be. For example, when labeling medical images, many images will be obvious and easy to label. However, others are likely to be ambiguous even for experts. When every case is treated the same way, teams either waste time reviewing easy cases or fail to give difficult cases the attention they require.
‍


Dynamic labeling pipelines solve this problem by adapting to the task's difficulty in real time.


‍


# Not Every Case Deserves the Same Workflow


‍


During the Segmed webinar, Erik explained that Centaur approaches annotation as a statistical aggregation problem. Instead of relying on a single expert or applying the same rule to every image, the system combines multiple independent judgments. It then evaluates the degree of agreement among those judgments. To explain the concept, Erik used a simple analogy. “Sometimes I liken the approach to putting together the optimal trivia team,” he said. “If you want to answer a bunch of trivia questions, you probably want more than one person, and you don’t necessarily want five people that are all the same.”


‍


Different annotators excel at different subtasks. Some are better at identifying melanoma, while others perform better at detecting basal cell carcinoma or specific imaging artifacts. By measuring these strengths and combining them intelligently, a labeling system can outperform any single expert working alone.


‍


This approach also exposes a core flaw in traditional annotation pipelines. Many organizations apply fixed rules, such as assigning every case to a predetermined number of reviewers. That rule is simple to implement, but it assumes every case requires the same level of scrutiny.


‍


# Why Disagreement Is a Signal, Not a Failure


‍


Dynamic labeling pipelines work differently because they use disagreement as useful information. Instead of treating conflicting opinions as noise, the system treats them as a signal that a case may be more complex. That signal triggers additional review.


‍


Erik described the process during the conversation. “If the first three people at a task all agree and they are all very good at that task, we might say with very high confidence that if all three say this is melanoma, then it is melanoma,” he explained. “But if they disagree with each other, we are automatically going to escalate that case and get additional votes on it.” This escalation process allows the pipeline to allocate effort where it is actually needed. Easy cases resolve quickly because experts agree on them. Hard cases receive additional review until a confident consensus emerges.


‍


The result is both more accurate and more efficient. As Erik summarized, “You would rather ask seven people the hard questions and ask three people the easy questions.” Fixed workflows cannot make that distinction, which means they inevitably waste effort in one direction or the other.


‍


# Confidence and Controversy Improve the Dataset


‍


Dynamic labeling pipelines also reveal valuable insights about the data itself. When annotators repeatedly disagree on certain examples, the issue is often not a lack of annotator skill. Instead, it may indicate unclear labeling guidelines or an ambiguous definition within the product specification. Erik offered a practical example from medical imaging. Lung nodule definitions can vary between regulatory environments such as the United States and the European Union.


‍


If annotators disagree on a borderline case, it may indicate that the target definition needs clarification before labeling continues. “Figuring out what is controversial early on is really valuable,” Erik noted. “It provides insights for when you are building your model.” Discovering those issues early prevents costly rework later in the development cycle.


‍


Centaur.ai’s platform also returns more than just the final label. It provides a confidence estimate derived from annotator agreement and measured performance over time. “We do not only send them what we think the answer is,” Erik said. “We give them this estimate of confidence and uncertainty.” For AI developers, that information is essential. High-confidence labels accelerate model training, while low-confidence cases highlight edge cases that often determine real-world reliability. By identifying those difficult examples early, teams can focus their attention where model improvements matter most.


‍


Dynamic annotation systems are built specifically to surface and resolve those edge cases. By continuously measuring annotator performance, escalating uncertain examples, and adapting the workflow to the task's difficulty, they deliver what fixed pipelines cannot: scalable data quality. For teams building healthcare AI, that difference often determines whether a model merely passes a benchmark or actually works in practice.


‍


# Next Steps
‍


If you want to see what this looks like in practice, you can listen to the[SegMed Bytes of Innovation conversation with Erik](https://www.youtube.com/watch?v=_LPE-4biL3c) , or simply[schedule a demo](https://info.centaurlabs.com/demo) with us. We’ll show you how to design a labeling and evaluation strategy that increases accuracy, lowers rework, and produces data you can defend.
