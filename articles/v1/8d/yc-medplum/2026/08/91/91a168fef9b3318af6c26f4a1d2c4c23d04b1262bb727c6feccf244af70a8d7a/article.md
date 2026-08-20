---
schema_version: "1.0.0"
document_id: "91a168fef9b3318af6c26f4a1d2c4c23d04b1262bb727c6feccf244af70a8d7a"
company_key: "yc-medplum"
company: "Medplum"
source_id: "yc-medplum-atom-baaaecda9acc"
canonical_url: "https://www.medplum.com/blog/july-2026-update"
published_at: "2026-08-01T00:00:00+00:00"
first_seen_at: "2026-08-02T01:37:44.587425+00:00"
fetched_at: "2026-08-02T01:37:45.621427+00:00"
content_hash: "sha256:de934527f0041368efb1360b9ab5be191faf4f9f9451ec81ade060f12db75510"
---

# Medplum Monthly Update - July 2026

The headline this month: **Medplum earned the[2026 FHIRplace Participant emblem](https://www.medplum.com/blog/fhirplace-participant-2026) from Drummond Group** for electronic prior authorization testing, ahead of the January 2027 enforcement date. July was another heavy shipping month — 206 commits from 28 contributors and five patch releases, v5.1.23 through v5.1.27.


On the revenue cycle side, real-time[insurance eligibility checks](https://www.medplum.com/docs/billing/insurance-eligibility-checks) arrived as a FHIR operation of their own, and` Claim/$submit` learned to route preauthorization requests to their own processor. Elsewhere,[server-scoped subscriptions](https://www.medplum.com/docs/subscriptions/server-scoped-subscriptions) give project-per-tenant deployments one central data-processing flow, a concentrated round of authentication hardening tightened JWT validation, outbound fetch, and MFA, and the[Decision Guides](https://www.medplum.com/docs/decision-guides) are now published natively in the docs. A first DICOMweb implementation also landed in[Alpha](https://www.medplum.com/docs/compliance/alpha-beta) for teams beginning to work with imaging.


All of this continues to drive forward our[2026 roadmap priorities](https://www.medplum.com/blog/2026-roadmap) .


## Features​


### Provider App​


**[David Yanez](https://github.com/techdavidy)** and **[Kevin Shaw](https://github.com/kevinwadeshaw)**


The[Provider App](https://provider.medplum.com/) focused on the lab review and messaging workflows this month, plus a round of consistency work across the inbox surfaces ([roadmap](https://www.medplum.com/blog/2026-roadmap#provider-application) ):


- **Message settings in Messages** — Thread-level message settings are now reachable from the conversation itself, so a user can adjust how a thread behaves without leaving it
- **Lab review depth** — The labs view now includes` DiagnosticReport` and` ServiceRequest` resources together, shows result details even when the ordering` ServiceRequest` cannot be read, and renders an ordering-requester and performing-labs footer on the report ([results and review docs](https://www.medplum.com/docs/labs-imaging/results-and-review) )
- **Document filters** — The patient Documents tab filters by source, separating lab-delivered documents from other uploads
- **Consistent detail panes** — The thread inbox and tasks views both moved to` ListWithDetailPane` , and` /new` routes now work for faxes, tasks, and communications so a user can deep-link straight into a compose screen
- **Charting** — An encounter can now be created without first choosing a care template ([Philip Knott](https://github.com/Filupnot) ), and address now displays on` DiagnosticReportDisplay`
- **Medplum App polish** — Current project shown in the navbar, a resource-type header on the resource list page, elements visually separated from metadata fields, inline errors when an AutoComplete option set is missing, a login link on the registration screen, a keyboard-accessible "View Password" toggle, and a contrast fix in the project picker (all[Philip Knott](https://github.com/Filupnot) )


### Scheduling​


**[Noah Silas](https://github.com/noahsilas)**


[Scheduling](https://www.medplum.com/blog/2026-roadmap#scheduling) work moved from the operation layer to the day-to-day calendar experience:


- **Schedule navigation and creation** — The[Provider App schedule](https://www.medplum.com/docs/provider/schedule) adds a schedule switcher and an inline "Create Schedule" path, so a practitioner without a schedule can create one in place instead of hitting a dead end
- **Appointment details** — The details drawer now always reveals from the calendar, and shows every appointment participant rather than only the first, which matters for group visits and appointments with multiple practitioners
- **Cleaner` $find` /` $book` results** —` Slot.serviceType` was removed from[$find](https://www.medplum.com/docs/scheduling/appointment-find) and[$book](https://www.medplum.com/docs/scheduling/appointment-book) responses, since service type belongs to the appointment request, not the returned slot
- **` useSchedulingResources`** — Scheduling resource loading was extracted into a reusable hook for teams building their own calendar UI


### Revenue Cycle and Billing​


**[David Yanez](https://github.com/techdavidy)** , **[Cody Ebberson](https://github.com/codyebberson)** , and **[Finn Bergquist](https://github.com/finnbergquist)**


[Revenue cycle](https://www.medplum.com/blog/2026-roadmap#revenue-cycle--billing) work extended the server-side operation pattern from claims to the two steps that surround them — eligibility before the visit and payer response after it:


- **` CoverageEligibilityRequest/$submit`** — A real-time[insurance eligibility check](https://www.medplum.com/docs/billing/insurance-eligibility-checks) as a FHIR operation, so front-desk and intake flows can verify coverage at the point of scheduling instead of batching it
- **Prior authorization routing in` Claim/$submit`** — The claim submit operation now accepts a` Bundle` as well as a bare` Claim` , and dispatches claims with` use: preauthorization` to their own configured processor instead of the standard claim path. The core server stays vendor-neutral here: either processor is a custom operation backed by a Bot, so a project points each path at whatever it uses
- **Claim response tracking** — Documentation and webhook wiring for claim acknowledgements and electronic remittance advice, so a submitted claim's status and payment can be followed to resolution rather than going dark after submission ([billing docs](https://www.medplum.com/docs/billing) )
- **CMS-1500 accuracy** — Box 25 and Box 24B were reading from the wrong sources on the generated PDF; both now read the correct fields ([Andrew Wong](https://github.com/ajwwong) )


### Identity and Access​


**[Cody Ebberson](https://github.com/codyebberson)** and **[Derrick Farris](https://github.com/ThatOneBro)**


A concentrated round of authentication hardening, plus admin tooling for the MFA features that shipped in June:


- **JWT best practices** — Audience validation now follows current JWT best-practice guidance, and the preferred JWKS algorithm moved to ES384
- **Basic Auth aligned with client credentials** — Basic authentication now follows the same validation path as the[client credentials flow](https://www.medplum.com/docs/auth/client-credentials) , removing a divergence between two ways of authenticating the same client
- **[MFA](https://www.medplum.com/docs/auth/mfa) administration** — A project admin UI for MFA status and password reset, MFA secrets generated on demand at enrollment rather than ahead of time, and project branding applied to MFA emails and authenticator app entries so the second factor looks like the customer's product
- **Default access policies** — Admin and Practitioner[default access policies](https://www.medplum.com/docs/access/access-policies) ship out of the box, with documentation on how they reconcile with project admin
- **Client application options** — A` ClientApplication` can skip the scopes consent screen for trusted first-party apps, and` $export` now works with read-only scopes
- **Security hardening** — SSRF-safe outbound fetch, a maximum password length enforced across every endpoint, project details on the security sessions tab, and MCP` fhir-request` pinned to the server origin


### Subscriptions, Events, and the Agent​


**[Derrick Farris](https://github.com/ThatOneBro)** and **[Finn Bergquist](https://github.com/finnbergquist)**


The big addition here is a new subscription scope for multi-tenant deployments:


- **[Server-scoped subscriptions](https://www.medplum.com/docs/subscriptions/server-scoped-subscriptions)** — A rest-hook` Subscription` that fires across every project on the server, not just its own. Project-per-tenant deployments can now run one central data-processing flow instead of duplicating a subscription into every tenant project. It requires the[Super Admin](https://www.medplum.com/docs/self-hosting/super-admin-guide) project and a server config flag, so it is self-hosted only, and firings emit system-scoped` AuditEvent` s
- **[Subscription extensions](https://www.medplum.com/docs/subscriptions/subscription-extensions)** — Multiple` subscription-supported-interaction` extensions on a single subscription, and an interaction option in the Resend Subscriptions modal ([Maddy Li](https://github.com/maddyli) )
- **Bot channel audit trail** —` AuditEvent` s are now logged for Bot channel subscriptions, closing a gap in the record of what fired and why ([Ian Plunkett](https://github.com/ianplunkett) )


On the on-premise[Agent](https://www.medplum.com/docs/agent) — Medplum's connectivity service for HL7 interfaces and legacy systems — building on the durable message queue that shipped in June. The[Agent documentation](https://www.medplum.com/docs/agent) covers[installation requirements](https://www.medplum.com/docs/agent/requirements) ,[configuration](https://www.medplum.com/docs/agent/configuration) , and[troubleshooting](https://www.medplum.com/docs/agent/troubleshooting) :


- **Auto-retry for committed-but-failed messages** — Messages that were committed to the durable queue but failed downstream are now retried automatically rather than needing manual intervention, which pairs with how[acknowledgement modes](https://www.medplum.com/docs/agent/acknowledgement-modes) decide when a message counts as committed
- **[$fetch-logs](https://www.medplum.com/docs/agent/fetch-logs) cursor pagination** — Log retrieval pages by cursor, so a long-running Agent's logs can be pulled without truncation
- **Version handling** — A version picker on the Agent Tools page for[upgrades](https://www.medplum.com/docs/agent/upgrade) , and a fix preventing the reported Agent version from being clobbered by a connect[status](https://www.medplum.com/docs/agent/status) update
- **Log preservation on uninstall** —[Uninstalling the Agent](https://www.medplum.com/docs/agent/manual-uninstall) no longer deletes` .log` files ([devnoct](https://github.com/devnoct) )


### Terminology and Search​


**[Matt Willer](https://github.com/mattwiller)**


[Terminology](https://www.medplum.com/docs/terminology) work this month was mostly about making large code systems behave at scale:


- **Faster terminology queries** — General query performance improvements, plus batched lookup of multiple codes in a single[ValueSet/$validate-code](https://www.medplum.com/docs/api/fhir/operations/valueset-validate-code) call instead of one round trip per code
- **Code prefix filtering** —[ValueSet/$expand](https://www.medplum.com/docs/api/fhir/operations/valueset-expand) supports filtering on a code string prefix, which is how most code-picker UIs actually search
- **Import guardrails** —[CodeSystem/$import](https://www.medplum.com/docs/api/fhir/operations/codesystem-import) now bounds import size and logs oversized imports, and the` CodeSystem` and` ConceptMap` import permission models were aligned ([Cody Ebberson](https://github.com/codyebberson) )
- **Better code selection** — Complete code systems are preferred over example ones, and retired resources are skipped during lookup ([Maddy Li](https://github.com/maddyli) )


On the[search](https://www.medplum.com/docs/search/basic-search) side, a server config setting controls which search parameters are enabled,` _include` and` _revinclude` now honor the declared target type ([Myk](https://github.com/oldrobotdev) ), and the SQL and in-memory repositories share a single` _include` /` _revinclude` implementation so tests and production behave the same way ([Cody Ebberson](https://github.com/codyebberson) ).


### Enterprise: Data Warehouse, Database, and Async Jobs​


**[Karl Pietrzak](https://github.com/The-Alchemist)** and **[Matt Long](https://github.com/mattlong)**


[Enterprise scale and infrastructure](https://www.medplum.com/blog/2026-roadmap#enterprise-scale--infrastructure) work centered on making the[analytics](https://www.medplum.com/docs/analytics) export easier to enable and operate, and the data layer easier to run:


- **Faster incremental syncs** — The data warehouse export reads its last-updated watermark from Iceberg manifest stats instead of scanning, retrieves watermarks asynchronously, caches them, and defers Postgres connections until needed. Together these cut both the time and the read load of an incremental sync
- **Sync robustness** — DuckDB connections reset per table with a bounded Postgres reader load, missing` meta.project` values are coalesced during sync, and DuckDB now sets an` application_name` so warehouse queries are identifiable in Postgres monitoring
- **Async batch maturity** — Async batch became reentrant, stopped redownloading result chunks it already has locally, and resets processor state correctly on transaction retry ([Matt Willer](https://github.com/mattwiller) )
- **Job observability** — Per-queue in-flight and completed job metrics, queue active counts, async batch entry throughput, and a` bullmq`` globalConcurrency` setting for tuning worker load
- **Database operations** — Additional database configuration options, repository resource-type access tracking, direct DB manipulation moved off the` Repository` path, cloned repositories for out-of-band saves, and history tombstones enriched with metadata
- **Reliability details** — Reading a` Binary` with no content is handled gracefully, and the FHIR quota in-memory block maintains its reset timestamp correctly ([Matt Willer](https://github.com/mattwiller) )


### AI​


**[David Yanez](https://github.com/techdavidy)** and **[Maddy Li](https://github.com/maddyli)**


[AI](https://www.medplum.com/blog/2026-roadmap#ai) work refined the[Spaces](https://www.medplum.com/docs/provider/spaces) workspace rather than expanding it:


- **Real-time questionnaire responsiveness** — The AI real-time questionnaire form handles narrow viewports and mid-session layout changes, which matters for documenting on a tablet during a visit
- **Voice controls** —` useWhisper` supports mute, so a clinician can pause capture without ending the session
- **Docs search in chat** — An Algolia search button in the chat surface, so a question about Medplum itself can be answered without leaving the workspace


### Imaging and DICOM (Alpha)​


**[Cody Ebberson](https://github.com/codyebberson)**


A first implementation of the DICOMweb service suite landed this month, so imaging studies can live in the same datastore, under the same access policies, as the rest of a patient's record:


- **DICOMweb services** — QIDO-RS for querying studies, series, and instances; WADO-RS for retrieving studies, series, frames, and metadata; STOW-RS for storing instances; and WADO-URI for direct object access
- **Three new resource types** —[DicomStudy](https://www.medplum.com/docs/api/fhir/medplum/dicomstudy) ,[DicomSeries](https://www.medplum.com/docs/api/fhir/medplum/dicomseries) , and[DicomInstance](https://www.medplum.com/docs/api/fhir/medplum/dicominstance) model the DICOM hierarchy as Medplum resources with their own search parameters
- **Viewer integration** — Studies can be opened in the OHIF viewer, and a background worker handles instance ingestion and metadata extraction


Alpha


DICOM support is an[Alpha](https://www.medplum.com/docs/compliance/alpha-beta) feature. The API shape and behavior may change between releases without a migration guide, so treat it as a prototyping and early-feedback surface rather than something to build production imaging workflows on yet. If you are working with imaging, we would like to hear from you athello@medplum.com .


### Developer Experience​


**[Noah Silas](https://github.com/noahsilas)** and **[Finn Bergquist](https://github.com/finnbergquist)**


Work aimed at teams building on the[React component library](https://www.medplum.com/docs/react) :


- **` @medplum/storybook`** — Storybook moved into its own package with a deploy script, separating component documentation from the` @medplum/react` build
- **Theme presets** — A set of theme presets in Storybook, so a team can preview Medplum components against something closer to their own brand before committing to a theme
- **` resourceModified` events** —` MedplumClient` emits` resourceModified` events, giving apps a hook for cache invalidation and live UI updates without polling
- **Testing ergonomics** —` MockClient` control methods moved into a` .mock` namespace to keep the mock surface distinct from the real client API, plus new` toHaveStatus` and` toContainExactly` matchers ([Matt Long](https://github.com/mattlong) ) and safer` EventTarget` listener exception handling
- **Component props** — An` additionalColumns` prop on` SearchControl` , and` questionnaire-optionExclusive` support in` QuestionnaireForm` ([Derrick Farris](https://github.com/ThatOneBro) ,[David Yanez](https://github.com/techdavidy) )


## Compliance​


**[Reshma Khilnani](https://github.com/reshmakh)** and **[Cody Ebberson](https://github.com/codyebberson)**


**Medplum earned the 2026 FHIRplace Participant emblem from Drummond Group** for our work in electronic prior authorization testing.[CMS-0057-F and HTI-4](https://www.medplum.com/docs/compliance/hti-4) require payers and providers to support FHIR-based electronic prior authorization with enforcement beginning January 2027, and Medplum handles ePA transactions on both sides. The[full post has the details](https://www.medplum.com/blog/fhirplace-participant-2026) . Other[compliance](https://www.medplum.com/blog/2026-roadmap#compliance-h1-2026) work this month:


- **Electronic prior authorization** — Continued progress on electronic prior authorization toward the[HTI-4](https://www.medplum.com/docs/compliance/hti-4) requirements, ahead of the January 2027 enforcement date
- **[ONC certification criteria](https://www.medplum.com/docs/compliance/onc)** — Updated documentation on which ONC criteria Medplum addresses and how
- **[Inferno](https://inferno.healthit.gov/) testing** — The` CapabilityStatement` now advertises the token introspection URL, which the Inferno certification suite requires. Our[SMART App Launch guide](https://www.medplum.com/docs/integration/smart-app-launch) covers pointing an Inferno client at a Medplum project, and the[ONC documentation](https://www.medplum.com/docs/compliance/onc) tracks which criteria we address
- **[Patient/$match](https://www.medplum.com/docs/api/fhir/operations/patient-match)** — Patient matching documentation was aligned with the actual behavior of the match operation, following June's move to CMS matching criteria. The docs now lay out the[approved CMS matching combinations](https://www.medplum.com/docs/api/fhir/operations/patient-match#approved-cms-matching-combinations) — the identity-attribute sets strong enough to return a` certain` match — along with the uniqueness gate that governs when a record is released and the match grades applied to everything else. Note that the CMS framework is a draft proposal and subject to change as the guidelines evolve
- **Supply chain and hardening** — Workflow permissions corrected for the[OpenSSF Scorecard](https://www.medplum.com/security#openssf-scorecard) run, a transitive dependency advisory resolved, and the JWT, SSRF, and password-length hardening described above. Current scores and badges are published on our[security page](https://www.medplum.com/security) , including[OpenSSF Best Practices](https://www.medplum.com/security#openssf-best-practices) and[Security Scorecard](https://www.medplum.com/security#security-scorecard)


## Documentation​


**[Maddy Li](https://github.com/maddyli)** and **[Everett Williams](https://github.com/everett-williams)**


- **[Should you Self-Host?](https://www.medplum.com/docs/self-hosting/considerations)** — A frank guide to evaluating self-hosting: what upgrades, Redis and Postgres operations, and compliance obligations actually cost month to month. Worth reading before committing either way
- **[Decision Guides](https://www.medplum.com/docs/decision-guides)** — The decision guides are now published natively in the docs rather than as separate documents, covering[intake](https://www.medplum.com/docs/decision-guides/intake) ,[charting](https://www.medplum.com/docs/decision-guides/charting) ,[messaging](https://www.medplum.com/docs/decision-guides/messaging) ,[access control](https://www.medplum.com/docs/decision-guides/access-control) ,[referrals](https://www.medplum.com/docs/decision-guides/referrals) ,[e-prescribing](https://www.medplum.com/docs/decision-guides/e-prescribe) , and[revenue cycle](https://www.medplum.com/docs/decision-guides/rcm-billing) — each one a scoping conversation about requirements and FHIR modeling decisions, linked from the corresponding technical docs. PDF and Word downloads are available for sharing with non-technical stakeholders


**Provider App and platform**


- **[Rate limits](https://www.medplum.com/docs/rate-limits)** — Clarified rate limit behavior with configuration instructions, and the[$rate-limits](https://www.medplum.com/docs/api/fhir/operations/project-rate-limits) endpoint now respects a` UserConfiguration` override
- **[Default access policies](https://www.medplum.com/docs/access/access-policies)** — Documentation on the built-in Admin and Practitioner policies and how they reconcile with project admin
- **[MFA routes and flows](https://www.medplum.com/docs/auth/mfa)** — The full MFA surface documented end to end ([Derrick Farris](https://github.com/ThatOneBro) )
- **[$ccda-export](https://www.medplum.com/docs/api/fhir/operations/ccda-export)** — Parameter documentation and the accompanying UI component
- **[Self-hosted token exchange](https://www.medplum.com/docs/auth/token-exchange)** and **[subscription extensions](https://www.medplum.com/docs/subscriptions/subscription-extensions)** — Both expanded ([Finn Bergquist](https://github.com/finnbergquist) )
- **[Products page](https://www.medplum.com/products)** — Redesigned, with the architecture diagram carried over to the docs home page ([Kevin Shaw](https://github.com/kevinwadeshaw) ,[Everett Williams](https://github.com/everett-williams) )
- **[Project.defaultProfile](https://www.medplum.com/docs/access)** — A dedicated section on default profiles ([Raphael Malikian](https://github.com/rtmalikian) ), plus typo and casing fixes across the docs ([Granis87](https://github.com/RobinALG87) )


**Integrations**


- **[Electronic prescribing](https://www.medplum.com/docs/integration)** — The e-prescribe integration moved out of beta in the docs, with new documentation for the multi-medication cart workflow ([Andy Stoneman](https://github.com/andystoneman) )
- **[Lab ordering](https://www.medplum.com/docs/integration)** — Eligibility criteria for lab connectivity documented, along with patient profile and diagnosis code requirements ([Tim Ray](https://github.com/timray721) ,[Maddy Li](https://github.com/maddyli) )
- **[SMS and messaging](https://www.medplum.com/docs/communications)** — Public documentation for the SMS and telephony messaging integration, including inbound messages ([Andy Stoneman](https://github.com/andystoneman) )


## Bug Fixes​


**FHIR and billing**


- Read the correct sources for CMS-1500 Box 25 and Box 24B (contributed by[Andrew Wong](https://github.com/ajwwong) )
- Honor the declared target type in` _include` and` _revinclude` (contributed by[Myk](https://github.com/oldrobotdev) )


**Client and CLI**


- Use` statusOptions` in` pollStatus` so status polls do not resend the request body (contributed by[Aidan-Thomas Anderson](https://github.com/atanderson19) )
- Fail CLI bot deploy when the server returns an error outcome, instead of exiting successfully (contributed by[alexei](https://github.com/driavysinus) )
- Improve the error message when the email feature is unavailable (contributed by[Agustin Bereciartua Castillo](https://github.com/galenzo17) )


**Agent**


- Preserve` .log` files during Agent uninstall (contributed by[devnoct](https://github.com/devnoct) )


## From the Blog​


Longer-form writing published this month:


- **[Everself: Enhanced Obesity Care](https://www.medplum.com/blog/everself-case-study)** by Petch Jirapinyo and Finn Bergquist — How Everself built a longitudinal care program around an outpatient endoscopic weight loss procedure, for the million-plus patients a month who drop off GLP-1 medications
- **[Preparing for 2027 Prior Auth Regulatory Requirements](https://www.medplum.com/blog/fhirplace-participant-2026)** by Reshma Khilnani — What CMS-0057-F and HTI-4 require, and where Medplum's ePA support stands


## Releases​


- [v5.1.23](https://github.com/medplum/medplum/releases/tag/v5.1.23) — July 1
- [v5.1.24](https://github.com/medplum/medplum/releases/tag/v5.1.24) — July 6
- [v5.1.25](https://github.com/medplum/medplum/releases/tag/v5.1.25) — July 9
- [v5.1.26](https://github.com/medplum/medplum/releases/tag/v5.1.26) — July 10
- [v5.1.27](https://github.com/medplum/medplum/releases/tag/v5.1.27) — July 24


## Looking Ahead​


July put the electronic prior authorization work on the record with the FHIRplace emblem, filled in the revenue cycle picture with real-time eligibility and preauthorization routing, and gave multi-tenant deployments a server-wide subscription scope. The authentication hardening and terminology performance work are the kind of changes nobody asks for by name but everybody feels. Imaging is the early one to watch — DICOM is in Alpha now, and feedback from teams working with imaging will shape where it goes.


Two things on the near calendar. Today we are hosting the[Agentic Healthcare Hackathon with Y Combinator](https://www.medplum.com/blog/yc-medplum-hackathon-2026) at the YC office in San Francisco. And[PlumCon 2026](https://www.medplum.com/blog/plumcon-2026) is on September 3 — the lineup is posted, and registration is open.


Join us on[Discord](https://discord.gg/medplum) to share feedback or follow along on[GitHub](https://github.com/medplum/medplum) . To learn more about anything in this update, or to talk through how it applies to what you are building, contact us athello@medplum.com .
