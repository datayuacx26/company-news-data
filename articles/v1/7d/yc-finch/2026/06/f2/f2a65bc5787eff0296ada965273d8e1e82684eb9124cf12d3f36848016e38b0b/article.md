---
schema_version: "1.0.0"
document_id: "f2a65bc5787eff0296ada965273d8e1e82684eb9124cf12d3f36848016e38b0b"
company_key: "yc-finch"
company: "Finch"
source_id: "yc-finch-news-import-d853ae2d7ef6"
canonical_url: "https://www.tryfinch.com/blog/q1-2026-product-recap"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-24T08:01:05.068281+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:43685b0a7197cfc7348fdc92f5ecb6391a7bf2d61372d4c0eb5ff028145f1448"
---

# Q1 2026 Product Recap: Finch Recordkeeper, Deductions Mapping, and FLSA Status

*Finch’s product is continuously improving, with releases posted to the*[changelog](https://developer.tryfinch.com/changelog) *as they go live. Once a quarter, VP of Product Gayathri Somanath shares a digest — one place to catch up on everything we released over the previous 3 months, why it matters, and what’s next on our roadmap.*


Q1 was a quarter of expansion, both of what Finch does and who we do it for. We shipped plenty of new releases, none bigger than Finch Recordkeeper, now in private beta.


For years, Finch has provided the data rails that allow companies from across the employment ecosystem in HR, fintech, and benefits to seamlessly connect with HR and payroll systems. Now, we’re applying our payroll connectivity playbook to the retirement layer, bringing the automated data exchange full circle.


But that wasn't the only thing we shipped. We deepened the foundation our customers build on through broader data and provider coverage, introduced smarter tools to power full end-to-end workflows, and simplified some of the most complex provider auth flows, so employers can connect more easily. Here's the full rundown.


## Finch Recordkeeper: bringing payroll connectivity full circle


Retirement plan administration still runs on manual work. Plan sponsors send contribution files by hand, chase down syncs when automated file transfers fail, and process employee election changes one at a time. The process is broken, but the fix didn’t exist.


That’s why we built[Finch Recordkeeper](https://www.tryfinch.com/product/recordkeeper) : full-circle payroll-to-recordkeeper connectivity that can be accessed directly by sponsors, embedded in recordkeeping platforms, or leveraged through TPAs. Plan sponsors can connect their payroll system to their recordkeeper through a single interface to allow data to flow in both directions, reducing or eliminating much of the manual administration work that exists today.


Here’s what that looks like in practice:


1. Sponsors connect their payroll system through Finch Connect in minutes
2. Contributions sync automatically every pay cycle, with real-time visibility
3. Election changes flow straight through — no manual reconciliation required


We’re excited to continue our investment in the Retirement space with this new offering that represents a win-win: sponsors get clean, automated data movement between their payroll system and their recordkeeper, and recordkeepers get a faster, more reliable way to onboard and service plans.


Finch Recordkeeper is currently in private beta with design partners. We’ve already connected to major recordkeepers including Empower and Transamerica, and are actively engaged with several more. We’re looking forward to a GA launch later this year.


**Read more:**[From unifying data to powering workflows:](https://www.tryfinch.com/blog/under-the-hood-from-unifying-data-to-powering-workflows) the “why now” behind Finch Recordkeeper


## A stronger foundation for developers building on Finch


Finch is the data foundation our customers build upon. When we strengthen our platform, every solution running on top gets stronger too.


We invest in that foundation in two ways: by expanding the data we can access (more fields, more providers in the network), and by adding the automation and tooling that powers end-to-end workflows.


### New field coverage and supported providers


Finch now supports the following fields for the specified providers:


- **FLSA status** on the employment endpoint for ADP Workforce Now, Gusto, Workday, Paycom, Paylocity, Paycor, UKG Ready, and UKG Pro. Our PM Joe broke down the logic behind our[opinionated data model](https://www.tryfinch.com/blog/under-the-hood-opinionated-data-modeling-case-study) for this field.
- **Custom fields** for Paychex, Paycom, and ECCA
- **SSN, ethnicity, gender, and manager ID** for Toast Payroll and Square Payroll
- **Rehire date** for QuickBooks Online
- **W-4 support** for Paychex (joining ADP Run, ADP Workforce Now, and QuickBooks)


We’ve also added new providers **7shifts** (beta) and **Employee Navigator** to our network, and automated our connections with **AccountantsWorld** , **isolved** , and **Workforce.com** . Plus: **ADP Run** deductions are now automated.


[Explore our field support →](https://developer.tryfinch.com/integrations/field-support)


### Deductions Mapping


Deduction codes are where payroll fragmentation gets most acute. The same concept can be labeled a dozen different ways across providers, with employer-specific configuration layered on top.


**Deductions Mapping** unifies those employer-specific implementations within the platform, so developers no longer have to manage that logic in their own code. The result is normalized, ready-to-use deduction data, without the per-employer guesswork.


Deductions Mapping


### Improved sandbox data


Pay statements and employment records now more accurately reflect real-world complexity using accurate U.S.-based values, realistic income amounts, and varied line items across earnings, taxes, deductions, and contributions.


## Complex auth, simple connection


Making it easy for employers to connect has always been a core value at Finch, and for a simple reason: an integration is only valuable when employers actually use it. This quarter we reworked some of our more complicated provider configurations, because while the underlying authentication method can be complex, the employer’s experience shouldn’t be.


- **Redesigned auth flows** – We rebuilt the multi-step authentication experiences for **UKG** , **Workday** , and **isolved** so they match the look and feel of each system’s native flow, with more of the journey embedded directly into Finch Connect. **Paycom** ’s connect experience now includes a dedicated pre-authentication screen that handles payment scope configuration up front, setting clear expectations before the employer enters the flow.
- **Auth fallback improvements** – **** We made authentication fallback improvements for ECCA, Heartland, and Payday. In practice, this means that if a provider’s primary authentication method fails, employers receive proactive solutions from Finch to establish the connection.
- **New /disconnect-entity endpoint** – For multi-entity connections, the new /disconnect-entity endpoint lets customers remove a specific entity from a connection without disconnecting or needing to re-authenticate the other entities associated with the employer.


**Read more:**[Auth fragmentation and connection health:](https://www.tryfinch.com/blog/under-the-hood-auth-fragmentation-and-connection-health) why staying connected is the real challenge


## Looking ahead


Q1 set the foundation for an ambitious year. In addition to Finch Recordkeeper (which we’ll continue building to support more recordkeeper and platform integrations), we’re steadily working toward another new frontier: benefits administration. Support for these providers is in process now, with a beta coming soon.


Across the board, the platform continues to scale to support enterprise deployments, with more coverage, deeper functionality, and the reliability our customers build on.


Want to see what Finch can do for your team?[Get in touch](https://www.tryfinch.com/contact) or[sign up](https://www.tryfinch.com/) to get started.
