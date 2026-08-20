---
schema_version: "1.0.0"
document_id: "ad12b48a82042adb633b31430e6e287e1978d7e88ab2ce44f91f1bdc4c48821e"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/console-pricing-update/"
published_at: "2025-01-23T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:8cddbae4b96b22e042edf6d94b01e181922e1c6d8a426c2200e4cfa031c3dc24"
---

# Console pricing update

[Blog](https://sst.dev/blog)


# Console pricing update


[Jay](https://x.com/jayair) January 23, 2025


Starting Feb 1, we’ll be rolling out a new pricing model for the Console. Currently the pricing is based on the number of times the Lambda functions in your apps are invoked. Moving forward, it’ll be based on the **number of active resources** in your apps.


---


### Why the change?


When SST was first released, there was a big focus on serverless and Lambda functions. The[Issues](https://sst.dev/docs/console#issues) feature was also the focal point of the Console. So it made sense to tie the pricing to the number of Lambda function invocations in your apps.


Over the last few months, we added the ability to[Autodeploy](https://sst.dev/docs/console#autodeploy) your apps. And added broader support for[container services](https://sst.dev/blog/container-support) . So a Lambda specific metric doesn’t really make sense anymore.


We think the number of active resources will be more representative of your usage.


---


### How does it work?


At the start of every month, or billing cycle, the Console will keep track of the stages that are updated across your workspace, add up the resources in those stages, and apply the following rate.


Resources Rate per resource


First 2000 $0.086


2000+ $0.032


**Free Tier** : Workspaces with 350 active resources or fewer.


---


#### Examples


So for example, if you have the following number of active resources in your workspace:


Resources Cost per month


350 You are in the free tier, so you’ll be charged $0.


500 You are above the free tier, so you’ll be charged $0.086 x 500 = $43.


2500 You are in a higher tier, so $0.086 x 2000 + $0.032 x (2500 - 2000) = $188.


The count of the active resources in your workspace resets at the start of every billing cycle.


---


### FAQ


1.


Do I need to use the Console to use SST?


You **don’t need the Console** to use SST. It compliments the CLI and has some features that help with managing your apps in production.


That said, it is completely free to get started. You can create an account and invite your team, **without** having to add a **credit card** .


2.


I’m still trying out the Console. Can I continue using it?


You can continue using the Console as a part of the free tier. If you go over the free tier, you won’t be able to access the *production* or deployed stages.


However, you can continue to **access your personal stages** . Just make sure you have` sst dev` running locally. Otherwise the Console won’t be able to detect that it’s a personal stage.


3.


What is an active resource?


Resources are what SST creates in your cloud provider. This includes the resources created by both SST’s built-in components, like` Function` ,` Nextjs` ,` Bucket` , and the ones created by any other Terraform/Pulumi provider.


Some components, like` Nextjs` and` StaticSite` , create multiple resources. In general, the more complex the component, the more resources it’ll create.


You can see a[full list of resources](https://sst.dev/docs/console#resources) if you go to an app in your Console and navigate to a stage in it.


For some context, the Console is itself a pretty large[SST app](https://github.com/sst/console) and it has around 320 resources.


A resource is considered active if the stage it belongs to has been updated during the billing cycle. However, if the only update to a stage was to remove it, the resources in that stage will not be counted as active.


4.


What about PR stages?


A stage has to be around for at least 2 weeks before the resources in it are counted as active. So if a PR stage is created and removed within 2 weeks, they don’t count.


However, if you remove a stage and create a new one with the same name, it does not reset the 2 week initial period.


5.


Does this apply to SST v2 and v3 apps?


Yes, it applies to any kind of SST app.


6.


What if I’m on the old plan?


If you are on the old plan, you don’t have to switch and you won’t be automatically switched over either.


You can go to the workspace settings and check out how much you’ll be billed based on both the plans. To switch over, you can cancel your current plan and then subscribe to the new plan.


At some point in the future, we’ll remove the old plan. But there’s no specific timeline for it yet.


7.


Are there any volume pricing options?


Yes you cancontact us and we can figure out a pricing plan that works for you.


[Learn more about the new pricing in our docs](https://sst.dev/docs/console#pricing) .


---


If you’ve got any questions about the new pricing, or about the Console in general, feel free toget in touch .
