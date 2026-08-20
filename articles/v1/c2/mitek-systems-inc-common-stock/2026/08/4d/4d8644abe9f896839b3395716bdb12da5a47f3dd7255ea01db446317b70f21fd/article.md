---
schema_version: "1.0.0"
document_id: "4d8644abe9f896839b3395716bdb12da5a47f3dd7255ea01db446317b70f21fd"
company_key: "mitek-systems-inc-common-stock"
company: "Mitek Systems Inc."
source_id: "mitek-systems-inc-common-stock-rss-3bf79578716e"
canonical_url: "https://www.miteksystems.com/blog/digital-ids-explained-types-standards-and-why-adoption-is-accelerating"
published_at: "2026-08-05T17:00:07+00:00"
first_seen_at: "2026-08-05T18:06:49.667292+00:00"
fetched_at: "2026-08-05T18:06:50.528243+00:00"
content_hash: "sha256:44927f9c2759b6cb6d6226f6eae8eb6e949475bacee9215d84f741d1cf0460da"
---

# Digital IDs explained: Types, standards, and why adoption is accelerating

Around the world, governments have started work on the infrastructure necessary for their residents to carry their official identification credentials on the same mobile device they already use every day for adjacent activities like making payments, displaying tickets and managing their accounts. Some examples include the European Union’s digital identity program, and the use of mobile drivers’ licenses in some states in the US.


These initiatives are broadly called “Digital ID,” which can refer to several different things. A “digital ID” might be a chip-enabled identity document, a mobile driver’s license, an identity wallet, or some credential issued by a bank, employer, or a university. Each of these types of digital ID uses a different method to establish trust and return verified identity information.


To fraud and risk teams at financial institutions, or public agencies deciding how digital credentials fit into onboarding and authentication processes, the distinctions matter. This article explains the main types of digital ID, the standards behind them, and the policies that have influenced their adoption.


## What is a digital ID?


A digital identity is an electronic representation of a person and verified attributes that are associated with them. Those attributes might be their legal name, date of birth, address, licensing, professional qualifications, or citizenship.


A digital identity credential packages one or more of those attributes, so that the holder can present them to another party. For example, a mobile drivers’ license can confirm that a person is licensed to operate a motor vehicle without disclosing other attributes, like their birth date or home address.


Many digital ID systems use both authentication and identity verification. Identity verification determines whether the identity in the document is connected to a genuine identity. Authentication determines whether someone may access an account, using methods like PIN, password, or biometrics.


## The different types of digital IDs


### Government-issued digital IDs


Common examples of government-issued digital ID are mobile drivers’ licenses, national digital identity accounts, electronic identity cards, and digital travel credentials (DTCs) like a passport.


Some of these use a centralized identity provider. For these, the user completes a step like entering a PIN or one-time password; the provider then returns normalized identity data along with proof that the check occurred. Others, like electronic passports and some national identity cards, rely on a chip embedded in a physical document. When the user scans the document with NFC, the verification system decrypts the chip, checks the digital signature, compares it to an authoritative trust registry, and returns the verified data.


Standard-based systems are used to improve interoperability. ICAO Doc 9303 is the main standard behind machine-readable passports and travel documents. The standard defines how identity data and biometric information are stored and protected on the chip.


Government-issued credentials have high-value source data, because the issuing authority has already established the identity of the holder. Their usefulness, however, still depends on document support, local policy, reader availability, and whether the holder has the ability to complete the required digital process. For example, a tablet without NFC would prevent someone from using their chip-enabled passport. Mobile drivers’ licenses follow a related but distinct model. The credential lives on the holder’s device and is presented to a reader in person or through comparable methods for online use. The credential is verified against an issuing state’s certificate. These processes are governed by ISO/IEC 18013-5 and 18013-7.


### Digital identity wallets


A digital identity wallet is used to store digital credentials on a phone or other device. The wallet holder approves when a credential is shared, and which attributes the requester receives.


Examples include US state IDs held in Apple Wallet or Google Wallet, as well as the European Union Digital Identity Wallet. These wallets support selective disclosure, so, for example, a business can receive confirmation that a customer meets age requirements without requiring that customer to disclose their full identity record.


The European Union is making the wallet model central to their digital identity policy. Under eIDAS 2.0, member states must provide an EU Digital Identity Wallet by the end of 2026. The framework is designed to support public services as well as regulated private sector use across borders.


Wallets do not perform every trust function by themselves. The verifier still needs to confirm that the credential came from an approved issuer and that the person presenting it is the legitimate holder of the credential.


### Enterprise digital IDs


Enterprises in the private sector, like banks, employers, universities, and healthcare providers, also issue digital identities. Generally, these credentials are narrower than national IDs and apply to a specific service or relationship.


