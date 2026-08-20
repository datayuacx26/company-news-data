---
schema_version: "1.0.0"
document_id: "a24786ee1111a4ef420547974be40ec9fbb88e7438e702d9d4648a666c0ca0b0"
company_key: "yc-yassir"
company: "Yassir"
source_id: "yc-yassir-news-import-6914a7494ed8"
canonical_url: "https://yassir.com/en/blog/how-we-achieved-a-93-success-rate-in-digital-payments-and-what-it-means-for-the-rest-of-africa"
published_at: null
first_seen_at: "2026-07-26T06:10:41.130229+00:00"
fetched_at: "2026-07-28T21:16:48.751829+00:00"
content_hash: "sha256:4e86336fb3507b63e5f258d7e9b50d0494cca560a4e00ab63f685f2e3a14dfac"
---

# 21/05/2024 How we achieved a 93% success rate in digital payments, and what it means for the rest of Africa Digital commerce continues to grow in Africa. With the recent COVID-19 outbreak, restrictions were...

21/05/2024


How we achieved a 93% success rate in digital payments, and what it means for the rest of Africa


by Onothoja Ron - Product Owner/Lead, Payments processing.


Digital Payments


Innovation


Software Trends


Digital commerce continues to grow in Africa. With the recent COVID-19 outbreak, restrictions were placed globally everywhere — from airports to retail outlets — forcing the adoption of digital commerce across multiple regions. As a result, Africans tuned in online and have not slowed down since. According to a global study by Mastercard conducted in 15 markets, consumers are increasingly moving away from cash and opting for contact-free and digital payment experiences — and they don’t expect to return. E-commerce is also seeing a surge, with Mastercard SpendingPulse reporting record growth rates across the globe as consumers increasingly shop online.


Although initially focused on the Maghreb, Yassir started its expansion across other EMEA regions, and this came with an accompanying and exciting challenge — localizing digital payments per region, which, according to the World Economic Forum, 2018, is a determinant of the success or failure of e-commerce. This report documents the exciting challenges we faced, how we conquered the challenges, and how we are using the insights from this experience to shape novel innovations in different parts of Africa.


**The Challenge**


Strong dominance in the Maghreb region and tight government policies on financial services may have stifled the pace of digital innovation (e.g., card tokenization, server-to-server integrations, etc.), regardless of PCIDSS, SOC, or any other security/compliance standards. Issues such as this will naturally impact the creative boundaries of technical creatives; however, with Yassir, it was different, as the Payment team is armed with diversity. As with any startup, technical depth is often sacrificed for speed, but this was a good problem.


We had built a multi-currency payment orchestrator localized for each region, but we needed some time to observe product adoption, study usage patterns, and discover insights for optimizations and improvements. We noticed that in one of the markets — South Africa- Mastercard (MC) had more local dominance than Visa; ironically, MC debit cards had an unusual failure rate.


*Fig 1: Comparative analysis of card processing networks adoption rate*


**Problem Solving with Data**


This challenge was a good one, as it originated from a hybrid security and user experience update MC had just made to their infrastructure to protect merchants from chargeback fraud while provisioning a seamless user experience for merchants to optimize their systems for recurring payments/revenue. A new instruction type was instituted — eliminating the need for cardholders to tokenize every time they wanted to make a payment online.


Analyzing the transactions revealed a format code 800.100.156 for unsuccessful transactions, unique to MC cards. This happened as a result of failed authentication when a customer wanted to make a payment. However, on discussing with our providers, we discovered a mandate requiring merchants to set their standingInstruction.type to UNSCHEDULED and point initial transactions to Customer initiated transaction (CIT) type, enabling first-time card users to authenticate but returning card not to, using the Merchant initiated transaction (MIT) type to by-pass auth enforcement.


*Fig 2: Analysis of error codes*


The concept of 3D secure authentication is one of the paradoxes of the payment processing industry, because if authentication is not enforced, it makes the card acquirer vulnerable to chargeback fraud. However, it is enforced for every transaction, it can reduce the success rate of payments by up to 40%. Finding a balance will always be a no-brainer, and Payment processing leaders such as Bolt who have succeeded at creating a seamless UX balance at scale, boast of increasing conversion rate by up to 70%.


**Business Outcome**


Like a Domino effect, solving this problem solved two other associated problems over time, such as risk management timeout and user authentication failure with format errors 100.380.501 and 100.380.401, respectively.


After deploying all the solutions, we observed a ~94% success rate for every unique customer who tried to make a payment and 84% success rate for unique transactions.


*Fig 3: Analysis of change in success/failure rate*


This is a noteworthy milestone because it puts Yassir’s payment on par with global industry giants that have been in the domain for decades.


A post-mortem call with the provider revealed that this was the highest possible result achievable and top 1% by industry standard, and internal data was sufficient to validate their claim. In the payment space, It is impossible to achieve a 100% success rate because of scenarios such as insufficient funds, issuer declining transactions, the amount exceeding credit, canceled by users, amongst others, which are also an important part of the payment lifecycle, which reveals the activities/behavior of the entire ecosystem — from the issuing bank to the providers, the infrastructure partners, the integrators and finally, the end users being served.


Fig 4: analysis of unsuccessful transactions


**What This Means for the Rest of Africa**


Sometimes, innovation can mean creating something completely new; other times, it can mean discovering something novel and expanding it into a region where it does not exist, and this has been the case for us. Inspired by this Journey, we have moved beyond just standardizing our payment designs to auditing and co-designing the payment experience of payment partners in other parts of Africa to create a seamless yet secure payment experience for our customers in parts of the Maghreb region and West Africa, giving Yassir an opportunity to frontier innovation in the payment industry across the EMEA region.


Achieving this milestone is significant as it creates trust, which is necessary for individuals to add and, in fact, save their credit card details on e-commerce solutions. It allows the insights gained from this experience to be replicated in other EMEA regions with notoriously low adoption of digital payment — unlocking these regions for complete digital commerce while becoming a leader in payment orchestration. We believe that this is just day


**Acknowledgement:**


Boipelo.M, Yousra.H, Amadou.B, Marho.O, Mehdi.A, Xiaoxi.Z & Other members of the payment processing team.


#####
