---
schema_version: "1.0.0"
document_id: "e6ce4e4c47eb18f0f2fc2a2fde5c5a5a112b8b73ac2716f02812478ec30071d4"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/ec2-auto-scaling-instance-refresh-cloudformation"
published_at: "2026-07-29T21:00:00+00:00"
first_seen_at: "2026-07-29T23:44:37.452284+00:00"
fetched_at: "2026-07-29T23:44:39.530052+00:00"
content_hash: "sha256:f2679c35d03b784f292ef2cfdaab69e36c426493ab20bdb29fd6b74b2faae850"
---

# Amazon EC2 Auto Scaling now supports Instance Refresh in CloudFormation

Amazon EC2 Auto Scaling now supports Instance Refresh as a new AWS CloudFormation update policy. When you configure the new AutoScalingInstanceRefresh update policy and update properties that require instance replacement, CloudFormation automatically triggers an Instance Refresh.


With this integration, you can now access Instance Refresh capabilities including replace root volume for in-place updates, launch-before-terminate, alarm-based monitoring, and checkpoints with bake time for controlled rollouts. Auto Scaling features such as scaling policies and health checks remain active throughout the update, so your service health is not at risk during deployments. Rollback is handled through CloudFormation stack rollback.


This feature is available in all AWS Regions at no additional cost. To learn more, see[AutoScalingInstanceRefresh update policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-attribute-updatepolicy.html#cfn-attributes-updatepolicy-instancerefresh) in the AWS CloudFormation Template Reference.
