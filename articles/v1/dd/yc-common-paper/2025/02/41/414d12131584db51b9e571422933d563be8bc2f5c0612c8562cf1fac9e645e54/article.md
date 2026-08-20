---
schema_version: "1.0.0"
document_id: "414d12131584db51b9e571422933d563be8bc2f5c0612c8562cf1fac9e645e54"
company_key: "yc-common-paper"
company: "Common Paper"
source_id: "yc-common-paper-rss-3b76f5ee637c"
canonical_url: "https://commonpaper.com/blog/responsibility-for-customer-actions-in-contracts/"
published_at: "2025-02-11T15:18:00+00:00"
first_seen_at: "2026-07-20T23:20:15.555416+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:1d66143658084028d0c0ba8023a7fa0e78f8a1645174c82fe4d26af07a210e78"
---

# Responsibility for Customer Actions in Contracts

Software products can be used in a wide variety of contexts. While the vast majority of customers are honest, some might use your product in a shady way. It’s natural for a vendor to ask “What happens if customers use our product for illegal activities?”


Let’s break down how your contract can protect you and the best practices for managing this kind of risk.


## Standard Contract Protections


Most modern software agreements include several provisions that specify that any violation of relevant laws would be a breach of the contract. For example, section 6.1 of the[Cloud Service Agreement v2.1](https://commonpaper.com/standards/cloud-service-agreement/) (CSA) says:


> Mutual. Each party represents and warrants to the other that: (a) it has the legal power and authority to enter into this Agreement; (b) it is duly organized, validly existing, and in good standing under the Applicable Laws of the jurisdiction of its origin; (c) it will comply with all Applicable Laws in performing its obligations or exercising its rights in this Agreement; and (d) it will comply with the **Additional Warranties** .


Additionally, the next section, 6.2, specifies that the customer has the rights to any content they upload or input into the product.


Section 2.1 requires the user to comply with relevant laws and prevents a number of activities that could harm the vendor, their product, or their other users.


> Except as expressly permitted by this Agreement, **Customer** will not (and will not allow anyone else to): (i) reverse engineer, decompile, or attempt to discover any source code or underlying ideas or algorithms of the Product (except to the extent Applicable Laws prohibit this restriction); (ii) provide, sell, transfer, sublicense, lend, distribute, rent, or otherwise allow others to access or use the Product; (iii) remove any proprietary notices or labels; (iv) copy, modify, or create derivative works of the Product; (v) conduct security or vulnerability tests on, interfere with the operation of, cause performance degradation of, or circumvent access restrictions of the Product; (vi) access accounts, information, data, or portions of the Product to which **Customer** does not have explicit authorization; (vii) use the Product to develop a competing service or product; (viii) use the Product with any High Risk Activities or with any activity prohibited by Applicable Laws; (ix) use the Product to obtain unauthorized access to anyone else’s networks or equipment; or (x) upload, submit, or otherwise make available to the Product any Customer Content to which **Customer** and Users do not have the proper rights.


Use of the Product must comply with all Documentation and Use Limitations.


You can bolster these protections by using the optional Use Limitations variable to add additional restrictions on how and where the product may (or may not) be used.


## Indemnity Protection


Another important aspect is how the agreement handles indemnification. The CSA can create a customer indemnification obligation through the Customer Covered Claims variable. This allows vendors to specify which things the customer is on the hook for to protect and compensate the vendor in the event of a third-party claim. The default language is:


> Any action, proceeding, or claim that (1) the Customer Content, when used according to the terms of the Agreement, violates, misappropriates, or otherwise infringes upon anyone else’s intellectual property or other proprietary rights; or (2) results from Customer’s breach or alleged breach of Section 2.1 (Restrictions on Customer).


Relevant to managing risk if a customer uses your product for illegal activities is the second option, “results from Customer’s breach or alleged breach of Section 2.1 (Restrictions on Customer). Remember, that includes a prohibition on a customer using the product “with any activity prohibited by Applicable Laws”.


## Different Agreement Types


The CSA and the[Software License Agreement](https://commonpaper.com/standards/software-license-agreement) (SoLA) have similar approaches to liability, indemnity, and protections for both the vendor and the customer. The main differences arise because CSA is built for SaaS, and the SoLA is intended for on-prem or private cloud products.


The[Design Partner Agreement](https://commonpaper.com/standards/design-partner-agreement/) is a much lighter weight contract, and does not have many of the same protections as the CSA and SoLA. It’s intended for earlier stage companies who are open to accepting more risk in exchange for streamlining the process of getting their first users onboard. The language library gives an example of how additional protections like[indemnification](https://commonpaper.com/standards/design-partner-agreement/add-intellectual-property-indemnity/) and[liability caps](https://commonpaper.com/standards/design-partner-agreement/add-limitation-of-liability/) can be added.


The[Letter of Intent](https://commonpaper.com/documents/letter-of-intent/) (LOI) is the lightest weight approach of all, and, in fact, is not actually a binding contract. Instead, it’s a letter that outlines a mutual plan to work together. The LOI typically includes no protections other than an optional confidentiality clause.


## Protection beyond the contract


While contractual protections are essential, they’re just one component of a strategy to reduce the risks posed by customers. Other tactics include automatic monitoring for problematic use and training for support staff.


One of the simplest steps is[blocking traffic from countries that you can’t do business with because of sanctions](https://ofac.treasury.gov/sanctions-programs-and-country-information) . This can be done at the level of the firewall or within your application if you collect location information. Other tools include scanning uploaded content and behavior of users to check for concerning patterns.


Another important tactic is training for the support team on what to watch out for. As an example at a prior company, our support team knew to watch out for support tickets that suggested we may be dealing with healthcare data, things like “Can you help solve a problem with column medical_condition in table patient_info?” When that happened, our team checked if we had a[Business Associate Agreement](https://commonpaper.com/standards/business-associate-agreement/) in place with that customer. If not, that triggered an immediate escalation to make sure both we and the client didn’t run afoul of[HIPAA](https://www.hhs.gov/hipaa/index.html) , a US healthcare privacy law.


## Key takeaways on risk management


Most modern software agreements include protections for the vendor against illegal use by the customer. These clauses are an important part of a risk management strategy, but not a complete solution. Vendors should also consider user monitoring and training for customer facing teams to surface potential problems. As always, there are tradeoffs between how much to focus on risk reduction versus other business priorities.
