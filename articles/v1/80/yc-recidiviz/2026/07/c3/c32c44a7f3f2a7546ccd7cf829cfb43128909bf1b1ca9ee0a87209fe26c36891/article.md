---
schema_version: "1.0.0"
document_id: "c32c44a7f3f2a7546ccd7cf829cfb43128909bf1b1ca9ee0a87209fe26c36891"
company_key: "yc-recidiviz"
company: "Recidiviz"
source_id: "yc-recidiviz-news-import-9fcc86c2623e"
canonical_url: "https://www.recidiviz.org/updates/part-iv-breaking-down-data-analytics-and-ai-in-criminal-justice"
published_at: null
first_seen_at: "2026-07-22T11:03:47.789737+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:00878fa3b92f804b7669f225564e269a3dbf4d81173e112ccd8c51d2575a9b6b"
---

# Part IV: Breaking Down Data Analytics and AI in Criminal Justice

In recent years, artificial intelligence (AI) and data analytics tools (DAT) have exploded in popularity. And everyone wants in: vendors are taking advantage of this hype by “AI washing” and applying the AI label (sometimes indiscriminately) to their offerings; organizations are eager to see how DATs can help with their work.


The criminal justice tech space is no exception. As we explored in a previous[post](https://www.recidiviz.org/updates/part-i-a-mapping-of-tech-in-the-criminal-justice-system) , technology is increasingly being used across all parts of the justice system. Many new tools are AI-enabled, including algorithmic risk assessments, facial recognition, and gunshot detection.


But these tools aren’t perfect. Even the most sophisticated companies in the consumer AI space have grappled with algorithms that disproportionately impact[women](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight/amazon-scraps-secret-ai-recruiting-tool-that-showed-bias-against-women-idUSKCN1MK08G) or[people of color](https://blog.twitter.com/engineering/en_us/topics/insights/2021/sharing-learnings-about-our-image-cropping-algorithm) . Most companies[now have “Responsible AI” teams](https://ai.facebook.com/blog/facebooks-five-pillars-of-responsible-ai/) dedicated to studying and mitigating these biases. In the criminal justice space, where the stakes can be much higher, DATs may cause significant harm by perpetuating historical bias and violating privacy. One thing is clear – not all DATs are good, and criminal justice practitioners need to very carefully evaluate the types of technologies they are looking to adopt.


This posts aims to help practitioners make better procurement decisions by:


- Explaining the differences between common types of DATs, including those that use AI
- Applying these definitions to the criminal justice context
- Exploring the different types of data involved in different technologies and corresponding pitfalls
- Providing practical and actionable advice to practitioners that can be implemented during the technology procurement and deployment process


Why are we writing about this? First, our team has a lot of experience in data analysis, statistics, machine learning, and AI. We want to share what we’ve learned. Second, we’d like to show where our tools fit into this landscape, and the precautions we’ve found effective so far in working at the intersection of tech and criminal justice.


# Predictive analytics, deep learning, machine learning, reinforcement learning, and more…


Before we dive into criminal justice specifics, let’s parse some of the common technical terms we encounter in this space. A few caveats to keep in mind throughout the rest of this section: First, these definitions are constantly evolving as technology changes; what constituted “machine learning” 30 years ago is very different from what it is today. This guide captures the present-day understanding of these terms. Second, data analytics is an expansive field – these definitions aren’t meant to be comprehensive, but we hope they will at least help to untangle the complex web that is data-related terminology, methodology, and use cases.


To better illustrate these concepts, imagine you are the owner of an orchard that produces Red Delicious apples, and your goal this year (and every year) is to harvest as many ripe apples as possible.


#### Artificial Intelligence (AI) - Robot, please go out and pick the apples!


AI is a broad term used to describe a computer system able to perform tasks that ordinarily require human intelligence. Machine learning (ML) is often, but not always, used to power these AI systems. A robot that picks apples on its own is an *AI* – picking fruit efficiently surely requires a certain level of intelligence. The robot uses ML, which we’ll explain next, to do specific tasks like recognize apples on a tree or grasp each individual fruit.


#### Machine learning (ML) - Is this an apple?


ML is a type of *method* that trains computer systems to identify patterns from data and make decisions without human help. We can use ML to train our apple-picking robot to recognize apples from other fruits, tree branches, and owls. The data we use to teach the robot might look something like this:


With enough of these examples, our robot will eventually learn the distinguishing features of apples (like roundness and redness) and be able to classify new images with relatively high accuracy.


*Deep learning* and *reinforcement learning* are specific types of ML techniques.


#### Predictive Analytics - How many apples can we expect to harvest next month?


Predictive analytics is a specific category of data analysis that uses historical data and trends to make predictions about the future. This analysis can involve ML methodologies. For example, we can train our system with harvest records from previous years, which include information like weather conditions, pest patterns, and total apple output, to help it learn what conditions lead to what quantities of apple output.


#### Descriptive Analytics - How many apples did our orchard produce this year? How does this compare to last year?


Descriptive analytics is the “examination of data or content to answer the question ‘What happened?’”, for an event that’s already occurred. It may also be used to answer deeper questions like “Why did our orchard only produce half the number of apples it usually does?” Did a new type of pest emerge? Did the irrigation system break down? Or were our apples stolen during the night? Although data is critical to this analysis, ML is not usually used.


# What do these technologies look like in criminal justice?


Data tools in criminal justice roughly fall into three different buckets:


- **Automated Identification Tools,** which use a combination of AI and ML methods
- **Predictive Forecasting Tools** , which use predictive analytics
- **Tools that Help us Understand the System,** which use descriptive analytics


Each type of tool is useful in its own way, but also carries risks specific to its inner technical workings. In this section, we’ll explore both risks and potential mitigation strategies associated with each of the three different tool types.


## 1. Automated Identification Tools


Automated identification tools use ML to help with complex classification problems. These tools are used to automate tasks of varying difficulty, from call transcription to fingerprint matching, and help humans extract insights from large amounts of data.


#### How can I spot these tools?


If you’re using a piece of technology to answer these kinds of questions, then it’s an automated identification tool:


- Does this fingerprint match a print in our database?
- What words were just said in this courtroom?
- Was that sound outside a gunshot or fireworks?


Traditionally, these tasks are what a team of analysts are paid to do – now, computers can look at millions of images or records in just a few minutes. This is possible because the availability of data, as well as the ability to process that data, has significantly increased.


#### What are some common examples in the CJ space?


The most common types of data processed by these tools are **images** (e.g. face detection in bodycam redaction, facial recognition in prison surveillance, license plate recognition), **voice** (e.g. prison call monitoring, court transcription, audio analytics for emergency calls), **sounds** (e.g. gunshot detection), and **text** (e.g. entity matching between different types of records).


#### What are the risks associated with these tools?


Recall from our orchard example that in order to train a robot to recognize apples, we had to show it example images of apples and non-apples. Similarly, to train a face detection tool using ML, the system is shown many images of faces and non-faces, so that the tool is able to learn the distinguishing features of a human face when it encounters one in bodycam footage; for gunshot detection, the distinguishing features between a real gunshot and fireworks, and so on.


Now, imagine that you’re training your robot to recognize different types of apples. What happens if you only have one or two images of green apples on hand? Not surprisingly, your robot probably won’t do too well when asked to classify a green apple. Unfortunately, this problem is pervasive in data that is used to train criminal justice tools. Popular open source image datasets that companies use to train facial recognition models[have been shown to overrepresent](https://venturebeat.com/2021/03/16/imagenet-creators-find-blurring-faces-for-privacy-has-a-minimal-impact-on-accuracy/) males between 18-40 and underrepresent people with dark skin. Various other commercial classifiers[tend to perform worse](http://gendershades.org/overview.html) on women and those with darker skin tones. And sometimes, even without obvious data issues, the underlying problem ML is trying to solve is just really hard – a new analysis in Chicago showed that police officers who respond to ShotSpotter, a gunshot detection tool, subsequently come back without finding a problem[86% of the time](https://chicago.suntimes.com/2021/5/4/22417660/shotspotter-analysis-macarthur-justice-center-chicago-police-chicago-gun-violence-editorial) .


#### How do we mitigate these risks?


*Ask a lot of questions about the data.* Are there known issues with the types of data that were used to train the system (e.g. image datasets that don’t adequately represent faces with dark skin)? What, if any, steps have been taken to ensure that the data is high quality, fresh, and representative across protected characteristics? Data that is not appropriately representative can lead to systems that don’t work as intended or actively perpetuate harm.


*Ask whether the tool actually works.* What is its proven accuracy? What is its false positive rate, and false negative? Just because a piece of technology sounds impressive doesn’t mean that it actually works. Understanding the limitations of a tool are crucial in mitigating the risks of its use.


## 2: Predictive Forecasting Tools


Predictive forecasting tools apply ML and other statistical methods to data from the past in order to make predictions about what will happen in the future. These tools are used to guide decision making in various stages of the criminal justice system.


#### How can I spot these tools?


If you’re using a piece of technology to answer these kinds of questions, then it’s a predictive forecasting tool:


- What is the expected population of the county jail next month?
- What is the likelihood that this person will show up to their trial if they are released from jail?
- How would the decriminalization of marijuana impact the size of the prison population over the next 10 years?


#### What are some common examples in the CJ space?


Predictive forecasting tools are used to make predictions about an individual, a community, or an entire system. **Individual-level predictive tools** include risk assessment instruments like[COMPAS](https://en.wikipedia.org/wiki/COMPAS_(software)) ,[ORAS](https://doc.mo.gov/justice-reinvestment-initiative/oras) , and the[PSA](https://advancingpretrial.org/psa/factors/) . These instruments rely on trends from historical criminal records to predict the future behavior of an individual, e.g. whether they are likely to commit another crime or not show up to court. **Forecasting at the** **community-level** includes predictive policing tools like[Geolitica](https://www.wired.com/story/plainfield-geolitica-crime-predictions/) , which analyzes historical arrest data to determine where criminal activity is most likely to occur in a city throughout the day. **System-level predictive analytics** include models of how a system has changed over time in order to predict how it will change in the future. For example, prison facility population models rely on historical prison admission and release trends to forecast the growth of the prison population in the coming years.


#### What are the risks associated with these tools?


These tools are usually trained on what we call “ **tabular data** ,” a fancy way of saying information that is stored in a table. Each row represents a data point (e.g,. a formerly incarcerated person) and the columns represent different facts about that individual (e.g. age, race, sex, nature of crime committed, whether they recidivated, etc). In our orchard example, this would be equivalent to the harvest records that not only document the number of apples harvested in a given year, but also various other features like soil acidity and rainfall. The system will then use that data to learn which features contribute to a successful harvest.


But criminal justice records are far from perfect –[research has shown](https://www.nyulawreview.org/wp-content/uploads/2019/04/NYULawReview-94-Richardson-Schultz-Crawford.pdf) that these records are produced during “documented periods of flawed, racially biased, and sometimes unlawful practices and policies.” For instance, the ACLU of Illinois found that a significant number of stop and frisks conducted by the Chicago Police Department were unlawful, and Black residents were[disproportionately subjected to these unlawful stops](https://www.aclu-il.org/sites/default/files/wp-content/uploads/2015/03/ACLU_StopandFrisk_6.pdf) . Records from these encounters constitute data that can play a role in (wrongly) teaching a computer system that crime is more likely to happen in Black neighborhoods, or that a Black person is more likely to commit a crime, just because they are Black. For example,[researchers have found](https://gizmodo.com/crime-prediction-software-promised-to-be-free-of-biases-1848138977) that PredPol over-recommends policing Black and Latino neighborhoods and “mostly avoid\[s\] Whiter neighborhoods” due to the crime report data it is trained on.


The use of individual and community-level criminal prediction tools can have severe and disparate consequences. Using biased or flawed data to inform new decisions about where to send police forces, or which individuals should be barred from parole, can further incriminate vulnerable communities – which in turn creates more records that are used to inform decisions down the line. These self-reinforcing systems replicate and deepen the existing disparities that we see in the current criminal justice system.


There are also risks in forecasting the impact of changes on an entire population. A policy modeling tool that doesn’t look at projected outcomes across race or other important factors can lead to the promotion of policies that at first seem promising, but end up increasing adverse outcomes for marginalized groups. Any issues in data quality or freshness can lead to decisions made on inaccurate information and loss of ability to track the impact of new policies or practices.


#### How do we mitigate these risks?


*Interrogate the underlying data used to make predictions.* Is this a domain in which the data is likely to reflect historical biases? (The answer is likely yes). Do any of the inputs correlate highly to protected class characteristics? Is the data used in the system likely to be a good indicator of what the tool is trying to do? (For example, recorded arrests could be a flawed input for predictive use cases – an arrest may be deemed unconstitutional, or might not actually lead to a conviction).


*Ask for context-specific validations of the tool in your jurisdiction.* Predictive tools are often not built or tested in the jurisdiction for which an agency is looking to procure technology, despite vendors’ claims that their tools are “nationally validated.” What the tool learns from data in one place may not be useful when applied to another (risk factors are likely to be a bit different in rural New Mexico compared to New York City). Agencies should ask vendors to do validations for a specific location and use case, and revalidate whenever there are significant changes in local law, composition of the community, or other factors. Important questions to ask: What are key differences between our jurisdiction and where the tool was tested or previously deployed? How might those differences impact performance? How does the tool actually perform on data from our jurisdiction? Have we talked to other jurisdictions who use the tool, to understand common failure scenarios or edge cases they’ve found?


*Recognize that “fixing the data” is not always a solution* **.** Data is not always “fixable.” For example, even if crime data was 100% accurate and certified to be obtained from legal practices that resulted in conviction, that data still inherently reflects historical biases in policing. Similarly, excluding race information from the data will not automatically make a tool less biased; often, other information fields like zip code and income may serve as a proxy for race. There isn’t really a way to “remove” these biases, and in many cases it may make sense to stop using these technologies altogether (as[some agencies have already done](https://www.latimes.com/california/story/2020-04-21/lapd-ends-predictive-policing-program) ).


## 3. Tools that Describe the System


There are a handful of other data tools that fall into this third bucket: **** ones that describe the system. These are also known as **descriptive statistics tools** , and don’t rely on any ML techniques.


#### How can I spot these tools?


If you’re using a piece of technology to answer these kinds of questions, then it’s a descriptive statistic tool:


- How many people were admitted to prison last year because they had their parole revoked?
- How many people currently on probation in this county are overdue for being discharged?
- Which counties have had the highest failure to appear rates over the last 10 years?


#### What are some common examples in the CJ space?


Descriptive analytics tools in criminal justice include dashboards that describe the historical trends of a given system, e.g. how a state’s county jail population has changed over the last 10 years, or provide a snapshot of what’s happening in the system today, e.g. the number of people currently on probation in each county in the state. These tools don’t always present aggregated data; for instance, case management tools tell a parole officer who is on their caseload, what the contact requirements are for each person, and when each person is scheduled to be discharged.


#### What are the risks associated with these tools?


Descriptive statistics can only be as accurate as their underlying data. As with any of these tools, inaccurate data collection directly impacts the usability of the tool. If a correctional officer tends to misclassify new admissions from the court as revocations from probation, then the descriptive statistics that report these numbers will be inaccurate.


Even with accurate data, there is a risk that the data presented by these tools will be interpreted to harmful ends. Poorly designed data visualizations can be easy to misinterpret or taken out of context. There is a risk that someone will make a decision based on data they are seeing in these kinds of tools, even though their understanding of the data visualization is mistaken.


#### How do we mitigate these risks?


*Build disaggregated views into dashboards.* **** It is possible for important disparities in outcomes among racial and other minorities to be hidden from view in data visualizations showing aggregate trends. For instance, a chart may show that the rate of probation success has been rising steadily over the last 5 years even though the completion rate has actually been *declining* for Native Americans during that same time period. Tracking important outcomes for marginalized groups is important to understanding if a given problem is getting worse for communities that are the most vulnerable.


### An aside: How does Recidiviz fit into the picture?


Recidiviz focuses on building **system-level predictive forecasting** and tools that **describe the system** .


Our system-level forecasting tools include[policy impact modeling](https://www.recidiviz.org/policy) . We build probabilistic data models that simulate people moving through the criminal justice system to forecast the projected impact of passing a given policy. We project five-year impacts on cost, population changes, and life years saved by a person being out of the system. We have also built system-level forecasting capabilities in our[leadership tools](https://www.recidiviz.org/corrections) , which help agencies anticipate future changes in their system’s population sizes.


The descriptive statistics tools that we build include the public-facing dashboards showing the number of people[currently on parole in Pennsylvania](https://dashboard.cor.pa.gov/us-pa/narratives/parole/2) and that highlight how the[prison population in North Dakota](https://dashboard.docr.nd.gov/us-nd/narratives/prison/3) plummeted when COVID hit. The data visualizations in our internal leadership tools help state agencies better understand trends in their systems. We also build tools that surface data at the individual-level; our tool for parole and probation officers identifies which individuals on an officer’s caseload are overdue for discharge based on the person’s sentence and the state’s discharge policies, which helps ensure that no one stays on supervision past their sentence’s expiration date.


### Parting thoughts about AI and data analytics in criminal justice


In a previous post, we published a[Criminal Justice Tech Playbook](https://www.recidiviz.org/updates/part-iii-tips-for-procuring-and-adopting-criminal-justice-technology) that aims to help practitioners procure and deploy tech tools more effectively. Tools that use AI and data analytics bring their own set of benefits, caveats, and risks. In this section, we will expand upon the playbook with considerations specifically tailored to the use of these tools.
__


#### Know that AI, and more complex AI, is not always better


Terms like “deep neural networks'' are popping up to describe new products with high price tags. But just because a technology sounds fancy doesn’t mean that it is actually better than something simple. In fact, a[research study showed](https://advances-sciencemag-org.stanford.idm.oclc.org/content/4/1/eaao5580) that “despite \[the COMPAS automated risk assessment’s\] collection of 137 features, the same accuracy can be achieved with a simple linear classifier with only two features.” The latter tool is not only far easier to use, but is also far more explainable.


#### Never use high-risk technologies on their own


Tools that can lead to significant negative consequences for an individual (e.g. a facial matching tool used to make an arrest, or risk assessments that determine eligibility for parole) should never be fully trusted for decision-making. Instead, they should be treated as decision aids for people making the final decision, similar to high-risk tools in the medical field (e.g. cancer screeners). **‍**


#### Don’t underestimate the downsides of scale


Scale is what enables technology to be effective – by ingesting more data, extracting more trends, and making more predictions in a minute than thousands of humans can do in a week, data analysis tools can dramatically speed up operations and derive insights that otherwise would be missed entirely.


But scale comes at a cost, and two of the most significant costs are *privacy* and *transparency* . Just because automated license plate readers and facial recognition robots can be placed everywhere doesn’t mean that they *should* be, or *can* be constitutionally. Though machines are now able to process and understand data in ways that help them perform tasks better, these ways are sometimes so technical and complex that they can no longer be explained—sometimes even by the people who developed them. These are all costs that should be factored into a decision to use any data analytics tool.
__


Lastly, we’ll emphasize that these considerations stand *in addition to* those described in the Playbook – analyzing the system’s performance across different groups, tracking impact and feedback loops, and weighing cost/benefit to decide whether to use the tool at all are still (if not even more) important to do with data analytics tools.


As the AI landscape changes and criminal justice technologies involve, we hope to continuously update this piece. If you have thoughts or feedback, please let us know!


For further reading:


- [aimyths.org](http://aimyths.org/) — A resource that aims to debunk the most common and harmful misconceptions about AI
- AI Now Institute’s[Algorithmic Impact Assessments](https://ainowinstitute.org/publication/algorithmic-impact-assessments-report-2) — A framework that describes how public agencies can increase accountability[‍](https://cjtec.org/files/5f5f93ea20ded) ‍
- ‍[AI in the Criminal Justice System](https://cjtec.org/files/5f5f93ea20ded) — A set of briefs authored by the The National Institute of Justice that looks at AI technology and its use in different parts of the justice system
