---
schema_version: "1.0.0"
document_id: "ea558dfe02c820aa82ebad2f08e3e812ea72c3ba7487b456785ede377e8a304a"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/contextual-offer-recommendation-engine"
published_at: "2024-07-11T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:f4441481f83aa6f5af72db2828313e4316b87b625ebdfddd273be4085acfe2d0"
---

# Contextual Offer Recommendations Engine at Target

Privacy Note:


While providing personalized experiences, Target also prioritizes guest privacy by honoring applicable guest privacy choices, following legal requirements, and adhering to Target’s


[Privacy Policy](https://www.target.com/c/target-privacy-policy/-/N-4sr7p) .


Foreign Filing License -


FA/1291/CHE/2024


Imagine walking into a Target store or browsing


[Target.com](http://target.com/) , and


the offers that you see seem like they were picked just for you.


This isn't just a happy coincidence; it's the result of a fine blend of machine learning and retail strategy.


Target developed a new system called Contextual Offer Recommendation Engine (CORE), used to recommend personalized offers to each Target Circle guest. Under the hood, CORE is powered by a contextual


[multi-arm bandit model](https://en.wikipedia.org/wiki/Multi-armed_bandit) built on top of a rich custom feature set including transactions, promotions, and guest behavior that optimizes for guest engagement including offer adds and redemption.


At the heart of this joyful shopping experience is our team, Offer Personalization. We are an Artificial Intelligence/Machine Learning (AI/ML) team within Target Tech responsible for creating segmented offers for Target Circle members. In 2022, our team's scope increased, and we were tasked to improve offer performance in both offer engagement and incremental revenue.


In this blog, we will dive into how our recommendation system (


[Recsys](https://recsys.acm.org/)[)](https://recsys.acm.org/) addresses interaction sparsity and utilizes a deep neural agent to deliver highly relevant offers to our guests.


Creative illustration of the offers within a Target Circle bonus


What is Target Circle?


In Target Circle, Target's loyalty program, members receive personalized spend-based offers called


[Target Circle Bonus](https://www.target.com/l/target-circle-bonus/-/N-7xl7q) (TCB). CORE recommends customized TCB offers with specific spend and reward dollar amounts for Target Circle members. Multi-trip-based spend threshold offers involve a series of shopping experiences where each trip's spending must exceed a certain dollar threshold to earn a reward of a specified value. For example, the first image above illustrates an offer: "Make 2 qualifying purchases of $80 or more to earn a $15 reward in Target Circle earnings." These offers are designed to encourage frequent engagement with Target, rewarding consistent shopping behavior.


Our Goal


Our goal is to increase guest engagement and drive trips to Target by determining the most effective offers for our guests.


Our Solution


To address the need for personalization at a guest level, our team developed an advanced AI-driven contextual offer recommendation engine.


How CORE Works


Our contextual multi-arm bandit (CMAB) algorithm includes the state of the environment in the decision-making process, allowing context-specific decisions. CORE employs a combination of matrix factorization techniques and CMAB to generate pertinent offers. We start by pulling historic guest offer interactions to construct an interaction matrix. This matrix is highly sparse as individual guests might have only a few interactions with Target Circle offers. In the image below, you can see that the rows show individual guests, and the columns reflect offer indices.


Interaction matrix showing guest interactions with Target Circle offers


a) Non-Negative Matrix Factorization (NNMF)


is used to reduce sparsity in interactions. This is used because of its proficiency in uncovering latent features that represent underlying guest preferences and offer attributes. This method is particularly beneficial as it can capture the nonlinear relations in the data, thereby providing a deeper insight into user-offer interactions.


We apply NNMF to factorize the guest-offer matrix into matrices W (user matrix) and H (offer matrix).


The factorization can be represented as follows:


𝑉≈𝑊 𝑥 𝐻𝑇


where 𝑉 is the original interaction matrix, 𝑊 is the user matrix, 𝐻 is the offer matrix, and 𝐻𝑇 is the transpose of the offer matrix.


In the next steps,


the offer latent features


(H) will be extensively leveraged as


bandit's per


-


arm features


in the CMAB approach, aiding in the fine-tuning of personalized offer recommendations.


The reverse computation can be carried out to find an approximate interaction matrix


𝐼′ using the factorized matrices as:


𝐼′=𝑊 𝑥 𝐻𝑇


This 𝐼′


is the approximation of the original interaction matrix, which


retains most of the significant information from the original matrix. Each offer becomes a single arm bandit and guest features become context.


Aided by


the dense interaction data


𝐼′ the neural networks foster a stable environment where the loss function aptly gauges the divergence between predicted and actual values. This not only guards against overfitting but also improves the model's accuracy over successive iterations.


b) The CORE’s CMAB Workflow:


CMAB


excels in environments where the reward distributions of actions are not known


a priori


and must be estimated from observed outcomes. The algorithm iteratively refines these estimates, maximizing the expected reward. CMAB leverages contextual guest information compared to standard multi-arm bandits to ultimately provide more accurate recommendations.


Additionally, CMAB is adept at handling scenarios with


inherent uncertainties,


where outcomes of actions are unknown. Due to the sparse nature of guest-offer interaction data, the algorithm employs


risk-reward balancing


techniques


,


enhancing decision accuracy as it gathers more data.


Guest offer CMAB workflow


- Environment


: The environment provides


contextual guest information


and offers metadata which includes


latent offer features


(H), allowing the algorithm to make informed decisions.


- Reward


Estimation: The agent learns from observed rewards generated by the environment. Rewards are computed based on approximate Interaction in matrix


I′


.


- Agent


: The agent interacts with the environment, selecting offers based on state information and predicted rewards. It employs a


[Neural Epsilon-Agent approach](https://proceedings.mlr.press/v162/dann22a/dann22a.pdf) to balance exploration and exploitation. Here, a deep network is developed to approximate the expected reward for each offer.


c)


Neural Epsilon Agent


with two towers + common tower:


A neural Epsilon-Greedy agent is a type of reinforcement learning agent that combines the Epsilon-Greedy algorithm with a deep neural network. The neural network is used to approximate the expected reward for each action, based on the current context. One of the benefits of the Epsilon-Greedy algorithm is that it is simple to implement and easy to tune. CORE's deep learning agent has 3 main components:


- Guest Network


: This network handles the processing of the guest or context features. The output of this network is fed into the Common Network.


- Offer Network


: This network processes the features specific to each offer. The output of this network is also fed into the Common Network.


- Common Tower Network


: This network combines the information from both the Guest Network and the Offer Network and processes it through three more layers. The final output layer gives the action predictions or estimated rewards, which are used by the agent to make action selections.


```text
Agent Network                Guest network                             Offer network             | (256 units)                           | (256 units)             |                                       |             V (512 units)                           V (512 units)             |                                       |             V (256 units)                           V (256 units)             |                                       |             +------------------+     +--------------+                                |     |                                V     V                               Common Network                                   | (256 units)                                   |                                   V (1024 units)                                   |                                   V (512 units)                                   |                                   V (estimated reward)
```


d)


Maximizing Efficacy in Recommendations:


The


E-Greedy algorithm


works by selecting actions in one of two ways:


- With a probability


epsilon


, the agent will select a random action (exploration).


- With a probability


1-epsilon


, the agent will select the action that has the highest expected reward (exploitation).


Diagram of E-Greedy Algorithm


Epsilon is a hyper-parameter that determines the balance between exploration and exploitation. A high value for epsilon will result in more exploration, while a low value will result in more exploitation.


In uncertain environments, it's crucial to explore different options to gain more information. CMAB incorporates an exploration mechanism, which helps in discovering the effectiveness of various offers despite limited information.


- Explore Vs Exploit:


Exploration delves into testing different offers to uncover what truly resonates with guests for long-term gain. In contrast, exploitation focuses on presenting guests with offers that are most likely to engage them in the short term. The feedback gained from exploration is crucial in refining the future offer recommendations.


- Balancing the Trade-off: The right balance between exploration and exploitation is critical as the model learns from dynamic guest preferences, ensuring its effectiveness aligns with evolving market trends.


Balancing the explore and exploit tradeoff is similar to balancing a bias vs. variance tradeoff. High levels of exploration might result in less-than-optimal offers in the short term, whereas a low exploration rate can create an


echo effect


, where the recommendation model becomes restricted to its previous predictions for future learning.


A/B Testing - Results


For testing the model, we used a robust A/B experimentation framework built on a multi-variate stratified sampling method. We split the initial audience list into two groups:


- Variant (This group receives offers from CORE model)


- Control (This group receives offers from the production baseline method)


A holdout group is kept separate from both Variant and Control for incremental sales measurement. The primary metric of interest here is the


opt-in rate


, i.e. the percentage of guests who opted in or added the offer divided by the total number of guests who received the offer. The secondary metric here is the


Completion Rate


, defined as the percentage of guests who completed the offer from the initial pool of guests who opted in.


A/B Testing Audience with Test and Decision Metrics


We performed seven tests with CORE and observed a statistically significant positive lift in both of our engagement metrics in each test. The variant in the testing, CORE, enabled more guests to engage with


[Target.com](http://target.com/) , on the Target app, and in our sped a streamlined data preparation and model deployment process, using PySpark for efficient data handling and TensorFlow for machine learning, with model training and scoring orchestration on Kubeflow. CORE powered millions of recommended offers in 2023.


Next Steps


The Offer Personalization team's goal is to


make shopping with Target more engaging and fun


by presenting offers that guests are excited about, turning every trip into a uniquely tailored experience. This work has taken the mantel from personalization at a cohort level to a state of hyper-personalization at a guest level. The current implementation also allows for enhancements to bring multi-objective models, where many metrics can be optimized at once.


## RELATED POSTS


### Target AutoComplete: Real Time Item Recommendations at Target


By Bhavtosh Rath, July 25, 2023


A look at our Data Science team's patent-pending AI recommendation model


### Real-Time Personalization Using Microservices


By Amit Pande, Prathyusha Kanmanth Reddy, and Pushkar Chennu, May 11, 2023


How Target's Personalization team uses microservices to improve our guest experience


### Elevating Guest Repurchasing Behavior Using Buy It Again Recommendations


By Amit Pande and Rankyung Park, November 2, 2023


Target’s Data Science team shares an inside look at our Buy It Again model.
