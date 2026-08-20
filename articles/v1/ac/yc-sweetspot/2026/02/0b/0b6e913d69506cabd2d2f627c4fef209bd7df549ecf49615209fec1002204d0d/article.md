---
schema_version: "1.0.0"
document_id: "0b6e913d69506cabd2d2f627c4fef209bd7df549ecf49615209fec1002204d0d"
company_key: "yc-sweetspot"
company: "Sweetspot"
source_id: "yc-sweetspot-news-import-95ae5efddaf0"
canonical_url: "https://www.sweetspot.so/articles/fips-140-architecture/"
published_at: "2026-02-20T00:00:00+00:00"
first_seen_at: "2026-07-22T15:23:10.673345+00:00"
fetched_at: "2026-07-28T22:19:31.715486+00:00"
content_hash: "sha256:bec6548c3def75257d06d223596236c11c3da39b7069ea2274b33b4a33f5e318"
---

# FIPS 140 Compliance Is an Architecture Decision

Most vendors enable FIPS on one layer and stop. Usually the load balancer. Sometimes the database. Rarely anything else.


They check the box, write “FIPS-compliant” on the marketing page, and move on. FIPS 140-2 (and its successor, FIPS 140-3) is not a single control you toggle. It’s a property of every cryptographic operation in your system. If any layer (the OS, the database driver, the container registry connection, the service mesh) uses non-validated cryptography, you have a gap. The data flowing through that layer is not protected by FIPS-validated modules, regardless of what the layers above and below it do.


For platforms handling CUI, that gap matters. NIST SP 800-171 control SC.L2-3.13.11 requires FIPS-validated cryptography. Not “FIPS-capable.” Not “FIPS-compatible.” Validated.


Enforcing FIPS across a modern cloud-native application stack means addressing six layers.


Layer What Needs FIPS Common Gap


**Node OS** Kernel crypto modules, system libraries FIPS-capable but not FIPS-enforced at boot


**Service mesh** TLS between services (pod-to-pod) Application TLS using non-validated libraries


**Load balancers** TLS termination, cipher suite selection ”TLS 1.2 minimum” without FIPS cipher restriction


**Container registry** Image pull connections Registry endpoint not using FIPS TLS


**Application deps** Database drivers, HTTP clients, SDKs Bundled OpenSSL bypassing system FIPS provider


**Storage** Volume encryption, key management KMS region not FIPS 140-2 Level 3 validated


## Layer 1: The Node Operating System


Everything starts at the OS. If the kernel isn’t running in FIPS mode, any cryptographic operation performed by system libraries (OpenSSL, GnuTLS, NSS) may use non-validated code paths.


“FIPS-capable” and “FIPS-enforced” are not the same thing. Most Linux distributions ship with FIPS-capable cryptographic modules, but they don’t activate FIPS mode by default. Activating it requires explicit configuration at boot time, and the system needs to self-test its cryptographic modules before making them available.


For Kubernetes workloads, this means your node images must boot with FIPS enforced, not just have the libraries installed. The node’s container runtime, kubelet, and any SDK or CLI tools running on the node should also be configured to use FIPS endpoints for cloud API calls.


Immutable, purpose-built node operating systems (with no shell, no package manager, and no mechanism for runtime modification) are ideal. You build the image with FIPS enforced, verify it in your pipeline, and any node that launches from that image inherits the correct posture automatically. No drift. No manual configuration.


Rebuilding these images on a regular cadence (weekly, at minimum) ensures you’re pulling in the latest security patches while continuously verifying that FIPS mode is active.


## Layer 2: Service-to-Service Communication


In a microservices architecture, most network traffic is internal, service to service, pod to pod. If this internal traffic uses TLS (and it should), the TLS implementation needs to use FIPS-validated cipher suites.


FIPS proxy sidecars are a common pattern. A small, purpose-built proxy container runs alongside each service, terminating and originating TLS connections using a FIPS-validated OpenSSL provider. The application container communicates with its sidecar over localhost (unencrypted, within the same network namespace), and the sidecar handles all external TLS with validated cryptography.


