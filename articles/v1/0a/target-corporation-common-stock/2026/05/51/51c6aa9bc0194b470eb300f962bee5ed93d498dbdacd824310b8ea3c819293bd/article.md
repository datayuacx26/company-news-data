---
schema_version: "1.0.0"
document_id: "51c6aa9bc0194b470eb300f962bee5ed93d498dbdacd824310b8ea3c819293bd"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/targetrun-shopping-offers"
published_at: "2026-05-06T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T22:12:46.079122+00:00"
content_hash: "sha256:fbcac60e6e015b9dc04982f7e9ecd8900fc014159ffbffbc66070be927ac2637"
---

# TargetRun: AI-Powered Trip Prediction for Delivering Timely Offers

Privacy Note:


At Target, personalization is built responsibly. While providing personalized experiences, we honor guest privacy choices, follow applicable legal requirements, and adhere to Target’s


[Privacy Policy](https://www.target.com/c/target-privacy-policy/-/N-4sr7p) .


Have you ever opened the Target app and found an offer that showed up at just the right time, like a deal on something you were already planning to buy?


That kind of relevance isn’t accidental. It’s the result of systems designed to understand common shopping patterns and help make everyday trips a little easier.


TargetRun, a deep learning model built by Target’s ML/AI Offer Personalization MarTech team, helps estimate when a guest might shop next, so offers can be timed and structured in a way that feels useful.


In this post, we’ll look at how TargetRun works, how it solves common data challenges in retail forecasting, and how better predictions translate directly into better guest experiences.


Using shopping patterns to make more useful offers


Every successful personalized offer starts with a simple question: How many times is a guest likely to shop in the next few weeks?


To answer that, we use TargetRun. By predicting a near-term trip count, the model helps determine what kinds of deals you should see. For example, if a guest typically shops twice in a given period, offers can be designed to fit naturally into those trips.


These forecasts flow directly into Target’s contextual offer recommendation engine


(


[read more](https://tech.target.com/blog/contextual-offer-recommendation-engine) )


, guiding which deals a guest sees and how those deals are structured. The result is offers that are easier to use in real life and aligned to how guests already shop.


Model design: Learning a guest’s shopping rhythm


Traditional forecasting models often treat shopping interactions as a series of disconnected moments rather than a whole story. That approach misses something essential: Shopping is inherently sequential.


TargetRun is designed to learn patterns over time. Instead of looking at isolated data points, it models a guest’s shopping history as a continuous sequence, capturing rhythms such as weekly trips, seasonal surges, or pauses after major promotions.


A) The Building Blocks of Prediction: Sequential and Static Features


Learning from sequences, not snapshots


At the core of TargetRun is a Long Short-Term Memory (LSTM) based model, a type of neural network designed specifically to learn from sequences. Rather than treating each shopping moment in isolation, the LSTM allows TargetRun to understand how a guest likes to shop over time.


This approach helps the model recognize patterns such as:


- Seasonal shopping routines


- Impulse buying


- Steady repeat visits


By learning this data, TargetRun can forecast near-term trip patterns with greater accuracy than non-sequential models such as fully connected neural networks.


Not every moment in a shopping history carries the same weight. Some weeks are more informative than others.


To account for this, TargetRun uses an attention mechanism that dynamically highlights the most relevant parts of a guest’s recent history when making a prediction. This enables the model to focus on signals such as recent changes in engagement while still considering longer-term patterns.


Balancing history with long-term context


While recent behavior is critical, some guest characteristics change slowly over time. TargetRun combines sequential shopping patterns with stable, guest-level context to form a unified view.


This balance ensures predictions reflect both:


- What a guest has been doing lately


- Who that guest is over the long term


Together, these elements allow TargetRun to generate trip forecasts that are both responsive and grounded.


Data creation: Building a complete view of guest behavior


To accurately predict a guest’s shopping habits, TargetRun needs a comprehensive set of signals.


Sequential data:


To train TargetRun, we use one year (52 weeks) of guest data (guest spend habits, digital activity, etc.) rolled into ordered shopping sequences. Predictions are needed for two weeks and three weeks ahead. To account for multiple horizons, we aggregate training data windows of two weeks and three weeks aligned with each prediction window.


Because we forecast across multiple horizons, we aggregate those weeks into windows aligned to each target. For a two-week forecast, we combine weeks into biweekly steps, yielding a 26-step sequence per guest.


This lets the model learn patterns such as seasonal surges, post-promotion cooldowns, and steady repeats.


Static data:


Not all guest attributes change from week to week. Some signals provide important context about long-term shopping propensity. TargetRun incorporates static, guest-level features, such as guest engagement and membership details, that help anchor predictions in a broader understanding of who the guest is over time. These are later combined with the sequential data to ensure forecasts are informed by guest behavior and context.


B) Model Architecture


At each point in time, the model acts like a smart filter: It forgets old and no longer relevant behaviors, writes new information to its memory, and surfaces only the important insights to make its next prediction.


To add another layer of sophistication, the model considers the full context of a guest’s shopping journey, not just the past. This is achieved via Bi-directional LSTMs (Bi-LSTM), which capture both earlier and later context in a guest’s history.


On top of this, the model uses a multi-head attention mechanism to intelligently spotlight relevant weeks allowing the model to focus on the most important signals for its prediction, ensuring it finds complex patterns that span across time.


Next, these sequential patterns are refined and stabilized to ensure the signals are consistent. Concurrently, the model processes static data such as long-term guest habits to create a concise summary of the guest's overall profile. Finally, the dynamic shopping rhythm and the stable, long-term profile are merged, forming a single, unified view of the guest. A final prediction layer evaluates this complete picture to calculate the exact probability of how many trips the guest will make during the chosen timeframe.


C) Training Recipe


