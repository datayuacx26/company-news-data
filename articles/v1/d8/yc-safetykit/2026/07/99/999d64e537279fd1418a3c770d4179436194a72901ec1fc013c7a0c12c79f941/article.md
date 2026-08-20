---
schema_version: "1.0.0"
document_id: "999d64e537279fd1418a3c770d4179436194a72901ec1fc013c7a0c12c79f941"
company_key: "yc-safetykit"
company: "SafetyKit"
source_id: "yc-safetykit-news-import-5b3167bc4180"
canonical_url: "https://www.safetykit.com/news/safetykits-engineering-culture"
published_at: null
first_seen_at: "2026-07-22T12:32:34.588323+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:17dd0e568b9f34dbc4f29cd548324cc7991ea124e375b7d10b95940c131734f0"
---

# SafetyKit's Engineering Culture

# SafetyKit's Engineering Culture


[Steven Guichard, CTO](https://www.linkedin.com/in/stevenguichard/)


*SafetyKit is hiring engineers, AEs, and GTM generalists. Join us:[https://www.safetykit.com/jobs](https://www.safetykit.com/jobs)*


SafetyKit has an intentionally-designed engineering culture built for autonomy, output, and speed. SafetyKit processes 20 billion LLM tokens per day with a small engineering team. We support multiple Fortune 500 companies, in many cases processing 100% of their production risk and compliance data. They have hundreds of engineers supporting their own scale. We have 12 (who are busy building, inventing, and scaling it!). My cofounder David and I have been running engineering teams together for 15 years. We’ve operated inside the world’s largest companies, handled hundreds of billions of dollars in payments risks, and scaled to thousands of demanding customers. SafetyKit is built on what we’ve learned. SafetyKit is engineering-led from top to bottom. Individual engineers independently manage large portions of the product, directly manage customer relationships, and are responsible for running and monitoring production deployments. Without careful thought this could be an overwhelming amount of work. Here's how we pull it off:


## Serverless and Infra-as-Code


We don't have DevOps engineers. We don't have a platform team. Every engineer is a full-stack engineer who owns their infrastructure. We're all-in on serverless. Lambda functions scale from zero to millions of requests without capacity planning. DynamoDB and S3 handle our state. Everything is provisioned through[AWS CDK](https://docs.aws.amazon.com/cdk/v2/guide/home.html) . If you can't` git diff` it, it doesn't go to production. When an engineer ships a feature, they ship the infrastructure too. You write the code, you write the CDK, you deploy it. The infrastructure lives next to the application code in our monorepo. Scaling is effortless. When a customer goes from 10 million tokens a day to 100 million, nothing happens on our end. No capacity planning meetings. No provisioning servers. Lambda concurrency increases automatically. DynamoDB scales up. When a Fortune 500 customer has a traffic spike, our system handles it. The first we hear about it is when we see the metrics go up. Engineers stay focused on building product instead of managing infrastructure.


## Automation Everywhere


If you do something twice, automate it the third time. This isn't a suggestion; it's how we survive. Every deployment goes through CI/CD. Every test runs automatically. We're ruthless about eliminating toil. Manual processes don't just slow you down when you're a team of 12; they kill you. We'd rather spend 4 hours automating a 10-minute task than do it manually for six months. When a Fortune 500 customer asks, "Can you process 10x more volume next week?" the answer is yes because the automation handles it.


## Consistent Monorepo


Our entire codebase—backend services, infrastructure, data pipelines, monitoring—lives in one repository. One PR can touch the API server, update the CloudFormation, and modify the alerting thresholds. Everything versions together. Everything is TypeScript. Shared types flow from frontend to backend to infrastructure. We use Zod for runtime validation, which means our schemas are both TypeScript types and runtime validators. When we change an API contract, TypeScript catches every callsite that needs updating. This drives consistency by default. When we improve our token processing pipeline, every service gets the update. When we harden security, it propagates everywhere. Onboarding is fast and context switches are cheap. New engineers clone one repo and see the entire system. An engineer can jump from the classification pipeline to the customer API because it's the same codebase with the same patterns. The coordination overhead of distributed repos would require more people. The monorepo keeps us fast.


## Optimizing for Blast Radius


With 12 engineers each owning substantial surface area, mistakes happen. The question is: how do we contain them? We design for graceful degradation. Services are loosely coupled. Customer A's deployment is isolated from Customer B's. We model compute by customer. A bug processing one customer's tokens doesn't cascade. Deploys go to internal environments first, then a canary customer, then broader release. Small team means high trust, but every engineer thinks about blast radius before shipping. We don't have layers of review because that would slow us down. Instead, we have testing, observability, and a culture of asking, "What's the worst that could happen, and how do I limit it?" When something breaks, the observability stack identifies the scope and pages the owner. They fix it, write a post-mortem, and we extract the lesson into guardrails or automation. This is how 12 engineers support billions of tokens a day for the world's largest companies. Not by working around the clock. By building systems that scale without us.
