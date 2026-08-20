---
schema_version: "1.0.0"
document_id: "cb3be0e13a1f0952b43af3ad202980708c0b654048e8e74897534064dc1d88ed"
company_key: "taboola-com-ltd-ordinary-shares"
company: "Taboola.com Ltd."
source_id: "taboola-com-ltd-ordinary-shares-news-import-1349b39c90c0"
canonical_url: "https://www.taboola.com/engineering/stop-reading-start-talking-a-new-way-to-share-knowledge/"
published_at: "2025-01-14T14:37:11+00:00"
first_seen_at: "2026-07-22T15:33:14.584366+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:db1942d305f322cdf1d487a28496ad0694bad320cf3602ad1dfeef90b7ebe9d7"
---

# Stop reading, start talking. A new way to share knowledge.

Knowledge sharing is critical for every company that wants to grow and improve. The bigger the company – the harder it gets. Inefficiency, a lack of alignment within your peers, difficulty training new workers – you name it. In this post we will take a look at the existing methods for knowledge sharing. How they can’t keep up with growth and fast paced changes, and why people are your best resource for knowledge.


### What is Knowledge?


In general, there are three main types of knowledge that need to be shared in a software company – Technical Knowledge, Product Knowledge and Business Knowledge. When a new employee begins their job, most companies will help them to learn, using some of the more traditional methods to share knowledge:


1. Learn from others – via **frontal training** or assigning a **mentor**
2. Allow Self learning – online course, or from the company’s **knowledge center** ([Atlassian](https://www.atlassian.com/) ,[Convo](https://www.convo.com/) ,[Jive](https://www.jivesoftware.com/) ) that holds company’s videos, or an internal Wiki, etc.


### Which types of knowledge are hard to deal with?


Technical knowledge is being shared all the time on the web. Online courses, YouTube videos and professional forums teach and share the existing knowledge between professionals and novices alike. The other two types of knowledge (Product Knowledge and Business Knowledge) represent the **company’s knowledge** and they are **almost** **impossible** to share and manage, as the knowledge and the people who have it – changes constantly. The more developers and services you have – the more effort you will invest in sharing your knowledge and keeping it up to date. When there’s a gap between the existing knowledge being shared and the knowledge that isn’t, **your best resource is** **your own people** . They are the ones making the changes, they are the ones who can share knowledge with you when you need it. They hold the company’s knowledge, which is what is needed.


### Sometimes all you need is a cup of coffee


At *Taboola* , we deliver fast. We write thousands of lines of code every day and use continuous deployment. That means there’s a lot of knowledge to share, a lot of features to develop and a lot of changes are being made. That’s when knowledge sharing becomes a real issue and **people become your best source** . They are up to date.


But if you’re a new employee, you soon find out that people are less available than a *Wiki* page and it takes you time to learn the names and responsibilities of your peers. Sometimes all you need is a cup of coffee with that specific person who can help you the most. 10 minutes talk and you can get the job done.


### Instead of knowledge, search for people


*“It is nothing for one to know something unless another knows you know it.”* ― Persian Proverb


In our latest *Taboola* Hackathon, my team and I tried to tackle that gap. How can you tell who is that specific person who has the most relevant and up to date knowledge? We thought it would be useful to have a search engine for the people who are the most relevant for what we need.


That search engine has to be dynamic, automatic and not reliant on manual management.


To do that, we would need to find an information source that gets updated anyway and not as part of an effort to maintain knowledge management. This description suits many systems we all use to manage our work as developers. For example *Git* , *Slack* , or any other task management systems. But how can we use them to find people?


Let’s take *Jira* for example. In *Jira* , every task is represented by a ticket. Each ticket holds information about the task: who should develop the task, what the task is, who does the Code Review, etc.


Now let’s say we want to find someone in our company that knows something about a product called ‘Items Manager’. A naive approach would be to find the person in our company who has solved the most tickets containing the phrase ‘Items Manager’. It could be within the tickets’ title, description, comments, and so on.


### Our solution – creating *Wiser*


During our latest Hackathon we took that naive approach and made it more sophisticated. We thought having one source of information was not enough. Our results would be more accurate if we had multiple information sources. It would be even better if every source had different inputs, with different associated scoring. That way we would have an aggregated result that takes many parameters into consideration, making the result more relevant.


*Wiser* was created in the Hackathon, and it’s our attempt to tackle the difficulty of reaching the knowledge that is not being shared. We decided to start with *Jira* as our source, since it holds a lot of useful information. The result was a **list of people** who were **very relevant** for each field of knowledge we were searching for. We are planning to keep on working on *Wiser* and make it available for *Taboola’s* employees to use. Hopefully making it a service available for other people as well in the future.


###### An example for a search result with Wiser


### Not every solution suits everyone


Every company has different needs. Some companies has developers all over the world, others are geographically close. Every company is at different stages and needs different methods for knowledge sharing and management. Not every solution suits every company. Try to think about your company’s characteristics and find the solution that suits you best.


Knowledge Sharing is a complex issue every company has, and it gets harder as the company grows. When the traditional knowledge sharing methods can’t keep up with the changes, people become your best resource of knowledge.


So the next time you search for knowledge in your company, remember that sometimes all you need is a cup of coffee with that person who can help you.
