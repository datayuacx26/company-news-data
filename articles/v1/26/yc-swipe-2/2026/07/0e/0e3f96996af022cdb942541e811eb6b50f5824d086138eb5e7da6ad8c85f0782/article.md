---
schema_version: "1.0.0"
document_id: "0e3f96996af022cdb942541e811eb6b50f5824d086138eb5e7da6ad8c85f0782"
company_key: "yc-swipe-2"
company: "Swipe"
source_id: "yc-swipe-2-news-import-e93617422a04"
canonical_url: "https://getswipe.in/blog/article/e-invoice-credit-notes-debit-notes-reporting-workflow"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T22:20:59.643+00:00"
fetched_at: "2026-07-31T22:21:00.628476+00:00"
content_hash: "sha256:a2bda260997e3dd0290313cdebc4c3759668276d9dda1740b434f3c4b5bc52db"
---

# E-invoice for credit notes and debit notes: Reporting workflow

# **E-invoice for credit notes and debit notes: Reporting workflow**


## **What is an E-invoice**


The electronic[invoice](https://getswipe.in/blog/article/e-invoice-gst-instruments-guide) , also known as an e-invoice, refers to a goods and services tax invoice that has received electronic confirmation from the Invoice Registration Portal (IRP). Instead of preparing the invoices on the government portal, companies prepare them with the help of the ERP or GST compliant billing systems and send them to the IRP for authorization. Therefore, the generated invoices will fulfill the requirements of GST law and ensure precision. The following transactions are carried out once the invoice has been authorized:


- Invoice Reference Number ([IRN](https://getswipe.in/blog/article/what-is-irn-invoice-reference-number) )
- JSON and QR Code signed by digital signature
- Validation status


After the e-invoice is created, it will automatically be transmitted to the GST and e-way bill systems. Thus, it eliminates the need for data input and complies with tax regulations.


## **Understanding credit notes and debit notes**


The issuance of credit notes and debit notes is one of the effects of GST on businesses when they need to change the amount of tax charged in invoices already issued.


### **Credit note**


Credit notes can be issued in several situations:


- When goods are returned back by a customer
- When goods are damaged
- When it is necessary to reduce the value of the invoice
- When there is an overcharge of GST
- When discounting takes place after invoice is issued
- When the quantity supplied decreases


### **Debit note**


A debit note can be drawn in cases like:


- When the invoice amount needs to be increased
- When goods are supplied additionally
- When GST has been undercharged
- When there is a mistake in pricing
- When the quantity supplied increases after invoicing


Both documents are designed to change the invoice issued and need to reference it correctly.


## **Are credit notes and debit notes covered under e-invoicing?**


If your company is subject to the GST e-invoicing requirement, you should assure that all credit notes and debit notes issued for[B2B](https://blog.nuorder.com/b2b-transaction) transactions are reported to the Invoice Registration Portal. Every credit/debit note will have its own:


- Invoice Reference Number (IRN)
- QR code
- Digital signature


Only when IRN is generated successfully will these documents become valid.


## **E-invoice reporting workflow for credit notes and debit notes**


The report is based on a pre-defined workflow.


1. **Step 1: Generate credit/debit note**


Create the credit/debt note with the help of your accounting software. Such a note contains:


- GST registration number of the supplier
- GST registration number of the receiver
- Invoice reference number
- Invoice date
- Referring to the original invoice
- Value of the transaction
- GST details
- Reason for a credit/debit note.


1. **Step 2: Create the JSON file**


A document will be in the format of GST e-invoice JSON. It has following required fields:


- Type of a document
- Reference of an invoice
- GST details
- Breakdown of taxes
- HSN codes and names of the items


1. **Step 3: Upload the file to Invoice registration portal**


JSON file will be uploaded through the following systems:


- Integration with ERP
- GST Suvidha Provider (GSP)
- API integration
- Third parties providing services of e-invoicing


1. **Step 4: Validation at IRP**


IT system validates the following:


- Correctness of the GSTIN
- Duplicate document numbers
- All required fields
- Calculation of tax
- Overall format of the invoice
- Complying with the schema


If the validation fails, the document will be returned for correction before re-submission.


1. **Step 5: IRP generates IRN**


Moreover, a system will produce:


- Invoice Reference Number (IRN)
- Signed[JSON](https://filesign.icegate.gov.in/JSONSignVerify.do)
- QR
- Digital signature


This credit or debit note thus becomes a valid GST document.


1. **Step 6: GST portal updates**


The authenticated details are automatically transmitted to:


- GST Portal
- Supplier records
- Recipient records
- E-way bill system (if needed)


This reduces manual reporting work.


1. **Step 7: Keep business records**


Business entities must keep safe:


- Original credit/debit note
- IRN
- QR Code
- Signed JSON
- GST reporting information


These are necessary during GST audits and assessments.


## **Information required for reporting**


A credit and debit note usually contains necessary details such as the details of suppliers, details of recipients, GSTIN of both, original invoice number with date, credit and debit note number, document date, taxable value, applicable CGST, SGST, IGST, and Cess (if applicable), HSN code, item description, quantity, unit price, total invoice amount and reason for adjustment. Including these particulars ensures GST reporting, reconciliation with the original invoice, and compliance with e-invoicing requirements.


## **Common reasons for issuing credit notes**


Credit notes are mainly issued in case of:


- Returns
- Faulty merchandise
- Wrong invoicing
- Extra tax imposed
- Sale rebates
- Price cuts post sale
- Cancellation of orders


## **Common reasons for issuing debit notes**


Debit notes are mostly issued for:


- Extra billing
- Revised pricing
- GST undercharge
- Additional volume provided
- Freight changes
- Improved service
- Contract changes


## **Benefits of reporting credit and debit notes through E-invoicing**


- **Enhanced GST compliance**


The usage of the Invoice Registration Portal (IRP) ensures that credit and debit notes meet the necessary GST rules and e-invoicing specification, thus reducing the risks of being fined for non-compliance and allowing businesses to comply with the law.


- **Enhanced transparency**


Thanks to the fact that verified documents are available to both the seller and the buyer, companies can create a consistent record of transactions, which helps to enhance the level of transparency and avoid conflicts during the reconciliation.


- **Lowered error levels**


By automating the verification process, businesses reduce the chances of making the mistakes related to manual data entry. Consequently, companies can generate reliable credit and debit notes without any concerns.


- **Faster reconciliation**


As the data from e-invoices gets registered automatically with the GST system, businesses can perform the reconciliation of invoices, credit notes, and debit notes much quicker.


- **Easier audits**


With the help of IRN and QR code, the process of GST[audits](http://capincrouse.com/a6KG1) is made much simpler for the businesses because they can easily find the evidence that tracks any transaction and adjustment.


## **Credit note vs Debit note**


Parameter Credit Note Debit Note


Purpose Reduces the value or tax of an original invoice. Increases the value or tax of an original invoice.


Common Reasons Sales return, damaged goods, discounts, excess tax charged. Additional supply, price revision, undercharged GST, billing corrections.


Impact on Invoice Value Decreases the invoice amount. Increases the invoice amount.


IRN Required Yes, if covered under the e-invoicing mandate. Yes, if covered under the e-invoicing mandate.


Reported to IRP Yes Yes


QR Code Generated Yes Yes


Reference to Original Invoice Mandatory Mandatory


## **Conclusion**


The use of credit and debit notes is significant because they help in correcting transactions in businesses operating under GST. For businesses affected by the mandate of e-invoicing, it is essential to report such documents through the Invoice Registration Portal as part of compliance requirements to be fulfilled. Following the correct workflow for reporting will help in accomplishing correct filing of taxes, avoiding reconciliation problems, and achieving ease of compliance with GST.


Suggested Read:[Time Limits to Issue GST Invoices, Debit & Credit Notes](https://getswipe.in/blog/article/gst-invoice-debit-credit-note-time-limits)


## **FAQs**


### **Do debit notes require an IRN?**


Yes, all the debit notes that meet certain criteria need an Invoice Reference Number (IRN) as produced by the IRP.


### **What do you need to do if a credit note does not pass IRP validation?**


It will be rejected, and you need to correct your errors and resubmit the documentation in order for the IRN to be generated.


### ‍ **Can billing software create credit notes?**


Yes, there is a lot of billing software available that is compliant with the GST, which can help create, validate, and transmit credit notes to the IRP directly.


### **Is the QR code created for debit notes?**


Yes, the QR code for debit notes is created once they are validated by the IRP.


### **Why would the original invoice reference be important for credits and debit notes?**


Because it connects the adjustment document with the original invoice thereby ensuring proper GST reconciliation and auditing.


‍
