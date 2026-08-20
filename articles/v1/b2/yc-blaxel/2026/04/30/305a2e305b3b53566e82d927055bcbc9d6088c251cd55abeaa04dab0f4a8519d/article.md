---
schema_version: "1.0.0"
document_id: "305a2e305b3b53566e82d927055bcbc9d6088c251cd55abeaa04dab0f4a8519d"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/aws-lambda-gpu"
published_at: "2026-04-09T18:45:30+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:52:48.201959+00:00"
content_hash: "sha256:4fb85a5753d3d2f713f23d9dea1ada2d396ccddf5ee0a09371c1e0622d52077a"
---

# AWS Lambda GPU support in 2026: the state of serverless AI infrastructure

Your team is building an AI agent workload on AWS. Lambda handles orchestration well. Then someone asks where the GPUs go. A second question follows: where does the agent actually execute code? Lambda has no GPU support in 2026, and its stateless model resets between invocations. Those two gaps reshape the entire architecture.


This article covers how enterprise teams build around Lambda's GPU gap, the design constraints to plan for, and where agent execution infrastructure fits alongside GPU services.


## **The current state of Lambda GPU support**


AWS Lambda does not expose GPUs as a resource type. No GPU selection exists in the configuration console. No CUDA drivers ship in any runtime. No GPU billing dimension appears on any invoice.


