---
schema_version: "1.0.0"
document_id: "7529baf53aaa34c96c921a043d52e851a0c6dbed1584334bb0b52c6dec909885"
company_key: "yc-medplum"
company: "Medplum"
source_id: "yc-medplum-atom-baaaecda9acc"
canonical_url: "https://www.medplum.com/blog/june-2026-update"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.588032+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:08035976aafc7cc09f71b9ce734f0d1a0cfe3e7f76c10f7dae11a0891d454458"
---

# Medplum Monthly Update - June 2026

The headline this month: **Medplum achieved[HITRUST certification](https://www.medplum.com/blog/hitrust-e1-certification)** , a validation of our security and compliance program. June was also another busy month of shipping, with 200+ commits from 25+ contributors and nine patch releases — v5.1.14 through v5.1.22.


[Scheduling](https://www.medplum.com/docs/scheduling) saw a lot of activity. The[Provider App](https://provider.medplum.com/) gained a reusable inbox shell, and operation-based claim submission. SMART Health Cards and Links arrived with a QR code scanner, the Enterprise data warehouse export was released, and the AI workspace added configurable model routing. The platform also gained email-based[multi-factor authentication](https://www.medplum.com/docs/auth/mfa) , enforceable per project.


All of this continues to drive forward our[2026 roadmap priorities](https://www.medplum.com/blog/2026-roadmap) .


## Features​


### Scheduling​


**[Noah Silas](https://github.com/noahsilas)**


Building on May's operation suite, June moved[scheduling](https://www.medplum.com/blog/2026-roadmap#scheduling) to a Beta release and tightened the rules around how appointments are booked:


- **Scheduling API Beta** — The appointment operation suite is now a Beta release, and the older Slot-based alpha implementations have been removed in favor of the appointment-based flow
- **[$book alignment enforcement](https://www.medplum.com/docs/scheduling/appointment-book)** — Booking now enforces scheduling alignment, and[$find](https://www.medplum.com/docs/scheduling/appointment-find) supports nondivisible alignment times so slots respect configured intervals
- **Planning horizon constraints** — Scheduling operations are now constrained by` Schedule.planningHorizon` , and scheduling parameters can be inherited from the` HealthcareService`
- **Availability defaults** — Availability now defaults to "always" when unset, with the requested time slot included in availability error messages for clearer feedback ([Rohith Vangalla](https://github.com/RohithVangalla1) )
- **Provider scheduling flow** — The[Provider App](https://www.medplum.com/docs/provider/schedule) streamlines encounter creation, links` Appointment.slot` , adds an Appointment confirm button, and moves to FullCalendar v7
- **Cancelled-appointment slots** — The schedule view now hides slots tied to cancelled appointments, so a cancellation frees the time back up instead of leaving a stale block


### Provider App​


**[David Yanez](https://github.com/techdavidy)**


The[Provider App](https://www.medplum.com/blog/2026-roadmap#provider-application) gained reusable building blocks and revenue-cycle improvements:


- **ResourceBoard inbox shell** — A new React shell that encapsulates inbox logic, providing a reusable foundation for task and worklist views
- **Performing practitioner in care teams** — Care team fields now support a performing practitioner, improving attribution in clinical workflows
- **Operation-based claim submission** — Claim submission now uses a server-side FHIR operation, and the app creates or updates the` Claim` during export or submit, simplifying the[billing](https://www.medplum.com/blog/2026-roadmap#revenue-cycle--billing) integration
- **Patient documents tab** — A dedicated documents tab on the patient view, with inbound faxes now stored as` DocumentReference` resources so they land in the chart alongside other records
- **Medications in the patient summary** —` MedicationStatement` resources appear in the` PatientSummary` component, and both` MedicationStatement` and` DocumentReference` now resolve to readable display strings ([Cody Ebberson](https://github.com/codyebberson) )
- **Self-service registration** — A Register page for the Provider App lets new users sign up directly, with the reCAPTCHA widget hidden in the app shell ([Cody Ebberson](https://github.com/codyebberson) )
- **Navigation polish** — Navbar visual tweaks and a HeaderDropdown refactor ([Kevin Shaw](https://github.com/kevinwadeshaw) ), plus fixes to the back button after fax loading and to autoscrolling ([Noah Silas](https://github.com/noahsilas) )


### AI​


**[David Yanez](https://github.com/techdavidy)** and **[Maddy Li](https://github.com/maddyli)**


[AI](https://www.medplum.com/blog/2026-roadmap#ai) work centered on the[Spaces](https://www.medplum.com/docs/provider/spaces) workspace — the in-app AI assistant in the Provider App — and real-time documentation:


- **Configurable model routing** —[Spaces](https://www.medplum.com/docs/provider/spaces#model-selection) now supports a configurable base URL and a dynamic model list, so deployments can route AI requests through their own gateway to control cost and security
- **AI real-time questionnaire form** and **real-time voice** — A real-time questionnaire form plus[voice input](https://www.medplum.com/docs/provider/spaces#voice-input) improvements that keep the socket active during a session
- **Spaces UX** — Prompt composer UI updates and grouping of each FHIR request with its corresponding response for clearer traceability


The[Spaces documentation](https://www.medplum.com/docs/provider/spaces) covers setup, the agent loop, and how to author the system prompts that drive its behavior. Here is a short walkthrough:


### Identity, Sharing, and Documents​


**[Cody Ebberson](https://github.com/codyebberson)**


- **[Multi-factor authentication](https://www.medplum.com/docs/auth/mfa)** — A new email-based MFA method, plus a project-level` mfaRequired` setting that enforces MFA for every member, with documentation for enabling it ([Derrick Farris](https://github.com/ThatOneBro) )
- **External token exchange across project memberships** — Token exchange now works across project memberships, for users who span multiple projects
- **Attachment handling** — Inline attachments in` Patient/$everything` , base64 data support in` <AttachmentDisplay>` , and cursor pagination across patient-everything and multi-type searches
- **Patient invite with RelatedPerson** — The user invite endpoint accepts a` Patient` to support RelatedPerson, enabling caregiver and proxy access ([Maddy Li](https://github.com/maddyli) )


### Enterprise: Data Warehouse and Reliability​


**[Karl Pietrzak](https://github.com/The-Alchemist)** and **[Matt Long](https://github.com/mattlong)**


[Enterprise scale and infrastructure](https://www.medplum.com/blog/2026-roadmap#enterprise-scale--infrastructure) work made the[analytics](https://www.medplum.com/docs/analytics) export production-ready and hardened the data layer:


- **Data warehouse export controls** — Configuration for which tables to sync, include/exclude filters, restored` ORDER BY` , and DuckDB lifecycle cleanup give data teams precise control over what reaches their lakehouse
- **Transaction-scoped repositories** — Repository connections are now transaction-scoped, with idle-in-transaction time tracked, improving reliability under load
- **FHIRPath Patch** — A[FHIRPath Patch](https://www.medplum.com/docs/fhir-datastore) utility wired into the server for targeted resource updates ([Matt Willer](https://github.com/mattwiller) )
- **Project-level SMTP** — Projects can now configure their own SMTP settings for sending email ([Darren Eam](https://github.com/daream) )


### Agent and Connectivity​


**[Derrick Farris](https://github.com/ThatOneBro)**


The on-premise[Agent](https://www.medplum.com/docs/agent) — Medplum's connectivity service for legacy healthcare systems and HL7 interfaces — saw a concentrated round of reliability work this month:


- **Durable HL7 message queue** — The Agent now persists inbound HL7 messages to a durable queue, so messages survive restarts and transient outages instead of living only in memory
- **Resilient` Agent/$upgrade`** — A batch of fixes to the[Agent upgrade flow](https://www.medplum.com/docs/agent/upgrade) : no more duplicated messages during an upgrade, a guard that checks for the upgrade artifact before unlinking it, and a documented upgrade bugfix, alongside a new[Agent/$upgrade documentation page](https://www.medplum.com/docs/agent/upgrade)
- **Reconnection hardening** — The Agent now reconnects across a range of error states, records the acknowledgement before returning in[acknowledgement-only mode](https://www.medplum.com/docs/agent/acknowledgement-modes) , and triggers a clean shutdown before awaiting channel start
- **[Agent/$reload-config](https://www.medplum.com/docs/agent/reload-config)** — A documented operation to reload a deployed Agent's configuration without a full restart
- **WebSocket subscription reliability** — A series of fixes recreate[WebSocket subscriptions](https://www.medplum.com/docs/subscriptions) on reconnect, tear down the Redis listener cleanly on shutdown, bind the message listener before subscribing, and send a handshake on connection establishment


## Compliance​


**[Cody Ebberson](https://github.com/codyebberson)** and **[Reshma Khilnani](https://github.com/reshmakh)**


**Medplum achieved[HITRUST certification](https://www.medplum.com/blog/hitrust-e1-certification) this month** — the headline of our[compliance](https://www.medplum.com/blog/2026-roadmap#compliance-h1-2026) track. HITRUST is one of the most rigorous, prescriptive security frameworks in healthcare, and certification gives customers third-party assurance of Medplum's security and compliance program for their own vendor reviews. Several other pieces of the track also moved:


- **[HITRUST documentation](https://www.medplum.com/docs/compliance/hitrust)** — Published HITRUST documentation alongside the certification, and pointed compliance references at the[Medplum Trust Center](https://trust.medplum.com/) (with an updated Vanta Trust Center link)
- **SMART Health Cards and Links** — Support for SMART Health Cards and Links, including a QR code scanner and updates for the CMS "Keep the Card" (KTC) program, giving patients a verifiable, portable record


- **[Patient/$match](https://www.medplum.com/docs/api/fhir/operations/patient-match) with CMS criteria** — The patient match operation now follows CMS matching criteria, aligning identity resolution with federal guidance
- **Patient data access fixes** — Corrected[Patient/$everything](https://www.medplum.com/docs/api/fhir/operations/patient-everything) pagination links and normalized C-CDA address handling, keeping patient-record exchange complete and standards-conformant ([Joshua Kelly](https://github.com/jdjkelly) )
- **Electronic prior authorization** — Continued progress on electronic prior authorization toward the[HTI-4](https://www.medplum.com/docs/compliance/hti-4) requirements ahead of the January 2027 enforcement date


## Documentation​


**[Cody Ebberson](https://github.com/codyebberson)**


- **Self-hosting fixes** — Corrections across the Azure, GCP, and Kubernetes self-hosting guides, a Debian package fix that preserves local edits on upgrade and restores the DuckDB native dependency, and production Docker base images pinned to Node 24.18 ([Karl Pietrzak](https://github.com/The-Alchemist) )


**Provider App and platform**


- **[Shared Projects](https://www.medplum.com/docs/access)** — New documentation on shared projects ([Finn Bergquist](https://github.com/finnbergquist) )
- **[Multi-lingual FHIR support](https://www.medplum.com/docs/fhir-datastore/multilingual-support)** — Docs and a demo app for multi-lingual FHIR support ([Ian Plunkett](https://github.com/ianplunkett) )
- **Agentic engineering guide** — A new agentic engineering guide and companion blog post ([Everett Williams](https://github.com/everett-williams) )
- **[AI real-time docs](https://www.medplum.com/docs/provider/spaces)** and **Alpha/Beta feature expectations** — Documentation for the AI real-time operation and a guide to what Alpha and Beta feature labels mean ([Andy Stoneman](https://github.com/andystoneman) ,[Noah Silas](https://github.com/noahsilas) )


**Integrations**


- **[Electronic prescribing](https://www.medplum.com/docs/integration)** — Expanded e-prescribe documentation, including prescriber profile setup and staging NPI creation ([Andy Stoneman](https://github.com/andystoneman) ,[Maddy Li](https://github.com/maddyli) )
- **[Lab orders](https://www.medplum.com/docs/integration)** — Public docs for lab account number updates ([Andy Stoneman](https://github.com/andystoneman) )
- **[Clearinghouse claim submission](https://www.medplum.com/docs/billing)** — Claim submission documentation for clearinghouse connectivity ([Finn Bergquist](https://github.com/finnbergquist) )


## Bug Fixes​


**FHIR and billing**


- Render CMS-1500 Box 24E diagnosis pointers correctly (contributed by[Andrew Wong](https://github.com/ajwwong) )
- Prevent duplicate resources in` convertToTransactionBundle` (contributed by[Steven Matthiesen](https://github.com/slmatthiesen) )
- Normalize address part text nodes in C-CDA` mapAddresses` (contributed by[Joshua Kelly](https://github.com/jdjkelly) )


**Server and async**


- Stop polling a cancelled` AsyncJob` by returning 200 from the status endpoint, and add` Accept-Encoding: identity` to token exchange (contributed by[Raphael Malikian](https://github.com/rtmalikian) )
- Bound the 401 retry in` MedplumClient` to prevent infinite retry loops (contributed by[Joshua Kelly](https://github.com/jdjkelly) )
- Paginate AWS Textract results via` NextToken` (contributed by[Jake Diaz-Iglesias](https://github.com/lolkat247) )
- Recreate WebSocket subscriptions on reconnect (contributed by[Drew McDonald](https://github.com/d-vincent) )
- Record the authenticating` ClientApplication` on` AuditEvent.agent\[\]` (contributed by[Jesse Carter](https://github.com/WonderPanda) )


## From the Blog​


Longer-form writing published this month:


- **[Medplum Achieves HITRUST e1 Certification](https://www.medplum.com/blog/hitrust-e1-certification)** by Reshma Khilnani — The certification announcement and what it means for customers
- **[Medplum Scheduling API: Beta Release](https://www.medplum.com/blog/scheduling-beta)** by Noah Silas — A deeper look at the appointment operation suite now in Beta
- **[Large Patient Charts](https://www.medplum.com/blog/large-patient-charts)** by Cody Ebberson — Handling patients with very large charts at scale
- **[Vibe Coding on Medplum](https://www.medplum.com/blog/building-on-medplum-with-ai)** by Everett Williams — Building on Medplum with AI coding assistants, the companion to the agentic engineering guide
- **[From BLE to FHIR: Streaming Clinical-Grade Biosignals with AnyBio + Medplum](https://www.medplum.com/blog/ble-to-fhir-anybio-medplum)** by Stephen Saine — Streaming device biosignals into FHIR


## Releases​


- [v5.1.14](https://github.com/medplum/medplum/releases/tag/v5.1.14) — June 1
- [v5.1.15](https://github.com/medplum/medplum/releases/tag/v5.1.15) — June 3
- [v5.1.16](https://github.com/medplum/medplum/releases/tag/v5.1.16) — June 8
- [v5.1.17](https://github.com/medplum/medplum/releases/tag/v5.1.17) — June 9
- [v5.1.18](https://github.com/medplum/medplum/releases/tag/v5.1.18) — June 15
- [v5.1.19](https://github.com/medplum/medplum/releases/tag/v5.1.19) — June 20
- [v5.1.20](https://github.com/medplum/medplum/releases/tag/v5.1.20) — June 20
- [v5.1.21](https://github.com/medplum/medplum/releases/tag/v5.1.21) — June 20
- [v5.1.22](https://github.com/medplum/medplum/releases/tag/v5.1.22) — June 24


## Looking Ahead​


June brought scheduling to Beta, made the Enterprise data warehouse export production-ready with selectable table sync, and added configurable model routing to the AI workspace. SMART Health Cards and Links, operation-based claim submission, and a reusable inbox shell round out a month focused on Provider App depth and platform reliability.


Join us on[Discord](https://discord.gg/medplum) to share feedback or follow along on[GitHub](https://github.com/medplum/medplum) .