For example, a bank may create a customer identity after completing know-your-customer (KYC) checks, an employer might issue a credential that confirms an employee’s role and access rights, or a university could issue proof of enrollment.


With enterprise credentials, their value depends on their recognition. A verifier will need to be able to identify the issuer, validate the digital signature, determine the credential exists, and confirm that it hasn’t expired or been revoked. This is only possible with technical compatibility standards and a shared trust framework.


### Decentralized identity and verifiable credentials


In decentralized identity systems, the credential exists separate from a permanent account held by one centralized provider. Instead, an issuer creates a digitally signed credential; its holder stores it in a wallet, and a verifier checks the credential when presented.


W3C Verifiable Credentials provides a common data model for these signed credentials. With it, decentralized identifiers can identify the issuers, holders, or other entities without needing to route every interaction through the same identity provider.


With this model, holders have more control over how their credentials are stored and presented. But it also shifts more responsibility to the verifier, who must know whether the issuer is legitimate and whether the credential was presented by its actual holder, as well as whether it’s expired or revoked.


This dependence on governance partially explains why decentralized identity adoption has moved slowly, despite the existence of standards. The tech still needs issuer rules, trust registries, revocation processes, and wallets that everyday users can understand.


## Major digital identity standards & schemes


Without standards, every issuer, wallet, and verifier would need a separate integration, making the use of digital IDs unwieldy for all but the largest organizations. Standards make it easier for systems to read and validate credentials issued by another organization. Major standards include:


- ICAO Doc 9303, discussed earlier, is the main standard behind machine-readable passports and travel documents.
- ISO/IEC 18013-5 defines the core model for mobile drivers’ licenses, including the in-person presentation of these credentials on a mobile device in front of a reader. This model is extended for remote use with ISO/IEC 18013-7, which provides standards for using a mobile drivers’ license during an online interaction.
- W3C Verifiable Credentials is a standard that defines the way a digitally signed credential expresses claims about a person or organization.
- OpenID for Verifiable Presentations is used to define how a wallet responds to a request and sends those credentials to a verifier.
- eIDAS 2.0 provides legal and trust framework used for European identity wallets.
- NIST Digital Identity Guidelines in the US are used for federal and private sector identity proofing, authentication, and federation.


Each of these standards addresses a different part of the process, so a verification platform may need to support more than one of them to be functional.


## Global adoption snapshot


### European Union


The EU is pursuing a coordinated, wallet-based model. eIDAS 2.0 requires member states to provide interoperable wallets. It also creates common rules for credential issuance, electronic attestations, and cross-border recognition.


This type of regulatory structure gives relying parties like banks and public agencies a clear implementation path. It also creates requirements for wallet providers, credential issuers, and organizations that must accept digital credentials.


---


#### Understanding eIDAS 2.0: What European organizations need to know


Europe is leading one of the world's most ambitious digital identity initiatives through the Electronic Identification, Authentication and Trust Services (eIDAS) regulation. While the original framework established trusted electronic identities and digital transactions across the EU, eIDAS 2.0 expands those capabilities by introducing the European Digital Identity (EUDI) Wallet.


By December 2026, every EU member state must make an EUDI Wallet available to its citizens. Beginning in 2027, many regulated organizations, including banks, healthcare providers, telecommunications companies, government agencies, and Very Large Online Platforms (VLOPs), will be required to accept these wallets during customer interactions.


For organizations doing business in Europe, eIDAS 2.0 is becoming a key milestone in the evolution of digital identity.


eIDAS (2014) eIDAS 2.0


Established a framework for electronic identification and trust services Introduces the European Digital Identity (EUDI) Wallet


Focused on cross-border recognition Creates a standardized digital identity ecosystem across the EU


Covered electronic signatures and certificates Expands trust services and qualified attestations of attributes


Limited commercial adoption requirements Requires acceptance by regulated industries and VLOPs


Basic interoperability Strengthens interoperability through updated technical standards


#### The technology behind eIDAS


The legal framework is supported by technical standards developed by the European Telecommunications Standards Institute (ETSI). One of the most significant is ETSI TS 119 461, which establishes requirements for remote identity proofing and introduces multiple Levels of Identity Proofing (LoIP) to support high-assurance digital identities across the EU.


For many organizations, particularly financial institutions, these standards will become the technical foundation for interoperable, cross-border identity verification.


#### eIDAS 2.0 Timeline


Year Milestone


**2014** Original eIDAS regulation adopted


**2024** eIDAS 2.0 entered into force


**2025–2026** Member states prepare EUDI Wallet infrastructure


**December 2026** All EU member states must issue EUDI Wallets


#### Why eIDAS 2.0 matters so much


