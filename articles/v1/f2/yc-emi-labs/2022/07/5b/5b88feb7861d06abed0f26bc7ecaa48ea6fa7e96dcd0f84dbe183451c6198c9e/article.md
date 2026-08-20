---
schema_version: "1.0.0"
document_id: "5b88feb7861d06abed0f26bc7ecaa48ea6fa7e96dcd0f84dbe183451c6198c9e"
company_key: "yc-emi-labs"
company: "Emi Labs"
source_id: "yc-emi-labs-rss-87232385bc09"
canonical_url: "https://medium.com/@EmiLabsTech/data-privacy-the-netflix-prize-competition-84330d01cc34"
published_at: "2022-07-02T15:50:18+00:00"
first_seen_at: "2026-07-27T09:02:34.241036+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:2c4f46bad45b7fe7fa44fb9870011f61ddaf16b9b1f0b9bf94e1513ae86c13fb"
---

# Data Privacy — The Netflix Prize competition

# Data Privacy — The Netflix Prize competition


[Emi Labs Tech - Ravens](https://medium.com/@EmiLabsTech?source=post_page---byline--84330d01cc34---------------------------------------)


5 min read


·


Jul 2, 2022


--


by[Nico Oteiza](https://www.linkedin.com/in/nicolasoteiza/?lipi=urn%3Ali%3Apage%3Ad_flagship3_feed%3BlxpX7oknSn%2BHJX5MVX1PqA%3D%3D)


In this post, we’d like to share a story that shows how a seemingly harmless dataset, without any obvious personally identifiable information (PII), ended up becoming a big privacy concern nonetheless. We hope this will show how encumbered the issue with data privacy is, and why it is such an important area of research today.


Most of the content of this post is strongly based on the book[“The ethical algorithm”](https://books.google.com.ar/books/about/The_Ethical_Algorithm.html?id=sE2jwgEACAAJ) by Micheal Kearns and Aaron Roth.


Press enter or click to view image in full size


The ethical algorithm book cover


## What is anonymous data?


This is a great question, because in the surface it seems there is a fairly good common-sense answer: “Data without PII”. Nevertheless, different real life experiences show it’s not that simple.


The question of data privacy is especially relevant in medical research, because it is usually highly sensitive data of individual patients, which should not freely shared, that is needed to perform investigations. Being able to share this information between institutions may be the only way of getting the benefits of large scale data science, and find important correlations that can help treat patients and even save lives.


In the 1990s, in the state of Massachusetts (USA), a government agency decided to release data summarizing hospital visits for every state employee. To protect the individual patients identity, they removed the obvious PII like their name, address and social security numbers. They nevertheless kept a few data points they deemed would be useful to summarize the data: zip-code, birthdate and sex.


A PhD student at MIT at the time,[Latanya Sweeney](https://en.wikipedia.org/wiki/Latanya_Sweeney) , wanted to prove that this anonymization process was not enough, so she purchased the voter rolls for the city of Cambridge, which included zip code, birthdate and sex of every voter of the city. Just by joining the two datasets with these three features, she managed to identify exactly the governor’s medical records. There was only one record in the medical dataset that had exactly the same values for the three features. Of course, sex, birthdate and zip code can’t be used separately to identify someone, but as a combined set of features, Sweeney estimated that actually 87 percent of the US population could actually be uniquely identified.


One could answer to this case by just saying “Ok, so lets just coarsen these features even more” or even “anonymize zip code, birthdate and sex altogether”. Of course, both would help, but as we will see, even much less obvious information can uniquely identify you with high confidence.


## The Netflix competition


In 2006, Netflix launched a very famous competition, where they shared data of approximately 100 million individual movie ratings and the date of the individual ratings for roughly 500,000 users. All other data of the user was anonymized and reduced to a unique numeric ID, used only to know which ratings belonged to the same user. The competition was aimed at letting the public experiment with new techniques and find a recommendation algorithm that beat what Netflix had at the time by a 10% accuracy gain. The prize was set at U$D 1,000,000.


Compared to the example of the medical records, Netflix had been very careful not to add any data that could identify a user, like zip-code, birthdate, and of course name, personal IDs, etc. Nevertheless, only a couple of weeks after the release, another PhD student,[Arvind Narayanan](https://en.wikipedia.org/wiki/Arvind_Narayanan) , announced that they (together with his advisor Vitaly Shmatikov), had been able to connect many of the unique IDs in the Netflix dataset to real people, by cross referencing another publicly available dataset: the movie ratings in the IMDB site, where many users post publicly with their own names.


It’s tempting to say that if these people are publicly posting these reviews in IMDB, then it would hardly be a violation of privacy, but, and here again we see how complicated this issue is: a person could have publicly commented on IMDB a few of the movies they watched, and maybe rated privately other movie they had watched, and that could expose more sensitive information like political inclination or even sexual orientation. This is why movie rental records in the US are protected by the Video Privacy Protection Act (passed in 1988), that states that video rental providers are liable for up to U$D2,500 in damages per customer whose records are released.


This ended up with a[big lawsuit](https://www.wired.com/2009/12/netflix-privacy-lawsuit/) and with the[competition canceled](https://www.wired.com/2010/03/netflix-cancels-contest/) , and the dataset being unavailable for further analysis. Of course, another problem here is that once a dataset is available, it can’t be made “unavailable”, because the world already has it. Netflix can stop sharing it, but whoever downloaded it, already have the data themselves.


The Netflix competition story shows how hard it is to really anonymize data, because whoever is sharing the data should be able to know in advance what other sources of information may be available today (or in the future!) that in conjunction would de-anonymize the data. This seems like an impossible task, and[Cynthia Dwork](https://en.wikipedia.org/wiki/Cynthia_Dwork) (one of the inventors of a technique called Differential Privacy) says “anonymized data isn’t”, meaning data is either not anonymized or so much has been removed from it, that it isn’t data anymore.


## Some thoughts


We hope this story helps show how a seemingly simple question like “how to anonymize a dataset” is actually a very hard problem to solve. We just scratched the surface of Data Privacy in this article, and hopefully we will be sharing other articles in the future with some existing approaches that try to solve this problem, by adding very clever algorithms either to data collection or by redacting information from individual records.


An important rule-of-thumb is to understand that none of the existing techniques to protect data privacy will come “for free”. A way of illustrating this that I find easy to understand, is for example adding ethical goals directly into our algorithms as constraints to the optimization goals, which will probably come with a cost for the business metric, for example accuracy.


Of course, “traditional” solutions, such as laws and regulations around data and algorithms are also a very important part of making this world a better place in terms of our data privacy, but we should not forget about how we, as data practitioners, can help by designing social constraints directly into our algorithms.
