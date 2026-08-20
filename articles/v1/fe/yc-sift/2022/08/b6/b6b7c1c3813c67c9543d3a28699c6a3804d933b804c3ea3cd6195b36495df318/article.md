---
schema_version: "1.0.0"
document_id: "b6b7c1c3813c67c9543d3a28699c6a3804d933b804c3ea3cd6195b36495df318"
company_key: "yc-sift"
company: "Sift"
source_id: "yc-sift-rss-afa0f6dcd9d7"
canonical_url: "https://engineering.sift.com/response-recommendations-for-dispute-management/"
published_at: "2022-08-31T21:49:39+00:00"
first_seen_at: "2026-08-12T04:14:07.509173+00:00"
fetched_at: "2026-08-12T04:14:09.389902+00:00"
content_hash: "sha256:baabc1f9ec8f53947f6a98315a0d284be9c9e9a632c77b2cc02afc89d3c1ad54"
---

# Response Recommendations for Dispute Management

Post category:


[Articles](https://engineering.sift.com/articles/)


# [Response Recommendations for Dispute Management](https://engineering.sift.com/response-recommendations-for-dispute-management/)


Alex Forbess


• August 31, 2022


## Introduction


You may currently be working on a chargeback response, a representment, or a “pick-a-noun” content that you hope will give your merchant their money back. The cardholder claims the transaction was fraudulent, but this might not be true. So you search for the cardholder’s transaction history, their delivery receipt, and even their online activity in an effort to show the cardholder’s issuing bank that the disputed transaction was legitimate. Several days later, you hear from the bank, and the paraphrased answer you get is “you lose.” And just like that, the cardholder keeps your money.


This recurring experience for merchants is part of the fraud cycle, and is why Sift acquired then-Chargeback.com. Revenue cannot only be lost from fraudulent transactions, but also from an underrated headache: disputes. According to our


[Q4 2021 Digital Trust & Safety Index](https://resources.sift.com/ebook/q4-2021-digital-trust-safety-index-digital-fraud-and-disputes/) , 62% of surveyed consumers disputed a purchase from a digital transaction. What makes this headache into a migraine is that over 4 out of 5 consumers shared they were likely to file a dispute again. So chargeback and pre-arbitration cases won’t be going away any time soon.


With supervised learning, the


[Dispute Management (DM)](https://sift.com/products/dispute-management) product offers Response Recommendations, the first-ever product that optimizes your chances of winning dispute cases by offering recommendations of what data should be inserted into your response. So whether that response can automatically be created or modified by a dispute analyst, you will be informed how likely the disputed amount will return to your account.


## Empower analysts; retain your revenue


Dispute management is often dealt with partial solutions. Some banks may ask for evidence that fulfills the minimum requirement to prove the transaction was legitimate. Most of these requirements, which are stated by


[one of many reason codes](https://resources.sift.com/ebook/chargeback-reason-code-encyclopedia/) , can be inserted in a response with automation. However, other banks may have their own preference for a dispute to be ruled in favor of the merchant. And knowing what Wells Fargo, Chase, and other banks like (and don’t like) in a response requires domain knowledge from the most valuable asset in the DM space: dispute analysts. And that is when we asked ourselves:


***“What feature can we offer that not only saves time for analysts to create and send responses, but also empowers them by being reminded of evidence(s) that have the best chance to retain revenue against a given dispute case?”***


This was the beginning of Response Recommendations. We hypothesized if we simply analyzed years of


**\[dispute case-dispute response\]** interactions, we then would be able to see what combination of evidence retained revenue more often than not. Additionally, we hypothesized that if this same combination of evidence were submitted against that same chargeback or pre-arbitration case again in the future, we can confidently predict that this combination has a ŷ% chance to retain revenue. Fortunately, both of our hypotheses were confirmed to be true.


*Test set volumes from three of our integrated payment service providers (aka, payment processors)*


This distributes the number of times our predictions have matched our observations. A Win Rate Scale of 10 means the merchant will very likely win against a dispute case, as long as their response contains the recommended evidence.


## A sense of deja vu


During the exploratory phase of Response Recommendations, we were constantly reminded of these two key themes:


1. The same type of chargeback/pre-arbitration case that was submitted before will be submitted again; and


2. In most cases, the outcome (won or lost) that the bank gives for Response A will always be the same outcome if Response A is submitted again.


No matter how many times a bank submits a Fraud or even a Canceled Subscription chargeback case, their preferences to return (or withdraw) the merchants’ funds are locked. So whether the bank follows Visa or MasterCard’s guidelines, or if they choose to want extra evidence, the goal to retain revenue is to figure out what they want. Is Bank of America satisfied if merchants just insert line items into the response? Or do they want something more qualitative, such as a transcript of an online chat the merchant had with their cardholder?


Unfortunately, some merchants may also be locked in the number of different types of responses they can create. And if they keep using the same template (or using the same, overpriced advice from third-parties), they may never receive a “win” against their most frequent dispute cases.


Response Recommendations offer both customer- and global-specific recommendations that are the most optimal to retain revenue against the dispute case you’re working on right now. The benefit of our customer models is that it can recommend what type of response works specifically for merchant A. And our global models, trained on data from other merchants, can recommend responses that merchant A has never created before, given the recommended response has a history of winning more often than not.


For example, let’s say merchant A often receives losses against a dispute case. Within our global models, there is a possibility that merchant B—who is in merchant A’s industry—has received a similar dispute case, and has won against it. Whenever that happens, merchant A will then be recommended to create a response, based on the model trained on global data, similar to how merchant B created it. This helps merchant A exit from a vicious cycle of revenue loss and enter a virtuous cycle of revenue retainment.


## Dispute Management, powered by machine learning


We know our patent-pending feature helps our customers and analysts focus on creating responses that are the most optimal to retain revenue. There will be a minority of dispute cases that will always, or on occasion, cause revenue loss. Moreover, some banks give cardholders the right to reopen a dispute case that entered the chargeback stage. This will withdraw your funds (again), and you will need to respond to the dispute in another stage of the dispute life cycle called


[pre-arbitration](https://sift.com/sift-edu/chargebacks/chargeback-arbitration) . You may now have a few follow-up questions, such as:


1. Can Sift know the likelihood of winning for responses that are generated by 100% automation?


2. Can Sift inform me of which dispute cases I will unequivocally lose against, no matter what response was recommended at the customer-level and the global-level?


3. Can Sift tell me how likely a dispute will move from chargeback to pre-arbitration?


4. Can Sift dig deeper and show me what variables are contributing to certain losses and chargeback-to-pre-arb migrations?


The answer to these questions is:


1. Yes


2. Yes


3. Yes


4. Yes


As we were building Response Recommendations, there were multiple follow-up questions we had that compelled us to create products out of it. Because at the end of the day, we understand machine learning is an arms race that is best won not just by being first, but applying machine learning correctly to ensure Digital Trust & Safety for all. Dispute Management requires a different approach than how we apply machine learning to


[Payment Protection](https://sift.com/products/payment-protection) ,


[Account Defense](https://sift.com/products/account-defense) , and even


[Passwordless Authentication](https://sift.com/products/keyless) . But that has not stopped us from wanting to offer a holistic Digital Trust & Safety solution for the


*entire* transaction cycle: from authorization and settlement, to dispute representment.


[Schedule a demo](https://sift.com/products/dispute-management) to learn more about how Dispute Management is retaining revenue now, and later.


## Author


-


[Alex Forbess](https://engineering.sift.com/author/aforbess/)
