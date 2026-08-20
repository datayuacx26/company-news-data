---
schema_version: "1.0.0"
document_id: "00effc41a714f259d9d735b54d113a035e9ebb261b3fccde6f826a391e0333ec"
company_key: "yc-okteto"
company: "Okteto"
source_id: "yc-okteto-rss-a64bce3f80ea"
canonical_url: "https://www.okteto.com/blog/automating-development-environments-and-infrastructure-with-terraform-and-okteto/"
published_at: "2023-11-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.770894+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:3c8eb73da040e286e6fa9ccc9ada7656f9a50474af44a004fd271bbe90277bbb"
---

# Automating Development Environments and Infrastructure with Terraform and Okteto

# Automating Development Environments and Infrastructure with Terraform and Okteto


## Integrations With Okteto (8 Part Series)


1. [Cloud Native Development Made Easy With Sprkl and Okteto](https://www.okteto.com/blog/cloud-native-development-made-easy-with-sprkl-and-okteto/)
2. [Using LaunchDarkly and Okteto To Automate Modern Feature Flag Management](https://www.okteto.com/blog/using-launchdarkly-and-okteto-to-automate-modern-feature-flag-management/)
3. [Developing Cloud-Native Apps With MongoDB Atlas and Kubernetes](https://www.okteto.com/blog/developing-cloud-native-apps-with-mongodb-atlas-and-kubernetes/)
4. [Using ArgoCD With Okteto for a Unified Kubernetes Development Experience](https://www.okteto.com/blog/using-argocd-with-okteto-for-a-unified-kubernetes-development-experience/)
5. [Making Your Helm-Packaged Applications Ready for Cloud Native Development with Okteto](https://www.okteto.com/blog/making-your-helm-packaged-applications-ready-for-cloud-native-development-with-okteto/)
6. [Automate Provisioning Any Dev Resource on Any Cloud Provider With Pulumi and Okteto](https://www.okteto.com/blog/automate-provisioning-any-dev-resource-on-any-cloud-provider-with-pulumi-and-okteto/)
7. Automating Development Environments and Infrastructure with Terraform and Okteto


8. [Enhance CI Pipelines with Dagger and Okteto Preview Environments for a Better Developer Experience](https://www.okteto.com/blog/ci-pipelines-dagger-okteto/)


If you're developing a modern cloud-native app, it's very likely that your app is leveraging some cloud resources. These could be things like storage buckets, messaging queues, or any of the numerous services that different cloud providers provide these days. The question that often arises is, how do you ensure that when your developers write code, they have access to these services and resources in a running cloud-native environment? In this blog, we will learn how you can automate this process and provide developers with development environments that take care of all this with the help of Terraform and Okteto!


## The Problem When Developing Modern Apps


When approaching the problem we discussed above, organizations traditionally take one of these few approaches.


First is simply being unable to provide access to these resources. This case is farthest away from ideal because it means developers on your team just won't be able to develop properly. They would have to wait until staging to be sure that the changes they made are working or not.


The second approach is to teach developers how to create these resources manually. This isn't very ideal for two reasons:


- One, it involves developers learning different cloud providers and about their resources, thus adding to the cognitive load. Developers in many cases also don’t know how to configure things and how the infra is configured. This limits their ability to create production-like dev environments.
- And second, it is very time-consuming and cumbersome to have to repeat this process manually each time.


This is a problem people faced with their production infrastructure as well. And that is what led to the rise of Infrastructure as Code tools like Terraform. So what I want you to ask yourselves is, if you're already using Terraform scripts to create this infrastructure for production, why not use it for development as well? Now you might be tempted to just ask developers to run these Terraform scripts themselves. But that again suffers from more problems:


- It involves developers having some extent of Terraform knowledge and even installing and managing Terraform and the cloud credentials it needs on their own, which again adds to the cognitive load.
- It again, at the end of the day, is a manual process because developers need to run the scripts and take care of deleting things when they're done.


The first problem might be a big one from just a compliance and security perspective. Because you would need to ensure that you provide access to your cloud provider to developers in a secure way instead of them using any ad hoc credentials or IAM users/roles to create resources, thereby making managing things extremely tough in the long run. So what's the solution?


The solution is automating this problem away with Okteto. You can leverage your same Terraform scripts and automate this entire process with the help of Okteto. This means whenever a developer creates their development environment, the cloud resources will automatically be created for them using the right credentials that you, as platform engineers, have configured. Developers don't need to do anything themselves. In fact, they wouldn't even need Terraform installed locally.


On top of that, whenever they delete these ephemeral environments (or they remain unused for a fixed period), the resources will automatically get deleted for them. Pretty cool, right? This saves costs since you don't have to worry about resources running when nobody is using them.


If you would like to see this in action and learn about the value it provides, check out this webinar we recently did with our friend Richard, who is a HashiCorp Ambassador as well.


## How To Set It Up


The best part is that integrating Terraform with Okteto is pretty simple, thanks to Okteto External Resources feature. Check out this video, which walks through how you can do this in a few easy steps and automate the pains of developers away!


## Conclusion


As platform and devX engineers, you want to be able to provide the best workflows that make developers' lives easier. This involves giving them access to the resources and running environments they need without adding any additional cognitive load. With Terraform and Okteto, you can now automate the creation of development environments and access to cloud resources, making it a seamless experience for developers. This is a win for platform teams as well, as the access is now provided in a secure and compliant way, giving them complete governance over resources.


So go ahead and try out this feature with Okteto. You can access our sample app, which demonstrates using Terraform and Okteto,[here](https://github.com/okteto/oktetodo-terraform-s3) .


To get started, you can sign up for a free 30-day trial of Okteto[here](https://www.okteto.com/free-trial/) ! And if you have any questions or need help setting this up, don't hesitate to reach out to us. Happy automating!


## Integrations With Okteto (8 Part Series)


Arsh Sharma


Developer Experience Engineer / Emojiologist 😜


[View all posts](https://www.okteto.com/blog/authors/arsh-sharma/)


[terraform](https://www.okteto.com/blog/tags/terraform/)


#### Share this:
