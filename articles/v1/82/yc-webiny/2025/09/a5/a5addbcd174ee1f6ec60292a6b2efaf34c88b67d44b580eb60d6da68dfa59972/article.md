---
schema_version: "1.0.0"
document_id: "a5addbcd174ee1f6ec60292a6b2efaf34c88b67d44b580eb60d6da68dfa59972"
company_key: "yc-webiny"
company: "Webiny"
source_id: "yc-webiny-news-import-09ea055e8444"
canonical_url: "https://www.webiny.com/blog/how-to-set-up-environments-playground-to-production"
published_at: "2025-09-19T09:37:07.905+00:00"
first_seen_at: "2026-07-22T19:34:09.199841+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:9e423dec3edf6cd9568a8d358e36bc86d3d5a9a8c6665199794fe6834394f11f"
---

# How to Set Up Environments: From Playground to Production | Webiny

If you’ve spent any time in IT, you’ve probably experienced one of those moments where the UAT (User Acceptance Testing) team raises a bug because something odd happened—maybe a feature that used to work suddenly stopped, or data mysteriously disappeared. Large organizations often rely on robust (and sometimes clunky) environment and change management processes to prevent exactly this.


In this article, we will explore how to strike the right balance: staying agile and lean while still having enough structure and well-defined environments in place to support rapid improvements and maintain a stable codebase.


When starting a new project in Webiny, one of the most common questions we hear is:


**“How should I structure my environments?”**


The short answer: think beyond just


**development** and


**production** . To get the most out of Webiny (and any software product ), we recommend setting up a clear environment strategy that supports experimentation, testing, stakeholder reviews, and reliable production deployments.


Here’s the approach we suggest for most teams:


## **🧪 Development Playground**


This is your


**sandbox** .


- **Purpose** : For developers to quickly try ideas, test features, or validate integration routes without worrying about breaking something critical.


- **Characteristics** :


-


- Can be reset, rebased or redeployed frequently.


- May not hold long-term content or data.


- Minimal governance.


- **Good for** : Prototyping new page layouts, testing custom plugins, experimenting with deployment pipelines.


- **TOP TIP** : in Webiny, we recommend that each developer has their own dev playground and all changes are merged into the common dev environment once they have been validated by the engineer themselves. We also offer the


[watch command](https://www.webiny.com/docs/core-development-concepts/basics/watch-command) in Webiny. It will help you to see the changes you are making both back and front end in a matter of seconds without having to deploy the whole project. Please note this tool is only meant to be used in non-Prod environments to speed up the development and debugging process.


## **✅ Staging Environment**


This is your


**first “serious” testing environment** .


- **Purpose** : A stable space for QA teams, product owners, or stakeholders to test features before they move closer to production.


- **Characteristics** :


-


- Receives regular updates from development.


- Contains test data that mimics production.


- Used for validating features against acceptance criteria.


- **Good for** : Formal testing, bug retest, new feature playbacks, stakeholder sign-off.


## **🚀 Pre-Production Environment**


This is your


**staging area** —data and code as close to production as possible.


- **Purpose** : Final rehearsal before going live.


- **Characteristics** :


-


- Mirrors the production configuration (infrastructure, integrations, permissions).


- Can hold real(istic) data or even a copy of production data. Still, here the team needs to be extra careful with GDPR and confidential content - either mask it or not copy it in the new environment, or lock the permissions to be the same like in production to avoid data leaks and security risks.


- Used for performance testing, final QA, and deployment validation.


- **Good for** : Ensuring “it works here, so it should work in prod.”


## **🌍 Production Environment**


This is your


**live environment** —the real deal.


- **Purpose** : Hosting your live Webiny applications and content, serving end users.


- **Characteristics** :


-


- Higher infrastructure specs by default in comparison to the dev playground and staging environment.


- Stable, secure, and monitored.


- Receives only well-tested updates.


- Backups and disaster recovery policies are in place. To find out more about this topic, we have a special step-by-step guide on the topic. Please reach out on Slack for a copy.


- **Good for** : Delivering reliable and delightful digital experiences to your users.


## **➕Others**


The combination above is optimal, but it is not set in stone. Some organizations prefer to have separate QA (Quality Assurance) and/or SIT (System Integration Testing) or UAT (User Acceptance Testing) environments. In some companies, content creators want to have a playground for data modeling and page design instead of doing this directly in Production. Occasionally you may create a temporary environment to build a specific POC. The list of use cases is endless. The right strategy depends entirely on your organizational process and goals. The smaller the number of environments, the less complexity and the less cost you will have to handle.


## **Why Multiple Environments Matter**


- **Safety** : Developers can innovate in dev/ playground without risking live content.


- **Quality** : Bugs are caught in UAT, not by end users.


- **Confidence** : Pre-prod reassures teams that deployments will succeed in production.


- **Better Planning** : Pre-prod can confirm the time estimates on migrations, updates, and other processes that need to run in the background upon upgrading the code base or data.


- **Collaboration** : Different teams (engineering, QA, marketing, stakeholders) each get an environment that fits their workflow.


## **Final Thoughts**


Your exact setup may vary depending on your team size, project complexity, and release cadence, the project stages you want to go through. But at minimum, having a


**playground** ,


**staging** ,


**pre-prod** , and


**production** environment will give you the balance of flexibility and control needed for a smooth Webiny development lifecycle. If you want to find out more about how you do the actual technical setup for your environments, please have a look at this


[article](https://www.webiny.com/docs/core-development-concepts/ci-cd/environments) .


At Webiny, we have seen this environment structure help teams ship faster, collaborate better, and maintain high-quality standards—all while keeping production stable and reliable.
