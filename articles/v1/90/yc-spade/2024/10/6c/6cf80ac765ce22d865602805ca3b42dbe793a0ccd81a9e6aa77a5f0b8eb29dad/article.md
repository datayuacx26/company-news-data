---
schema_version: "1.0.0"
document_id: "6cf80ac765ce22d865602805ca3b42dbe793a0ccd81a9e6aa77a5f0b8eb29dad"
company_key: "yc-spade"
company: "Spade"
source_id: "yc-spade-news-import-2301de415681"
canonical_url: "https://spade.com/resources/spade-mercury-building-a-best-in-class-corporate-card-program/"
published_at: "2024-10-29T15:02:34+00:00"
first_seen_at: "2026-07-24T01:52:25.955244+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:46262b00d5990c9722c01de9de8759ed59f30e5bca41c91809ebaa8afe16372e"
---

# Spade & Mercury: Building a best-in-class corporate card program | Spade

Startups and small businesses use Mercury’s banking and software to power all their financial workflows. With banking, treasury, and credit products tailored to founders, finance leads, and teams, Mercury provides innovative tools that make it easier to grow your business.


To build their corporate card product, Mercury partnered with Spade, a leading provider of real-time merchant intelligence and enriched transaction data. Because scalability is so important to Mercury’s customers, it was essential to build Mercury’s corporate card on the most comprehensive transaction data available.


Using Spade’s data, Mercury has created a best-in-class corporate card that:


- Leverages automation to save hours of manual accounting work each month
- Features granular spend controls with sophisticated merchant and category locking
- Works as well for a team of hundreds as it does for a team of one


In addition, Spade provides a reliable data layer that will power Mercury’s roadmap as they develop new products to meet their customers where they are.


---


# The challenge


Mercury is a financial technology company that builds best-in-class banking products for startups and other high-growth businesses. Founders bank with Mercury because their products are designed to work at any scale while minimizing complexity and manual processes, which drastically cuts down on time and money spent overseeing finance and accounting.


Mercury wanted to build a card program tailored to these requirements, but found that the transaction data coming from the card networks wasn’t sufficiently detailed. A typical “raw” transaction, for instance, could look like this:


**A typical raw transaction without Spade’s enrichment**


It’s difficult to decipher where this transaction actually took place, and this is the rule when it comes to card transaction data, not the exception. Almost everyone has struggled trying to decipher short and confusing descriptors on their card statements. Because card transactions have become so ubiquitous, teams have to build products using the data they generate – however, the poor quality poses a challenge to building innovative products.


Having access to more granular, detailed information about card transactions would allow Mercury to enable a variety of other innovations necessary to deliver a best-in-class card program including:


- Automation for customers’ most time-consuming processes
- Granular spend management tools that work at scale
- A seamless experience that makes recognizing spend effortless


---


# The solution


To build an effective corporate card program, Mercury needed better transaction data. To build a best-in-class one, they needed Spade.


After a thorough evaluation of the market, Mercury selected Spade as their enrichment partner. In real-time, Spade receives transactions from Mercury and matches them to a real merchant identity in Spade’s proprietary data network. Once matched, Spade provides granular firmographic (factual) information about each merchant (including a unified ID for every instance of a merchant, accurate categories, granular location data, and even vendor logos). Spade also provides insights to help assess the risk profile of a merchant (for example, whether that merchant has risky content on their website or frequent negative reviews).


The same “raw” transaction described above — this time enriched with Spade’s real-time merchant intelligence — provides much more information:


**The same transaction, enriched with Spade’s merchant intelligence**


### **Today, Mercury uses Spade’s enriched data to support use cases across their best-in-class card program:**


**Automating transaction categorization**


- Businesses are responsible for effectively categorizing transactions on P&L statements for accounting purposes, and inaccurate categories lead to hours of manual work. Mercury knew it was critical to automate categorization and take this work off founders and finance teams’ plates. However, the categories that are established via international payment standards (Merchant Category Codes, or MCCs), don’t have the accuracy or granularity needed to do this effectively – for example, Amazon is still categorized as a bookshop.
- Spade’s industries are accurate, granular, and effective – showing AWS as Technology – Web Infrastructure, Amazon Music as Media – Streaming Services and day-to-day Amazon orders as Retail – Online Marketplace transactions.
- Using Spade’s industries, Mercury built a custom auto-categorization system that accurately assigns transactions to categories with limited to no manual intervention.


