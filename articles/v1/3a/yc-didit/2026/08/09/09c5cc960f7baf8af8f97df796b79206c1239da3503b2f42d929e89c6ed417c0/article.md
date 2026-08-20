---
schema_version: "1.0.0"
document_id: "09c5cc960f7baf8af8f97df796b79206c1239da3503b2f42d929e89c6ed417c0"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/egypt-expatriate-kyc-update-2026/"
published_at: "2026-08-18T16:00:00.987+00:00"
first_seen_at: "2026-08-18T22:03:07.820071+00:00"
fetched_at: "2026-08-18T22:03:09.000730+00:00"
content_hash: "sha256:f26848108e27e5fe601542ff0e6531794832be5b387924255ba7b89d9352624f"
---

# Egypt is absorbing the cost of a KYC refresh instead of passing it to the customer

[Back to blog](https://didit.me/blog/) Blog · August 18, 2026


# Egypt is absorbing the cost of a KYC refresh instead of passing it to the customer


Egypt's central bank and foreign ministry now let expatriates refresh their bank KYC at a consulate instead of flying home. Why the state moved the cost, and where remote verification fits.


By Didit


·


August 18, 2026 ·


Updated Aug 18, 2026


On 2 August 2026 the Central Bank of Egypt and the foreign ministry launched a scheme that lets Egyptians working abroad update their banking records at a consulate instead of flying home. Remittances reached 43.1 billion dollars in eleven months. The two facts belong together.


## A stale bank record hurts the customer long before it troubles the regulator


Banks must keep customer information current, and a customer whose record has gone stale can find the account restricted. For someone living and working in another country, the usual fix has been to turn up in person at a branch. On **2 August 2026** the Central Bank of Egypt and the Egyptian foreign ministry launched a scheme to remove that trip.


The obligation underneath is the one that runs through most anti-money-laundering regimes: a customer identified once is not identified forever, and records have to be refreshed. The European Union writes the outer limit into[Article 26 of its anti-money laundering regulation](https://didit.me/blog/eu-amlr-ongoing-monitoring-article-26/) , which caps how stale customer information may become.


What differs in Egypt is who bears the cost of the refresh. In most of the stories on this blog the friction lands on the customer, and the question is whether the harm being prevented justifies it. Egypt has answered a version of that question in the other direction, and it has said why in its own announcement.


## The consulate verifies the signature, the foreign ministry authenticates it, the bank updates the record


Under the "Update Your KYC in Egypt" initiative announced on **2 August 2026** , an Egyptian working abroad visits the nearest Egyptian diplomatic mission or consulate to complete and sign a data update form. The mission verifies the form and forwards it to the Ministry of Foreign Affairs, which completes verification before submitting it to the customer's bank to finalise the update.


Three things stand out in that chain. It is **paper and signature based** , not remote biometric verification: the consulate's job is to witness that the person signing is who they say they are. It runs on **infrastructure the state already operates** , since consulates authenticate documents for citizens every day. And the bank still updates the record, so regulatory responsibility stays where it was.


The Central Bank of Egypt implemented it in partnership with the **National Bank of Egypt** and **Banque Misr** , "in coordination with the Egyptian Money Laundering and Terrorist Financing Combatting Unit (EMLCU), and in collaboration with the Federation of Egyptian Banks". The EMLCU is Egypt's financial intelligence unit. Its presence is what marks this as an anti-money-laundering measure and not merely a customer-service one.


The protocol was signed by Ambassador Nabil Habashi, Deputy Minister of Foreign Affairs, with Mohamed El-Etreby and Hisham Okasha, the chief executives of the two banks, in the presence of Tarek ElKholy, the central bank's Deputy Governor for Banking Stability. It was signed at the seventh Egyptians Abroad Conference, implements a recommendation from the sixth, and follows an earlier scheme called "Open Your Account in Egypt".


The announcement sets **no deadline** . It is framed as the removal of an obstacle, not as a compliance sweep with a date attached.


## The government names foreign currency inflow as an objective, in its own words


The Central Bank of Egypt reported on **9 July 2026** that remittances from Egyptians working abroad reached about **43.1 billion US dollars** in the eleven months from July 2025 to May 2026, up **31.2%** on the roughly 32.8 billion dollars recorded a year earlier. May 2026 alone accounted for about 3.9 billion dollars.


Against a flow that size, the arithmetic on identity friction changes. An expatriate whose account is restricted because a record went stale does not stop sending money home. They send it another way, through channels the central bank cannot see and the financial intelligence unit cannot monitor.


That is the reading, and the government did not leave it to be inferred. The foreign minister said it directly.


> The initiative serves the interests of Egyptians abroad, streamlines their financial transactions within Egypt, and *encourages the transfer of their foreign currency savings into the country* .
>
>
> *Dr Badr Abdelatty, Minister of Foreign Affairs, International Cooperation and Egyptian Expatriates. Central Bank of Egypt announcement, 2 August 2026*


Hassan Abdalla, the Governor of the Central Bank of Egypt, framed the same measure in compliance terms, calling it "a strategic step towards simplifying customer data update processes, while ensuring compliance with regulatory requirements, due diligence measures, and international banking best practices". Both men are describing one thing: a check that still has to happen, made cheaper to complete.


Be careful about what this does and does not show. Egypt has not relaxed a requirement, and the announcement claims no reduction in money laundering risk. What it demonstrates is a state deciding that the cost of a periodic identity check was falling in the wrong place, and moving it. Whether that judgement is right is not something a press release settles, and this post does not settle it either.


## Where Didit fits: a periodic KYC refresh done remotely


The framing here is narrow on purpose, because Egypt chose a consular route rather than a remote one. Where a bank elsewhere is solving the same problem, a periodic refresh is mostly a re-collection, and it maps onto four modules.[ID Verification](https://docs.didit.me/core-technology/id-verification) at **$0.15 per check** re-reads the document.[NFC Reading](https://docs.didit.me/core-technology/nfc-verification) at **$0.15 per check** reads the chip in a passport or national identity card and validates the signature on what it returns.[Passive Liveness](https://docs.didit.me/core-technology/liveness) at **$0.10 per check** confirms a real person is present rather than a stored image.[Proof of Address](https://docs.didit.me/core-technology/proof-of-address) at **$0.20 per check** covers the part of a customer record most likely to have gone stale when someone has moved country. Current module prices are on the[pricing page](https://didit.me/pricing/) .


What we do not do is the thing Egypt's initiative actually turns on. Didit does not authenticate a signature on behalf of a foreign ministry, and it does not substitute for consular authentication where a state has decided that a consulate is the verifier. Nor does it decide when a customer record must be refreshed: that judgement belongs to the bank and its supervisor, and in Egypt's case sits alongside the Egyptian Money Laundering and Terrorist Financing Combatting Unit. Verification helps a bank re-collect and re-check what it already needs. It does not write the policy that says when.


## Frequently asked questions


### What is the Update Your KYC in Egypt initiative?


A scheme launched on 2 August 2026 by the Central Bank of Egypt and the Ministry of Foreign Affairs, International Cooperation and Egyptian Expatriates, letting Egyptians working abroad update their banking records with the National Bank of Egypt or Banque Misr without travelling to Egypt. It is coordinated with the Egyptian Money Laundering and Terrorist Financing Combatting Unit and the Federation of Egyptian Banks.


### How does the consulate process work?


In three steps. The customer visits the nearest Egyptian diplomatic mission or consulate to complete and sign the data update form. The mission or consulate verifies the form and forwards it to the Ministry of Foreign Affairs to complete verification procedures. The ministry then submits it to the customer's bank, which finalises the update.


### Is there a deadline for Egyptian expatriates to update their KYC?


The Central Bank of Egypt's announcement of 2 August 2026 does not state a deadline. It describes the initiative as a way to update records without travelling to Egypt, not as a time-limited requirement.


### Which Egyptian banks are covered?


The National Bank of Egypt and Banque Misr. The two banks signed the cooperation protocol with the Ministry of Foreign Affairs, and the Federation of Egyptian Banks is named as a collaborator. The announcement does not say whether other banks will join.


### How much do Egyptians abroad send home?


About 43.1 billion US dollars in the eleven months from July 2025 to May 2026, according to the Central Bank of Egypt, a rise of 31.2% on the roughly 32.8 billion dollars recorded in the same period a year earlier. In May 2026 alone remittances were about 3.9 billion dollars.


## Related reading


- [AMLR Article 26: how often must you recheck?](https://didit.me/blog/eu-amlr-ongoing-monitoring-article-26/) — The obligation Egypt is making cheaper to satisfy, written as an outer limit in EU law.
- [Asia is moving the cost of weak identity checks onto banks and platforms](https://didit.me/blog/asia-identity-verification-rules-2026/) — Vietnam removed banking access from unverified accounts. Egypt removed the trip instead.
- [Six Latin American economies rewrote their identity rules in 21 months](https://didit.me/blog/latam-identity-verification-rules-2026/) — Mexico built a mandatory biometric credential; another state deciding what identity infrastructure it runs itself.
- [The stablecoin identity rule covers issuance and redemption, not what happens next](https://didit.me/blog/stablecoin-issuer-customer-identification-2026/) — Name, address and identification number, written into a new sector on the other side of the world.


## Sources


1. [Central Bank of Egypt and Ministry of Foreign Affairs launch "Update Your KYC in Egypt" initiative for Egyptians working abroad](https://www.cbe.org.eg/en/news-publications/news/2026/08/02/11/32/cbe-and-ministry-of-foreign) — Central Bank of Egypt · press release · 2 August 2026
2. [Remittances from Egyptians working abroad record USD 43.1 billion over eleven months](https://www.cbe.org.eg/en/news-publications/news/2026/07/09/09/58/remittances-6-26) — Central Bank of Egypt · press release · 9 July 2026
3. [Personal remittances received, Egypt, Arab Republic](https://data.worldbank.org/indicator/BX.TRF.PWKR.CD.DT?locations=EG) — World Bank open data · long-run series, context only


## Who wrote this


**Tuan Nguyen** — Growth · Didit


Writes about identity verification, fraud and compliance at Didit.


*Last reviewed 4 Aug 2026 against the sources above*


Keep reading


## Related articles


- [Europe's deepfake rule is now in force, and it lands on the tool, not the fraud](https://didit.me/blog/eu-ai-act-deepfake-labelling-2026/)
- [AI is now on both sides of the gambling identity check](https://didit.me/blog/gambling-commission-ai-identity-checks-2026/)
- [The stablecoin identity rule covers issuance and redemption, not what happens next](https://didit.me/blog/stablecoin-issuer-customer-identification-2026/)
- [Unico Partners with Didit to Expand Access to State-of-the-Art Identity Verification for SMEs in Brazil](https://didit.me/blog/unico-didit-partnership-brazil/)
- [Didit vs Onfido: coverage, pricing, automation, and migration](https://didit.me/blog/didit-vs-onfido/)
- [Identity Verification Software in 2026: A Buyer's Guide](https://didit.me/blog/identity-verification-software/)
