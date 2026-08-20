---
schema_version: "1.0.0"
document_id: "f71bc0677301bcbdf1db4dc282c656797c57ab1cd57ba730465aa458ecd9eee4"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/3-ai-truths-paris-digital-science-innovation-day"
published_at: "2026-02-03T08:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T22:22:21.045319+00:00"
content_hash: "sha256:19ef9185748dcc5e55d4bf41c1055d950ceb3c4a7936d1b066288b97df886865"
---

# 3 AI truths from the 3rd Paris Digital Science & Innovation Day

When a venture capitalist opens a keynote with "We've been disappointed," you know you're in for an honest conversation.


That's exactly what happened at[Benchling's 3rd Annual Paris Digital Science & Innovation Day](https://events.benchling.com/pdsid2026) . Simon Turner, Partner, Digital Medicine Strategy at Sofinnova Partners set the tone early. The first wave of AI in biotech didn't deliver what everyone expected.


However, Turner argued the disappointment was necessary. It forced the industry past buzzwords and into the hard work of implementation.


"We've seen a huge shift. We believe that we are in this new S curve," Turner explained. "The same that we've seen when the internet was coming of age and now it’s happening with AI."


Throughout the day, leaders from across the European biotech ecosystem shared hard-won lessons from deploying AI in real workflows. Here’s the top three practical takeaways:


## Train foundation models on the science, not just patterns


The most successful AI implementations don't build a new model for every task. They train a foundational model on diverse, high-quality data to help it learn the underlying biology. That knowledge is then applied to multiple downstream tasks.


Eric Letouzé, Head of Single Cell at One Biosciences, a company developing single-cell oncology biomarkers, described their approach. Critical tasks like cell annotations used to take hours or even days to complete.


"We cannot afford that time and level of expertise in a clinical setting," Letouzé said.


With their model, this can now happen automatically. They start by training a foundation model on millions of human cells, ranging from normal tissues to cancerous ones. The model learns cell states, transcriptomes, and gene regulatory networks. It also handles drug discovery, gene prioritization, and phenotype prediction. As a result, One Biosciences is able to generate oncologist reports within two weeks of receiving the original sample.


Owkin, an AI-native biotech focused on drug discovery and clinical development, took a similar approach with protein language models trained on antibodies. These foundation models enable de novo antibody design and help predict which designs will fail downstream, allowing teams to weed out problems early.


"To me, one step that is really important for AI is data. And if you have more data, I'll take it," said Elodie Pronier, VP of Biomedical Discovery at Owkin.


The use cases don't stop at biomarkers and antibody design. Matching bacteriophages to bacteria requires scientists to manually select which features to analyze — a slow and labor-intensive process. Phagos, a company developing bacteriophage therapies, tackled this challenge with their CAPHARD model.


"The best approach is where the model itself can decide what features to use," said Andrea Di Gioacchino, Head of Data and AI at Phagos.


CAPHARD takes full genomes and discovers which proteins interact. Wet lab scientists can query it throughout the day, asking which phages will work for the strains they're trying to eliminate. The model can answer because it learned the underlying biology from training examples.


All three companies emphasized the same prerequisite: unified data infrastructure. One Biosciences needed millions of cells in consistent formats. Owkin spent years accumulating training data. Phagos needed a continuous wet-dry lab workflow.


"Benchling is our connector. It's the language our data speaks," Di Gioacchino said.


This is where platform architecture comes into play. Benchling automatically captures relationships as scientists work, connecting notebooks, inventory, samples, and results. When you ask Benchling AI to summarize your oncology study, it understands how your data connects — not just the text.


"All of these AI agents, features, and models are built on top of our data foundation," said Jean Louis Honeine, Solution Consultant for IT and Data Science at Benchling.


Get the infrastructure right so you can build robust foundational models.


## Small batches and iteration beat big bang launches


Successful implementations don't happen through massive transformations. They happen through careful, iterative rollouts with continuous learning.


Aurore Morello, Head of Research at OSE Immunotherapeutics, a clinical-stage immuno-oncology company, faced a familiar challenge: data scattered across Excel files, disconnected systems, and various CRO formats. Usually, the temptation is to design a mature system and roll it out at once. OSE took a different path.


"We work on small batches and then by successive iteration we add the other data," Morello said. Their iterations involved getting feedback from bench scientists, adjusting based on the learnings, and then expanding to the next data type. Each small win built momentum for the next phase.


Amine Raji, CEO of SporeBio, described a similar journey. When they first proposed using biophotonics and AI for rapid microorganism detection, experts told them "it was not possible and it's going to be really long and painful." But they started anyway. Their first challenge was to prototype a system to generate data at scale. Their machine learning model has now seen more than a million samples globally and hundreds of thousands of microorganisms.


The validation came in stages. They proved their system could match traditional methods in ten minutes versus days. But Raji was candid about what's ahead. "There are still lots of challenges to be solved because we need to do that on lots of different types of products in the industry and clinical fields."


Di Gioacchino described the same philosophy with Phagos' AI development. They feed both successes and failures back into the model. Each iteration — they're now on generation four — improves performance as they advance their understanding of phage-bacterial interactions.


"There are still cases where it doesn't work," he acknowledged. "But if it doesn't work, we still win because we learn."


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## The best algorithms fail without the right teams


The companies succeeding with AI didn't just buy better algorithms. They made sure to involve domain experts from day one.


Morello was direct about the role of scientists in developing OSE's computational strategy. "The key is the people working on this internally. Especially when we work with very expert people, scientists and super experts on some topic, it's always a key point to involve them as much as possible within the project."


They put together user teams — bench scientists working directly within Benchling to design the platform based on real daily workflows. And it worked.


"The go live was super easy because it was well prepared by people that know what they do every day on the bench," Morello said.


Owkin built their organization around the same cross-functional principle. Pronier described a structure of 300+ scientists across data science, biology, medicine, and chemistry working together. Their computational drug discovery team is deeply embedded within the broader research organization. Some of that data came from a huge network of academic centers and pathology labs across France and Europe. This structure ensures the people generating the data and the people building the models work together from the start.


Turner reinforced this from the investor side. "The most successful organizations we've seen are the ones that make sure people aren't scared of them. These remain tools."


You can't force AI adoption overnight. Expert involvement builds the confidence that makes implementations stick.


The conversations in Paris weren't about whether AI will transform biotech. It's already happening. Attendees discussed practical strategies for implementing AI and what differentiates the companies actually making it work.


"We predict that there will be a startup doing a hundred million in revenue with only ten people in their life science department within the next five years," Turner said. "Because the technology is there to enable 10x or 1000x the scale of people here."


That's not hype. The evidence is already emerging. SporeBio is compressing microorganism detection from days to minutes. One Biosciences is generating clinical oncology reports in two weeks. Owkin is training models that understand biology well enough to design new molecules from scratch. And Benchling is the platform enabling it — helping scientists get more out of their data so they can focus on the questions that matter.


Turner also noted Europe's strength in research excellence as "better than any other geography in the world." The challenge has been moving from research to market fast enough. The companies presenting in Paris are showing how AI can change that equation — not by replacing scientific rigor, but by amplifying it.


AI isn't coming to biotech. It's already here. Don't wait to catch up.


## Want to be part of the conversation?


Digital Science & Innovation Day returns to Paris on 11 June 2026. Join France's leading minds in R&D, science, and innovation to explore how AI is accelerating research, modernizing biopharma process development, and transforming antibody drug discovery.


[Register for the event →](https://events.benchling.com/pdsid2026)


*All photographs by Johanne Romezin Photographe.*
