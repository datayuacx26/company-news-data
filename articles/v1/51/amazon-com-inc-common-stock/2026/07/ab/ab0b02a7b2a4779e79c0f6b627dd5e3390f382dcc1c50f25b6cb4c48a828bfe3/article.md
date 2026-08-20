---
schema_version: "1.0.0"
document_id: "ab0b02a7b2a4779e79c0f6b627dd5e3390f382dcc1c50f25b6cb4c48a828bfe3"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/ecs-adv-deployments-eu-sovereign-cloud/"
published_at: "2026-07-21T17:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:c3b04daa949e99995b5479ecb0ca0bb79c852a880cc34ab3305bfac2802f3cb4"
---

# Amazon ECS advanced deployment strategies now available in AWS European Sovereign Cloud

[Amazon Elastic Container Service](https://aws.amazon.com/ecs/) (Amazon ECS) now supports built-in blue/green, linear, and canary deployment strategies in the AWS European Sovereign Cloud, making software updates for containerized applications safer and allowing you to release new application versions faster with greater confidence. Built directly into Amazon ECS, these capabilities eliminate the need for custom deployment tooling while providing production-ready controls including deployment lifecycle hooks, bake time, and quick rollback.


With these strategies, Amazon ECS provisions a new application version alongside the existing version and allows you to validate it before shifting production traffic. Blue/green deployments shift traffic in a single step, linear deployments shift traffic in equal increments over a specified period, and canary deployments shift a small percentage initially before shifting the remainder. You can use deployment lifecycle hooks, including Lambda hooks and pause hooks, to run custom validation logic and approval workflows at specific deployment stages. You can also configure bake time to evaluate the new version after traffic has shifted, and roll back without downtime if regressions are identified. To automatically detect failures, configure Amazon CloudWatch alarms or Amazon ECS deployment circuit breaker, or initiate a rollback directly using the StopServiceDeployment API.


These capabilities are available for Amazon ECS services using Application Load Balancers (ALB), Network Load Balancers (NLB), and Amazon ECS Service Connect. You can configure deployments using the AWS Management Console, AWS CLI, AWS SDKs, or infrastructure-as-code tools on new or existing Amazon ECS services. To learn more, see our documentation for[Amazon ECS blue/green](https://docs.aws.eu/AmazonECS/latest/developerguide/deployment-type-blue-green.html) ,[linear](https://docs.aws.eu/AmazonECS/latest/developerguide/deployment-type-linear.html) , and[canary deployments](https://docs.aws.eu/AmazonECS/latest/developerguide/canary-deployment.html) , and[deployment lifecycle hooks](https://docs.aws.eu/AmazonECS/latest/developerguide/deployment-lifecycle-hooks.html) .
