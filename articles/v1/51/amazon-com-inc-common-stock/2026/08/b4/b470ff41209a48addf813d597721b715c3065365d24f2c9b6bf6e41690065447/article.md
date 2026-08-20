---
schema_version: "1.0.0"
document_id: "b470ff41209a48addf813d597721b715c3065365d24f2c9b6bf6e41690065447"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-ecs-fractional-gpu/"
published_at: "2026-08-06T22:00:00+00:00"
first_seen_at: "2026-08-07T00:09:00.624860+00:00"
fetched_at: "2026-08-07T00:09:02.541202+00:00"
content_hash: "sha256:fc0f90985e602cc966f51c0ee2de306960d4f1bf58fe417c9df952658747ba62"
---

# Amazon ECS now supports fractional GPU scheduling with Amazon EC2 G6f instances

[Amazon Elastic Container Service](https://aws.amazon.com/ecs/) (Amazon ECS) now supports fractional GPU scheduling with Amazon EC2 G6f instances, enabling you to run your workloads on GPU partitions as small as one-eighth of an NVIDIA L4 Tensor Core GPU with 3 GB of GPU memory. Fractional GPUs give you the flexibility to right-size your containers for small-model AI inference, model experimentation, graphics rendering, and other workloads that do not require a full GPU, helping reduce infrastructure costs compared to provisioning a full GPU instance.


You can request a fractional GPU by setting GPU=0.125, GPU=0.25, or GPU=0.5 in the container definition of your Amazon ECS task definition. Amazon ECS then places the task on a G6f instance that satisfies the request. Fractional GPU configuration is supported on both Amazon ECS Managed Instances and Amazon ECS on EC2. With[ECS Managed Instances](https://aws.amazon.com/ecs/managed-instances/) , you get a fully managed experience where ECS automatically handles instance provisioning, scaling, patching, and lifecycle management, so you can focus on your GPU workloads rather than infrastructure operations. ECS Managed Instances also include capabilities built specifically for accelerated workloads, such as GPU metrics through[Amazon CloudWatch Container Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-enhanced-observability-metrics-ECS.html) , and[automatic health monitoring](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/managed-instances-gpu-auto-repair.html) that detects GPU hardware failures and replaces unhealthy instances to minimize workload disruption.


This capability is available in all AWS Regions where Amazon EC2 G6f instances are available. To get started, use the AWS Management Console, AWS CLI, AWS SDKs, AWS CloudFormation, or other infrastructure-as-code tools to configure a capacity provider with G6f instances and specify a fractional GPU value in the container definition of your ECS task definition. To learn more, visit the[Amazon ECS fractional GPU documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-gpu-specifying.html#ecs-gpu-specifying-fractional) and the[Amazon EC2 G6 instance page](https://aws.amazon.com/ec2/instance-types/g6/) .
