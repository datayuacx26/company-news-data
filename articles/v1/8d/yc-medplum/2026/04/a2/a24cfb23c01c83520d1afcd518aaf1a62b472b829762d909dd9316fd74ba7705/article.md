---
schema_version: "1.0.0"
document_id: "a24cfb23c01c83520d1afcd518aaf1a62b472b829762d909dd9316fd74ba7705"
company_key: "yc-medplum"
company: "Medplum"
source_id: "yc-medplum-atom-baaaecda9acc"
canonical_url: "https://www.medplum.com/blog/march-2026-update"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.588032+00:00"
fetched_at: "2026-07-28T22:16:19.189952+00:00"
content_hash: "sha256:9c52025631a6f9b37c8403cadd4cb9179d18f759add2daa6e97144f7381fc9ec"
---

# Medplum Monthly Update - March 2026

March was yet another busy month for Medplum. Three patch releases — v5.1.2, v5.1.3, and v5.1.4 — arrived alongside over 100 commits from 20+ contributors, and the work underneath the surface was just as significant as the new features. Spaces gained richer AI tooling. The[communications documentation](https://www.medplum.com/docs/communications) received a full suite of guides covering everything from the data model to automations. Worker infrastructure got a substantial rethink, and[WebSocket subscriptions](https://www.medplum.com/docs/subscriptions) were hardened across the board. All of it continues to advance our[2026 roadmap priorities](https://www.medplum.com/blog/2026-roadmap) .


## Features​


### Provider App: Spaces and AI​


**[David Yanez](https://github.com/techdavidy)**


Spaces continues to advance our[AI roadmap initiative](https://www.medplum.com/blog/2026-roadmap) and the[Provider Application roadmap goal](https://www.medplum.com/docs/provider) :


- **System prompts on` Communication` resources** — Spaces stores system prompts as` Communication` resources for clearer modeling and reuse
- **Component preview, code, and FHIR resources** — Previews and visibility into generated UI, code, and underlying resources in the Spaces chat experience
- **Translator tool looping** — The translator can iterate until the user's request is satisfied for more reliable multi-step workflows
- **Updated default language model** — Newer model defaults for improved quality and consistency
- **Spaces demo bots** — Demo bots for Spaces are easier to discover in development environments
- **Tab navigation** — Standardized tabs across high-traffic screens ([Kevin Wadeshaw](https://github.com/kevinwadeshaw) )


### Provider App: Messaging, Scheduling, and Billing​


**[David Yanez](https://github.com/techdavidy)**


The[Provider app](https://provider.medplum.com/) gained messaging, calendar, and billing improvements:


- **Communications payload and eFax** — A dedicated payload tab on Communications, eFax integration, and ThreadChat updates (subject on communications, clearer file metadata for uploads and search)
- **Calendar scheduling** — Switch practitioner schedules from the calendar; more robust Find pane when service types omit a system; visit setup right after[$book](https://www.medplum.com/docs/scheduling/appointment-book) ; schedule pickers resolve actors via` Schedule:actor`
- **Candid billing** — Claim status in the Provider app and use of external identifiers when a Candid encounter id is absent
- **SMART App Launch** —` fhirContext` support and respect for an existing` login` query parameter on launch
- **Project-scoped Provider access** — Project membership aligns with who can open the Provider experience ([Cody Ebberson](https://github.com/codyebberson) )
- **Domain rules for external apps** —` DomainConfiguration` can target apps hosted outside the core deployment ([Cody Ebberson](https://github.com/codyebberson) )
- **Charting and sample data** — Controlled-substance documentation updates ([Maddy Li](https://github.com/maddyli) ) and CPT handling fixes on sample data


### Scheduling Operations​


**[Noah Silas](https://github.com/noahsilas)**


Server-side scheduling advances the[Scheduling roadmap initiative](https://www.medplum.com/docs/scheduling) :


- **` $find` service type matching** — More accurate matching when resolving available slots
- **Wildcard availability removed** — Clearer availability semantics and fewer ambiguous slot definitions
- **[FooMedical scheduling demo](https://github.com/medplum/medplum/tree/main/examples/foomedical)** — Updated patient-facing scheduling showcase
- **Demo schedules** — Refreshed demo schedule content and calendar examples ([Reshma Khilnani](https://github.com/reshmakh) )


### Platform and Infrastructure​


**[Cody Ebberson](https://github.com/codyebberson)** and **[Matt Long](https://github.com/mattlong)**


This work advances[Enterprise Scale & Infrastructure](https://www.medplum.com/blog/2026-roadmap) :


- **Workers** — Dispatch worker path, configurable background workers, CDK support for worker-only services, and conditional worker deployment
- **` auth/me` project features** — Clients read enabled project features from the authenticated session
- **Marketplace definitions** — Foundation resources for marketplace packages ([package resource](https://www.medplum.com/docs/api/fhir/medplum/package) )
- **Bot` $init` operation** — Initializes Bot content for repeatable deployments ([Bots overview](https://www.medplum.com/docs/bots) )
- **Auth UI extraction** — Shared React components for sign-in and session flows


**[Matt Willer](https://github.com/mattwiller)**


- **Presigned Binary uploads** — Secure, efficient client-side uploads without routing through the server
- **Rate-limit caching** — In-memory tracking of heavily rate-limited IPs for faster enforcement
- **Database tuning** — PostgreSQL transaction idle timeouts and faster login flow
- **Observability** — Broader FHIR interaction metrics from the system repository; transaction consistency fixes in database reads


**[Derrick Farris](https://github.com/ThatOneBro)**


- **WebSocket subscription hardening** — Subscription token lifecycle, stale subscription detection against user limits, and FHIRcast topic key refresh
- **HL7 client resilience** — Clients now warn instead of error on unknown message control IDs


### Developer Experience​


- **` ThreadInbox` / ThreadChat** — Optional` allowPatientSelection` for patient-facing apps; prop type exports; QuestionnaireForm fixes for repeated multiple-choice items; questionnaire signature stories ([David Yanez](https://github.com/techdavidy) ,[Noah Silas](https://github.com/noahsilas) )
- **` useSearch` typing** — Results use` WithId<T>` for safer access to server-assigned ids ([Noah Silas](https://github.com/noahsilas) )
- **SMART launch** — Opens in a new tab by default; respects an existing` login` query parameter on launch ([Maddy Li](https://github.com/maddyli) )
- **Reference display fallback** — UUID fallback when a reference has no display string ([Maddy Li](https://github.com/maddyli) )
- **` ValueSetAutocomplete`** — Configurable count behavior ([Aditya Suri](https://github.com/adityasuri) )
- **MedplumClient retry options** — Configurable maximum retry time and default` maxRetries` ([Cody Ebberson](https://github.com/codyebberson) ,[Aditya Suri](https://github.com/adityasuri) )
- **CLI healthcheck validation** — Custom base URLs are validated against the server healthcheck when configuring profiles ([Medplum CLI](https://www.medplum.com/docs/cli) ,[Rahul Agarwal](https://github.com/rahul1) )
- **[SMART on FHIR demo app](https://github.com/medplum/medplum/tree/main/examples/medplum-smart-on-fhir-demo)** — New example app for SMART launch and patient context ([Alex Lin](https://github.com/alexanderxlin) )
- **[Why Open Source](https://www.medplum.com/open-source)** — New page on Medplum's open-source philosophy


## Documentation​


March's documentation push emphasizes **communications** , **clinical and scheduling workflows** , and **platform operations** — supporting builders who ship[workflow-heavy provider experiences](https://www.medplum.com/docs/provider) .


**Communications and messaging**


**[Everett Williams](https://github.com/everett-williams)**


- **[Thread lifecycle, participants, and access control](https://www.medplum.com/docs/communications/thread-lifecycle-participants-access-control)** — How threads behave over time, who can see them, and how access policies apply
- **[Messaging data model](https://www.medplum.com/docs/communications/messaging-data-model)** — Updated mental model for how conversations are represented and evolve
- **[Task-based message response tracking and routing](https://www.medplum.com/docs/communications/message-response-tracking-and-routing)** — Patterns for tying tasks to message workflows
- **[Searching and querying threads](https://www.medplum.com/docs/communications/searching-and-querying-threads)** — Practical search patterns for threaded communications
- **[Sending messages and attachments](https://www.medplum.com/docs/communications/sending-messages-and-attachments)** — Attachment flows for secure messaging
- **[Read receipts and message status](https://www.medplum.com/docs/communications/read-receipts-and-message-status)** — Delivery and read state for auditing and UX
- **[Messaging automations with Bots](https://www.medplum.com/docs/communications/messaging-automations)** — Automations that react to messaging events
- **Example code** — New messaging snippets aligned with the guides
- **Sidebar reorganization** — Clearer navigation for communications and related topics


Additional communications documentation pages are in the works beyond what shipped in March, so expect the[communications section](https://www.medplum.com/docs/communications) to keep growing.


**Clinical charting and scheduling**


**[Darren Eam](https://github.com/deam65)**


- **[Visit Templates and the SOAP Approach](https://www.medplum.com/docs/charting/visit-templates)** — Visit templates tied to SOAP (structured S/O/P, narrative assessment) in clinical charting
- **[Time zones and timestamps](https://www.medplum.com/docs/scheduling/timezones)** — Scheduling with zones and instants in FHIR and the app ([Cody Ebberson](https://github.com/codyebberson) )
- **Scheduling guides** — Broader refresh of scheduling documentation ([Finn Bergquist](https://github.com/finnbergquist) )
- **[Defining availability](https://www.medplum.com/docs/scheduling/defining-availability)** — Clarified alpha labeling ([Aditya Suri](https://github.com/adityasuri) )


**Platform and operations**


- **[External identity providers](https://www.medplum.com/docs/auth/external-identity-providers)** — Routing strategies for external IdP integrations ([Rahul Agarwal](https://github.com/rahul1) )
- **[Multi-tenant access policies](https://www.medplum.com/docs/access/multi-tenant-access-policy)** — Tenancy guidance in access documentation ([Maddy Li](https://github.com/maddyli) )
- **[Server configuration: maxBatchSize](https://www.medplum.com/docs/self-hosting/server-config#maxbatchsize)** — How` maxBatchSize` interacts with async batches and` maxJsonSize` ([Rahul Agarwal](https://github.com/rahul1) )
- **[Medplum Events Calendar](https://www.medplum.com/blog/events-calendar)** — Updated upcoming events ([Reshma Khilnani](https://github.com/reshmakh) )
- **Lab account onboarding** — Revised instructions for requesting a lab account ([Reshma Khilnani](https://github.com/reshmakh) )


## Bug Fixes​


**Server**


- Return HTTP 400 when PostgreSQL rejects date/time values that overflow field bounds, instead of a generic database error (contributed by Léandre Chamberland-Dozois)
- Improved patch behavior when JSON patch operations implicitly create arrays
- Hardened` ProjectMembership.invitedBy` population for consistent invite attribution
- Additional edge cases for subscription token expiry and rebinding
- Fixed basic authentication handling for inactive project memberships


**Clinical and UI**


- Fixed` mapReferenceRange` so numeric reference ranges map cleanly into FHIR` Range` for observations (contributed by[Amanda McGivern](https://github.com/amcgivern) )


## Releases​


- [v5.1.2](https://github.com/medplum/medplum/releases/tag/v5.1.2) — March 7
- [v5.1.3](https://github.com/medplum/medplum/releases/tag/v5.1.3) — March 7
- [v5.1.4](https://github.com/medplum/medplum/releases/tag/v5.1.4) — March 20


## Looking Ahead​


March deepened the Provider app's Spaces, messaging, and scheduling story, while worker and subscription work strengthened the platform's operational backbone. Communications documentation now gives teams a clearer map from threads and tasks to automation — supporting the next wave of patient and clinician-facing workflows.


Join us on[Discord](https://discord.gg/medplum) to share feedback or follow along on[GitHub](https://github.com/medplum/medplum) .
