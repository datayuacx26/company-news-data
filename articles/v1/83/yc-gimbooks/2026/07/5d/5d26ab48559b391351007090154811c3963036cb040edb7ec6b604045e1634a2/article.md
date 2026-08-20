---
schema_version: "1.0.0"
document_id: "5d26ab48559b391351007090154811c3963036cb040edb7ec6b604045e1634a2"
company_key: "yc-gimbooks"
company: "GimBooks"
source_id: "yc-gimbooks-rss-a270650329c9"
canonical_url: "https://www.gimbooks.com/blog/e-way-bill-branch-transfer-between-gstins/"
published_at: "2026-07-13T08:01:31+00:00"
first_seen_at: "2026-07-20T23:23:58.428442+00:00"
fetched_at: "2026-08-11T06:22:24.738422+00:00"
content_hash: "sha256:3849c519768419bc24435352a53b7be836fb2497695675163eb097d6a87741f4"
---

# E-Way Bill for Branch Transfers Between GSTINs

Branch transfers are common for businesses that operate from multiple locations, warehouses, depots, stores or state-wise GST registrations. Goods may move from a head office to a branch, from one warehouse to another, or from one GSTIN to another GSTIN under the same PAN.


From a GST and e-way bill perspective, the most important question is:


**Are the goods moving between separate GSTINs or within the same GSTIN?**


If goods are transferred between two different GST registrations, the branches are treated as distinct persons under GST. In many cases, the movement is treated as a taxable supply even if no payment is made between branches. This means the business must prepare the correct GST document and generate an e-way bill where applicable.


This guide explains how to handle an e way bill branch transfer, what document to use, which GSTIN to enter, how to avoid common data-entry errors and how businesses can keep invoice, inventory and e-way bill records aligned.


## **Quick Answer**


For a branch transfer between two separate GSTINs under the same PAN, create a tax invoice where applicable, enter the sending branch as the supplier and receiving branch as the recipient, charge the correct GST based on the place of supply, and generate an e-way bill if the consignment value exceeds the applicable threshold.


For movement between locations under the same GSTIN, a delivery challan may be used where there is no taxable supply, and an e-way bill may still be required if the goods cross the applicable value threshold.


## **Key Takeaways**


- Branch transfers between separate GSTINs are generally treated as supplies between distinct persons.
- A tax invoice is usually required when goods move between separate GST registrations as a taxable branch transfer.
- A delivery challan is used when goods are moved without issuing a tax invoice, such as movement within the same GSTIN or other non-supply movements.
- An e-way bill is generally required when the consignment value exceeds ₹50,000, subject to applicable rules and state-specific requirements.
- In an e-way bill between GSTINs, the sending GSTIN should be entered as the supplier and the receiving GSTIN as the recipient.
- The document number, date, value, GSTINs, HSN, item quantity and vehicle details should match the invoice or delivery challan.
- For interstate branch transfers between GSTINs, IGST is generally charged.
- For same-state branch transfers between separate GSTINs, CGST and SGST may apply depending on the registration structure and transaction.
- The receiving branch may be able to claim Input Tax Credit subject to normal ITC conditions.
- E-way bill generation should be completed before movement starts.
- From 2025, e-way bill generation is restricted for documents older than 180 days, so branch transfer documents should not be left pending.
- Businesses should reconcile stock transfer invoices, e-way bills and inventory movement records after dispatch.


## **What Is a Branch Transfer Under GST?**


A branch transfer is the movement of goods from one business location to another business location of the same legal entity.


Examples include:


- head office to branch;
- factory to warehouse;
- warehouse to retail outlet;
- one state GSTIN to another state GSTIN;
- one registered branch to another registered branch;
- central depot to regional depot;
- branch to exhibition location;
- branch to job-work location;
- warehouse to another warehouse.


Not every branch movement has the same GST treatment. The GST document depends on whether the movement is between:


1. two different GSTINs; or
2. two locations under the same GSTIN.


## **Branch Transfer Between GSTINs vs Same GSTIN Movement**


Scenario


GST treatment


Common document


E-way bill relevance


