---
schema_version: "1.0.0"
document_id: "6727289a712c720c027bef40e6f1bb539a52abc4fac6f55261d61249f882b0bb"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-b653f977f25a"
canonical_url: "https://building.nubank.com/from-complexity-to-simplicity-a-principled-approach-to-big-data-understanding/"
published_at: "2026-05-14T14:30:00+00:00"
first_seen_at: "2026-07-20T04:36:16.841262+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:9830480dc909cf2ed8b158c0db0cb63448c26ec93bbe856dec7f0abaf5949e27"
---

# From complexity to simplicity: A principled approach to big data understanding

*Written by: Rodrigo Medina, Ivette Ayala*


At Nubank, hundreds of teams generate thousands of data assets to serve more than 130 million customers across multiple countries. The distributed architecture that enables this scale also introduces an analytical challenge: as the data platform grows, it becomes increasingly complex and fragmented, making it harder to understand and navigate.


DiscoverDf was created from the premise that in a distributed environment, understanding data assets cannot be left to chance. It needs to be approached systematically, transforming complexity into something structured and accessible. The goal is to enable any analyst, in any team, to go from first contact with a data asset to making meaningful contributions in a matter of days, not weeks or months.


## **The invisible onboarding tax**


Anyone who has joined a new team as a Business Analyst knows this feeling. You arrive with energy and ideas, but before you can contribute, there’s an invisible toll to pay: understanding the team’s datasets. Which tables actually matter, what each filter represents, where the edge cases live. Reaching that level of fluency can take weeks, sometimes months.


Now imagine a cross-functional project with a tight deadline. You need to work with datasets owned by other teams, that you have not had time to deeply understand. The usual approach is pragmatic: pull a row count, scan a few columns, and then reach out to the team’s BA or AE to fill in the gaps.


It works, but can break easily. Important filters tied to your specific context might be overlooked, segmentation opportunities can go unnoticed, and decisions end up being built on an incomplete mental model of the data. This doesn’t happen due to a lack of skill on the analyst’s part, but because there isn’t a structured way to explore this.


This is where the hypothesis starts. The onboarding tax is not inevitable, because datasets are not inherently hard to understand, but because we lack a consistent, repeatable way to approach them from the very first contact.


What if that process could be compressed? Instead of weeks of passive context gathering, an analyst could run a single function and, within minutes, understand the dataset’s contents, boundaries, distributions, and structure. From there, conversations with data owners would shift and the analyst would move from dependency to contribution, and from asking for explanations to proposing informed ideas.


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## **You can’t explore what you don’t know exists**


When analysts approach a new dataset today, they usually start with what they recognize, exploring familiar fields, asking the owning team about the rest, and gradually building a working understanding. Once you know where to look, this process is fast and effective. The challenge is that it assumes you already know where to look.


Now consider a dataset with dozens or hundreds of columns covering customer identities, time horizons, and behavioral categories. The analyst profiles what feels familiar, gets context on what does not, and moves forward, but there is more to uncover.


Potential segmentation dimensions remain hidden, and patterns that reveal how the data is structured are present, but not always surfaced. These elements can directly shape better decisions, yet they often stay unexplored.


This becomes even more relevant in projects that involve multiple datasets, sometimes owned by different teams. Each additional layer of understanding adds value, and the more complex the picture, the stronger the foundation for everything that comes next.


There is a meaningful difference between knowing a few dimensions and truly understanding a data object that captures customer activity, statuses, and behavior. When an analyst reaches that level of understanding, alignment conversations change. Instead of asking for context, they arrive ready to contribute, with a clear view of the full landscape the data represents.


## **Three principles for understanding any dataset**


The core insight behind DiscoverDf is that every dataset, regardless of domain or ownership, can be understood through three universal principles. Each one is tied to a different level of column cardinality.


### **The What: What entities does this dataset contain and at what scale**


Columns with the highest cardinality, often with millions or hundreds of millions of unique values, answer the most fundamental question: what entities exist in this dataset and at what scale?


These are typically identifiers Customer IDs, account IDs, transaction IDs, bill IDs. By looking at the unique value counts at the top of a sorted profile, an analyst can quickly infer the grain of the data, estimate the population size, and understand how entities relate to each other.


### **The When: The temporal structure of the data**


Columns with cardinality ranging from hundreds to tens of thousands usually represent time. Even without relying on column names, this range is a strong signal of temporal structure.


From there, the analyst identifies the primary date column by checking null counts, minimum and maximum values, and distribution over time. In a matter of seconds, it becomes clear how far back the data goes, how recent it is, and whether there are gaps that need attention.


### **The How: The dimensions available for analysis**


Columns with lower cardinality, often with one hundred or fewer unique values, define how the data can be explored. These are categorical and status fields such as product types, loan statuses, customer segments, flags, and behavioral categories.


