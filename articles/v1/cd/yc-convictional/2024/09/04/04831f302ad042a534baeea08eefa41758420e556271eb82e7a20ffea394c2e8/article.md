---
schema_version: "1.0.0"
document_id: "04831f302ad042a534baeea08eefa41758420e556271eb82e7a20ffea394c2e8"
company_key: "yc-convictional"
company: "Convictional"
source_id: "yc-convictional-atom-2bd8494c6cc9"
canonical_url: "https://get.convictional.com/posts/using-criteria-to-make-decisions/"
published_at: "2024-09-13T00:00:00+00:00"
first_seen_at: "2026-08-09T21:02:07.552290+00:00"
fetched_at: "2026-08-09T21:02:10.293363+00:00"
content_hash: "sha256:1f4ceb3f884e8efe799b5bdc0a9b39671f978a0943d23816ca552494b42ea5a0"
---

# Using Criteria to Make Decisions

#


It's been almost four months since our team started using our product to make business decisions. Since then, we've noticed that our "decision awareness" has grown, especially around decision criteria.


Picking the right criteria in decisions is important. It helps ensure that decisions align with company goals, and aids in reducing our bias and emotion. People, not AI, perform all judgment tasks in our product, including choosing and ranking criteria and making the final decision. But there are still a lot of non-judgement jobs to be done in good decision making. When it comes to criteria, there is a lot the product does to help.


In May we started using the product to make business decisions here. Since writing this, our team has considered 656 business criteria over the course of 111 considered decisions. For decisions that were made, we averaged about 5 criteria. The number varied between 1 criteria and 8. Some decisions that are made in the product should, and are made quickly. Other, more impactful, irreversible decisions are worth slowing down for.


Users can write their own decision criteria or generate them using an AI-powered feature in the product. We've found the AI generated criteria to be extremely helpful. It's also improved over time. Our product builds business context over time toward our goal of enabling context-driven decisions. Our product team has also made a lot of improvements to the product since we started using it.


Since May, our platform has generated 68% of the criteria considered by our team and about 90% of these have been kept for decision evaluation. Likely the other 10% were dismissed by the user as irrelevant or low priority. The product continually impresses us by generating criteria we *should* consider but would not have without the product.


The product is built to be collaborative, and anyone invited to collaborate on a decision can work on the criteria or suggest new ones. We track all this activity in our timeline. Our team has debated and prioritized the criteria to use for a decision. I've learned that this aspect of decision collaboration is really useful. We learn about each teammate's angle on decision and having criteria ranked and evaluated in the product creates a culture of trust and transparency. I've noticed that when we make decisions outside the product, in a meeting perhaps, we rarely talk about criteria explicitly, let alone their relative priority. We have started including the Convictional meeting bot to our meetings, which records, transcribes and pulls out decision-relevant highlights from the meeting. However, we're experimenting with ways to influence better live decision making in real time.


For teams using the product, it's possible to analyze decision making more broadly across the organization, including the criteria that is used to decide. Decisions can be private to the team or public to the organization. In the realm of public decisions it can be useful to explore decision trends to ensure *how decisions are* *made* align with the business priorities and values.


This type of team decision analytics isn't exposed in the product yet, but thanks to Anthropic's Claude I became a data scientist 10 minutes ago and analyzed our team's decision criteria to see if there were categories of criteria that stood out.\[^1\] The analysis identified several categories of criteria that were used in decisions we made. The analysis doesn't include a deeper look at which criteria was favored or prioritized but is interesting to see how we make decisions here at Convictional:


## Criteria evaluated for decisions made:


- cost and financial considerations - 16% of criteria
- user experience and interface -16%
- integration and compatibility - 14%
- technical implementation and performance - 13%
- team and organizational impact - 12%
- strategic alignment and decision quality - 11%
- scalability and future growth - 9%
- market and customer relations - 7%
- other - 2%


If you're interested in improving decision making with Convictional you can sign up and use our research version for free.


---


**Notes**


\[^1\]: First, the criteria was transformed to vectors with a text vectorization method called TF-IDF (Term Frequency-Inverse Document Frequency). Next, K-means clustering was used to group semantically similar criteria. Finally before publishing this essay, our real data science expert Adam verified the work!


Interested in trying Convictional? Email us atdecide@convictional.com .
