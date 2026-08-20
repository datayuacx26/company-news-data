---
schema_version: "1.0.0"
document_id: "1f50c596f5180eb2c9271830fa4cbea2cb01baecd7676c7fcc2d1cb3a2cf9ac0"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-news-import-53e8505a9d97"
canonical_url: "https://about.gitlab.com/blog/how-indeed-transformed-its-ci-platform-with-gitlab/"
published_at: "2024-08-27T00:00:00+00:00"
first_seen_at: "2026-07-25T06:37:24.143169+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:7f789893a149ffe5e938cb35a23b132f0aad7dd5bd9caf7bbdab3499c611b32e"
---

# How Indeed transformed its CI platform with GitLab

***Editor's note: From time to time, we invite members of our customer community to contribute to the GitLab Blog. Thanks to Carl Myers, Manager of CI Platforms at Indeed, for sharing your experience with GitLab.***


Here at Indeed, our mission is to help people get jobs. Indeed is the[#1 job site](https://www.indeed.com/about?isid=press_us&ikw=press_us_press%2Freleases%2Faward-winning-actress-viola-davis-to-keynote-indeed-futureworks-2023_textlink_https%3A%2F%2Fwww.indeed.com%2Fabout) in the world with more than 350 million unique visitors every month.


For Indeed's Engineering Platform teams, we have a slightly different motto: "We help people to help people get jobs." As part of a data-driven engineering culture that has spent the better part of two decades always putting the job seeker first, we are responsible for building the tools that not only make this possible, but empower engineers to deliver positive outcomes to job seekers every day.


GitLab Continuous Integration has allowed Indeed’s CI Platform team of just 11 people to effectively support thousands of users across the company. Other benefits Indeed has realized by moving to GitLab CI include:


- 79% increase in daily pipelines
- 10-20% lower CI hardware costs
- Decreased support burden


## Evolving our CI platform: From Jenkins to a scalable solution


Like many large technology companies, we built our CI platform organically as the company scaled, using the de facto open source and industry standard solutions available at the time. Back in 2007, when Indeed had fewer than 20 engineers, we were using Hudson, Jenkins’ direct predecessor.


Today, through nearly two decades of growth, we have thousands of engineers. As new technology became available, we made incremental improvements, switching to Jenkins around 2011. Another improvement allowed us to move most of our workloads to dynamic cloud worker nodes using[AWS EC2](https://aws.amazon.com/ec2/) . As we entered the Kubernetes age, however, the system architecture reached its limits.


Jenkins’ architecture was not created with the cloud in mind. Jenkins operates by having a "controller" node, a single point of failure that runs critical parts of a pipeline and farms out certain steps to worker nodes (which can scale horizontally to some extent). Controllers are also a manual scaling axis.


If you have too many jobs to fit on one controller, you must partition your jobs across controllers manually. CloudBees offers ways to mitigate this, including the CloudBees Jenkins Operations Center, which allows you to manage your constellation of controllers from a single centralized place. However, controllers remain challenging to run in a Kubernetes environment because each controller is a fragile single point of failure. Activities like node rollouts or hardware failures cause downtime.


In addition to the technical limitations baked into Jenkins itself, our CI platform also had several problems of our own making. For example, we used the Groovy Jenkins DSL to generate jobs from code in each repository. This led to each project having its own copy-pasted job pipeline, resulting in hundreds of versions that were hard to maintain and update. While Indeed’s engineering culture values flexibility and allows teams to operate in separate repositories, this flexibility became a burden as teams spent too much time addressing regular maintenance requests.


Recognizing our technical debt, we turned to the[Golden Path pattern](https://tag-app-delivery.cncf.io/whitepapers/platforms/) , which allows flexibility while providing a default route to simplify updates and encourage consistent practices across projects.


The CI Platform team at Indeed is not very large. Our team of around 11 engineers supports thousands of users, fielding support requests, performing upgrades and maintenance, and enabling always-on support for our global company.


Because our team not only supports our GitLab instance but also the entire CI platform, including the artifact server, our shared build code, and multiple other custom components of our platform, we had our work cut out for us. We needed a plan that would help us address our challenges while making the most efficient use of our existing resources.


## Moving to GitLab CI


After a careful design review with key stakeholders, we decided to migrate the entire company from Jenkins to GitLab CI. The primary reasons for choosing GitLab CI were:


- We were already using GitLab for source code management.
- GitLab is a complete offering that provides everything we need for CI.
- GitLab CI is designed for scalability and the cloud.
- GitLab CI enables us to write templates that extend other templates, which is compatible with our golden path strategy.
- GitLab is open source software and the GitLab team has always been supportive in helping us submit fixes, giving us extra flexibility and reassurance.


By the time we officially announced that the GitLab CI Platform would be generally available to users, we already had 23% of all builds happening in GitLab CI from a combination of grassroots efforts and early adopters.


The challenge of the migration, however, would be the long tail. Due to the number of custom builds in Jenkins, an automated migration tool would not work for the majority of teams. Most of the benefits of the new system would not come until the old system was at 0%. Only then could we turn off the hardware and save the CloudBees license fee.


## Feature parity and the benefits of starting over


Though we support many different technologies at Indeed, the three most common languages are Java, Python, and JavaScript. These language stacks are used to make libraries, deployables (web services or applications), and cron jobs (a process that runs at regular intervals, for example, to build a data set in our data lake). Each of these formed a matrix of project types (Java Library, Python Cronjob, JavaScript Webapp, etc.) for which we had a skeleton in Jenkins. Therefore, we had to produce a golden path template in GitLab CI for each of these project types.


Most users could use these recommended paths without change, but for those who did require customization, the golden path would still be a valuable starting point and enable them to change only what they needed, while still benefiting from centralized template updates in the future.


We quickly realized that most users, even those with customizations, were happy to take the golden path and at least try it. If they missed their customizations, they could always add them later. This was a surprising result! We thought that teams who had invested in significant customization would be loath to give them up, but in the majority of cases teams just didn't care about them anymore. This allowed us to migrate many projects very quickly — we could just drop the golden path (a small file about 6 lines long with includes) into their project, and they could take it from there.


## InnerSource to the rescue


The CI Platform team also adopted a policy of "external contributions first" to encourage everyone in the company to participate. This is sometimes called InnerSource. We wrote tests and documentation to enable external contributions — contributions from outside our immediate team — so teams that wanted to write customizations could instead include them in the golden path behind a feature flag. This let them share their work with others and ensure we didn't break them moving forward (because they became part of our codebase, not theirs).


This also had the benefit that particular teams who were blocked waiting for a feature they needed were empowered to work on the feature themselves. We could say "we plan to implement the feature in a few weeks, but if you need it earlier than that we are happy to accept a contribution." In the end, many core features necessary for parity were developed in this manner, more quickly and better than our team had resources to do it. The migration would not have been a success without this model.


## Ahead of schedule and under budget


Our CloudBees license expired on April 1, 2024. This gave us an aggressive target to achieve the full migration. This was particularly ambitious considering that at the time, 80% of all builds (60% of all projects) still used Jenkins for their CI. This meant over 2,000[Jenkinsfiles](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/) would still need to be rewritten or replaced with our golden path templates.


To achieve this target, we made documentation and examples available, implemented features where possible, and helped our users contribute features where they were able.


We started regular office hours, where anyone could come and ask questions or seek our help to migrate. We additionally prioritized support questions relating to migration ahead of almost everything else. Our team became GitLab CI experts and shared that expertise inside our team and across the organization.


Automatic migration for most projects was not possible, but we discovered it could work for a small subset of projects where customization was rare. We created a Sourcegraph batch change campaign to submit merge requests to migrate hundreds of projects, and poked and prodded our users to accept these MRs.


We took success stories from our users and shared them widely. As users contributed new features to our golden paths, we advertised that these features "came free" when you migrated to GitLab CI. Some examples included built-in security and compliance scanning, Slack notifications for CI builds, and integrations with other internal systems.


We also conducted a campaign of aggressive "scream tests." We automatically disabled Jenkins jobs that hadn't run or succeeded in a while, and told users that if they needed them, they could turn them back on. This was a low-friction way to identify which jobs were actually needed. We had thousands of jobs that hadn't been run a single time since our last CI migration (which was Jenkins to Jenkins). This told us we could safely ignore almost all of them.


In January 2024, we nudged our users by announcing that all Jenkins controllers would become read-only (no builds) unless an exception was explicitly requested. We had much better ownership information for controllers and they generally aligned with our organization's structure, so it made sense to focus on controllers rather than jobs. The list of controllers was also a much more manageable list than the list of jobs.


To obtain an exception, we asked our users to find their controllers in a spreadsheet and put their contact information next to each one. This enabled us to get a guaranteed up-to-date list of stakeholders we could follow up with as we sprinted to the finish line, but also enabled users to clearly let us know which jobs they absolutely needed. At peak, we had about 400 controllers; by January we had 220, but only 54 controllers required exceptions (several of them owned by us, to run our tests and canaries).


We had a manageable list of around 50 teams we divided among our team and started doing outreach to understand how each team was progressing with the migration. We spent January and February discovering that some teams planned to finish their migration without our help before February 28 others were planning to deprecate their projects before then, and a very small number were very worried they wouldn't make it.


We were able to work with this smaller set of teams and provide them with “white-glove” service. We still explained that while we lacked the expertise necessary to do the migration for them, we could partner with a subject matter expert from their team. For some projects, we wrote and they reviewed; for others, they wrote and we reviewed. In the end, all of our work paid off and we turned off Jenkins on the very day we had announced 8 months earlier.


## The results: Enhanced CI efficiency and user satisfaction


At its peak, our Jenkins CI platform ran over 14,000 pipelines per day and serviced our thousands of projects. Today, our GitLab CI platform has run over 40,000 pipelines in a single day and regularly runs over 25,000 per day. The incremental cost of each job of each pipeline is similar to Jenkins, but without the overhead of hardware to run the controllers. Additionally, these controllers served as single points of failure and scaling limiters that forced us to artificially divide our platform into segments. While an apples-to-apples comparison is difficult, we find that with this overhead gone our CI hardware costs are 10-20% lower. Additionally, the support burden of GitLab CI is lower since the application automatically scales in the cloud, has cross-availability-zone resiliency, and the templating language has excellent public documentation available.


A benefit just as important, if not moreso, is that now we are at over 70% adoption of our golden paths. This means that we can roll out an improvement and over 5,000 projects at Indeed will benefit immediately with no action required on their part. This has enabled us to move some jobs to more cost-effective ARM64 instances, keep users' build images updated more easily, and better manage other cost saving opportunities. Most importantly, our users are happier with the new platform.


**About the author:** *Carl Myers lives in Sacramento, CA, and is the manager of the CI Platform team at Indeed. Carl has spent his nearly two-decade career dedicated to building internal tools and developer platforms that delight and empower engineers at companies large and small.*


**Acknowledgements:** *This migration would not have been possible without the tireless efforts of Tron Nedelea, Eddie Huang, Vivek Nynaru, Carlos Gonzalez, Lane Van Elderen, and the rest of the CI Platform team. The team also especially appreciates the leadership of Deepak Bitragunta, and Irina Tyree for helping secure buy-in, resources and company wide alignment throughout this long project. Finally, our thanks go out to everyone across Indeed who contributed code, feedback, bug reports, and helped migrate projects.*


**This is an edited version of the article[How Indeed Replaced Its CI Platform with Gitlab CI](https://engineering.indeedblog.com/blog/2024/08/indeed-gitlab-ci-migration/) , originally published on the Indeed engineering blog.**
