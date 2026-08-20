---
schema_version: "1.0.0"
document_id: "0d7c0d1d75456e3ca1ca9a338b9ecb3a5949e173331e94a935ebbea9a588e12d"
company_key: "yc-xendit"
company: "Xendit"
source_id: "yc-xendit-rss-9a882734f77a"
canonical_url: "https://www.xendit.co/en/blog/pci-dss-for-merchants-what-it-is-why-it-matters-and-what-you-need-to-do/"
published_at: "2026-06-30T08:11:42+00:00"
first_seen_at: "2026-07-20T23:21:00.355601+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:763e7ec93d45e2c71089e4a4e547aae8238a92468a198cc48042dad2406e1afa"
---

# PCI DSS for Merchants: What It Is, Why It Matters, and What You Need to Do

If your business accepts card payments, PCI DSS compliance is not optional. It is a global security standard that governs how businesses handle cardholder data - and failing to meet it carries real consequences, from fines to the suspension of card payment services.


For most merchants, PCI DSS can feel like a complex compliance exercise. In practice, the requirements that apply to your business depend on how many card transactions you process and how your payment integration is set up - which means many smaller merchants face a lighter compliance burden than they might expect.


This article explains what PCI DSS is, which requirements apply to your business, and how Xendit supports merchants through the compliance process.


**Key Takeaways**


- PCI DSS is a global security standard established by major card networks to protect cardholder data and reduce fraud


- All businesses that store, process, or transmit cardholder data must comply with PCI DSS


- The specific requirements that apply to your business depend on your annual card transaction volume and integration method


- PCI DSS compliance is not a one-time activity - it must be maintained and renewed annually


- Xendit determines the correct PCI requirements for your account and notifies you when your compliance status needs to be updated


## **What is PCI DSS?**


PCI DSS or