This sidecar pattern has two advantages. First, it decouples FIPS compliance from the application’s language and runtime. You don’t need every Python, Go, and Node.js service to independently configure FIPS-validated TLS. Second, it creates a single, auditable configuration point for cryptographic policy.


The sidecar’s base image matters. Using a minimal, well-maintained base with FIPS cryptographic policies baked in (such as RHEL UBI with` update-crypto-policies --set FIPS` ) ensures the proxy only negotiates FIPS-approved cipher suites.


## Layer 3: Load Balancers and Ingress


This is the layer most people get right, because it’s the most visible. Every external-facing load balancer must be configured with a TLS policy that restricts negotiation to FIPS-validated cipher suites.


For AWS, this means selecting a FIPS-specific security policy on your Application Load Balancers, something like the TLS 1.3 + 1.2 FIPS policies that restrict cipher suites to those covered by FIPS validation. A “TLS 1.2 minimum” policy still allows non-FIPS cipher suites to negotiate.


This should be applied consistently across every ingress point: your API, your authentication endpoints, any internal services exposed via load balancer (databases, caches, message brokers).


## Layer 4: The Container Registry


When your orchestration platform pulls a container image, that pull is a TLS connection to your container registry. If that connection doesn’t use FIPS-validated TLS, you have a gap. Most people don’t think about this layer.


Most cloud providers offer FIPS-specific registry endpoints. Using them ensures that even the image pull, which carries your application code and dependencies, happens over FIPS-validated TLS. It’s a one-line configuration change in your registry settings, but it’s easy to miss.


## Layer 5: Application Dependencies


Many popular libraries and frameworks bundle their own cryptographic dependencies rather than linking against the system’s libraries. When they do this, they bypass the OS-level FIPS enforcement entirely.


Database drivers are a common example. Many database client libraries ship pre-built binary packages that include their own copy of OpenSSL or libpq. These bundled libraries are not FIPS-validated and don’t respect the system’s FIPS configuration. Your OS can be in full FIPS mode, your TLS proxy can be using validated cipher suites, and your database connections can still be using non-validated cryptography because the driver brought its own.


The fix is to use driver variants that compile against the system’s cryptographic libraries rather than bundling their own. This is a detail buried in package documentation. You need to know it’s a concern to even look for it.


This generalizes beyond database drivers. Any dependency that bundles its own TLS or cryptographic implementation (HTTP clients, message queue libraries, cloud SDK components) needs to be audited. If it’s not linking against the system’s FIPS-validated provider, it’s a gap.


## Layer 6: Storage Encryption


Every persistent storage volume (OS partitions, data partitions, ephemeral storage on every node class) must be encrypted at rest. Cloud providers make this straightforward with managed encryption keys, and most offer automatic key rotation.


The less obvious piece: ensuring that the encryption uses FIPS-validated key management. Cloud KMS services are generally FIPS 140-2 Level 3 validated for the key management hardware, but you need to verify this for your specific service and region.


Database encryption at rest is table stakes. Object storage must enforce HTTPS-only access policies, rejecting any unencrypted access attempts at the bucket policy level.


## Keeping All Six Layers Right


Each of these layers, individually, is a reasonable engineering task. The challenge is getting all of them right simultaneously and keeping them right as your stack evolves. A new dependency gets added that bundles its own OpenSSL. A node image gets rebuilt without verifying FIPS mode. A new service gets deployed without a FIPS proxy sidecar. A load balancer gets configured with a non-FIPS TLS policy.


FIPS compliance is a property of your architecture that you maintain continuously, through automation, through pipeline verification, and through a deep understanding of where cryptographic operations happen in your stack.


---


At Sweetspot, FIPS-validated cryptography is enforced at every layer, from the node OS through service communication, load balancers, container registry connections, application dependencies, and storage. We built automated pipelines that verify FIPS enforcement on every image build, and we audit our dependency chain for bundled cryptography that would bypass system-level FIPS controls.


Your team shouldn’t have to become experts in OpenSSL FIPS providers and crypto policy configuration to use AI on government contracts. We already did that work, so you can focus on proposals, not pipeline configs.
