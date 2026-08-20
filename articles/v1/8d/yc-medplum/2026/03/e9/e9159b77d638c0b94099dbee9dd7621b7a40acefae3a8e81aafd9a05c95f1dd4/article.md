---
schema_version: "1.0.0"
document_id: "e9159b77d638c0b94099dbee9dd7621b7a40acefae3a8e81aafd9a05c95f1dd4"
company_key: "yc-medplum"
company: "Medplum"
source_id: "yc-medplum-atom-baaaecda9acc"
canonical_url: "https://www.medplum.com/blog/february-2026-update"
published_at: "2026-03-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.588032+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:ab21a7c5d949abecd56da5005851efc25251d72155b89f6ea3e6b29348f0b2b0"
---

# Medplum Monthly Update - February 2026

February was a productive month for Medplum. We shipped four releases — v5.0.14, v5.0.15, v5.1.0, and v5.1.1, including a minor version bump reflecting the depth of new capabilities — with over 130 commits from 25+ contributors. The biggest themes this month were **WebSocket subscription stability and performance improvements** , **AI-powered provider experiences** , **scheduling refinements** , and **revenue cycle tooling** — all advancing our[2026 roadmap priorities](https://www.medplum.com/blog/2026-roadmap) .


## Features​


### Provider App: AI-Powered Spaces and Chat​


**[David Yanez](https://github.com/techdavidy)**


The[Provider app](https://provider.medplum.com/) 's Spaces experience gained significant AI capabilities this month, advancing our[AI roadmap initiative](https://www.medplum.com/blog/2026-roadmap) :


- **LLM components and visualization preview in Spaces** — AI-generated structured components and previews now render inside the chat interface, enabling richer, more interactive AI responses
- **Attachment support in chat** — Providers can now send and receive file attachments within the base chat component
- **Updated AI models** — The provider app is configured with the latest available language models
- **[ThreadInbox](https://storybook.medplum.com/?path=/story/medplum-chat-threadinbox--basic) as a reusable React component** — Moved to the` @medplum/react` package, making inbox functionality available to any Medplum-powered application
- **Navbar alerts and counts** —[Provider app](https://provider.medplum.com/) navigation links now support badge icons and resource counts for at-a-glance workflow status


The[Foomedical demo](https://github.com/medplum/medplum/tree/main/examples/foomedical) — a sample patient-facing experience — was also updated to showcase the latest chat experience.


### AWS Lambda Streaming for Bots​


**[David Yanez](https://github.com/techdavidy)**


[Bots](https://www.medplum.com/docs/bots) deployed on AWS Lambda can now stream responses back to callers in real time. This unlocks[Server-Sent Events](https://en.wikipedia.org/wiki/Server-sent_events) -style streaming from Lambda-hosted bots — a foundational capability for building AI scribes, copilots, and other latency-sensitive applications. This directly supports the[AI roadmap initiative](https://www.medplum.com/blog/2026-roadmap) .


### WebSocket Subscriptions: Scale and Observability​


**[Derrick Farris](https://github.com/ThatOneBro)**


February saw a major investment in[WebSocket subscriptions](https://www.medplum.com/docs/subscriptions) , improving performance, reliability, and operational visibility at scale — directly advancing the[Enterprise Scale & Infrastructure roadmap priority](https://www.medplum.com/blog/2026-roadmap) :


- **Per-user WebSocket subscription limits** — Prevents runaway connections from any single user from destabilizing the platform
- **Super admin dashboard for subscription stats** — Real-time visibility into active WebSocket subscriptions, connection counts, and resource utilization across the entire platform ([super admin guide](https://www.medplum.com/docs/self-hosting/super-admin-guide) )
- **Admin operation to clear all WebSocket subscriptions** — Administrative control for operations teams managing large deployments
- **Multiple performance improvements** — Subscription criteria is now only evaluated when criteria match, access policy decisions are cached in the hot evaluation loop, active subscription lists are partitioned by resource type, Redis key deletion is non-blocking, and a new efficient event payload format reduces latency and CPU load at high subscription volumes


### Scheduling Enhancements​


**[Noah Silas](https://github.com/noahsilas)**


Building on January's[$find and $book](https://www.medplum.com/docs/scheduling/defining-availability) foundation, scheduling received several refinements this month:


- **Customizable appointment creation before[$book](https://www.medplum.com/docs/scheduling/appointment-book)** — Developers can now inject custom fields and logic into the appointment object before the booking operation executes, enabling clinic-specific workflows
- **` _count` parameter for[$find](https://www.medplum.com/docs/scheduling/appointment-find)** — Limits the number of available slots returned, improving efficiency for high-volume scheduling queries
- **Adding a visit now creates a busy slot** — The[Provider calendar](https://www.medplum.com/docs/provider/schedule) correctly blocks time when a visit is added, ensuring accurate availability display
- **Improved UTC/local day discrepancy handling** — Scheduling operations behave correctly when the server's UTC time and the local calendar day differ


### Revenue Cycle: Claim Submission and Eligibility​


**[David Yanez](https://github.com/techdavidy)**


Advancing the[Revenue Cycle & Billing roadmap initiative](https://www.medplum.com/blog/2026-roadmap) :


- **Claim submission to Candid** — A proof-of-concept integration for submitting claims directly to Candid Health, enabling automated professional billing workflows from within the Provider app ([billing overview](https://www.medplum.com/docs/billing) )
- **CoverageEligibilityRequest in` $extract`** — The[SDC $extract operation](https://www.medplum.com/docs/api/fhir/operations/extract) now uses the standard` CoverageEligibilityRequest` resource for insurance eligibility checks, aligning with the FHIR specification


### Pharmacy Components​


**[Oleg Rocklin](https://github.com/oleg-mp)**


New React components for **patient preferred pharmacy selection** are now available in` @medplum/react` . This supports e-prescribing workflows and the[ePrescribe integration](https://www.medplum.com/docs/medications/e-prescibe) roadmap initiative.


### Multiple Redis Support​


**[Matt Long](https://github.com/mattlong)**


Medplum server now supports **multiple Redis instances** , enabling more flexible infrastructure topologies including Redis Cluster deployments and failover configurations. This is an important step for enterprise-scale deployments that require high availability for caching and pub/sub. ([self-hosting guide](https://www.medplum.com/docs/self-hosting) )


### Instance-Level Custom FHIR Operations​


**[Rahul Agarwal](https://github.com/rahul1)**


Developers can now define[custom FHIR operations](https://www.medplum.com/docs/bots/custom-fhir-operations) at the instance level with resource input — enabling more granular, resource-specific API extensions. Combined with updated[bot operation documentation](https://www.medplum.com/docs/bots) , this gives builders more control over custom workflow APIs. For self-hosters, this is particularly valuable: you can expose clinic- or organization-specific operations directly on your FHIR server without forking Medplum, keeping your customizations cleanly separated from the core platform.


### Security and Authentication​


- **Public PGP key published** — Medplum now has a published security PGP key for responsible disclosure, listed on the[security page](https://www.medplum.com/docs/compliance) ([Cody Ebberson](https://github.com/codyebberson) )
- **Sub claim fallback in external auth** — Improved compatibility with external OIDC providers that use non-standard subject claim formats ([Rahul Agarwal](https://github.com/rahul1) )
- **In-memory rate limit tracking for high-volume abuse** — Heavily rate-limited users are now tracked in memory for faster enforcement ([Matt Willer](https://github.com/mattwiller) )
- **Access policy search permission explicitly checked** — Tightened enforcement of access control on search operations ([Matt Willer](https://github.com/mattwiller) )


### Developer Experience​


- **Members tab in project admin** — Users and Patients are now combined into a single Members tab on the project admin page for a cleaner project management experience ([Maddy Li](https://github.com/maddyli) )
- **SMART app launch with patient identifier** — SMART app launch URLs now support patient identifiers, improving interoperability with EHR launch contexts ([Maddy Li](https://github.com/maddyli) )
- **Token` :text` modifier in[SearchControl](https://storybook.medplum.com/?path=/story/medplum-searchcontrol--checkboxes)** — The` SearchControl` component now supports the token text modifier for more flexible searches ([Cody Ebberson](https://github.com/codyebberson) )
- **Hidden field support in forms** — Form components now support hidden fields for passing context without user input ([Maddy Li](https://github.com/maddyli) )
- **Membership label filtering in profile chooser** — The profile selection form can now filter memberships by label, contributed by[jeffrey-peterson-vanna](https://github.com/jeffrey-peterson-vanna)
- **Operation to refresh reference display strings** — A new operation allows refreshing cached display strings on resource references, keeping display data current after resource updates ([Matt Willer](https://github.com/mattwiller) )
- **Native bcrypt for improved authentication performance** — Switched from the pure-JavaScript` bcryptjs` library to native bcrypt bindings, significantly improving password hashing throughput ([Cody Ebberson](https://github.com/codyebberson) )
- **Configurable CDS services URL** — The Clinical Decision Support (CDS) Hooks service URL is now configurable, enabling integration with custom CDS endpoints. CDS Hooks support is part of Medplum's[HTI-4/CMS-0057-F compliance initiative](https://www.medplum.com/docs/compliance/hti-4) , which covers payer interoperability requirements ahead of the January 2027 enforcement date. ([Cody Ebberson](https://github.com/codyebberson) )


## Documentation​


February's documentation work was substantial, with a major restoration of SDK docs and expanded coverage of operations, subscriptions, and integrations.


**Operations and API**


**[Darren Eam](https://github.com/deam65)**


- **Reorganized FHIR operations sidebar and index** — The[FHIR operations docs](https://www.medplum.com/docs/api/fhir/operations) are now organized with a cleaner sidebar and index page for faster navigation
- **New documentation for custom and bot operations** — Full coverage of[custom FHIR operations](https://www.medplum.com/docs/bots/custom-fhir-operations) , including instance-level and system-level patterns
- **Project and system administration operations** — New reference pages for[admin operations](https://www.medplum.com/docs/self-hosting/super-admin-guide)
- **Data export and import operations** — Documented the[Bulk Data Export](https://www.medplum.com/docs/api/fhir/operations/bulk-fhir) ,[C-CDA Export](https://www.medplum.com/docs/api/fhir/operations/ccda-export) ,[Claim Export](https://www.medplum.com/docs/api/fhir/operations/claim-export) , and[CSV export](https://www.medplum.com/docs/api/fhir/operations/csv) API operations
- **Clinical decision support operations** — New reference for CDS Hooks operations, part of Medplum's[HTI-4/CMS-0057-F compliance initiative](https://www.medplum.com/docs/compliance/hti-4)


**Subscriptions**


- **Subscription extension docs** — Clarified the[subscription extension](https://www.medplum.com/docs/subscriptions/subscription-extensions) format for interaction filters and retry policy ([Ian Plunkett](https://github.com/ianplunkett) )
- **` useSubscription` lifecycle hooks** — Added documentation for lifecycle hook callbacks in the[useSubscription](https://www.medplum.com/docs/react/use-subscription) React hook ([Rahul Agarwal](https://github.com/rahul1) )


**Provider and Clinical**


- **DoseSpot notifications** — Updated documentation for[DoseSpot](https://www.medplum.com/docs/medications/e-prescibe) notification handling ([David Yanez](https://github.com/techdavidy) )
- **Patient login and launch URI clarification** — Improved docs for patient-facing login flows and SMART launch URIs, contributed by[Mallikharjuna Mulpuri](https://github.com/mallikharjunamulpuri)
- **Allergy representation** — Clarified allergy status and AllergyIntolerance formatting in the[chart data model](https://www.medplum.com/docs/charting/chart-data-model#allergies-and-intolerances) , contributed by[Aaron Hong](https://github.com/aaronhong)
- **CareTeam tenancy patterns** — Clarified per-patient CareTeam cardinality and multi-tenancy patterns ([Rahul Agarwal](https://github.com/rahul1) )


**Infrastructure and Security**


- **Docker Hardened Images** — Added Docker Hardened Images to the[security page](https://www.medplum.com/docs/compliance) ([Cody Ebberson](https://github.com/codyebberson) )
- **Medplum Agent solutions page** — Created a dedicated[Medplum Agent solutions page](https://www.medplum.com/docs/agent) . The[Medplum Agent](https://www.medplum.com/docs/agent) is an application that runs inside your firewall and bridges on-premise devices (HL7/MLLP, ASTM, DICOM) to the cloud via secure WebSocket channels ([Maddy Li](https://github.com/maddyli) )
- **Branding guide** — New[branding documentation](https://www.medplum.com/docs/self-hosting/branding) explaining how to customize the Medplum logo and application name ([Aditya Suri](https://github.com/adityasuri) )


**Blog**


- **Identity Management: A Practical Guide** — New blog post by[Everett Williams](https://github.com/everett-williams) on managing digital identities in healthcare applications ([read the post](https://www.medplum.com/blog/identity-management) )
- **Medplum Secures Healthcare Platform on Docker Hardened Images** — Published on the Docker blog, this post by Cody Ebberson and Docker's Ajeet Singh Raina covers how Medplum uses[Docker Hardened Images](https://www.docker.com/blog/medplum-healthcare-docker-hardened-images/) to reduce CVE noise, meet HIPAA and SOC 2 requirements, and minimize container security configuration burden


## Bug Fixes​


**Clinical Data**


- Fixed handling of` nullFlavor` values in CCDA-to-FHIR conversion, improving interoperability with C-CDA documents that contain null-flavored data elements (contributed by[Amanda McGivern](https://github.com/amcgivern) )
- Fixed handling of terminology designations without a language code


**Scheduling**


- Fixed scheduling operations during UTC/local day boundary discrepancies


**Server and Authentication**


- Fixed project-scoped users being properly removed when their project membership is deleted
- Fixed a string equality check error in the token endpoint


**Infrastructure**


- Fixed the CDK deployment option for retaining RDS instances on stack deletion
- Fixed basic auth behavior for inactive project memberships
- Reverted an overly strict date validation change that caused regressions


## Releases​


- [v5.0.14](https://github.com/medplum/medplum/releases/tag/v5.0.14) — February 3
- [v5.0.15](https://github.com/medplum/medplum/releases/tag/v5.0.15) — February 20
- [v5.1.0](https://github.com/medplum/medplum/releases/tag/v5.1.0) — February 24
- [v5.1.1](https://github.com/medplum/medplum/releases/tag/v5.1.1) — February 27


## Looking Ahead​


February's investments in WebSocket subscription performance and Lambda streaming set the stage for more capable real-time and AI-powered workflows in Q1. The scheduling enhancements continue to push toward a full self-scheduling experience, and the Candid claim submission proof-of-concept marks meaningful progress on revenue cycle automation.


Join us on[Discord](https://discord.gg/medplum) to share feedback or follow along on[GitHub](https://github.com/medplum/medplum) .
