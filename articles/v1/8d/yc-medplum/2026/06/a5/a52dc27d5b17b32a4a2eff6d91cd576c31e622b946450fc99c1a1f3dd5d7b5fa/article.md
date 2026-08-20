---
schema_version: "1.0.0"
document_id: "a52dc27d5b17b32a4a2eff6d91cd576c31e622b946450fc99c1a1f3dd5d7b5fa"
company_key: "yc-medplum"
company: "Medplum"
source_id: "yc-medplum-atom-baaaecda9acc"
canonical_url: "https://www.medplum.com/blog/may-2026-update"
published_at: "2026-06-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.588032+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:51e93af0d9c3e52fece24ac0554f25db2f640f4b2620a5b9b8719e1e108767c2"
---

# Medplum Monthly Update - May 2026

May was a heavy month for Medplum, with 100+ commits from 20+ contributors and four patch releases — v5.1.10 through v5.1.13.[Scheduling](https://www.medplum.com/docs/scheduling) was the headline: the full appointment operation suite —` $find` ,` $hold` ,` $book` ,` $confirm` , and` $cancel` — landed in Alpha. The[Provider App](https://provider.medplum.com/) also gained medication ordering components, order set import, and operation-based claim submission. On the enterprise side, a new data warehouse export features were enabled, and the platform team shipped passwordless magic-link login, AWS GuardDuty malware protection, and a round of OAuth2 spec-compliance fixes. All of this continues to drive forward our[2026 roadmap priorities](https://www.medplum.com/blog/2026-roadmap) .


## Features​


### Scheduling​


**[Noah Silas](https://github.com/noahsilas)**


May completed the core scheduling operation suite that the[scheduling roadmap](https://www.medplum.com/blog/2026-roadmap#scheduling) has been building toward. Each step of the booking lifecycle now has a dedicated, spec-aligned FHIR operation:


- **[Appointment/$find multi-schedule search](https://www.medplum.com/docs/scheduling/appointment-find)** — Search for open slots across multiple schedules in a single request, the foundation for self-scheduling across providers and locations
- **[$hold operation](https://www.medplum.com/docs/scheduling/appointment-hold)** — Temporarily reserve a slot while a patient completes booking, preventing double-booking during checkout
- **[$book for proposed appointments](https://www.medplum.com/docs/scheduling/appointment-book)** —` $book` now accepts a proposed appointment as input, with test coverage for the patient self-booking path
- **[$confirm operation](https://www.medplum.com/docs/scheduling/appointment-confirm)** — New` Appointment/:id/$confirm` operation to confirm a held or proposed appointment
- **[$cancel operation](https://www.medplum.com/docs/scheduling/appointment-cancel)** — New scheduling cancel operation with improved error-path handling throughout the booking flow
- **Appointment-based Provider flow** — The[Provider scheduling calendar](https://www.medplum.com/docs/provider/schedule) now uses the Appointment-based flow, reveals more slots in the schedule page, and is gated behind a scheduling feature flag for controlled rollout
- **Self-scheduling reference flow** — Foomedical, a sample patient-facing experience, now uses` Appointment/$find` +` $hold` to demonstrate the end-to-end patient booking pattern


### Provider App: Medication Ordering and Prescribing​


**[Oleg Rocklin](https://github.com/oleg-mp)**


Medication ordering moved into the[Provider App](https://www.medplum.com/blog/2026-roadmap#provider-application) this month:


- **Meds ordering components** — New React and Provider App components for ordering medications, bringing prescribing into the standard clinical workflow
- **Batched pharmacy/org searches** — Preferred pharmacies and organizations are now fetched with batched searches during prescribing for faster load times
- **Resilient draft handling** — Draft` MedicationRequest` resources are soft-deleted when an order-medication step fails, keeping the chart clean after errors


### Provider App: Order Sets and Billing​


**[Andy Stoneman](https://github.com/andystoneman)** and **[David Yanez](https://github.com/techdavidy)**


- **Order set import** — An order set bundle and import button were added to the Provider App, with a sync bot trigger that runs automatically after an example order set is imported
- **Operation-based claim submission** — Claim submission now uses a server-side FHIR operation instead of a bot, simplifying the[billing](https://www.medplum.com/docs/billing) integration and reducing moving parts
- **Self-pay coverage filtering** — Self-pay coverage is now filtered out of the visit eligibility check and removed from the` PatientSummary` insurance display, so eligibility reflects only billable payers ([Insurance Eligibility Checks](https://www.medplum.com/docs/billing/insurance-eligibility-checks) )
- **` ClinicalImpression` in the timeline** — Clinical impressions now appear in the patient timeline ([Maddy Li](https://github.com/maddyli) )
- **Cancel Visit** — A "Cancel Visit" button and a fixed "Details" link tab round out the encounter workflow ([Noah Silas](https://github.com/noahsilas) )
- **Configurable onboarding** — The Provider App "Get Started" screen can now be hidden via a project setting ([Ian Plunkett](https://github.com/ianplunkett) )


### AI: Real-Time Clinical Documentation​


**[David Yanez](https://github.com/techdavidy)**


AI work this month focused on real-time clinical documentation inside[Spaces](https://www.medplum.com/docs/provider/spaces) , the AI-powered chat workspace in the Provider App, as part of the[AI roadmap](https://www.medplum.com/blog/2026-roadmap#ai) :


- **Real-time speech-to-text** — Live speech-to-text in the Provider App for AI-assisted clinical documentation, backed by updates to the` $ai-realtime` operation
- **Markdown responses in Spaces** — Spaces now renders markdown in AI responses for clearer, better-formatted output
- **Whisper hook in` react-hooks`** — The Whisper transcription hook moved into the` @medplum/react-hooks` package so it can be reused across applications


### Enterprise: Data Warehouse Export​


**[Karl Pietrzak](https://github.com/The-Alchemist)**


A major[Enterprise Scale](https://www.medplum.com/blog/2026-roadmap#enterprise-scale--infrastructure) feature landed: Medplum can now export directly from PostgreSQL into[Apache Parquet](https://parquet.apache.org/) and[Apache Iceberg](https://iceberg.apache.org/) tables on[S3 Tables](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-tables.html) . This gives data teams an efficient, columnar path into modern lakehouse[analytics](https://www.medplum.com/docs/analytics) without standing up a separate ETL pipeline. A new` startDate` parameter lets exports run incrementally. This is new functionality related to Medplum Enterprise.


### Platform, Security, and Infrastructure​


**[Cody Ebberson](https://github.com/codyebberson)**


- **Passwordless magic-link login** — New OAuth2 pre-authorized code flow enables[magic-link sign-in](https://www.medplum.com/docs/auth/pre-authorized-code) , with the pre-authorized code lifetime extended to 7 days for email-based flows
- **Unified external authentication** — Consolidated the external identity-provider paths into a single[unified external auth](https://www.medplum.com/docs/auth/external-identity-providers) flow, including Google Cloud Identity Platform userinfo support
- **AWS GuardDuty Malware Protection** — Added support for[AWS GuardDuty Malware Protection](https://docs.aws.amazon.com/guardduty/latest/ug/malware-protection.html) on uploaded files, with graceful handling of images quarantined by the scanner
- **HTTPS-only subscription URLs** — Rest-hook[Subscription](https://www.medplum.com/docs/subscriptions) URLs now require HTTPS by default, and a server option can require a verified email address before login
- **OAuth2 spec-compliance fixes** — A batch of[OAuth2 token endpoint](https://www.medplum.com/docs/api/oauth/token) corrections including` offline_access` handling when` refresh_token` is in` grant_types` ,` www-authenticate: bearer` responses, and enforcing login membership before scope
- **` log-streaming` project feature** — A new[log-streaming](https://www.medplum.com/docs/integration/log-streaming) project feature plus a` Project.features` search parameter for managing it


**[Derrick Farris](https://github.com/ThatOneBro)**


- **Bot Lambda lifecycle management** — Medplum now prunes old[bot Lambda](https://www.medplum.com/docs/bots/bots-in-production) versions, deletes the Lambda when a bot is deleted, and removes stale versions, keeping cloud deployments tidy and within service limits
- **[Agent/$stats operation](https://www.medplum.com/docs/agent/stats)** — New operation to retrieve statistics for a deployed[Agent](https://www.medplum.com/docs/agent) , Medplum's on-premise connectivity service for legacy healthcare systems
- **[User/$rescope operation](https://www.medplum.com/docs/auth)** — New operation to change a user's project scope without re-issuing credentials
- **Rate-limit reliability** — Optimized Redis key access for[rate limiting](https://www.medplum.com/docs/rate-limits) , exposed the` RateLimit` header for CORS, and improved WebSocket subscription error messages


**[Maddy Li](https://github.com/maddyli)**


- **Rate-limit administration** — A new[project rate-limits operation](https://www.medplum.com/docs/api/fhir/operations/project-rate-limits) , an admin API endpoint for rate-limit utilization status, an active-consumer index, and a Rate Limits tab on the admin page give operators full visibility into[rate limiting](https://www.medplum.com/docs/rate-limits)
- **NPI Lookup example bot** — A new example bot for NPI lookups using the[NPPES API](https://npiregistry.cms.hhs.gov/)


**[Matt Willer](https://github.com/mattwiller)**


- **Range search** — FHIR search now supports range queries with correct overlap and boundary handling
- **Patient scope hardening** — Patient-scoped tokens are now restricted to their context Patient compartment, and chained search and` _include` types are validated for safety
- **Vendored JSON Patch** — The JSON Patch library is now vendored, and FHIR quota-limit computation was unified into a single helper
- **Larger payload handling** — Oversized request bodies are transformed into a clean HTTP 413 response


**[Matt Long](https://github.com/mattlong)**


- **Transaction reliability** — Serialized transaction state transitions and more consistent repository connection-state tracking improve[site reliability](https://www.medplum.com/blog/2026-roadmap#enterprise-scale--infrastructure) under load
- **` Project.link` search parameter** — New search parameter for navigating linked projects


## Documentation​


May's documentation work spanned self-hosting, compliance, integrations, and the Provider App.


**Self-hosting and reliability**


**[Cody Ebberson](https://github.com/codyebberson)**


- **[Install on DigitalOcean](https://www.medplum.com/docs/self-hosting/install-on-digital-ocean)** — New guide for self-hosting Medplum on DigitalOcean
- **[Uptime and reliability](https://www.medplum.com/docs/uptime)** — New documentation covering Medplum's uptime and site reliability practices
- **[VM context bot security note](https://www.medplum.com/docs/bots/bots-in-production)** — Added a security note for VM context bots in production


**Compliance and scale**


- **[Electronic prior authorization testing](https://www.medplum.com/docs/integration/electronic-prior-auth)** — Testing documentation for electronic prior authorization, part of the[HTI-4 compliance initiative](https://www.medplum.com/docs/compliance/hti-4) ahead of the January 2027 enforcement date ([Cody Ebberson](https://github.com/codyebberson) )
- **[Async batch processing](https://www.medplum.com/docs/fhir-datastore/processing-async-bundles)** — New documentation for asynchronous batch processing, with a note on how FHIR quotas apply ([Finn Bergquist](https://github.com/finnbergquist) ,[Andy Stoneman](https://github.com/andystoneman) )


**Integrations**


- **[Health Gorilla sync-back resources](https://www.medplum.com/docs/integration/health-gorilla/sync-resources-from-health-gorilla)** — Documentation for resources synced back from Health Gorilla ([Maddy Li](https://github.com/maddyli) )
- **[Self-service prescriber enrollment bot](https://www.medplum.com/docs/integration/dosespot/enroll-user)** — Documented the self-service prescriber enrollment bot ([Ian Plunkett](https://github.com/ianplunkett) )


**Provider App and platform**


- **[Spaces feature documentation](https://www.medplum.com/docs/provider/spaces)** — Added Spaces feature documentation for the Provider App, including a walkthrough video ([Everett Williams](https://github.com/everett-williams) )
- **[Charting documentation restructure](https://www.medplum.com/docs/charting)** — Restructured the charting documentation with additional decision-guide content ([Everett Williams](https://github.com/everett-williams) )
- **[Project vs. server-scoped users](https://www.medplum.com/docs/user-management/project-vs-server-scoped-users)** and **[direct external auth](https://www.medplum.com/docs/auth/direct-external-auth)** — Clarified user scoping and external auth setup ([Finn Bergquist](https://github.com/finnbergquist) )
- **[Rate limits operation](https://www.medplum.com/docs/api/fhir/operations/project-rate-limits)** — Documented the new rate-limits operation, plus a clarification on bots with` ProjectMembership` ([Maddy Li](https://github.com/maddyli) )
- **Trust center links** — Updated compliance links to point to the[Medplum Trust Center](https://trust.medplum.com/) ([Reshma Khilnani](https://github.com/reshmakh) )


**Testing infrastructure**


**[Karl Pietrzak](https://github.com/The-Alchemist)**


A broad effort migrated many internal packages — including` hl7` ,` fhir-router` ,` generator` ,` create-medplum` ,` cdk` ,` ccda` ,` mock` , and` definitions` — from Jest to[Vitest](https://vitest.dev/) for faster, more consistent test runs across the monorepo.


## Bug Fixes​


**AWS and CDK**


- Fixed handling of complex server configuration, and corrected CDK to send the prefix instead of the bucket name (contributed by[Jim Fiorato](https://github.com/jimfiorato) )


**Authentication**


- Serialized cross-tab token refresh using the Web Locks API to prevent race conditions across browser tabs (contributed by[Dillon Streator](https://github.com/dillonstreator) )
- Fixed the` RECAPTCHA_SITE_KEY` always reverting to the default value (contributed by[Meade](https://github.com/TheDarkula) )
- Added` defaultProjectFeatures` to the config object keys set (contributed by[Nate Watkins](https://github.com/nate-watkins) )


**FHIR and Agent**


- Fixed indexing of fragment` CodeSystem` resources in the coding table (contributed by[Jeffry Looijestijn](https://github.com/jeffrylooijestijn) )
- The Agent now returns an explicit error when an upgrade artifact is missing (contributed by[Agustin Bereciartua Castillo](https://github.com/galenzo17) )
- Display of` onBehalfOf` was added to History, Timeline, and Blame views (contributed by[Alex Lin](https://github.com/alexanderxlin) )


## Releases​


- [v5.1.10](https://github.com/medplum/medplum/releases/tag/v5.1.10) — May 1
- [v5.1.11](https://github.com/medplum/medplum/releases/tag/v5.1.11) — May 6
- [v5.1.12](https://github.com/medplum/medplum/releases/tag/v5.1.12) — May 17
- [v5.1.13](https://github.com/medplum/medplum/releases/tag/v5.1.13) — May 20


## Looking Ahead​


May brought the scheduling operation suite to completion —` $find` ,` $hold` ,` $book` ,` $confirm` , and` $cancel` now cover the full booking lifecycle — and moved the Provider App onto an Appointment-based flow with medication ordering and order set import. Passwordless magic-link login and GuardDuty malware protection strengthen the platform's security posture.


Join us on[Discord](https://discord.gg/medplum) to share feedback or follow along on[GitHub](https://github.com/medplum/medplum) .
