---
schema_version: "1.0.0"
document_id: "2ccde83e54cf58e259ed60eb1beefd55ce474b20f9e24135fc6214ae7ab96305"
company_key: "yc-onefin"
company: "OneFin"
source_id: "yc-onefin-rss-7ca07195f9bf"
canonical_url: "https://www.onefin.in/post/mythos-and-the-shifting-cybersecurity-threat-for-india-s-lenders"
published_at: "2026-04-30T12:08:35+00:00"
first_seen_at: "2026-07-20T23:20:39.909173+00:00"
fetched_at: "2026-07-28T22:15:30.724879+00:00"
content_hash: "sha256:1c19a12089662c21032c79bb4ced7d45bf91d6cde29dad1b003623888462e27f"
---

# Mythos and the Shifting Cybersecurity Threat for India's Lenders

Recently, the RBI has had quiet consultations with the US Federal Reserve and the Bank of England. The subject was not monetary policy. On April 23, the Finance Minister chaired a meeting with RBI officials, bank chiefs, and MeitY representatives. The agenda was to assess the risks posed by Anthropic's Claude Mythos, a frontier AI model able to find and exploit software vulnerabilities on its own. She described the threat as unprecedented.


Key directives emerged from the meeting: the Indian Banks' Association was tasked with building a coordinated cyber response mechanism, banks were required to report suspicious activity immediately to CERT-In, and lenders were directed to take pre-emptive steps to secure systems and protect customer data. What made this moment significant was not the specific model. It was what the model signalled. For banks and NBFCs operating digital lending infrastructure in India, the cybersecurity calculus has changed in ways that go well beyond any single tool or incident.


##


**AI-Assisted Attacks: How the Landscape Has Changed**


The standard model of a cyberattack assumes a human attacker with limited time and a specific target. AI-assisted attacks break all three. Mythos can scan codebases without direction, find zero-day flaws across major operating systems and browsers, and run multi-step attack chains. A skilled human operator would need days to do the same work. The model can find and use vulnerabilities faster than institutions can patch them.


The more important point is not Mythos itself. It is under tight access control by Anthropic as of now. The point is what it signals. Each new generation of frontier AI lowers the skill, time, and cost needed to run a complex attack. India's NPCI is seeking early access to Mythos to find payment system flaws before bad actors do. That reflects a clear view that this is not a temporary spike in risk. The patch-lag problem, which lenders have always managed at acceptable cost, is becoming structurally dangerous.


##


**Geopolitical Escalation: An Added Dimension**


The AI threat does not operate in isolation. India's geopolitical context adds a layer of exposure that is specific to this market. The Pahalgam terror attack in April 2025 triggered an immediate cyber response. Within 48 hours, hacktivist groups launched campaigns against Indian digital targets. CERT-Maharashtra logged over 10 million intrusion attempts across DDoS floods, phishing runs, and exploit attempts. Authorities put BFSI on explicit high alert. Enterprises across the sector reported a 30-40% surge in security enquiries during the period.


A Pakistan-linked group APT36 deployed Crimson RAT malware through Pahalgam-themed phishing documents, targeting government and defence personnel. The broader lesson, though, is not about any single campaign. It is about timing. A geopolitical trigger now activates organised cyber operations within hours. For financial institutions, this compresses the response window to near zero. Planning for cyber incidents as occasional, isolated events is no longer a realistic posture.


##


**Vendors: How They Play a Role**


The preferred entry point into India's financial sector has shifted. In January 2025, the Bashe ransomware group claimed to have breached ICICI Bank's database through a third-party vendor portal, threatening to release customer data unless a ransom was paid. ICICI Bank has not confirmed the breach. In December 2024, the same group claimed to have stolen over 600,000 database records from Federal Bank. In both cases, the claimed entry point was a vendor or third-party access channel, not the institution's core systems.


###


**The Structural Exposure**


Consider a lender with co-lending arrangements and an out-hosted LOS vendor. AI-assisted tools scan the vendor's public-facing API endpoints, identify an unpatched vulnerability, and generate a working exploit (or execute a phishing campaign). Within hours, an attacker has read access to borrower KYC records and disbursement data through the shared integration. The lender has a board-approved cybersecurity policy, but operationalization is loose. By the time anything is flagged, the data has already moved.


