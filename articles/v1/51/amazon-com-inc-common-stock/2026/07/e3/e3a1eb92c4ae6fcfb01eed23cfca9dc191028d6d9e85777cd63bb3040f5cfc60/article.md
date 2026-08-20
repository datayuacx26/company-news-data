---
schema_version: "1.0.0"
document_id: "e3a1eb92c4ae6fcfb01eed23cfca9dc191028d6d9e85777cd63bb3040f5cfc60"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/ec2-public-images-ssm-parameters"
published_at: "2026-07-16T18:20:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:19a6eb12fbc92f99995b2205dae46df84c4a81248c8550b162bfcf1ec6f0bd5b"
---

# Amazon EC2 now surfaces the public SSM parameters associated with public AMIs

Amazon EC2 now surfaces the AWS Systems Manager (SSM) Parameter Store parameters associated with public AMIs directly in the AMI metadata. When you describe a public AMI, the response includes the associated public SSM parameter, making it easy to discover and reference in your configurations.


Previously, finding the SSM parameter associated with a public AMI required searching through SSM parameter namespaces manually. Now, when you describe a public AMI, the response includes the public SSM parameter it is associated with. This allows you to discover the SSM parameter for a public AMI easily and use it as an alias that always resolves to the latest version, simplifying AMI updates across your infrastructure.


This capability is available to all customers at no additional cost in all AWS regions including AWS China (Beijing) Region, operated by Sinnet, and AWS China (Ningxia) Region, operated by NWCD, and AWS GovCloud (US) Regions. To learn more, please visit the[documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami-parameter-store.html) .
