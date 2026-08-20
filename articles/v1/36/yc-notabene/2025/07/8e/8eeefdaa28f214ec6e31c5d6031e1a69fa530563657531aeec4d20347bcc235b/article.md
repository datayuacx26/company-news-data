---
schema_version: "1.0.0"
document_id: "8eeefdaa28f214ec6e31c5d6031e1a69fa530563657531aeec4d20347bcc235b"
company_key: "yc-notabene"
company: "Notabene"
source_id: "yc-notabene-news-import-e4585f8c666e"
canonical_url: "https://notabene.id/post/how-can-vasps-ensure-travel-rule-compliance-during-transactions-with-unhosted-wallets"
published_at: "2025-07-24T00:00:00+00:00"
first_seen_at: "2026-07-26T08:22:57.520792+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:be98e47ed6416d13af030732a37a025879c1f3ee77108e46263921b52becc4f4"
---

# How Can VASPs Ensure Travel Rule Compliance During Transactions With Self-Hosted Wallets?

Since the[Travel Rule](https://notabene.id/product-travel-rule?utm_source=blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) was first applied to cryptocurrency by FinCEN in 2019, and with the Financial Action Task Force (FATF) following suit with its own related recommendations, self-hosted wallets (also known as non-custodial wallets) have come under increased scrutiny.


In October 2021,[FATF released](https://www.fatf-gafi.org/publications/fatfrecommendations/documents/guidance-rba-virtual-assets-2021.html) its Updated Guidance for a Risk-Based Approach to Virtual Assets and Virtual Asset Service Providers (VASPs). This guidance builds upon FATF’s initial 2019 recommendations, including directives on peer-to-peer (P2P) transactions—cryptocurrency exchanges that occur without the involvement of a VASP or other obliged entity.


While the standards do not apply to transactions solely between self-hosted wallets, FATF highlighted the potential money laundering and terrorist financing (ML/TF) risks they pose. Moreover, FATF clarified that transactions involving self-hosted wallets can fall under the scope of the Travel Rule under certain circumstances.


## **VASPs: What to Expect When Transacting With Self-Hosted Wallets**


VASPs face significant implementation challenges due to varying regulatory requirements across jurisdictions.


- In regions such as the[EU](https://notabene.id/world/eu?utm_source=blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) ,[UK](https://notabene.id/world/united-kingdom?utm_source=blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) , and[Gibraltar](https://notabene.id/world/gibraltar?utm_source=blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) , VASPs are required to collect information on their clients' self-hosted wallets.
- In[Singapore](https://notabene.id/world/singapore?utm_source=blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) and[Germany](https://notabene.id/world/germany?utm_source=blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) , VASPs must go a step further and verify the identity of the self-hosted wallet owner.
- [Liechtenstein](https://notabene.id/world/liechtenstein?utm_source=blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) mandates enhanced due diligence.
- [Switzerland](https://notabene.id/world/switzerland?utm_source=blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) requires both identity verification and proof of ownership.


Many in the cryptocurrency community have expressed concerns about these measures. Since blockchain is inherently public, sharing personal information associated with a self-hosted wallet could potentially expose the entire transaction history of that client, going beyond what the Travel Rule requires from traditional financial institutions.


Despite these concerns, VASPs must integrate solutions and establish processes to comply with FATF’s recommendations.


Below is an overview of what FATF expects from VASPs when interacting with self-hosted wallets:


‍


### **1. Obtain the Originator and Beneficiary Information from the VASP’s Customer (¶ 295)**


When sending or receiving a virtual asset transfer to a self-hosted wallet, the originator and beneficiary information must be obtained from the VASP’s customer, as there is no other VASP from which to obtain the information. This requirement generally applies to transactions above USD 1,000/EUR, but this threshold might vary depending on how jurisdictions implement it.


To remain compliant, VASPs must collect all the necessary Travel Rule information, such as names, account numbers or wallet addresses, addresses or IDs, birth dates, and birthplaces, without compromising user experience.


Blockchain analysis solutions like[Chainalysis KYT](https://www.chainalysis.com/chainalysis-kyt/?utm_source=notabene&utm_campaign=TR-blog-series) enable VASPs to identify Travel Rule transactions, ensuring frictionless data collection automatically. In combination with solutions like Notabene, VASPs can[gather the necessary data](https://notabene.id/wallet-identification?utm_source=blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) in a user-friendly way and automatically detect the jurisdictional requirements and thresholds applicable to each transaction.


‍


### **2. Enforce AML/CTF Obligations (¶ 295 & 296)**


Travel Rule guidance applies only above certain thresholds, which vary depending on the jurisdiction. However, VASPs are required to perform Know Your Customer (KYC) checks and implement transaction monitoring, regardless of whether their customer’s transactions meet the Travel Rule requirements.


Tools like Notabene can assist compliance teams in efficiently implementing the[data collection](https://notabene.id/wallet-identification?utm_source=blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) and verification process for the owner of a self-hosted wallet. Integrating a Travel Rule solution with an automated transaction monitoring tool allows VASPs to identify which transactions meet the Travel Rule threshold immediately. Additionally, these tools help compliance teams automatically detect if transactions are related to potential high-risk activities and take action when historical transactions become risky in light of new regulatory information through[continuous monitoring](https://blog.chainalysis.com/reports/kyt-continuous-monitoring/) .


Implementing the right solution enables compliance teams to adapt more efficiently to ongoing industry changes. If a solution flags a high number of false positives, analysts may have to allocate significant time to investigating non-critical alerts. Worse still, incorrect data could lead them to draw inaccurate conclusions.


‍


### **3. Implement Additional Risk Mitigation Measures (¶ 297)**


Additional risk mitigation measures may be necessary when interacting with self-hosted wallets. FATF’s guidance considers transactions with self-hosted wallets potentially higher risk, providing VASPs with options to treat them accordingly. These measures can range from imposing additional limitations and controls to avoiding interactions with self-hosted wallets altogether.


FATF advises VASPs to observe patterns of conduct, evaluate local and regional risks, and review information and bulletins issued by regulators and law enforcement to form their own risk assessments. Although this recommendation is optional, it raises concerns about the potential impact on industry adoption, as self-hosted wallets are integral to the cryptocurrency ecosystem. They are commonly used for legitimate purposes, such as securely moving funds and holding long-term investments.


Blockchain analysis tools can equip VASPs with the necessary data regarding self-hosted wallets to conduct comprehensive risk assessments, mitigate risks, and support their decisions in front of regulators.


‍


## ‍ **Global Approaches to Self-Hosted Wallet Regulation**


VASPs face numerous challenges due to differing requirements across jurisdictions. The FATF’s third targeted update on the global implementation of its standards revealed that around 70% of jurisdictions are still undecided on their approach to transactions between VASPs and self-hosted wallets. Among the jurisdictions that have made decisions, about 40% align with FATF recommendations, requiring VASPs to collect relevant beneficiary or originator information from their customers. Additionally, 25% of these jurisdictions have implemented mitigation measures or transaction limitations, such as identity verification of self-hosted wallet owners or enhanced due diligence procedures.


- In Liechtenstein, VASPs are not required to apply the Travel Rule to transactions with self-hosted wallets. However, they must enforce enhanced risk mitigation measures, such as using blockchain analytics to assess transaction risks, collecting documentation on the purpose of the transaction, and requiring customers to prove ownership of their self-hosted wallets when transacting with them.
- Japan closely aligns with FATF recommendations. VASPs in Japan are required to collect the necessary information from their customers regarding the owner of the self-hosted wallet involved in a transaction. However, there is no obligation to verify this information. This approach, requiring data collection without verification, is widely adopted and can also be seen in jurisdictions like Gibraltar and the European Union for transactions amounting to 1,000 EUR or less.
- The European Union follows a stringent approach when dealing with self-hosted wallets, as outlined in the revised Transfer of Funds Regulation. For transactions exceeding 1,000 EUR, European CASPs (Crypto Asset Service Providers) must verify the ownership of the self-hosted wallet, whether they are sending or receiving funds. This wallet ownership verification requirement aligns with FATF recommendations and is similarly applied in other jurisdictions like Hong Kong and Portugal.
- Switzerland has adopted one of the strictest approaches to self-hosted wallet transactions. Under Article 10 of FINMA’s guidelines, Swiss VASPs are required to identify and verify the identity of the self-hosted wallet owner, regardless of whether the transaction involves another VASP or a self-hosted wallet. This requirement ensures that VASPs can prevent problematic payments by ensuring all transactions meet stringent identity verification standards.


‍


## **What the data says about self-hosted wallets**


In December 2020, when the[Treasury’s 72-page NPRM for transactions with self-hosted wallets and certain foreign jurisdictions](https://blog.chainalysis.com/reports/treasury-department-nprm-unhosted-wallets-2020/) came out, Chainalysis analyzed the data on cryptocurrency transactions involving self-hosted wallets.


The data shows that the majority of the funds held in self-hosted wallets often come from VASPs, which are related to investing purposes or are used by individuals or organizations to move funds between regulated exchanges. It is important to mention that the 2021 data didn’t vary significantly in comparison to the 2020 analysis. There are still three trends related to the usage of self-hosted wallets.


### **1. The vast majority of the Bitcoin funds transferred to self-hosted wallets came from VASPs**


During Q3 of 2021, almost 83% of the bitcoin sent from one self-hosted wallet to another originated from cryptocurrency exchanges, and only 2% came from illicit services. This means that in the vast majority of cases, law enforcement can investigate illicit activity related to self-hosted wallets by working with cryptocurrency exchanges, which are obligated entities, and obtaining KYC information from them through legal process.


### **2. The majority of bitcoin sent to non-VASPs are eventually sent to a VASP**


Many transfers sent and received by self-hosted wallets have VASPs on the other side of the transaction. If cryptocurrency is being used for illicit purposes, criminals will eventually need to cash out their illicit proceeds. This means going through a cryptocurrency exchange (we can see this behavior reflected in our data). As long as they are in a country that regulates cryptocurrency exchanges – and this list is growing – exchanges will collect KYC information. Access to this information is vital to financial crime investigations.


During Q3 2021, the percentage of funds that were not sent to an exchange service decreased from 29% to 18% in comparison with Q2 2020. Meanwhile, the percentage of funds sent to exchanges increased from 62% to 71%. This means that crypto holders moved the funds they were holding inside self-hosted wallets to an exchange, maybe to take out some profits due to the crypto bull market we experienced this year.


‍


### **3. The transaction activity levels among self-hosted wallets highly suggest that their primary use is for investment**


After funds are deposited to a self-hosted wallet from an exchange, the percentage of bitcoin moved to another self-hosted wallet in a given month is significantly low. The majority of the bitcoin stays in the original wallet for a long period of time. On average, the funds originated from a VASP to self-hosted wallets move only once a month, which likely indicates that the primary use case is investment.


Chainalysis’ robust blockchain dataset provides key insights into the role of self-hosted wallets in the cryptocurrency ecosystem. If the main purpose of these regulatory requirements is to decrease illicit transactions and avoid money laundering, targeting self-hosted wallets may not accomplish the intended objective.


**Chainalysis's blockchain analysis data makes it clear that self-hosted wallets are not** **inherently risky and do not inhibit law enforcement’s ability to** **investigate the illicit use of cryptocurrency.** Blockchain analytics can inform risk analysis and compliance programs so that compliance teams can mitigate risks responsibly and effectively.


## **What’s next?**


Travel Rule guidelines have already been released by the regulators and VASPs have a deadline to build compliance programs to comply with it. We know this process can be overwhelming, but luckily, there are many available solutions to facilitate this process for VASPs, and there will likely be many more as the cryptocurrency industry continues to overlap with the traditional financial system.


[Chainalysis](https://www.chainalysis.com/?utm_source=Notabene&utm_campaign=TR%20blog%20series) and Notabene have created an[integrated solution](https://blog.chainalysis.com/reports/chainalysis-notabene-travel-rule-integration/) that helps VASPs save time and money while looking to meet the complete Travel Rule requirements and build their own risk assessment on self-hosted wallets.


Our integration covers a variety of compliance needs that can simplify the technical and operation integration process. Notabene’s end-to-end Travel Rule solution provides counterparty[wallet identification tools](https://notabene.id/wallet-identification?utm_source=notabene_blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) , a[VASP due-diligence directory](https://app.notabene.id/directory?utm_source=notabene_blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) , and[a secure dashboard](https://notabene.id/product-travel-rule?utm_source=notabene_blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) to help financial institutions manage counterparty risks without hindering user experience. In conjunction with Chainalysis, VASPs can immediately identify counterparties’ wallet types, get automatic transaction alerts on risky activity, and perform continuous monitoring, all in one place.


Choosing the right partners can save compliance teams time, resources, and protect the company from additional regulatory scrutiny or even fines.


Contact the[Chainalysis](https://www.chainalysis.com/contact-us/?utm_source=blog&utm_medium=cta&utm_campaign=notabene-travel-rule-blog-2) and[Notabene](https://notabene.id/sign-up-for-notabene?utm_source=notabene_blog&utm_medium=organic&utm_campaign=toolkit&utm_term=unhosted%20wallets) teams for more information.
