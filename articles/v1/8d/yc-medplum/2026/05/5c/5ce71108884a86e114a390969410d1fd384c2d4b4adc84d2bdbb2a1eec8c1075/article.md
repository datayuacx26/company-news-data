---
schema_version: "1.0.0"
document_id: "5ce71108884a86e114a390969410d1fd384c2d4b4adc84d2bdbb2a1eec8c1075"
company_key: "yc-medplum"
company: "Medplum"
source_id: "yc-medplum-atom-baaaecda9acc"
canonical_url: "https://www.medplum.com/blog/april-2026-update"
published_at: "2026-05-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.588032+00:00"
fetched_at: "2026-07-28T22:15:29.906392+00:00"
content_hash: "sha256:801877360c318eed61fea26e3a1236c5ae821aa0ce34cbcd9360d04441b9d051"
---

# Medplum Monthly Update - April 2026

Medplum stayed active this April, with 130+ commits from 20+ contributors amounting to three patch releases — v5.1.7, v5.1.8, and v5.1.9. The Provider app's billing and coverage workflows took a major step forward with integrated claim submission and live eligibility visibility.[Scheduling](https://www.medplum.com/docs/scheduling) added support for service line configurations on` HealthcareService` , aligning with R5/R6 FHIR conventions. Spaces, an AI-powered chat workspace inside the Provider App, now surfaces tool calls and responses directly in the chat so you can see exactly what the AI is doing. Clinical documentation expanded across intake, care plans, diagnostics, authentication, and integrations. All of this continues to drive forward our[2026 roadmap priorities](https://www.medplum.com/blog/2026-roadmap) .


## Features​


### Provider App: Spaces and AI​


