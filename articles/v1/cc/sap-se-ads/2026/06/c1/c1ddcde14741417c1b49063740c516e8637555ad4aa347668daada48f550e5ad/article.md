---
schema_version: "1.0.0"
document_id: "c1ddcde14741417c1b49063740c516e8637555ad4aa347668daada48f550e5ad"
company_key: "sap-se-ads"
company: "SAP SE"
source_id: "sap-se-ads-rss-5a5b87a458d9"
canonical_url: "https://news.sap.com/2026/06/omv-data-driven-workforce-decisions/"
published_at: "2026-06-29T12:15:00+00:00"
first_seen_at: "2026-07-25T22:15:27.001075+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:f3b8ca592d1a078060be88dd2229383c9610c7242e388cb9a7b4b5d32822e82e"
---

# OMV’s Approach to Data-Driven Workforce Decisions

Do your workforce insights drive decisions or sit in dashboards? OMV uses the[People Intelligence solution in SAP Business Data Cloud](https://www.sap.com/products/hcm/data-cloud.html) (SAP BDC) to spot workforce composition patterns and move talent where it’s needed. Looking ahead, OMV plans to expand into workforce planning and learning analytics, bringing people investments closer to measurable business outcomes.


[OMV](https://www.omv.com/) is a multinational oil, gas, and chemical company headquartered in Vienna, Austria. With operations spanning Europe, the Middle East, Africa, New Zealand, and Norway, OMV is a truly global enterprise.


Like much of the energy sector, OMV is navigating a significant strategic pivot. The company is investing heavily in sustainability initiatives: transforming plastic waste back into oil, building one of Europe’s largest waste-sorting facilities to produce feedstock for refineries, and recycling plastic cups collected from aircrafts into sustainable kerosene. This shift in the business model has triggered a corresponding transformation inside the business—and nowhere more so than in HR.


OMV’s People and Culture (P&C) function launched a strategic program to place people at the center of the company’s transformation. The ambition was clear: become a global HR center of excellence, increase service quality, standardize and harmonize processes, and move decisively towards digitization, automation, and self-service.


## **The challenge: fragmented data and a manual reporting cycle**


Before SAP BDC entered the picture, OMV’s HR data landscape was fragmented across a patchwork of systems that were never designed to work together for workforce reporting and analytics. Employee data lived in two on-premise SAP[HCM](https://news.sap.com/topics/human-resources/) systems and SAP SuccessFactors HCM, alongside Microsoft Excel, SharePoint, and a system originally built for financial consolidation that P&C used for headcount reporting and planning.


Drive better people and business decisions across hiring, retention, pay, and more


[Discover how](http://www.sap.com/people-intelligence)


The day-to-day consequences were significant. When a business unit head or department manager wanted a workforce KPI—headcount figures, turnover rates, or anything beyond a basic report available in the system—they would raise a request with their HR business partner. From there, the HR business partner would spend considerable time navigating multiple systems, manually pulling data, compiling it into spreadsheets, and formatting it into a presentation before handing it back to the manager. It was time-consuming, error-prone, and consumed HR capacity that should have been spent on strategic work. Managers had no direct, self-service access to their own workforce data.


## **Choosing SAP Business Data Cloud and People Intelligence**


“Normally, our strategy is not to be the first with a new solution. With SAP BDC it was different,” Bernhard Graser, head of SAP Finance, HR, and Reporting at OMV, said. “We saw the potential immediately and wanted to stop the outbound migration of our HR and SAP data and keep it firmly in the SAP ecosystem.”


The timing was fortuitous. OMV had already completed a substantial SAP SuccessFactors HCM implementation, having deployed[SAP SuccessFactors Performance & Goals](https://www.sap.com/products/hcm/performance-goals.html) ,[SAP SuccessFactors Learning](https://www.sap.com/products/hcm/corporate-lms.html) , and[SAP SuccessFactors Succession & Development](https://www.sap.com/products/hcm/career-talent-development.html) and going live in 2023 with[SAP SuccessFactors Employee Central](https://www.sap.com/products/hcm/employee-central-hris.html) , SAP SuccessFactors Compensation, and SAP SuccessFactors Recruiting. With all core employee data now sitting in a cloud-based SaaS system, the foundation for SAP BDC connectivity was already in place.


## **OMV’s implementation of SAP BDC and People Intelligence**


OMV structured its SAP BDC journey in three steps.


The first step—turning People Intelligence live—was connecting SAP SuccessFactors HCM to SAP BDC. This was not entirely without friction: OMV discovered that its on-premise HCM systems sat on a different Identity Authentication service than SAP SuccessFactors HCM, which required alignment before integration could proceed.


A more substantive challenge was data governance. As an Austrian company with a Works Council, it was not possible for OMV to simply push all HR data into SAP BDC. The team implemented data masking, configured Read Access Logging, and established permission controls that mirror SAP SuccessFactors HCM exactly, meaning a user can only see data in SAP BDC that they are already authorized to view in SAP SuccessFactors HCM. This level of governance was a prerequisite before any business users could interact with the system.


The second step, currently in progress, involves migrating both HCM systems to[SAP S/4HANA](https://www.sap.com/products/erp.html) . Once complete, SAP S/4HANA will connect directly to SAP BDC, enabling a fully unified data feed from both SAP SuccessFactors HCM and SAP S/4HANA into a single platform.


The third step, planned for the near future, is the retirement of the legacy reporting stack entirely, eliminating the spreadsheets and replacing the current workaround in the financial consolidation system with SAP BDC as the single reporting and planning environment for HR.


SAP BDC’s architecture played a key role in the decision. SAP-managed data products—pre-built data models maintained and updated by SAP—were particularly attractive, especially because OMV had kept its SAP SuccessFactors HCM configuration close to standard. That near-standard posture meant a larger share of OMV’s HR data could be served through SAP-managed products, reducing the internal maintenance burden. When something changes in a source system or a data definition, it is SAP’s responsibility to update the model, not OMV’s.


## **Current and future use cases**


After evaluating the intelligent content available in People Intelligence, OMV decided to start with workforce composition insights, now live and providing out-of-the-box dashboards on headcount, workforce structure, and composition, fully configurable and filterable by business users.


With the foundation in place, OMV’s P&C team has been actively collecting ideas for what to build next on SAP BDC. On the operational side, the team wants to track accident-related data as a workforce KPI, monitor open positions across the business, and measure time-to-hire. Diversity is another priority—data that currently sits fragmented across systems. Through its participation in SAP’s forward deployed engineering program, OMV is co-building use cases around learning and certification compliance—a business-critical need in a refinery environment where workers must hold current safety certifications to enter operational sites—as well as skills and headcount. Looking further ahead, OMV intends to move into machine learning and predictive modelling, using the[SAP Databricks](https://www.sap.com/products/data-cloud/databricks.html) capability in SAP Business Data Cloud to forecast workforce demand and identify gaps in skills and FTEs before they materialize.


## **The bottom line**


The direction is clear: a single source of truth for HR data, self-service access for every manager and business unit head, and a platform capable of growing from descriptive reporting today into predictive workforce intelligence tomorrow. As Graser encouraged his audience at the end of his session at[SAP Sapphire](https://www.sap.com/events/sapphire/virtual.html) Madrid: “We see great potential in SAP BDC—not only in HR, but also in finance. You should try it.”


Learn more about People Intelligence[here](https://www.sap.com/products/hcm/data-cloud.html) .


---


Get weekly updates from the SAP News Center, delivered straight to your inbox


[Sign up now](https://www.sap.com/campaigns/nl/sap-news-center-newsletter)
