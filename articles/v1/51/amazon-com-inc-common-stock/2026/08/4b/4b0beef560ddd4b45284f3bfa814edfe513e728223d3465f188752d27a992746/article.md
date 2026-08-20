---
schema_version: "1.0.0"
document_id: "4b0beef560ddd4b45284f3bfa814edfe513e728223d3465f188752d27a992746"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-config-new-resource-types"
published_at: "2026-08-03T15:00:00+00:00"
first_seen_at: "2026-08-03T21:24:28.279899+00:00"
fetched_at: "2026-08-03T21:33:45.218976+00:00"
content_hash: "sha256:83fb588f441970eb2d7926d2902d33ef5ebee57efd9ed5f7afa2405f7206bb89"
---

# AWS Config now supports 15 new resource types

# AWS Config now supports 15 new resource types


Posted on: Aug 3, 2026


AWS Config now supports 15 additional AWS resource types across key services including Amazon Bedrock, Amazon OpenSearch Serverless, and Amazon SageMaker. This expansion provides greater coverage over your AWS environment, enabling you to more effectively discover, assess, audit, and remediate an even broader range of resources.


With this launch, if you have enabled recording for all resource types, then AWS Config will automatically track these new additions. The newly supported resource types are also available in Config rules and Config aggregators.


You can now use AWS Config to monitor the following newly supported resource types in all[AWS Regions](https://docs.aws.amazon.com/config/latest/developerguide/what-is-resource-config-coverage.html) where the resources are available:


Resource Types:


AWS::AppSync::DomainName AWS::OpenSearchServerless::AccessPolicy


AWS::Bedrock::AutomatedReasoningPolicy AWS::OpenSearchServerless::LifecyclePolicy


AWS::Bedrock::AutomatedReasoningPolicyVersion AWS::SageMaker::ImageVersion


AWS::Bedrock::Blueprint AWS::SageMaker::InferenceComponent


AWS::Bedrock::DataAutomationProject AWS::SageMaker::PartnerApp


AWS::BedrockAgentCore::ApiKeyCredentialProvider AWS::SageMaker::Project


AWS::Connect::UserHierarchyGroup AWS::SageMaker::Space


AWS::Glue::Trigger
