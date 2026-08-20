---
schema_version: "1.0.0"
document_id: "ce11a2aa8658c4f2f1a42a81141f0d2163e5affcf5d03570531af6e887f1108c"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/bun-environment-variables"
published_at: "2023-09-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:1685a7b5f64454623b4a1394b937afaba345ecc706e7119b401f64e17d996c28"
---

# Guide to Environment Variables in Bun

## What is Bun again?


Bun is an extremely fast, all-in-one toolkit for running, building, testing, and debugging JavaScript and TypeScript – from a single file to a full-stack application.


The best thing about Bun is its speed. It's way faster than both Node.js and Deno, which makes it pretty compelling for edge computing.


Since environment variables are very commonly used to store configurations and secrets in Bun, we decided to explore how to use environment variables in Bun.


## Reading environment variables


Environment variables in Bun can be called in two equivalent ways:


1. The traditional Node.js way


```text
process.env.SECRET


```


1. The new Bun way


```text
Bun.env.SECRET


```


In addition, you can run` bun run env` to show all the secrets in the current process.


## Injecting environment variables


Depending on your use case, you might want to choose one of the following options:


### 1. Injecting secrets from a .env file


Unlike older Node.js version, .env files are supported natively – you do not need to download the` dotenv` package.


Environment variables will be pulled from the following files listed in the orger of increasing precedence:` .env` ->` .env.production, .env.development, .env.test (depending on value of NODE_ENV)` ->` .env.local`


In other words, after adding a` .env` file with the following secrets:


```text
FOO=BAR
SECRET=TEST


```


You will be able to read their values with the following code:


```text
console.log(process.env.FOO)
console.log(Bun.env.SECRET)


```


### 2. Injecting using a secret manager like Infisical


.env files could work well for individual developers with a small number of secrets and fairly simple infrastructure. However, when your team starts growing, code complexity increases, and you need to make sure the highest levels of security and compliance, you should consider a secret manager like[Infisical](https://infisical.com/) .


To inject secrets locally in Bun apps, you can use Infisical's CLI:


1. Add secrets to using[Infisical Dashboard](https://app.infisical.com/) .
2. [Install the CLI](https://infisical.com/docs/cli/overview) .
3. Initialize the project in CLI.


```text
infisical init


```


1. Run` infisical run -- bun run dev` –> the latest version of your secrets will always be pulled automatically.


#### What are the benefits of using Infisical?


- None of the secrets are stored locally – secrets are end-to-end encrypted.
- Manage multiple environments in one location: local, staging, production, and more.
- You always have the latest version of your environment variables – no need to sync .env files with your teammates.
- You can integrate Infisical easily across your infrastructure and various 3rd-party tools (GitHub Actions, CircleCI, Docker, Kubernetes, etc.).
- Secrets are version-controlled with the ability of point-in-time recovery.
- Automatically prevent hardcoded secrets and scan for secret leaks.


You can learn more about this process as well as other great features on[Infisical's website](https://infisical.com/) .
