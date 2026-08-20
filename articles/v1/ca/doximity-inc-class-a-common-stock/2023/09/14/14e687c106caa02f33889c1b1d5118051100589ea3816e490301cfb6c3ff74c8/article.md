---
schema_version: "1.0.0"
document_id: "14e687c106caa02f33889c1b1d5118051100589ea3816e490301cfb6c3ff74c8"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/on-the-road-to-effective-data-analyses"
published_at: "2023-09-07T16:45:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:d435d15c226caef1564e520d8dc2c235411d93891560403a0dc9f5b22f3581d2"
---

# On the Road to Effective Data Analyses

In the modern business landscape, the significance of data analysis cannot be overstated. It empowers organizations to uncover insights, make informed decisions, and gain a competitive edge by deciphering the hidden patterns within vast datasets. Data analysis is the cornerstone of strategic innovation and effective decision-making in today's dynamic markets.


At Doximity, data is crucial to the success of business operations, playing a key role in every step of the product development cycle. From determining the potential of new features to testing and tracking their performance, data analysts and business intelligence analysts are critical to Doximity's success.


Data analysts are equipped with powerful data analysis tools. While the variety of tools makes data analysis enjoyable, it still requires practice and self-reflection to create engaging analyses that cater to their audience. Here are a few tips for making analyses more compelling and useful.


## Ask the right questions from your data


Every data analysis begins with an open-ended product question, which must be broken down into more specific questions related to the data. Taking the time to structure the analysis with well-defined questions can prevent analysts from getting lost in unnecessary details. Brainstorming early with colleagues and product managers can provide valuable insights and help identify and avoid potential data issues that could lead to flawed findings.


As an example, consider the open-ended production question - "Why is the engagement with component x of our app increasing/decreasing in the last 2 months?" Upon brainstorming, this leads to questions “closer” to the data, such as:


- Is this a local high/low or an all-time high/low? In particular, have we seen seasonality with this metric in recent years?
- Is there a competing component of the app whose engagement has gone down/up in recent times that is correlated with this component’s engagement going up/down?


## Write readable code


Although coding as a data analyst may not be as strict as that of a software developer, adhering to certain rules can make queries less error-prone and easier to review. For SQL queries, using Common Table Expressions (CTEs) can improve code readability and ease debugging. Organizing Python code into functions reduces repetition, especially when generating multiple charts with slight variations in input data frames.


**Breaking logical blocks of SQL into CTE’s makes code readable and easier to debug**


**Moving the code to generate charts into functions can reduce repetitive code**


## Visual representations are powerful


Selecting the right visualization for data analysis is a pivotal step in effectively communicating insights. It hinges on a nuanced understanding of the data's nature and the objectives of the analysis. Begin by comprehending the type of data you possess – whether it's numerical, categorical, or temporal – and the relationships you aim to highlight, such as comparisons, trends, or distributions. Consider the audience too; the chosen visualization should resonate with their level of expertise and help convey the message clearly. From bar charts and line graphs to heatmaps and scatter plots, each type of visualization has its strengths. Strive to match the visualization's characteristics with the inherent traits of your data. Ultimately, experimentation and iteration play a role – don't hesitate to try different options and refine your choices based on how effectively they convey insights, and aid in the decision-making process.


## Add a summary of findings on the top (BLUF)


In military communications, BLUF stands for “bottom line up front,” meaning put the most important details first. To make analyses more reader-friendly, begin with a bulleted summary of the main findings. This allows stakeholders to immediately grasp the key insights without having to delve into lengthy analyses, acknowledging readers' time constraints while elevating the impact of the insights offered.


## Give the user control of the narrative


Make the charts interactive by adding filters (e.g., date filters) to provide users with control over the data they want to explore. This feature encourages them to observe insights they may not have originally considered, promoting a collaborative approach to data analysis. We like to think of this as crowd-sourcing data analysis. 😜 At the same time, making a dashboard too “filter happy” can convolute usability of the dashboard, and at the back end, may have unintended consequences when various filters interact with each other. Striking the right balance requires engaging stakeholders in a dialogue to determine which filters prove most beneficial.


## Iterate on the Analysis


Resist the urge to share analyses with stakeholders immediately after answering the initial questions. Data analysis is both a science and an art. Writing SQL queries can put us in a somewhat rigid and almost mechanical frame of mind, thus limiting our ability to think creatively. Therefore, taking a little more time to review and revisit the analysis with fresh eyes can lead to new discoveries and more creative insights. Consider sharing your preliminary findings with a peer for early feedback and additional ideas as well.


## Take a leaf out of others’ notebooks


Encourage a culture of reviewing and sharing data analyses within the organization. Here at Doximity, we ensure that every analysis gets code reviewed by another analyst before it goes into production. This process ensures accuracy and adherence to best practices, while providing analysts with the opportunity to learn from each other and incorporate new techniques into their work.


In conclusion, mastering the skill of data analysis takes time, practice, reflection, and learning from others' experiences. By following these tips and continuously improving their craft, data analysts can make their work more effective and enjoyable.


---


Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech) if you'd like to be notified about new blog posts.