Before training, we make sure the data the model sees reflects the audience and outcomes we care about. The raw data was lopsided, presenting two major imbalances that could have skewed predictions.


1. Solving Audience Imbalance


High-engagement shoppers, who drive outsized value, were a minority in the raw data. Training without rebalancing might have missed the mark on these critical customers.


To fix this, TargetRun uses a technique called


importance weighting


to adjust the sample representation during training, ensuring the model learns from the customer mix Target intends to serve.


2. Focal Loss


Because predicting these rare, high-value interactions like someone shopping four times in a few weeks is hard but crucial, TargetRun uses Focal Loss, which down-weights easy wins and up-weights hard misses, ensuring the model achieves accuracy across all customer moments, not just the easiest ones.


The Ultimate Test: Proving it via live A/B testing


Offline accuracy is important, but the true test of a forecasting model is whether it improves real guest experiences. To validate TargetRun, we ran a controlled live A/B experiment to measure its impact on both campaign performance and prediction quality.


Test Design


The experiment evaluated how improved trip forecasts influenced personalized offers across different guest engagement levels.


- Audience: High (H), Medium (M), and Low (L) engagement guests.


- Sample: Stratified split: 20% to TargetRun and 80% to Trip Count Predictor (fully connected neural network).


- Treatment vs. Baseline: Both arms received offers from CORE (


[read more](https://tech.target.com/blog/contextual-offer-recommendation-engine) ) The trips input came from TargetRun (treatment) vs. Trip Count Predictor (baseline).


- Horizon: Three-week period, consistent with campaign cycles.


Campaign Outcomes: Creating the 'Joy of Completion'


TargetRun led to more people opting in, completing and redeeming offers across all levels of participation.


There was a dramatic 78% lift in redemption among guests who rarely redeem offers. This shows the model’s ability to learn from sparse data available for guests who engage less often.


This success shows the AI was creating offers that resonated with and felt attainable for those guests. These results were driven by more accurate predictions of how guests actually shop.


Model Performance:


The new model was 15% more accurate, and its overall predictive power increased by over 40%. However, the most meaningful improvement was the 44% reduction in over-prediction. It means the model stopped setting goals that were unrealistic for many guests. Instead, it started creating realistic offers that shoppers could achieve.


Looking Ahead: The Next Evolution of Prediction


TargetRun’s success is just the starting point. It has now replaced our previous baseline model, raising the bar on guest-first personalization.


The next evolution goes beyond predicting


if


a guest will shop, to understand


where


that trip is mostly likely to happen, whether in the app or on Target.com, or in a store. Knowing the channel will help ensure we present the right offer in the right place and moment, so every trip feels easy, exciting, and uniquely personal.


## RELATED POSTS


### Contextual Offer Recommendations Engine at Target


By Utsav Mishra, July 11, 2024


Learn more about how Target's CORE recommends personalized offers to guests