The[Lambda quotas documentation](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html) defines memory allocation from 128 MB to 10,240 MB with proportional CPU scaling. Other limits include timeout, ephemeral storage, and concurrency. The[Lambda pricing page](https://aws.amazon.com/lambda/pricing/) lists GB-seconds of memory duration and per-request charges. No GPU-hour or GPU-second billing exists.


Lambda's resource model allocates CPU in proportion to memory. At[1,769 MB, a function gets one vCPU equivalent](https://aws.amazon.com/lambda/faqs/) . The hard limits for AI-relevant workloads are 10,240 MB memory, 900 seconds maximum runtime, and 10,240 MB ephemeral storage. These ceilings work well for orchestration, preprocessing, and lightweight inference on quantized models. They don't fit large model inference, training, or agents that need state across multi-step workflows.


AWS announced[Lambda Managed Instances](https://docs.aws.amazon.com/lambda/latest/dg/lambda-managed-instances.html) , allowing functions to run on EC2 managed instances in your account. No GPU instance types (P5, P6, G-series) are supported. The[re:Invent 2025 Lambda announcements](https://aws.amazon.com/blogs/compute/serverless-icymi-q4-2025/) included Durable Functions, Tenant Isolation Mode, and new runtimes. None introduced GPU compute.


AWS and NVIDIA[announced at GTC 2026](https://aws.amazon.com/blogs/machine-learning/aws-and-nvidia-deepen-strategic-collaboration-to-accelerate-ai-from-pilot-to-production/) that AWS will deploy over one million NVIDIA GPUs across regions starting in 2026. These expansions target EC2 instance capacity such as G7e instances, not Lambda.


### **How other clouds compare**


Google Cloud Run GPU reached[general availability](https://www.infoq.com/news/2025/06/google-cloud-run-nvidia-gpu/) for services. NVIDIA L4 GPUs are available at $0.0001867 per second without zonal redundancy. Cloud Run GPU services can scale to zero under instance-based billing. Cloud Run functions (2nd gen) run on the[unified Cloud Run infrastructure](https://cloud.google.com/blog/topics/inside-google-cloud/whats-new-google-cloud-2024) . An RTX Pro 6000 (Blackwell) option with 96 GB vGPU memory is[available in preview](https://cloud.google.com/blog/products/serverless/cloud-run-supports-nvidia-rtx-6000-pro-gpus-for-ai-workloads) .


Azure Container Apps supports GPU serverless containers with NVIDIA T4 and A100 options. Access requires[submitting a GPU quota request](https://learn.microsoft.com/en-us/azure/container-apps/gpu-image-generation) through a support case. GPU-enabled Azure Functions on Container Apps requires the[Dedicated plan with workload profiles](https://learn.microsoft.com/en-us/azure/azure-functions/functions-container-apps-hosting) .


The market is moving toward serverless GPU. For teams invested in AWS, neither alternative is a drop-in replacement. Using Cloud Run GPU with AWS-resident data in S3 or RDS introduces cross-cloud egress charges. Lambda's event-driven trigger breadth (SQS, SNS, S3, DynamoDB, Kinesis, EventBridge) remains unmatched for orchestration workloads, even without GPU support.


## **How enterprises use Lambda alongside GPU services**


Enterprise teams stopped waiting for native Lambda GPU support. They built hybrid architectures instead. Lambda handles orchestration, authentication, preprocessing, and routing. GPU services handle inference and training. Agents that execute code or interact with file systems across multiple steps need a dedicated execution layer. Lambda's stateless model can't handle these workloads.


[AWS Prescriptive Guidance on agentic AI](https://docs.aws.amazon.com/pdfs/prescriptive-guidance/latest/agentic-ai-serverless/agentic-ai-serverless.pdf) discusses related architectural patterns for agentic systems on AWS.


### **Lambda as the orchestration layer for GPU inference**


The most common production pattern follows a specific sequence. API Gateway receives the request. Lambda parses input, handles authentication, applies tenant routing, and runs light preprocessing. The request routes to a GPU-backed service for inference. Lambda collects and post-processes results before returning them through API Gateway.


The[AWS ML Blog documents this flow](https://aws.amazon.com/blogs/machine-learning/call-an-amazon-sagemaker-model-endpoint-using-amazon-api-gateway-and-aws-lambda/) : a client calls an API Gateway action, which passes parameters to Lambda, which sends them to the SageMaker endpoint. Lambda performs zero GPU compute. Its role is coordination.


A variant uses Amazon Bedrock instead of SageMaker. The[referenced AWS Compute Blog post](https://aws.amazon.com/blogs/compute/serverless-generative-ai-architectural-patterns) documents Bedrock and SageMaker as separate architectural options, not combined in the same slot. For teams that want fully managed model access without GPU provisioning, Bedrock handles inference at per-token pricing. Lambda manages everything around it.


One hard constraint to design for: API Gateway's default integration timeout is[29 seconds for synchronous requests](https://aws.amazon.com/blogs/compute/serverless-generative-ai-architectural-patterns) . Any inference workload exceeding this must use asynchronous patterns.


### **Hybrid batch and offline workloads**


Lambda triggers GPU-heavy batch jobs through AWS Batch or Step Functions. This decouples event ingestion (bursty, unpredictable) from GPU scheduling (expensive, capacity-constrained).


DTN's production weather prediction system demonstrates this at scale. Per a[summary of DTN's architecture](https://aws.amazon.com/blogs/hpc/how-dtn-accelerates-operational-weather-prediction-using-nvidia-earth-2-on-aws/) using NVIDIA Earth-2 on AWS, Step Functions orchestrates a three-phase workflow. AWS Batch manages compute resources by deploying Earth2Studio jobs across GPU-enabled instances. Lambda handles the event trigger. Step Functions manage state. AWS Batch runs the GPU compute. Each service does one thing well.


This separation works for training pipelines, large-scale embedding generation, and offline analysis. For agents that execute code across parallel tasks, the same separation applies even without GPU involvement. Processing document batches, running test suites, and analyzing datasets chunk by chunk all follow this pattern. Perpetual sandbox platforms like Blaxel provide[Batch Jobs](https://docs.blaxel.ai/Jobs/Overview) for parallel task processing. Jobs are triggered via HTTP endpoints or SDK/API calls with results returned or sent to callbacks.


### **On-CPU inference for lighter models**


Not every AI workload needs GPUs. The[AWS Prescriptive Guidance on inference architecture](https://docs.aws.amazon.com/pdfs/prescriptive-guidance/latest/gen-ai-inference-architecture-and-best-practices-on-aws/gen-ai-inference-architecture-and-best-practices-on-aws.pdf) discusses cost-effectiveness for generative AI inference on AWS. The[AWS Compute Blog confirms](https://aws.amazon.com/blogs/compute/deploying-ai-models-for-inference-with-aws-lambda-using-zip-packaging/) that Lambda functions run on CPU-only EC2 instances. It recommends llama-cpp-python for LLM inference on Lambda CPUs.


Quantized models in ONNX format can run on Lambda's CPU allocation. Text classification and entity extraction are viable candidates. Research benchmarks show that[INT8 quantization can improve CPU inference performance](https://arxiv.org/html/2601.03290v1) for NLP models.


Reported speedups reach 2.5-3.5x in some mixed-precision settings. The tradeoff becomes clear with generative tasks. A[DeepSeek-R1-Distill 7B INT4 model runs at 3.184 tokens/second on a high-end Intel i9](https://onnxruntime.ai/blogs/deepseek-r1-on-device) versus 161 tokens/second on an RTX 4090. Lambda's vCPU allocation is weaker than that i9. Generative workloads belong on GPU infrastructure. Encoder tasks that classify or extract stay on Lambda.


## **Architectural constraints to design around**


Lambda's limits collide with GPU-centric AI workloads in specific ways. Engineering leaders need to account for these when designing hybrid architectures. The wrong assumption about what Lambda can handle leads to overprovisioned GPU compute or intolerable latency.


### **Runtime and memory ceilings**


The[15-minute maximum execution](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html) and 10 GB memory cap force model serving onto dedicated GPU services. Lambda stays focused on orchestration and I/O. The 250 MB unzipped deployment package limit means models larger than a quantized encoder can't ship inside the function.


Larger models[require S3 download at init time](https://aws.amazon.com/blogs/compute/deploying-ai-models-for-inference-with-aws-lambda-using-zip-packaging/) , adding cold start overhead. For longer execution windows, Step Functions can chain Lambda invocations at the cost of added latency and complexity. Lambda Durable Functions (announced at re:Invent 2025) allow checkpoint, suspend, and resume cycles for[up to one year](https://www.aboutamazon.com/news/aws/aws-re-invent-2025-ai-news-updates) .


These same limits affect agents that execute code across multiple steps. An agent that clones a repository, runs tests, and analyzes results needs a persistent environment. Lambda's stateless model resets between invocations.


Every tool call starts from scratch. For coding assistants and PR review bots, environment state needs to survive between actions. Installed dependencies, cloned repos, and open files must persist without keeping infrastructure running. Persistent sandbox environments fill that role. They maintain state between actions while shutting down compute during idle periods. Blaxel doesn't guarantee long-term data persistence in standby. For guaranteed retention, use Volumes.


### **Cold starts in AI orchestration paths**


Lambda cold starts vary widely by configuration. Minimal functions start well under one second. Python functions with ML libraries often hit the 3-4 second range. Some image-based functions take up to 9 seconds. Benchmark data from[fourTheorem](https://fourtheorem.com/optimise-python-data-science-aws-lambda/) measured container-image Python functions with data science libraries at over 4,500 ms total init duration. Pandas alone accounted for 3,000 ms.


In an orchestration path, cold start latency stacks on top of inference time. For real-time AI applications,[provisioned concurrency](https://docs.aws.amazon.com/lambda/latest/dg/provisioned-concurrency.html) prewarms environments for double-digit millisecond startup but adds cost. SnapStart captures microVM snapshots to reduce cold starts. It[doesn't support container image deployments](https://docs.aws.amazon.com/lambda/latest/dg/snapstart.html) , which limits its use for Python ML functions that typically need container packaging.


The cold start problem compounds for agent architectures. Each step might trigger a new Lambda invocation, a GPU inference call, and a code execution request. If the execution environment also has multi-second cold starts, total response times push past what users tolerate.


## **Alternatives and complementary approaches for serverless GPU**


When Lambda can't cover the workload, several options fill the gap. The right choice depends on whether the workload needs GPU compute, CPU-bound execution with state, or both.


### **AWS-native GPU options**


Four AWS services cover different GPU workload types:


- **SageMaker endpoints** for managed GPU inference with auto-scaling. Supports a wide range of instance types, including Inferentia and Graviton. Real-time endpoints need at least one instance running. Async endpoints support[scale-to-zero](https://docs.aws.amazon.com/sagemaker/latest/dg/async-inference-autoscale.html) with MinCapacity=0. SageMaker Serverless Inference[doesn't support GPUs](https://docs.aws.amazon.com/sagemaker/latest/dg/serverless-endpoints.html) .
- **ECS and EKS with GPU-enabled EC2** for container-level control. EKS Auto Mode supports[GPU nodes natively](https://aws.amazon.com/blogs/containers/how-to-run-ai-model-inference-with-gpus-on-amazon-eks-auto-mode/) , provisioning GPU instances via Karpenter based on pod requirements. Fargate doesn't support GPU instances. All GPU container workloads require EC2 launch type.
- **AWS Batch** for scheduled GPU workloads. Supports[g4dn and g5 instance families](https://docs.aws.amazon.com/batch/latest/userguide/gpu-jobs.html) with explicit GPU resource requirements. The g6 and g6e families aren't listed as supported for GPU jobs. Batch integrates with Spot instances and can[submit SageMaker Training jobs](https://docs.aws.amazon.com/batch/latest/userguide/getting-started-sagemaker.html) through job queues.
- **Amazon Bedrock** for fully managed model access. Per-token pricing with no infrastructure provisioning. Supports[dozens of models](https://docs.aws.amazon.com/bedrock/latest/userguide/models-supported.html) from Anthropic, Meta, Amazon Nova, Mistral, and others. Batch inference runs at roughly[50% below on-demand pricing](https://aws.amazon.com/bedrock/pricing/) .


### **Third-party serverless GPU platforms**


Platforms like Modal and Replicate offer serverless GPU compute with Lambda-like programming models. Modal uses Python decorators that define GPU requirements inline with the function code. This contrasts with SageMaker's SDK-based approach, which requires explicit endpoint creation as a separate infrastructure step.


These platforms solve "attach a GPU to a function" directly. They introduce considerations around data locality. Running inference on a third-party platform when data resides in S3 introduces cross-cloud transfer charges. SageMaker Real-Time Inference supports VPC configuration for direct access to VPC-resident resources. Evaluate third-party platforms against data residency requirements and existing AWS investment before adopting.


### **Agent execution infrastructure**


A category distinct from GPU compute but equally critical for production agents is the execution layer. Agents that run code, interact with tools, or maintain state across multi-step workflows need more than Lambda's stateless model provides. ECS or EKS can work, but they require managing container lifecycle, scaling, and isolation yourself.


Perpetual[sandbox platforms](https://blaxel.ai/blog/ai-sandbox) handle this layer as a managed service. Blaxel provides isolated microVM environments where agents execute untrusted code with hardware-enforced tenant isolation. Sandboxes resume from standby in under 25ms with filesystem and memory state intact.


[Agents Hosting](https://docs.blaxel.ai/Agents/Overview) on the same infrastructure removes network roundtrip latency between the agent's reasoning loop and its execution environment. This matters for coding assistants, PR review agents, and workloads that make dozens of tool calls per interaction. The platform also provides[Model Gateway](https://docs.blaxel.ai/Models/Overview) for centralized LLM routing with telemetry and token cost controls,[MCP Servers Hosting](https://docs.blaxel.ai/MCP-Servers/Overview) for tool execution, and Batch Jobs for parallel fan-out processing.


## **How to design a serverless AI architecture on AWS**


One reference pattern covers most enterprise AI agent workloads. It separates a serverless control plane from distinct inference and execution planes. Each plane optimizes for a different workload type and scales independently.


### **Reference architecture**


Three layers, each owned by a different cost driver:


- **Control plane (Lambda and Step Functions).** Authentication, routing, preprocessing, post-processing, and workflow orchestration. API Gateway receives requests. Lambda handles lightweight logic. Step Functions manage multi-step agent workflows that exceed single-invocation scope.[AWS sample architectures](https://github.com/aws-samples/sample-serverless-pydantic-agents) illustrate how Lambda fits into orchestration as part of a broader serverless system.
- **Inference plane (GPU services).** SageMaker endpoints, Bedrock, or ECS GPU tasks handle model inference. The control plane routes requests based on model type, latency requirements, and cost policies. Costs depend on the mix of services used, including Bedrock inference and supporting components like storage or vector databases. Model selection is the primary FinOps lever. Switching from Claude Opus to Claude Haiku cuts inference cost. Haiku is priced lower per token within the same provider.
- **Execution plane (sandbox infrastructure).** Where agents run tools and interact with file systems. This layer needs isolation so one customer's code can't reach another's. It needs persistence, so the state survives between invocations. It needs low latency because agents make many sequential tool calls. Perpetual sandbox platforms like[Blaxel](https://docs.blaxel.ai/Sandboxes/Overview) serve this layer with microVM isolation and indefinite standby. Co-located Agents Hosting removes the network round-trip between the agent and execution environment.


The three planes scale independently. Lambda scales with request volume. GPU services scale with inference demand. Sandbox infrastructure scales with agent activity. FinOps teams can track spend separately because the cost drivers differ.


### **Governance and cost visibility**


Centralize metrics and tracing across all three layers. CloudWatch and X-Ray cover the AWS-native control plane.[CloudWatch Generative AI Observability](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GenAI-observability.html) provides token usage metrics, latency percentiles, and cost attribution by application or user role. GPU services on EKS support[split cost allocation data](https://aws.amazon.com/about-aws/whats-new/2025/09/split-cost-allocation-data-amazon-eks-nvidia-amd-gpu-trainium-inferentia-ec2/) for container-level accelerator tracking through AWS Cost and Usage Reports.


The execution plane needs its own observability for agent runs, tool call traces, and sandbox lifecycle metrics. Blaxel includes OpenTelemetry-based observability with metrics, logs, and traces designed for[agentic workloads](https://blaxel.ai/blog/how-to-deploy-ai-agents) .


Tag each layer separately (orchestration, inference, execution) to allow per-layer rollups in cost reporting. Inference and compute choices typically drive costs. Lambda orchestration is a much smaller line item. Sandbox compute is charged during active execution. Standby preserves the state at storage cost only.


## **What this means for your architecture**


Lambda's GPU gap reflects a fundamental design boundary, not temporary friction. Lambda optimizes for stateless, event-driven coordination. GPU inference optimizes for throughput on dedicated hardware.


Agent code execution optimizes for isolated, stateful environments that persist between tool calls. These three workload types scale differently, cost differently, and fail differently. Teams that treat them as a single compute problem overpay on one axis and underperform on another.


The three-plane separation covered in this article (Lambda for orchestration, GPU services for inference, sandbox infrastructure for agent execution) isn't a workaround. It's the architecture that lets each layer evolve independently. When AWS eventually adds GPU options to Lambda, only the inference routing changes. Everything else stays.


Perpetual sandbox platforms like[Blaxel](https://blaxel.ai/) provide the execution plane for agents that run code in production. Sandboxes resume from standby in under 25ms with complete filesystem and memory state intact.


[Agents Hosting](https://blaxel.ai/agents-hosting) co-locates the agent's reasoning loop with its execution environment to remove network round-trip latency. Batch Jobs handle parallel fan-out processing. MCP Servers Hosting provides tool execution with 25ms boot times. Model Gateway centralizes LLM routing with built-in telemetry and token cost controls. Together, these fit alongside AWS-native orchestration and inference services without replacing them.


[Sign up free](https://app.blaxel.ai/) to deploy your first agent, or[book a call](https://blaxel.ai/contact) to discuss your architecture.


Run AI agents alongside your AWS stack


Sandboxes resume in under 25ms. Co-located Agents Hosting eliminates network latency. Up to $200 in free credits.


[Start free](https://app.blaxel.ai/)


## **Frequently asked questions about AWS Lambda GPU support**


### **Does AWS Lambda support GPUs in 2026?**


No. Lambda has no GPU resource type, no CUDA driver exposure, and no GPU billing dimension. There is no public roadmap commitment for GPU support. Lambda's resource model is defined in terms of memory with proportional CPU allocation. The re:Invent 2025 announcements (Managed Instances, Durable Functions, new runtimes) didn't introduce GPU compute. AWS's GPU expansion focuses on EC2 instance capacity for SageMaker, ECS, EKS, and AWS Batch.


### **Can I run AI inference on AWS Lambda without GPUs?**


Yes, for lightweight models. AWS demonstrates quantized CPU inference using llama-cpp-python for 4-bit quantized GGUF models. Encoder-only tasks like text classification and entity extraction are viable. Generative workloads show a large GPU throughput advantage over CPU, making them better suited to dedicated GPU infrastructure.


### **How should I use Lambda with GPU services on AWS?**


Production architectures use several orchestration patterns. These include Step Functions, Lambda Durable Functions, and event-driven approaches with EventBridge. API Gateway receives requests. Lambda handles authentication, input validation, and routing. It then calls SageMaker endpoints or Amazon Bedrock for inference.


### **How do Lambda cold starts affect AI workloads?**


Python functions with ML libraries can see cold starts in the 3,000-4,500 ms range. Timings vary by dependency set, packaging method, and memory configuration. This latency stacks on top of GPU inference time in orchestration paths. Provisioned concurrency eliminates cold starts by prewarming environments but adds cost. SnapStart doesn't support container image deployments, which limits its use for heavy Python ML functions.


### **Where should agent code execution run if not on Lambda?**


Lambda's stateless model resets between invocations. Agents that need persistent filesystems or multi-step code execution need a different layer. Perpetual sandbox platforms like Blaxel serve coding assistants, PR review tools, and data analysis workflows. Sandboxes resume from standby in under 25ms. Agents Hosting removes network roundtrip between agent and execution environment. Batch Jobs handle parallel workloads. MCP Servers Hosting provides tool execution. Model Gateway offers unified LLM routing. These components fit alongside AWS-native GPU infrastructure.
