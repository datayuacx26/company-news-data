---
schema_version: "1.0.0"
document_id: "6f7766478abeb5bb2826a54ef52a22aeee78bcf7861ff31d32fde10789ee28e5"
company_key: "yc-ryvn"
company: "Ryvn"
source_id: "yc-ryvn-news-import-a6f556f77bcf"
canonical_url: "https://ryvn.ai/blog/how-clearly-ai-accelerated-byoc-deployments"
published_at: "2026-02-17T00:00:00+00:00"
first_seen_at: "2026-07-23T23:41:20.719716+00:00"
fetched_at: "2026-07-28T22:19:59.143850+00:00"
content_hash: "sha256:d29651dbc2ee9a68941b32a34aa29b5e3fac2705bdac2c3e3b34e90e8419a542"
---

# How Clearly AI accelerated BYOC deployments from 2 months to 2 days

[Clearly AI](https://www.clearly-ai.com/) is an AI-native platform for security and privacy assessments. They landed a contract with a global automotive manufacturer, but it had a catch: strict enterprise data sovereignty policies meant they had to deploy their entire stack inside the customer's AWS environment. Two months into the deployment, their stack still wasn't up. They called on Ryvn, and we helped them get it done in two days.


## Challenge


Clearly AI already had a CloudFormation stack to provision single-tenant environments in their own AWS account, so they tried that first. But using this approach required privileged access to the customer's AWS account in order to run the provisioning process. So to get access, they kicked off months of IT red tape: corporate laptops, VPN access, and sign-off on privileged AWS roles. Two months in, their stack still wasn't deployed, and end users were getting frustrated.


Alternatively, they could let the customer's IT team handle the deployment end-to-end, but that came with its own set of problems. Clearly AI would lose control over when they could push updates, and any misconfiguration by the customer's IT team would lead to long troubleshooting calls, ultimately leaving end users with delayed updates and a poor experience.


Neither path worked. Clearly AI needed to keep operational control without wading through months of IT bureaucracy.


## Solution


After a quick onboarding call, Clearly AI was set up to deploy. With Ryvn, they no longer needed credentials to their customer's AWS account and instead their customers could follow just 2 simple steps:


1.


The customer runs a CloudFormation template, provided by Ryvn, in their AWS account that deploys a small EC2 instance installed with the Ryvn Provisioner.


1.


The Ryvn Provisioner automatically spins up an EKS cluster inside the customer's VPC and installs the Ryvn Agent onto the cluster.


2.


The Ryvn Agent provisions AWS resources such as S3, DynamoDB, and Bedrock, which Clearly AI's application needs. The Agent also deploys Clearly AI's application containers onto the EKS cluster.


2.


The customer's IT team updates their private DNS to route internal traffic to Clearly AI's application.


End-to-end—from Ryvn onboarding to end users accessing the BYOC deployment—took two days. No AWS credentials needed. No direct access to the customer's AWS account. No drawn-out IT involvement.


> “
>
>
> Ryvn transformed BYOC from our biggest challenge into a competitive advantage. Now we can deploy securely and easily in any customer environment, without IT bureaucracy. ”
>
>
> Joe Choi-Greene,
>
>
> Co-founder & CTO


## Results


Clearly AI now manages updates, monitoring, and configuration changes all from Ryvn's control plane. No VPNs, corporate laptops, or privileged credentials.


Ryvn's zero-trust BYOC architecture ensures the customer's data never leaves their VPC. Every action the Ryvn Agent takes is logged, with a full audit trail. When issues come up, Clearly AI can immediately push a fix or roll back changes.


The next time Clearly AI needs to do a BYOC deployment, it'll only take minutes of effort. What used to be the bottleneck in delivering on enterprise deals is now a repeatable process they can run for future customers in manufacturing, healthcare, finance, government, and other regulated industries.
