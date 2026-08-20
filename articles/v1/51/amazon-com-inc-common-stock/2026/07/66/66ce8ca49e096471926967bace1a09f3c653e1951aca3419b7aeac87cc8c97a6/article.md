---
schema_version: "1.0.0"
document_id: "66ce8ca49e096471926967bace1a09f3c653e1951aca3419b7aeac87cc8c97a6"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-workspaces-personal-pcoip/"
published_at: "2026-07-14T17:00:00+00:00"
first_seen_at: "2026-07-25T04:04:10.463980+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:3670df4dd93aa118f4d46bb3a5235549c244247b966f4730ab4ff2ea17f6f047"
---

# Amazon WorkSpaces Personal simplifies bulk PCoIP to DCV protocol migration

Amazon WorkSpaces Personal now provides automated rollback and support for PCoIP to DCV protocol migration of stopped WorkSpaces, building on the recently launched console-based migration workflow and checkpoint snapshot support. These new capabilities enable administrators to migrate WorkSpaces at scale with minimal manual intervention.


Amazon DCV is a high-performance streaming protocol built by AWS that powers Amazon WorkSpaces services. By migrating to DCV, customers gain access to broader operating system support including Windows 11 and Windows Server 2025, enhanced security features such as certificate-based authentication and WebAuthN redirection, and improved streaming performance. With this launch, if a protocol modification fails, the WorkSpace automatically rolls back to the pre-migration snapshot, ensuring it returns to a known healthy state without manual intervention. Additionally, administrators can now initiate migration for WorkSpaces in a stopped state, removing the need to manually start each stopped WorkSpace before modifying its protocol. This helps customers significantly speed up large scale migrations.


These enhancements are available in all AWS commercial and AWS GovCloud (US)[Regions](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) where Amazon WorkSpaces Personal is supported.


To get started, sign in to the[Amazon WorkSpaces console](https://us-east-1.console.aws.amazon.com/workspaces/v2/home) . For more information, see[Modify protocols section](https://docs.aws.amazon.com/workspaces/latest/adminguide/modify-workspaces.html#modify_protocols) in the[Amazon WorkSpaces Administration Guide](https://docs.aws.amazon.com/workspaces/latest/adminguide/amazon-workspaces.html) . To learn more about Amazon WorkSpaces, visit the[Amazon WorkSpaces product page](https://aws.amazon.com/workspaces/) .
