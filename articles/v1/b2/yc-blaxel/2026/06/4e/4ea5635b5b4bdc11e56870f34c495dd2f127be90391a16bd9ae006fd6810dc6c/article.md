---
schema_version: "1.0.0"
document_id: "4ea5635b5b4bdc11e56870f34c495dd2f127be90391a16bd9ae006fd6810dc6c"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/sandbox-provider-security-assessment-checklist"
published_at: "2026-06-18T15:50:50+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:164efe18ea445b3ceea15b03e844f938d9709c2d67a29528d005ac65d3d37d49"
---

# Sandbox provider security assessment: a checklist for enterprise buyers

Your team runs a sandbox provider through the standard vendor security questionnaire. It passes. Three months into production, a container escape vulnerability exposes customer data across tenants. The questionnaire never asked about the isolation model.


Standard vendor risk frameworks cover encryption and certifications. They miss sandbox-specific questions about isolation boundaries and how session data or agent network access behave after agent code runs. AI agents execute arbitrary, untrusted code at runtime. That threat model differs from any SaaS tool your procurement team has assessed before.


Production-grade sandbox reviews should start with the isolation boundary. Then verify how the provider handles compliance scope, data, network access, secrets, and reliability.


## **Why AI sandbox assessments need a different framework**


Standard SaaS vendor questionnaires assume the vendor controls what code runs.[Sandbox](https://blaxel.ai/blog/ai-sandbox) providers supply isolated environments where your agents generate and execute code at runtime. That inverts the trust model every procurement framework was built around.


That inversion creates gaps standard assessments don't cover. Standard questionnaires rarely ask about customer-generated code running against shared infrastructure. Isolation architecture may be absent from the review entirely. The same questionnaire may treat data as persistent SaaS storage. Sandboxes produce filesystem state and memory snapshots between sessions. When customer-generated code can reach external services on its own, network controls need a separate review.


A mismatched assessment creates false confidence. The provider checks every box on a questionnaire designed for SaaS tools but leaves sandbox-specific attack surfaces unexamined.


## **Isolation architecture**


A provider's isolation model determines whether a compromised agent can reach the host kernel, neighboring tenants, or shared resources. Buyers need to evaluate the boundary the provider enforces and test its resistance to escape.


### **Verify the isolation boundary**


Sandbox infrastructure often starts with containers. These use namespaces and control groups with Linux capabilities enforced by a shared host kernel. gVisor intercepts application system calls in user space. That minimizes direct host interaction. MicroVMs use hardware virtualization through the Linux Kernel-based Virtual Machine. Each workload gets a dedicated kernel behind a hardware-enforced boundary.


Containers are the industry standard for running trusted, first-party code. For untrusted, AI-generated code, the shared kernel becomes the problem. An unpatched vulnerability in the host kernel may be reachable by a container tenant on the same host. Reachability depends on the flaw and the tenant's level of access.


MicroVMs use hardware virtualization to enforce a strong boundary through hypervisor or KVM enforcement. Kernel-policy-based isolation alone is weaker.


Ask which virtualization technology isolates each sandbox and whether the boundary is hardware-enforced or process-level. Confirm whether one tenant's workload can reach another tenant's kernel.


The right answer depends on your threat model. If agents execute code you've reviewed and trust, containers work. If agents execute arbitrary code generated at runtime, the isolation boundary must sit below the kernel.


### **Test for sandbox escape resistance**


Ask for penetration test reports that specifically target the sandbox boundary. General infrastructure pen tests may miss the compute layer where agent code runs. That layer needs explicit inclusion in scope.


The shared-kernel surface has a documented, recurring escape history. The "Leaky Vessels" flaw,[CVE-2024-21626](https://nvd.nist.gov/vuln/detail/CVE-2024-21626) , affected runc 1.1.11 and earlier. A leaked file descriptor let an attacker break out to the host filesystem. Kernel-level flaws like[CVE-2022-0492](https://nvd.nist.gov/vuln/detail/CVE-2022-0492) confirm this pattern.


That cgroups privilege escalation sits on CISA's Known Exploited Vulnerabilities list. Both the runtime and the shared kernel are exploitable attack surfaces. Firecracker-based microVM platforms treat each vCPU thread as malicious from startup. Blaxel uses this microVM-based isolation so that an exploit inside one sandbox stays contained. It cannot reach the host system or other tenants.


Ask whether the provider has conducted a recent sandbox-specific penetration test. Ask what CVEs affecting the isolation layer have been disclosed and patched. Ask whether the provider publishes a responsible disclosure program. Escape resistance is the difference between an isolated incident and a[multi-tenant breach](https://blaxel.ai/blog/container-escape) .


## **Compliance certifications and data residency**


Certifications confirm a provider met a security standard at a point in time. They don't reveal whether that standard covers sandbox-specific risks. The audit scope may not include the compute layer your agents run on. Buyers need to audit certification depth and map data residency controls to their regulatory requirements.


### **Audit certification scope and recency**


Enterprise buyers commonly expect SOC 2 Type II and ISO/IEC 27001. HIPAA compliance with a Business Associate Agreement applies when the provider handles protected health information. Check whether each certification's scope includes the sandbox compute layer or only the control plane.


Scope is narrower than marketing implies. A SOC 2 report's boundary is defined in the vendor's system description. The service organization typically determines this scope with auditor input.


That system description[does not mandate](https://www.aicpa-cima.com/resources/download/illustrative-soc-2-r-report-with-description-and-assertion) a specific format and covers only what the vendor chooses to describe. ISO 27001 works the same way. When an organization is part of a larger entity, the certification[scope covers only](https://committee.iso.org/files/live/sites/jtc1sc27/files/resources/ISO-IECJTC1-SC27-WG1_N3298_Auditing%20Practices%20Note%20-%20SoA.pdf) the defined Information Security Management System. A certification scoped to a vendor's orchestration layer does not automatically extend to the compute workload where AI-generated code executes.


Request the full SOC 2 Type II report, not the summary. Read the system description section to confirm scope includes sandbox execution environments. For ISO 27001, request the Statement of Applicability. Verify which controls are implemented versus excluded. For HIPAA, verify the BAA covers data processed inside sandboxes along with data at rest in the provider's storage. Certification breadth matters more than certification count.


### **Map data residency to your regulatory requirements**


Map where sandbox compute runs and where state is stored to your regulatory requirements. For teams subject to the General Data Protection Regulation (GDPR) or data sovereignty laws, this matters. It determines whether a provider is even eligible.


Under GDPR, processing location matters because storage is a subset of processing. Remote access from a third country[may constitute a transfer](https://www.edpb.europa.eu/system/files/2023-02/edpb_guidelines_05-2021_interplay_between_the_application_of_art3-chapter_v_of_the_gdpr_v2_en_0.pdf) that triggers Chapter V obligations. One exception: an EU controller's employee who travels abroad and accesses the controller's data from a third country.


Verify whether the provider stores data and executes code in the same approved region. That gap matters legally. Cross-border transfer rules turn on whether personal data is transmitted or made available to a third-country recipient.


Ask how many regions the provider operates in. Ask whether you can restrict workloads to specific regions. Ask whether region enforcement is configurable per workspace or only at the account level. Distinguish clearly between data residency (where data is stored) and compute residency (where code executes). Request written confirmation of region enforcement scope before moving to procurement.


## **Data lifecycle and retention policies**


Sandbox data lifecycle differs from typical SaaS retention. Sandboxes produce ephemeral compute artifacts: filesystem state, memory snapshots, and process outputs. Standard data classification frameworks don't cover these. The questions that matter are what happens to data between sessions and how long artifacts persist. Buyers also need to know whether deletion is guaranteed or best-effort.


### **Assess zero data retention capabilities**


For zero data retention (ZDR), no customer data should survive sandbox deletion. The implementation architecture determines whether that guarantee holds. The difference comes down to where the filesystem lives.


RAM-based filesystems store all data in volatile memory. Deleting a microVM erases the volatile memory but does not guarantee secure erasure from physical storage. Tmpfs files are destroyed the moment the volatile store is gone. Disk-based filesystems are harder.


Standard deletion only removes filesystem pointers. The physical data remains. On solid-state drives, over-provisioned storage is generally inaccessible to host overwrite commands, according to[flash-erasure research](https://static.usenix.org/event/fast11/tech/full_papers/Wei.pdf) . That leaves recoverable remnants. This introduces erasure timing and verification questions a buyer must resolve.


Ask whether the sandbox filesystem is RAM-based or disk-based. Ask whether any artifacts can be recovered from underlying storage after deletion. Ask whether ZDR is optional or enforced by default. Native ZDR architectures backed by RAM-based filesystems erase all state when the microVM is deleted. For teams handling sensitive customer data through agent workflows, ZDR should be a non-negotiable requirement.


### **Validate state persistence and cleanup mechanisms**


Teams that need sandboxes to persist between sessions face a different set of questions. How long does state survive standby, and is cleanup time-based or event-driven? What data survives cleanup versus what is erased?


Ask whether idle sandboxes remain in standby, move to archive, or get reclaimed after a fixed window. Ask for the exact standby duration and whether it's configurable to your workload.


Distinguish between standby (sandbox paused, state preserved) and archived (state stored but requiring restoration time). Archived state introduces cold start latency that affects agent responsiveness. Document the maximum standby duration and restoration latency in your assessment scorecard. These numbers directly affect your agents' production service-level agreements.


## **Network security and credential management**


Agents inside sandboxes make outbound network requests to external APIs and databases. The provider's network controls determine whether those requests are auditable and restrictable. Credential delivery determines whether secrets reach sandboxes without unnecessary exposure to agent code.


### **Evaluate egress controls and IP management**


Egress controls restrict which external services a sandbox can reach. Without them, a compromised agent can exfiltrate data to any endpoint. Permissive defaults let workloads initiate unrestricted outbound connections.


This matters even with strong isolation. Firecracker's design documentation confirms it[does not filter](https://github.com/firecracker-microvm/firecracker/blob/main/docs/design.md) network traffic. All guest egress is untrusted and must be filtered at the host level. Host-level egress controls are architecturally required. A default-deny posture followed by explicit allowlisting is the established pattern for restricting outbound traffic.


Ask whether the provider supports network egress allowlists. Ask whether you can assign static outbound IPs for firewall whitelisting. Ask whether outbound traffic is logged and auditable. Static IPs matter for enterprises that require firewall rules for outbound sandbox traffic. Ask whether the provider offers a managed egress gateway with static outbound IPs.


Those gateways remove the need to route through your own proxy infrastructure. Factor the operational cost of self-managed networking into your total cost of ownership. Network controls are the second line of defense after isolation. If the sandbox boundary fails, egress restrictions limit the blast radius.


### **Review secrets injection and credential handling**


Agent workflows require service credentials: API keys, database credentials, and service tokens. How those credentials reach the sandbox determines whether a compromised agent can steal them.


Credential delivery usually falls along a weaker-to-stronger control path:


- **Environment variable injection:** Credentials sit in memory accessible to all processes. Environment variables are accessible to every process in the sandbox and may leak into logs or system dumps. Security guidance broadly recommends against this pattern.
- **Sidecar or shared in-memory volume:** A sidecar process fetches credentials and writes them to a shared in-memory volume. The agent reads credentials from the volume without direct access to the secrets store.
- **Runtime API retrieval with short-lived, runtime-generated secrets:** Credentials are fetched at runtime through a secure API. Short-lived, runtime-generated secrets expire automatically. That expiration reduces credential reuse and limits the blast radius of a compromised token.


Sidecar injection and runtime API retrieval both limit credential exposure compared to environment variables. Short-lived, runtime-generated secrets add a time-bound constraint that further reduces risk. Environment variables are the weakest option because any process in the sandbox can read them.


Ask how secrets are delivered to sandboxes. Ask whether credentials can be scoped to specific sandboxes or agents. Verify that credential access events are logged. Credential theft is a top-tier risk in agent security. The delivery mechanism matters as much as the credential rotation policy.


## **Vendor reliability and incident transparency**


Logging depth, incident transparency, support access, and vendor stability determine whether security controls hold over time.


Audit logging depth comes first. Confirm the provider logs sandbox creation, deletion, network requests, and credential access. Verify you can export those logs to your own SIEM. Retention matters for investigation. CISA's guidance says log retention should match your risk and incident-response needs. Logs should be[retained for investigations](https://www.cisa.gov/resources-tools/resources/secure-demand-guide) . Use that as an evaluation criterion. On incident response, ask for a record of recent security incidents, root cause analyses, and remediation timelines. Providers that refuse to share incident history are a red flag.


Support access and vendor stability close out the assessment. Enterprise sandbox deployments need direct access to the provider's engineering team during incidents. Ticket-based support alone is insufficient when a sandbox escape affects production. Factor in vendor stability: funding stage, customer retention, and core product focus.


A sandbox feature bolted onto a larger platform carries different risk than a dedicated provider. Strong isolation without incident transparency creates risk you can't measure. Operational commitment matters as much as the technical controls.


## **How to build your sandbox provider security assessment**


An inadequate sandbox assessment can lead to customer data exposure or a production shutdown. That outcome goes far beyond a compliance finding. Every assessment area in this checklist targets a risk that standard questionnaires miss. These span isolation boundaries, data lifecycle, networking, and credential delivery.


For teams evaluating sandbox infrastructure for untrusted agent code,[Blaxel's perpetual sandbox platform](https://blaxel.ai/sandbox) uses Firecracker microVM isolation. The platform holds SOC 2 Type II, ISO 27001, and HIPAA compliance (with BAA), plus proxy secrets injection. For storage,[Agent Drive](https://docs.blaxel.ai/Agent-drive/Overview) lets agents share data and session context across sandboxes. Volumes provide block storage for raw performance and long-term persistence.


For networking,[dedicated egress gateways](https://docs.blaxel.ai/Infrastructure/Dedicated-egress-gateways) in private preview cover routing and egress requirements. Agents that run AI-generated code benefit from hardware-enforced isolation paired with sub-25ms resume from standby. That resume speed keeps interactive agent workflows responsive when sessions move in and out of standby.


Talk to the team at[blaxel.ai/contact](https://blaxel.ai/contact) or start building at[app.blaxel.ai](https://app.blaxel.ai/) .


Evaluate Blaxel against your sandbox security checklist


Firecracker microVM isolation, SOC 2 Type II, ISO 27001, HIPAA BAA, proxy secrets injection, and dedicated egress gateways — built for teams assessing sandbox providers for untrusted agent code.


[Start free](https://app.blaxel.ai/)


## **Frequently asked questions**


**What is a sandbox provider security assessment?**


A sandbox provider security assessment evaluates isolation architecture, data lifecycle, network egress, and credential handling at the vendor level. Unlike standard vendor risk questionnaires, it targets risks specific to sandboxed compute. These include isolation boundary strength, escape resistance, state retention policies, and the unique threat model of untrusted code execution.


**Why do AI sandbox providers need different security evaluations than SaaS vendors?**


AI agents generate and execute arbitrary code at runtime. Standard SaaS tools run vendor-controlled code, and that difference inverts the trust model procurement frameworks were built around. A sandbox assessment must verify tenant-to-tenant isolation at the kernel level. It must confirm that data lifecycle, network egress, and credential controls match the risks created by agent-generated code.


**What isolation model is most secure for AI agent sandboxes?**


MicroVMs provide stronger isolation for untrusted code than containers or user-space sandboxing. They enforce a hardware-level boundary with a dedicated kernel per sandbox. Containers share the host kernel and provide weaker isolation. Running untrusted code in containers requires additional hardening or stronger isolation technologies. gVisor sits between the two, intercepting system calls in user space.


**Which compliance certifications should a sandbox provider have?**


SOC 2 Type II is often treated as a baseline requirement in enterprise procurement. ISO 27001 is a common additional assurance. HIPAA with a Business Associate Agreement is required when handling protected health information. Verify that the certification scope covers both the sandbox compute layer and the management console. A SOC 2 report that excludes the execution environment leaves the highest-risk surface unaudited.
