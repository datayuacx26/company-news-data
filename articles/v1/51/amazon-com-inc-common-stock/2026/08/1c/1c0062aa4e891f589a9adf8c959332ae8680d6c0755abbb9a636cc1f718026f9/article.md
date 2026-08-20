---
schema_version: "1.0.0"
document_id: "1c0062aa4e891f589a9adf8c959332ae8680d6c0755abbb9a636cc1f718026f9"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-emr-ec2-spark-connect/"
published_at: "2026-08-04T20:29:00+00:00"
first_seen_at: "2026-08-04T23:02:13.139824+00:00"
fetched_at: "2026-08-04T23:10:21.652752+00:00"
content_hash: "sha256:1224874e85ce7ab705d304cb7d141bc822acf16fdb20ef6bedf2897adc7a5c01"
---

# Run interactive workloads on Amazon EMR on EC2 with Spark Connect

Amazon EMR on EC2 now supports interactive Apache Spark sessions with Spark Connect. Data engineers and data scientists can develop and debug Apache Spark applications interactively from managed notebooks in Amazon SageMaker Unified Studio and their own IDEs, such as Jupyter and Visual Studio Code, with each session running on dedicated EMR on EC2 clusters. You can also monitor and debug active and completed sessions in the EMR console.


An interactive session provides a persistent Spark context that spans across cells and scripts, letting you blend local Python code execution with remote Spark operations. Spark Connect's client-server architecture decouples your application client from the Spark driver and allows you to maintain your preferred development environment and tooling while Spark infrastructure runs on the cluster. This architecture supports workflows including ad hoc data exploration, iterative step-by-step debugging, and incremental PySpark job development before deploying to production. For observability, you get real-time session monitoring via the Spark UI, history tracking through the Spark History Server, and session management from the EMR console or API/CLI/SDK.


Interactive Sessions is available on Amazon EMR on EC2 with AWS runtime for Apache Spark (emr-spark-8.0) and later, in all AWS Regions where Amazon EMR is available, except the AWS GovCloud Regions and the China Regions. The Amazon SageMaker Unified Studio experience is available in[supported regions](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/adminguide/supported-regions.html) . To get started, visit the[Interactive sessions with Spark Connect](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-spark-connect-sessions.html) guide or the[Amazon SageMaker Unified Studio Getting Started guide](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/what-is-sagemaker-unified-studio.html) .