> “If we can categorize the type of spend that’s happening at a company, we can set up rules to automatically categorize those transactions, and this alone can save hours each month. If you add that up over a year, that’s days to weeks each year. For finance teams, it definitely helps, right? It makes their jobs easier. But for founders, this is especially important because that’s time they could be using to invest in their company.” – *Anish Bhayani, Engineering Manager, Mercury*


***Mercury’s auto-categorization feature built using Spade’s enriched data***


**Implementing granular spend controls**


- Giving employees the ability to spend without manual processes and approval for each transaction helps teams scale – but it adds risk, unless granular controls like locking cards to categories and merchants can be implemented. Mercury uses Spade’s unified merchant IDs and accurate categories to do just that, allowing businesses to maintain control while operating efficiently.
- Card payments are tracked via MIDs (merchant IDs linked to the point-of-sale device the payment is processed on). This level of detail is critical for money movement, but there’s a piece missing – the view of payments on a brand level. The card payment standards don’t offer a unified ID for every instance of a merchant – e.g., Walmarts in the US have over 40,000 unique IDs. This unification is critical for use cases from locking cards to specific merchants, blocking transactions at high risk merchants, and offering insights into spend at a brand level.
- To offer those types of products, financial institutions often have to keep track of those IDs, or rely on regex that can miss new descriptors like WMRTUS. This is unreliable and time intensive.


- Spade offers a single counterparty ID that is a unified identifier for all locations and channels of a merchant, allowing Mercury to easily lock cards to merchants without false approvals or declines.
- Additionally, Spade’s accurate industry system ensures reliability of category-locking cards, allowing founders and finance teams to confidently issue cards to their employees without manually reviewing each purchase.


**Mercury merchant-locked card flow**


**Detecting and preventing fraud**


- Mercury came to Spade focused on transaction categorization, but the consistently high quality of Spade’s data unlocked further innovations across Mercury’s products — including in fraud and risk.
- Mercury now uses Spade to detect and prevent high-risk transactions by identifying meaningful correlation between Spade’s insights and risky transactions — allowing them to better support and protect their customers.


---


# Why Spade


Mercury chose Spade because the unique speed, accuracy, and support unlocked key use cases for today, and enabled continued partnership to support future innovation.


**Industry-leading accuracy**


- Because Spade enriches transactions with real merchant data, it offers industry-leading accuracy – Spade’s internal testing shows >99% accuracy.
- This was substantiated with Mercury’s findings: Using Spade’s industries, Mercury powered a proprietary P&L categorization feature that saves finance teams hours. **Less than 1% of transactions have been recategorized manually.**


**Unparalleled speed**


- Mercury’s corporate card product needed excellent transaction data, but it also needed it quickly. Card issuers have <2 seconds to decide whether to approve or decline a transaction — this means that unless Mercury could also obtain the enriched data in real-time, they couldn’t use it for authorization decisions.
- **Spade’s industry-leading speed of 50ms p99 was faster than Mercury’s requirements for what they wanted to build.**


**Hands-on partnership**


- Spade offers hands-on, consultative support throughout the customer journey – enabling our customers to get the most out of our partnership.


> *“We’ve been working with the Spade team for over a year now and our experience has been very positive. Spade plays a key role in helping us achieve our goals on our roadmaps, and we feel that the team is knowledgeable and professional. Spade is one of the partnerships that we genuinely value and look forward to continue to build together”. – Woolie Tsui, Partnerships Manager, Mercury*


---


# Key takeaways


With Spade, Mercury created a best-in-class corporate card program. Startups and other growing businesses can use Mercury to:


- Reliably automate time-consuming accounting processes
- Implement granular spend controls using detailed categorization
- Stop fraud and card misuse before it happens


Mercury and other financial services companies can rely on Spade for:


- Fast, 99.9% accurate transaction data that’s more comprehensive than the alternatives
- A rich data layer that opens doors to innovative use cases
- Ongoing support to derive meaningful value from the partnership


Spade’s real-time merchant intelligence powers Mercury’s best-in-class products and guarantees a better banking experience for startups and small businesses that want to grow.


Want to learn more about Spade?Get in touch with our team .


*Note from Mercury: Mercury is a financial technology company, not a bank. Banking services provided by Choice Financial Group and Evolve Bank & Trust®; Members FDIC. The IO Card is issued by Patriot Bank, Member FDIC, pursuant to a license from Mastercard.*
