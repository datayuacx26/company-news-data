---
schema_version: "1.0.0"
document_id: "c8dae45a1f458c7ca2f07b2df518d1a7bf223bbc7031dbbc8140830fe81a5b1a"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2024-12-20-life-of-sre-as-a-salesperson/"
published_at: "2025-03-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:019e6c2d4b845c39acbce13e201d31b44065683577fa2cac92777acf3cf79f06"
---

# Life of SRE as a Salesperson

### How I buy (learn) tech from Cloud Platform teams and sell (teach) solutions to Product Engineering teams


## Introduction


If you are a *Developer* or a *Product* person, you might have this feeling of achievement when you work on a specific product. When it’s launched successfully in the market, you see people start using something built by you. You become proud because your hard work results in something that is helping many people to achieve their tasks. This is a rewarding feeling for many of us!


But to go back a little, after working on a product, is it you who is promoting this to the mass users? Sure, you can have a chat with your close friends and say, “Hey, I have worked on this product, it’s so cool. You should totally use it”. Does that reach enough people? Probably not! That’s why there are marketing and sales teams who focus on promoting and selling the product, thus helping you to have that feeling of accomplishment, because a lot of people are now using the product that you built and you are getting valuable user insights to further improve the product.


## Discovering Myself as a Salesperson


Working as a Site Reliability Engineer at trivago, I discovered myself as a Salesperson (or an influencer for Tech Teams), who goes out to help teams adopt trivago best practices by using the tools and solutions we craft together with *Platform Engineering* , *Developer Experience* , and *Observability* teams. My job is to be the bridge between the cloud infrastructure and product development teams. One of our main goals is to maximize the adoption of best practices at trivago.


In this blog post, I will try to share my 4 years of SRE journey compressed into a 12 minutes read. Hold tight!


## Site Reliability Engineering at trivago


Our Head of SRE, Thomas Khalil, once said:


> *Infrastructure should be as easy and boring as electricity.*
> *When we switch it on, the light should come on.*


That means the infrastructure solutions we provide should be boring (easy enough) for the customers, and they should expect resource creation in cloud just like pushing a button.


That also aligns with our SRE mission statement at trivago


> *We empower teams by delivering secure, scalable and predictable cloud-based solutions, so they can focus on solving business problems*


### SRE Structure


We have 6 different SRE pillars/squads at the moment who specialize on specific set of areas.


#### Squads


- **Backend** - Expertise on backend services for product teams
- **Interface** - Expertise on frontend services and mobile apps which are facing internet traffic
- **Data** - Expertise on Big Data and Kafka streaming


#### Pillars


- **Platform** - Takes care of workload schedulers, networking configuration, and baseline infrastructure.
- **Observability** - Expertise on monitoring, logging, and tracing
- **Developer Experience** - Making custom tooling and software to improve experience of our developers.


#### Information flow across teams/squads


The general strategy of work is: all squads will utilize the platform, tools, solutions, and knowledge from the Platform, Observability, and Developer Experience teams. They will then share best practices with the product engineering teams we support.


I work in the Backend Squad. So from this point on, I will focus only on the backend squad. But the work is very similar for Interface and Data squads.


## Day to Day business of SRE Backend Squad


These are the common tasks we do regularly:


- Onboard services into **Cloud**
- Automate and **CI/CD**
- Help maintain **IaC**
- Preach **GitOps** practices
- Set up **SLI** s and **SLO** s
- Make our systems more **reliable**
- Enable teams to be more **independent**
- Participate in voluntary **on-call** rotations


## How do I Work?


At the moment, I’m assigned to a couple of backend teams as a dedicated support person. These teams are “Search & Ranking” and “Marketing Solutions”. I understand the system architecture of the services in both teams and also know what those products are offering to their customers, and these teams are customers of the SRE Backend Squad.


I work with these teams closely to help configure CI/CD workflows in GitHub Actions, Cloud and on-prem resource provisioning using Terraform, Metrics and Alerts instrumentation in Prometheus, visualize business metrics in Grafana dashboards, set up related SLIs and SLOs, optimize workload resource utilization etc.


Assigning an SRE person to a specific team can create dependencies on that person. We aim to solve this by applying common tooling and practices across the teams we support, as well as assigning a primary support person and a secondary support person. If one SRE decides to go on a longer vacation, the product teams will not be affected by this. Because of the similar tooling and infrastructure setup, the secondary SRE or anyone from the team can easily jump in and help out.


In addition to the support, we continuously share knowledge with the product teams and do regular demo sessions showing how reusable solutions work. Since the teams are getting more and more educated on cloud infrastructure and are becoming more self-sufficient, we are receiving less and less support requests. This means they rely less on SREs and can focus more on writing business features.


## What do I Sell?


The solutions I sell to product teams are coming mostly from Platform, Developer Experience, and Observability pillars. Here are some of the solutions which aim for easy configuration management and increases team productivity:


### Application packaging with Helm Chart


Kubernetes is our main orchestration tool at trivago. In order to package an application, we often need common resource types, including Deployment, Service, HorizontalPodAutoscaler, ConfigMap, ServiceMonitor, PrometheusRule, GrafanaDashboard etc. In a world of micro-service patterns where we are running hundreds of micro-services, writing similar Kubernetes resource definition again and again is cumbersome and error prone. Instead we aim for providing a common Helm chart that provides sane defaults with customizability and teams can adjust certain parameters based on the service needs.


### Reusable release workflow


When it comes to releasing a set of changes, we often need to build the application, push application artifacts to a registry, and synchronize application manifests from Helm Charts to ArgoCD. Since we have thousands of GitHub repositories, writing similar set of GitHub Actions workflows every time is very tedious. Instead we use a shared workflow that serves the main purpose, and again is offering customization options to the teams based on service needs.