Branch A and Branch B have different GSTINs


Treated as movement between distinct persons


Tax invoice, where taxable supply applies


Required if value crosses threshold


Warehouse to store under the same GSTIN


Movement within the same registration


Delivery challan


Required if value crosses threshold


Goods sent for job work


Movement for job work


Delivery challan


E-way bill may be required


Goods sent for exhibition or fair


Movement without sale


Delivery challan


E-way bill may be required


Goods transferred to another GSTIN for resale


Taxable stock transfer between registrations


Tax invoice


Required if value crosses threshold


Goods moved to transporter before further movement


Movement to transport hub


Invoice or delivery challan, depending on case


Part-B exemption may apply for short prescribed distance


The key point is simple: **a branch transfer between GSTINs is different from a warehouse movement within the same GSTIN.**


## **Why Separate GSTINs Matter**


Under GST, establishments of the same business having different registrations can be treated as distinct persons. When goods are moved between such registrations in the course of business, the transaction may be treated as supply even if no money is exchanged.


This affects:


- invoice creation;
- GST tax treatment;
- e-way bill data;
- stock transfer accounting;
- GSTR-1 reporting;
- recipient branch ITC;
- inventory reconciliation.


### **Example**


A business has:


- Maharashtra GSTIN;
- Karnataka GSTIN;
- Delhi GSTIN.


If goods move from the Maharashtra GSTIN to the Karnataka GSTIN, the receiving branch is not treated like the same GST registration. The transfer should be documented properly as a transaction between two GSTINs.


## **Is an E-Way Bill Required for Branch Transfer?**


Yes, an e-way bill may be required for branch transfers where the consignment value exceeds the applicable threshold.


In most cases, an e-way bill is required when goods worth more than ₹50,000 are moved. However, state-specific rules and exemptions may apply, especially for intrastate movement and specified goods.


Businesses should check:


- value of goods;
- nature of goods;
- interstate or intrastate movement;
- whether goods are exempt from e-way bill requirements;
- whether movement is by road, rail, air or vessel;
- applicable state notification for intrastate movement;
- whether the movement is supply, job work, own use or another reason.


Even if the movement is not a sale, an e-way bill can still be required because e-way bills apply to movement of goods, not only sales.


## **Which Document Should Be Used for Branch Transfer?**


The document depends on the nature of the transfer.


## **1. Branch Transfer Between Separate GSTINs**


Use a tax invoice where the transfer is treated as a taxable supply between distinct persons.


This is common when:


- goods move from one state GSTIN to another state GSTIN;
- one registered branch transfers stock to another registered branch;
- goods are transferred for resale or further supply;
- the recipient branch will take credit and sell the goods later.


### **Example**


Field


Entry


Supplier


Maharashtra branch GSTIN


Recipient


Karnataka branch GSTIN


Document


Tax invoice


Tax type


IGST, where interstate


E-way bill


Required if consignment value crosses threshold


## **2. Movement Within the Same GSTIN**


Use a delivery challan where goods move between locations under the same GSTIN and there is no taxable supply.


This is common when:


- goods move from one warehouse to another within the same GST registration;
- goods move from branch to storage location under the same GSTIN;
- goods are sent for internal use;
- goods are sent for exhibition or display;
- goods are moved without sale or transfer to a different registration.


### **Example**


Field


Entry


From


Main warehouse under same GSTIN


To


Retail store under same GSTIN


Document


Delivery challan


Tax type


No GST charged in the document


E-way bill


Required if applicable threshold is crossed


## **3. Goods Sent for Job Work**


Goods sent to a job worker are generally moved under a delivery challan.


The e-way bill should be generated where applicable, and the reason for transportation should reflect job work.


## **4. Goods Sent in Batches or Lots**


Where goods are transported in batches or lots, the documentation must be handled carefully. In such cases, the invoice and delivery challan references should remain consistent across consignments.


## **E-Way Bill Data Entry for Branch Transfer Between GSTINs**


When generating an e-way bill for branch transfer between separate GSTINs, use the correct branch details.


E-way bill field


What to enter


Transaction type


Outward, where goods are moving from sending GSTIN


