---
schema_version: "1.0.0"
document_id: "8861348c54448522bc96beffc456d55a9e4c7316308152bb189f2500a430cc57"
company_key: "ciso-global-inc-common-stock"
company: "CISO Global Inc."
source_id: "ciso-global-inc-common-stock-news-import-3c300a5ba5f3"
canonical_url: "https://www.ciso.inc/blog-posts/what-data-should-employees-keep-out-of-ai-tools/"
published_at: "2026-08-11T03:00:00+00:00"
first_seen_at: "2026-08-11T12:09:54.839120+00:00"
fetched_at: "2026-08-11T12:09:55.989043+00:00"
content_hash: "sha256:542bd6cb1cea84118a68fdd8a16a24b1a8b69d41bd676401f8d8748db3efad08"
---

# Data to Keep Out of AI Tools | CISO Global

###### *Tom Coffey, VP, Information Security*


August 11, 2026


### “ *Paying for a subscription does not automatically prevent the provider from storing company information or using it to train its models.* “


### Key Takeaways


- Do not enter sensitive data into an AI tool unless the organization has approved both the platform and that type of data.
- Common high-risk data includes credentials, personal information, confidential business records, proprietary code, legal documents, and detailed security findings.
- Use fictional or sample data whenever possible and follow company-approved procedures for de-identifying information.
- When the rules are unclear, stop and ask the security, privacy, legal, or compliance team.


Employees use standalone AI assistants, coding copilots, and AI features built into everyday workplace tools to draft, summarize, analyze, and troubleshoot. A single prompt or upload containing customer records, source code, legal material, or security findings can create privacy, contractual, and cybersecurity risks when the organization has not approved the tool or that type of data.


Before sharing company information, confirm that the organization has approved both the platform and that type of data.


[OWASP](https://genai.owasp.org/llmrisk/llm022025-sensitive-information-disclosure/) lists personally identifiable information, financial and health records, security credentials, confidential business data, and legal documents as information that large language model applications may expose. The[Federal Trade Commission](https://www.ftc.gov/policy/advocacy-research/tech-at-ftc/2024/01/ai-companies-uphold-your-privacy-confidentiality-commitments) has also warned that users may reveal internal documents and customer data while working with AI providers.[NIST](https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence) recommends managing risk throughout the AI lifecycle, including how organizations use AI tools.


### Approved AI Tools Still Have Data Limits


Organizations should treat free or consumer AI accounts as public unless they have reviewed and approved the provider’s terms, settings, and data practices. Paying for a subscription does not automatically prevent the provider from storing company information or using it to train its models. Before approving an enterprise platform, organizations should look for explicit commitments that company data will remain within the approved tenant or environment and that the provider will not use prompts, uploads, or responses to train its models.


An organization may approve one tool for public marketing content but not customer records, and another for internal documents but not regulated data. It should define which data employees may use in each tool.


### 1. Passwords, Credentials, and Security Secrets


Employees should never enter passwords, authentication codes, private encryption keys, API keys, access tokens, recovery codes, or answers to security questions into an AI tool.


These details can provide access to company systems or help someone bypass security controls. Do not assume a credential is safe to share simply because it expired or the organization rotated or retired it.


Configuration files and code may also contain embedded credentials or other secrets. Employees should remove credentials and other sensitive values before submitting code to an approved AI coding assistant.


### 2. Customer and Employee Information


A name is personal information. Combined with account details, health information, or employment records, it can reveal much more.


Keep the following data out of public or unapproved AI tools:


- Social Security numbers
- Driver’s license or passport information
- Banking and payment details
- Medical or insurance information
- Employee performance records
- Background check results
- Customer account information
- Contact lists containing private information


Privacy laws, contractual requirements, or internal handling rules may govern this data. Removing a person’s name may not be enough if other details can still identify them.


When using an approved AI tool to review a document or dataset, use fictional or sample data whenever possible. Only use de-identified information when the organization has prepared it according to company policy.


### 3. Confidential Company Information


Employees frequently use AI to revise reports, summarize meetings, or prepare presentations. Those tasks become risky when the source material contains nonpublic company information.


Confidential information may include:


- Financial projections, internal budgets, and nonpublic results
- Sales pipelines, pricing, and customer terms
- Business plans, product roadmaps, and expansion proposals
- Customer and vendor contracts
- Board materials and internal meeting notes
- Merger discussions and other unannounced business developments


A document does not need to carry a “confidential” label to be sensitive. Employees should consider who normally has access to the information and what could happen if it reached a competitor, customer, or the public.


### 4. Proprietary Code and Intellectual Property


Employees should not upload proprietary source code, algorithms, technical designs, unpublished research, product specifications, or trade secrets unless the organization has approved the platform for that purpose.


Even a small code sample may reveal how an application works, which third-party systems it uses, or where security weaknesses might exist. Developers should use sample code that reproduces the problem without including proprietary code, credentials, or other sensitive information.


In 2023,[Samsung](https://techcrunch.com/2023/05/02/samsung-bans-use-of-generative-ai-tools-like-chatgpt-after-april-internal-data-leak/) temporarily restricted the use of generative AI tools on company devices and internal networks after employees uploaded sensitive internal code and other company data to ChatGPT.


### 5. Legal, Human Resources, and Financial Records


Do not use public or unapproved AI tools to review or summarize:


- Attorney-client communications and litigation strategies
- Draft agreements
- Employee complaints, investigations, and disciplinary records
- Payroll and tax records
- Audit findings
- Nonpublic financial statements


Sending legal material to an outside platform may also create questions about confidentiality or privilege. Employees should consult legal counsel before using an AI tool for legal documents or case-related information.


### 6. Detailed Cybersecurity Information


Do not enter detailed security information into a public or unapproved AI tool.


Restricted security information may include:


- Vulnerability scan and penetration testing reports
- Incident response records and active investigations
- Network diagrams and security architecture
- Firewall rules and system configurations
- Logs containing user or system data
- Details about unpatched vulnerabilities


A prompt asking for help prioritizing vulnerabilities may expose asset names, IP addresses, software versions, and known weaknesses. Those details could help an attacker identify valuable systems or likely entry points.


### What to Do After Sharing Sensitive Data


Stop using the AI tool for that task and report the disclosure as soon as possible to the appropriate security, privacy, legal, or compliance contact. Include the name of the tool, the account used, when the disclosure occurred, and the type of data involved.


Do not assume deleting the conversation resolves the issue. Follow the instructions you receive rather than trying to handle the disclosure yourself.


### Check Before You Share Data


Before using company information in an AI tool, employees should ask:


1. Has the organization approved both this tool and this type of data?
2. Does the information contain personal, confidential, regulated, or proprietary data?
3. Can I complete the task using fictional or sample data, or information de-identified according to company policy?
4. Would sharing this information with an outside provider violate a policy, contract, or customer expectation?


When the answer is unclear, employees should stop and ask the security, privacy, legal, or compliance team.


AI policies should not force employees to guess. Organizations need to identify approved platforms, define prohibited data, explain acceptable use, and reinforce those expectations through[employee security awareness training](https://www.ciso.inc/capabilities/security-awareness-training/) .


CISO Global helps organizations assess AI-related risk, develop[acceptable use policies](https://www.ciso.inc/capabilities/strategy-risk-solutions/) , and set data-handling rules employees can follow.[Talk with a CISO Global security expert](https://www.ciso.inc/request-a-consultation/) about reducing the risk of sensitive data exposure through AI tools.


[Learn How CISO Global Can help protect your business](https://www.ciso.inc/company/contact-us/)
