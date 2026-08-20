---
schema_version: "1.0.0"
document_id: "88058039d33497c9d45557baab3fdd6257b6d9c28fdb8ed37de900f944e69bc2"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/developer-productivity-ephemeral-environments/"
published_at: "2024-08-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:3b8056ab1dc0c4d367501ab2b1174ce37ccbf7c08f0e0aab3563f690982e07cd"
---

# Elevating Developer Productivity with Speedscale Ephemeral Environments

## TL;DR


Speedscale leveraged Ephemeral / Preview Environments to help balance developer productivity and high-quality code by dynamically spinning mini-staging environments up and down on command for experimentation and deployment safety. The primary goals of the project were to reduce cloud infrastructure operating costs and increase deployment frequency. Benefits included speed, resource efficiency, and improved collaboration.


## What are ephemeral environments?


Ephemeral or[Preview Environments](https://speedscale.com/blog/preview-environments/) help balance the need for developer productivity while maintaining high-quality code. The idea of dynamically spinning “mini-staging” environments up and down on command has enormous promise for increasing experimentation and deployment safety. In theory, it lets you[test in prod](https://speedscale.com/blog/shift-right-testing/) without actually breaking prod. In this brief engineering leader-level summary we’ll talk about the business benefits we’ve experienced from ephemeral environments as well as an overview of technical challenges. Ephemeral environment tools are commonly thought of as infrastructure deployment tools but this article will show that this assumption is incomplete. Replicating the infrastructure turns out to only be the first task and not necessarily the most difficult. The primary goals of our project were to:


1.


Reduce cloud infrastructure operating costs and


2.


increase deployment frequency. Measuring deployment frequency is an easy metric to game but in this case we made a judgment call that velocity was increasing proportionally to deployments.


Prior to using ephemeral environments, we used traditional developer, staging and production environments exclusively. At the end of the project we retained these environments but were able to dramatically reduce our staging and dev cloud costs. Our developer team is small but we’ve experienced similar gains rolling out these solutions at larger companies. Continue reading to hear our learnings for self-service developer productivity.


## !\[Practice soccer field\](./content-image.jpg


)


## Why Ephemeral Environments?


### Just-in-time Cost Reduction:


- **Speed:** Ephemeral environments can be quickly provisioned and destroyed. This agility allows developers to create isolated environments for each feature branch, bug fix, or experiment, ensuring that testing and validation can begin immediately without waiting for shared resources.
- **Resource Efficiency:** These environments consume resources only when needed. Once the task is completed, the environment is torn down, freeing up resources for other tasks. This dynamic allocation of resources optimizes infrastructure costs.


### Isolation and Consistency:


- **Reduced Conflicts:** By providing isolated environments for each development task, we minimize the risk of environment-related conflicts. Developers can work independently without worrying about dependencies or configuration changes affecting their work.
- **Consistency:** Ephemeral environments are consistently provisioned using predefined templates. This ensures that every environment is identical, reducing the “works on my machine” problem and increasing code quality by making it easier to reproduce and fix bugs.


### Enhanced Testing Capabilities:


- **Realistic Testing:** Developers can test their code in environments that closely mimic production. This leads to more accurate testing results and higher confidence in the quality of the code.
- **Automated Testing Integration:** These environments can be integrated with CI/CD pipelines to automatically spin up environments for testing, run automated tests, and tear them down once testing is complete. This streamlines the development workflow and reduces manual intervention.


### Improved Collaboration:


- **Review and Feedback:** Ephemeral environments make it easier for team members to review and provide feedback on code changes. Developers can share links to these environments, allowing reviewers to see the changes in action and provide more meaningful feedback.
- **Experimentation:** Developers can experiment with new technologies, frameworks, or approaches in isolated environments without risking the stability of the main development or production environments.


### Faster Iterations and Deployment:


- **Shorter Development Cycles:** By enabling development teams to quickly test and validate their changes, ephemeral environments reduce the time it takes to move from development to production. This accelerates the overall development cycle and allows the team to deliver features and fixes faster.
- **Continuous Deployment:** With the confidence gained from thorough testing in ephemeral environments, the team can adopt continuous deployment practices, releasing smaller, incremental changes more frequently and reducing the risk of large, disruptive releases.


## !\[Kraken graphic\](./kraken.jpg


)


## Release the Kraken:


