---
schema_version: "1.0.0"
document_id: "4b84f9c7943ec74d09c26f2f5cc4c6a382e68ba1e0617c2ffd564c3fc9640e0c"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-e942a00c1738"
canonical_url: "https://www.paloaltonetworks.com/blog/2026/08/strengthening-security-of-ai-coding-prisma-airs-api-integration-with-openai-codex/"
published_at: "2026-08-07T16:00:44+00:00"
first_seen_at: "2026-08-12T22:12:37.028174+00:00"
fetched_at: "2026-08-12T22:12:39.592460+00:00"
content_hash: "sha256:2f899a0de29a49e7ff7e58cf0e92bcf7dfad14a74f3de78877640951b4ac8690"
---

# Strengthening Security of AI Coding: Prisma AIRS API Integration with OpenAI Codex

[Announcement](https://www.paloaltonetworks.com/blog/category/announcement/)


[Products and Services](https://www.paloaltonetworks.com/blog/category/products-and-services/)


Palo Alto Networks works closely with OpenAI across our product platform and Unit 42, leveraging advanced frontier model capabilities. Furthermore, we are a partner in the


[OpenAI Daybreak Cyber Partner Program](https://openai.com/daybreak/partners/) , working with OpenAI to bring trusted, AI-powered cyber defense to more organizations. Today, we’re announcing native integration of Prisma AIRS Runtime API with OpenAI Codex. This milestone deepens our partnership and drives our shared mission forward:


*equipping defenders with the industry's most advanced tools* .


AI coding assistants have transformed software development, accelerating shipping velocity and changing how engineers solve problems. But as integration of AI coding assistants deepens across development teams, security and compliance teams constantly struggle with enabling developer productivity while helping protect proprietary source code and credentials, and reducing the risk of runtime threats entering the codebase.


## Streamlined Security in Minutes


We designed this integration to eliminate complex traffic steering and heavy client-side hooks. Securing your entire developer organization takes just a few clicks inside


**Codex Enterprise Management UI** . Here is how you can enable it:


1. **Retrieve Credentials:** Generate your API key and endpoint from the Prisma AIRS management console.


2. **Configure Codex:** Paste the Prisma AIRS API key and endpoint directly into your Global Admin Console.


3. **Activate Org-Wide Scanning:** Once saved, all user prompts across your entire organization in Codex are automatically routed through the Prisma AIRS Runtime API for real-time inspection.


## What Prisma AIRS Brings to OpenAI Codex


By connecting the Codex in ChatGPT for Enterprise workspace to the Prisma AIRS Runtime API, security teams gain comprehensive, automated security controls across two critical vectors:


### 1. Enterprise Data Loss Prevention (DLP)


Developers frequently paste context – logs, config snippets, or functions – into AI coding assistants. Prisma AIRS scans inputs to help identify sensitive data before it is submitted.


- **Secrets & Credentials:** Intercept API keys, hardcoded passwords, tokens, and private keys.


- **PII & Financial Data:** Automatically detect personally identifiable information and regulated data patterns.


- **Proprietary Code & IP:** Enforce custom pattern rules to help prevent sensitive internal code, architecture details, or trade secrets from being submitted.


### 2. Advanced Threat & Malicious Code Detection


AI prompts and contextual inputs can contain content that introduces security risks into development workflows. Prisma AIRS Runtime API inspects incoming developer inputs for:


- **Malicious Code Patterns:** Detect obfuscated scripts, dangerous command executions, or known exploit patterns within developer prompts.


- **Malicious URLs & Links:** Flag unverified, phishing, or malicious domain references before they influence generated code or enter internal repos.


- **Prompt Manipulation Attacks:** Detect adversarial inputs that may attempt to bypass system controls or alter model behavior.


## Built for DevSecOps Alignment


Security controls only work if developers actually use them. Because the inspection happens at the platform administrative level via API, developers continue working natively in Codex without changing their IDE setup or downloading local hooks. SecOps gain centralized visibility, consistent policy enforcement, and audit-ready logging capabilities across the entire engineering organization, while developers keep the speed and experience they expect.


### Key Takeaways


- **Security as Invisible Infrastructure:** Prisma AIRS integrates at the Codex administrative layer, applying security and DLP controls without requiring additional developer workflow changes. Developers can continue working in Codex while security teams maintain centralized policy controls.


- **Unified Governance for Frontier Models:** Prisma AIRS acts as an inline security layer that scans AI traffic in real-time before prompts ever reach the destination model. This architecture can help organizations identify sensitive information moving outward while detecting prompt manipulation and potentially malicious content moving into development workflows. If Prisma AIRS API detects a threat, a block verdict is sent to OpenAI Codex, and the prompt does not reach the destination model or MCP server.


- **Organization-Wide Compliance:** Deployment speed is a strategic advantage, not just an IT convenience. By configuring the Prisma AIRS API within the Global Admin Console, organizations can achieve org-wide governance without complex traffic steering. This can help development teams shift from unmanaged AI use toward more consistent, auditable security practices.


## Getting Started


The Prisma AIRS integration helps organizations maintain consistent security controls while preserving the speed and productivity gains of Codex. Organizations can combine Palo Alto Networks’ security capabilities with OpenAI Codex to maintain centralized security and governance controls.


**Choose your path forward:**


- [Attending Black Hat 2026](https://register.paloaltonetworks.com/paloaltonetworks-blackhat-2026) **:** Stop by the Palo Alto Networks booth to check out the Prisma AIRS live demo.


- **Ready to secure your rollout:**[Contact Prisma AIRS AI Security Experts](https://www.paloaltonetworks.com/sase/prisma-airs-contact-us) to discuss your specific governance and deployment needs.


- **Ready to build:** Visit the


[Prisma AIRS Documentation](https://pan.dev/prisma-airs/api/airuntimesecurity/airuntimesecurityapi/) to connect your endpoints in minutes.


---


*Forward-Looking Statements*


*This blog contains forward-looking statements that involve risks, uncertainties and assumptions, including, without limitation, statements regarding the benefits, impact, or performance or potential benefits, impact or performance of our products and technologies or future products and technologies. These forward-looking statements are not guarantees of future performance, and there are a significant number of factors that could cause actual results to differ materially from statements made in this blog. We identify certain important risks and uncertainties that could affect our results and performance in our most recent Annual Report on Form 10-K, our most recent Quarterly Report on Form 10-Q, and our other filings with the U.S. Securities and Exchange Commission from time-to-time, each of which are available on our website at investors.paloaltonetworks.com and on the SEC's website at www.sec.gov. All forward-looking statements in this blog are based on information available to us as of the date hereof, and we do not assume any obligation to update the forward-looking statements provided to reflect events that occur or circumstances that exist after the date on which they were made.*
