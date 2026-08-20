---
schema_version: "1.0.0"
document_id: "4309f65d018a4288d9d3eb8ecfc671983e0b3a8968a6b40d92853b5fe817dcc4"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-7cfcee1b8c2a"
canonical_url: "https://www.porter.run/blog/does-the-world-really-need-another-heroku"
published_at: "2023-07-27T00:00:00+00:00"
first_seen_at: "2026-07-22T09:55:21.208768+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:637221303233d8586c079de956687fe13eb44d361523ca84519e0cba6fc9764d"
---

# Does the world really need another Heroku?

There are so many Platform as a Service (PaaS) companies in the world right now. As Heroku declines, many next-gen platforms have emerged to take on the mantle of the platform that was once regarded as the insurmountable incumbent of the category.


‍


It's a saturated space with so many options. Looking to host your frontend? use Vercel or Netlify. General workloads? Render. Edge compute? Fly. Framework-specific options? Deno, Gigalixir, … the list goes on.


‍


On the one hand, this is a welcome development. It’s getting easier and easier for developers to deploy their applications. Boring infrastructure setup is becoming increasingly commoditized and accessible to developers. On the other hand, one cannot help but ask - Does the world really need another Heroku?


‍


Sure, Heroku has its problems, but it’s still a phenomenal platform. While many of these next-gen PaaS companies (including Porter) try to replicate the “Heroku magic” with a massive second-mover advantage and 10 years of technological progress that Heroku did not have, my honest opinion is that Heroku is still hard to beat when it comes to developer experience.


‍


Every time we log into Heroku to help a customer migrate, we are genuinely humbled by the fact that this platform was created in 2007 - with the[12 Factor App](https://12factor.net/) philosophy and features like Buildpacks and Review Apps, Heroku was truly years ahead of its time. Keep in mind that when Heroku came out, Docker and Kubernetes (or Nomad or Mesos for that matter) didn’t even exist. They used Foreman and had to write their own orchestration layer. Despite having to build a lot of stuff from scratch on the infrastructure-level, they somehow still nailed developer experience. It’s pretty mind-blowing what Heroku accomplished at that point in time the more you think about it.


‍


We believe that there is really not much more that any company can do better than Heroku on the developer experience front. Heroku has already achieved the best-in-class DX, and its descendants can only strive to emulate it.


‍


So if Heroku is that amazing, why are we even bothering to build Porter?


‍


Well, we also think Heroku did something wrong.


‍


If you’re going to build a spiritual descendant of a generation-defining product like Heroku, it seems prudent to not only emulate what it did well, but also learn from where it fell short. Surely, Heroku’s gradual decline cannot just be entirely attributed to the fact that it was acquired by Salesforce. If you’re recreating Heroku in 2023, you owe it to yourself to ask: why should it work this time?


‍


We believe that Heroku, and really any other traditional PaaS, fundamentally suffers from a “graduation problem”, in which their most mature customers move off the platform to their own cloud infrastructure in order to meet the growing complexity of their applications.


‍


This “graduation” happens on many axes. From our experience migrating companies off Heroku as they outgrew the platform, the most common axes tend to be:


- networking - e.g. customizing timeouts, connection limits
- scaling - e.g. having more granular control over how much RAM/vCPU is assigned to an application
- security - e.g. running apps in a VPC with private inter-service communication
- reliability - being able to control your own destiny on top of AWS’s SLA
- cost - hosting cost rises exponentially, “punishing” you as you scale
- political - something along the line of "we're growing now so we're moving into our own cloud no matter what"


… along with a ton of other reasons that are as minor as the *slug size limit* that leads to a death by a thousand paper cuts of a sort.


‍


So where should startups that anticipate scale but do not want to deal with infrastructure deploy their applications? Is it inevitable that we have to log into the dreadful AWS console and waste time managing infrastructure at work? Is the “Heroku magic” truly something we can only experience on our personal projects?


‍


We believe that it is possible to build a platform that hits the sweet spot between simplicity and flexibility. A platform that is just as easy to get started as Heroku, but provides enough escape hatches that allows users to customize their infrastructure as they grow if desired.


‍


These are some things we are doing differently with Porter to build that kind of a platform:


- We deploy applications in your own AWS account. You have full ownership over your infrastructure, can customize under the hood if desired, and integrate with other AWS services (or on GCP or Azure). Deploying to your cloud account also has the benefit that we can’t possibly screw up and incur downtime on your production servers.
- We fully manage infrastructure for you, even if it’s in your own AWS account. The only difference between using Porter and using a traditional PaaS like Heroku is the extra 20 minutes it takes to connect your AWS account. If you accidentally make a change directly from the AWS console, Porter will immediately reconcile it and keep you within guardrails.
- We build on top of standard open-source projects and tooling, so that there is no vendor lock-in. If you have to eject for any reason, you can bring on a DevOps engineer who will find that under the hood it’s all just the same tools and framework they’re used to, already set up with best practices.


‍


The big three cloud providers throw hundreds of thousands of dollars in cloud credits at early-stage startups every year, yet so many still choose to deploy on a PaaS for simplicity, only to find themselves go through a painful migration to their own cloud infrastructure a few years down the road. It’s been almost two decades since the birth of Heroku, and there is still so much yak to be shaved when it comes to deploying applications.


‍


But we don’t believe the answer is to build another carbon copy of Heroku. We’ve already seen how that plays out. You gotta do *something* different.‍
