---
schema_version: "1.0.0"
document_id: "a3d3116a8cb5c40a95c727081cff98ab955c40fcf7a615650a3e0c5dfd677523"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-sharechat-tests-every-release-on-kubernetes/"
published_at: "2023-06-02T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:700c6b4ee13eb51e79a2b814036e3ce6013e7966ef31ca40e49f836595fb1240"
---

# How ShareChat Tests Every Pull Request on Kubernetes using Signadot

We’re very happy to share the story of ShareChat’s success using Signadot to test code on a shared Kubernetes cluster.


## Background


ShareChat is an Indian social media startup that caters to over 1 billion wireless network users. It offers a content consumption and sharing platform in 15 vernacular Indian languages.


## The Problem


ShareChat has a large engineering team with over 500 members. They manage more than 100 microservices that run on Kubernetes, the popular open-source container orchestration platform. The engineering team at ShareChat has faced challenges with testing every feature independently before merging to production. The team lacked isolation in using the shared staging environment for testing. Developers would block each other during testing as any regressions introduced would bring the env down and impact other teams’ ability to test. They recognized a need for a separate environment, similar to staging, to thoroughly integration test their microservices. This would help ensure that API contracts weren’t broken and that everything worked seamlessly together. With this new environment, ShareChat can feel confident that their microservices are fully tested and ready for release to their users. However, given the scale of their microservices based application, a traditional approach of duplicating environments was not feasible.


Moreover, developers were not able to collaborate on features that touched multiple microservices. This involved long feedback cycles and manual coordination across teams that had service dependencies. Each iteration required developers to make the code changes, wait on CI/CD pipelines to push the changes to staging in the right order and test the combined changes once all of the dependencies have been deployed. Going through multiple such iteration cycles slowed down developers considerably. This significantly impeded the teams’ ability to ship features fast.


## The curse of ‘it works on my machine:’ testing in staging on K8s


When trying to replicate production for testing changes, ShareChat faced a problem familiar to anyone with a large microservice build. The iteration cycles were long as developers had to wait on slow CI/CD pipelines to deploy changes to staging. Moreover, debugging in staging was slow as there are many commits going into staging every day and troubleshooting the root cause of test failures took a long time. Developers who were pushing code that broke contracts weren’t finding out within minutes, but often waiting days to discover some interaction that caused errors.


> *Developers were experiencing frequent rollbacks of code, due to issues discovered late, in the staging environment.*


*-Devarshi Khanna, Backend Developer, ShareChat*


## Testing the Signadot Way


With Signadot, developers at ShareChat are able to test their changes using Sandboxes in the staging environment while actively writing code without impacting other developers. To streamline the testing process and enable developers to work more efficiently, ShareChat integrated the Signadot CLI into their CI pipeline for every Pull Request (PR). This integration automatically creates a Sandbox for each PR, providing developers with isolated testing environments. Here’s how they use Signadot:


### Automated Sandbox Creation


When developers submit a Pull Request, the Signadot CLI automatically creates a Sandbox, which is a dedicated environment for testing the changes introduced in the PR. This Sandbox allows developers to test their microservices independently without interfering with other developers’ work. Services running in Sandboxes can participate in end-to-end flows


### Routing Keys for Testing


Developers use Routing Keys associated with their Sandboxes to test changes in microservices from the frontend, such as the mobile app. The mobile app has a mechanism to set request headers on outgoing calls, and the Routing Key is used to direct these calls to the corresponding Sandbox.


### Collaboration Across Service Dependencies


ShareChat developers often work on features that span multiple microservices with dependencies on one another. To facilitate collaboration and testing of these complex scenarios, developers create RouteGroups that combine multiple Sandboxes into a single context and Routing Key. This allows them to test end-to-end flows involving multiple microservices, each being actively developed in their respective branches.


The integration of Signadot into ShareChat’s CI pipeline, along with the use of Sandboxes and Routing Keys, significantly enhances the testing process by providing isolated environments for each change and allowing collaborative testing across service dependencies.


## Results after Using Signadot


After implementing Signadot and adopting its testing approach, ShareChat observed remarkable improvements in their development and deployment workflows. Here are the results they experienced:


### Increased Confidence to merge code


In the Sandboxes, the “under-test” versions of microservices can actively participate in end-to-end request flows, enabling developers to develop and test in a high-fidelity environment without using mocks. This seamless integration gives developers a high degree of confidence to merge their code changes.


### Faster Iteration in the Staging Environment


With the ability to use Sandboxes in the shared staging environment, developers could iterate on code changes without impacting other teams. Sandboxes offered isolated developer environments, allowing issues to be discovered and resolved during active development. This led to faster iterations, reducing the time developers were blocked by frequent outages in the shared environment.


### Increased Collaboration and Faster Feature Shipping


Signadot enabled developers to iterate on service dependencies much earlier in the development lifecycle. The ability to test end-to-end flows involving multiple microservices in their development branches improved collaboration on Service APIs. As a result, ShareChat’s development teams achieved a 2-3x faster cadence for shipping new features to production.


By leveraging Signadot’s capabilities, ShareChat successfully addressed the challenges they faced with testing and collaboration, ultimately leading to more efficient development processes and accelerated feature delivery.


To stay updated on the latest insights, best practices, and industry trends in developer productivity and scaling microservices development, we encourage you to subscribe to our newsletter. Additionally, you can[sign up](https://www.signadot.com/signup/) for our free tier to experience the power of Signadot firsthand. Join our active[Slack group](https://join.slack.com/t/signadotcommunity/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ) to engage in discussions with like-minded developers and experts, and be part of a vibrant community!


‍