### trivago standard Terraform modules


We create cloud resources in a fashion that is easier to maintain and get predictable output. This is why we have many shared Terraform modules which create and configure a set of cloud resources to achieve specific tasks.


### Dependency update automations


We use[Renovatebot](https://github.com/renovatebot/renovate) to update packages from many different frameworks and runtimes. There are common configurations we provide to schedule and auto update dependencies, which is again customizable. In recent times, we enabled onboarding of renovate configuration for any internal GitHub repo by simply adding a specific topic in the repo settings.


### Centralized Monitoring Stack


Since we have many microservices, to construct proper business metrics, we need to connect telemetry data from all of them. This is why we have a central place where all telemetry data can be shown to achieve proper business metrics and set up SLIs and SLOs accordingly. That being said, with the help of Application Packaging Chart that I described above, we expose application metrics automatically on certain endpoints. Then the centralized monitoring setup collects all of those and shows them in one single place. That way, developers don’t have to care which part of application runs on which Kubernetes cluster. All metrics are connected and can be browsed in a single pane.


### Other PR helpers for common tasks


To achieve smoother releases we have more guard-rails like code linting, running tests, check compliance in certain places before making a code merge. We have shared GitHub Actions which can be used to achieve those tasks.


… and much more.


## How do I Sell?


Reliability is the number one feature of any software. If a product/service is not reliable enough, chances are we are not going to have enough customers for that service in the long run, no matter how hard we are promoting that service.


Inside the SRE team, we also work hard to make a reliable and robust toolchain, by aiming to provide a *Golden Path* with our service offering. The *Golden Path* is an opinionated and supported set of tools and processes that help product teams deliver software in a fast and efficient manner. These tools and processes are tried and battle tested at trivago and are guaranteed to be reliable.


We also provide customizability of our tools so that teams are in control of their service configuration. Since great power comes with great responsibility, more customization may lead to misconfiguration in many places. This is why we also put guard-rails in our toolchain with sane defaults so it becomes harder to go wrong with customizability.


When we have reliable software offerings, it becomes easier to convince product teams to adopt them. We kickoff with a demo session for the product teams to demonstrate how our recommended solutions work. Then we let the teams evaluate the solutions for a limited time before we continue with the complete adoption.


We also do regular follow-up sessions with the teams to ensure we are getting feedback on how well the solution is working for them. After getting feedback, we try to improve or add new features to our existing tools based on the teams’ requirements. This enables rapid development for all the teams at trivago.
If a team asks for a new feature in our tool, and we implement that, all the other teams can benefit from the same feature that was built once. So we really aim for the[DRY](https://en.wikipedia.org/wiki/Don't_repeat_yourself) principle.


## Where do I Make a Profit?


I became an SRE back in October 2020 when I joined trivago. Before that, I was working as a Backend Engineer for around 7 years. Having experience both in Backend Engineering and in Site Reliability Engineering, I noticed a common pattern across software developers: *Developers LOVE infrastructure magic!* Whenever infrastructure people work on “cool” stuff, developers are usually “wow”ed by this cool stuff. By “cool work” I mean all sorts of automations when one action triggers another predictable action without any manual steps in between.


Since developers love infrastructure magic, I started teaching them infrastructure magic! And that is how I do business. When developers learn how to wield this magic, it gives them a sense of accomplishment. And that’s where shared responsibility comes in. During an emergency situation, developers don’t have to wait for SREs to take actions. They can use the ”magic” they learned and make their application work again. By sharing knowledge and responsibility, we are removing silos as much as possible. So, the risk of getting burned out from doing[too much DevOps work](https://devops.com/the-great-devops-burnout/) for us is very minimal.


Another interesting point is: when product teams are self-sufficient, we (SRE Backend Squad) can also jump in with Platform & Developer Experience topics and work with other SREs. This enables us to develop ourselves in other infrastructure-related areas. We work with other SRE pillars on some heavy lifting tasks, for example, making a golden path for the CI/CD pipeline, helping them by giving input on proposed solutions and making contributions to the code.


### Inner Source and Open Source Contributions


The source code of the solutions and tools we provide are available and visible to the whole organization. Any developer from any team can see and deep dive into the source code. Not only do we encourage developers to read the code and the documentation we write, we also welcome them to contribute to the software they use. We are seeing increased contributions in our application package Helm charts, reusable terraform modules, CI/CD golden paths from all SREs in the company, not just from Platform and Developer Experience pillars. We even see enthusiastic developers from product teams contributing to these codebases as well. This kind of collaboration really improves the overall tech culture at trivago.


A lot of tooling and software that we provide are based on open-source projects. While working with open-source software, if we see a specific need which is not covered yet or needs improvements or customizations, we then often contribute to those open-source projects. And let’s be honest, contributing to open-source projects gives us engineers a good sense of achievement, knowing that we are not only using the software but we are also giving something back to the community.


## Communication is the Key!


While we are approaching the final part of this blog post, I want to emphasize the importance of communication between teams. We usually attend product teams’ scrum ceremonies and OKR plannings to stay up-to-date about the releases and upcoming changes. If there’s a larger change planned that teams need support with, we relay the information back to the bigger SRE group. We do meetings with all SREs on a fortnightly basis where we discuss challenges we are facing but also showcase exciting new technology.


Over time, we gain more confidence about the solutions we offer, so selling those ideas to the teams becomes easier for me. As we reflect over the SRE structure, we are happy with the outcomes and many learnings from the past couple of years. It has been quite Stable, Reliable, and Enjoyable!


---


Thank you for reading my journey of becoming a Tech Influencer at trivago.
We are always looking for more salespersons to increase our SRE business.
Keep an eye on our career page for new openings!
