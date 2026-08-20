---
schema_version: "1.0.0"
document_id: "fce052226a401d10d5e149346993ca5eb119daf983aa42a1aa87b9abb073db53"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/ai/the-attribution-gap-ai-regulation/"
published_at: "2026-07-14T07:00:00+00:00"
first_seen_at: "2026-07-22T07:05:37.626494+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:199e8a8a0ff8601a5135c50c0847448b7ee604610563f5f063a95b1a4e7f1cc0"
---

# The attribution gap: Why every AI regulation leads back to identity and authorization

### Topics


---


Cybersecurity


,


AI Agents


### Table of Contents


---


---


### Share


-
-
-


---


Ready to make Identity a business advantage? Sign up today.


[Get started](https://www.okta.com/free-trial/)


**Executive summary:** Today, autonomous AI agents are approving loans, giving legal advice, triaging patients, and controlling physical systems. When an AI agent causes operational, financial, or compliance harm, courts and regulators demand a clear audit trail showing who authorized the agent and what specific actions were permitted. Currently, most enterprises lack the identity infrastructure to produce this verification, creating a severe liability known as the AI attribution gap.


## Understanding the AI attribution gap


The attribution gap is the critical distance between an AI agent’s actions and your ability to prove who authorized it and what it was permitted to do. This governance deficit maps directly to the regulatory text of major frameworks across global jurisdictions.


### Active global AI compliance frameworks


Six out of eight existing compliance frameworks are fully active and enforceable today:


- **Sarbanes-Oxley Act (SOX):** Requires explicit internal access controls over automated financial reporting and ERP entries.
- **California Consumer Privacy Act (CCPA):** Provides a private right of action with statutory damages of $100 to $750 per consumer per incident for data breaches resulting from inadequate security.
- **Securities and Exchange Commission (SEC) cybersecurity disclosure rules (Form 8-K, Item 1.05):** Mandates disclosure of material cybersecurity incidents within four business days of determining materiality, which can include incidents involving non-human identities.
- **General Data Protection Regulation (GDPR):** Requires a documented lawful basis and data minimization for any automated processing of personal data.
- **Network and Information Systems Directive (NIS2):** Mandates comprehensive cybersecurity risk-management measures and supply chain security across critical infrastructure sectors, including access control policies that in practice must extend to non-human identities.
- **Digital Operational Resilience Act (DORA):** Imposes digital operational resilience requirements on the financial services sector, including ICT risk management and continuous oversight of third-party ICT providers, which in practice demands verifiable authorization records.


Meanwhile, the European Union Artificial Intelligence Act (EU AI Act) high-risk requirements take effect on December 2, 2027, for standalone systems and August 2, 2028, for AI embedded in regulated products, mandating automatic event logging, risk management, and human intervention capabilities.


Concurrently, US state legislatures are rewriting state AI laws in real time. As of February 2026,[78 AI chatbot bills](https://ourtake.bakerbotts.com/post/102mipe/ai-chatbot-regulation-78-state-bills-58-lawsuits) have been filed across 27 US states. The evolution of the Colorado AI Act, the first comprehensive US state AI law, demonstrates how fluid AI-specific legislation is: A federal court stayed Colorado's AI Act in April 2026, and a[narrower transparency law](https://www.hunton.com/privacy-and-cybersecurity-law-blog/colorado-ai-act-amended-and-effective-date-delayed) scheduled to take effect January 1, 2027, has since replaced it. While the specific requirements changed, the intent did not; even the narrowed law demands disclosure and record-keeping that trace back to identity and authorization.


### Five identity controls for closing the attribution gap


To protect your business, every agent action must trace back to an accountable human. This requires five core controls:


1. **Identify** which agent acted.
2. **Limit** what it can access.
3. **Trace** the authorization to a named human.
4. **Verify** permissions before data moves.
5. **Log** everything immutably.


## The legal evidence: Four case studies, one pattern


Courts are rapidly closing the escape routes. Recent rulings have allowed product liability claims against AI makers, rejected the defense that a chatbot is a separate legal entity, and levied multi-million dollar fines for unauthorized data processing. These cases highlight the severe operational and financial consequences of the AI attribution gap:


### Case 1: Nippon Life v. OpenAI (March 2026)


A landmark lawsuit alleges ChatGPT engaged in the unlicensed practice of law by advising a disability claimant to fire her attorney, assisting in drafting legal motions, and[citing fictitious case law](https://natlawreview.com/article/lawsuit-alleges-ai-chatbot-engages-unauthorized-practice-law) . Based on this automated guidance, the claimant used the tool to draft and file multiple legal documents in a suit seeking to undo a previous legal settlement. Consequently, Nippon Life is seeking millions of dollars in damages to offset the costs of defending against these AI-generated filings.


**The attribution gap:** Who authorized the agent to give legal advice? No corporate identity system recorded the answer.


### Case 2: Garcia v. Character Technologies (May 2025)


A federal court in Florida allowed[a product liability claim](https://www.darrow.ai/resources/character-ai-lawsuit) against an AI chatbot maker to proceed, rejecting the maker's First Amendment defense. Google, which provided the underlying LLM, could be liable as a component part manufacturer. This established that if an AI chatbot is a product, the deploying company owns every automated output and needs a comprehensive audit trail: which agent, which authorization, which human, and which scope.


**The attribution gap:** Without an identity control plane, the enterprise can’t prove which specific human supervisor configured the agent’s behavioral guardrails.


### Case 3: Moffatt v. Air Canada (February 2024)


Air Canada's AI-powered chatbot told a grieving passenger that he could apply for a retroactive bereavement discount, despite company policy. The company argued the AI chatbot was a separate legal entity. The[British Columbia tribunal rejected this](https://www.cbc.ca/news/canada/british-columbia/air-canada-chatbot-lawsuit-1.7116416) :


*While a chatbot has an interactive component, it is still just a part of Air Canada's website. It should be obvious to Air Canada that it is responsible for all the information on its website.*


**The attribution gap:** The chatbot lacked a unique identity, an authorization record, or an access trail linking its real-time outputs to a responsible owner.


### Case 4: Italy v. Replika / Luka Inc. (2025)


Italy's data protection authority[fined Replika's parent company EUR 5 million](https://www.edpb.europa.eu/news/national-news/2025/ai-italian-supervisory-authority-fines-company-behind-chatbot-replika_en) for three GDPR violations:


- Failing to identify the lawful basis for processing user data
- Providing an inadequate privacy policy
- Not deploying age verification.


Proper identity governance and access controls would have mitigated each of these compliance failures.


**The attribution gap:** The automated system operated completely outside the corporate identity boundary, executing data processing tasks without validating permissions.


These four cases are not outliers.[Kentucky became the first US state](https://www.kentucky.gov/Pages/Activity-stream.aspx?n=AttorneyGeneral&prId=1857) to sue an AI chatbot company. The Netherlands[fined Clearview AI EUR 30.5 million](https://data-privacy-office.eu/fines-for-gdpr-violations-in-ai-systems-and-how-to-avoid-them/) . NYC's[MyCity AI chatbot](https://www.cxtoday.com/contact-center/3-times-customer-chatbots-went-rogue-and-the-lessons-we-need-to-learn/) told business owners to break the law.


The pattern is the same across these incidents: An agent acts, harming a user or violating a policy. The enterprise cannot produce a trail showing who authorized the agent, what it was permitted to do, or how to attribute its actions to an accountable human. The courts establish liability, and regulators specify the necessary controls. That space is the attribution gap.


The attribution gap. The left side represents what agents do. The right side represents what enterprises need to prove. The chasm between them is where regulatory, legal, and operational risk live.


Fill out the form to access this content.


## How global regulations enforce AI identity and authorization


Your regulatory risk exposure is determined by your security team’s ability to verify four critical operational pillars.


### 1. Which agents are running across your environment? (identity)


Under[Article 9](https://artificialintelligenceact.eu/article/9/) of the EU AI Act, violations of high-risk requirements carry fines of up to EUR 15 million or 3% of global turnover, with the top tier of EUR 35 million or 7% reserved for prohibited AI practices. The law requires the establishment and ongoing update of a continuous risk management system for high-risk AI systems. You cannot risk-manage an agent you cannot identify.


[Article 15](https://artificialintelligenceact.eu/article/15/) mandates strict resilience against exploitation; running an unmapped agent on a shared service account creates a lateral movement vector that invalidates compliance.


Similarly, the[COSO GenAI Guidance](https://www.prnewswire.com/news-releases/coso-releases-practical-roadmap-for-managing-generative-ai-risks-and-controls-302694327.html) released on February 23, 2026, directly impacts SOX Sections 302, 404, and 906, which require executive officers to personally certify internal financial controls. An unmanaged AI agent utilizing a shared service account to access an ERP system and post journal entries constitutes a material weakness. Access controls must now be applied systematically to digital workers, not just human employees.


### 2. What specific data access footprint do your agents have? (scoping)


[Article 32](https://gdpr-info.eu/art-32-gdpr/) of the GDPR imposes fines up to EUR 10 million or 2% of global turnover for failing to implement appropriate technical security measures. If an AI agent possesses broad access privileges and retrieves confidential data on behalf of an end-user who lacks corresponding permissions, it constitutes unauthorized data processing. Security teams must verify the access rights of both the agent and the human user before data moves. In 2025,[this exact pattern triggered](https://www.okta.com/blog/ai/ai-agent-authorization-gap/) four critical vulnerabilities (CVSS 9.3–9.4) across the Anthropic, Microsoft, ServiceNow, and Salesforce ecosystems.


Furthermore, CCPA[§1798.150](https://leginfo.legislature.ca.gov/faces/codes_displayText.xhtml?division=3.&part=4.&lawCode=CIV&title=1.81.5) permits statutory damages of $100 to $750 per consumer per incident, backed by a private right of action that allows consumers to sue directly. Because autonomous agents operate at machine speed, an un-scoped agent can over-retrieve tens of thousands of personal data records in minutes. At statutory minimums, an ingestion error exposing 40,000 corporate records triggers an immediate $4 million liability.


### 3. Who authorized that specific access footprint? (attribution)


[Article 12](https://artificialintelligenceact.eu/article/12/) of the EU AI Act requires high-risk AI systems to be technically capable of automatically recording events throughout their operational lifetime. A basic workflow log merely reports what occurred; an audit trail must prove which human identity granted the authority and when that access expires.


COSO Principle 8 explicitly warns that fraud risks are accelerated by AI agents that introduce authorization risks, excessive agency, and insecure interfaces.


This creates a dangerous vulnerability known as[recursive delegation](https://www.okta.com/blog/ai/agent-security-delegation-chain/) , where permissions fail to narrow at each autonomous hop. Line-of-business teams routinely integrate shadow AI agents into SAP, Oracle, and Workday environments via generic service accounts with no unique non-human identity or delegation chain. When a system failure occurs, tracing the root action to an accountable human is impossible.


This exposure is magnified under[SEC cybersecurity disclosure rules](https://www.sec.gov/newsroom/press-releases/2023-139) , which require material incidents to be fully disclosed within four business days; without an identity trail, your regulatory disclosure is fundamentally incomplete.


### 4. Can you revoke an agent’s access privileges right now? (human oversight)


[Article 14 of the EU AI Act](https://artificialintelligenceact.eu/article/14/) mandates that organizations maintain the technical capability to intervene in high-risk AI operations or to immediately interrupt the system via a designated stop button or similar safety procedure. A long-lived OAuth token that cannot be invalidated mid-task does not fulfill this requirement. True human oversight requires a unified platform capable of pausing an action before execution, revoking credentials in flight, and enforcing real-time policy decisions at the access point.


## Solving the AI ephemerality and accountability problem


AI agents are not legal persons; they cannot be deposed, held in contempt, or testify to their intent. Unlike a human employee who can be interviewed six months after an incident, an AI agent is ephemeral—it spins up to execute a task and shuts down immediately. If your system doesn’t assign a unique identity to non-human identities and capture an immutable audit trail while the agent is running, the legal evidence vanishes permanently.


We saw this play out in the Air Canada case mentioned above. When the company argued its chatbot was a separate legal entity, the tribunal rejected that defense. But the ruling exposed a deeper problem: Air Canada’s chatbot had no unique identity, authorization record, or trail connecting its output to a human. The company could not prove who configured the agent, what policies governed it, or who was accountable. That is the accountability gap that identity and authorization close.


Enterprises deploy these ephemeral agents as digital workers across high-consequence decision streams: rebooking passengers, adjusting insurance, triaging intake, and approving credit. As we explored in[part five of this series](https://www.okta.com/blog/ai/ai-agents-cyber-physical-iam-safety/) , when an agent controls physical systems—such as HVAC, facility doors, and dosage recommendations—the real-world consequences amplify.


When an agent’s action is challenged, the justice system demands an accountable human. Identity defines which agent acted, and authorization defines what it was permitted to do. Without both, your digital workforce operates with profound, unmanaged risk.


## The friction fallacy in automated governance


When evaluating non-human identity security controls, executive leadership teams often raise a common objection: “We don’t want to add friction and slow down our AI adoption.”


This is the wrong framing. The true cost of friction is felt when an ungoverned agent makes headlines: An agent exfiltrating corporate data. An agent exposing protected health information (PHI). An agent giving inaccurate legal advice to thousands. An agent adjusting medication dosages from records it was never authorized to access. Every one of these is one control layer away from being contained.


Air Canada's AI chatbot gave one passenger the wrong answer, and it became a[landmark AI liability ruling](https://www.americanbar.org/groups/business_law/resources/business-law-today/2024-february/bc-tribunal-confirms-companies-remain-liable-information-provided-ai-chatbot/) . That was a simple AI chatbot. The AI agents enterprises deploy today have far broader access, far more systems, and far greater speed.


While the friction of implementing the authorization checks required to secure an agent is measured in milliseconds, the friction of a data breach, a regulatory investigation, a class-action lawsuit, or an SEC enforcement action is measured in months of litigation, millions in fines, and damaged executive careers. Deploying autonomous agents without identity controls does not eliminate friction; it merely defers it to the worst possible moment.


## The clock is ticking


The attribution gap is not a theoretical compliance exercise. Six major regulatory frameworks are imposing penalties today.[COSO](https://www.prnewswire.com/news-releases/coso-releases-practical-roadmap-for-managing-generative-ai-risks-and-controls-302694327.html) published its AI internal controls guidance on February 23, 2026. Public Company Accounting Oversight Board ([PCAOB](https://pcaobus.org/news-events/news-releases/news-release-detail/pcaob-staff-report-outlines-2025-inspection-priorities-with-focus-on-driving-improvements-in-audit-quality) ) inspection plans explicitly include GenAI. The[EU AI Act](https://artificialintelligenceact.eu/) takes full effect by December 2, 2027, and conformity assessments can take up to a year to complete. Organizations need to start preparing now.


You will face accountability regardless of whether you implement these controls. The only question is whether you will be able to demonstrate that accountability when regulators and courts come knocking.


## Bridge the attribution gap and secure your AI workforce


Your autonomous AI agents should act as secure, compliant business multipliers, not unmanaged liabilities. Securing this digital workforce doesn’t mean slowing down your technological roadmap. With Okta’s unified identity security fabric, you can establish an authoritative, identity-first control plane for every automated action across your enterprise.


Okta can help you[secure your agentic experience](https://www.okta.com/solutions/secure-ai/) and eliminate the attribution gap before regulators and courts come asking.[Speak with an Okta security expert](https://www.okta.com/contact-sales/) to get started preparing your enterprise for the future of automated governance today.


### What’s your AI readiness score?


Take our two-minute[AI Identity Assessment](https://www.okta.com/assessments/ai-readiness) to find out where you stand, uncover hidden gaps, and get actionable insights to close your vulnerabilities.


#####


Measure your AI readiness in 2 minutes


See where identity, access, and agent control break down in production


[Get your score](http://okta.com/assessments/ai-readiness)


About the Author


[Kundan Kolhe Sr Director, AI Product Strategy & Solutioning | Okta for AI Agents Building Okta’s agentic AI security business from zero to one, a $100M+ opportunity. Leads product strategy and solutioning for Okta’s AI Agents business, a CEO-level initiative defining how enterprises secure AI agents at scale before the market has fully settled. Leads a team of architects and partners with Fortune 500 security and IT executives across financial services, manufacturing, and technology. Shapes the agentic security product roadmap across Okta and Auth0. Brings two decades of experience across product, pre-sales, and marketing at startups and global enterprises across North America, the UK, Australia, and Asia.](https://www.okta.com/blog/author/kundan-kolhe/)
