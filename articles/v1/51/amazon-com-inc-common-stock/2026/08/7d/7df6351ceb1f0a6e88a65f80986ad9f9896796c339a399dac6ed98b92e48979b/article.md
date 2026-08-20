---
schema_version: "1.0.0"
document_id: "7df6351ceb1f0a6e88a65f80986ad9f9896796c339a399dac6ed98b92e48979b"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/nested-virtualization-workspaces/"
published_at: "2026-08-18T08:00:00+00:00"
first_seen_at: "2026-08-19T00:16:28.993170+00:00"
fetched_at: "2026-08-19T00:16:30.402166+00:00"
content_hash: "sha256:e18efdf131ce2280d0e58af43c64d4971b2e0a8cb04dcfbff7f366ce8529ebf5"
---

# Amazon WorkSpaces now supports Nested Virtualization

Amazon WorkSpaces now supports nested virtualization for Amazon WorkSpaces Personal and Amazon WorkSpaces Core Managed Bundles. With nested virtualization enabled, developers can run Docker Desktop, Windows Subsystem for Linux (WSL2), and other hypervisor-dependent tools directly on their Windows WorkSpaces, and KVM-based workloads, Android emulators, and nested containers on their Linux WorkSpaces - without needing separate physical hardware or workaround environments.


Nested virtualization is supported on license-included (public and custom) bundles, Bring Your Own License (BYOL) bundles, and Bring Your Own Protocol (BYOP) bundles running Windows Server 2019, Windows Server 2022, Windows Server 2025, Windows 11, Ubuntu 22.04, Ubuntu 24.04, Red Hat Enterprise Linux 8, Red Hat Enterprise Linux 9, Rocky Linux 8, or Rocky Linux 9 with DCV protocol. Power (4 vCPU) or higher is recommended. Nested virtualization is not supported on GPU bundles, the PCoIP protocol, or Amazon WorkSpaces based on Windows Server 2016 and Windows 10. Administrators can enable or disable nested virtualization at the individual workspace level through the console, API, or CLI.


You can get started by enabling nested virtualization on your WorkSpaces through the console or CLI. For more information, visit the Amazon WorkSpaces documentation on[Nested Virtualization](https://docs.aws.amazon.com/workspaces/latest/adminguide/nested-virtualization.html) . Nested virtualization is available in all AWS Regions where Amazon WorkSpaces is available, excluding China (Ningxia) Region and Israel (Tel Aviv) Region. For pricing information, visit Amazon WorkSpaces[pricing page](https://aws.amazon.com/workspaces/pricing/) .