**[David Yanez](https://github.com/techdavidy)**


April brought three focused Spaces improvements touching the full AI interaction loop — input, reasoning, and execution — as part of the[AI roadmap](https://www.medplum.com/blog/2026-roadmap#ai) :


- **Tool request/response visibility** — Tool calls and responses now surface inside the Spaces chat experience for full transparency into AI agent actions
- **Improved AI agent loop handling** — More robust multi-step agent execution with a resizable preview panel for inspecting generated output
- **` $ai` project API key** — The` $ai` operation now uses the project-scoped API key for consistent model configuration across environments


### Provider App: Billing and Coverage​


**[David Yanez](https://github.com/techdavidy)**


April's biggest Provider app push closed more of the end-to-end[billing loop](https://www.medplum.com/docs/billing) — from submitting a claim to understanding why it failed:


- **Claim submission** — Submit claims directly from the Provider app
- **Submit claim confirm modal** — Confirmation step before claim submission to prevent unintended submissions
- **Coverage eligibility display** — Coverage eligibility request and response panels in the Provider app, accessible from within encounters
- **Coverage request component** — Dedicated component for initiating coverage requests ([Insurance Eligibility Checks](https://www.medplum.com/docs/billing/insurance-eligibility-checks) )
- **Error display on claim submission** — Error messages surface clearly when a claim fails to submit


### Provider App: Prescribing​


**[Oleg Rocklin](https://github.com/oleg-mp)**


Two long-running prescribing threads landed this month:


- **Drug/Allergy warning enhancement** — Drug/Allergy Warning support brings prescribing safety checks into the Provider app workflow
- **Provider enrollment React hooks** — React hooks allow a prescriber self-enrollment bot to reduce the integration setup to a few hook calls


### Scheduling​


**[Noah Silas](https://github.com/noahsilas)**


The[scheduling overhaul](https://www.medplum.com/blog/2026-roadmap#scheduling) reached an inflection point in April:` HealthcareService` is now the authoritative source for scheduling parameters, replacing looser conventions and enabling far more reusable slot configurations:


- **` HealthcareService` scheduling parameters** — Scheduling operations now use parameters defined directly on` HealthcareService` resources, enabling more expressive and reusable configurations
- **Explicit` HealthcareService` references** — Scheduling uses explicit` HealthcareService` references for unambiguous slot resolution
- **R5/R6` Availability` type alignment** — The scheduling availability subextension is shaped to match the R5/R6` Availability` type for forward compatibility, which will help with future-proofing
- **` HealthcareService.offeredIn` backport** — R5` offeredIn` field backported to Medplum's R4` HealthcareService` for richer service-location relationships
- **Scheduling UI consistency** — Provider scheduling UI fixups and month view corrections for a more consistent experience
- **[State-by-state medical licensure scheduling](https://www.medplum.com/docs/scheduling/state-by-state-licensure)** — Scheduling support for state-by-state practitioner licensure requirements ([Finn Bergquist](https://github.com/finnbergquist) )
- **Insurance eligibility FHIR operation** — New custom FHIR operation` $stedi-check-eligibility` for[insurance eligibility checks](https://www.medplum.com/docs/integration/stedi/insurance-eligibility/eligibility-checks) ([Finn Bergquist](https://github.com/finnbergquist) )


### Platform and Infrastructure​


**[Cody Ebberson](https://github.com/codyebberson)** and **[Matt Long](https://github.com/mattlong)**


April's platform work covered security hardening, search correctness, and operational reliability — feeding into[Enterprise Scale](https://www.medplum.com/blog/2026-roadmap#enterprise-scale--infrastructure) and the[H1 compliance](https://www.medplum.com/blog/2026-roadmap#compliance-h1-2026) track:


- **mTLS passthrough on ALB** — Mutual TLS passthrough at the Application Load Balancer for certificate-based client authentication
- **Tightened access controls for linked projects** — Stricter permission enforcement and consolidated logic for determining permitted project IDs
- **Auto-disable subscriptions on failure** —` Subscription` resources automatically disable after repeated delivery failures, reducing noise and resource waste
- **` Patient.$match` operation** — Server-side FHIR` $match` operation for[patient de-duplication](https://www.medplum.com/blog/patient-deduplication) and record linkage, which is part of a master patient index pattern
- **Project-scoped presigned URLs** — Project context is now included in presigned Binary URLs for more secure access
- **Login rate limiting** — Separate, stricter rate limit on login paths to improve brute-force resistance
- **JWT` jti` claim** — The JWT ID (` jti` ) claim is now always set for replay-attack protection
- **Skip post-deploy migrations in` firstBoot`** — Operators can skip post-deploy migrations during initial boot for faster first-start deployments, which will benefit self-hosted Medplum users
- **Transaction dead connection management** — Improved handling of dead connections during database transactions
- **SSE-C encryption for S3 Binary storage** — Server-Side Encryption with Customer-Provided Keys (SSE-C) support for S3 Binary storage (Nicolas Weiß)


**[Matt Willer](https://github.com/mattwiller)**


- **Range search** — Range-based FHIR search correctly handles overlapping ranges and boundary conditions
- **` _filter` modifier support** — Modifiers in` _filter` search parameters now work consistently
- **OTel delta aggregation** — OpenTelemetry metrics use delta aggregation temporality with exponential histogram views for accurate reporting
- **Rate-limit delay improvement** — Rate-limited requests delay in async context rather than consuming rate-limit tokens for fairer enforcement


**[Derrick Farris](https://github.com/ThatOneBro)**


- **Agent message tracking** — Agent messages are tracked independently of connected clients for more reliable delivery and observability
- **Load balancer algorithm config** — CDK support for configuring the load balancer routing algorithm via` loadBalancerAlgorithm`
- **WebSocket handler colocation** — All WebSocket handlers consolidated into a single location for a cleaner server architecture


### Developer Experience​


- **` ResourcesInput` component** — New React component to search and select multiple resources, simplifying multi-select UI patterns ([David Yanez](https://github.com/techdavidy) )
- **` MedplumClient` streaming support** —` MedplumClient` now accepts` ReadableStream` and Node.js` Readable` for efficient streaming uploads ([Cody Ebberson](https://github.com/codyebberson) )
- **Streaming bulk export** — Bulk export uses streaming to handle large datasets without buffering in memory ([Cody Ebberson](https://github.com/codyebberson) )
- **` valueSetElementToCoding` /` codingToValueSetElement`** — Utility functions now exported from` @medplum/react` for mapping between value set elements and codings ([Darren Eam](https://github.com/deam65) )
- **` PatientSummary` modular architecture** — Refactored into a config-driven, modular architecture for easier customization per deployment ([Rahul Agarwal](https://github.com/rahul1) )
- **Tenants tab on Patient page** — Multi-tenancy visibility directly on the Patient detail page ([Maddy Li](https://github.com/maddyli) )
- **Admin MFA reset endpoint** — New admin endpoint to reset a user's MFA enrollment ([Ian Plunkett](https://github.com/ianplunkett) )
- **` externalAuthProviders` via environment variable** — External auth providers can now be configured via environment variable for simpler deployment ([Ian Plunkett](https://github.com/ianplunkett) )
- **Apply markdown rendering to Resource documentation fields** — Resource documentation fields now render markdown for richer display ([Noah Silas](https://github.com/noahsilas) )
- **Two new FHIR search parameters** — Additional search parameters added to expand query capabilities ([Matt Long](https://github.com/mattlong) )


## Documentation​


April saw a concentrated documentation effort on two fronts: filling out the clinical workflow narrative from intake through care plans and diagnostics, and hardening the auth and integrations reference material that production deployments rely on.


**Clinical workflows**


**[Everett Williams](https://github.com/everett-williams)**


- **[Intake & Registration clinical workflow](https://www.medplum.com/docs/intake)** — End-to-end guide for patient intake and registration flows
- **[Longitudinal patient case tracking with care plans](https://www.medplum.com/docs/careplans/longitudinal-patient-case-tracking)** — How to model ongoing patient cases using` CarePlan` resources
- **[Diagnostic Orders: restructured IA and workflow content](https://www.medplum.com/docs/labs-imaging)** — Reorganized Diagnostic Orders section with deeper workflow guidance
- **[Visits page: video walkthrough and tutorials](https://www.medplum.com/docs/provider/visits)** — Video walkthrough and tutorial notes added; sidebar reorganized to surface questionnaires more clearly
- **[Message editing and drafts](https://www.medplum.com/docs/communications/message-editing-and-drafts)** — Guide for editing messages and managing drafts in the communications system
- **[Async encounters with SDK walkthrough](https://www.medplum.com/docs/communications/async-encounters)** — Expanded guide with full SDK code walkthrough for async clinical encounters
- **Clinical sidebar reorganization** — Care Coordination, Diagnostic Orders, and Clinical Configuration sections reorganized for clearer navigation


**Authentication and security**


- **[Client assertion authentication](https://www.medplum.com/docs/auth/client-assertion)** — How to authenticate using signed client assertion JWTs in support of[CMS-0057](https://www.medplum.com/docs/compliance/hti-4) ([Cody Ebberson](https://github.com/codyebberson) )
- **[mTLS authentication](https://www.medplum.com/docs/auth/mtls)** — Setup and configuration guide for mutual TLS client certificate authentication ([Cody Ebberson](https://github.com/codyebberson) )
- **[Direct external auth](https://www.medplum.com/docs/auth/external-identity-providers)** — Added to the auth sidebar and overview for better discoverability ([Ian Plunkett](https://github.com/ianplunkett) )
- **[MFA reset flow](https://www.medplum.com/docs/auth/mfa)** — Admin documentation for resetting user MFA enrollment ([Ian Plunkett](https://github.com/ianplunkett) )


**Platform and integrations**


- **[Clinical Protocols execution guide](https://www.medplum.com/docs/careplans/protocols)** — How to execute clinical protocols using Bots and automation ([Rahul Agarwal](https://github.com/rahul1) )
- **[Health Gorilla: receiving flow and migration guide](https://www.medplum.com/docs/integration/health-gorilla/receiving-results)** — Lab result receiving flow and migration guidance for Health Gorilla integrations ([Rahul Agarwal](https://github.com/rahul1) )
- **[Patient $match documentation](https://www.medplum.com/docs/api/fhir/operations/patient-match)** — Documentation for the new patient matching FHIR operation ([Reshma Khilnani](https://github.com/reshmakh) )
- **[CDS Hooks](https://www.medplum.com/docs/integration/cds-hooks)** — Basic CDS Hooks integration documentation ([Reshma Khilnani](https://github.com/reshmakh) )
- **[HTI-4 / CMS-0057-F update](https://www.medplum.com/docs/compliance/hti-4)** — Compliance documentation updated to include CMS-0057 requirements ([Reshma Khilnani](https://github.com/reshmakh) )
- **[Self-hosting: enabling rollbacks](https://www.medplum.com/docs/self-hosting/enabling-rollbacks)** — How to enable and execute server rollbacks in self-hosted deployments ([Derrick Farris](https://github.com/ThatOneBro) )
- **[Prescriber enrollment statuses](https://www.medplum.com/docs/integration/dosespot/enroll-user)** — All registration statuses documented for the prescriber enrollment flow ([Andy Stoneman](https://github.com/andystoneman) )


## Bug Fixes​


**C-CDA**


- Fixed` nullFlavor` allergy reactions and single translation element handling in C-CDA parsing, preventing import failures on common EHR exports (contributed by[Amanda McGivern](https://github.com/amcgivern) )


**Agent**


- Fixed agent channel reload when the endpoint protocol changes, preventing stale connections after a protocol switch (contributed by Adewoye Adegoke)


## Releases​


- [v5.1.7](https://github.com/medplum/medplum/releases/tag/v5.1.7) — April 8
- [v5.1.8](https://github.com/medplum/medplum/releases/tag/v5.1.8) — April 14
- [v5.1.9](https://github.com/medplum/medplum/releases/tag/v5.1.9) — April 23


## Looking Ahead​


April deepened the Provider app's billing story — from claim submission to live coverage eligibility display — while Spaces gained speech-to-text for AI-assisted clinical documentation. Scheduling's alignment with` HealthcareService` parameters makes multi-location and multi-service configurations significantly cleaner. Clinical documentation now spans intake through care plans, diagnostics, and async encounters, giving builders a fuller end-to-end picture of workflow implementation on Medplum.


Join us on[Discord](https://discord.gg/medplum) to share feedback or follow along on[GitHub](https://github.com/medplum/medplum) .
