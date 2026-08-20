---
schema_version: "1.0.0"
document_id: "d029286c8c94b877517082733f3d26294728099be383b5be413b3c2c5ba46457"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/banking-and-capital-markets/impact-iso-20022-correspondent-banks-part-2"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:6027c6b520e31d6a2cc031a33494ca3a5baf2e6c6bf047c0382234184b2bd9d6"
---

# The impact of ISO 20022 on correspondent banks, part 2

As correspondent banks prepare for the global migration to ISO 20022, they face two key challenges. The first involves making the necessary changes to their IT infrastructure. Should they replace their legacy systems, apply an intelligent wrap or turn to managed services? This was the topic of[my first blog](https://www.cgi.com/en/blog/banking-and-capital-markets/impact-iso-20022-correspondent-banks-part-1) in this two-part blog series. In part 2, we address the second challenge—providing dual support for both legacy MT messages and the new MX messages during the transition to ISO 20022.


Between the years 2022 to 2025, as ISO 20022 goes into effect, the current SWIFT MT messaging network will undergo massive change. During this transition phase, correspondent banks will need to support both legacy SWIFT MT messages and new ISO 20022 MX messages. This period of duality will be difficult to manage.


Essentially, if your core processing solution is legacy MT and you receive an enriched MX message that you need to pass on to a correspondent bank, there are a few key requirements:


1. Completing regulatory and risk scanning of the full MX message
2. Storing full MX messages for both regulatory and reconstruction purposes
3. Reconstructing an outgoing MX message by combining the original incoming MX message with the downgraded MT message that you have processed
4. Validating that your reconstructed message is still compliant with the MX rules.


What is the best approach for meeting these requirements and managing dual support for MT and MX messages? Below are two paths for consideration.


**A case for rapid renewal**


Modern deployment methods, agile processes and cloud technologies now make it possible to replace core legacy infrastructure in 12 months or less. Deploying a modern, highly available, service-based solution will vastly reduce your operational costs, and putting that solution into cloud infrastructure will further reduce costs while driving business agility.


While the investment in translation and reconstruction technology is tempting, this is inevitably throwaway work with no shelf life beyond 2025. If you can commit to a rapid deployment renewal and be ready ahead of the November 2022 duality phase, then your bank will be in a strong position.


Ideally, to achieve this, you need a solution that meets the following criteria:


- Is truly ISO-native, with an XML data store
- Is ready to be deployed on cloud infrastructure
- Is supported by implementation and managed service resources


**A case for modernization-in-place**


Modernization-in-place (MIP) is an alternative to rapid renewal that focuses on achieving immediate performance gains while preserving the value of legacy technologies. By proceeding incrementally and using extensive testing along the way, successes accumulate in a series of small project steps, leading to better operational continuity and quality outcomes with reduced risk.


The MIP approach offers several key advantages:


- Protects past knowledge investments
- Creates a more open and nimble architecture
- Significantly reduces risk by leveraging the same hardware and external interfaces, as well as the same people
- Facilitates the migration of capable components to the cloud while leaving other legacy components in place
- Supports a clear vision and path for modernization


However, to succeed with MIP as a correspondent bank, you need to have a service that goes beyond translation and can successfully reconstruct a message from the incoming MX and processed MT. ISO 20022 will take effect beginning in November 2022. While that may seem a ways off, the timeline for change is short at best. CGI is working with correspondent banks to help them prepare. To learn more about our work, feel free to contact me. More in-depth information on the migration to MX messages also is available in our new white paper,[From Heritage to Hypernew: Exploring MX Options for Correspondent Banks](https://www.cgi.com/us/en/white-paper/banking-and-capital-markets/heritage-hypernew-exploring-mx-options-correspondent-banks) .
