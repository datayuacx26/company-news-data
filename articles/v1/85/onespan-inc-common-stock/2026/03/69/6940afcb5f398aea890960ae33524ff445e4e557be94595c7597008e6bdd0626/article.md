---
schema_version: "1.0.0"
document_id: "6940afcb5f398aea890960ae33524ff445e4e557be94595c7597008e6bdd0626"
company_key: "onespan-inc-common-stock"
company: "OneSpan Inc."
source_id: "onespan-inc-common-stock-news-import-99f0e67edec7"
canonical_url: "https://www.onespan.com/cybersecurity/blog/why-european-banks-must-act-now-on-EUDI-wallets"
published_at: "2026-03-19T12:00:00+00:00"
first_seen_at: "2026-07-22T07:23:32.079388+00:00"
fetched_at: "2026-07-28T22:18:23.526103+00:00"
content_hash: "sha256:4b59cc066d12b4822b06b2237919bd29a5d5dd328a9885a8969960db443352a6"
---

# Why European banks must act now on EUDI Wallets: Regulatory drivers, deadlines, and current ambiguities

Digital identity in European banking is on the verge of an important shift. As the European Union moves towards a unified digital market, the way banks onboard and authenticate digital banking application users is changing. At the heart of this transformation is the[European Digital Identity (EUDI) Wallet](https://ec.europa.eu/digital-building-blocks/sites/spaces/EUDIGITALIDENTITYWALLET/pages/694487738/EU+Digital+Identity+Wallet+Home) .


Various European Union regulations, specifically the Digital Identity Regulation (eIDAS 2.0), AML Regulation (AMLR), and draft Payment Services Regulation (PSR / PSD3), require banks to adopt ​​EUDI Wallets for customer onboarding and strong customer authentication.


In this blog, we provide a breakdown of the requirements in the various regulations, highlight major deadlines, and discuss a few areas where the precise requirements remain ambiguous. To meet upcoming regulatory deadlines, European banks should start acting now to support EUDI Wallets for customer onboarding and authentication.


## European Digital Identity Regulation mandates strong user authentication


The[Digital Identity Regulation](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex%3A32024R1183) , formally referred to as Regulation (EU) 2024/1183 and also called eIDAS 2.0, is the primary engine behind the introduction of EUDI Wallets. Its goal is to provide every EU citizen with a secure, sovereign digital identity that can be widely used with public and private organizations.


Article 5f(2) requires large and medium-sized relying parties that (a) have a legal or contractual obligation to use strong user authentication, and (b) are active in transport, energy, banking, financial services, social security, health, drinking water, postal services, digital infrastructure, education, or telecommunications, to accept EUDI Wallets for strong user authentication, upon the voluntary request of the user.


Financial institutions in the European Union have a legal obligation to use strong user authentication, albeit under a different name. Indeed, the revised Payment Services Directive (PSD2) uses the terminology “strong customer authentication” (SCA) instead of “strong user authentication” (SUA). Furthermore the definition of SUA does not refer to dynamic linking, a key concept of SCA. However, according to the European Commission’s Directorate-General for Communications Networks, Content and Technology (DG Connect), which spearheads the EUDI Wallet initiative, these terms[should be considered synonyms](https://ec.europa.eu/digital-building-blocks/sites/spaces/EUDIGITALIDENTITYWALLET/pages/713526976/FAQ) . Nevertheless, further clarification in the final PSR about this topic would be beneficial.


The key deadlines of the Digital Identity Regulation are as follows:


- **24 December 2026:** Every EU member state must ensure at least one EUDI Wallet is available to its citizens.
- **24 December 2027:** The above-mentioned relying parties need to support EUDI Wallets for strong user authentication. This means that by the end of 2027, European banks must allow their users to use EUDI Wallets for login and transaction authentication. (Thank you, Santa Claus!)


## AML Regulation leveraging EUDI Wallets for customer due diligence


Parallel to the digital identity mandate, the new EU Anti-Money Laundering (AML) regulations, including the AML Regulation (AMLR), Anti-Money Laundering Authority (AMLA), and the 6th AML Directive (AMLD6), are transforming how financial institutions approach anti-money laundering and customer onboarding.


Nowadays, banks’ customer due diligence processes often rely on users scanning physical ID documents (e.g. identity cards, passports) and taking selfies. With the advent of EUDI Wallets, banks can request identity information in the form of so-called[Qualified Electronic Attestations of Attributes](https://www.digital-identity-wallet.eu/news/what-are-the-3-types-of-electronic-attestations-of-attributes-eaa/) (QEAAs). These are highly secure digital credentials issued by[Qualified Trust Service Providers](https://digital-strategy.ec.europa.eu/en/policies/eu-trusted-lists) (QTSPs) that offer the highest legal assurance, equivalent to paper documents. The usage of EUDI Wallets for customer due diligence promises lower friction, reduced drop-off rates, and higher levels of security against identity fraud.


Article 22(6b) of the[AML Regulation](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R1624) allows for electronic identity verification mechanisms. In addition,[Article 7](https://www.amla.europa.eu/document/download/3d430294-5171-455c-b565-a86fc5f3cb1c_en?filename=Consultation%20Paper%20Draft%20RTS%20under%20Article%2028%281%29.pdf) of the draft Regulatory Technical Standards (RTS) under the AML Regulation defines requirements for remote identity verification. More specifically, Article 7 stipulates that banks have to use identity verification mechanisms that meet the requirements of Regulation (EU) 2014/910 (eIDAS 1.0) with regard to the assurance levels ‘substantial’ or ‘high.’ This opens the door for using EUDI Wallets in the context of customer due diligence. Finally, Article 5f of the Digital Identity Regulation, discussed above, is often interpreted to mean that banks need to support EUDI Wallets for customer due diligence.


> While the AML Regulation becomes applicable on 10 July 2027, **the Digital Identity Regulation deadline of 24 December 2027** defines the date by which banks need to support EUDI Wallets for customer due diligence.


## Draft Payment Services Regulation (PSR / PSD3)


European regulators are[currently finalizing](https://www.onespan.com/blog/psd3-psr-updates-2025) the Payment Services Regulations (PSR) and third Payment Services Directive (PSD3). The final text is expected around May 2026. Once enacted, the PSR will be the main European regulation defining requirements for strong customer authentication in financial services, replacing PSD2. The draft PSR contains several articles that relate to the support of EUDI Wallets by banks.


**Outsourcing agreements with technical service providers.** Article 87 of the draft PSR proposal of the European Commission stipulates that each bank has to “enter into an outsourcing agreement with its technical service provider in case that technical service provider is providing and verifying the elements of strong customer authentication.”


Since banks have to support EUDI Wallets for strong customer authentication, one could argue that banks have to enter into outsourcing agreements with all providers of EUDI Wallets. This would be a heavy burden for banks, as there will be at least 27 EUDI Wallets, and probably many more.


However, according to DG Connect, there is[no need for such outsourcing agreements](https://ec.europa.eu/digital-building-blocks/sites/spaces/EUDIGITALIDENTITYWALLET/pages/713526976/FAQ) . Its rationale is that banks supporting EUDI Wallets act both as issuer and verifier of the digital credential used for strong customer authentication, so they retain full control over the authentication decision and do not actually delegate authentication to a third party.


> We call upon the European regulators to embed this clarification into the final PSR.


**Security of personal security credentials.** The Regulatory Technical Standards (RTS) on Strong Customer Authentication (SCA) and Common and Secure Communication (CSC) under PSD2 state that banks have to ensure the safety, security, and confidentiality of personalized security credentials, such as credentials used for strong customer authentication.


Since a bank does not manage all EUDI Wallets itself, one can wonder how this requirement can be met in practice. The current answer to this concern is that the existing RTS on SCA and CSC will be repealed by the new version that the European Banking Authority (EBA) is mandated to develop under Article 89 of the PSR. Under Article 89(3), when developing the new RTS, the EBA should take into account the use of EUDI Wallets for payment authentication.


> It will be important for the final PSR (or RTS) to clarify the liability in case of fraud with EUDI wallets, as banks do not have full control of the wallets themselves.


## Current conclusions about EUDI Wallet regulation updates


Banks are subject to various existing and upcoming regulatory mandates related to EUDI Wallets, including the Digital Identity Regulation, Anti-Money Laundering Regulation, and Payment Services Regulation. While still in flux, it is clear that banks need to support EUDI Wallets for customer due diligence and strong customer authentication by 24 December 2027. Banks should therefore start preparing and aligning their onboarding and authentication programs with the emerging EU regulatory framework.


For more information about how OneSpan can help you support EUDI Wallets for customer onboarding and strong customer authentication, please reach out to your ​​OneSpan account executive or[schedule an intro call with one of our experts](https://www.onespan.com/security/contact-us) .


##### Contact us


## Talk to a cybersecurity expert


We're happy to answer your questions and get you acquainted with our solutions.


[Contact us](https://www.onespan.com/security/contact-us)
