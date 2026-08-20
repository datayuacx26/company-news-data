---
schema_version: "1.0.0"
document_id: "9d3f4d733c58a055d51c37f0b652615f2998071392d1abcdf7f60ef17ed1f3a5"
company_key: "nextdoor-holdings-inc-class-a-common-stock"
company: "Nextdoor Holdings Inc."
source_id: "nextdoor-holdings-inc-class-a-common-stock-rss-10247875dc01"
canonical_url: "https://engblog.nextdoor.com/let-ai-entertain-you-increasing-user-engagement-with-generative-ai-and-rejection-sampling-50a402264f56"
published_at: "2023-10-16T17:03:52+00:00"
first_seen_at: "2026-07-20T23:17:36.074163+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:79711c8a2e9843d77ddce466029ff82c582074bc2a152eae742ac47ce2be56b8"
---

# Let AI Entertain You: Increasing User Engagement with Generative AI and Rejection Sampling

Generative Ai


ChatGPT


Notifications


Model Training


A B Testing


# Let AI Entertain You: Increasing User Engagement with Generative AI and Rejection Sampling


[Jaewon Yang](https://medium.com/@jaewonyang?source=post_page---byline--50a402264f56---------------------------------------)


11 min read


·


Oct 16, 2023


--


Generative AI (Gen AI) has demonstrated proficiency in content generation but does not consistently guarantee user engagement, mainly for two reasons. First, Gen AI generates content without considering user engagement feedback. While the content may be informative and well-written, it does not always translate to increased user engagement such as clicks. Second, Gen AI-produced content often remains generic and may not always provide the specific information that users seek.


Nextdoor is the neighborhood network where neighbors, businesses, and public agencies connect with each other. Nextdoor is building innovative solutions to enhance the user engagement with AI-Generated Content (AIGC). This post outlines our approach to improving user engagement through user feedback, specifically focusing on Notification email subject lines. Our solutions employ Rejection sampling \[1\], a technique used in reinforcement learning, to boost the engagement metrics. We believe our work presents a general framework to drive user engagement with AIGC, particularly when off-the-shelf Generative AI falls short in producing engaging content. To the best of our knowledge, this marks an early milestone in the industry’s successful use of AIGC to enhance user engagement.


## Introduction


At Nextdoor, one of the ways to drive user growth and engagement on platform is through emails. One of the emails we have is called New and Trending[notifications](https://engblog.nextdoor.com/nextdoor-notifications-how-we-use-ml-to-keep-neighbors-informed-57d8f707aab0) , where we send a single post that we think the user might be interested in and want to engage with. As part of sending an email, we need to determine a subject line of the email for the email audiences. Historically, we simply pick the first few words of the post being sent to be the subject line. However, in certain posts, these initial words are often greetings or introductory remarks and may not provide valuable information to the user. In the provided image example below, we observe a simple greeting, “Hello!”


Press enter or click to view image in full size


Figure 1. New and Trending email where we show a single post. Prior to the Gen AI systems we build, we use the first words of the post as the subject line (Life and Mother Nature always find a way!)


In this work, we aim to use Generative AI technologies to improve the subject line. With Generative AI, we aim to generate informative and interesting subject lines that will lead to more email opens, clicks and eventually more sessions.


Writing a good subject line with Generative AI is challenging because the subject line needs to satisfy the following criteria. First and foremost, the subject line needs to be engaging so that the users want to open the email. To see if ChatGPT API can write engaging subject lines, we tried generating subject lines with ChatGPT API with a small traffic A/B test, and found that the users are less likely to click on emails if we use subject lines made by ChatGPT API (e.g. Table 1). As we show later, we tried to improve the prompts (prompt engineering) but the results were still inferior to the user-generated subjects. This finding implies that Generative AI models are not trained to write the content that is particularly engaging to our users, and we need to guide Generative AI models to increase user engagement.


Press enter or click to view image in full size


Table 1. Subject line made by ChatGPT API and its CTR. ChatGPT API’s subject line is more informative but looks like a marketing phrase, and produced only 56% clicks compared to the user-generated subject line.


Second challenge is that the subject line needs to be authentic. If it reads like a marketing phrase, the email will look like spam. The example in Table 1 “Support backyard chickens in Papillion, NE!” shows this issue.


Third, the subject line should not contain hallucinations (a response that is nonsensical or not accurate). And it is well known that Generative AI is vulnerable to hallucinations \[2\]. For example, given a very short post saying “Sun bathing ☀️”, ChatGPT API in Table 1 generated the subject line “Soak Up the Sun: Tips for Relaxing Sun Bathing Sessions”, which had nothing to do with the post content.


We developed a novel Generative AI method to overcome the three challenges faced by the ChatGPT API mentioned above. We made three contributions:


- **Prompt engineering to generate authentic subject lines with no hallucination:** Given a post, ChatGPT API creates a subject line by extracting the most interesting phrases of the post without any rewriting. By extracting the user’s original writing, we are able to prevent marketing phrases and hallucinations.
- **Rejection sampling with a reward model:** To find the most interesting subject line, we develop a reward model whose job is to predict if the users would prefer a given subject line over other subject lines. After ChatGPT API writes a subject line, we evaluate it by the reward model and accept it only if its reward model score is higher than the user-written subject line’s score. This technique is called Rejection Sampling and recently introduced to Reinforcement Learning for Large Language Model training \[1\].
- **Cost optimization and model accuracy maintenance** : We added engineering components to minimize the serving cost and stabilize the model performance. By using caching, we reduced our cost to 1/600 compared to the brute-force way. By daily performance monitoring, we can catch if reward models fail to predict which subject is more engaging due to external factors such as user preference drift and address it by retraining.


We believe that this framework is generally applicable when off-the-shelf Generative AI fails to improve user engagement. We also analyzed the importance of each component in our design. Even with the aforementioned prompt engineering, ChatGPT API did not necessarily produce more engaging content. This highlights the necessity of the rejection sampling component: in such cases, we can develop another AI model as a reward model and use the Generative AI’s output only if the reward model approves \[1\].


## Proposed Method


For every post, we employ the following system to create a subject line. It’s important to mention that we generate a single subject line for each post, without personalization. This decision was made to minimize computational cost. Exploring cost-effective methods for implementing personalized subject lines will be an interesting future work.


### Model Overview


Figure 2 illustrates our approach. We develop two different AI models.


- Subject line generator: This model generates a subject line given a post content.
- Reward model (Evaluator): Given a subject line and the post content, this model predicts if the given subject line would be the better subject line than the user-generated subject line.


Press enter or click to view image in full size


Figure 2. Overview of our approach.


Given a post, the Subject line generator produces subjects in Figure 2 (green boxes). The reward model compares the OpenAI API subject line (green) with the user-generated subject line (red), and selects the more engaging one. For the top post, the OpenAI API subject line contains more relevant information and is selected. For the bottom post which was about a health alert, the reward model selects the user-generated subject. While the OpenAI API subject line shows the main content of the alert, the reward model picks the user-generated subject because it shows the importance of the post and thus is more engaging.


### Developing Subject Line Generator


We use OpenAI API without fine-tuning. In the prompt, we require that OpenAI API extracts the most interesting part of the post without making any change. This way of extracting user content provides multiple benefits: First, it removes hallucinations. Second, it keeps the subject line authentic as OpenAI API does not rewrite the original content. To test the prompt engineering, we A/B tested generator outputs without reward models. We found that asking OpenAI API to extract in the prompt improves Sessions by 3% relatively compared to asking OpenAI API to rewrite the subject line from scratch (See the Results section for the details).


### Developing Reward Model


We fine-tune OpenAI API to develop a reward model. This is the main innovation we applied on top.


**Training data collection:** The challenge is to collect training data on which subject line was more engaging. Manual annotation is not possible because there are no rules deciding what subject line is more engaging. We found that the subject lines that we thought to be more engaging than the user-generated ones turned out to be less engaging (Table 2).


Press enter or click to view image in full size


Table 2. Emails with a user-generated subject (left) generated 3x as many clicks as the emails with OpenAI API-generated subjects on the right.


To tackle this issue, we collect training data via experimentation. For each post, we generate subject lines in two ways. One way is to use user-generated ones and the other is to use the OpenAI API generator described above. Then we serve 2–3% users (~20k) that are randomly selected with each subject line. The goal is to learn which subject line was more engaging through click data.


## Get Jaewon Yang’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**Model training:** We used OpenAI API to fine-tune with the labels we collected. We used ~50k examples and 40% of examples had the OpenAI API subject as the winning subject and the rest had the user subject as the winner. Given a subject line and post content, our model is fine-tuned to predict if the subject line would generate more engagement (clicks) than the user-generated subject line. The model is asked to predict if the subject line is more engaging and output “Yes” or “No”.


**Training details:** We used the smallest OpenAI API model “ada” for fine-tuning. We found that larger models did not improve the predictive performance despite higher cost. We added[a logit bias](https://help.openai.com/en/articles/5247780-using-logit-bias-to-define-token-probability) of 100 for “Yes” and “No”. These biases boost the probability for the model to output “Yes” or “No”. We tried to change the number of epochs and selected the model with 4 epochs, but we did not see much difference in offline performance after 2–3 epochs.


**Engineering details:** We added the following components for optimization and safeguarding.


- **Caching:** For each post, we cache the outputs of our model. By processing each post only once, we reduced the cost to 1/600. In other words, each post gets sent 600 times on average and we process the post only once instead of 600 times. Caching also optimizes the OpenAI API usages (the number of tokens and the number of requests).
- **Reward model performance maintenance** : We monitor the reward model’s predictive performance daily, using the next day’s user clicks after the training phase as the ground truth to compare with the model’s output. Model’s predictive performance can change because our users’ preference may change and the content in Nextdoor can shift in the writing styles or topics.
For monitoring purposes, we collect the engagement performance of different subject lines in the following way. We created a “control” user bucket where we always send emails with the user-generated subject and a “always OpenAI API” bucket where we always send with the OpenAI API subject, regardless of the reward model’s output. From these two buckets, we know the ground-truth on which subject line was more engaging, and measure the reward model’s accuracy. If the accuracy goes down by 10+%, we retrain the reward model with new data.
- **Retries with Fallback:** Since OpenAI API may return an error due to the rate limit or transient issues, we added retries with[exponential backoffs with Tenacity](https://tenacity.readthedocs.io/en/latest/) . If we fail after a certain number of retries, we fallback to the user-generated subject.
- **Controlling the length of output:** We found that the Subject line generator would write a subject line longer than our desired length (10 words). This happened even if we specified the 10 word limit in the instruction and added examples. We post-processed the generator output by cutting the first 10 words from the generator’s output. We A/B tested different word limits and found that 10 is the optimal value.


## Results


We did A/B tests with different versions of the subject line generator, and with and without the reward model. For the generator, we tested the following options


- Writing with OpenAI API: We ask OpenAI API to “write an engaging subject line for a given post”. This was the first version we tested without much prompt engineering.
- Extracting with OpenAI API: We ask OpenAI API to extract the most interesting part and provide 5 examples. We also add requirements in a numbered list such as “Do not insert or remove any word.”, “Do not change capitalization”, “If the first 10 words are interesting, use them as a subject line”. We tried 4 different versions of prompts and picked the best version by A/B test metrics.


For the A/B test metrics, we primarily focus on Sessions. A session is an activity sequence made by the same user within a certain timeframe, and sessions quantify the number of unique user visits.


Table 3 shows the results on Session lift compared to the “control” bucket where we use user-generated subject lines. In addition to the session metrics, our final model (last row) increased Weekly Active Users by 0.4% and Ads revenue by 1%.


Press enter or click to view image in full size


Table 3. Session lift compared to the user-generated subject lines from A/B tests. The final model (last row) achieved 1% lift in sessions.


Here is what we learned from A/B tests:


- Prompt engineering improves the performance but has a ceiling. After a few iterations, the A/B test metrics showed only marginal improvements, failing to beat the control.
- Finding the “optimal” prompt is an elusive task, as the space of potential prompts is boundless, making it difficult to explore. Moreover, there is no established algorithmic or systematic method for enhancing prompts. Instead, the task relies on human judgment and intuition to update the prompt.
- Reward model was the key factor in improving sessions.
- Predicting popular content is challenging, as is the reward model’s task of forecasting popular subject lines, which currently achieves about 65% accuracy. Enhancing the reward model’s performance by leveraging real-time signals like the current engagement numbers for the subject can be an interesting future work.


## Conclusions


We developed a novel Generative AI system to increase user engagement by combining the reward model and prompt engineering. Our systems have engineering components for cost saving and monitoring. A/B tests showed that our systems can deliver more engaging subject lines than the user-generated subject lines.


There are many avenues for future work. First is to fine-tune the subject line generator. In this work, we used vanilla ChatGPT API as the generator. Instead, we can fine tune OpenAI API with the most engaging titles that the reward model identifies. For each post, we generate multiple subject lines and use the reward model to pick the winner. Then we use the winner subject to fine tune the subject line generator. This approach is called Reinforcement Learning by Rejection Sampling \[1\].


Second is to rescore the same post daily. Currently, we pick the best subject line with a reward model once and never rescore. However, as time goes on, we may be able to see which of the OpenAI API subject line or user-generated subject line is getting more engagement, and our reward model can predict more accurately. Third is to add personalization without significantly escalating computational costs.


## Acknowledgments


The post was written by[Jaewon Yang](https://www.linkedin.com/in/jaewonyang/) and[Qi He](https://www.linkedin.com/in/qi-he/) .


This work was led by the Generative AI team with cross-org collaboration between Notification team and ML teams. We would like to give a shout out to all the contributors:


[Jingying Zeng](https://www.linkedin.com/in/joyzengjy/) ,[Waleed Malik](https://www.linkedin.com/in/malikwaleed/) ,[Xiao Yan](https://www.linkedin.com/in/xiaoyan2/) ,[Hao-Ming Fu](https://www.linkedin.com/in/hao-ming-fu/) ,[Carolyn Tran](https://www.linkedin.com/in/carolyn-tran-4904759b/) ,[Sameer Suresh](https://www.linkedin.com/in/ssuresh2/) ,[Anna Goncharova](https://www.linkedin.com/in/annabgoncharova/) ,[Richard Huang](https://www.linkedin.com/in/richardhuang11/) ,[Jaewon Yang](https://www.linkedin.com/in/jaewonyang/) ,[Qi He](https://www.linkedin.com/in/qi-he/)


Please reach out to us if you are interested to learn more — we are hiring!


## References


\[1\] Touvron et al. Llama 2: Open Foundation and Fine-Tuned Chat Models, Arxiv preprint, 2023


\[2\] Ji et al. Survey of Hallucination in Natural Language Generation, ACM Computing Surveys, 2022
