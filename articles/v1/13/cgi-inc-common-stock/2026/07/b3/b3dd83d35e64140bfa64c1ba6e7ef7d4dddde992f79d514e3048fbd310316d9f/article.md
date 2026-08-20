---
schema_version: "1.0.0"
document_id: "b3dd83d35e64140bfa64c1ba6e7ef7d4dddde992f79d514e3048fbd310316d9f"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/banking-and-capital-markets/three-data-must-haves-in-applying-genai-to-payments-processing"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:8051ab62eeb83d2572ddd659b7096f75fdaa8cf758bf0bbdfbcbfc87813d9e65"
---

# Three data must-haves in applying GenAI to payments processing

A few years ago, I lived in a house in Toronto with a very common street name, Main Street. While the address was easy to remember, such a common name is often used as a “generic” or “fictional” address by people wanting to avoid junk mail being sent to their real addresses. This meant a lot of junk mail came rattling through my letterbox.


In 2018, this situation took a more sinister turn when I started receiving letters and debit cards from a large Canadian bank with my address but a variety of names. Having reported this to a senior contact at the bank, I later found out this was part of an insider fraud scheme where thousands of fake accounts had been created, and a large investigation was required to address the problem. The bank was subsequently fined an exceptionally large sum by their regulator for failures that led to the fraud.


The time it took to uncover the scheme, the money lost and laundered, and the subsequent fines imposed serve as a poignant example of the fiscal and reputational value of proactive data analytics, which could have revealed the issues much sooner.


Publicly available information shows that the modest three-bedroom, semi-detached home at my address could realistically house a maximum of six adults. Likewise, it’s uncommon for every person in a household to have different family names. These are anomalies that trained AI-driven data analytics could have quickly identified in a database to ultimately avoid the fraudulent activity that took place. However, many banks are challenged in managing data and leveraging the full power of generative AI (GenAI), particularly in the payments space, to provide this proactive protection. Common issues include lack of data quality, alignment, and governance.


### Data quality: GenAI demands clean and compliant data


Clearly, the starting point for getting the best out of GenAI tools in the payments space is improving data quality. While these tools can deliver a wealth of advantages, Marcus Martinez, FSI Industry Adviser at Microsoft, gave cautionary advice on its use during a CGI roundtable at EBAday 2024: “There's a huge concern with data quality. And, while I appreciate the whole hype around GenAI adoption, I think the industry is coming to the realization that, first and foremost, the data quality must be good before you start the journey.”


Different organizations have different data cleansing approaches—some better than others. But the bottom line is that, before GenAI can be effectively applied and deliver the best results, data must be both clean and compliant.


### Data alignment: Fraud and right-to-be-forgotten challenges


Payment service providers typically focus on maximizing straight-through processing (STP), which checks for affirmative answers to questions such as: Is this an account? Are there funds? Is there permission to send funds? The payment is then shuttled through screening and filtering processes to check if it’s fraudulent, non-compliant, or embargoed.


This approach, however, can create potential fraud gaps because data checked as part of the STP processing isn’t always validated in the compliance process flow. As a result, the payment data is never examined holistically.


Likewise, inbound and outbound payments are rarely compared, enabling problems to creep in. An inbound payment, if accepted, is generally stored “as is,” so in a payments database there may be multiple versions of customer records versus a single unified record. Not only does this create issues for traditional pattern checking, but it also makes the right-to-be-forgotten, mandated under Europe’s[General Data Protection Regulation (GDPR)](https://gdpr.eu/what-is-gdpr/) law, significantly more difficult for rule-based systems to deliver.


ISO 20022 has been a big step toward achieving data alignment, particularly once structured address formats are adopted. Thanks to native systems such as[CGI All Payments](https://www.cgi.com/en/solutions/cgi-all-payments) , the bulk of banks now store payment data in this rich format.


However, rationalizing data and applications requires a better understanding of the payments data journey within an institution. This is yet another opportunity where GenAI tools can create complex mappings that simplify and unify data while driving efficiency.


### Data governance: Keeping humans in the loop


Aligning payment recipient details for inbound payments with existing customer data makes it easier to synchronize and manage payment histories. If, at the same time, an AI bot runs anomaly and compliance checks on that inbound data, you can connect the dots more easily.


Catching patterns that indicate your customer is a potential mule or part of a pool of fake accounts becomes much faster when those patterns are in line with acceptance and tied to internal or external data sources (e.g., land registries, electoral registers, and social media).


However, as we implement GenAI tools, it’s important to maintain the right checks and balances to ensure that identified behavior patterns are verified before action is taken and that false positives are returned to the GenAI model so that it can improve. In short, we need to keep humans in the loop.


During the CGI roundtable at EBAday 2024, Daniel Szmukler, Head of Innovation at the Euro Banking Association, explained, “I believe AI doesn’t stand for artificial intelligence when we’re talking about banking. It stands for augmented intelligence. We augment with automation and machine intelligence, but we can’t let the machine entirely take over the decision-making process.”


The European Union strongly agrees with this view with its release of the recent[EU Artificial Intelligence Act](https://artificialintelligenceact.eu/the-act/) , which is designed to govern how this powerful technology can be used responsibly. In September 2024,[CGI signed the EU’s AI Act](https://www.cgi.com/en/cgi-signs-the-european-commissions-artificial-intelligence-act-pledge) Pledge as part of its engagement with the European Commission’s AI Pact. The signature reinforces CGI’s commitment globally to uphold the highest standards and best practices in the development and use of responsible technologies, including innovative AI and GenAI technologies.


### Advancing your GenAI journey through better data management


The power of GenAI to transform payments processing is a reality. However, effective data management that ensures high-quality, aligned, and well-governed data across the enterprise is key to GenAI’s success. CGI is working with payment providers across the globe to create competitive advantage through GenAI. To learn more about our work, feel free tocontact me or visit[cgi.com](https://www.cgi.com/en/banking-capital-markets/artificial-intelligence) .
