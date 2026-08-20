---
schema_version: "1.0.0"
document_id: "90704d2844804cd36672207e9d5f050bb3cb2142e84da95b56499a2adf02b8a3"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/terraform-self-service"
published_at: "2023-03-24T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:3a76bf71da42d10bcc3545c4a1f8a314a2613a3044ae26cd78f9dee3d46ca5e8"
---

# Curation is vital for Terraform self-service

## Why Provide Developer Terraform Self-Service?


Infrastructure is typically the responsibility of specialized DevOps and Infra teams. In most software organizations, developers make requests through these teams for any infrastructure, and this governance is valuable to enforce security and compliance protocols. However, this impairs developer productivity and limits a team's ability to ship. As a result, many DevOps practitioners have preached moving the infra engineers into their respective teams to accelerate delivery. Still, unfortunately, this spreads infra engineers thin and requires enormous overhead to operate a team.


Instead, we should let developers operate their infrastructure, right? The "you build it, you run it" philosophy enables shipping software faster. Terraform is a universal developer interface for infrastructure, making it a logical choice for developer self-service. However, it can also significantly distract developers focused on new product features or fixing critical bugs.


Many software organizations are turning to Terraform self-service as an unlock for developer productivity while ensuring no reliability issues or security breaches affect customers negatively. But, first, look at how Terraform self-service works and the hurdles to implementing an effective program in a software organization.


## How does Terraform self-service work?


An internal team of infrastructure experts builds and maintains a catalog of infrastructure components as Terraform modules. Then, developers choose which components to use for the apps they manage. As needs evolve, the platform team upgrades each Terraform module and coordinates with developers to upgrade their infrastructure. (Some teams manage Helm charts and other Infrastructure-as-Code formats within this program, but the primary focus is on Terraform since it is a universal format for all providers and platforms.)


## Sounds easy; what could go wrong?


In concept, this sounds like a balanced and straightforward approach; however, this introduces other problems that can sink the effectiveness of an infrastructure team unless adequately addressed. Therefore, it's essential to understand the hurdles that can derail the program. Otherwise, confused developers will lobby for a new program or set up shadow tech.


### Which modules do I use?


The first hurdle a developer will experience is choosing a module to launch (or migrate) their application. Developers genuinely care about selecting the right one for their use case but have little to no awareness of internal infrastructure management and usually limited cloud knowledge. It is up to the platform team to name modules appropriately and provide guidance on which module to use.


Most apps rely on other data stores and vendors to operate. To stitch these together, developers usually need "glue" Terraform connecting an application to the data store and configuring the application with the endpoint and credentials. Some organizations reach a fork in the road when developing modules to support various use cases.


1. Provide a prescribed tech stack. For example, one module supports a web app with ruby, postgres, and redis.


2. Provide composable modules that work together. For example, there are three modules: a module for a ruby web app, a module for postgres, and a module for redis.


Depending on the organization's strategy, providing a path of least resistance to developers is crucial. The biggest challenge to the platform is providing modules that are compatible with each other. For instance, if a postgres module configures a container application using Ruby on Rails, will another developer be successful if they use that same module to launch a Python Django app deployed as an AWS Lambda function? Since the variations compound quickly, developers must know which module works for their use case.


### How do I configure the module?


Now, let's assume developers have chosen the correct modules. Now, they must configure them properly so that the app launches gracefully, maintains uptime, and scales appropriately to demand. Too often, our teams see platform teams expand their variables to cover all use cases instead of serving a targeted need. As a result, this leaves developers confused and frustrated while wasting precious time.


The first mistake that platform teams make is providing too many variables. If your platform team follows the most popular modules on the public Terraform registry, you'll notice they have 50+ variables for a fundamental component. This strategy is the quickest way to confuse developers.


Another mistake is not detailing a variable's description and possible choices so a developer knows what to expect. The description of the variable must speak to the developer and not a technical specification for how it works. (The source code should be accessible to a developer — they can explore how it works.) Unfortunately, Terraform has no built-in mechanism for declaring a list of possible choices. For example, when creating an EC2 instance, you need to specify an instance type that determines the resources on the box. How does a developer know which instance types are available to choose from? Or, how can the team provide supplemental knowledge to the developers?


### What happens if Terraform errors?


Debugging Terraform errors can be a frustrating experience for developers unfamiliar with the tool. Two main types of errors can occur: Terraform syntax and cloud provider errors. Usually, Terraform syntax errors result from not testing or poorly testing conditional cases in the Terraform scripts. On the other hand, cloud provider errors result from issues with the cloud infrastructure itself, such as exceeding IP address limits, resource conflicts, exceeding quotas, or many other scenarios.


Developers must not be bombarded with Terraform errors when provisioning and updating. If developers encounter too many Terraform errors and cannot resolve their issues, the platform team will regress into firefighting — defeating the goal of self-service. Instead, a developer should have the resources to resolve or communicate the problem to the module creator.


### How do I deploy my app?


After provisioning the infrastructure, developers need guidance to deploy their applications effectively. Developers want to build new features, not configure yml files to make their deployments work correctly. There are several pieces of configuration to coordinate the deployment code with the correct cluster configuration. CI is tough to configure when developers cannot test and execute locally. There's nothing more frustrating than spending a week building a feature only to get stuck deploying it. Because of these challenges, platform teams must provide simple deployment practices that are reliable and easy to maintain.


### Can I launch a new environment on demand?


While Terraform self-service can empower developers to manage their infrastructure, launching a new environment can still be tricky and time-consuming. For some organizations, standing up new environments is complex, and \[staging is frequently broken\](https://www.nullstone.io/blog-posts/why-is-staging-always-broken). In addition, remember that launching a new environment isn't a one-and-done operation — the team needs to ensure that environments stay in sync with production over time. These difficulties incur significant overhead and discourage developers from testing their changes before releasing them.


Here are some challenges that teams usually face when launching environments:


1. Developers do not have access to or understanding of necessary configuration and resources. (API keys, database endpoints, etc.)


2. The launch and deployment processes are unreliable. For various reasons, it takes several attempts to launch the app.


3. Developers do not have access to cloud accounts or environments.


## Conclusion


While Terraform self-service can increase developer productivity and accelerate delivery, the platform team must address several hurdles to make it effective. These include:


1. Providing clear guidance on which modules to use


2. Ensuring compatibility between modules


3. Streamlining module configuration


4. Minimizing Terraform errors


5. Providing resources for app deployment


6. Enabling developers to launch new environments on demand.


***Curation*** is crucial to the success of self-service. Remember that the ultimate goal is empowering developers while ensuring customers aren't victims of downtime or security breaches.


‍
