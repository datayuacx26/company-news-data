---
schema_version: "1.0.0"
document_id: "638f0a644db39d25a8e767db3049a142f5ae0ef374f17e741582d665b2b911a3"
company_key: "s-p-global-inc-common-stock"
company: "S&P Global Inc."
source_id: "s-p-global-inc-common-stock-rss-ff630ac34bbe"
canonical_url: "https://engineering.global.com/to-what-extent-is-estimating-effort-beneficial-to-squads-does-it-help-or-hinder-us-edb82070b0f4"
published_at: "2024-06-10T10:02:36+00:00"
first_seen_at: "2026-07-20T04:36:47.908335+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:6b099b6c24795cf4203a90dc0ae68ba1ad7d9f243109c80eaadf2cb4c26bd738"
---

# To What Extent Is Estimating Effort Beneficial to Squads: Does It Help or Hinder Us?

Press enter or click to view image in full size


# **To What Extent Is Estimating Effort Beneficial to Squads: Does It Help or Hinder Us?**


[Global Engineering](https://global-engineering.medium.com/?source=post_page---byline--edb82070b0f4---------------------------------------)


4 min read


·


Jun 10, 2024


--


*By James Lawrence*


In the world of agile development, estimating effort is a common practice. It involves predicting the amount of work required to complete a task, often quantified using story points. But does this practice actually benefit software teams, or does it create more obstacles than it removes?


**Key Definitions**


Before we go any further, it’s important to align on the meaning of the two terms below:


- **Estimating effort** helps teams plan the amount of work they can realistically complete within a given timeframe.
- **Story points** are numerical values assigned to tasks based on their complexity, often following the Fibonacci sequence (0, 1, 2, 3, 5, 8, 13, etc.). These numbers help teams gauge the relative effort required for different tasks.


When I first encountered these concepts, they seemed straightforward. However, I soon realised that estimation practices are highly subjective and can vary significantly between teams. This lead me to explore whether estimating effort is truly beneficial or if it might actually hinder team productivity; comparing the use of Story Points with the popular industry standard approach of #NoEstimates.


**Using Story Points**


In my role as a Delivery Manager for the Radio Fulfilment (aka gSchedule) team, I observed that our team lacked a consistent estimation process. The velocity (the number of story points committed vs. completed per sprint) appeared low due to inconsistent ticket sizing (despite a lot of work being done). This made it difficult to report on performance, confidently plan work going into sprints and create timelines.


To address this, I implemented a bucket system for estimation, where the team quickly discussed and sized all tickets into swim lanes. This method helped us calculate a more accurate velocity, averaging 56.4 points per sprint compared to the previous 9.4 points. The benefits were clear:


- **Accurate Reporting** : Consistent estimation enabled more accurate performance tracking.
- **Credible Planning** : With reliable data, we could commit to release dates with greater confidence.
- **Enhanced Coordination** : Our improved planning process facilitated better coordination with other teams, especially as we moved towards self-service.


**The No Estimates Approach**


Not all teams find value in estimating effort. The #NoEstimates movement, popularized by Woody Zuill, questions the purpose and effectiveness of traditional estimation methods. Here’s why some teams prefer not to estimate:


- **Focus on Value** : Shifting focus from estimating tasks to delivering customer value encourages prioritizing features based on their importance rather than complexity.
- **Reduced Overhead** : Estimating can be time-consuming and sometimes leads to unproductive disagreements within the team.
- **Improved Collaboration** : Less time spent on estimation means more time for collaboration and breaking down work into smaller, manageable tasks.


For instance, the gPO team works in Kanban and does not estimate effort. Instead, they track the average weekly number of tickets completed. This then allows them to monitor throughput by comparing the issues created versus resolved against their average.


## Get Global Engineering’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**Comparing the Two Approaches**


Both techniques have pros and cons; their effectiveness can also depend on the team’s experience and working style.


### **Story Points**


**Pros** :


- **Versatile Planning** : Allows teams to adjust their plans based on the specific work at hand.
- **Momentum** : Anecdotally, developers who size their own work can feel a greater sense of ownership and drive to meet delivery goals.


**Cons** :


- **Time-Consuming** : Estimation can lead to lengthy discussions and disagreements.
- **Inconsistent Reference Points** : Without regular reflection, the accuracy of estimates can vary over time.


### **No Estimates**


**Pros** :


- **More Time for Development** : Developers spend less time on estimation and more on actual development.
- **Agile Focus** : Prioritises features based on user value rather than complexity.


**Cons** :


- **Consistency and Experience in Story Splitting Required** : Throughput tracking without estimating requires tickets to be of a similar size, so that the calculated average accurately represents the throughput. Stories must be broken down consistently to the same granular level eg. ~3 days or less to complete each Story.


**Conclusion: Does Estimating Effort with Story Points Help or Hinder?**


Based on my personal experience, I believe that estimating with Story Points is beneficial. Particularly for newer or changing teams that could benefit from a clear structure and more driven communication. For the Radio Fulfilment example, implementing a consistent estimation process improved visibility over our throughput and planning accuracy.


It’s important to note that, estimation practices should evolve with the team — it’s important to keep the technique relevant to current performance and needs.


Ultimately, the approach used to estimate effort depends on the team. It is a subjective technique and there isn’t a “correct” way of doing it. Teams should choose the method that best supports their workflow, ensuring they base their planning and reporting on accurate data. Regardless of the approach, tracking progress consistently and with team buy-in is crucial for delivering work with credibility.