We decided to start from core principles and design our own[preview environments](https://speedscale.com/blog/preview-environments/) using Github Actions and Gitlab CI/CD. This release project was appropriately titled, the Kraken. We started applying the concepts and tooling for Preview Environments to three major code repositories:


1.


Documentation - It’s exactly what the name implies.


2.


Dashboard - The front end for our application.


3.


Analyzer Backend Microservice - A sample of one of our API-heavy microservices.


To quickly summarize, the list above is organized from best to worst outcome with high variance between best and worst. Our documentation project had very good ROI and delivered on all promises while adding preview environments to our backend Microservices had limited payback. The question becomes, why do some codebases succeed and others fail with Preview or Ephemeral Environments? We made the following chart to try and quantify our most important observations:


Service Name


Business Logic Complexity


Request Volume


Request Cardinality


Dependencies


Validation Maturity


[Documentation](https://docs.speedscale.com/)


⬇️


➖


⬇️


AWS S3 (file system)


⬇️


Dashboard


➖


⬆️


⬆️


api-gateway (via HTTP/2)


⬆️


Microservice (analyzer)


⬆️


⬆️


⬆️


api-gateway, AWS SQS, AWS S3, kube-api, 3rd party payment service


⬇️


We’ve worked with dozens of companies to build preview environments and the table above is representative of a much larger data set. Some of our findings are intuitive but there are two that took some digging to understand. We started with the intuition that simpler services with less complex logic would be easier to insert into a preview environment. That shouldn’t be a surprise. However, business logic complexity ended up not being a very good indicator of how hard it would be gain value from a preview environment. As we kept digging we actually found there are only a few factors that should drive decision making:


1.


How diverse are the requests going into and out of the application?


2.


How complex are the dependencies (Databases = hard, simple API=easy)?


3.


Are we relying on third party systems like Salesforce or Stripe?


Every other variable in the difficulty equation rounded to zero in comparison to the impact of these factors.


## !\[Process of assembly\](./content-image.jpg


)


## Process and Tool Architecture:


At this point it’s probably helpful to talk briefly about the steps and tools involved in setting up an ephemeral environment. Let’s just run through each piece in plain English rather than getting hung up on specific tools. From our experience, you need some form of the following component:


1.


A plan


2.


CI Action to trigger environment creation on an event like MR submission


3.


Infrastructure provisioner to make sure there is a place to run the preview environment


4.


Application provisioner to make sure the app is running


5.


Provision replicas of key services that service being tested depends on


6.


Populate replicas with realistic data to support preview use cases


7.


Infrastructure de-provisioner


8.


Application de-provisioner


9.


(Optional) Break the CI pipeline if basic validations don’t run


Most of this is self explanatory if you work in platform engineering but learning gotchas involves some exploration.


## Data, data and more data:


The beating heart of Preview Environments is the data. Systems like Kubernetes and CI can help mitigate issues in other areas but there is no shortcut to good data. We started building preview environments back in 2020 and we did so using scripts and CI scripts. There are much better tools available now but it’s still helpful to think of these tools as essentially fancy CI/CD. If you’re doing some very fancy infrastructure work then you might need fancy tools. However, our infrastructure is very modern and so we just used scripts in Github Actions. From our experience, the largest source of effort by far is providing realistic replicas of production data. As an example, this version of our analyzer service depends on AWS S3, AWS SQS, an api-gateway and even the Kubernetes API. We attempted to virtualize each of these services using a combination of service mocks, local file store, test accounts and queuing scripts. At the end, we were rewarded with an analyzer microservice that would start up and run the most basic workload. It proved nothing beyond basic functionality and that our deployments architecture worked. To get realistic validations, we needed realistic inputs and outputs to the service. “Realistic” inputs is hard to define but it includes a high diversity of incoming requests and the correct datastores to satisfy those requests. Just “grabbing some GETs” from production only got us to basic validations. Input requests need to be a complete and relevant statistical sample for validations to be meaningful. For us, this ended up being a set of T-tests and other edge case detection algorithms. Determining whether we had enough inputs was highly application specific and labor intensive. Additionally, there is the question of data freshness. For example, if one services asks for the last 15 minutes of data then those dates need to represent the current time instead of when it was recorded. If you have realistic input, then your datastores must contain the right data to satisfy those requests. Service mocking is the traditional way to solve this problem. Attempting to manually build and maintain this level of service mocks proved impossible for our team size. Based on our customer experience, the problem actually gets worse as the system gets more complex and the team bigger. Building curated sets of input and dependency data was impractical and within a number of months we ended up turning off the microservice preview environments. It’s too hard to get enough heterogeneous data and to mirror it in service mocks for long term use. Going back to research, we read patents and papers from the large tech companies and found that they had also encountered this problem. Given the resourcing available at these large companies they were able to employ many approaches but the one that stood out was environment replication. It was the only approach that solved both the inbound and outbound data problems. A distant second as far as understandability and capability was to attempt to route traffic from production directly to the developer desktop. This solution is wildly impractical in even moderately sensitive data environments. It also puts an enormous load on the Platform Engineering team because of the network configuration and difficulty in providing self service. Imagine PII data being sent unintentionally to developer desktops around the world. It’s certainly possible to execute, but far too labor intensive for our current environment.


## !\[Airplane Tail\](./content-image.jpg


)


## Enter Environment Replication:


Environment Replication differs from simple preview or ephemeral environments in three key ways:


### Data Fidelity:


- **Real Data Replication:** Traditional ephemeral environments stand up the infrastructure but often lack cost-efficient ways to mirror real data. Without real data, the value of a preview environment diminishes. Speedscale excels in replicating production data with perfect fidelity into ephemeral environments, ensuring that tests and validations are based on real, representative data.


### Third-Party System Behavior Replication:


- **Comprehensive API Simulation:** It’s important to replicate the exact behavior of third-party systems, such as AWS or Hubspot. Copying an entire Hubspot SaaS system into a preview environment is impractical, but an environment replication system can simulate this type of third-party API behavior accurately, providing a more comprehensive and realistic testing environment.


### Continuous Updates from Production Data:


- **Real-Time Environment Updates:** Replication continuously updates the ephemeral environments with real production data. This means the environments are always representative of real users interacting with the application, as well as how databases and dependent services react. This constant synchronization with production data ensures the highest fidelity and relevance of the testing environments.


### Data Transformation:


- **Transforms** : Replication must have some semantic understanding of the data in the application and how it needs to modified for replay. Complex scripting is a sign that you are engaged in an anti-pattern. The data transformation system needs to understand the structure of the requests and how they fit together.


These issues are the central challenge when creating just-in-time environments. We were unable to solve these problems with off the shelf tools and a reasonable amount of effort. Your results will vary but we ended up building an entire developer self-service product to solve this problem. It’s called Speedscale.


## Accelerate Developer Productivity


Integrating self-service preview environments into our internal developer platform not only improved developer productivity and efficiency but also finally gave us a way to improve code quality without hiring a testing department. Using this approach, we provide our developers with the most accurate and up-to-date environments, enabling faster, more reliable development cycles and higher-quality code.
