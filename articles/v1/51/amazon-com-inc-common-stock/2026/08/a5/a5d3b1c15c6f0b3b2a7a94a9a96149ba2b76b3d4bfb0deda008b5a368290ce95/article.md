---
schema_version: "1.0.0"
document_id: "a5d3b1c15c6f0b3b2a7a94a9a96149ba2b76b3d4bfb0deda008b5a368290ce95"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/storage-gateway-fips-privatelink/"
published_at: "2026-08-19T10:00:00+00:00"
first_seen_at: "2026-08-20T00:12:43.329206+00:00"
fetched_at: "2026-08-20T00:12:46.759003+00:00"
content_hash: "sha256:5b99e62f969e6da73da4018bc90d6a8761fe77435a573a357035446af31df5a5"
---

# AWS Storage Gateway now supports FIPS-compliant private connectivity for Tape and Volume Gateway

AWS Storage Gateway now supports FIPS 140-3 validated endpoints over AWS PrivateLink for Tape Gateway and Volume Gateway. Previously, FIPS endpoints were available only over the public internet. Now you can keep FIPS-compliant traffic on the private AWS network, making it easier to use Storage Gateway for regulated workloads.


With this launch, your Tape Gateway and Volume Gateway can reach the Storage Gateway service endpoints privately through an interface VPC endpoint in your VPC, while using FIPS validated encryption. To get started, you can create a FIPS interface endpoint for Storage Gateway in your VPC, then choose the FIPS VPC endpoint option when activating your gateway. Once activated, your gateway connects to the Storage Gateway service over FIPS validated endpoint on the private AWS network. To activate a gateway with a FIPS PrivateLink endpoint, your gateway must be running software version 3.2.7 or later.


This launch is available in the eight AWS Regions where Storage Gateway offers FIPS endpoints: US East (N. Virginia), US East (Ohio), US West (N. California), US West (Oregon), Canada (Central), Canada West (Calgary), AWS GovCloud (US-East), and AWS GovCloud (US-West). To learn more, visit the[AWS Storage Gateway User Guide](https://docs.aws.amazon.com/storagegateway/) or the[product page](https://aws.amazon.com/storagegateway/) .
