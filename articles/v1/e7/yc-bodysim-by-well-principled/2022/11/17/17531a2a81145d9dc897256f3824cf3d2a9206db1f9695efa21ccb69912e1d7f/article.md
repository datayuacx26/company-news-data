---
schema_version: "1.0.0"
document_id: "17531a2a81145d9dc897256f3824cf3d2a9206db1f9695efa21ccb69912e1d7f"
company_key: "yc-bodysim-by-well-principled"
company: "BODYSIM by Well Principled"
source_id: "yc-bodysim-by-well-principled-news-import-782e20cd8d85"
canonical_url: "https://wellprincipled.com/blog-posts/whats-wrong-with-data-driven-decisions/"
published_at: "2022-11-12T00:00:00+00:00"
first_seen_at: "2026-07-26T23:53:59.787640+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:e941f56d6d37fd5187812cb33a6b03cd5fd6b48f6d381b9b562fbb5c0225e46d"
---

# What’s Wrong With “Data-Driven Decisions”

#### The Mystery of Data -> Decision


If you’re like me, you’re probably about as tired of hearing people say “data-driven decisions” as you are of Kardashian memes, skinny jeans, and COVID data updates. But I submit to you that this rallying cry is not just tired, it is also just plain wrong.


What do we even mean when we say this? We could interpret it as pulling managers away from “gut feel” decision making, but that ignores the data inherent in the experiences from which they formed those beliefs. We could interpret it as pushing managers towards “quantitative” decision making, but this is also just linguistic sleight-of-hand. When you write down the steps clearly:


1. Collect Data
2. ???
3. Better Decisions!


Now we see the problem. What is the unspecified process to translate that quantitative data into a quantitative decision? In some cases, the “???” is summarizing the data into a KPI. Or maybe it’s building out an entire dashboard of charts and graphs. Many companies believe they can create value just by putting the data in front of their teams.


Obviously, there’s another option for that “???”: a model. A model is the theoretical understanding that posits how to *translate* this quantitative data into theoretical learnings that are transferable to other situations. Which bits are noise and which are noteworthy? What is incidental and what is causal? In the hard sciences, we spend a lot of time thinking formally about this process of “theory induction,” using the tools from “philosophy of science”1 . Listening with this in mind, you can frequently hear how your colleagues have induced valuable mental models from their experiences that “data-driven decisions” ignore at their peril:


- That’s just the effect of the pandemic; when it’s over, brick and mortar will come back
- Ignore that sales data, we were out-of-stock / on-promo / competitor was on on-promo
- Ignore it was on a holiday, we sold because we spent a ton of ads
- That SKU we cut wasn’t incremental, sales were going to dip anyway


These statements all implicitly establish a causal, theoretical model of how the world works. This expert theory-induction sounds like a lot of work, because it is. Worse, some may say an “experts in the loop” system is just an opportunity to introduce bias. So you’re surely familiar with a family of alternatives that offers a shortcut through that work and also dispenses with these possibly-biased experts: black-box statistical or machine learning models. Many analytics companies allow you to choose “the best model” from this family for prediction, but perhaps none of those models capture what these mental models do. Given infinite data, a more modern black-box A.I. could potentially produce expert-level results, but do you actually have enough? In fact, two black-box machine learning models seeing the same data can reach contradictory conclusions about how to steer the business next. How do you choose? How can businesses capture the potential value from data? Professor Anne Marie Knott, who chairs our Scientific Advisory Board, has a name for these sorts of models which points towards the solution: “atheoretical.”


#### Moving Beyond Atheoretical


This matters because in your quest for making more quantitative decisions, you must ensure that you aren’t blindly running your business on a black-box that turns data into bad decisions.


The well-principled way to think about this problem is to *explicitly* posit a model of business drivers: How will consumers behave in response to our products/promos/ads/distribution, and how does that translate into profits or losses? This allows us to interpret the data that validate causality of business drivers independently. Your multitudes of data are necessary to efficiently improve, but they are neither sufficient nor are they the hard part of the profitable decisions equation. The completed story of a data-driven business should read:


1. Collect Data
2. Train and Interpret Models
3. Better Decisions!


Industry leaders like Amazon, Netflix and even Dominos have already figured out this central role of models, and have generated out-sized returns2 .


But where do we start with this theoretical model building if a “black box” approach is insufficient? Beyond making money on MBA’s, business schools fund academic research that formalizes and refines intuitions and learnings of those oft-maligned “gut feel” managers. B-school academics separate the most important decision considerations from the chaff, and they define precisely which data we need, how to interpret it, and how to deploy it to make better business decisions.


So why didn’t your MBA program teach us all of these models? They are all available essentially publicly in the academic business journals3 . However, there is a challenge in making these models work at scale in a real world context. Oftentimes your price discount coincides with a shift in media spend or consumer sentiment. The model training necessary to pick apart these “concurrent causes” requires not just all of these causal theories, but also the incredibly large and sophisticated compute power of modern A.I. techniques that a spreadsheet just can’t muster.


At Well Principled, we have built such an engine with the goal of removing the “???” from your data driven decisions. Our platform gives the “gut feel” managers a chance to validate their assumptions with a quantified approach to decision making. Our robust set of thoroughly researched causal models provides transparent insight into what will improve sales, profitability and forecasting. We believe that better decision making in business will only happen from this kind of synthesized approach connecting data, models and the business professionals who have the depth of experience with the nuance of what makes consumers tick.


---


1.


[Our favorite intro philosophy of science book is: “Scientific Method in Practice” by Hugh Gauch](https://www.amazon.com/Scientific-Method-Practice-Hugh-Gauch/dp/0521017084%22%3Ehttps://www.amazon.com/Scientific-Method-Practice-Hugh-Gauch/dp/0521017084)↩︎


2.


[Models will Run the World by Steven A. Cohen, Wall Street Journal](https://www.wsj.com/articles/models-will-run-the-world-1534716720%22%3Ehttps://www.wsj.com/articles/models-will-run-the-world-1534716720)↩︎


3.


[like the illustrious Management Science](https://pubsonline.informs.org/journal/mnsc)↩︎
