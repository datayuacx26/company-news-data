---
schema_version: "1.0.0"
document_id: "29e42cf1650628456d76416fb28433045b2287b01364138957aa0ecbdb59074d"
company_key: "yc-gaiia"
company: "Gaiia"
source_id: "yc-gaiia-news-import-0ba41f3cf900"
canonical_url: "https://gaiia.com/newsroom/a-b2b-module-to-power-complex-business-operations-for-isps"
published_at: "2026-01-07T00:00:00+00:00"
first_seen_at: "2026-07-23T10:16:42.852450+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:ca6ac8196e08d31d3216d3752938cbd0f2a9d707a0ab86b2f83dcdb82dec37a6"
---

# B2B Module for Complex ISP Business Operations

‍


Today we are excited to announce the launch of gaiia’s new B2B module, a purpose built system for managing business accounts, enterprise customers, and project based service delivery. This release positions gaiia as the first modern OSS and BSS platform that allows ISPs to manage residential, MDU, and business operations inside one unified system.


As more ISPs expand beyond residential and into business and enterprise services, they need operational software that can support complex multi-location businesses, project-based deployments, account managers, contract renewals, and construction workflows. Until now, most providers have relied on spreadsheets, general-purpose CRMs, or expensive legacy enterprise platforms that are slow to configure and disconnected from network operations.


The new B2B module changes this by giving ISPs a complete commercial operations layer that works seamlessly with gaiia’s billing, workforce, network, product catalog, and customer portal.


‍


### Why ISPs Need a True Business OSS and BSS


Commercial operations work very differently from residential. These customers have multi-site footprints, negotiated contracts, project-based delivery, renewal management, and complex product catalogs that include DIA, transport, dark fiber, and voice.


ISPs today face several challenges:


- Most general CRMs do not connect to network operations, billing, or construction systems
- Legacy enterprise OSS and BSS platforms are expensive and require heavy customization
- Data is siloed across spreadsheets, email threads, and disconnected tools
- Construction and service delivery projects happen outside the operational ecosystem
- Account managers lack visibility into renewals, MACD activity, or service performance


ISPs want a single operational system that can support residential, MDU, and business customers. With this release, gaiia delivers exactly that.


‍


### Introducing the Business Account Entity


‍


‍


At the core of the new B2B module is a dedicated **Business Account** entity. This new account type is designed specifically for the workflows and data structures required by enterprise operations.


Business Accounts include:


- Dedicated CRM table with ID, name, hierarchy level, status, account manager, and created date
- Filters and sorting by renewal date, status, account manager, and more
- Account creation flow with contact information, address, and business details
- A permissions model that limits access to approved roles
- Snowflake synced database tables for reporting and analysis


This new entity creates a clear separation between residential and business workflows. ISPs can manage retail and enterprise accounts in parallel without forcing mismatched workflows into a single account structure.


‍


### Support for Parent and Sub-Account Hierarchies


‍


‍


Many business customers operate across multiple locations, departments, or business units. To support this, Business Accounts include a flexible hierarchy model that allows ISPs to create parent accounts and attach any number of sub-accounts beneath them.


This structure makes it easy to manage multi-site customers, breakout billing or service delivery by location, and maintain clear visibility across an organization’s full footprint. Parent accounts consolidate renewal dates, contract information, MRR, and project activity, while sub-accounts track site-specific services, contacts, and operational details.


The hierarchy ensures that ISPs can manage complex business relationships without losing clarity or creating duplicate records, and it improves reporting by giving leadership teams a unified view of enterprise customers.


‍


### A Commercial Grade CRM for ISPs


Business Accounts introduce a purpose-built CRM layer that includes:


- Account notes
- Activity updates
- Contacts with defineable roles
- Custom statuses
- Account labels
- Custom fields
- Document storage and contracts
- Tickets
- Communications and notifications
- Billing tab identical to residential, including invoicing, SMS, and email
- Contract details with signature status and renewal dates


This gives account managers a complete operational view of each business customer.


‍


### Assign Account Managers and Track Renewals


Business accounts include a dedicated field for Account Manager assignment, complete with:


- One account manager per account
- Easy filtering and reporting
- Visibility for managers and leadership teams
- Support for renewal pipeline reporting


ISPs gain a simple way to manage ownership, measure performance, and organize commercial territories.


‍


‍


### Support for Business Products, Billing, and Inventory


Business accounts integrate directly with existing gaiia modules:


- **Product Catalog:** Assign broadband, DIA, transport, voice, or custom business products to each account. Products can be restricted to business use only.
- **Billing:** Business accounts are fully billable entities with support for monthly recurring revenue, one time fees, and invoice communications.
- **Workforce and Work Orders:** Technicians can complete site visits, installations, turn ups, and MACD work directly tied to business accounts.
- **Inventory:** Equipment can be assigned, tracked, and managed at the business account level.
- **Customer Portal:** Business customers can pay invoices, view billing history, and submit support tickets through the same modern portal used for residential subscribers.


‍


### Introducing Projects: The Operational Backbone for Service Delivery


‍


‍


Business service delivery is project-based. DIA, transport, dark fiber, voice, and multi site connectivity all require coordinated progress across multiple teams.


To support this, gaiia is introducing **Projects** as a new core entity. Projects will allow ISPs to:


- Manage new service orders through structured project templates
- Capture order entry details, quotes, construction fees, and recurring charges
- Associate tickets, workflows, products, and work orders with each project
- Track project status through phases like Scoping, Construction, Activation, and Completion
- Store start dates, expected completion dates, and project numbers
- Enable accurate reporting across finance, construction, and operations
- Automatically generate standardized tickets from project templates, ensuring the right tasks are created and linked every time a new project is launched


Each project template can be customized for the specific requirements of DIA, Transport, Voice, or custom commercial services.


‍


### Integration with Clad for Construction Visibility


gaiia will integrate with[Clad](https://www.withclad.com/) to give ISPs complete visibility into construction progress. Users will be able to:


- Create a Clad project directly from gaiia
- Link existing Clad projects to new service orders
- Sync data between the two systems
- View construction progress within gaiia
- Pull files and field updates from Clad into project or account fields


This eliminates duplicate data entry and connects construction teams with account managers and leadership.


‍


### Better Contract Management for Business Customers


The B2B module introduces improvements to gaiia’s contract manager, including:


- Uploading signed contracts
- Storing start dates, terms, and renewal dates
- Reporting on contract stages and upcoming renewals
- Linking contracts to business accounts for full visibility


This gives ISPs reliable renewal management and clean historical data.


‍


### Why This Matters for the Future of ISP Operations


Supporting commercial customers is essential for ISPs that want to grow across all business lines. With the launch of the B2B module, gaiia becomes the first modern OSS and BSS platform that unifies Residential, MDU, & Business accounts in one operational system.


This reduces operational overhead, increases automation opportunities, improves reporting, and strengthens customer experience across all segments.


It also positions ISPs to win higher-value enterprise deals, accelerate renewals, and modernize their entire operating environment.
