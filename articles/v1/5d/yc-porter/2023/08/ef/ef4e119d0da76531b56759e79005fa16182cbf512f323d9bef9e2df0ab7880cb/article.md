---
schema_version: "1.0.0"
document_id: "ef4e119d0da76531b56759e79005fa16182cbf512f323d9bef9e2df0ab7880cb"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-7cfcee1b8c2a"
canonical_url: "https://www.porter.run/blog/why-pay-for-heroku-render-fly-io-when-you-have-aws-credits"
published_at: "2023-08-14T00:00:00+00:00"
first_seen_at: "2026-07-22T09:55:21.208768+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:2c4bb5b3e81dc1b34d6d527e9b7042f490b7587963d2a80f0dac0672dbacadfb"
---

# Why pay for a PaaS when you have AWS credits?

AWS, GCP, and Azure throw hundreds of thousands of dollars in cloud credits at early stage startups every year. Yet, so many startups still choose to deploy on another hosting platform like Heroku, Render, and Fly.io to move fast without worrying about DevOps. An average YC startup has access to a whopping total of 900k in cloud credits across the three cloud providers, but so many of them still deploy on some kind of a PaaS just to move fast.


Why do startups choose to do this? To focus on building product. No early-stage startup should be wasting time on undifferentiated infrastructure work. These platforms all provide a phenomenal developer experience that *just works* . **** For just a few hundred bucks, startups are buying the peace of mind that their product will run smoothly and reliably as long as they provide the code.


What many startups aren't aware of or don’t think about, is that many of these platforms still run on AWS (or some kind of public cloud like GCP and CloudFlare) under the hood. At the end of the day, you're still using AWS, except you're using someone else’s account and are paying for it with your own money instead of the credits that you already have. Furthermore, most startups that initially deploy on these platforms end up moving to their own cloud infrastructure at some point anyway as they scale and run into constraints.


‍


But what if these platforms deployed your applications in your own AWS account, as opposed to *their* AWS account, with the exact same developer experience?


That’s what we're trying to find out with[Porter](https://porter.run/for-seed-stage-companies) .


The obvious benefit of deploying to your own cloud account would be that you can use your cloud credits without having to pay out of pocket. But there are other benefits as well:


1. Flexibility - as previously noted, most startups that initially deploy on a PaaS end up moving to their own infrastructure on AWS/GCP/Azure as they scale in order to meet the growing complexity of their applications.[This so-called “graduation” happens on many axes](https://www.porter.run/blog/does-the-world-really-need-another-heroku) . And migrating away from these platforms at maturity is often quite difficult, because 1) you are actually serving production traffic at scale at that point 2) it is in the PaaS provider’s best interest to retain users by making them reliant on their ecosystem. Getting started on your own infrastructure from day 1 allows you to customize your infrastructure as you grow and even eject from the said PaaS if you want to start managing your DevOps in-house.


1. Reliability - When applications are deployed on these platforms, they are deployed on shared infrastructure on public clouds that the platform provider is managing and allocating among their users. Because allocating resources among users in a reliable way while guarding your margins is a gnarly scheduling problem, and protecting the shared infrastructure against bad actors (e.g. crypto miners and bots) is a logistical nightmare, it’s very difficult for these platforms to match the tried and true reliability of using one of the massive public cloud providers like AWS as a sole tenant in your own account.


1. Security - Related to #1, deploying on a large multi-tenant architecture comes with inherent threat vectors, especially when you are sharing the underlying infrastructure with potentially malicious actors. It’s hard to beat the safety of deploying in your own AWS account with all your build artifacts residing in your own registries and having your own VPC.


These are the benefits. Then are there limitations that comes with deploying to the user's own AWS account compared to a traditional PaaS?


Many people are initially skeptical that developer experience can be equally easy if things are happening in their own AWS account. Once they’ve tried out the platform, however, our users tell us that there is no difference between using Porter vs a traditional PaaS other than the extra 20 minutes it takes to connect your cloud account.


Of course, there are more benefits that traditional PaaS’s deliver, other than just the easy developer experience - infra management, workload-specific optimization, better support are some examples just to name a few. Does the platform's ability to deliver these benefits get compromised in any way if things are running in the user’s own cloud?


We strongly believe that the answer to this question is a no. Given the right set of permissions, it is possible to manage the underlying infrastructure and even interact with the public cloud provider on the user’s behalf (e.g. request a quota increase, optimize costs, request support) - if the provided permissions are the same, why does it matter which account the underlying infrastructure is running in?


Porter gets you started in your own AWS account from day 1 with the same easy developer experience, so you can use your credits instead of paying out of pocket. Unbeknownst to the user, an added bonus is that we deploy your applications on the same kind of infrastructure that growth-stage companies are running so you're prepared for the eventual moment that you find PMF and need to scale. And yes, as much as this seems to be a champagne problem that early stage founders shouldn’t care about,[this actually does happen in the best case outcome for your startup](https://www.porter.run/case-study/how-writesonic-runs-a-1-6-tb-kubernetes-cluster-with-no-devops-engineers) - and when it does, you need to scale *fast.*


Our intent is to make sure no early stage startup needs to pay for Porter when they are just getting started out. Redeem[our startup deal](https://gcpjnf9adme.typeform.com/to/vUg9SDWf) if your startup has less than 5M in funding and use it with your cloud credits.


Check out[this page](https://www.porter.run/for-seed-stage-startups) if you’re an early stage founder and want to better understand what we’re building.


‍
