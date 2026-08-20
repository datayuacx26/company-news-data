---
schema_version: "1.0.0"
document_id: "56bb5304922056cc0a17c88a277a9b5494385fa346379e3827bc06e036245724"
company_key: "yc-fampay"
company: "FamPay"
source_id: "yc-fampay-rss-25fb68ce5f72"
canonical_url: "https://blog.famapp.in/blog/the-story-behind-fampays-tech-stack/"
published_at: "2025-11-29T18:30:00+00:00"
first_seen_at: "2026-07-20T23:23:51.041833+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:ea98bc38b6dce62902a74a5b61c22f045918092dcffbb694cb9411107d068e22"
---

# The Story Behind Fam's Tech Stack🚀

It would be an understatement to say that *your tech stack makes or breaks the project.* Not only does a tech stack determine the kind of app that the team will be able to build in the end, but it also directly impacts the efficiency with which the developers build it. Thus, it was imperative for the core team at Fam to curate an effective yet dynamic tech stack.


When we started building Fam, the core team had experience in developing backends with Django and Android apps. We chose what we were most comfortable with and knew it was a scalable framework having been used by Instagram (until ~2015), Pinterest, Disqus, and Creative Commons.


# **Languages**


[GoLang](https://golang.org/?ref=blog.famapp.in) - Most of our backend code is in Go. Go is an open source programming language that makes it easy to build simple, reliable, and efficient software. We use[Gofiber](https://gofiber.io/?ref=blog.famapp.in) and net/http as our Go web framework for HTTP server implementations. For micro-services communication, we leverage[gRPC](https://grpc.io/?ref=blog.famapp.in) with Google's gRPC libraries for efficient, language-agnostic communication between services. Our Go projects follow a[hexagonal architecture](https://alistair.cockburn.us/hexagonal-architecture?ref=blog.famapp.in) , enabling modular design and easy evolution of services over time.


[Python](https://python.org/?ref=blog.famapp.in) - Python's design philosophy emphasises code readability with its notable use of significant indentation. Additionally, we use static code checking tools like[flake8](https://pypi.org/project/flake8/?ref=blog.famapp.in) ,[black](https://pypi.org/project/black/?ref=blog.famapp.in) ,[isort](https://pypi.org/project/isort/?ref=blog.famapp.in) to have consistent patterns. Now you can even code in notepad and still, that won’t break PEP8 guidelines xD.


**Java -** Mostly used by Android App, Java is a High Level, class-based, object-oriented programming language that is designed to have as few implementation dependencies as possible.


[Kotlin](https://kotlinlang.org/?ref=blog.famapp.in) - Kotlin is a cross-platform, statically typed, general-purpose programming language with type inference. Along with Java, most of our Android app is now written in Kotlin.


[Swift](https://swift.org/about/?ref=blog.famapp.in) - Swift is a general-purpose, multi-paradigm, compiled programming language developed by Apple Inc. and the open-source community.


**Dart -** Dart is a modern, client-optimized programming language developed by Google. Dart offers a familiar C-style syntax, sound type safety, and a rich standard library that makes development efficient and predictable. Its asynchronous programming model with` async` /` await` and support for isolates enables smooth concurrency handling, making it a versatile choice for both frontend and backend development using the Flutter framework.


[TypeScript](https://www.typescriptlang.org/?ref=blog.famapp.in) - TypeScript is an open-source language developed by Microsoft that builds on JavaScript with static typing and modern ES features. It compiles to plain JavaScript, ensuring full compatibility across browsers and Node.js. We use TypeScript across both frontend (React, Next.js) and backend (Node.js) projects for its type safety, better tooling, and maintainable, predictable codebases.


# **Frontend**


### **⚡️ Website**


We use[Next.js](https://nextjs.org/?ref=blog.famapp.in) to power our frontend applications. Next.js offers built-in server-side rendering (SSR), static site generation (SSG), and automatic code splitting, enabling fast, scalable, and SEO-friendly web experiences. Our frontend is deployed on[Cloudflare Pages](https://pages.cloudflare.com/?ref=blog.famapp.in) , leveraging its globally distributed edge network, automatic builds, and seamless CI/CD integration for reliable and lightning-fast performance.


### 🤖 **Android**


Fam’s Android app started as a native Android app in 2019 with majority of it built with Android specific frameworks using Java/Kotlin. The majority of views were written in XML, and there’s been an active effort to shift towards Jetpack Compose and hybrid technologies.


The app is built using MVP/MVVM architecture relying on popular libraries like[OkHttp](https://github.com/square/okhttp?ref=blog.famapp.in) ,[Retrofit](https://github.com/square/retrofit?ref=blog.famapp.in) ,[Glide](https://github.com/bumptech/glide?ref=blog.famapp.in) ,[EventBus](https://github.com/greenrobot/EventBus?ref=blog.famapp.in) ,[Chucker](https://github.com/ChuckerTeam/chucker?ref=blog.famapp.in) ,[RxJava](https://github.com/ReactiveX/RxJava?ref=blog.famapp.in) , & Kotlin Coroutines for its functioning.


Overtime, as the requirements grew more complex, we wanted to explore modern day hybrid solutions like **Flutter** , **Kotlin Multiplatform** & **PWAs** in order to deliver at a faster pace and maintain unified a codebase across our apps. Currently, our mobile apps utilize KMP for some of their core business logic handling and the plan is to further expand our dependence to KMP layer for more platform-level usecases.


Today our apps are structured in a way that we can deliver full-fledged features either in native Android or as a PWA or using Flutter. This architecture, today grants us the power of choice to deliver any feature using the framework of our choice.


### 🍎 **iOS**


Fam’s iOS app began its journey in 2019, built natively using **Swift** and **SwiftUI** . The app architecture primarily relies on the **Reactor pattern** to manage state and business logic, along with the **Coordinator pattern** to handle navigation and flow between screens.


As the product and its requirements evolved, our team started exploring cross-platform technologies to accelerate development and achieve greater code sharing across platforms. This led to the adoption of Flutter, which we integrated into our native iOS application using the Flutter Engine. This hybrid setup allows us to seamlessly embed Flutter-based views and features alongside our native SwiftUI components. Over time, we’ve continued to enhance this integration, giving our iOS app the flexibility to deliver complete features either natively in SwiftUI or via Flutter — depending on what best fits product requirements.


Today, this architecture empowers us with the same “power of choice” across iOS as on Android — the ability to develop and deploy features using the most suitable framework for each use case, whether it’s SwiftUI or Flutter.


Some of the libraries that we depend on include:[Reactor](https://github.com/vmanot/Reactor?ref=blog.famapp.in) ,[SwiiftUIX](https://github.com/SwiftUIX/SwiftUIX?ref=blog.famapp.in) ,[Merge](https://github.com/vmanot/Merge?ref=blog.famapp.in) &[Swallow](https://github.com/vmanot/Swallow?ref=blog.famapp.in)


###
🧶 **Web Apps / PWA**


Our **Progressive Web App (**[PWA](https://web.dev/progressive-web-apps/?ref=blog.famapp.in) **)** — is the primary web layer of Fam, built with **React** , **TypeScript** , **HTML** , and **CSS** . It powers multiple sections of the app organised through modular routes for rapid iteration.


Designed for **fast development and deployment** , the PWA runs seamlessly as a **web view inside both Android and iOS apps** , combining the agility of the web with the reach of native platforms. This flexibility allows us to **roll out updates instantly** , significantly accelerating gamified feature delivery across the Fam ecosystem.


Deployed on **Cloudflare Pages** , it benefits from **global edge delivery** , **automatic builds** , and **reliable CI/CD pipelines** , ensuring a smooth, scalable, and high-performance user experience.


### 🧩 **Kotlin Multiplatform**


As Fam evolved, maintaining shared logic between Android, iOS apps became critical. Kotlin Multiplatform was adopted to unify our core business logic and reduce redundancy across platforms. Its adoption began with a specific feature where it encapsulates critical operations such as **data handling** , **networking** , **caching** , and **business domain logic** , and integrated seamlessly between Android and iOS apps. It relies on libraries like[Ktor](https://github.com/ktorio/ktor?ref=blog.famapp.in) for network operations,[Kotlinx Serialization](https://github.com/Kotlin/kotlinx.serialization?ref=blog.famapp.in) for data parsing & others.


Going forward, our goal is to expand the KMP layer to include more platform-level use cases, enabling even greater code reuse, consistency, and development velocity across the Fam ecosystem.


### 🐦 **Flutter**


Fam’s Flutter journey began as part of our effort to accelerate feature delivery and maintain a consistent user experience across platforms. Flutter was introduced to power select modules within the app that required rapid iteration, pixel-perfect UI, and cross-platform parity. Currently it powers important flows like entire KYC flow, Home Screen Tabs & others.


We currently maintain a single codebase for all Flutter flows with parts running inside our native Android and iOS apps using separate flutter engines. We are managing multiple flutter engines inside our apps using **flutter engine cache** and[engine grouping](https://docs.flutter.dev/add-to-app/multiple-flutters?ref=blog.famapp.in) .


Our flutter codebase leverages popular libraries and frameworks such as[get](https://pub.dev/packages/get?ref=blog.famapp.in) for state management & dependency injection,[dio](https://pub.dev/packages/dio?ref=blog.famapp.in) for networking,[cached network image](https://pub.dev/packages/cached_network_image?ref=blog.famapp.in) &[flutter svg](https://pub.dev/packages/flutter_svg?ref=blog.famapp.in) for image loading.


# **Backend**


## Frameworks


### 🥃 **GoFiber & net/http (Go)**


Our backend services use both GoFiber and net/http, depending on the service requirements. Some services use GoFiber for its fast routing, lightweight API, and built-in middleware support, while others use Go’s standard net/http package for its simplicity, robustness, and direct integration with the Go ecosystem. This flexibility allows us to choose the right framework for each service’s performance and architectural needs.


### **⚙️ Asynq (Go)**


[Asynq](https://github.com/hibiken/asynq?ref=blog.famapp.in) is a simple, reliable, and efficient background task queue library built in Go. It enables distributed processing of asynchronous jobs with support for both real-time execution and scheduled task management. Asynq uses Valkey as its message broker, offering low-latency task dispatching and strong reliability guarantees.


We use Asynq to offload long-running or non-critical workloads — such as notifications, data syncs, and background processing — from our core request path, improving response times and system scalability. We deploy these workers on Kubernetes and with appropriate horizontal pod autoscaler (HPA) configurations our workers scale horizontally based on the backlog of tasks pending


### 🔮 **gRPC**


We use[gRPC](https://grpc.io/?ref=blog.famapp.in) for high-performance, language-agnostic communication between microservices. Built on top of HTTP/2, gRPC enables efficient request multiplexing, bidirectional streaming, and strongly typed interfaces defined through Protocol Buffers. This allows our services to communicate with low latency and minimal overhead while maintaining clear contracts and backward compatibility across service boundaries.


### 🛠️ **Temporal**


We use[Temporal](https://temporal.io/?ref=blog.famapp.in) for reliable task orchestration across distributed systems. Temporal allows us to define workflows as code, providing built-in reliability, state management, and automatic retries. This ensures that multi-step and long-running operations execute exactly once, even in the face of failures. By leveraging Temporal, we achieve guaranteed execution, simplified error handling, and greater resilience across our services.


### 🗿 **Django + Django Rest Framework**


Our first monolithic services were built in **Python** using **Django** and **Django REST Framework (DRF)** . Django is a high-level web framework that promotes rapid development and clean, pragmatic design — it handles much of the heavy lifting so developers can focus on business logic instead of boilerplate. DRF, on the other hand, made it easy to build and serve complex JSON-based REST APIs for our frontend applications.


Most of our early user-facing services were written in Django, but as our scale grew, we began transitioning toward a **microservices architecture** — gradually decomposing the monolith into smaller, domain-focused services written in **Go** , each optimized for its specific responsibility.


We’re continuing this migration journey and will soon share our learnings from moving out of the monolith — stay tuned for that upcoming blog!


### **📜 Celery**


[Celery](https://docs.celeryproject.org/en/stable/?ref=blog.famapp.in) is a simple, flexible, and reliable distributed system to process vast amounts of messages while providing operations with the tools required to maintain such a system. It’s a task queue with a focus on real-time processing, while also supporting task scheduling. Celery workers are also deployed on Kubernetes and with appropriate horizontal pod autoscaler (HPA) configurations our workers scale horizontally based on the backlog of tasks pending.


# **Databases**


### 🦣 **PostgreSQL**


We have highly relational data so choosing RDBMS was an obvious choice, Django has really good ORM support for Postgres as well. We heavily rely on its transactional properties. We are using[PostgreSQL](https://www.postgresql.org/?ref=blog.famapp.in) to store most of the data. It's deployed via AWS RDS - Postgres managed service.


### 🫀 **Valkey**


You may have heard of it as the in-memory key-value store, but Valkey is truly the Swiss Army knife of NoSQL databases.[Valkey](https://valkey.io/?ref=blog.famapp.in) provides a rich set of data structures — including strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes, and streams — making it highly versatile for a wide range of use cases. It supports built-in replication, Lua scripting, LRU eviction, transactions, and multiple levels of on-disk persistence, while offering high availability through Sentinel and automatic partitioning via Cluster mode. We run Valkey in cluster mode using AWS ElastiCache, ensuring scalability, fault tolerance, and low-latency access across services.


### **🤖** **DynamoDB**


We use[Amazon DynamoDB](https://aws.amazon.com/dynamodb/?ref=blog.famapp.in) as our NoSQL database for applications that require single-digit millisecond latency at any scale. It’s a key-value and document database that delivers high performance with seamless scalability. DynamoDB enables **Z-axis scaling** , allowing us to horizontally partition data across multiple shards to handle massive workloads efficiently while maintaining low latency and high availability.


### 💡 **Clickhouse**


We use[ClickHouse](https://clickhouse.com/?ref=blog.famapp.in) as our high-performance analytical database across multiple domains. It powers our **data warehousing** workloads, supports our **fraud and risk management engine** for near real-time analysis, and stores **HTTP access logs** for request-level analytics and observability. Being **self-hosted** , our ClickHouse deployments give us fine-grained control over performance tuning, data retention, and resource allocation, enabling efficient large-scale data processing while maintaining operational flexibility.


# **Queues & Messaging**


We use multiple queues and messaging platforms to support service decoupling, scalability, and real-time event processing:


**🛖 MSK (Managed Kafka)**
A managed Kafka service that powers our distributed messaging and high-throughput event pipelines. We use MSK extensively for event-driven communication between microservices.


**🚡 WAL Queues**
We use Write-Ahead Log (WAL) queues in Postgres to guarantee reliable and exactly-once message delivery between services. Our open-source project,[Factlib](https://github.com/fampay-inc/factlib?ref=blog.famapp.in) , provides a structured and fault-tolerant framework for producing events to Kafka using WAL semantics. It ensures that messages are durably persisted before being dispatched, enabling recovery and replay in case of service or network failures, and maintaining end-to-end delivery guarantees across our event-driven systems.


**🧀 SNS**
Used for push-based notifications and alerting. It integrates with CloudWatch to send alerts to our internal alerting service, which escalates issues to the on-call team for rapid response.


# **Analytics**


###
**🥷 Metabase**


[Metabase](https://www.metabase.com/?ref=blog.famapp.in) is the fastest, easiest way to share data and analytics inside your company. We can connect many DB sources and it creates beautiful graphs for us. It makes it easy for non-technical users to ask questions about data. Using its alert feature we get notified if something is wrong with the data or application. Like successful transactions rate or any failure with external services.


### **🤠 Mixpanel**


[MixPanel](https://mixpanel.com/?ref=blog.famapp.in) is a great product analytics tool to understand user behaviors, Segment integration with Mixpanel pushes data there.


### **🎯 CleverTap**


[CleverTap](https://clevertap.com/?ref=blog.famapp.in) helps us craft contextual and personalized campaigns to engage our users through Push Notifications, In-app Messages, SMS, Web Push, Emails, WhatsApp, Facebook. It provides comprehensive campaign management and user journey analytics.


### **🎩 Data Warehouse**


We operate multiple microservices, each with its own independent database. To enable centralized analytics and reporting, we built a data pipeline that continuously ingests data from these individual databases into our ClickHouse-powered data warehouse. We use a self-hosted deployment of the open-source project[PeerDB](https://peerdb.io/?ref=blog.famapp.in) to capture Change Data Capture (CDC) events from all databases and stream them into ClickHouse in near real time. Internally, PeerDB reads Write-Ahead Logs (WAL) from source databases, stores the data in Amazon S3, and then ingests it into ClickHouse. The entire architecture is orchestrated using Temporal, ensuring reliability and fault tolerance throughout the pipeline.


The entire system is **self-hosted** , giving us full control over performance, scalability, and data governance.


We’ll be publishing a detailed blog soon on how our data pipeline has evolved — from initial prototypes to a fully self-hosted, scalable architecture.


# **Automation**


### 🤖 **N8N**


[n8n](https://n8n.io/?ref=blog.famapp.in) is a fair-code licensed extendable workflow automation tool, you can automate all kinds of the mundane processes using this as we live in API world you can connect pretty much anything. For example, we use it to **automate push notifications** to customers when they **win rewards** or when there’s a **special campaign** , such as a **Diwali sale** .


### **🛠️ Appsmith**


[Appsmith](https://www.appsmith.com/?ref=blog.famapp.in) is our open-source alternative for building internal tools and admin panels. We use this for specific features that are not available in Metabase. We can set up webhooks here which helps in automation. This is also used internally by the customer support teams to make changes to user data as per customer’s request.


# Observability


### 🎩 **Sentry**


[Sentry](https://sentry.io/?ref=blog.famapp.in) helps us catch and fix those elusive, once-in-a-blue-moon bugs. It’s an application monitoring and error-tracking platform that enables developers to diagnose, fix, and optimize application performance with deep context. We use a self-hosted deployment of Sentry running on Kubernetes, giving us complete control over data and integrations, though their SaaS offering is also available. In addition to error tracking, we leverage Sentry for distributed tracing, allowing us to monitor request flows across services and identify performance bottlenecks in real time.


### **📊 Grafana & VictoriaMetrics**


For metrics and observability,[VictoriaMetrics](https://victoriametrics.com/?ref=blog.famapp.in) as our high-performance, Prometheus compatible time series database, and[Grafana](https://grafana.com/?ref=blog.famapp.in) as our visualization layer. This stack allows us to monitor infrastructure health, visualise trends, and set up alerts for critical events.


### 📔 **Logs**


We use[Loki](https://grafana.com/oss/loki/?ref=blog.famapp.in) and Amazon[CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html?ref=blog.famapp.in) for centralised log collection, viewing, and filtering across our services.


In addition, we ingest HTTP access logs from Cloudflare and various other providers into ClickHouse for analytical querying and long-term storage. This allows us to efficiently analyse traffic patterns, identify anomalies, and derive insights from large volumes of request data while keeping operational logging lightweight and real-time. This Clickhouse is self-hosted on an EC2. Checkout[Logverse](https://github.com/fampay-inc/logverse?ref=blog.famapp.in) which enables us to ingest these logs into Clickhouse in a reliable way.


# Infrastructure & Platform


### **💇🏼‍♀️ Github Actions**


We use GitHub Actions to automate our CI/CD workflows, including build, test, and deployment pipelines. It enables consistent and reproducible automation across projects, helping us maintain high development velocity. Our[Forgejo](https://forgejo.org/?ref=blog.famapp.in) setup is fully compatible with GitHub Actions, allowing us to run self-hosted runners and leverage the same workflow syntax for seamless integration between code hosting and continuous delivery.


### **🐳 Docker**


[Docker](https://www.docker.com/?ref=blog.famapp.in) takes away repetitive, mundane configuration tasks and is used throughout the development lifecycle for fast, easy, and portable application development. Most of our codebase is containerized, allowing it to run anywhere.


### 🚟 **AWS ECR**


[Amazon Elastic Container Registry](https://aws.amazon.com/ecr/?ref=blog.famapp.in) (ECR) is a fully managed container registry that makes it easy to store, manage, share, and deploy our container images and artifacts anywhere. Since we use Docker, we needed a place to store private images so what could be better than ECR which tightly integrates with other AWS services.


### ☸️ **Kubernetes**


[Kubernetes](https://kubernetes.io/?ref=blog.famapp.in) is our choice for orchestrating containerized workloads and services. With its declarative configuration and automation capabilities, Kubernetes simplifies deployment, scaling, and management of our microservices architecture. We utilize features such as namespaces, config maps, secrets, and horizontal pod autoscaling to ensure resilience and operational efficiency. Our environments run on managed Kubernetes services for easier upgrades, monitoring, and integration with AWS resources.


### 🪃 **Amazon Elastic Beanstalk**


[AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/?ref=blog.famapp.in) is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker on familiar servers such as Apache, Nginx, Passenger, and IIS. All of our core services are containerised and deployed using Beanstalk.


### 🦉 **Nginx**


[Nginx](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/?ref=blog.famapp.in) is a high-performance web server that can also be used as a reverse proxy, load balancer, mail proxy, and HTTP cache. We mainly use it as a reverse proxy in front of open source projects, next server, python WSGI server. It handles SSL and sending responses to the clients.


### 🪙 **Gunicorn**


Gunicorn 'Green Unicorn' is a Python WSGI HTTP Server for UNIX. It's a pre-fork worker model. We observed that most of our traffic is IO bound so we changed its worker pool to gevent and monkey patched psycopg.


### 🌐 **Envoy Gateway**


[Envoy Gateway](https://www.envoyproxy.io/?ref=blog.famapp.in) serves as our modern reverse proxy and load balancer, simplifying the deployment and management of microservices. It plays a key role in setting up isolated **development environments** , enabling each feature branch to be independently tested by developers and QA teams as part of our **Git Flow** process. Envoy also performs **authentication and authorisation checks** at the initial stage of the request lifecycle, ensuring only validated requests reach downstream services. We also are able to maintain route and user level rate limiting so that our APIs don’t endure abuse from attackers.


### ☁️ **Cloudflare**


We use **Cloudflare** as our primary DNS management and security platform, leveraging its built-in **WAF** and **DDoS protection** to safeguard our infrastructure. Several Cloudflare products play a key role in our stack:


- **Cloudflare Pages:** Hosts our React-based web applications and websites. It removes the overhead of managing deployments while providing built-in CI/CD pipelines for fast and reliable releases.
- **Cloudflare R2:** Serves as our CDN-backed object storage for media assets — including button images, website visuals, and other static content.
- **Cloudflare DNS:** Handles all DNS management and routing for our domains.
- **Cloudflare WAF & DDoS Protection:** Monitors traffic patterns, blocks malicious activity, and provides detailed security analytics and reports.


### 🍀 **Pulumi**


[Pulumi](https://www.pulumi.com/?ref=blog.famapp.in) helps adopt infrastructure as code automation, combined with tried and true software engineering practices. One of the projects we are working on is to create a data pipeline and data warehouse. We are leveraging this to create and destroy AWS infrastructure.


### 👋 **k6**


[k6](https://k6.io/?ref=blog.famapp.in) is our modern load testing tool that sends load to web applications and provides comprehensive performance metrics and latency distribution summaries. It's developer-centric and provides excellent insights into application performance.


# **Deployment**


Our deployment workflow is built around a common foundation — we build Docker images, push them to **Amazon ECR** , and then deploy them across environments using simple, automated scripts.


For AWS Elastic Beanstalk applications, deployments are executed via the AWS CLI, while for Kubernetes-based services, we trigger ArgoCD to handle the rollout and synchronization. Both processes are automated and abstracted behind a web interface powered by[Script Server](https://github.com/bugy/script-server?ref=blog.famapp.in) , allowing developers to trigger deployments for their respective services with a single click.


This setup provides a unified, low-friction deployment experience across environments, while maintaining safety, consistency, and operational control.


# OSS


### 👻 **Ghost**


[Ghost](https://ghost.org/?ref=blog.famapp.in) is designed for ambitious, professional publishers who want to actively build a business around their content. We use it for publishing blogs and sending newsletters to subscribers. There are built-in XML sitemaps, Google AMP pages, canonical tags, optimised URLs, Facebook Open Graph tags, support for Twitter cards and clean semantic markup for better SEO. Ghost is really fast, you can get >95% score on google page speed easily. We have self-hosted this on EC2.


### 👾 **OAuth2-Proxy**


A reverse proxy and static file server that provides authentication using Providers (Google, GitHub, and others) to validate accounts by email, domain, or group.[OAuth2](https://github.com/oauth2-proxy/oauth2-proxy?ref=blog.famapp.in) proxy adds an authentication layer in between for projects like ListMonk and N8N


### 📮 **Postal**


We use[Postal](https://github.com/postalserver/postal?ref=blog.famapp.in) as our self-hosted mail server for sending and managing transactional and system emails. Postal provides full control over email delivery, routing, tracking, and bounce handling, allowing us to maintain high deliverability without relying on third-party email providers. By self-hosting Postal, we gain flexibility in configuration, better cost efficiency, and the ability to tightly integrate email workflows with our internal systems.


### **🦅 Netbird**


We use[NetBird](https://netbird.io/?ref=blog.famapp.in) as our self-hosted, zero-trust networking solution to securely connect infrastructure and services across environments. NetBird simplifies private network management by creating an encrypted peer-to-peer mesh overlay using WireGuard, enabling seamless and secure access between cloud resources, on-prem systems, and developer machines without relying on traditional VPN setups. This setup enhances security, reduces operational overhead, and provides fine-grained access control through identity-based policies.


### **🧊 SonarQube**


We use[SonarQube](https://www.sonarqube.org/?ref=blog.famapp.in) as our self-hosted platform for continuous code quality and security analysis. It helps us maintain clean, reliable, and maintainable code by automatically scanning repositories for bugs, vulnerabilities, and code smells during the CI/CD process. SonarQube enforces consistent coding standards across teams, provides detailed quality metrics, and integrates seamlessly with our development workflow to ensure that code meets our quality gates before being deployed.


### **📓 Forgejo**


We use[Forgejo](https://forgejo.org/?ref=blog.famapp.in) as our self-hosted Git service for managing source code, pull requests, and CI/CD pipelines. Forgejo provides a lightweight, reliable, and privacy-focused alternative to cloud-based Git platforms, giving us full control over our repositories and development workflows. It integrates seamlessly with our automation tools and internal systems, enabling secure collaboration, efficient code reviews, and streamlined deployment processes across teams.


## Conclusion


There are some key takeaways that the core team has realized through the journey:


- Choose a tech stack that you are most comfortable with.
- Picking the right tool for the right job is equally important.
- If you are facing a problem there is a good chance that an open-source solution exists for it.
- Go with the solution that works now and easily modifiable when required. We like to keep things simple.
- Automate business processes and repeating workflows.
- Consult with an expert when in doubt.


Keeping these things in mind will help you lay a solid and thoughtful foundation when you begin developing a project. It only makes things easier if critical thinking and decision-making have been done beforehand when choosing from the plethora of options available. However, in the end, experimentation and dynamism is key to arranging your tech stack.


*Found this article interesting and want to share your insights? Drop in an email at*[\[email protected\]](https://blog.famapp.in/cdn-cgi/l/email-protection#a6d2c3c5cee6c0c7cbd6c7df88cfc8) *!*


### Tags


- [Tech](https://blog.famapp.in/blog/tag/tech/)
- [FamSays](https://blog.famapp.in/blog/tag/famsays/)