eIDAS 2.0 represents more than a compliance update. It establishes a common digital identity framework that enables citizens to securely prove who they are across borders while giving organizations a trusted, standardized way to verify identities. As adoption accelerates, businesses that prepare early will be better positioned to simplify onboarding, reduce fraud, and meet evolving regulatory requirements.


---


### United States


In the US, governance is mostly at the state level. Individual states can issue mobile drivers’ licenses, choose wallet partners, and create their own acceptance policies.


While standards like ISO 18013 for mobile drivers’ licenses and NIST guidance for identity proofing and authentication are commonly relied on in the US, adoption of the technology is uneven and the experience varies by state and use case.


### Australia


The Digital ID Act of 2024 established rules in Australia for accreditation, privacy, and governance for digital IDs. The government plans to expand its national Digital ID System to states and territories with a phased rollout.


The Australian approach offers national rules but also supports multiple approved identity providers, providing a less rigid structure than the EU wallet mandate but more federal guidance than the US-based mobile drivers’ license system.


### Canada


Canada is developing its digital credential infrastructure with a unified sign-in service for government access. The Canadian model emphasizes the ability to reuse verified information across public services,and privacy features like consent and selective disclosure.


Implementation across Canada is also uneven, with national, provincial, and private sector organizations developing at different speeds.


### Asia-Pacific


Several established examples of national identity systems exist in the region, along with a growing number of other wallet and credential programs. For example, Singpass, in Singapore, is a centralized national identity platform, and other countries are testing mobile documents, reusable credentials, and private wallets.


## Why organizations should pay attention


Digital IDs provide a route for replacing repeated document capture with verified data. For financial institutions, that can dramatically streamline the account opening process. Age-restricted services implementing digital identity verification also may see a significant reduction in the amount of personal data they collect, reducing their data governance burden. And in the public sector, governments with centralized digital identity can reuse identity checks across departments and branches.


In addition to these efficiency benefits, digital identity provides organizations with more fraud defenses. Generative AI has made the creation of synthetic documents and manipulated images and video faster and easier for fraudsters. Digitally signed credentials provide organizations with another, stronger way to confirm identity data.


This technology enhances, rather than replaces, the use of traditional identity verification tools. Organizations will still need to enroll users, provide support for customers who are not using digital identity tools or who don’t have a compatible wallet, assess credential presentation risks, and connect a credential to the live person using it.


Mitek supports those steps with document capture, document authentication, biometric matching, liveness detection, and identity verification workflows. Organizations that combine these tools with digital identity verification can accept newer digital credentials while retaining their support for existing onboarding methods and physical documents.


## What challenges still exist?


The main challenge to digital identity adoption is fragmentation. Countries and states are adopting different wallet architectures, credential formats, trust lists, acceptance rules and more.


Interoperability also remains a challenge, even when two systems are using the same standard, if those technical specifications have been interpreted differently. Optional features and local policy decisions can also create problems.


User adoption is also still limited. Users will expect using a digital ID to be secure and as seamless to use (or even easier) than physical documents, and will need broad education on the security of digital identity systems to make them look like a more appealing alternative. Broad acceptance will also be key to user adoption. Credentials will have limited appeal to users if they are only accepted in a single pilot program or one specific government office.


Cross-border recognition will be another significant challenge to widespread adoption. Governments will need to agree on issuer trust, liability, revocation, data protection, and the level of assurance for any particular credential that is used globally.


Implementation costs can slow the rollout of digital ID initiatives, but political priorities and public trust often prove to be even greater obstacles. The UK offers a clear example. After initially proposing an ambitious national digital ID program, the government first scaled back the plans amid concerns over privacy, civil liberties, implementation complexity, and cost, before a subsequent administration announced it would scrap the initiative altogether in favor of other policy priorities. The episode highlights how digital ID adoption depends as much on public confidence and political will as it does on funding.


## The road ahead


Digital ID is maturing, and moving out of isolated programs into more robust, regulated infrastructure, as can be seen with the EU’s wallet deadline, continued development of US mobile drivers’ licenses, and other global movement toward standards.


These programs, though, are taking different technical paths. Global organizations planning for the future of digital ID acceptance should expect a mixed identity environment where centralized identity providers, chip-based documents, mobile IDs and verifiable credentials will all be operating at the same time.


Practically, the goal for organizations should be to support multiple trusted identity processes, through one verification process. Organizations that build for that reality will be able to accept new credentials as they become available, apply their fraud controls consistently, and avoid rebuilding new system architecture whenever there is a new regional standard.


Businesses that begin preparing now will be better positioned as digital identity ecosystems continue to mature. While adoption will vary by region, initiatives like the European Union's eIDAS 2.0 framework and Digital Identity Wallets are setting the pace for how individuals will securely prove who they are online in the years ahead. Understanding these developments today can help organizations make more informed decisions as digital identity moves from emerging technology to everyday reality.
