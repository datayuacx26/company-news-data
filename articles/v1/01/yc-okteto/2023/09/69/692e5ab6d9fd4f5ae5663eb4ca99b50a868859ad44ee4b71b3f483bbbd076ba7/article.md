---
schema_version: "1.0.0"
document_id: "692e5ab6d9fd4f5ae5663eb4ca99b50a868859ad44ee4b71b3f483bbbd076ba7"
company_key: "yc-okteto"
company: "Okteto"
source_id: "yc-okteto-rss-a64bce3f80ea"
canonical_url: "https://www.okteto.com/blog/slash-your-kubernetes-costs-during-development/"
published_at: "2023-09-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.770894+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:5cb80e7fc196b8a5ac8b9cac33f7bbd08190186d1b7e2227630148d97574a4b3"
---

# Slash Your Kubernetes Costs During Development

# Slash Your Kubernetes Costs During Development


## Okteto Features (8 Part Series)


1. [Developing Microservices by Hot Reloading on Kubernetes Clusters](https://www.okteto.com/blog/developing-microservices-by-hot-reloading-on-kubernetes-clusters/)
2. [Four Reasons You Need Preview Environments](https://www.okteto.com/blog/four-reasons-you-need-preview-environments/)
3. [Bring Any Cloud Resource to Your Modern App Dev Workflow](https://www.okteto.com/blog/bring-any-cloud-resource-to-your-modern-app-dev-workflow/)
4. [The Modern Way of Sharing Dev Work With Your Team](https://www.okteto.com/blog/the-modern-way-of-sharing-dev-work-with-your-team/)
5. [The Way You're Using Kubernetes Clusters During Development Is Killing Productivity!](https://www.okteto.com/blog/the-way-you-re-using-kubernetes-clusters-during-development-is-killing-productivity/)
6. Slash Your Kubernetes Costs During Development


7. [How Developers Can Seamlessly Collaborate When Building Microservice Apps](https://www.okteto.com/blog/how-developers-can-seamlessly-collaborate-when-building-microservice-apps/)
8. [Simplifying Launching Development Environments With Okteto Catalog](https://www.okteto.com/blog/simplifying-launching-development-environments-with-okteto-catalog/)


Using Kubernetes can be quite costly, to be frank. Regardless of the cloud provider you choose for your cluster, there are expenses associated with the deployed applications and the cloud resources they consume. If you're considering adopting Kubernetes-based development and preview environments, you may wonder how to effectively manage these costs. Let's delve deeper into the topic and explore Okteto's Garbage Collector feature, which automates resource management to help reduce cloud infrastructure expenditures.


## What Is the Garbage Collector?


Firstly, let's demystify this cool sounding term - Garbage Collector. If you're picturing a physical bin truck rummaging through your Kubernetes cluster, you're not far off the mark. Except, this truck is virtual, and its job is to smartly **scale down** applications during periods of **inactivity** . This ensures your cluster isn't bloated with unused resources, keeping your costs manageable.


The beauty of Okteto's Garbage Collector is that it's fully automated, so you don't have to lift a finger. Gone are the days of meticulously tracking which developer's environment is utilizing unnecessary resources while they're on vacation, and constantly reminding them to discard it. The Garbage Collector effortlessly handles these tasks, granting you peace of mind and valuable time to focus on other crucial responsibilities. This powerful feature benefits both Platform Engineers and Developers in multiple ways:


- Platform Engineers are relieved of the burden of continuously monitoring developers' environments and manually shutting down unused ones.
- Developers are freed from the cognitive load of remembering to destroy their environments once they've completed their work.


With Okteto's Garbage Collector, efficiency and productivity are maximized, allowing everyone to thrive.


## How It Works


The Garbage Collector in Okteto operates through two configurable policies:


1. The first policy determines the duration of inactivity required to scale all Kubernetes resources in a development or preview environment to zero, effectively putting the namespace to sleep.
2. The second policy specifies when the environment should be completely destroyed, including all Kubernetes and external resources associated with it.


Configuring the sleep duration before destruction offers the benefit of allowing developers to effortlessly resume their work without the need for redeployment. Okteto simply wakes up their environment by scaling up all their Kubernetes resources to their specified values.


You might be wondering what qualifies as inactivity. According to the Garbage Collector, an application or its resources are considered inactive if a developer hasn't performed any of the following actions during the specified` inactivity` period:


- Upgrading or redeploying via the UI or command line.
- Launching a development container with` okteto up` .
- Deploying the latest version of their code using` okteto deploy` .


There are a couple of ways to wake up an environment that has been put to sleep. Developers can do this either through the user interface (UI) or via the command line interface (CLI). Another great feature is that if any of the endpoints associated with the development or preview environment receive traffic, Okteto will automatically wake up the environment. This comes in handy when developers share the URL with someone on their team for testing or feedback, and that person or team accesses it after a few days. The garbage collector will ensure that the environment goes to sleep when there is no activity, but as soon as someone accesses that endpoint, it wakes up again.


## How Can You Configure It


If you're aiming to optimize your cloud usage and reduce costs, consider configuring the garbage collector for your Okteto installation. You can find detailed steps on how to do this in the accompanying video.


For more information about this feature, please refer to our[documentation](https://www.okteto.com/docs/self-hosted/administration/configuration/#gc) .


## Conclusion


In conclusion, managing infrastructure costs associated with Kubernetes can be quite a challenge. However, with Okteto's Garbage Collector, you can smartly automate resource management, scaling down applications during periods of inactivity, and ensuring your cluster isn't running with unused workloads. This not only saves costs but also improves efficiency, allowing Platform Engineers and Developers to focus on more important tasks. So why wait? Start optimizing your cloud usage and cut back on unnecessary expenses by configuring the Garbage Collector for your Okteto installation today! For detailed steps on how to do this, refer to our accompanying video and documentation.


## Okteto Features (8 Part Series)


6. Slash Your Kubernetes Costs During Development


Arsh Sharma


Developer Experience Engineer / Emojiologist 😜


[View all posts](https://www.okteto.com/blog/authors/arsh-sharma/)


[kubernetes](https://www.okteto.com/blog/tags/kubernetes/)


#### Share this:
