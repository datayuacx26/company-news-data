---
schema_version: "1.0.0"
document_id: "8e680fecf2f2852607a8ee4989db16b5a369f9645ad3828f7eb348ecfbbbbbdc"
company_key: "yc-swipe-2"
company: "Swipe"
source_id: "yc-swipe-2-news-import-e93617422a04"
canonical_url: "https://getswipe.in/blog/article/e-invoice-for-dispatch-from-transactions"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-07-31T22:20:59.643+00:00"
fetched_at: "2026-07-31T22:21:00.628476+00:00"
content_hash: "sha256:ecc01b86976aa27ee00cd3e488363ff2a2ef281e8f407282768a19bfa3152d90"
---

# E-invoice for dispatch from transactions: Supplier, buyer and shipping fields

# E-invoice for dispatch from transactions: Supplier, buyer and shipping fields


> E-Invoice for Dispatch from Transactions is a crucial requirement for businesses under the GST regime, especially those with intricate supply chains. A business can send out goods from a warehouse, a third-party logistics provider or from a branch address that is different from the business's registered address.


In such cases, it becomes crucial to fill out the supplier, buyer, and shipping details correctly in the e-invoice to prevent GST mistakes, e-Way Bill mismatches, and avoid undue compliance complications. The knowledge of these areas can help companies prepare legitimate invoices and facilitate the flow of goods.


## What is an E-invoice dispatch from transactions?


A Dispatch from Transactions is a transaction where goods are dispatched from a location other than that stated on the invoice as the supplier’s main place of business. While the supplier is still the seller, the goods might be dispatched from another registered warehouse, depot or branch, or third party logistics.


The GST e-invoice system requires businesses to properly record the supplier, buyer and dispatch information for the invoice to be successfully validated by the Invoice Registration Portal (IRP). Common examples include:


- Goods sent from the warehouses rather than the head office.
- Third-party warehouse dispatches
- Manufacturer to customer shipments.
- The inventory of the branch offices for dispatching.


**Also read:**[E-Invoicing for B2C transactions](https://getswipe.in/blog/article/e-invoicing-for-b2c-transactions)


## Understanding supplier, buyer and shipping fields


The e-invoice schema has specific fields to identify all parties of the transaction. These fields are important to select correctly and avoid rejection of the invoice.


1. Supplier details


Supplier details provide details of the company who is issuing the invoice. These fields are typically:


- Supplier[GSTIN](https://simple.wikipedia.org/wiki/GSTIN)
- Legal name
- Trade name
- Address
- State code
- PIN code
- Place of supply


These details are the same if the goods are shipped from another registered address.


1. Dispatch from details


The Dispatch From section records the geographical origin of the goods from which the dispatch originates. It generally includes:


- If you have a GSTIN, please mention the same here.
- Dispatch Location
- Complete Address
- State
- Pincode


This section will be required whenever goods are sent to a different address than the supplier's billing address.


1. Buyer details


Buyer information is a piece of information that identifies the business or individual who is buying the goods. Required fields include:


- Buyer GSTIN
- Legal name
- Trade name
- Billing address
- State
- Pincode
- Place of supply


GST return mismatches and input tax credit issues can arise due to incorrect buyer’s information.


1. Ship to details


The ship to section specifies the ship to where the object is actually going. This may be a location:


- Customer’s warehouse
- Project site
- Branch office
- Distribution center
- Third-party location


The shipping address could be different to the registered billing address of the buyer.


**Also Read:**[Generated a wrong e-Way bill from e-Invoice, how to change Transaction Type of e-Invoice?](https://getswipe.in/blog/article/generated-a-wrong-e-way-bill-from-e-invoice-how-to-change-transaction-type-of-e-invoice)


## Difference between Supplier, Dispatch from, Buyer and Ship to


Field Purpose Can It Be Different?


Supplier Business issuing invoice No


Dispatch from Physical dispatch location Yes


Buyer Business purchasing goods Yes


Ship to Actual delivery location Yes


## The example E-invoice for dispatch from transactions


Let’s look at the following situation.


ABC Electronics Pvt. Ltd. registers with Delhi and issues the invoice from its Head office. The goods are sent from its store in Gurgaon, however. Buyer XYZ Traders, Jaipur and Delivery to the XYZ Traders warehouse, Kota.


The e-Invoice would include the following details:


Field Value


Supplier ABC Electronics Pvt. Ltd., Delhi


Dispatch from Gurugram Warehouse


Buyer XYZ trader, Jaipur


Ship to XYZ Warehouse, Kota


This will help in ensuring GST compliance as well as proper e-Way Bill generation.


## Why are accurate supplier, buyer and shipping fields so important?


There are many advantages to selecting the correct field:


- Prevents e-invoice rejection
- Minimizes mismatches in GST returns
- Manages accurate e-Way Bill generation
- Avoids shipment delays
- Improves audit rediness
- Provides adequate input tax credit reconciliation
- Delivers full supply chain visibility


It is especially beneficial for businesses with multiple locations to have accurate dispatch data.


## Common mistake in dispatch from transactions


Simple data entry errors can cause compliance problems for many businesses. Typical errors are:


- As supplier details is added in the warehouse details
- Using incorrect GSTIN
- The address in the "From" field is missing.
- Wrong Ship To address
- A problem with the connection to the state code has occurred.
- Mismatch between invoice and e-Way Bill
- When registration is required, using an unregistered dispatch location.


These are mitigated through regular validation prior to generating an invoice.


## Conclusion


E-Invoice for Dispatch From Transactions is an important feature to ensure GST compliance in case of dispatch from any location other than the business address of the supplier. Accurate supplier entery, Buyer entry, dispatch entry, and shipping reduces the risk of invoice errors, mismatches in e-Way Bill and smoothens the logistics process flow. For companies with several warehouses, branches, or third-party dispatch centers, it is essential to have a well-defined invoicing procedure to ensure compliance and streamline operations.


In case your business has a high volume of dispatch-from transactions check your invoicing procedure and also ensure that you have all the required details on each e-invoice, like supplier, buyer and shipping details before uploading it to the Invoice Registration Portal.


## FAQs


### In e-invoicing, what is known as dispatch from transactions?


It is the situation of sending goods from an address other than the supplier’s registered billing address.


### Is the fispatch from field required?


Yes, if the dispatch address is not the same as the supplier address then the Dispatch From details must be included.


### Is it possible for the buyer and ship to addresses to be different?


Yes, in GST transactions there can be different registered address of the buyer and the address where the goods are to be delivered.


### Will there be an impact on E-way bill generation by dispatch from?


Yes, accurate dispatch from details help to generate a correct e-Way bill and minimise transport related hassles.


‍
