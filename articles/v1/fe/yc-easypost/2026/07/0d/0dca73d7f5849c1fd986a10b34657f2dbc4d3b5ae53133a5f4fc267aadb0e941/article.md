---
schema_version: "1.0.0"
document_id: "0dca73d7f5849c1fd986a10b34657f2dbc4d3b5ae53133a5f4fc267aadb0e941"
company_key: "yc-easypost"
company: "EasyPost"
source_id: "yc-easypost-rss-904c17edb370"
canonical_url: "https://www.easypost.com/blog/important-changes-for-shipping-to-the-eu-starting-july-1-2026/"
published_at: "2026-07-01T18:59:39+00:00"
first_seen_at: "2026-07-20T23:20:22.619516+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:8d7b842f40c79021157329cf21c402c65d34303f3466530e14eead409a145deb"
---

# Important Changes for Shipping to the EU Starting July 1, 2026

Starting July 1, 2026, the European Union is eliminating the de minimis duty threshold for imported goods. This means **all shipments to the EU will now be subject to import duties, regardless of value.**


As a result, several EU member states will stop accepting Delivered Duty Unpaid (DDU) shipments, meaning packages without prepaid duties may be refused, returned, or subject to higher handling fees at destination.


These changes affect anyone importing to EU countries. EasyPost strongly recommends taking action ahead of peak season to ensure that packages reach their destinations on time without incurring extra fees.


##


What exactly is changing?


**On July 1, 2026:**


- The EU de minimis threshold is removed for all goods.
- A temporary €3 flat-rate duty per commodity/HS code takes effect.*
- Duties are non-refundable, even if the package is not delivered.
- DDU shipments will not be accepted by several EU countries, and countries that do accept them will apply a higher handling fee (€15–€25) that will increase the likelihood of customer refusals and returns.


**In the future:**


- A separate EU-wide handling fee, reported at roughly €2 per shipment, has been proposed for November 1, 2026, but is not yet finalized in the released regulation.
- Full ad valorem (percentage-based) duties are expected in 2028.


***Note:** The EU’s Import One-Stop Shop (IOSS) system does not support the new €3 flat-rate duty. IOSS shippers will need a separate solution, such as DDP, to cover this charge.


##


What you should do as a shipper


To keep shipping costs low and avoid an increase in returned products, EasyPost recommends making these three changes as soon as possible:


1. **Transition to Delivered Duty Paid (DDP):** Prepay duties and taxes at the time of shipment with DDP to avoid high handling fees and ensure smooth customs clearance.
2. **Prepare to share your Product Identifiers (PIDs):** This refers to the merchant SKU, the manufacturer’s own product code, and the standardized GTIN/EAN/UPC where one exists. Including the PID is voluntary from July 1, but mandatory starting November 1, 2026.
3. **Consult with carriers:** Contact your carriers for guidance, and consider adding new carriers to your mix.


##


Carrier FAQ


**USPS**


- USPS PC Postage does not support DDP today.
- USPS Ship supports DDP via **shipment.options.incoterms=DDP.**
- If you’re currently importing with EasyPost’s USPS account (PC Postage), you’ll want to add USPS Ship to your carrier mix use their DDP service.


**FedEx**


- Starting November 1, 2026, FedEx will require shippers to complete the PID field.
- We recommend FedEx shippers select “Bill Shipper” for their DDP packages to ensure duties and taxes are not invoiced to the recipient.


**UPS**


- UPS® Worldwide Economy DDU services to Germany will be suspended.


**Asendia**


- Supports shippers who need to use DDU temporarily or are just entering the EU market. Duties are calculated and handled earlier in the shipping journey but after checkout.


**DHL**


- DHL supports DDP under “Duties and Taxes Paid (DTP)”. Through EasyPost, this is enabled via **options.incoterm = “DDP”.**
- Starting July 1, 2026, DHL Express will require a signed Power of Attorney, attestation letter, and line-item data (buyer, seller, retail value) per shipment. Mixed-value shipments will lose consolidated clearance benefits.


With these changes taking effect on **July 1, 2026** , we recommend contacting your account team or integrating with DDP solutions as soon as possible.
