---
schema_version: "1.0.0"
document_id: "30750ef983cef2a2183714e1ec6bace29b4297b391c189cd0cf7caab2dbb7149"
company_key: "yc-chatwoot"
company: "Chatwoot"
source_id: "yc-chatwoot-news-import-2810aa3d6c74"
canonical_url: "https://www.chatwoot.com/blog/50-000-installations-of-chatwoot"
published_at: "2024-05-30T00:00:00+00:00"
first_seen_at: "2026-07-24T01:18:13.394314+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:aee28926bff821161bd44d06469dd0368b2239db9fe31b623024146ecb661975"
---

# 50,000 Installations of Chatwoot

I’m excited to share that we have crossed 50,000 self-hosted production installations of Chatwoot.


### What does Chatwoot do?


Chatwoot is an open-source support tool built as an alternative to Intercom and Zendesk. Our goal is to simplify the communication between businesses and their customers.


We offer a live-chat feature for your websites and integration with social media platforms. Chatwoot has everything from initiating conversations to publishing help articles for your customers in one product. You can either host the product on your own servers or utilize our cloud services.


### 50,000 milestone


Getting to a 50,000 milestone was not an easy task. Getting someone to take effort to setup the project and using it in production had a lot of challenges.


In most of the open-source application projects, to experience the product, you typically have to spend significant time configuring it. However, this has been changing with the rise of open-core companies.


From the start, we have been committed to simplify the installation experience. Our goal was twofold.


The second goal was to reduce the maintenance. Looking at the logs, running the migrations and upgrading the service was getting tricker when we grew. We launched a command-line tool[cwctl](https://www.chatwoot.com/docs/self-hosted/deployment/chatwoot-ctl?ref=www-internal-blog.chatwoot.com) to make it easier. Now, the upgrades are simple,` cwctl --upgrade` would do everything to upgrade the service to the latest version.


During the onboarding process, we collect minimal information from each installation to understand its usage. Although there is a mixed take on telemetry, a thoughtfully implemented non-intrusive metric collection can provide valuable insights to improve the product.


Here is the metric showing our growth: It took approximately 15 months to achieve the first 10,000 installations, an additional 9 months to reach 25,000, and 8 more months to hit 50,000.


Here is a breakdown of the installation types. The majority of users are either using Docker containers or the native Linux installation.


### How did we arrive at this idea?


During 2016, with the experience at FreshWorks and the trend in business being built on top social media platform, we thought of creating a support tool for social media channels and registered[chatwoot.com](http://chatwoot.com/?ref=www-internal-blog.chatwoot.com) .


We hacked together an MVP, and shared it with many people.The initial feedback was positive. We had an unexpected launch on Product Hunt and even secured an interview at 500 startups, although we were ultimately not accepted. After the initial excitement, fewer people used our product and our enthusiasm to improve it faded quickly.


We have seen the[HackerNews comment](https://news.ycombinator.com/item?id=15590934&ref=www-internal-blog.chatwoot.com) requesting a "Gitlab for Intercom". However, it took us an lot more time to build the motivation to add more features and make it open-source. We added live-chat as a feature and open-sourced the project. We[launched it on Hacker News](https://news.ycombinator.com/item?id=21559139&ref=www-internal-blog.chatwoot.com) in 2019, and it got great responses.


Seeing the interest and the opportunity to create a business around the project, **Chatwoot, Inc** was officially incorporated. At the end of 2020, we decided to apply to[YCombinator](https://www.ycombinator.com/?ref=www-internal-blog.chatwoot.com) and were accepted. After YC, we raised a seed round and have been actively shipping updates to the product. We release new updates every month and marked our 100th release last February.


### What are the future plans?


**Business Model:** We've adopted the open-core business model, where 90% of the features are free and open source, available to all users. The remaining features are source-available, meaning you can view and contribute to the code. These features help us generate revenue and sustain the business.


**Features:** LLMs have a huge impact on the way customer service is done. AI has a main consideration in the features we plan for. Instead of features screaming that they are AI based, we expect the AI to be improving an existing workflow within the product.


We have published our thesis on AI on our website, outlining our plans to integrate LLMs into Chatwoot. See more at[chatwoot.ai](https://www.chatwoot.ai/?ref=www-internal-blog.chatwoot.com) .


Thanks for reading, let me know if you have any feedback.