They represent the natural axes for filtering, grouping, and analysis. At the same time, they are often the most overlooked during initial exploration. DiscoverDf makes them explicit by surfacing each dimension along with its full value distribution. What would otherwise depend on domain knowledge becomes structured and immediately actionable.


### **Where the principles meet the code**


These three principles are independent of language or tooling. They describe a way of thinking about data, not a specific implementation.


DiscoverDf is implemented as a reusable Scala function running on Databricks. An analyst passes a DataFrame and receives a structured profile with sixteen analytical insights per column.


**The unique value count across all columns becomes the new sorting key:** when the analyst arranges columns from highest to lowest cardinality, **the dataset organizes itself into the three layers we described. The What, the When, and the How emerge directly from the data** .


Each column is then explored in depth. Timestamp fields are standardized into date formats, and every column is enriched with a consistent set of signals. This includes null and zero distributions, minimum and maximum values, categorical and date counts, and statistical summaries.


For low cardinality dimensions, the function provides full category breakdown distributions. For date fields, it surfaces time distributions and monitoring signals. For high cardinality columns, it returns representative sample values.


The result is a structured view of the dataset that makes its shape, boundaries, and analytical potential immediately visible.


### **Seeing the full picture before the first query**


To illustrate how the function works, consider running DiscoverDf on a dataset with hundreds of millions of rows and dozens of columns.


Once the columns are sorted by unique values in descending order, a structure begins to emerge. The first block shows three columns with hundreds of millions of unique values for transaction IDs, followed by tens of millions for customer and account IDs. The scale and grain become immediately clear, **and the What is answered** . This is a transactional dataset where each row represents an individual event connected to a broader customer population.


The next block contains nine columns in the range of one to two thousand unique values. That cardinality range signals date fields. The analyst identifies the column that serves as the primary date reference, then checks its name, minimum and maximum values, and null distribution. The timeframe becomes clear within seconds, spanning multiple years up to the most recent records. **The When is answered** : the dataset captures years of transactional history.


At the bottom of the profile, eleven columns have fewer than seventy unique values each. These are types, statuses, flags, and segmentation fields, all presented with their full value distributions. **The How is answered** : the analyst can now see eleven distinct dimensions for filtering, grouping, and analysis before writing a single query.


## **Discovery as a practice, not just a tool**


One of the most important distinctions we learned to draw is between discovery and execution. DiscoverDf is designed for the first interaction with a dataset , when an analyst needs to build a complete mental model from scratch. Once that model is in place and the context is clear, day-to-day work naturally shifts to faster and more targeted queries.


The goal is not to replace the analyst’s workflow, but to compress the time that comes before it. What usually takes weeks of passive understanding becomes a structured and much faster process.


A second use case emerged over time: validation. When teams create new datasets or refactor existing pipelines, DiscoverDf provides a structured way to check whether the output matches expectations. It helps confirm that the grain is correct, that the temporal coverage is complete, and that categorical dimensions contain the expected values.


**Analytical Principles that outlast technology**


The most durable contribution of this work is the framework. The What, The When, and The How are principles that any analyst can apply in any environment, regardless of the stack. DiscoverDf is one way to implement them today on Databricks. The thinking behind it is meant to last beyond any specific technology.


The next step is already taking shape. As AI-powered workflows evolve, the discovery process described here does not have to remain manual. Agents can run structured profiling, surface key dimensions, and generate summaries before an analyst writes a single query. The human role shifts from exploration to interpretation and judgment, which is where analysts create the most value.


The bigger question, and the one worth pursuing as an organization, is what happens when this stops being an individual practice and becomes a team standard, embedded in how we onboard, document, and collaborate across teams. That is the point where a useful tool turns into a lasting capability.


**Why this matters beyond the tool**


DiscoverDf is more than a tool. It reflects a way of thinking that analytical teams need more of. One of the biggest challenges Squad Leads face is helping analysts reach their full potential quickly.


The onboarding tax described in this article is also a confidence cost. Analysts who spend their first weeks operating in dependency mode, asking for context and waiting for guidance, tend to internalize that dynamic. And once it is established, it takes time to unlearn.


When that dynamic is removed, the effect is immediate. When an analyst arrives at a conversation already understanding the dataset, already able to ask better questions or suggest directions, the interaction changes. It changes how analysts see themselves and how teams perceive their role.


At scale, this compounds. When everyone ramps faster and operates with greater autonomy, the impact raises the standard of what Nubank can achieve with data. This is a multiplier of team capability.


There is also a cultural dimension to this work. A team identified a structural gap, built a solution, and shared it. This instinct to solve for the team and not only for individual needs is a defining part of the analytics culture being built at Nubank. DiscoverDf is one example. Many others will follow. The important shift is that this mindset is becoming the norm.