This is not a failure of intent. It is a structural gap. Automated vulnerability discovery can probe dozens of vendor integrations simultaneously, far faster than any manual audit cycle.


##


**The Regulatory Response: What Is Already in Effect**


The RBI has moved on multiple fronts. Three developments are directly relevant to lenders as of April 2026:


-


**Authentication Directions (effective April 2026):** Dynamic two-factor authentication is now mandatory for all non-recurring digital payments. Real-time risk scoring is required at the transaction level. Institutions are liable for losses from authentication design failures, not just from breaches. Accountability has moved into architecture decisions.


-


**Data Protection Advisory (April 2026):** Board-level approval is now required for data security policies. Quarterly or semi-annual board reviews of security incidents are mandated. Controls must be in place where AI systems or chatbots handle customer data.


-


**AI Enterprise Partnership Guidelines (developing):** The RBI is developing rules for institutions that partner with advanced AI models. Data localisation is expected to be a key condition. Formal directions have not yet been issued.


The IT Master Directions for NBFCs create tiered obligations. More requirements apply above the Rs 500 crore AUM threshold. In practice, many lenders have approved policies but lack the operational infrastructure those policies assume: continuous monitoring, vendor audit protocols, and a tested incident response process.


##


**What Lenders Should Actually Do**


The gap between policy and operational readiness is especially acute for NBFCs, which typically run leaner IT and security teams than scheduled commercial banks. Five areas warrant immediate attention:


1.


**Map the vendor attack surface:** Identify every third-party integration that carries access to borrower data. This includes LOS and LMS vendors, account aggregator connectors, co-lending APIs, and KYC providers. Require SOC2 or ISO 27001 attestations from critical vendors.


2.


**Establish a real patch cadence:** Determine how long it takes your institution to patch a known critical vulnerability, end to end. If the answer exceeds 72 hours, that is a structural exposure. AI-assisted discovery is shortening the window attackers need to act on known flaws.


3.


**Extend penetration testing to vendor integrations:** Most institutions run annual pen tests on their own infrastructure. Far fewer include vendor APIs and third-party access channels in scope. Given that the vendor supply chain is now the primary attack entry point, this is a material gap.


4.


**Move from policy to playbook:** A board-approved cybersecurity policy satisfies the regulatory minimum. A tested incident response playbook limits damage when an attack succeeds. Run tabletop exercises at least twice a year.


5.


**Know your breach detection baseline:** The scenario most likely to affect lenders is not a dramatic system failure but a quiet, prolonged data access through a compromised vendor channel. Most institutions have no formal way to measure how long it would take to detect this. Establish a baseline: how are anomalous API calls flagged, who reviews them, and within what timeframe?


##


**OneFin: Secure Lending Infrastructure**


The lending stack is itself a security surface. Decisions about API design, vendor links, and data flow between co-lending partners are security decisions. A fragmented stack with disconnected integrations widens the vendor attack surface. A purpose-built platform narrows it.


OneFin's LOS and LMS are built with this in mind:


-


**ISO 27001:2022 certified.** OneFin holds a verifiable certification and a zero data breaches record across clients managing over Rs 20,000 crore in loan disbursals.


-


**Bank-grade encryption and access controls.** Regular testing and independent audits find and fix vulnerabilities. We do not wait for incidents to surface them.


-


**Managed API integration layer.** OneFin covers 70+ external services within a single governed perimeter. Lenders manage one security boundary, not dozens of separate vendor connections to audit and monitor.


-


**Role-based access controls and full audit trails.** Granular visibility into who accessed what and when supports both internal governance and regulatory inspection readiness.


For lenders preparing for the RBI's developing AI partnership guidelines, infrastructure baseline matters. Regulatory readiness starts with the systems that hold borrower data.


##


**Conclusion**


Mythos is a signal, not the story. AI capability has crossed a threshold in autonomous vulnerability discovery. Geopolitical tensions now trigger financial sector cyber campaigns within hours. Vendor supply chains have become the preferred attack entry point into BFSI. The RBI's response confirms that the regulatory floor is rising on all three fronts. Banks and NBFCs that treat cybersecurity as a compliance formality will find it reclassified as a board-level risk exposure. That reclassification will happen on the regulator's timeline, not theirs.


To know more about OneFin


,


[schedule a Demo](https://www.onefin.in/request-demo) .
