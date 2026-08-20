---
schema_version: "1.0.0"
document_id: "4c31da05829f3edd5b3f614f182ae127c450e7eb90c2ee1de0f503a88dc9caf0"
company_key: "yc-yassir"
company: "Yassir"
source_id: "yc-yassir-rss-45239b019a57"
canonical_url: "https://medium.com/@Yassirtech/how-we-achieved-a-93-success-rate-in-digital-payments-and-what-it-means-for-the-rest-of-africa-d67f7bdc8293"
published_at: "2024-05-22T10:32:00+00:00"
first_seen_at: "2026-07-24T07:42:37.830050+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:98eec969136715fa4100c9c14838249a869de72a0d942faaac6df467823c03b7"
---

# How we achieved a 93% success rate in digital payments, and what it means for the rest of Africa

# **How we achieved a 93% success rate in digital payments, and what it means for the rest of Africa**


[Welcome to our Yassir Engineering Blog](https://medium.com/@Yassirtech?source=post_page---byline--d67f7bdc8293---------------------------------------)


5 min read


·


May 22, 2024


--


**Introduction**


Digital commerce continues to grow in Africa. With the recent COVID-19 outbreak, restrictions were placed globally everywhere — from airports to retail outlets — forcing the adoption of digital commerce across multiple regions. As a result, Africans tuned in online and have not slowed down since. According to a global study by Mastercard conducted in 15 markets, consumers are increasingly moving away from cash and opting for contact-free and digital payment experiences — and they don’t expect to return. E-commerce is also seeing a surge, with[Mastercard SpendingPulse](https://www.mastercard.com/newsroom/press-releases/2020/june/mastercard-spendingpulse-estimated-53-billion-in-additional-us-e-commerce-sales-as-pandemic-drives-consumers-online-in-april-and-may/) reporting record growth rates across the globe as consumers increasingly shop online.


Although initially focused on the Maghreb, Yassir started its expansion across other EMEA regions, and this came with an accompanying and exciting challenge — localizing digital payments per region, which, according to the[World Economic Forum, 2018](https://www3.weforum.org/docs/WEF_Addressing_E-Payment_Challenges_in_Global_E-Commerce_clean.pdf) , is a determinant of the success or failure of e-commerce. This report documents the exciting challenges we faced, how we conquered the challenges, and how we are using the insights from this experience to shape novel innovations in different parts of Africa.


([Check our open vacancies here](https://jobs.lever.co/Yassir) !)


**The Challenge**


Strong dominance in the Maghreb region and tight government policies on financial services may have stifled the pace of digital innovation (e.g., card tokenization, server-to-server integrations, etc.), regardless of PCIDSS, SOC, or any other security/compliance standards. Issues such as this will naturally impact the creative boundaries of technical creatives; however, with Yassir, it was different, as the Payment team is armed with diversity. As with any startup, technical depth is often sacrificed for speed, but this was a good problem.


We had built a multi-currency payment orchestrator localized for each region, but we needed some time to observe product adoption, study usage patterns, and discover insights for optimizations and improvements. We noticed that in one of the markets — South Africa- Mastercard (MC) had more local dominance than Visa; ironically, MC debit cards had an unusual failure rate.


Press enter or click to view image in full size


***Fig 1: Comparative analysis of card processing networks adoption rate***


**Problem Solving with Data**


This challenge was a good one, as it originated from a hybrid security and user experience update MC had just made to their infrastructure to protect merchants from chargeback fraud while provisioning a seamless user experience for merchants to optimize their systems for recurring payments/revenue. A new instruction type was instituted — eliminating the need for cardholders to tokenize every time they wanted to make a payment online.


Analyzing the transactions revealed a format code 800.100.156 for unsuccessful transactions, unique to MC cards. This happened as a result of failed authentication when a customer wanted to make a payment. However, on discussing with our providers, we discovered a mandate requiring merchants to set their standingInstruction.type to UNSCHEDULED and point initial transactions to Customer initiated transaction (CIT) type, enabling first-time card users to authenticate but returning card not to, using the Merchant initiated transaction (MIT) type to by-pass auth enforcement.


Press enter or click to view image in full size


***Fig 2: Analysis of error codes***


The concept of 3D secure authentication is one of the paradoxes of the payment processing industry, because if authentication is not enforced, it makes the card acquirer vulnerable to chargeback fraud. However, it is enforced for every transaction, it can reduce the success rate of payments by up to 40%. Finding a balance will always be a no-brainer, and Payment processing leaders such as[Bolt](https://res.cloudinary.com/dugcmkito/image/upload/v1673854929/Transform_Guest_Shopping_to_A_Member_Experience_117f3292bf.pdf?updated_at=2023-01-16T07%3A42%3A09.831Z) who have succeeded at creating a seamless UX balance at scale, boast of increasing conversion rate by up to 70%.


**Business Outcome**


Like a Domino effect, solving this problem solved two other associated problems over time, such as risk management timeout and user authentication failure with format errors 100.380.501 and 100.380.401, respectively.


After deploying all the solutions, we observed a ~94% success rate for every unique customer who tried to make a payment and 84% success rate for unique transactions.


Press enter or click to view image in full size


***Fig 3: Analysis of change in success/failure rate***


This is a noteworthy milestone because it puts Yassir’s payment on par with global industry giants that have been in the domain for decades.


([Check our open vacancies here](https://jobs.lever.co/Yassir) !)


A post-mortem call with the provider revealed that this was the highest possible result achievable and top 1% by industry standard, and internal data was sufficient to validate their claim. In the payment space, It is impossible to achieve a 100% success rate because of scenarios such as insufficient funds, issuer declining transactions, the amount exceeding credit, canceled by users, amongst others, which are also an important part of the payment lifecycle, which reveals the activities/behavior of the entire ecosystem — from the issuing bank to the providers, the infrastructure partners, the integrators and finally, the end users being served.


Press enter or click to view image in full size


***Fig 4: analysis of unsuccessful transactions***


**What This Means for the Rest of Africa**


Sometimes, innovation can mean creating something completely new; other times, it can mean discovering something novel and expanding it into a region where it does not exist, and this has been the case for us. Inspired by this Journey, we have moved beyond just standardizing our payment designs to auditing and co-designing the payment experience of payment partners in other parts of Africa to create a seamless yet secure payment experience for our customers in parts of the Maghreb region and West Africa, giving Yassir an opportunity to frontier innovation in the payment industry across the EMEA region.


Achieving this milestone is significant as it creates trust, which is necessary for individuals to add and, in fact, save their credit card details on e-commerce solutions. It allows the insights gained from this experience to be replicated in other EMEA regions with notoriously low adoption of digital payment — unlocking these regions for complete digital commerce while becoming a leader in payment orchestration. We believe that this is just day 1.


([Check our open vacancies here](https://jobs.lever.co/Yassir) !)


**Acknowledgement:**


Boipelo.M, Yousra.H, Amadou.B, Marho.O, Mehdi.A, Xiaoxi.Z & Other members of the payment processing team.


**Onothoja Ron (Ogheneruona),**


**Product Owner/Lead, Payments processing.**


[Learn more about Yassir](https://www.welcometothejungle.com/en/companies/yassir)
