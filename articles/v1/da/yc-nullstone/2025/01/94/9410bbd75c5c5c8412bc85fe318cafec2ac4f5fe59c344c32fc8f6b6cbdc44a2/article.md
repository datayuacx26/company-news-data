---
schema_version: "1.0.0"
document_id: "9410bbd75c5c5c8412bc85fe318cafec2ac4f5fe59c344c32fc8f6b6cbdc44a2"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/how-evolve-10x-deployments-and-environments"
published_at: "2025-01-17T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:75ce4204326be09d76bde16cc67e928aed8e6c383cb36a4c848ccaf4ff524f79"
---

# How Evolve 10x deployments and environments

# Overview


Evolve faced challenges that are all too common with cloud-based product teams. While building the first version of the product, the team brought in a DevOps engineer to help with initial infrastructure and CI/CD. As customers adopted Evolve’s cloud solution, DevOps became a bottleneck prompting Evolve to consider alternatives. Nullstone’s managed service helped Evolve adopt developer self-service leading to massive gains in developer productivity.


# Who is Evolve?


EVOLVE MEP is a software and services company dedicated to continuous improvement of the mechanical, electrical, and plumbing (MEP) trades within the construction industry. The company offers a suite of tools designed to streamline the VDC (virtual design construction) workflow and facilitate a seamless transition from design to fabrication. Their products include Evolve Electrical and Evolve Mechanical - detailing and modeling automation tools for Autodesk Revit - as well as Evolve Shop, a cloud hosted solution for shop floor automation.


# Challenge: Product development slowed after releasing product


As customer demand grew, Evolve faced challenges that are extremely common with growth-stage products. The delivery process became brittle, security/compliance issues grew in importance, technical debt grew, and DevOps could no longer keep up with the development of the product.


Early on, Evolve shipped code quickly; however, as the product grew, so too did the infrastructure. With this growth and complexity came a brittle delivery process. As customers joined, they could only deploy during off-hours because each deployment caused downtime and much manual effort. Many deployments required migrations which would sporadically fail or timeout. This made for a stressful situation, prompting all developers to be available during off-hours deployments.


As the development team grew commensurate with customers, Evolve faced security and compliance issues with larger and broader impact. Developers had full access to production infrastructure — a policy that existed because developers needed production access to operate the product and continue building features and fixing bugs. Evolve needed to address this to achieve compliance for their customers, but also feared that a developer would make a mistake and cause production downtime.


Evolve had looming technical debt pronounced by increasing constraints from customers. Since Evolve adopted many “quickstart” technologies, they were stuck without the necessary controls to run in production. They could stay with these technologies, but it meant a mediocre product and capped development velocity. They faced another common post-MVP problem, the system had performance issues and a swath of bugs that they needed to quell. Without robust observability (i.e. logs, metrics, and tracing), they were frustrated with an inability to diagnose issues.


All of these problems compounded into a situation where “DevOps” was unable to keep pace with the developers. Developers wanted to stand up more environments to isolate feature development, but it could not since DevOps took one week to configure each new environment. Along with other requests to change infrastructure, DevOps became a bottleneck for solving many high priority issues. Do they focus on security/compliance improvements, improving deployments, developer diagnostics, or put out production fires?


# Solution: Adopt self-service via Nullstone managed service


Evolve adopted a “you build it, you run it” philosophy. However, they needed a path to get there and a way to maintain going forward. Evolve engaged with Nullstone to achieve this goal with four initiatives.


## Build automated, reliable CI/CD pipeline


Nullstone collaborated with Evolve to understand their current architecture, then developed a plan to improve reliability of deployments, automate the entire pipeline (including migrations), add admin approvals for production, and implement canary deploys to achieve zero-downtime deploys. Once Evolve was happy with the proposed changes, Nullstone built a new pattern for CI/CD pipelines in CircleCI that coordinated infrastructure changes, deployments, admin approvals, and database migrations. Used in tandem with the Nullstone platform, Evolve’s developers could quickly add or change services without impairing automation.


## Automate environment management with Nullstone


During the process of rebuilding the CI/CD pipeline, Evolve codified their architecture into the Nullstone platform. By using Nullstone’s official Terraform modules, Evolve transitioned to environments that were fully controlled by Infrastructure-as-code with the benefit of managing through GitOps or through a UI. In doing, they removed all environment disparities and gained the ability to launch an entire environment with a single click or through a GitHub pull request. Evolve’s infrastructure pipeline kept changes synchronized and prevented future drift.


### Implement standard security practices using Nullstone blueprints


With Nullstone’s catalog of blueprints and Terraform modules, Evolve dramatically improved the security and compliance of their infrastructure. Out of the box, Evolve gained the following benefits:
- Services are provisioned in private networks by default and can only be accessed from the internet through secure appliances (e.g. CDN, Load Balancer, SSH Tunnel).
- Production infrastructure is located in separate AWS accounts (rolled up into a single AWS organization for billing) so that developers can experiment/access non-production infrastructure without exposing production.
- AWS IAM is configured with least-privileged access. A malicious actor cannot exploit automations to gain broader access to sensitive resources.


By adopting this “secure-by-default” strategy, Evolve’s development velocity did not trade security — the easiest and quickest solution is the most secure.


## Improve developer visibility/productivity using Nullstone


Finally, Evolve’s developers gained access to a central, self-service portal. This allowed them to stay focused on shipping features and bug fixes. The Nullstone UI and CLI provide quick access to logs in a consistent format whether viewing an AWS Lambda app or an ECS container app. Metrics are viewable from a central UI without any configuration or setup. A developer can quickly request new infrastructure resources — an admin is given a list of changes to approve. Changes to infrastructure and deployments are synchronized and centralized into a single activity stream. With Nullstone’s in-depth logs and dashboards, Evolved greatly expanded visibility and diagnostics for deployments.


# Results


> EVOLVE’s implementation of Nullstone has revolutionized our cloud infrastructure and development environments. We have realized both an increase in stability and reliability of deployments while drastically decreasing our DevOps overhead. Nullstone’s staff has been fantastic in supporting us every step of the way.
>
> Jason Faulkner, VP of Engineering


Evolve dramatically accelerated feature development and reduced operational overhead.
- Evolve is deploying several times a week. (Before: 1-2 times a month)
- Ephemeral environment creation takes ~2 hours. (Before: 1 week)
- Evolve is able to manage ~10-15 ephemeral environments in parallel. (Before: 2 environments)
- Deployments have zero downtime.
- A single developer can deploy during work hours.
- Developers no longer have access to sensitive resources.
- Dramatically reduced technical debt and ongoing security maintenance.
- Recognized a drastic reduction in overhead costs for DevOps resources


# Conclusion


Evolve's journey with Nullstone is a testament to the transformative power of embracing self-service and automation. By eliminating the bottlenecks of traditional DevOps and empowering developers with tools to take ownership of their workflows, Evolve not only addressed its immediate challenges but also positioned itself for sustainable growth and innovation.


Through automated CI/CD pipelines, secure infrastructure practices, and enhanced visibility, Evolve now delivers features faster, operates with greater resilience, and ensures security and compliance. The collaboration highlights the value of solutions like Nullstone that prioritize simplicity, efficiency, and scalability, enabling teams to focus on what truly matters — building exceptional products for their customers.


‍
