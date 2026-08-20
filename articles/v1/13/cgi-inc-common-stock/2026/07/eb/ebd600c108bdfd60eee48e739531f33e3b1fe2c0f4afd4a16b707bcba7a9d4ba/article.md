---
schema_version: "1.0.0"
document_id: "ebd600c108bdfd60eee48e739531f33e3b1fe2c0f4afd4a16b707bcba7a9d4ba"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/insurance/strategies-for-migrating-your-life-and-annuity-policy-administration-systems"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:826dbef134bcd1c61dab88bfc7d5b01328823a4937452da25cb93d826bf11869"
---

# Strategies for migrating your life and annuity policy administration systems

Life and pensions (L&P) insurance carriers can face serious challenges when migrating their legacy policy administration systems (PAS) or when shifting blocks of business during acquisitions or other critical events. In our latest[CGI Voice of Our Clients](https://www.cgi.com/en/life-insurance/voice-of-our-clients) research, 77% of L&P executives say they are experiencing high to medium challenges with their legacy systems. Further, cloud migration and legacy modernization are executives’ top two cited IT priorities, respectively.


Migrating to modern systems is a business imperative, but not easily achieved. While my previous blog—[Life insurance policy administration system migrations - keys for staying on course](https://www.cgi.com/en/blog/insurance/life-insurance-policy-admin-system-migrations) —discussed some of the main complications that plague legacy PAS migrations, in this post, I’ll examine in more detail three strategies for successfully implementing these large, complex, and potentially risky programs.


### 1. Be realistic: Accurately assess scale and complexity


Every PAS migration has several key elements that can create challenges:


> - Multiple internal and external stakeholder groups
> - Multiple workstreams, including sourcing legacy data and defining rules to correctly configure the target system (the platform your organization is migrating to) and ensure continuity for operations teams
> - Complex inter-dependencies across operations, IT, and third-party vendors
> - Significant budget/burn-rate issues requiring rapid decision-making and agility when implementation doesn’t go according to plan


At the same time, a lack of experience in executing these projects can mean there is little in-house knowledge about how to structure and execute a program with such inter-dependencies. Given this complexity, we recommend that any organization undertaking a migration establish a strong program office that ensures dedicated representation from the following stakeholders:


- **Business owners** with expertise, including:


- Actuarial/product manufacturing
- Administration, with a focus in-force servicing complexities
- Distribution and claims, if impacted by the migration


- **IT owners:**


- Application and data services
- Integration services, including financial systems and document management
- Quality engineering
- Systems analysis/business analysis


- **External vendors:**


- Source PAS team, if maintained/operated by a third party
- Target PAS team, if maintained/operated by a third party


Depending on how your company is organized and how many external partners you have and need, the above list can grow significantly. For example, when acquired books are involved, governance and coordination can rapidly become challenging. Failure to create a clear plan with defined ownership and dependencies often leads to on-the-fly changes in methodology or strategy, which can create a cascading set of challenges related to legacy data quality and policy valuation/balancing.


### 2. Think beyond data: Product analysis is key


Migration is more than “just another data project.” While it’s true that data plays a significant role in any migration, business rules and related logic are of equally high importance. Because the source and target systems may have very different approaches in setting up products and administering policies, the mapping of data and rules across the systems can be complex.


It’s important to focus on the insurance product complexities upfront by establishing joint product analysis teams early on, especially with products such as variable and indexed families. Examples of areas where common pitfalls occur include the following:


> - Interest crediting rules and logic for investment-linked products
> - Application of taxation rules
> - Calculation logic and storage of various cash values (e.g., dynamically calculated cash surrender value versus stored cash surrender value)
> - Interpretation of policy status (e.g., lapse or grace periods) between systems


Finally, correctly **configuring the target PAS as early as possible is crucial** for conducting testing and reconciliation once test data is extracted from the legacy PAS. This often is a critical bottleneck with complex products. Although it can be tempting to shrink the testing and reconciliation window to accommodate delays in this configuration, doing so will lead to other issues, including short-changing operations testing and pushing defects into post go-live activities.


### 3. Attract and retain the right people


For most carriers, migrating a PAS legacy system is a strategic initiative, but in-house teams may lack experience with execution. Success depends on bringing together the best talent from within the organization and from external partners, aligning talent with appropriate responsibilities, and ensuring the project is their primary focus during key periods.


Following are some critical requirements based on CGI’s experience across multiple programs:


> - **Allocate experts with deep product/actuarial understanding** : It can be challenging to recruit professionals with the experience needed to analyze and document products manufactured 30 years ago. However, finding these people and making these tasks a priority can help to mitigate downstream migration problems stemming from incorrectly configured products on the target system due to missed requirements with respect to both data and business rules.
> - **Engage operations experts who work with the legacy system and related products** : Many arcane administration rules are not obvious, even after close analysis. People who work with the system daily have valuable knowledge that can positively shape the future state model. Including them at the table throughout the project to ensure all administrative nuances are accounted for will improve outcomes. It is crucial to include the operations experts throughout every phase of the project, from analysis and design to user acceptance testing (UAT).
> - **Focus on the IT team supporting the legacy PAS** : On the IT side, the talent pool is often limited. Many legacy systems are either on limited support (users do the bare minimum and try to avoid breaking anything) or support has been outsourced. In many cases, there are vast areas of the system that have not been explored in many years, and documentation is usually incomplete or missing entirely. While finding the right people and ensuring their availability throughout the project is essential, it’s equally important to have a “plan B” to leverage source code analysis tools and techniques. At CGI, for example, we make extensive use of[CAST](https://www.castsoftware.com/) tools to locate critical actuarial and administrative rules embedded in legacy code and stored procedures.


Prioritizing the migration project means backfilling for key resources. For big projects with long timelines, it may make sense to hire contractors to support routine “run” operations, so that key resources do not get pulled into non-critical issues.


External resources to consider include the following,


**Professional services from the target PAS** : The vendor of the target system should offer professional services consultants, along with tools and methodologies unique to their platform. Carriers should include an analysis of the new platform vendor migration tools and processes in their due diligence during project planning.


**Specialized migration partners** : Organizations like CGI have deep experience, critical methodology, and advanced tools that are crucial to a project’s success. In a legacy migration, specialized partners are the experts who can deliver results on time and on budget while effectively managing risk. I like to use the analogy of the home handyperson who can (eventually) figure out how to wire a three-way switch through trial and error. A licensed electrician, on the other hand, will get it right the first time. Your migration project is similar. You can protect your investment and get the desired results by including specialized partners at the beginning of your project.


**Program management** : Smaller carriers may not have a core competency in legacy migration, especially for a large-scale initiative. External partners can provide additional capabilities focused on governance and methodology for stakeholder management and project execution. These offerings can save significant costs and reduce risk in the long run. The carrier should be honest in their assessment of internal program management capabilities when deciding whether to invest in external support.


With a focus on setting up a strong program office and aligning the right talent before launching the project, a PAS migration project can be successful. In my next blog, I’ll explore a few of the surprising yet important details that can typically hinder a project, along with recommendations on how to address them.Contact me to explore how CGI can support your organization in its digital transformation journey.
