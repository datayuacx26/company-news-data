---
schema_version: "1.0.0"
document_id: "17cd2fbe29bfe29e37b0e03af1bfe8c4b7c3240c6198eacf17637d80dea1aba0"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-ec2-application-status-checks"
published_at: "2026-08-10T17:47:00+00:00"
first_seen_at: "2026-08-10T21:25:41.621452+00:00"
fetched_at: "2026-08-10T21:25:43.316318+00:00"
content_hash: "sha256:f7228b24a5ecd3639e1851314b66ce51b2ed11a6dcad0c611ea5ad5e0ac43176"
---

# Amazon EC2 introduces application status checks

Amazon EC2 introduces application status checks, a new status check that helps customers detect and respond to application-level issues on their EC2 instances. With application status checks, EC2 monitors applications to detect issues such as a web server that has stopped accepting requests, a Docker daemon that is not running, an incorrect networking configuration, or a network interface that is no longer passing traffic.


Customers rely on EC2 status checks today to receive alerts when an instance or the underlying system is unreachable. However, to monitor application issues, customers had to build and maintain their own monitoring solution. Now, with application status checks customers can monitor the status of their applications running on EC2 instances alongside existing EC2 instance and system status checks. Customers create a check by specifying the protocol, port, and path to monitor, along with the response codes that indicate a healthy application. After customers associate the check with their instances by instance ID or tag, Amazon EC2 sends HTTP or HTTPS requests to that port and path and reports on the application’s status every 60 seconds. Auto Scaling groups act on application status, initiating recovery by replacing instances when their applications report unhealthy.


Application status checks are available in all commercial AWS Regions and AWS GovCloud (US) Regions.


To get started with application status checks and review pricing, see the[Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/application-status-checks.html) .
