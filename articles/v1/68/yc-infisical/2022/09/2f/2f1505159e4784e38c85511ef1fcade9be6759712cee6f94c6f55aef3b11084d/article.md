---
schema_version: "1.0.0"
document_id: "2f1505159e4784e38c85511ef1fcade9be6759712cee6f94c6f55aef3b11084d"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/stop-using-env-files"
published_at: "2022-09-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:2d27a73042ba499b17046b22d0dc410a2566b86d2ed1c1ae08e3d94158e261cb"
---

# Stop using .env files!

Like many full stack developers, I began my foray into programming through online courses on Udemy where I learned to build and deploy applications on Heroku. Fast forward 7 years later, my tech-stack evolved to include tools like Next.js and Apollo GraphQL but many principles stayed the same — one such principle was how I handled environment variables with .env files.


### History


In 2012, developers at Heroku came up with the[Twelve-factor app methodology](https://12factor.net/) to help build scalable SaaS apps. One of the 'factors' in this framerwork was about replacing hard-coded configs and secrets with` environment variables` . Benefits were obvious: keep code and sensitive secrets separate to minimize the risk of leaking these secrets and making deployment processes easier.


And this idea became really popular – millions of software developers emraced it and are keeping their app secrets in the so-called .env files.


For context, if you’re unfamiliar with .env files, they’re a simple but flawed way to store secrets and configs as environment variables. They look as simple as:


To this day, .env files are very popular (32 million downloads on npm alone ) - most of the companies and individual developers use them in one way or another. At the same time, .env files have created a set of problems...


### Problem


Here are **a few common issues** that I’ve seen arise in dev teams:


1. **Security** . First of all, let's just consider the fact that with .env files you are storing completely unencrypted secrets locally on your machine... Most of the hacks these days are done through social engineering or forging employee identity. You can see why having a file with company secrets on your local machine is a terrible idea!
2. **Syncing .env files** . Imagine this: you add a Twillio API-key to .env and happily leave for vacation. As it turns the next day, you forgot to tell your teammates about this new secret, so their code is crashing and neither do they have access to Twillio to generate a new API-key. When your team reaches you, how do you now send them this secret securely to them? (spoiler: many teams still do it via Slack)
3. You **can't control who has access to which secrets** . With .env files, you don't know which teammates or 3rd-party services are able to access which secrets/environments. When a secret needs to be updated, a developer would have to access all the other secrets in a .env file.
4. **Committing .env files to version control** : Developers sometimes forget to git-ignore their files and accidentally end up exposing secrets to bad actors who, by the way, now employ bots to scrape and detect leaked files — this happens to the best of us (even the most senior developers).
5. **Secret versioning** . Imagine you are deploying a new version of the code that also requires an update of the .env file. A few minutes later, you uncover a bug, and the application gets rolled back. "Hold on... what was the previous value we had in the .env file?" 🥲


The larger your project grows, the harder it becomes to sync .env files across your team, environments and infrastructure. In general, you can think of .env files as Notepad++ compared to Google Docs.


### Solution


To solve this, in the past years, there appeared many secrets managers (e.g, Azure Secrets Manager or Vault). That said, most of them can be split into two categories:


1. Too simplistic (which means that they can't scale with you) and closed-source (which means that you can't self-host them, so it won't satisfy compliance and security requirements for many companies). Some of this solutions are by default not even end-to-end encrypted.
2. Compliant and scaleable but cumbersome and frankly overkill to set up and manage (especially Vault which is notoriously hard to manage).


Seeing this gap, we’ve made an open-source end-to-end encrypted solution called Infisical; you can check it out[here](https://www.infisical.com/) . It’s an open-source, end-to-end encrypted platform that developers can use to sync environment variables across their team and infrastructure. It enables devs to:


1. Easily pull and **inject secrets into local processes** with a 10 minute setup.
2. Integrate with **AWS, Vercel, GitHub Actions** , and more.
3. Manage secrets in **Docker and Kubernetes** .
4. **Version** every secret and **rollback** to any version when needed.
5. Track **audit logs** by every member of your organization.
6. **Self-host** if you want your secrets to never leave your own infrastructure.


And everything is packed into a dark-mode dashboard 😎:


### Disclaimer


I hope, after reading this blog, more developers and companies start abide by better security and productivity practices – in particular using centralized secret stores.


While .env files have a lot of disadvantages, I admit that they can be the right solution for some very simple and non-sensitive use cases (in the development environment). Though even then, we made it very easy to pull secerts locally without .env. You just need to download the Infisical CLI, and run 1 command (` infisical run -- \[Your start script\]` )
