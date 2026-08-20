---
schema_version: "1.0.0"
document_id: "701eb29ef663d77885eaf21b644c61c6a07ab6048ae780910e6d02e7f93b45bd"
company_key: "backblaze-inc-class-a-common-stock"
company: "Backblaze Inc."
source_id: "backblaze-inc-class-a-common-stock-rss-a06767c1ff83"
canonical_url: "https://www.backblaze.com/blog/backblaze-drive-stats-academic-ai-ml-research/"
published_at: "2026-08-13T15:08:38+00:00"
first_seen_at: "2026-08-13T16:04:34.891940+00:00"
fetched_at: "2026-08-13T16:04:36.096256+00:00"
content_hash: "sha256:c9331b1ef27287673bc166cefe49c9ebb6070b70be5a31dfdd648612374ece4f"
---

# Backblaze Drive Stats: How an Open Dataset Powers Academic and AI/ML Research

Since April 2013, Backblaze has published the daily health readings of every hard drive running in our data centers. Model, serial number, failure flag, and dozens of Self-Monitoring, Analysis, and Reporting Technology (SMART) attributes, collected into a CSV for each day, released publicly every quarter, for free. We built[Drive](https://www.backblaze.com/cloud-storage/resources/hard-drive-test-data)[Stats](https://www.backblaze.com/cloud-storage/resources/hard-drive-test-data) as a mechanism to understand our own fleet, then published almost on a whim—so much so that[the original idea is credited to two Brians](https://www.backblaze.com/blog/10-stories-from-10-years-of-drive-stats-data/) . What happened next was an exciting surprise.


More than[227 papers and articles](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C5&as_ylo=2018&as_yhi=2026&q=%22www.backblaze.com%2Fb2.hard-drive-test-data.html%22&btnG=) have cited Drive Stats as a primary dataset since 2018. Researchers have trained transformer architectures,[long short-term memory (LSTM) networks](https://en.wikipedia.org/wiki/Long_short-term_memory) , survival models, and gradient-boosted ensembles on it. The dataset that started as an internal reliability tool has become one of the most-cited open benchmarks in hard drive failure prediction research.


## What makes Drive Stats so valuable?


Drive Stats reflects a live, continuously operating commercial fleet of drives: different manufacturers, different models, different capacities, all spinning in production[Storage Pods](https://www.backblaze.com/blog/the-drive-stats-of-backblaze-storage-pods/) under real workload conditions. That combination of scale and heterogeneity is rare, and it is exactly what makes the dataset especially useful to researchers.


For storage engineers and data scientists, the dataset checks every important box: real-world origin, a long time horizon, labeled failures, an open license, and an active maintainer that publishes new data each quarter. Here are a few other specifics worth understanding before diving into the research:


- **Download access:** The full quarterly archive is available for free on the[Backblaze Drive Stats](https://www.backblaze.com/cloud-storage/resources/hard-drive-test-data) page under the MIT License.


- **Open license** : Backblaze asks three things of anyone who uses the data: that they cite Backblaze as the source, they accept responsibility for how the data is used, and they do not resell it.


- **SMART attributes:** Each daily snapshot includes raw and normalized values for more than 70 different SMART attributes per drive. We define a drive failure in our dataset based on a few metrics, which we talk about[in previous](https://www.backblaze.com/blog/backblaze-drive-stats-for-q3-2025/)[reports,](https://www.backblaze.com/blog/backblaze-drive-stats-for-q3-2025/) as well as articles dedicated to[parsing the program.](https://www.backblaze.com/blog/drive-stats-data-deep-dive-the-architecture/)


- **Scope and coverage** : We began publishing quarterly hard drive data in 2013. The current dataset spans more than a decade of operation and covers hundreds of thousands of individual drives across manufacturers including Seagate, HGST, Western Digital, and Toshiba. Each year, we also compile data related to annualized failure rates (AFR) and lifetime failure rates across all manufacturers. Lifetime failure rates indicate the probability that a drive will fail over the course of its lifetime, while AFR indicates the probability a drive will fail during a year of operation. Rates are expressed as a percentage. Data related to lifetime failure excludes drive manufacturers with less than 500 units deployed, and all drives must have 100,000 active drive days to be included in the lifetime data set. The[2025 annual report](https://www.backblaze.com/blog/backblaze-drive-stats-for-2025/) recorded an annualized failure rate (AFR) of 1.36% across the fleet, down slightly from 1.57% in 2024, reflecting multiple factors (average drive age, technology improvements, drive size increases, cost per gigabyte, and market demand).


Most importantly: the economics of drives, and the measures people take to keep them spinning within a data center, are fundamentally different than in a consumer use case. In some ways, it’s the perfect test—the drives are always on, and we run them until they[give up the ghost.](https://www.backblaze.com/blog/quoth-the-drive-stats-nevermore-an-elegy-for-our-seagate-4tb-drives/) In others, it’s a bit deceptive—most people don’t have alerts set up to monitor drive health in their home environments.


Of course, many do. Data is important, and for those who schedule weekly maintenance for your home networks: we love it, we’re here for it, and us too.


## Academic research built on Drive Stats


The papers below represent a cross-section of research that uses Drive Stats as a primary dataset. Each represents a meaningfully different approach to the same core problem: predicting when a hard drive will fail, with enough lead time to act on it.


### TFBEST: Dual-Aspect Transformer With Learnable Positional Encoding for Failure Prediction


**Authors:** Rohan Mohapatra, Saptarshi Sengupta


**Venue:**[arXiv preprint](https://arxiv.org/abs/2309.02641)


**Submitted:** September 2023


Experiments on Seagate hard disk drive (HDD) data led the authors to propose a novel transformer architecture for predicting failures in hard drives that significantly outperformed prior state-of-the-art remaining useful life methods on the Drive Stats benchmark.


Their proposed architecture—the Temporal-Fusion Bi-Encoder Self-attention Transformer—is an encoder-decoder model trained on the full 10-year Drive Stats corpus (from 2013 to the time of article). Rather than classifying drives as failed or healthy within a fixed horizon, TFBEST predicts a sequence of days-to-failure, giving operators a window, rather than a binary alarm. The paper also introduces a confidence-margin statistic that manufacturers can use to set replacement thresholds with quantified uncertainty.


### Large-Scale End-of-Life Prediction of Hard Disks in Distributed Datacenters


**Authors:** Rohan Mohapatra, Austin Coursey, Saptarshi Sengupta


**Venue:**[IEEE](https://ieeexplore.ieee.org/document/10207630) ****


**Submitted:** August 2023


The authors presented a long short-term memory (LSTM) model that used understanding gleaned from Drive Stats to aid in predicting an output sequence of the number of days remaining before the possible failure of a disk. The LSTM posted a root mean square error of 0.83 during training, and 0.86 during testing across the full 10-year corpus, and generalized competitively across multiple Seagate model families.


The core architecture was an encoder-decoder LSTM network: the encoder processed a window of historical SMART readings for a given drive; the decoder produced a multistep output sequence representing the predicted days-to-failure. The model was trained and validated on all Drive Stats data available at the time, roughly 35GB, covering Seagate drive models with significant failure populations.


### Leveraging Survival Analysis in Cost-Aware Deepnet for Efficient Hard Drive Failure Prediction


**Authors:** Jishan Ahmed, Robert C. Green II


**Venue:**[Neural Computing and Applic](https://link.springer.com/article/10.1007/s00521-024-10479-6)[a](https://link.springer.com/article/10.1007/s00521-024-10479-6)[tions, Vol. 37](https://link.springer.com/article/10.1007/s00521-024-10479-6)


**Published** : October 2024


To address the significant imbalance of real-world datasets used for drive-failure detection—the relatively small number of failures when compared to the number of drives operating normally—the authors relied on the SMART attributes found in Drive Stats to uncover new insights into drive health and failure.


They used a dual-track approach: a deep-learning track for failure prediction and a survival-analysis track for identifying which attributes most strongly govern time-to-failure. Together, the two tracks provided both operational predictions and mechanistic insights useful for data-center management strategy.


### Examining the Impact of Critical Attributes on Hard Drive Failure Times: Multi-State Models for Left-Truncated and Right-Censored Semi-Competing Risks Data


**Authors:** Jordan L. Oakley, Matthew Forshaw, Pete Philipson, Kevin J. Wilson


**Venue:**[Applied Stochastic Models in Business and Industry, Vol. 40, Issue 3](https://onlinelibrary.wiley.com/doi/abs/10.1002/asmb.2829)


**Published** : December 2023


Many hard-drive failure prediction papers ask a binary question: Will this drive fail in the next N days? This paper asked a more nuanced statistical question: How do intermediate critical states defined by deteriorating SMART attributes affect the time distribution of eventual failure?


Oakley and colleagues first defined critical attributes and critical states using Drive Stats SMART readings, and then fit multistate survival models to the resulting semicompeting risks structure. These risks arise because a drive can move from healthy to critical (nonterminal) before failing (terminal), but failure can also occur without a detectable prior critical state. The multistate framework handled both pathways in a single coherent model.


The key contribution was a set of dynamic predictions of conditional survival probability that updated as the observed state of a drive changed – so operators got a live risk estimate, not a static score. Experiments on Drive Stats data confirmed that drives entering critical states are substantially more likely to fail.


## AI/ML Models and Projects Built on Drive Stats


Academic papers are one signal that a dataset has earned its place in a field. Practitioners building things with it are another. A growing body of work, including open-source projects on platforms like GitHub, helps translate academic research into practical, runnable code, and provides additional confirmation of Drive Stats as a standard benchmark for the field.


### HDD Failure Prediction Using Machine Learning


**Contributor:** Marcos Garcia Estevez (warc0s)


**Platform:**[GitHub](http://github.com/warc0s/hdd-failure-rate-ml)


**Contributions Made:** October 2024


The project aimed to create a binary classification model using machine-learning algorithms to predict the probability of drive failures based on SMART data, along with other features such as brand and storage capacity. It applied three methods: random forest, XGBoost, and a combined ensemble to Drive Stats SMART attribute data.


### Large-Scale End-of-Life Prediction of Hard Disks in Distributed Datacenters


**Contributor:** Rohan Mohapatra (rohanmohapatra) (Austin Coursey, Saptarshi Sengupta)


**Platform:**[GitHub](http://github.com/sjsu-micosys-lab/hdd-rul-prediction)


**Contributions Made:** June 2024


For practitioners building their own Drive Stats pipelines, this is one of the few public examples that addresses the full stack data ingestion, feature engineering, class imbalance, and sequence modeling, rather than demonstrating a model on a precleaned subset. Its combination of XGBoost for feature selection and LSTM for sequence prediction serves as a practical template for anyone working with raw quarterly CSV files.


Beyond the model architecture described in the[academic paper](https://arxiv.org/abs/2303.08955) section above, this project is notable for its engineering approach to handling Drive Stats at scale. The team built a preprocessing pipeline using PostgreSQL to ingest, filter, and join the quarterly files; used XGBoost across the full SMART attribute set; and applied interpolation to fill gaps, before feeding sequences to the encoder-decoder LSTM.


### Are There Manufacturer Differences in Hard-Drive Reliability?


**Contributor:** Christoph Siemroth, Yeomyung Park


**Venue:** IEEE Transactions on Cloud Computing


Researchers used Backblaze’s large data-center dataset to[compare failure rates across four manufacturers](https://repository.essex.ac.uk/43022/) (HGST, Seagate, Toshiba, and Western Digital). Duration regression models controlled for drive age, capacity, and form-factor, and the findings concluded that HGST drives fail least often (about 42% of Seagate’s failure rate. However, WD drives outperformed Seagate but fared worse than HGST. Toshiba’s failure rate is similar to Seagate’s.


The study revealed a significant reliability gap between HGST and Seagate, doubling the financial burden for large-scale operators related to replacement-related labor and other costs. Drive failure analytics highlighted in the study can be used by large-scale operators to forecast future costs, informing procurement decisions.


## Drive Stats research at a glance


Paper / Project Authors Venue / Platform What It Predicted / Built


TFBEST: Dual-Aspect Transformer With Learnable Positional Encoding for Failure Prediction Mohapatra, Sengupta arXiv (2309.02641) A novel, high-performing transformer architecture for predicting hard-drive failures


Large-Scale End-of-Life Prediction of Hard Disks in Distributed Datacenters Mohapatra, Coursey, Sengupta IEEE An LSTM helps to predict the number of days to a given disk’s failure to a high accuracy level


Leveraging Survival Analysis in Cost-Aware Deepnet for Efficient Hard Drive Failure Prediction Ahmed, Green II Neural Computing and Applications, Vol. 37 Operational predictions and mechanistic insights for data-center management strategy


Examining the Impact of Critical Attributes on Hard Drive Failure Times Oakley, Forshaw, Philipson, Wilson Applied Stochastic Models in Business and Industry, Vol. 40, Issue 3 Confirmation that drives entering critical states defined by deteriorating SMART attributes are substantially more likely to fail


HDD Failure Prediction Using Machine Learning warc0s GitHub A binary classification model using machine-learning algorithms to predict the probability of drive failures


Large-Scale End-of-Life Prediction of Hard Disks in Distributed Datacenters rohanmohapatra GitHub A practical template for practitioners working with raw quarterly CSV files to build their own prediction pipelines


Are There Manufacturer Differences in Hard-Drive Reliability? Siemroth, Christoph Park, Yeomyung IEEE A comparison of hard-drive reliability across four manufacturers, using data regression models.


## The case for open data


The breadth of research is a direct result of open datasets. These citations occur because the data was consistently available every quarter for more than a decade—and it helps that we built a community of similarly interested people, too.


For research communities, open datasets function the way open-source libraries do: they create a shared foundation that everyone can build on and compare against. Drive Stats has earned that role in hard drive failure prediction by showing up reliably for over 13 years. A few things make open data particularly useful:


- **Ecosystem reach** . Drive Stats doesn’t exist in isolation: We publish it on[Hugging Face,](https://huggingface.co/datasets/backblaze/Drive_Stats) where it sits alongside hundreds of thousands of open datasets, across domains from natural language processing to genomics.[Kaggle](https://www.kaggle.com/search?q=Backblaze+drive+stats) hosts it alongside tens of thousands of community notebooks and kernels.


- **Reproducibility.** Drive Stats is public, permanently archived, and available for download. A paper published today cannot recreate historical data. There’s not really a corollary in a field where most real-world fleet data is proprietary and inaccessible—but we’d love people to join us.


- **Research velocity.** When data is freely available, there’s no need to spend months negotiating access agreements. You simply download Drive Stats, read the schema documentation, and start building. The papers mentioned in this article collectively span transformer architectures, survival models, deep neural networks, and gradient boosting, all on the same dataset. You can’t necessarily call it a direct comparison, but it does make one thing clear: hardware is central to the cloud conversation.


## Sign up for the Drive Stats newsletter


The same data that powers academic research also powers our own reliability reporting: The annualized failure rates, SMART attribute analysis, and transparency have made Drive Stats a standard in the field for more than a decade. And that data keeps coming, with a new release every quarter, since 2013.


If you’re working on failure prediction, predictive maintenance, or just want a real-world labeled dataset for benchmarking, this is the one researchers keep reaching for.


[Sign Up for the Drive Stats Newsletter](https://hub.backblaze.com/drive-stats-newsletter-sign-up)


**** **What is Backblaze Drive Stats?**


Backblaze Drive Stats is a publicly available dataset of daily hard drive health snapshots from our data centers. Published quarterly since 2013, it includes Self-Monitoring, Analysis, and Reporting Technology (SMART) attribute readings; failure labels; and model information for hundreds of thousands of drives. The data is free to download from the[Backblaze Drive Stats](https://www.backblaze.com/cloud-storage/resources/hard-drive-test-data) page.


**** **Has Drive Stats been used in academic research?**


Yes. More than 227 papers and articles have cited Drive Stats as a source since 2018. Researchers have used it to develop and test hard drive failure prediction models, survival analysis frameworks, and deep learning architectures across venues including IEEE, Springer, Wiley, and arXiv.


**** **Which AI/ML models were trained on the Backblaze dataset?**


Researchers have trained a wide range of models on Drive Stats data, including long short-term memory networks, transformer architectures, 1D convolutional neural networks, gradient-boosted trees, survival analysis models, and ensemble methods.


**** **Is the Backblaze Drive Stats dataset on Hugging Face?**


We publish Drive Stats on Hugging Face at[huggingface.co/datasets/backblaze/Drive_Stats](http://huggingface.co/datasets/backblaze/Drive_Stats) . The repository contains over 388 million records and grows by more than 240,000 records per day. It is append-only, meaning daily snapshots are never updated or deleted once written, making it particularly useful for reproducible research.


**** **Why do researchers use open hard drive datasets?**


Open datasets like Drive Stats allow researchers to benchmark models against consistent real-world data, reproduce published results, and build on prior work without access to proprietary fleet data. Because Drive Stats reflects a heterogeneous, production-scale environment rather than a lab setting, it provides a uniquely credible benchmark for evaluating failure prediction methods.


**** **How many times has Drive Stats been cited in research?**


More than 227 papers and articles have cited Drive Stats since 2018. The actual number continues to grow as researchers publish new work on hard drive reliability, predictive maintenance, and artificial intelligence/machine learning (AI/ML) model benchmarking.
