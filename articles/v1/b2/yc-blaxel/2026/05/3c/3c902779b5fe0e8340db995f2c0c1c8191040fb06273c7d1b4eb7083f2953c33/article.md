---
schema_version: "1.0.0"
document_id: "3c902779b5fe0e8340db995f2c0c1c8191040fb06273c7d1b4eb7083f2953c33"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/zero-data-retention-ai-sandboxes"
published_at: "2026-05-21T16:09:10+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:711346d4bb0f065770fac6764a3b222fa9048037c5601d284a211a5a8dcff436"
---

# Zero data retention AI sandboxes: how to evaluate vendor claims

Your vendor's sales deck says "zero data retention (ZDR) compliant." After signing, your security team runs a routine review and finds retention surfaces the contract never named. Execution logs sit in a centralized backend while standby snapshots serialize full RAM to disk. Observability spans carry request payloads to subprocessors outside your Data Processing Agreement (DPA).


[GDPR Article 5](https://gdpr-info.eu/art-5-gdpr/) violations have driven substantial fines tracked by the[Enforcement Tracker](https://www.enforcementtracker.com/?insights) database. Regulated buyers need proof that ZDR holds across every data surface. Without it, deals stall in procurement indefinitely.


This guide breaks down what ZDR means inside a sandbox runtime. You'll learn how to pressure-test vendor claims across data surfaces contracts miss, and which architectural patterns survive procurement.


## What zero data retention actually means for AI sandboxes


Most teams encounter ZDR in the context of LLM API providers. Prompts and completions aren't stored beyond the request lifecycle. The provider processes your input, returns a response, and retains nothing. OpenAI's[ZDR API specification](https://developers.openai.com/api/docs/guides/your-data) documents the lifecycle: inputs and outputs are never logged and never retained for application state. That definition covers a single data surface, the API boundary, and works well for stateless inference calls.


Sandbox ZDR is a materially harder problem. A sandbox runtime writes to disk, captures stdout, holds state in memory, and emits telemetry. An API call produces prompt and completion artifacts. A sandbox execution generates files, caches dependencies, writes logs, snapshots memory, and emits distributed traces. Each artifact lands on a different storage surface with its own retention behavior. The vendor's ZDR commitment must cover all of them to hold up under audit.


The reframe for procurement teams: ZDR in sandboxes is an architectural property of how the execution environment handles state. GDPR Article 5(1)(e) requires personal data be kept "no longer than is necessary." That obligation applies to every layer where data lands.


Every evaluation should map four distinct data surfaces:


- **Code and filesystem artifacts:** Files written, volumes mounted, dependencies cached during execution.
- **Execution logs and stdout/stderr:** Process output captured by the runtime and shipped to log backends.
- **Memory snapshots and standby state:** RAM contents serialized when sandboxes pause or hibernate.
- **Observability traces and telemetry:** Spans, metrics, and request metadata flowing to monitoring pipelines.


The next section breaks down why each one creates independent retention risk. Each surface has its own technical lifecycle, its own contractual treatment, and its own subprocessor dependencies. Conflating them under generic "customer data" language is exactly what lets retention slip past procurement review.


## The four data surfaces vendors gloss over


Most ZDR compliance gaps trace back to a data surface the contract didn't name. Generic DPA language covering "customer data" hides where retention actually occurs.


### Code and filesystem artifacts


During execution, sandboxes write generated files, mount volumes, and cache dependencies. Each artifact creates a retention surface independent of the sandbox process. Containers allowed to mount host directories can change files in those directories, and those files[persist after termination](https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-190.pdf) . The same NIST Application Container Security Guide flags data exfiltration via shared storage as a recognized risk category.


[Template caching](https://docs.blaxel.ai/Sandboxes/Templates) adds another layer. Vendors pre-build image layers to speed sandbox creation, and those layers can carry tenant-specific build artifacts across sessions. The operational question is whether the filesystem dies with the instance or persists in a[persistent volume](https://docs.blaxel.ai/Volumes/Overview) . A 2023 study from RWTH Aachen University analyzed 337,171 Docker Hub images. The researchers found[28,621 of those images](https://www.bleepingcomputer.com/news/security/thousands-of-images-on-docker-hub-leak-auth-secrets-private-keys/) contained private keys and API secrets.


If your vendor recycles base images without scrubbing, credentials from one execution can leak into another tenant's environment. Ask whether sandbox filesystem state is bounded to the execution lifecycle.


### Execution logs and stdout/stderr


Logs are the most common ZDR leak point. In cloud-native architectures, applications write logs to stdout/stderr. The runtime redirects those streams to log files on the node. Separate logging agents then ship and store them centrally. That retention schedule is independent of the workload lifecycle. "We don't retain customer code" rarely means "we don't retain the log output of that code."


GDPR's storage limitation principle applies directly: personal data collected through execution logs can't be stored indefinitely. The path from process output to compliance exposure runs through centralized log aggregation. Retention windows on the vendor's logging backend extend that exposure.


Support staff often query logs during incident triage. A vendor claiming ZDR while shipping stdout to a logging service hasn't eliminated the data surface. The service's own retention window defines a new one. Require your vendor to document the full[log pipeline](https://docs.blaxel.ai/Sandboxes/Log-streaming) :


- **Capture mechanism:** What converts process output into log entries.
- **Shipping destination:** Where those entries flow and which agent ships them.
- **Retention window:** How long the destination keeps them.
- **Access controls:** Who can query or export the stored logs.


### Memory snapshots and standby state


Snapshot storage is the sandbox-specific surface no LLM-vendor ZDR template covers. When a sandbox pauses or enters standby, the platform either suspends in-memory or writes a snapshot to disk.[Firecracker snapshots](https://github.com/firecracker-microvm/firecracker/blob/main/docs/snapshotting/snapshot-support.md) save "the full state of guest memory and emulated hardware." The resulting guest memory file contains complete RAM contents at snapshot time.


Any secret, payload, or generated artifact resident in memory at pause time persists in that file. Memory dumps capture in-flight cryptographic keys, session tokens, and intermediate user data. Instant resume from standby requires snapshot storage, and snapshot storage by definition retains state.


Enterprise teams evaluating ZDR need clear answers. Does the vendor's[standby mechanism](https://docs.blaxel.ai/Sandboxes/Standby-control) create persistent memory snapshots? What happens to those snapshots when the sandbox is deleted?


### Observability traces and telemetry


Spans, metrics, and distributed traces routinely carry metadata such as timing, context, attributes, tags, logs, and error information. OpenTelemetry and similar frameworks collect telemetry data but can't determine what's sensitive in a specific context. The implementer bears full responsibility.


Auto-instrumentation libraries for HTTP frameworks and ORMs can capture telemetry such as request metadata. Some support capturing HTTP headers by configuration, all without explicit developer code changes. Once that data enters the[observability pipeline](https://docs.blaxel.ai/Observability/Overview) , it flows to vendor-side collectors and downstream subprocessors. Default retention windows create a fixed compliance exposure.


Observability backends like Grafana Tempo and Prometheus require explicit scrutiny of retention configuration. "PII-safe by default" claims typically rely on customer-side scrubbing the vendor can't enforce. If personally identifiable information (PII) reaches a backend before scrubbing, those retention settings define your minimum exposure period.


## Why most zero data retention claims don't survive procurement review


Marketing pages make ZDR sound binary: on or off. DPAs reveal it's conditional, partial, and subprocessor dependent. Three structural problems surface repeatedly during enterprise procurement review.


Problem Why it fails procurement


1 Opt-in defaults ZDR is a flag, not the default. Every sandbox created before activation operated without it.


2 Subprocessor sprawl Logging, analytics, and observability subprocessors retain on their own schedules.


3 Behavioral versus architectural Process-based deletion breaks under incident response, backups, and forensic holds.


The first gap is between "we support ZDR" and "ZDR is the default tenant configuration." Opt-in ZDR means logs and traces flow to standard storage until someone flips a flag. Procurement teams correctly flag this as residual risk, since every sandbox created before activation operated without it.


The EDPB's[2025 erasure enforcement report](https://www.edpb.europa.eu/system/files/2026-02/edpb_cef-report_2025_right-to-erasure_en.pdf) flagged a recurring failure. Controllers often don't define clear retention periods or communicate them in privacy notices. Opt-in ZDR without documented pre-activation retention state is the procurement equivalent of that finding.


Subprocessor sprawl creates the second exposure. The prime contract covers the vendor but not the observability vendor, log aggregator, or analytics pipeline downstream.[EDPB Opinion 22/2024](https://www.edpb.europa.eu/system/files/2024-10/edpb_opinion_202422_relianceonprocessors-sub-processors_en.pdf) requires controllers to keep a current list of every processor and sub-processor. Names, addresses, and contact persons must be "readily available at all times."


Each subprocessor has its own retention behavior, and flow-down obligations may not cover where your data lands. A short deletion window means nothing if the vendor's logging subprocessor enforces a longer retention minimum.


Behavioral versus architectural guarantees mark the third gap. "We delete on a regular cycle" is a process commitment. It breaks under incident response, forensic preservation holds, backup architecture, or process failure. Architectural ephemerality means data never lands in durable storage in the first place.


That approach survives operational failure modes.[NIST SP 800-88r2](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-88r2.pdf) distinguishes between "Clear" (logical overwrite via read/write commands) and "Purge" (logical or physical techniques including cryptographic erase). Teams should evaluate whether deletion depends on a process completing as designed.


[Perpetual sandbox platforms](https://blaxel.ai/sandbox) like Blaxel close this gap differently from container platforms. MicroVM isolation makes the execution environment itself the deletion boundary. In ZDR-oriented workflows, the entire microVM is deleted rather than kept in standby with retained snapshot state.


## How to scrutinize vendor zero data retention claims contractually


Use this diligence checklist before signing. It targets the specific failure modes that surface during legal and security review.


### 1. Map every data surface in the DPA


Require the DPA to enumerate each of the four surfaces explicitly: filesystem artifacts, execution logs, memory snapshots, and observability telemetry. Generic language like "customer data" hides retention risk. EDPB draft[Recommendations 1/2026](https://www.edpb.europa.eu/system/files/2026-01/edpb_recommendations202601_bcr-p_v1_en.pdf) on Binding Corporate Rules for Processors (BCR-P), adopted for public consultation, address processor BCR requirements.


Surface Retention window to specify Deletion mechanism to specify


Filesystem artifacts How long files persist after sandbox termination Automatic on-delete vs. operator-triggered cleanup


Execution logs Retention at the centralized logging backend Purge schedule, including subprocessor flow-down


Memory snapshots Standby lifetime and post-deletion behavior Whether snapshots are created in ZDR mode


Observability telemetry Backend retention defaults Customer-side scrubbing vs. vendor-enforced scrubbing


Demand named retention windows per surface. A DPA that commits to deleting customer data after termination provides incomplete coverage. Execution logs ship to a centralized backend immediately.


Snapshot storage persists as long as the sandbox exists. The two surfaces need separate retention commitments. Article 5(1)(e) requires personal data be kept no longer than necessary. Controllers must set retention time limits or periodic review points. Start by listing the four surfaces in your procurement questionnaire. Require the vendor to document retention behavior and deletion mechanism for each one separately.


### 2. Request the subprocessor inventory with retention windows


Ask for the full subprocessor list with each provider's role, data categories accessed, and contractual retention terms. EDPB Opinion 22/2024 requires named entities with addresses and contact persons. Category lists like "cloud infrastructure providers" are insufficient.


Watch for observability vendors, log aggregators, and customer support platforms, since data routinely lives in those systems for extended periods. An[IAPP case study](https://iapp.org/news/a/cybersecurity-and-the-cloud-lessons-from-fcc-cloud-breach-enforcement) on FCC cloud-breach enforcement examined vendor agreements with data deletion requirements.


Even those agreements highlighted the limits of contractual language alone. Pair the subprocessor inventory with evidence of operationalized deletion: automated lifecycle policies, deletion certification timelines, and documented audit mechanisms.


### 3. Test the tenant-deletion scenario in writing


Require the vendor to document what happens after workspace deletion or ZDR-mode activation. The documentation should specify which surfaces purge immediately, which purge on a schedule, and which require manual operator action. The[EU Data Act](https://iapp.org/news/a/eu-data-act-all-your-threshold-questions-answered) introduces a mandatory framework for cloud switching scenarios. The full erasure requirement includes exceptions where the parties agree otherwise or where retention is required by law.


Reject any answer that includes "we work with our subprocessors to delete." That phrase signals a behavioral commitment layered on another behavioral commitment, with no architectural guarantee underneath. Require a written deletion certification within a documented timeframe. Require the vendor to obtain and provide sub-processor deletion certifications as well.


### 4. Negotiate audit rights and breach windows


Reserve the right to audit ZDR posture regularly or after material architecture changes.[GDPR Article 28(3)(h)](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/accountability-and-governance/contracts-and-liabilities-between-controllers-and-processors-multi/what-needs-to-be-included-in-the-contract/) requires processors to "allow for and contribute to audits, including inspections." The operative language is "allow for and contribute to."


Structure audit rights in tiers:


- **Baseline certifications:** SOC 2 Type II, ISO 27001,[ISO 27701:2025](https://www.iso.org/standard/27701) , and a HIPAA Business Associate Agreement (BAA) for workloads handling protected health information.
- **Compliance questionnaires:** Written responses to specific control questions within a defined response window.
- **Inspection rights:** On-site or technical inspections triggered by material compliance concerns.


Define a breach notification window measured in hours for any ZDR violation. Tie financial penalties to specific retention failures per surface. Post-termination audit rights for a defined period let you verify deletion actually completed.


## Architectural patterns that pass enterprise procurement


Procurement-defensible ZDR comes from architecture. Architectural guarantees hold when processes fail. Policy-based guarantees require correct execution across every storage location and subprocessor.


#### MicroVM ephemerality over container persistence


Hardware-isolated microVMs run a separate kernel for each workload. Terminating the microVM process terminates the entire isolated memory space, with no shared page cache path to other tenants. The[Firecracker design](https://github.com/firecracker-microvm/firecracker/blob/main/docs/design.md) specifies one-process-per-VM as the architectural foundation.


Container-based sandboxes share the host kernel, which is the industry standard for running trusted software in multi-tenant settings. In untrusted or arbitrary code execution contexts, they rely on software-enforced isolation: namespaces, groups, and seccomp filters.[NIST SP 800-233](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-233.pdf) classifies containers as providing "some guarantee" of isolation. MicroVMs provide a "stronger degree" by comparison.


Memory and filesystem state can persist in the host kernel's page cache after container termination. No hardware-enforced memory scrubbing runs on exit. A March 2026[SandboxEscapeBench study](https://arxiv.org/abs/2603.02277) from Oxford and the UK AI Security Institute showed frontier LLMs can escape container sandboxes. The benchmark spans four vulnerability classes:


- **Misconfiguration:** Exposed sockets, writable host mounts, and privileged-mode containers.
- **Privilege allocation:** Capabilities granted beyond what the workload actually requires.
- **Kernel flaws:** Vulnerabilities in the host kernel that all containers share.
- **Orchestration weaknesses:** Gaps in runtime and orchestration-layer enforcement.


MicroVM isolation provides a robust hardware-enforced security boundary. The ZDR guarantee for Blaxel's sandboxes relies on[the execution environment's filesystem being entirely RAM-based](https://blaxel.ai/blog/how-to-slash-sandbox-memory-usage-by-75-using-overlayfs) . All files, artifacts, and state reside in volatile memory rather than durable disk storage. Deleting the microVM becomes the definitive moment of data destruction. All information is wiped without residual traces on physical disks.


#### Network-inactivity auto-shutdown over fixed timeouts


A short network-inactivity trigger limits the data residency window. Shorter active windows mean less state in memory and fewer log lines generated. The data footprint at any moment stays small.


[Blaxel sandboxes](https://docs.blaxel.ai/Sandboxes/Overview) transition to standby after 15 seconds of network inactivity. This keeps the window during which execution data exists in memory short by design. Customers operating under Blaxel's ZDR guarantee forego standby. The guarantee applies only when a sandbox was never placed in standby mode. Sandboxes are deleted rather than suspended. No snapshot is ever saved to disk. All data stored in memory is wiped on deletion. You trade <25ms resume from standby for 200–600ms initial creation latency.


### Snapshot-less ZDR mode as a configurable tenant property


The honest tradeoff for sandbox ZDR is accepting cold creation latency. Initial creation from a template takes longer than resuming from standby, and snapshots themselves are a retention surface. Firecracker guest memory files contain the full RAM contents of the microVM at snapshot time. Any data processed in memory at the moment of snapshot is serialized to disk.


Some workloads absorb this tradeoff without difficulty. Compliance-bound batch jobs that run, complete, and delete fit a ZDR lifecycle naturally. Real-time agent interactions that require instant resume need standby.


Those workloads must scope ZDR differently: configure time-to-live (TTL)[sandbox expiration](https://docs.blaxel.ai/Sandboxes/Expiration) paired with volumes for data that genuinely needs persistence. Make this tradeoff a configurable tenant property. Enterprise procurement teams respect vendors who surface the constraint honestly.


## Building a zero data retention posture that holds up under audit


ZDR posture in[AI sandbox infrastructure](https://blaxel.ai/blog/ai-sandbox) decides whether enterprise deals close or stall in legal review. The posture has to be contractual and architectural at once. Each of the four data surfaces needs three commitments. The DPA must name a retention window, document deletion mechanisms per subprocessor, and require architectural controls independent of process completion.


Blaxel's perpetual sandbox platform runs each sandbox in an individual Firecracker microVM. The sandbox filesystem runs in memory: deleting a sandbox immediately erases all data in the filesystem. SOC 2 Type II, ISO 27001, and HIPAA BAA coverage extends across the full platform. That includes Sandboxes and Batch Jobs. When the sandbox was never placed in standby, Blaxel guarantees native zero data retention. The tradeoff is real: ZDR mode forgoes perpetual standby. That's the architecturally honest answer to a problem policy alone can't solve.


[Start building free](https://app.blaxel.ai/) to test the ZDR lifecycle against your compliance requirements. Or[book a demo](https://blaxel.ai/contact) to walk through the ZDR architecture with the founding team.


Test zero data retention against your compliance requirements


RAM-based filesystem, microVM isolation, no snapshot persistence in ZDR mode. SOC 2 Type II, ISO 27001, and HIPAA BAA certified.


[Start free](https://app.blaxel.ai/)


## FAQ


### What does zero data retention mean in an AI sandbox?


In an AI sandbox, zero data retention means the vendor's commitment covers every surface where execution data can land. The API request boundary is only one of those surfaces. That includes filesystem artifacts, execution logs, memory snapshots, and observability telemetry. Each surface has its own retention behavior and deletion mechanism. Treating ZDR as a single-surface property leads to gaps that surface during procurement or audit.


### Why are memory snapshots a ZDR risk?


When a sandbox enters standby, snapshot storage serializes RAM contents to disk. Secrets, payloads, and intermediate execution data resident in memory at pause time persist in that snapshot file. The snapshot remains tied to the sandbox lifecycle, so deletion behavior on workspace teardown matters. Avoiding snapshot creation entirely is the only architectural way to eliminate the surface.


### Why don't vendor ZDR claims survive procurement review?


Claims often fail because they're partial, opt-in, or scoped to a single layer of the stack. Procurement teams typically find gaps in subprocessors, logging backends, telemetry pipelines, or standby-state handling. The marketing page rarely spells those out. The DPA language usually doesn't match the architectural reality the vendor describes in sales conversations.


### What should a DPA say about ZDR?


The DPA should name each retention surface explicitly: filesystem artifacts, execution logs, memory snapshots, and observability telemetry. For each surface, the document should specify retention behavior, deletion mechanism, deletion timeframe, and subprocessor involvement. Generic references to "customer data" are too vague to evaluate real retention risk and rarely hold up under legal review.


### What architecture is most compatible with sandbox ZDR?


ZDR relies on the execution environment's filesystem being RAM-based. All files, artifacts, and state reside in volatile memory. Deleting the microVM instantly erases everything because no data is written to durable disk storage. MicroVM isolation makes the execution environment itself the deletion boundary. Snapshot-less ZDR modes accept cold creation latency to eliminate the snapshot retention surface.


### How does Blaxel handle ZDR tradeoffs?


Blaxel offers a native zero data retention option, guaranteed for sandboxes that were never placed in standby. The tradeoff is direct: forgoing standby means no snapshot is created, so deletion satisfies a true zero-retention posture. Sandboxes still run inside individual microVMs with hardware-enforced isolation, so the security boundary holds throughout the workload's life. Workloads that need instant resume from standby can't operate under ZDR mode.
