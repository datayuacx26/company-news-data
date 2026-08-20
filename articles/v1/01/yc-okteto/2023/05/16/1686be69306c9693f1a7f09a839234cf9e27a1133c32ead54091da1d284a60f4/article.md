---
schema_version: "1.0.0"
document_id: "1686be69306c9693f1a7f09a839234cf9e27a1133c32ead54091da1d284a60f4"
company_key: "yc-okteto"
company: "Okteto"
source_id: "yc-okteto-rss-a64bce3f80ea"
canonical_url: "https://www.okteto.com/blog/using-launchdarkly-and-okteto-to-automate-modern-feature-flag-management/"
published_at: "2023-05-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.770894+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:7327f391f7a5564d2207ea929cc5fa655efb6091683287988298242ba50a0648"
---

# Using LaunchDarkly and Okteto To Automate Modern Feature Flag Management

# Using LaunchDarkly and Okteto To Automate Modern Feature Flag Management


## Integrations With Okteto (8 Part Series)


1. [Cloud Native Development Made Easy With Sprkl and Okteto](https://www.okteto.com/blog/cloud-native-development-made-easy-with-sprkl-and-okteto/)
2. Using LaunchDarkly and Okteto To Automate Modern Feature Flag Management


3. [Developing Cloud-Native Apps With MongoDB Atlas and Kubernetes](https://www.okteto.com/blog/developing-cloud-native-apps-with-mongodb-atlas-and-kubernetes/)
4. [Using ArgoCD With Okteto for a Unified Kubernetes Development Experience](https://www.okteto.com/blog/using-argocd-with-okteto-for-a-unified-kubernetes-development-experience/)
5. [Making Your Helm-Packaged Applications Ready for Cloud Native Development with Okteto](https://www.okteto.com/blog/making-your-helm-packaged-applications-ready-for-cloud-native-development-with-okteto/)
6. [Automate Provisioning Any Dev Resource on Any Cloud Provider With Pulumi and Okteto](https://www.okteto.com/blog/automate-provisioning-any-dev-resource-on-any-cloud-provider-with-pulumi-and-okteto/)
7. [Automating Development Environments and Infrastructure with Terraform and Okteto](https://www.okteto.com/blog/automating-development-environments-and-infrastructure-with-terraform-and-okteto/)
8. [Enhance CI Pipelines with Dagger and Okteto Preview Environments for a Better Developer Experience](https://www.okteto.com/blog/ci-pipelines-dagger-okteto/)


## What Are Feature Flags


Feature flags allow developers to enable or disable features in an application without deploying a new version. This enables teams to release new features gradually and test them with a subset of users before rolling them out completely. In today's world, feature flagging has become a standard practice in modern software development.[LaunchDarkly](https://launchdarkly.com/) is a popular feature flagging service that allows developers to manage their features and rollouts with ease.


## What Does Okteto Do


Okteto is a platform for modern development experience automation on Kubernetes. One of the features we offer is provisioning and de-provisioning environments that are exactly like production. At Okteto, we've always believed in providing developers with a realistic development environment that's set up instantly and has everything they need for development. Our goal is to give developers the ultimate experience when developing modern applications.


This means that if you use feature flags in production, you should be able to work with them during development as well. As part of this philosophy, we recently launched a new feature to our platform: External Resources. This feature makes working with LaunchDarkly during development a breeze.


See Okteto External Resources in action. Join us


[Register for the webinar](https://www.okteto.com/devx-with-external-resources/)


## Why Develop Using LaunchDarkly With Okteto


Developers are already using LaunchDarkly's SDK to write code with feature flags. However, a common problem they face is the difficulty of testing these feature flags during development. A major issue is having to manually set up a LaunchDarkly environment with pre-populated flags for every developer who wants to work on the application or test features. This process takes time away from coding and can lead to a poor developer experience. Additionally, there is no easy way to replicate the same feature flags present in the production environment for each developer.


Using LaunchDarkly dashboards as External Resources with Okteto solves both of these problems by:


- Automatically provisioning (and deprovisioning) feature flag dashboards
- Cloning existing environments, which ensures that the LaunchDarkly environment during development is pre-populated with all necessary flags


## How To Use LaunchDarkly Dashboards As External Resources


You can configure a feature flag dashboard in LaunchDarkly to be set up for each developer when they spin up their development environment. This dashboard is already configured to work with the application being developed and requires no configuration from the developer’s side. It has all the flags you want to include from your existing production environments.


To demonstrate the power of using LaunchDarkly as an External Resource in your development environments, we’ve put together a sample application for you to try out:[https://github.com/okteto/external-resources-launchdarkly](https://github.com/okteto/external-resources-launchdarkly)


This simple application demonstrates how easy External Resources make working with services that live outside your Kubernetes cluster, a LaunchDarkly dashboard in this case.


The application displays some text with an emoji. If you look at the code, you’ll see we use LaunchDarkly’s SDK to change the text and emoji when the feature flag is enabled. When you deploy the application using Okteto, Okteto not only deploys the app itself but also provisions a feature flag dashboard for you in LaunchDarkly. You can view this right from your Okteto dashboard.


If you visit the endpoint shown for the LaunchDarkly dashboard, you should be able to toggle the “Use new emojis” feature flag on and off. Refreshing your application should show you the replaced text and emoji.


The power of this feature is that developers can work in a running development environment that already has all the feature flags that the application uses without needing to set anything up. They don't have to create a dashboard in LaunchDarkly every time or manually clone all the flags they need from the production environment. Another benefit of External Resources is that, like your dev environments, they're ephemeral. This means that when you destroy your dev environment, the LaunchDarkly dashboard will also be destroyed with it. This makes it easier for platform engineers and team leads to manage resources without having to worry about chasing people around.


## Conclusion


Leveraging feature flags during development with LaunchDarkly and Okteto can help teams release new features gradually and test them with a subset of users before rolling them out completely. By using this combination:


- developers can easily manage feature flags in their development environments
- ensure that their code is working correctly before releasing it to production
- save a lot of time and effort during development by allowing developers to connect to resources that their application depends on without having to set them up locally.


Check out this video demonstrating the benefits you get from using LaunchDarkly dashboards as an External Resource in Okteto:


Find this interesting?


[Try out the sample!](https://github.com/okteto/external-resources-launchdarkly)


For more information about External Resources, check out our[press release post](https://www.okteto.com/blog/okteto-extends-developer-experience-to-all-resources/) announcing the feature or head to the[docs](https://www.okteto.com/docs/cloud/external-resources/) !


## Integrations With Okteto (8 Part Series)


2. Using LaunchDarkly and Okteto To Automate Modern Feature Flag Management


Arsh Sharma


Developer Experience Engineer / Emojiologist 😜


[View all posts](https://www.okteto.com/blog/authors/arsh-sharma/)


#### Share this:
