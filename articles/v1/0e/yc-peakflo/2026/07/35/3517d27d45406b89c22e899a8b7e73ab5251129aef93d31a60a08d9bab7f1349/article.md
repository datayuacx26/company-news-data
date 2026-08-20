---
schema_version: "1.0.0"
document_id: "3517d27d45406b89c22e899a8b7e73ab5251129aef93d31a60a08d9bab7f1349"
company_key: "yc-peakflo"
company: "Peakflo"
source_id: "yc-peakflo-news-import-4ba227f4ca0c"
canonical_url: "https://peakflo.co/blog/unknown-sku-handling-new-product-code-creation-customer-po-erp"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T22:55:40.942818+00:00"
fetched_at: "2026-07-31T22:55:41.445460+00:00"
content_hash: "sha256:ef7e0cbc82eb94c1595422f03b0e9f79fa42ee58c9592eb971829beee7122070"
---

# Unknown SKU in Customer Purchase Order: How Manufacturers Handle New Product Code Creation Without Blocking Order Processing (2026)

## ⚡ TL;DR


When a customer purchase order contains products that don't exist in your ERP, traditional order processing grinds to a complete halt — even if 19 out of 20 items on the PO are perfectly matched. In Asian manufacturing and distribution, unknown items affect 15–25% of incoming PO lines in new customer relationships. The solution is partial PO processing: known items get drafted into a Sales Order immediately, while unknown items are routed to the product team for code creation via AI-assisted workflows. Once the new code is created and the match is stored, every future order from that customer auto-processes without exception routing.


## The Unknown Item Problem in Customer Purchase Order Processing


Picture a common scenario in Asian manufacturing: a new customer sends their first purchase order. It contains 15 line items. Twelve of them can be matched to your internal ERP SKUs through a combination of historical order data and description matching. Three of them — new products the customer has just started sourcing — have never been set up in your system.


In a traditional workflow, the answer to this scenario is simple: nothing moves. The entire PO sits in a pending queue until someone manually creates the three missing product codes, loads them into the ERP, and restarts the SO creation process. The twelve items that were perfectly ready to process are also frozen — waiting for the three new ones to be resolved.


