---
schema_version: "1.0.0"
document_id: "9db38fa4a01c124564656ca4c28f093e32c81e86f693f916adc4624a7e6c4ffe"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/insurance/life-insurance-policy-admin-system-migrations"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:756aa489bc0c73b227a5bf291513948ed5f190958b4792808bf4c47b61859c42"
---

# Life insurance policy administration system migrations: keys for staying on course

For over 20 years, experts in the life insurance industry have talked about the need for carriers to migrate from legacy administration systems to modern platforms that are more flexible, easier and cheaper to maintain, and offer digital distribution and customer service capabilities.


According to our[latest Voice of Our Clients](https://www.cgi.com/en/life-insurance/voice-of-our-clients) research, IT modernization efforts are a high priority across the life and pension industry. This priority is further amplified by the significant number of merger and acquisition (M&A) deals where carriers are selling or acquiring blocks of business that require cost-effective administration.


So why do so many life insurance carriers continue to live with outdated legacy administration platforms? The answer is that migrations are difficult and can go off course without the right people, governance, methodology and tools in place. In this blog I’ll discuss common challenges in legacy migrations and share three keys to achieving successful outcomes.


### Common challenges: It’s not (primarily) the technology


My conversations with CIOs and COOs often start with the premise that the PAS in question is a 30-year-old mainframe system that was built before some of their team members were born. It’s true that aging systems contribute to challenges such as poor agility and time-to-market limitations, especially as supporting personnel near retirement age. But in my experience, the technology itself often isn’t the main problem. Don’t get me wrong — finding high-performing COBOL/CICS/VSAM/IMS systems analysts certainly isn’t easy. But there are still many highly skilled professionals who have experience working with this technology stack. And there are an increasing number of AI-powered tools available to help with the reverse engineering of legacy code.


Following are the most pressing challenges that I believe complicate legacy policy administration system migrations:


### Product complexity


The primary problem is that products supported by legacy systems are often very complex, especially when we look beyond term life and simple annuities to various whole life products—in particular, those in the indexed or variable families. The rules that determine policy values, death benefits, and cash surrender value are complicated to say the least.


For example, sitting in front of me is a stack of prospectus documents from products built in the 1990s and 2000s that are typically over 90 pages long. While half of that content is boilerplate legalese, the costs/premiums/interest sections and related administrative rules legitimately take close to 50 pages to describe. Of course, underlying these prospectuses are underwriting memoranda, product fact sheets, and numerous tables that all need to be reflected in business rules and related data. On top of that, a given base product or chassis might have multiple plans and/or variations—each with subtle but meaningful differences that require slightly different rules.


### Undocumented modifications and updates


A related challenge is that products and systems are often modified over time and not all modifications are clearly documented. Following lengthy product review sessions, it can become clear that a system “no longer works that way” since a new variation was added. This reveal is often followed by a 15-minute discourse on an arcane topic like interest crediting methodologies. The good news is that various product families follow very similar design patterns across carriers. So, with a little bit of discipline and some very strong structure and governance, it’s possible to reverse-engineer the requirements for older, complex products.


### Data quality and completeness


First-generation administration systems often captured only a bare minimum of policy and party data. Instead, these systems relied on the application/policy documents (either physical or scanned) for some required information. Over time, the system may have been modified to increase the amount of information stored as data, but in most cases that data was not applied retroactively to existing policies. As a result, it’s common to find systems where key information, such as beneficiaries or underwriting class, are not available. In many cases, much of the information was manually entered from paper documents, which means quality is variable or even poor.


### Keys to success: people, governance, methodologies and tooling


The good news is, with the right people, governance, methodologies and tooling, it’s possible to migrate products from legacy administration systems, on time and on budget, while successfully addressing user needs.


Below are the three key areas to address for a successful legacy migration:


> 1. **People:** Successful migration projects require lining up the right talent, both from within the organization and externally. It takes highly skilled experts with significant experience in overseeing complex migration projects, as well as experts in the carrier’s business, technical and functional domains.
> 2. **Governance** : Migrations are large, complex programs involving multiple stakeholder groups. They require a governance framework that recognizes this complexity and helps facilitate coordination and communication. Carriers can accelerate results with proven governance and operating models and a smart transition approach.
> 3. **Methodologies and tooling:** Proven strategies, methodologies, techniques and tools are needed to deliver lower risk migrations. This includes the mapping of application, infrastructure and data dependencies and business impacts, and developing optimized move sequences using automated software tools.


Pleasecontact me to share your perspectives on timely and cost-effective strategies for migrating legacy administration systems.