Sub-type


Supply or appropriate movement reason


Document type


Tax invoice, where taxable branch transfer applies


From GSTIN


Sending branch GSTIN


To GSTIN


Receiving branch GSTIN


Document number


Tax invoice number or transfer invoice number


Document date


Invoice date


Place of delivery


Receiving branch location


HSN code


HSN of goods transferred


Value


Transfer value including applicable tax for consignment value


Transport mode


Road, rail, air or ship


Vehicle number / transporter details


As applicable


Distance


Approximate distance between dispatch and delivery locations


For movement within the same GSTIN using a delivery challan, choose the appropriate reason such as own use, others, job work, exhibition or line sales depending on the actual movement.


## **Step-by-Step Process to Generate E-Way Bill for Branch Transfer**


## **Step 1: Identify the Type of Branch Movement**


Before creating any document, confirm whether the movement is:


- between two different GSTINs;
- between two places under the same GSTIN;
- to a job worker;
- to an exhibition;
- to a transporter;
- to a temporary site;
- for sale, resale, internal use or return.


This step decides whether a tax invoice or delivery challan is required.


## **Step 2: Confirm the GSTINs**


For transfer between GSTINs, verify:


- sending branch GSTIN;
- receiving branch GSTIN;
- state code of both GSTINs;
- active registration status;
- registered business address;
- delivery address;
- whether the receiving location is the correct branch.


Do not use the same GSTIN in both supplier and recipient fields when the movement is actually between two separate GSTINs.


## **Step 3: Create the Correct Document**


For taxable branch transfer between separate GSTINs, create a tax invoice.


The invoice should include:


- supplier branch name and GSTIN;
- recipient branch name and GSTIN;
- invoice number and date;
- item description;
- HSN code;
- quantity;
- taxable value;
- GST rate;
- IGST or CGST/SGST;
- place of supply;
- total invoice value.


For movement under delivery challan, include:


- challan number and date;
- consignor details;
- consignee details;
- HSN code;
- description of goods;
- quantity;
- taxable value;
- tax rate and tax amount, where applicable;
- place of supply for interstate movement;
- signature.


Read the[GST invoice mandatory fields checklist](https://www.gimbooks.com/blog/gst-invoice-mandatory-fields-rule-46-checklist/) for field-level invoice review.


## **Step 4: Check the Consignment Value**


Calculate consignment value based on the invoice or delivery challan.


The value should include applicable taxes shown in the document and should exclude exempt supply value where the same document contains both exempt and taxable goods.


If the value exceeds the applicable threshold, generate the e-way bill before movement.


## **Step 5: Enter Part-A Details**


Enter:


- GSTIN of recipient branch;
- place of delivery;
- document number;
- document date;
- value of goods;
- HSN code;
- reason for transportation;
- document type.


Make sure document details exactly match the tax invoice or delivery challan.


## **Step 6: Enter Part-B Details**


Enter transport details such as:


- transport mode;
- vehicle number;
- transporter ID;
- transporter document number;
- dispatch date;
- approximate distance.


For road transport, the e-way bill is not valid for movement unless Part-B vehicle details are furnished, except in limited cases where rules provide relaxation.


## **Step 7: Generate and Share the E-Way Bill**


After reviewing all details, generate the e-way bill.


Share the e-way bill number with:


- driver;
- transporter;
- dispatch team;
- receiving branch;
- accounts team.


The person in charge of the vehicle should carry the invoice, bill of supply or delivery challan, along with the e-way bill number.


## **Step 8: Reconcile After Dispatch**


After goods are dispatched and received, reconcile:


- transfer invoice or challan;
- e-way bill number;
- goods received note;
- inventory issue and receipt;
- GSTR-1, where applicable;
- recipient branch ITC;
- branch stock reports.


This is important because branch transfers affect both compliance and inventory records.


## **Practical Example 1: Branch Transfer Between State GSTINs**


A company transfers goods from its Maharashtra GSTIN to its Karnataka GSTIN.


Detail


Entry


Sending branch


Maharashtra GSTIN


Receiving branch


Karnataka GSTIN


Movement


Interstate branch transfer


Document


Tax invoice


Tax type


IGST


E-way bill


Required if value exceeds threshold


Recipient ITC


Karnataka branch may claim ITC subject to conditions


### **Correct workflow**


1. Create branch transfer invoice from Maharashtra GSTIN to Karnataka GSTIN.
2. Apply IGST based on interstate supply.
3. Generate e-way bill using the Maharashtra GSTIN as supplier.
4. Enter Karnataka GSTIN as recipient.
5. Add vehicle or transporter details.
6. Dispatch goods.
7. Receiving branch records stock receipt and ITC.


## **Practical Example 2: Stock Movement Within Same GSTIN**


A business moves goods from its Chennai warehouse to its Chennai retail outlet under the same GSTIN.


Detail


Entry


Sending location


Chennai warehouse


Receiving location


Chennai outlet


GSTIN


Same GSTIN


Movement type


Internal stock movement


Document


Delivery challan


Tax


No tax invoice if not a taxable supply


E-way bill


Required if value exceeds applicable threshold


### **Correct workflow**


1. Create delivery challan.
2. Select proper reason for movement.
3. Generate e-way bill if required.
4. Enter vehicle details.
5. Move goods.
6. Update inventory at both locations.


## **Practical Example 3: Branch Transfer With Multiple Consignments**


A central warehouse sends goods to three branches in one truck.


Each branch transfer should have its own tax invoice or delivery challan depending on the GSTIN and transaction type.


If multiple consignments are carried in one vehicle, the transporter can generate a consolidated e-way bill after individual e-way bills are created.


Read:[How to Generate a Consolidated E-Way Bill for Multiple Consignments](https://www.gimbooks.com/blog/how-to-generate-consolidated-e-way-bill/)


## **Branch Transfer E-Way Bill Checklist**


### **Before Document Creation**


- Identify whether the transfer is between separate GSTINs or same GSTIN locations.
- Confirm sending branch GSTIN.
- Confirm receiving branch GSTIN.
- Check whether the movement is taxable supply, own use, job work, exhibition or other movement.
- Confirm whether tax invoice or delivery challan is required.
- Check HSN code and item classification.
- Confirm quantity and stock availability.
- Verify place of supply.
- Confirm transfer value.


### **Before E-Way Bill Generation**


- Document number is final.
- Document date is correct.
- Consignment value is checked.
- Recipient GSTIN is active and correct.
- Delivery address is complete.
- HSN and item value match the document.
- Vehicle or transporter details are available.
- Transport distance is reviewed.
- Correct reason for transportation is selected.
- E-way bill is generated before movement.


### **During Movement**


- Driver has invoice or delivery challan.
- Driver has e-way bill number.
- Vehicle number matches e-way bill.
- Transporter details are correct.
- Goods match the document.
- Delivery route and destination are clear.


### **After Receipt**


- Receiving branch confirms goods received.
- Inventory is updated.
- E-way bill is mapped to transfer document.
- ITC is reviewed where tax invoice was issued.
- Branch-wise stock report is reconciled.
- Exceptions are recorded.


## **Common Mistakes in E-Way Bill Branch Transfer**


### **1. Using Delivery Challan for Transfer Between Separate GSTINs Without Review**


A transfer between separate GSTINs may be a taxable supply. Using only a delivery challan can create compliance risk.


### **2. Entering Same GSTIN in Supplier and Recipient Fields**


For movement between separate GSTINs, enter the actual receiving branch GSTIN in the recipient field.


### **3. Selecting Wrong Transaction Sub-Type**


Selecting “Own Use” for a taxable branch transfer between GSTINs can create mismatch risk.


### **4. Not Charging GST on Transfer Between Distinct Persons**


Where the transaction is treated as supply between distinct persons, GST should be applied according to the law.


### **5. Wrong Place of Supply**


Place of supply affects whether IGST or CGST/SGST is charged. Do not copy the warehouse state blindly.


### **6. E-Way Bill Generated With Wrong Document Type**


If the movement is backed by a tax invoice, use tax invoice as the document type. If it is a delivery challan movement, use delivery challan.


### **7. Inventory Updated in One Branch Only**


Branch transfers must update both outward stock from the sending branch and inward stock at the receiving branch.


### **8. E-Way Bill Not Reconciled With GSTR-1**


Where a tax invoice is issued for branch transfer, the transaction should be checked against GST return records.


### **9. Not Updating Vehicle Details**


If goods are moved by road, Part-B vehicle details are usually needed before movement.


### **10. Waiting Too Long to Generate the E-Way Bill**


The e-way bill system restricts generation against old documents. Branch transfer documents should be processed before dispatch, not left pending.


## **Branch Transfer: Tax Invoice or Delivery Challan?**


Movement type


Usually use tax invoice?


Usually use delivery challan?


Notes


Transfer between separate GSTINs for business stock


Yes


No, unless specific exception applies


Treated as supply between distinct persons


Transfer between warehouses under same GSTIN


No


Yes


Movement without separate taxable supply


Goods sent for job work


No


Yes


Job-work documentation applies


Goods sent for exhibition


No


Yes


Use appropriate movement reason


Goods sent in batches or lots


Depends on supply structure


Yes for subsequent consignments where applicable


Invoice and challan references must be linked


Goods returned from branch to another GSTIN


Depends on transaction


Depends on nature of movement


Review tax and stock treatment


Businesses should confirm transaction treatment with their GST advisor for unusual cases such as capital goods transfer, discontinued stock, stock written off, damaged goods, job-work returns or movement after GSTIN cancellation.


## **How to Handle Stock Transfer Value**


For branch transfers between GSTINs, transfer value should not be casually entered as zero.


The value affects:


- taxable value;
- GST amount;
- recipient ITC;
- consignment value for e-way bill;
- stock valuation;
- branch profitability;
- return reporting.


The valuation should follow applicable GST valuation rules. Where the receiving branch is eligible for full ITC, businesses may have certain valuation flexibility, but this should be reviewed with the accountant or GST advisor.


## **E-Way Bill Validity for Branch Transfers**


E-way bill validity depends on the distance goods need to travel.


For regular cargo, validity is generally calculated based on 200 km blocks. For over-dimensional cargo and certain multimodal movements, a different distance basis applies.


Dispatch teams should check:


- distance between sending and receiving branch;
- type of vehicle;
- route delays;
- trans-shipment points;
- vehicle breakdown risk;
- delivery timeline.


If a branch transfer is delayed due to vehicle breakdown or logistics disruption, read:[E-Way Bill Validity Extension for Vehicle Breakdown and Delivery Delays](https://www.gimbooks.com/blog/e-way-bill-validity-extension-vehicle-breakdown-delays/)


## **How GimBooks Helps With Branch Transfer and E-Way Bill Records**


Multi-location businesses need billing and inventory records that stay aligned across GSTINs, branches and warehouses.


GimBooks helps businesses manage:


- GST invoice creation;
- e-way bill generation;
- delivery challans;
- customer and supplier records;
- item and HSN details;
- inventory records;
- purchase and sales documentation;
- branch-wise billing workflows;
- invoice and document reports;
- mobile and web access.


A practical workflow is:


**Create branch transfer invoice or challan → generate e-way bill → move goods → update receiving branch stock → reconcile reports**


Explore[GimBooks e-way bill software](https://www.gimbooks.com/e-waybills/) for invoice-linked e-way bill workflows.


Also read:


- [GST billing software](https://www.gimbooks.com/)
- [How Billing Software Automatically Generates E-Way Bills](https://www.gimbooks.com/blog/how-billing-software-automatically-generates-e-way-bills/)
- [GST Invoice Mandatory Fields Checklist](https://www.gimbooks.com/blog/gst-invoice-mandatory-fields-rule-46-checklist/)
- [How to Generate a Consolidated E-Way Bill](https://www.gimbooks.com/blog/how-to-generate-consolidated-e-way-bill/)
- [E-Way Bill Validity Extension Guide](https://www.gimbooks.com/blog/e-way-bill-validity-extension-vehicle-breakdown-delays/)
- [GimBooks distributor billing software](https://www.gimbooks.com/distributor-billing-software)


## **Direct Answers**


### **Is e-way bill required for branch transfer?**


Yes, an e-way bill is generally required for branch transfer if the consignment value exceeds the applicable threshold and the goods are not exempt from e-way bill requirements.


### **Is GST applicable on branch transfer between GSTINs?**


Branch transfers between separate GSTINs are generally treated as supplies between distinct persons when made in the course or furtherance of business. GST may apply even if there is no payment between branches.


### **Which document is used for branch transfer under GST?**


A tax invoice is generally used for taxable branch transfers between separate GSTINs. A delivery challan is used for movement without invoice, such as movement within the same GSTIN or other non-supply movements.


### **What should be entered in To GSTIN for branch transfer?**


For transfer between GSTINs, enter the receiving branch GSTIN. For movement within the same GSTIN, use the correct movement reason and delivery details according to the e-way bill portal rules.


### **Can stock transfer be done without invoice?**


Stock movement within the same GSTIN may be done using a delivery challan where no tax invoice is required. But stock transfer between separate GSTINs should be reviewed as a supply between distinct persons and may require a tax invoice.


## **Frequently Asked Questions**


### **What is e way bill branch transfer?**


E way bill branch transfer refers to generating an e-way bill when goods are moved from one branch, warehouse or GSTIN of a business to another branch, warehouse or GSTIN.


### **Is e-way bill required for stock transfer between branches?**


Yes, an e-way bill may be required for stock transfer between branches when the value crosses the applicable threshold and the goods are not exempt from e-way bill rules.


### **Is branch transfer treated as supply under GST?**


A branch transfer between separate GSTINs can be treated as supply between distinct persons when made in the course or furtherance of business, even without consideration.


### **Should I use tax invoice or delivery challan for branch transfer?**


Use a tax invoice where the transfer is a taxable supply between separate GSTINs. Use a delivery challan where goods are moved without invoice, such as movement within the same GSTIN, job work or own-use movement.


### **What GST should be charged on interstate branch transfer?**


For interstate branch transfer between separate GSTINs, IGST generally applies.


### **Can Input Tax Credit be claimed by the receiving branch?**


The receiving GSTIN may claim ITC subject to normal eligibility conditions, possession of tax invoice, receipt of goods and proper GST reporting.


### **What value should be entered in branch transfer invoice?**


The value should be determined according to GST valuation rules and should not be left blank. Businesses should maintain a consistent stock-transfer valuation policy.


### **Can the same GSTIN be used as From and To in e-way bill?**


For movement within the same GSTIN, the same GSTIN may appear depending on the transaction type and portal fields. For transfer between separate GSTINs, enter the correct receiving branch GSTIN.


### **What documents should the vehicle carry?**


The vehicle in-charge should carry the tax invoice, bill of supply or delivery challan, as applicable, along with the e-way bill number.


### **Can branch transfers be consolidated in one vehicle?**


Yes. If multiple consignments have separate e-way bills and move in one vehicle, the transporter can generate a consolidated e-way bill.


## **Conclusion**


Branch transfers under GST need careful documentation because the same physical movement can have different compliance treatment depending on whether goods move between separate GSTINs or within the same GSTIN.


For branch transfers between GSTINs, businesses should usually treat the movement as a supply between distinct persons, issue the correct tax invoice, charge applicable GST and generate an e-way bill when required.


For movement within the same GSTIN, a delivery challan may be appropriate, but e-way bill requirements can still apply.


The safest process is to:


1. identify the sending and receiving GSTINs;
2. decide whether the movement is taxable supply or internal movement;
3. create the correct invoice or delivery challan;
4. enter accurate e-way bill details;
5. generate Part-A and Part-B before movement;
6. reconcile stock and GST records after receipt.


Using[GimBooks e-way bill software](https://www.gimbooks.com/e-waybills/) can help businesses manage GST invoices, delivery challans, e-way bills and inventory movement in a more organised workflow.


**Create accurate branch transfer documents and manage e-way bills more efficiently with GimBooks.**