This all-or-nothing bottleneck is one of the most common sources of order processing delays in[manufacturing companies](https://peakflo.co/industries/manufacturing) across Southeast and East Asia, and it becomes most acute during two predictable moments: the first 6 months of a new customer relationship (when the product catalog overlap is lowest), and product range expansion cycles (when customers begin sourcing new items you manufacture but haven’t set up for that customer yet).


According to[Gartner research on supply chain management](https://www.gartner.com/en/supply-chain) , order processing delays tied to data quality issues — including missing product master records — cost manufacturers an average of 1.5–2% of order value in expediting costs, lost sales, and relationship friction annually.


The operational reality is that unknown items are not a rare exception — they are a predictable, recurring feature of B2B manufacturing order processing that deserves a purpose-built workflow.


## Why New Items Block the Entire PO in Traditional ERP Workflows


To understand why this problem is so persistent, it helps to understand the structural constraint that creates it.


In most ERP systems — including SAP, NetSuite, Microsoft Dynamics, and Globe3 — a Sales Order line item cannot be created without a valid internal product code. The ERP enforces referential integrity: every SO line must point to a product master record. If that record doesn’t exist, the line cannot be added to the SO.


This constraint exists for good reasons. Product master records carry cost, GL coding, tax treatment, unit of measure, warehouse storage rules, and vendor relationships — all information that downstream processes (production scheduling, warehouse picking, invoicing, reporting) depend on. An SO line without a valid product master would create cascading data gaps throughout the business.


The problem is not the constraint itself — it is the absence of a workflow to handle the constraint gracefully. Traditional approaches leave only two options:


**Option A — Block the entire PO** until all line items are resolved. Twelve ready items wait while the three unknown items are manually researched and created.


**Option B — Manually split the PO** into two parts: one SO for known items (processed now) and a second SO promised for later (when unknown items are created). This requires manual judgment, manual split, and coordination with the customer that their delivery will be split — adding communication overhead and customer friction.


Neither option scales for manufacturers processing 50+ customer POs per month across multiple customers, each with different product catalogs at different stages of relationship maturity.


## How Frequently Does the Unknown Item Problem Actually Occur?


The frequency varies significantly by customer relationship stage and product category complexity. Based on[APQC benchmark data on order-to-cash processes](https://www.apqc.org/resource-library) , here is a reliable pattern for Asian manufacturers and distributors:


Customer Relationship Stage % of PO Lines with Unknown Items Primary Cause


Months 1–3 (new customer) 20–30% Broad catalog mismatch — no shared order history


Months 4–12 (early relationship) 10–20% Ongoing product mix stabilization


Months 13–24 (established) 5–10% Occasional new product introductions


Months 25+ (mature relationship) 2–5% Only genuine new product launches


After product range expansion event Spike to 15–25% temporarily Customer adds new sourcing categories


For a manufacturer receiving 40 customer POs per month with an average of 12 line items each, even a 10% unknown item rate means 48 line items per month requiring new product master creation — a significant recurring workload that falls disproportionately on product, operations, and finance teams.


## The Better Model: Partial PO Processing with Exception Routing


The operational solution to the unknown item problem is partial PO processing — the ability to create a Sales Order for all resolvable line items from a customer PO while simultaneously routing only the unresolvable (unknown item) lines to a separate exception workflow.


This decoupling removes the all-or-nothing constraint:


**Known items** proceed immediately through the standard[customer PO to sales order automation](https://peakflo.co/blog/customer-po-to-sales-order-automation-product-code-mismatch) workflow — AI-matched, price-validated, and drafted into the ERP as a Sales Order in minutes.


**Unknown items** are isolated and routed as structured product creation requests to the product or operations team, with all AI-extracted attributes pre-populated from the customer PO document.


**The customer** receives an immediate acknowledgment for the known items and a separate notification that 1–3 items are being set up in the system, with an estimated processing time.


This model matches how experienced operations coordinators already work mentally — they would never hold a 20-item order because one item is new. Partial PO processing simply makes this intelligent split automatic, consistent, and fully auditable.


## AI-Assisted Product Master Creation: Extracting the Right Data


The most time-consuming part of creating a new product master record is gathering the required input data. In traditional workflows, a product manager might need to ask the sales team for the customer’s original description, call the warehouse to ask about storage requirements, and check with finance for the GL code mapping — a multi-hour coordination chain for what is ultimately a data entry task.


AI automation changes this by extracting all available product attributes from the customer PO document at the point of exception detection:


**From the customer PO directly:**


- Customer’s product code (their internal SKU or catalog number)
- Product description (in whatever language the customer uses — including Chinese, Japanese, or Korean for regional manufacturers)
- Unit of measure as listed on the PO
- Unit price (which helps infer the product tier and pricing range)
- Line item quantity
- Any technical specifications embedded in the description field


**Inferred from the AI system:**


- Product category classification (the AI infers from description keywords — e.g., “LFP Battery 100Ah” → category: Energy Storage / Battery)
- Suggested matching to the closest existing product in the ERP catalog (for review — this may be a variant rather than a genuinely new item)
- Historical price context from similar items sourced by the same customer


This pre-population means the product team receives a structured request with 60–70% of the required fields already filled in. Their job is to review, add the internal fields only they can supply (GL code, cost, storage location, vendor), and approve — a 5–15 minute task versus a 30–90 minute manual effort.


Data Field Source Manually Sourced (Traditional) AI-Extracted (Automated)


Customer product code Customer PO Manual copy from PDF Auto-extracted by OCR


Product description Customer PO Manual copy from PDF Auto-extracted, multi-language


Unit of measure Customer PO Manual lookup Auto-extracted


Customer price Customer PO Manual copy Auto-extracted


Product category Team judgment Manual decision AI inference from description


GL code Finance team Email to finance Finance team confirms only


Internal cost price Procurement Email to procurement Procurement team confirms only


Vendor/supplier link Supply chain Email to supply chain Supply chain team confirms only


Internal product code format Operations Manual generation Follows configured naming convention


By shifting the manual sourcing burden from coordination work to confirmation work, AI-assisted product master creation reduces the total time per new item from a multi-day coordination cycle to same-day resolution in most cases.


## Who Reviews New Item Exceptions and What Decisions They Make


The exception routing workflow for unknown items involves multiple teams, each accountable for a specific piece of the approval. A well-designed workflow routes the right information to the right person at the right time, rather than creating a sequential chain of emails and follow-ups.


A typical exception routing sequence for a new unknown item looks like this:


**Stage 1 — Operations / Sales Support (5 minutes)** Receives the alert: “New item on Customer X’s PO — no match found.” Confirms whether the item is genuinely new to the company or potentially a variant of an existing product. If a variant, redirects to product management. If genuinely new, approves routing to next stage.


**Stage 2 — Product Management (5–10 minutes)** Reviews AI-pre-populated description, category inference, and suggested closest existing product. Decides: (a) genuinely new item — creates new product code; (b) variant of existing — maps to existing code with a customer cross-reference; (c) insufficient information — requests additional data from the customer before proceeding.


**Stage 3 — Finance (2–5 minutes)** Assigns GL code, cost center, and tax classification. In most implementations this is a dropdown confirmation from a short list of valid options for the inferred product category — not a free-form data entry exercise.


**Stage 4 — Supply Chain / Procurement (2–5 minutes, if new to procurement)** Associates the new item with an existing vendor, confirms the purchasing UOM, and flags if a new supplier relationship needs to be established.


**Stage 5 — ERP Creation (Automated)** Once all stages are approved, the automation platform submits the product creation request to the ERP via API. The new product code is created, and the held PO line items are automatically matched and re-queued for SO processing.


Total elapsed time: as short as 30–60 minutes for standard items where all teams respond quickly, compared to 24–72 hours in traditional email-based coordination workflows. This aligns with the exception management principles outlined in[manufacturing AP automation exception handling](https://peakflo.co/blog/manufacturing-ap-automation-exception-handling) research, where routing clarity and response time SLAs are the primary levers for reducing exception backlog.


## ERP Integration: Creating Product Codes via API vs. Manual Entry


A critical enabler of fast new item creation is ERP API access. Without API integration, even a workflow-guided exception process ends in manual ERP data entry — the product manager must log into the ERP, navigate to the product master creation screen, and enter each field manually.


With API integration, the approved product master request is submitted programmatically:


The automation platform formats the approved attributes into the ERP’s required API payload, submits the product creation call, receives the new internal product code in the response, and immediately uses that code to process the held SO line items — all within seconds of final approval.


Major ERP platforms that support product master creation via API include:


- **SAP S/4 HANA** : Material Management API (MM01 equivalent via OData)
- **SAP Business One** : Item Master Data API (the same integration path used in[SFTP-based AP automation for Asian manufacturing](https://peakflo.co/blog/sap-business-one-ap-automation-sftp-asian-manufacturing) )
- **NetSuite** : Item record creation via SuiteScript or REST APIs
- **Microsoft Dynamics 365 Business Central** : Item API (v2.0 REST)
- **Globe3 and custom ERPs** : Via REST API or SFTP-based batch file with agreed product master record format


The[Peakflo integrations platform](https://peakflo.co/integrations) supports API-based product master creation across these ERP systems, enabling automated SO processing to resume within minutes of new item approval rather than waiting for the next manual data entry window.


## Building Your Catalog Over Time Through Incoming Customer POs


The most valuable long-term outcome of a well-managed unknown item exception workflow is systematic product catalog enrichment. Every new item exception that is resolved properly adds a permanent match record to the system — so the same item never triggers an exception again for that customer.


This creates a compounding improvement in automation rates that is measurable over time:


Metric Month 1 Month 3 Month 6 Month 12


Unknown item exception rate 20–25% of PO lines 12–15% of PO lines 7–10% of PO lines 3–5% of PO lines


New product codes created High volume Moderate Declining Low (true new launches only)


Average SO creation time (per PO) 45–90 minutes 20–40 minutes 10–20 minutes 5–10 minutes


% of PO lines fully auto-processed ~70% ~82% ~90% ~95%


Staff time per PO (total) 60–120 minutes 30–60 minutes 15–30 minutes 5–15 minutes


As[McKinsey’s operations research](https://www.mckinsey.com/capabilities/operations/our-insights) notes, the compounding benefit of AI systems that learn from operational decisions — rather than requiring periodic manual updates — is one of the key drivers of sustainable operational efficiency in manufacturing.


This dynamic is why the initial investment in a proper unknown item exception workflow pays dividends across the entire duration of a customer relationship, not just in the early months. Each exception that is properly resolved and stored enriches the product mapping database, building an institutional knowledge layer that survives staff turnover and scales without additional headcount.


The same principle applies to[item master synchronization](https://peakflo.co/blog/item-master-synchronization-sap-manufacturing-ap-automation) across ERP systems — the continuous enrichment of product data through operational processes is more sustainable than periodic batch imports.


## Peakflo’s Approach to Unknown Item Exception Handling


Peakflo’s[procure-to-pay automation platform](https://peakflo.co/accounts-payable) handles the full lifecycle of unknown item exceptions in incoming customer purchase orders for[manufacturing companies](https://peakflo.co/industries/manufacturing) :


- **Exception detection at extraction** : When AI OCR processes the customer PO, unknown items are flagged in real time — the system does not wait until SO creation fails to surface the problem
- **Partial SO creation** : Known items are drafted into the ERP immediately, removing the all-or-nothing processing constraint
- **AI-pre-populated creation requests** : All available product attributes from the customer PO are extracted and structured into a product master creation request, reducing manual input for the product team
- **Multi-team exception routing** : The workflow routes each stage of the approval (operations, product, finance, procurement) with context-appropriate information and clear action prompts
- **API-based ERP product creation** : Integration with SAP, NetSuite, Microsoft Dynamics BC, Globe3, and other ERPs via the[Peakflo integrations platform](https://peakflo.co/integrations) enables automated product record creation once approved
- **Permanent match storage** : Every resolved unknown item is stored as a permanent customer-to-SKU mapping, feeding the[AI-powered autonomous PO matching](https://peakflo.co/blog/ai-agents-autonomous-po-matching) engine to prevent future exceptions for the same items
- **[Agentic workflow orchestration](https://peakflo.co/blog/agentic-workflows-erp-integration-sap-oracle-netsuite-dynamics)** : The entire exception resolution process — detection, routing, approval, ERP creation, SO resumption — runs as an orchestrated agentic workflow with no manual step needed to advance between stages


According to[Harvard Business Review’s supply chain management research](https://hbr.org/topic/subject/supply-chain-management) , the companies with the highest customer satisfaction scores in B2B manufacturing are those that maintain order responsiveness even when handling product complexity and data gaps — exactly the scenario that unknown item exception handling addresses.


## Our Verdict: When Should Manufacturers Prioritize Unknown Item Workflows?


Based on an assessment of the operational impact and implementation effort across Asian manufacturing environments, here is our verdict:


### High Priority for Formal Exception Workflows If:


- You are onboarding multiple new customers per year with distinct product catalogs
- Your product range is expanding and customers regularly add new items to their orders
- Your current workflow blocks entire POs when any single item is unknown
- Customer order acknowledgment time exceeds 4 hours due to unknown item delays
- Your product team spends more than 20 hours per month on new product code creation from customer requests


### Lower Urgency If:


- You serve a small, stable customer base where the product catalog overlap is already high
- Your customers submit POs exclusively through a portal that enforces your internal SKU codes
- You process fewer than 20 customer POs per month with minimal product catalog variation


**Our Recommendation** : Any manufacturer or distributor receiving POs from 3+ customers in active growth phases should implement structured unknown item exception handling before it becomes a scaling bottleneck. The initial investment in workflow design and ERP API integration returns value from day one — every exception that is resolved cleanly reduces future processing time and builds a more complete product matching database.


## Conclusion


The unknown SKU problem in customer purchase order processing is not an edge case — it is a structural reality of any manufacturing or distribution business where customers maintain their own product catalogs independent of yours. For companies in active customer acquisition or product range expansion, it can affect 15–25% of incoming PO lines, creating processing delays that cost real revenue and real customer relationships.


The resolution lies in two connected capabilities: partial PO processing that decouples known items from unknown items, and AI-assisted product master creation that reduces the time and coordination burden of setting up new product codes from hours to minutes. Together, they transform the unknown item exception from a process blocker into a managed workflow that resolves itself — and leaves behind a permanently richer product matching database for every future order.


Manufacturers who build these workflows early establish a significant operational advantage as their customer base grows: faster order acknowledgment, cleaner product data, and a matching database that compounds in value with every order processed.


Ready to eliminate unknown item bottlenecks from your customer PO processing?[Request a demo](https://peakflo.co/request-demo) to see how Peakflo handles new product code creation from incoming customer POs for Asian manufacturers.


---


## Frequently Asked Questions


**What is an unknown SKU in a customer purchase order?**


An unknown SKU is a product line item on an incoming customer PO that has no match in your internal ERP product master — the product simply doesn’t exist in your system yet and must be created before a Sales Order can be confirmed.


**How often do customer POs contain completely new items not in the ERP?**


In the first 6 months of a new customer relationship, 15–25% of PO lines may contain unknown items. This drops to 5–10% for established relationships and spikes temporarily during product range expansion cycles.


**Why does one unknown item block the entire PO in traditional workflows?**


ERP systems require all SO line items to reference a valid product master record. A missing record means the line cannot be added to the SO — and most systems cannot create a partial SO, so the entire PO waits until all items are resolved.


**What is partial PO processing?**


Partial PO processing creates a draft Sales Order for all successfully matched items immediately, while routing only the unknown items to a separate exception queue. This removes the all-or-nothing constraint and ensures known items are not delayed by unknown ones.


**How does AI assist with new product master creation?**


AI extracts all product attributes from the customer PO — code, description, UOM, price, inferred category — and pre-populates the product master creation request. The product team only needs to add internal fields (GL code, cost, vendor) and approve, reducing manual effort by 60–70%.


**How long does new item creation take with AI assistance?**


With AI pre-population, the product team can review and approve a new product master in 5–15 minutes. Without it, manual product master creation typically takes 30–90 minutes including coordination time with multiple internal teams.


**Once a new product code is created, does the system remember the match?**


Yes. The customer-to-internal-code mapping is stored permanently. All future orders from the same customer for that item auto-match without exception routing, improving automation rates progressively over the life of the relationship.


**Which ERPs support API-based product master creation?**


SAP S/4 HANA, SAP Business One, NetSuite, Microsoft Dynamics 365 Business Central, and Globe3 all support product master creation via API. This enables automated product code creation from approved exception requests without manual ERP navigation.


**What teams are involved in approving new product codes from customer POs?**


Typically: operations or sales support (confirms genuinely new), product management (creates or maps the code), finance (assigns GL and cost center), and procurement or supply chain (associates vendors). AI exception routing delivers each stage’s request automatically with pre-populated context.


**What is the long-term benefit of structured unknown item exception handling?**


Every resolved unknown item adds a permanent match to the system. Over 12 months, manufacturers typically see unknown item exception rates drop from 20–25% to 3–5%, with corresponding reductions in SO creation time from 60–90 minutes per PO to under 15 minutes.