[Payment Card Industry Data Security Standard](https://www.pcisecuritystandards.org/) , is a global security framework established by major credit card companies to protect cardholder data and reduce the risk of card fraud and data breaches.


***Read also:***[What Is a Chargeback Fee? A Complete Guide for Businesses](https://www.xendit.co/en/blog/what-is-a-chargeback-fee-a-complete-guide-for-businesses/)


The standard consists of 12 requirements covering network security, data protection, access control, and vulnerability management. Any organization that stores, processes, or transmits credit card information is required to comply with these requirements, regardless of size or transaction volume.


Complying with PCI DSS helps your business in three concrete ways. It builds customer trust by demonstrating that card data is handled securely. It protects your business from the financial and reputational damage of a data breach. And it helps you avoid fines that can be levied for non-compliance.


## **PCI DSS compliance levels explained**


Not all merchants face the same PCI DSS requirements. The level that applies to your business is determined by two factors: your annual card transaction volume on Visa or Mastercard, and your payment integration method.


[Xendit uses the following framework](https://docs.xendit.co/docs/pci-dss-compliance?highlight=PCI%20DSS) to determine which requirements apply to merchants on its platform.


### **Level 1**


Applies to merchants processing more than 6 million online card transactions per year on Visa or Mastercard. Level 1 merchants must submit an Attestation of Compliance (AOC) or Report on Compliance (ROC) signed by a Qualified Security Assessor (QSA) or security officer, along with quarterly network scans.


### **Level 2**


Applies to merchants processing between 1 million and 6 million online card transactions per year. Depending on the integration method, Level 2 merchants submit a Self-Assessment Questionnaire (SAQ-A, SAQ-A-EP, or SAQ-D). SAQ documents must be signed by a QSA or Certified Internal Security Assessor (ISA). Quarterly network scans are also required.


### **Level 3**


Applies to merchants processing between 20,000 and 1 million online card transactions per year. Similar to Level 2, the applicable SAQ type depends on your integration method. Documents may be self-signed by a QSA, ISA, or the merchant. Quarterly network scans apply.


### **Level 4**


Applies to merchants processing fewer than 20,000 online card transactions per year. Level 4 merchants are required to comply with PCI DSS and typically validate compliance using an SAQ and quarterly network scans, unless otherwise specified by their acquiring bank or applicable regulatory requirements.


The specific SAQ type required at each level depends on your payment integration method - whether you use Payment Links, the Payment API via Components, or Direct API integration. Xendit determines the correct requirements for your account based on your volume and integration setup.


## **What is an SAQ and do you need one?**


A Self-Assessment Questionnaire (SAQ) is a validation tool used by merchants to assess and document their PCI DSS compliance. There are


[several SAQ types](https://listings.pcisecuritystandards.org/pdfs/instructions_guidelines_v1-1.pdf) - SAQ-A, SAQ-A-EP, and SAQ-D being the most common - and the applicable type depends on how your business handles cardholder data and how your payment integration is configured.


SAQ-A applies to merchants who have fully outsourced all cardholder data functions to a third party and have no electronic storage, processing, or transmission of cardholder data on their systems.


SAQ-A-EP applies to merchants who partially outsource payment processing but whose website directly affects the security of the payment transaction.


SAQ-D is the most comprehensive SAQ type and applies to merchants who store, process, or transmit cardholder data in ways not covered by SAQ-A or SAQ-A-EP.


If you are unsure which SAQ applies to your business, Xendit will determine this for you when you activate card payments on your account.


## **Third-party service providers and PCI DSS**


If your business uses a third-party service provider (TPSP) that stores, processes, or transmits cardholder data on your behalf, your PCI DSS obligations extend to that relationship.


A TPSP is any party that has access to your customers' cardholder data, manages system components within your payment environment, or can otherwise affect the security of cardholder data. When you use a TPSP, you are outsourcing part of your PCI DSS responsibilities - but not the ultimate accountability.


In practice, this means you are required to obtain your service provider's Attestation of Compliance (AOC) or Report on Compliance (ROC), maintain a list of all service providers you use, and monitor their compliance status annually. Using a compliant service provider reduces your risk, but does not eliminate your own compliance obligations.


If you use a TPSP as part of your payment setup on Xendit, you will be required to provide the provider's name along with their AOC or ROC as part of your compliance documentation.


## **PCI DSS compliance for platform merchants**


If your business operates a platform or marketplace that onboards sub-merchants through[XenPlatform](https://www.xendit.co/en/products/xenplatform/) , PCI DSS validation requirements are assessed at the platform level.


Where the platform manages the payment acceptance architecture on behalf of its sub-merchants, Xendit collects and validates PCI DSS documentation from the platform rather than from each individual sub-merchant. Sub-merchants operating solely within the approved platform payment ecosystem are not required to submit separate PCI DSS documentation.


However, sub-merchants are required to complete a lightweight self-attestation during onboarding confirming that they do not directly collect, process, or store cardholder data outside the approved platform integration.


## **Annual renewal and ongoing compliance**


PCI DSS compliance is not a one-time onboarding activity. Merchants are required to maintain valid compliance documentation and renew it at least once every twelve months from the date of their most recently accepted validation.


Xendit monitors compliance status and will notify you ahead of renewal deadlines so your business can prepare updated documentation in time. If your transaction volume grows to a point where a different compliance level applies, Xendit will also notify you of the change in requirements.


Staying on top of these notifications matters: if compliance documentation is not renewed in time, your card payment services may be suspended until the required documentation is submitted and approved.


To obtain your AOC or ROC, you will typically need to complete the applicable Self-Assessment Questionnaire (SAQ) for your compliance level and have it signed by a Qualified Security Assessor (QSA), a Certified Internal Security Assessor (ISA), or, where permitted, self-signed by an authorized officer of your business.


Please have your latest Attestation of Compliance (AOC) available to verify your compliance. If you are unsure how to obtain this, you may refer to


[this list of QSAs](https://www.pcisecuritystandards.org/assessors_and_solutions/qualified_security_assessors/) that can help your business with an assessment.


Merchants requiring a ROC will need to engage a QSA to conduct a formal assessment. Your acquiring bank or payment provider can typically guide you to the correct documentation based on your transaction volume and integration method.


## How Xendit helps merchants maintain PCI DSS compliance


Xendit acts as a PCI DSS advocate for merchants on its platform, managing much of the compliance determination process on your behalf.


When you activate card payments on your Xendit account, Xendit prompts you to provide information about your transaction volumes and integration method. Based on that information, Xendit determines the correct PCI DSS requirements for your account - so you are not required to navigate the compliance matrix yourself.


Xendit will also notify you in advance when your compliance documentation is approaching its renewal date, and when a change in your transaction volume means your compliance level requirements have changed.


For full documentation on PCI DSS requirements at Xendit and the validation process,


[visit our documentation](https://docs.xendit.co/docs/pci-dss-compliance?highlight=PCI%20DSS) .


New to Xendit?


[Create an account](https://dashboard.xendit.co/register) and accept card payments on a platform that guides you through PCI DSS compliance from day one.


## **Frequently Asked Questions**


### **What is PCI DSS?**


PCI DSS stands for


[Payment Card Industry Data Security Standard](https://www.pcisecuritystandards.org/) . It is a global security framework established by major credit card companies to protect cardholder data and reduce the risk of card fraud. Any business that stores, processes, or transmits cardholder data is required to comply.


### **Does PCI DSS apply to my business?**


Yes, if your business accepts card payments. The specific requirements depend on your annual card transaction volume and your payment integration method. Xendit determines the applicable requirements for your account when you activate card payments.


### **What is an SAQ?**


A Self-Assessment Questionnaire is a compliance validation tool that merchants use to document their PCI DSS compliance. The type of SAQ required depends on your integration method and how your business handles cardholder data. Xendit will determine which SAQ type applies to your account.


### **How do I obtain my AOC or ROC?**


You typically obtain an Attestation of Compliance (AOC) or Report on Compliance (ROC) by completing the Self-Assessment Questionnaire (SAQ) applicable to your compliance level and having it signed by a Qualified Security Assessor (QSA), a Certified Internal Security Assessor (ISA), or self-signed where permitted. A ROC requires a formal assessment conducted by a QSA. Xendit can help confirm which validation type applies to your business based on your transaction volume and integration method.


### **How often do I need to renew my PCI DSS compliance?**


PCI DSS compliance documentation must be renewed at least once every twelve months. Xendit monitors renewal deadlines and will notify you in advance so you can prepare updated documentation in time.


### **What happens if I do not maintain PCI DSS compliance?**


Failure to submit required compliance documentation by the applicable deadline can result in the suspension of card payment processing services until the documentation is submitted and approved by Xendit's security team.


### **Does PCI DSS apply to my subaccounts on XenPlatform?**


For platform merchants using XenPlatform, PCI DSS validation is assessed at the platform level. Sub-merchants are not required to submit separate documentation but must complete a lightweight self-attestation confirming they do not handle cardholder data outside the approved platform integration.


### **Where can I find more information about PCI DSS compliance at Xendit?**


Full documentation on PCI DSS requirements, SAQ types, and the compliance process at Xendit is available at


[Xendit Documentation](https://docs.xendit.co/docs/pci-dss-compliance?highlight=pci%20dss)
