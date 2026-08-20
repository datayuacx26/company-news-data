---
schema_version: "1.0.0"
document_id: "84cb0ead5cc3400366ef2575bfb268502f10bd9cdf3d98f214a14e4bc0f9ca99"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/big-data-analytics/kaggle-predictive-analytics-lessons-learned"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:f4666d7a9a1bae98a240fb00bc405744327f560119eb97a4728dca2cb463074e"
---

# What Kaggle taught us about predictive analytics

Last year, CGI’s data science team from Prague had the great honor of winning the[Kaggle purchase prediction challenge sponsored by Allstate](https://www.kaggle.com/c/allstate-purchase-prediction-challenge) , competing against 1,500 teams worldwide, and I wanted to share some of our lessons learned.


[Kaggle](https://www.kaggle.com/) is an open community where top data scientists can solve complex business problems and learn the latest techniques. It works with major organizations (e.g., Amazon, Facebook, GE, Microsoft, NASA, etc.) as well as top universities, and is a recognized reference for professional competence.


The Allstate challenge was to predict the most probable choice of a combination of insurance products purchased by a customer, based on demographic profiles and history of recently viewed products. Ideally, the answers could help the company reduce churn and increase acquisitions by offering the right combination of products early in the decision process.


**Takeaway #1: Pre-processing**


A key observation made by our team was the importance of properly understanding the provided datasets. You can’t make a great model just by putting data into a machine and expecting great results. Data pre-processing is 70%-80% of the work.


Our Allstate challenge solution was based on three independent models. The data we used for each model was different. The first just used the data as provided. The second added variables such as mean cost in each state and location. The third was the most complex, adding many features regarding changes in a customer’s previous offers (e.g., ratios of current and previous costs, time differences, etc.).


**Takeaway #2: Evaluation metrics**


Another critical lesson learned was the importance of understanding the evaluation metrics, and how those metrics can greatly affect the right approach.


In real world problems, you must determine what question will drive the most business value and how to evaluate whether an answer is good. Evaluation metrics must be chosen carefully based on underlying cost and benefit factors. Different metrics can require completely different approaches, and choosing the wrong one can result in a suboptimal solution.


The greatest influence on our approach was the chosen evaluation metric provided – all options of a policy had to be predicted correctly. Kaggle provided a simple heuristic called last quoted benchmark (LQB). This represented the last insurance product option viewed by a customer. The performance of this heuristic was very good and we wanted to be really sure when making any changes to it. So, sometimes even when all three models suggested a change from the LQB, we still kept it. In the end, less than 4% of records were changed from the LQB. This is not much but, unlike our competitors, we focused more on the quality than on the quantity.


**Continued participation**


We regularly participate in Kaggle competitions because they are great for professional development. You get to hone your skills and learn from the very best in the discipline.


CGI chooses competitions that are most related to our business and that could benefit our clients. For example, we have predicted click-through rates, judged whether a loan would default, and looked for customers that could become frequent buyers. Kaggle is a great community for trying cutting-edge technologies.


I’ve personally participated in about 20 Kaggle competitions – it’s addictive! Learn how my colleagues also felt about the Allstate challenge in[this interview](http://blog.kaggle.com/2014/07/11/first-place-in-purchase-prediction-challenge/) .


Members of the winning team (left to right): Lukáš Drápal, Jana Papoušková, Jiří Materna, Nomindalai Naranbaatar, Emil Škultéty
