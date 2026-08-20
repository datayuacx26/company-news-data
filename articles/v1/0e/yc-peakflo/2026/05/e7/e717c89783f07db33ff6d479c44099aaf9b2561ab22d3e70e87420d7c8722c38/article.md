---
schema_version: "1.0.0"
document_id: "e717c89783f07db33ff6d479c44099aaf9b2561ab22d3e70e87420d7c8722c38"
company_key: "yc-peakflo"
company: "Peakflo"
source_id: "yc-peakflo-news-import-4ba227f4ca0c"
canonical_url: "https://peakflo.co/blog/manufacturing-ap-exception-handling-guide"
published_at: "2026-05-22T00:00:00+00:00"
first_seen_at: "2026-07-22T08:27:10.135588+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:978b10df752723bc6c96e4ebbb1a0d13e78a564d36f174585692d39751be9e94"
---

# Manufacturing AP Exception Handling: Partial Shipments, Substitute Materials & Quality Rejections (2026)

---


## TL;DR


**Manufacturing AP faces unique exception complexity: partial shipments (vendor capacity constraints), substitute materials (discontinued parts, supply shortages), quality rejections (defective goods), and batch variations (measurement tolerances). These exceptions occur in 45-55% of manufacturing invoices vs. 25-35% in service industries.**


**Why Traditional ERP Fails:**


-


- No contextual intelligence: Cannot determine if partial shipment was authorized or vendor error


-


- No manufacturing system integration: Cannot check engineering change orders for approved substitutes


-


- No operational depth: Cannot validate quality rejections against inspection reports


-


- Binary matching logic: Flags 2% batch variation as exception despite industry-standard tolerance


**AI Manufacturing Exception Resolution:**


-


- Integrates with WMS (goods receipts), QMS (quality inspections), ECO database (approved substitutes)


-


- Validates partial shipments against ASN and backorder schedules


-


- Confirms substitute materials approved by engineering before auto-approving price variances


-


- Matches quality rejections to inspection reports and vendor credits


-


- Applies industry-standard batch tolerances (±2-3% for bulk materials)


-


- Auto-resolves 75-82% of manufacturing exceptions vs. 0% with traditional ERP


**Implementation:** 8-10 weeks (manufacturing system integration, AI training on historical exceptions)


**ROI:** 400-585% year 1 from exception investigation time savings + early payment discount capture


---


## Why Manufacturing AP Exceptions Are Different (And Harder)


### Manufacturing vs. Service Industry Exception Rates


**Service Company Exception Profile:**


- Total invoices monthly: 1,000
- Exception rate: 25-30%
- Exception types: Price variances (contract amendments), missing PO numbers, approval routing errors
- Resolution complexity: Moderate (email procurement, verify contract, approve)


**Manufacturing Company Exception Profile:**


- Total invoices monthly: 1,200
- Exception rate: 45-55%
- Exception types: Partial shipments, substitute materials, quality rejections, batch variations, over-delivery, damaged goods, tool/die charges, packaging variances
- Resolution complexity: High (requires warehouse confirmation, engineering validation, quality inspection reports, vendor credits reconciliation)


**The Difference:** Manufacturing exceptions require **operational context** beyond financial data.


Service industry exception:


> “Vendor increased price from $100 to $110. Check contract amendment. Approve if legitimate.”


Manufacturing exception:


> “Vendor shipped Part# ABC-200 instead of ABC-100. Invoice shows 5% price increase. Is ABC-200 an approved substitute? Does engineering change order exist? Does material spec match? Is price increase justified for higher-grade material? Did quality inspection pass? Should PO be updated for future orders?”


**Manufacturing exceptions require cross-functional validation** (engineering, quality, warehouse, procurement) that traditional AP automation cannot orchestrate.


---


## Manufacturing Exception Taxonomy: The Seven Common Scenarios


### Exception Type 1: Partial Shipments Due to Production Capacity


**What Happens:**


PO: 500 units Component XYZ at $45/unit = $22,500


Vendor reality:


- Production line capacity: 300 units/week
- Lead time for 500 units: 2 weeks
- Customer needs material ASAP for production schedule


Vendor solution:


- Week 1: Ship 300 units immediately (partial shipment #1)
- Week 2: Ship remaining 200 units (partial shipment #2)


**Invoicing:**


- Invoice #1: 300 units × $45 = $13,500
- Invoice #2 (1 week later): 200 units × $45 = $9,000


**Three-Way Matching Result:**


Field PO GR (Week 1) Invoice #1 Match?


Quantity 500 300 300 - MISMATCH (invoice 60% of PO)


Amount $22,500 N/A $13,500 - MISMATCH


**Result** - EXCEPTION FLAGGED


**Manual Investigation Required:**


1. AP emails warehouse: “Did we receive 300 units or 500 units?”
2. Warehouse responds (1-2 days): “Received 300 units. Delivery note mentions ‘Partial shipment 1 of 2, backorder 200 units’”
3. AP emails procurement: “Was partial shipment authorized?”
4. Procurement responds (1 day): “Yes, vendor requested split shipment due to capacity. Approved.”
5. AP creates variance documentation and approves $13,500 invoice
6. AP updates PO: 300 units invoiced, 200 units open


**Time: 3-5 days**


**AI Resolution:**


- AI detects 40% quantity variance
- Checks goods receipt: 300 units received -
- Finds Advanced Shipping Notice (ASN) in vendor email: “Partial shipment 1 of 2”
- Confirms PO allows partial shipments
- Auto-approves invoice for 300 units
- Updates PO status: 300 invoiced, 200 backorder
- Sets flag: Expect invoice #2 when backorder ships


**Time: 8 minutes**


### Exception Type 2: Substitute Materials (Approved Alternates)


**What Happens:**


**PO:** 1,000 units Steel Sheet ASTM A36, 4mm thickness, Part# STEEL-A36-4MM at $120/unit


**Vendor Situation:**


- ASTM A36 steel mill experiencing production delays (raw material shortage)
- Vendor has ASTM A572 (higher strength grade) in stock
- A572 costs more ($125/unit) but meets customer’s engineering specs


**Vendor Action:**


- Contacts customer engineering: “Can we substitute A572 for A36? Meets your load requirements.”
- Engineering approves: “Yes, A572 acceptable. Update drawings to reflect material change.”
- Engineering creates ECO (Engineering Change Order) #2026-0095: “Part# STEEL-A36-4MM superseded by STEEL-A572-4MM”


**Invoicing:**


- Vendor ships: 1,000 units ASTM A572, Part# STEEL-A572-4MM
- Vendor invoices: Part# STEEL-A572-4MM, 1,000 units × $125 = $125,000


**Three-Way Matching Result:**


Field PO Invoice Match?


Part Number STEEL-A36-4MM STEEL-A572-4MM - MISMATCH


Unit Price $120 $125 - MISMATCH (4.2% increase)


Total $120,000 $125,000 - MISMATCH


**Result** - EXCEPTION FLAGGED


**Manual Investigation:**


1. AP sees part number mismatch and price variance
2. AP emails procurement: “Vendor shipped wrong part and increased price. Error or authorized?”
3. Procurement emails engineering: “Did you approve A572 substitute?”
4. Engineering responds (2-3 days): “Yes, ECO-2026-0095 approved substitution. Update PO.”
5. Procurement forwards ECO to AP
6. AP reviews ECO, confirms substitution legitimate
7. AP checks if 4.2% price increase reasonable for higher-grade material
8. AP creates variance approval request
9. Approvers review and approve (1-2 days)
10. AP manually releases invoice


**Time: 5-7 days**


**AI Resolution:**


- AI detects part number mismatch + 4.2% price variance
- Searches ECO database for “STEEL-A36-4MM”
- Finds ECO-2026-0095: “Approved substitute: A572 for A36, effective immediately”
- Validates material specs compatible
- Confirms price increase <5% (within tolerance for material upgrade)
- Checks goods receipt: 1,000 units A572 received and accepted by quality
- Auto-approves invoice with documentation: “Material substitution per ECO-2026-0095”
- Notifies procurement: “Update PO part number to A572 for future orders”


**Time: 15 minutes**


### Exception Type 3: Quality Rejections with Vendor Credits


**What Happens:**


**PO:** 5,000 units Machined Component ABC at $25/unit = $125,000


**Receiving Process:**


- Vendor ships 5,000 units
- Warehouse receives shipment, creates goods receipt for 5,000 units
- Quality team performs inspection sampling (per ISO 9001 procedures)
- Inspection finds: 4,650 units pass spec, 350 units fail (dimensional tolerance out of range)


**Vendor Communication:**


- Quality team notifies vendor: “350 units rejected due to dimensional defects”
- Vendor requests return of defective units for rework
- Vendor issues credit memo: 350 units × $25 = $8,750


**Invoicing:**


- Original invoice: 5,000 units × $25 = $125,000
- Vendor credit memo: -$8,750
- Net invoice amount: $116,250 (4,650 accepted units)


**Three-Way Matching Result:**


Field PO GR (Initial) Net Invoice Match?


Quantity 5,000 5,000 4,650 - MISMATCH (7% under)


Amount $125,000 N/A $116,250 - MISMATCH


**Result** - EXCEPTION FLAGGED


**Manual Investigation:**


1. AP sees invoice for 4,650 units but goods receipt shows 5,000 received
2. AP emails warehouse: “Why is invoice 350 units less than goods receipt?”
3. Warehouse responds: “Check with quality team. We received 5,000.”
4. AP emails quality: “Did you reject any units from this shipment?”
5. Quality responds (1-2 days): “Yes, 350 units failed inspection. See report QC-2026-0234.”
6. AP requests quality inspection report
7. AP verifies vendor credit memo received for rejected quantity
8. AP matches: 350 rejected units = 350 unit credit = invoice reduced by 350 units -
9. AP approves net invoice $116,250


**Time: 3-5 days**


**AI Resolution:**


- AI detects 7% quantity variance (invoice 4,650 vs. GR 5,000)
- Searches quality management system for inspection report on this shipment
- Finds QC-2026-0234: “Inspected lot ABC-2026, 350 units failed dimensional tolerance”
- Pulls vendor credit memo: -$8,750 for 350 rejected units
- Validates: 5,000 received - 350 rejected = 4,650 accepted = invoice quantity -
- Confirms credit amount: 350 × $25 = $8,750 -
- Auto-approves net invoice $116,250
- Documents: “Invoice adjusted for quality rejection per QC-2026-0234, vendor credit applied”


**Time: 10 minutes**


### Exception Type 4: Batch Variation Tolerances


**What Happens:**


**PO:** 10,000 liters Industrial Chemical XYZ at $12/liter = $120,000


**Vendor Delivery:**


- Vendor fills tanker truck from storage tank
- Tank measurement accuracy: ±1.5% (industry standard for liquid bulk materials)
- Actual delivered: 10,150 liters (1.5% over PO quantity)
- Vendor invoices actual delivered quantity


**Invoicing:**


- Invoice: 10,150 liters × $12 = $121,800 (1.5% over PO amount)


**Three-Way Matching Result:**


Field PO GR Invoice Match?


Quantity 10,000 L 10,150 L 10,150 L - MISMATCH (1.5% over)


Amount $120,000 N/A $121,800 - MISMATCH


**Result** - EXCEPTION FLAGGED


**Traditional ERP Logic:**


- Exact match required: 10,150 ≠ 10,000 → EXCEPTION
- Even though 1.5% variance is **industry-standard tolerance** for bulk liquids


**Manual Investigation:**


1. AP sees 1.5% over-delivery and price variance
2. AP emails warehouse: “Did we receive 10,150 liters or 10,000 liters?”
3. Warehouse responds: “Tanker delivered 10,150 L per meter reading. Normal variance for bulk liquid.”
4. AP emails procurement: “Is 1.5% over-delivery acceptable?”
5. Procurement responds: “Yes, industry standard ±2% tolerance for bulk chemicals.”
6. AP approves invoice for actual quantity delivered


**Time: 2-3 days**


**AI Resolution:**


- AI detects 1.5% quantity over-delivery
- Identifies material category: Bulk liquid chemical
- Applies industry-standard tolerance: ±2% for bulk liquids
- Calculates actual variance: (10,150 - 10,000) / 10,000 = 1.5% - WITHIN TOLERANCE
- Confirms goods receipt quantity matches invoice: 10,150 L -
- Auto-approves invoice for actual delivered quantity
- Documents: “Batch variation 1.5% within ±2% tolerance for bulk liquid materials”


**Time: 5 minutes**


### Exception Type 5: Packaging Minimum Over-Delivery


**What Happens:**


**PO:** 480 units Component DEF at $15/unit = $7,200


**Vendor Packaging:**


- Component DEF ships in boxes of 100 units
- 480 units would require 4 full boxes (400 units) + partial box (80 units)
- Vendor policy: No partial boxes (shipping/handling efficiency)


**Vendor Action:**


- Ships 5 full boxes = 500 units
- Invoices for actual shipped: 500 units × $15 = $7,500


**Three-Way Matching Result:**


Field PO GR Invoice Match?


Quantity 480 500 500 - MISMATCH (4.2% over)


Amount $7,200 N/A $7,500 - MISMATCH


**Manual Investigation:**


1. AP sees 20 unit over-delivery (4.2%)
2. AP emails warehouse: “Why did vendor ship 500 instead of 480?”
3. Warehouse: “Vendor ships in boxes of 100. Received 5 boxes = 500 units.”
4. AP emails procurement: “Should we pay for 500 or 480?”
5. Procurement: “Packaging minimum is standard for this vendor. Approve 500 units. We can use extras.”
6. AP approves


**Time: 2-3 days**


**AI Resolution:**


- AI detects 4.2% over-delivery (20 units)
- Checks vendor master data: “Packaging = 100 units/box”
- Calculates: 480 units = 4.8 boxes → rounds to 5 boxes = 500 units
- Validates: Over-delivery matches packaging minimum -
- Confirms over-delivery <5% (within typical tolerance) -
- Checks if company accepts over-delivery for this commodity (historical pattern: yes)
- Auto-approves invoice for 500 units
- Documents: “Over-delivery due to vendor packaging minimum (100 units/box)”


**Time: 6 minutes**


---


## How AI Resolves Manufacturing Exceptions: Required Integrations


### Integration 1: Warehouse Management System (WMS)


**Critical Data from WMS:**


- Goods receipt records (actual quantities received)
- Receiving inspection notes (“Partial shipment 1 of 2”, “Damaged units: 15”)
- Delivery documentation (packing slips, bills of lading, delivery notes)
- Warehouse acceptance/rejection status
- Physical inventory confirmation


**AI Use Cases:**


- Match invoice quantity to actual received quantity (not PO quantity)
- Validate partial shipments against delivery notes
- Confirm damaged goods documented before approving vendor credits


### Integration 2: Quality Management System (QMS)


**Critical Data from QMS:**


- Inspection reports (pass/fail results, defect types, quantities)
- Vendor quality ratings (defect rates, on-time delivery, compliance)
- Material certifications required (mill certs, COC, material test reports)
- Quality holds (shipments awaiting inspection, quarantined lots)
- Non-conformance reports (NCR) documenting quality issues


**AI Use Cases:**


- Validate quality rejections before approving reduced invoice quantities
- Verify vendor provided required material certifications
- Check if vendor quality performance warrants stricter invoice scrutiny


### Integration 3: Engineering Change Management (ECM)


**Critical Data from ECM:**


- Engineering Change Orders (ECO) documenting approved modifications
- Approved substitute materials (part A can replace part B)
- Bill of Materials (BOM) alternates
- Specification updates (material grades, dimensions, tolerances)


**AI Use Cases:**


- Validate substitute materials approved by engineering before approving price variances
- Check if part number mismatches are legitimate ECO-driven changes
- Confirm material spec changes authorized


### Integration 4: Production Planning System


**Critical Data:**


- Expected delivery schedules
- Partial shipment authorizations
- Backorder status and expected ship dates
- Production urgency (JIT requirements, line-down situations)


**AI Use Cases:**


- Validate partial shipments against authorized delivery schedules
- Prioritize invoice processing for critical materials needed for production
- Set expectations for backorder invoice arrival


---


## Real-World Manufacturing Exception Resolution: Before and After AI


### Company Profile


- Industry: Automotive component manufacturing
- Monthly invoice volume: 1,200 invoices
- Exception rate: 52% (625 exceptions monthly)
- AP team size: 3 FTE
- Exception types: 35% partial shipments, 25% substitute materials, 18% quality rejections, 12% batch variations, 10% other


### Before AI (Manual Exception Resolution)


**Time Allocation:**


- Exception investigation: 32 hours/week across team
- Email follow-up (warehouse, quality, engineering, procurement): 18 hours/week
- Variance approval documentation: 8 hours/week
- Total exception effort: **58 hours/week = 1.45 FTE dedicated to exceptions**


**Financial Impact:**


- Late payment due to exception delays: 18-22 days average
- Lost early payment discounts: $195,000 annually (missed 2% 10 net 30 on $10M material spend)
- Vendor escalation calls: 25 monthly
- Payment terms at risk: 3 key suppliers threatened payment-on-receipt due to chronic delays


**Operational Pain Points:**


- Production delays: 2 instances where vendor delayed shipment due to unpaid invoices
- AP team burnout: High turnover, difficulty filling positions
- Month-end close: 3 extra days due to exception backlog accruals


### After AI Implementation (8 Months Post-Deployment)


**Automation Results:**


Exception Type Monthly Volume AI Auto-Resolution Rate Manual Review Required


Partial shipments (with ASN) 220 92% (202 invoices) 18 invoices


Substitute materials (approved ECO) 155 88% (136 invoices) 19 invoices


Quality rejections (with QC report) 115 94% (108 invoices) 7 invoices


Batch variations (within tolerance) 75 97% (73 invoices) 2 invoices


Packaging minimums 35 91% (32 invoices) 3 invoices


Other exceptions 25 65% (16 invoices) 9 invoices


**TOTAL** **625** **90% (567 invoices)** **58 invoices**


**Time Savings:**


- Exception investigation: 32 hours/week → 6 hours/week (81% reduction)
- Email follow-up: 18 hours/week → 2 hours/week (89% reduction)
- Manual variance documentation: 8 hours/week → 1 hour/week (88% reduction)
- Total exception effort: **9 hours/week (vs. 58 hours) = 49 hours/week saved**


**Financial Impact:**


- Early payment discount capture: $195K lost → $28K lost = **$167K annual recovery**
- Late fees eliminated: **$8,500 annual savings**
- Vendor escalation calls: 25/month → 4/month
- Payment terms: Restored preferred supplier status with all vendors


**Operational Improvements:**


- Production continuity: Zero instances of shipment delays due to payment issues
- AP team morale: Reduced burnout, staff focusing on vendor relationships and process improvement
- Month-end close: Reduced by 2 days (exception backlog eliminated)


**ROI Calculation:**


Annual Benefits:


- Labor savings: 49 hours/week × 50 weeks × $35/hour = $85,750
- Early payment discount recovery: $167,000
- Late fee elimination: $8,500
- **Total annual benefit: $261,250**


Implementation Cost:


- Year 1: $45,000 (software + implementation + training)
- Year 2+: $28,000 (annual licensing)


**Year 1 ROI: 481%** | **Payback: 2.1 months**


---


## Our Verdict: Manufacturing Requires AI-Powered Exception Handling


### Key Insights


**1. Manufacturing Exception Rates Are 50-80% Higher**


Service companies: 25-35% exception rate Manufacturing companies: 45-55% exception rate


**Reason:** Operational complexity (partial shipments, substitutions, quality issues) unavoidable in manufacturing supply chains.


**2. Traditional ERP Cannot Handle Operational Exceptions**


ERP three-way matching is binary: exact match or exception.


Manufacturing reality: Legitimate variances (batch tolerances, packaging minimums, quality rejections) flagged as exceptions despite being operationally valid.


**3. AI Requires Manufacturing System Integration**


Generic AP automation won’t suffice. AI must connect to:


- WMS (goods receipt context)
- QMS (quality inspection results)
- ECM (engineering approvals for substitutes)
- Production planning (partial shipment authorizations)


**4. Exception Resolution Speed Directly Impacts Vendor Relationships**


Manufacturing depends on reliable supplier partnerships. Chronic late payments due to exception delays damage:


- Payment terms (vendors demand shorter payment windows)
- Supply priority (vendors allocate scarce materials to faster-paying customers first)
- Pricing leverage (vendors less willing to negotiate discounts with chronically late payers)


**AI exception resolution preserves supplier relationships** by enabling on-time payment despite high exception rates.


---


## How Peakflo Handles Manufacturing AP Exceptions


[Peakflo](https://peakflo.co/) ’s agentic AI platform includes manufacturing-specific exception resolution agents that integrate with WMS, QMS, and ECM systems to autonomously validate operational variances.


### Peakflo Manufacturing Capabilities


**Manufacturing System Integrations:**


- WMS: SAP EWM, Oracle WMS, Manhattan, Blue Yonder, NetSuite WMS
- QMS: ETQ Reliance, Sparta Systems TrackWise, MasterControl, Qualio
- ECM: Windchill, Teamcenter, Arena PLM, Propel


**Exception Resolution Agents:**


- Partial shipment agent: Validates ASN, backorder schedules, goods receipts
- Substitute material agent: Checks ECO database, validates material equivalency, confirms pricing reasonableness
- Quality rejection agent: Matches inspection reports to vendor credits, validates rejected quantities
- Batch variation agent: Applies industry-standard tolerances by material category
- Multi-line invoice agent: Processes mixed exceptions, enables partial approval


**Confidence-Based Processing:**


- High confidence (88%+): Auto-approve with manufacturing documentation
- Medium confidence (75-87%): Auto-approve with AP notification
- Low confidence (<75%): Escalate with operational analysis and recommendation


**Auto-Resolution Rates:**


- Partial shipments: 90-94%
- Substitute materials: 85-90%
- Quality rejections: 92-96%
- Batch variations: 96-98%
- Overall manufacturing exceptions: 75-85%


### Peakflo Implementation for Manufacturing


**Week 1-2:** Manufacturing system discovery (WMS, QMS, ECM), exception type analysis **Week 3-4:** System integrations (ERP + manufacturing systems via APIs) **Week 5-6:** AI training on historical exceptions (ECO patterns, quality rejection workflows, vendor behavior) **Week 7-8:** Pilot testing with live manufacturing invoices **Week 9-10:** Production rollout, continuous optimization


**Time to value: 8-10 weeks** from kickoff to 75%+ exception auto-resolution


### Related Peakflo Resources


- [Three-Way Matching Exception Resolution Guide](https://peakflo.co/blog/three-way-matching-exceptions-ai-solutions)
- [AI Exception Management for Accounts Payable](https://peakflo.co/blog/ai-agents-exception-management-accounts-payable-guide)
- [Agentic Workflows vs Traditional AP Automation](https://peakflo.co/blog/agentic-workflows-vs-traditional-ap-automation)
- [AI GL Coding Automation](https://peakflo.co/blog/ai-gl-coding-automation-non-po-invoices)
- [Accounts Payable Automation ROI](https://peakflo.co/blog/accounts-payable-automation-roi-analysis)


---


## Frequently Asked Questions


**What are manufacturing-specific AP exceptions?**


Manufacturing AP exceptions are invoice variances unique to production environments: (1) Partial shipments - vendor ships 300 of 500 ordered units due to production capacity, backorders remaining units, (2) Substitute materials - vendor ships alternate part (higher grade steel, different batch, upgraded component) when original unavailable, (3) Quality rejections - goods received but fail quality inspection, vendor issues credit for defective units, (4) Batch variations - ingredient quantities vary by batch (±2-3% for liquids, powders), creating quantity variances, (5) Scrap allowances - vendor ships 5% extra units accounting for expected manufacturing scrap, (6) Over-delivery for packaging minimums - vendor ships 520 units instead of 500 to fill complete pallet. These scenarios occur in 45-55% of manufacturing invoices vs. 25-35% in service industries.


**Why do partial shipments happen in manufacturing?**


Partial shipments occur due to five manufacturing realities: (1) Production capacity constraints - vendor’s line can only produce 300 units/week, splits 500-unit order across two deliveries, (2) Material shortages - vendor receives insufficient raw materials, ships what’s available immediately, backorders remainder, (3) Quality holds - 200 of 500 units fail vendor’s internal QC, vendor ships 300 passing units, reworks failed batch, (4) Logistics optimization - vendor ships partial quantity to meet customer production deadline rather than delaying full shipment, (5) Long-lead items - standard components ship immediately, custom components with 8-week lead ship separately. For manufacturers with JIT (just-in-time) production, partial shipments create invoicing complexity: invoice for 300 units at $45 = $13,500 doesn’t match PO for 500 units at $45 = $22,500, requiring exception investigation.


**How do substitute materials create AP exceptions?**


Material substitutions create three-way matching failures: (1) Part number mismatch - PO specifies “Steel Grade A36”, vendor ships “Steel Grade A572” (higher strength), invoice shows different part number, (2) Price variance - substitute material costs more ($125/unit vs. $120 PO price) but vendor absorbs difference or charges premium, (3) Unit of measure changes - PO for “sheets”, vendor ships “coils” (different UOM), invoice quantity appears wrong. Real scenario: Electronics manufacturer orders capacitor Part# CAP-1000. Vendor discontinues CAP-1000, ships CAP-1200 (equivalent spec, different manufacturer). Invoice shows CAP-1200 at $0.85 vs. PO CAP-1000 at $0.80. AP clerk must verify: (1) Is CAP-1200 approved substitute? (Check engineering change order), (2) Is price variance acceptable? (5% increase justified?), (3) Should PO be updated for future orders? Manual investigation: 45-60 minutes per invoice.


---


**Ready to automate manufacturing AP exception handling?**[Schedule a demo with Peakflo](https://peakflo.co/request-demo) to see how AI resolves partial shipments, substitute materials, and quality rejections autonomously.
