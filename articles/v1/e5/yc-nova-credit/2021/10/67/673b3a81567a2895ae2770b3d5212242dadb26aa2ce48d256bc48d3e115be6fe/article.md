---
schema_version: "1.0.0"
document_id: "673b3a81567a2895ae2770b3d5212242dadb26aa2ce48d256bc48d3e115be6fe"
company_key: "yc-nova-credit"
company: "Nova Credit"
source_id: "yc-nova-credit-rss-8f3179f5a680"
canonical_url: "https://medium.com/@nova-credit/engineering-an-income-model-at-nova-credit-7498c64d1dc4"
published_at: "2021-10-13T19:17:48+00:00"
first_seen_at: "2026-07-25T16:35:22.821563+00:00"
fetched_at: "2026-07-28T22:26:35.155530+00:00"
content_hash: "sha256:424a4a973dc3caea9f94601c04dc5f1dd8a54f79b51885e33e342a9cb1dff682"
---

# Engineering an Income Model at Nova Credit

# Engineering an Income Model at Nova Credit


[Nova Credit Engineering Blog](https://medium.com/@nova-credit?source=post_page---byline--7498c64d1dc4---------------------------------------)


5 min read


·


Oct 13, 2021


--


*How Nova Credit’s engineering and data science teams delivered a robust, high-performance income model for customers.*


by[James Xue](https://www.linkedin.com/in/jamesxue100/)


Press enter or click to view image in full size


Photo via[Pixabay](https://pixabay.com/photos/money-finance-mortgage-loan-2696229/) .


In the summer of 2020, we at Nova Credit found ourselves facing a new challenge of verifying consumer income. While we had previously focused on credit history as a measurement of a consumer’s *willingness* to take on debt, ultimately we were missing a significant part of the equation: a consumer’s *capacity* to repay that debt. Furthermore, an unprecedented macro environment driven by the coronavirus pandemic in 2020 led to many lenders questioning existing solutions for verifying income. So we asked ourselves an important question: **can we create a true picture of income using bank transaction data?**


This question led us to develop our first-ever income model, which over the past year has efficiently identified and verified our applicants’ overall income based on their consumer-permissioned bank transaction data. In this article, I’ll dive into some valuable engineering lessons we’ve learned while developing this solution for our customers.


## **📈 Income Model Details**


In short, the income model identifies which deposit streams in an applicant’s bank transaction data represent “income”, using a combination of statistical classification (logistic regression) as well as fixed rules.


The first step in the model is generating **model features** for each of the income streams from an applicant’s bank transaction data. These features include numerical values, such as percentage of total deposits, or boolean values, such as whether or not the income stream is a tax refund.


After normalizing these feature values, the model then computes **confidence scores** which helps identify three classes of income streams:


- **conventional** — derives from full-time employment (i.e. regular paycheck)
- **non-conventional** — income from any other form of employment (i.e. contract work)
- **non-wage** — income not tied to employment (i.e. retirement, interest)


In engineering the confidence score calculation, we made several decisions to optimize performance, reuse existing code, and ensure correctness. For example, we use a lookup map to get the corresponding intercept value for each model feature. This allows us to store each model feature’s coefficient value in one location, which improves readability and enables O(1) lookup time. Furthermore, several of the calculations needed came from[mathjs](https://www.npmjs.com/package/mathjs) , an extensive math library for NodeJS. Finally, each input and output in the code is typed, which we will elaborate on in a later section.


After identifying these income streams, the model then reconciles the three confidence scores generated from the three income stream classes. Next, it calculates **net** income, then applies a series of adjustments to determine **gross** income, accounting for pre-tax deductions (such as healthcare and retirement) as well as anticipated state and federal taxes.


Finally, the model returns net and gross income on an **annual** and **recent** (current quarter) basis for our customers to ingest via Nova Credit’s API.


### ⭐️ Performance


Overall, the income model has achieved high precision rates since its launch. It currently classifies conventional income correctly for more than 85% of income streams; for non-conventional income, more than 95%. Furthermore, in over 60% of cases, the income amounts generated via the model match the stated income supplied by applicants.


What this means is that our customers can rely on the model for automated decisioning, saving time and valuable resources on their end.


## ⌨️ Typescript


One of the biggest engineering challenges of this project was making sure we developed well-defined types for all of the different kinds of data being passed around, from raw inflow streams and transactions to formatted net and gross incomes to be stored in our database, and later delivered in our API.


For example, in the reconciling confidence scores step described above, we have a function that accepts an array of inflow streams, each with confidence scores:


Essentially, what the function header is saying is:


- Each object in the input object list can be of any generic type, as long as it includes` ConfidenceScores` , which is an object containing the confidence scores.
- Each object in the output object list must contain the same properties as each object in the input object list, as well as an additional` confidence_score` property.


While this is a relatively simple example, ultimately Typescript allows us to leverage strong[static typing](https://www.section.io/engineering-education/typescript-static-typing/#:~:text=TypeScript%20was%20developed%20by%20Microsoft,be%20run%20in%20any%20browser.) in a large project like this. This not only helps eliminate type errors in runtime, but also assists in readability and maintainability of our code across our engineering organization.


## 👩‍🔬 Testing


One way that we were able to ensure high levels of accuracy and uptime while delivering this project was through diligent testing. We addressed several levels of testing, including unit tests, integration tests, and test coverage tools at the code level, and QA (Quality Assurance) tests beyond.


For each of the functions in our income model codebase (which is a self-contained package in Nova’s monorepo), we wrote unit tests to cover the breadth of edge cases. For example, using the reconciling confidence scores example above:


In this test file, we cover both a general case where the input includes multiple` ConfidenceScores` objects, and a series of edge cases: for example, where the inputs could be empty, zero, or have undefined non-wage score values.


To help ensure that we hit all the edge cases, we use a Javascript test coverage tool called[Istanbul](https://istanbul.js.org/) . By executing a simple command in the terminal, we can view how well our unit tests exercise our codebase:


Press enter or click to view image in full size


An example of an[istanbul](https://github.com/istanbuljs/nyc) run in the income model package


Furthermore, at the integration test level, we have tests that simulate the data we get back from our bank transaction data suppliers, then run through our income model to ensure that all side effects, such as Amazon S3 uploads, database storage, and status webhooks work properly. We also have tests that ensure that the final API output, with net and gross income values, appears as expected, so that our customers can rest assured.


Finally, for an additional layer of confidence (no pun intended), the income model engineering team works with data science closely on QA, or Quality Assurance. This process consists of running any new income model improvements on the thousands of past raw data samples that we’ve received from our bank transaction data suppliers. This allows us to more reliably evaluate the **accuracy** and **performance** of any new changes that we release.


*Nova Credit is hiring! Check out our*[careers page](https://www.novacredit.com/careers) ,[current job openings](https://jobs.lever.co/neednova) *, and*[engineering values](https://www.keyvalues.com/nova-credit) *.*
