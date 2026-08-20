---
schema_version: "1.0.0"
document_id: "48dad60d2a46f4338084af5c30685bae4388b2d6ae5e7505af3d2d6f83aea4e6"
company_key: "yc-doppler"
company: "Doppler"
source_id: "yc-doppler-news-import-86fd2de2d678"
canonical_url: "https://www.doppler.com/blog/oidc-edge-functions"
published_at: null
first_seen_at: "2026-07-25T01:53:43.668103+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:6179a0deb2789d4153ffdf848e70d267c083845257b3edeca6cb0fc9c108f2c5"
---

# Should you use identity or static credentials for edge functions?

If you're building an edge function that needs to write to S3 or query a database, your first instinct is probably to reach for an API key, drop it in an environment variable, and ship it. The setup might work quickly, but it’s also how security debt gets introduced.


On a traditional server, a secret lives in one place you control. At the edge, that same secret gets replicated across dozens or hundreds of edge nodes that exist outside your control boundary.


GitGuardian's[2026 State of Secrets Sprawl](https://www.gitguardian.com/state-of-secrets-sprawl-report-2026)


report recorded approximately 29 million secrets leaked on GitHub in 2025, a 34% year-over-year increase and the largest single-year jump ever recorded. Many of those credentials remain valid long after they're exposed, and while network boundaries on a traditional server at least limit the blast radius, at the edge, there's no VPC, no IP allowlist, and no fence between a leaked key and full access.


## TLDR


This article shows you why static secrets and edge functions are a dangerous combination, how OpenID Connect (OIDC) and Workload Identity Federation eliminate the problem at its root, and how to implement both on the edge platforms you're already using.


## Replacing static secrets with OIDC and workload identity


Workload Identity Federation (WIF) and OpenID Connect (OIDC) replace string-based authentication with identity tied to the workload itself. Instead of giving your function a secret to prove who it is, the function proves its identity, and the downstream service verifies that proof directly.


Benefits of replacing static secrets with OIDC in edge functions


Think of it like the difference between a shared password and a government-issued ID. The password works for whoever has it. The ID is bound to a person, signed by an authority, and expires. Static secrets are the password. OIDC is the ID.


OIDC is an identity layer built on OAuth 2.0. When a workload runs on a supported platform (such as Vercel or an EKS cluster), the platform issues a signed, short-lived JSON Web Token asserting that the request comes from a specific workload, in a specific environment, owned by a specific team. The token is cryptographically signed by the platform's private key, and the downstream service verifies it using the corresponding public key. No shared secret is exchanged, and the token expires in minutes, making it valid only for its intended context.


Workload Identity Federation, on the other hand, is the mechanism cloud providers use to exchange these tokens for temporary credentials. Instead of an IAM user with a permanent AWS_ACCESS_KEY_ID


, you define a trust policy that allows AWS to accept a valid OIDC token from a trusted issuer with specific claims. AWS STS validates the token against that policy and returns short-lived credentials scoped to a role, typically expiring in 15 to 60 minutes.


## Implementing OIDC and workload identity on edge platforms


### Vercel


Right now, you're probably doing something like this:


Those keys are long-lived IAM user credentials replicated across every regional node where Vercel deploys your function. To fix this, Vercel provides native[OIDC support](https://vercel.com/docs/oidc)


through its runtime. When your function runs, Vercel issues a signed JWT that identifies the specific project and environment making the request. You exchange that token with AWS STS for temporary credentials that expire in 15 minutes.


The examples below use AWS, but the same exchange works with[GCP](https://vercel.com/docs/oidc/gcp)


and[Azure](https://vercel.com/docs/oidc/azure)


. You configure the trust once on the provider side and never touch a secret again.


#### Step 1: Create an OIDC Identity Provider in AWS IAM


In AWS IAM, create an OIDC Identity Provider with the following values, replacing \[TEAM_SLUG\]


with your Vercel team's URL slug:


- Provider URL: \[ https://oidc.vercel.com/\](<https://oidc.vercel.com/>)\[TEAM_SLUG\]


- Audience: \[ https://vercel.com/\](<https://vercel.com/>)\[TEAM_SLUG\]


Then create an IAM Role with this trust policy:


The sub claim binds the trust policy to a specific project and environment, ensuring that only that exact workload can assume the role. That means a staging function cannot assume the production role.


#### Step 2: Set the role ARN as an environment variable


After creating the role, copy its ARN from the AWS IAM console (shown on the role's summary page) and add it to your Vercel project:


This is not a secret; it’s a resource identifier that is useless without a valid OIDC token.


#### Step 3: Use the credentials provider in your function


Next, install the AWS SDK along with Vercel's OIDC credentials provider:


You can now use the provider to fetch an OIDC token and exchange it with STS for temporary credentials:


With this setup, you can remove **AWS_ACCESS_KEY_ID** and **AWS_SECRET_ACCESS_KEY** from your Vercel environment entirely. Authentication is handled through short-lived credentials derived from your function's identity. There's nothing to rotate, nothing to audit for leakage, and nothing an attacker can exfiltrate that remains useful.


### Cloudflare Workers


Unlike Vercel, Cloudflare Workers don't currently expose a native workload identity that AWS IAM can trust directly.


If your Cloudflare Worker needs to access AWS services, you'll need to introduce an external identity source that AWS can trust. This is typically either a third-party identity provider that issues OIDC tokens or a long-lived AWS credential used to bootstrap access. At runtime, your Worker uses this identity to obtain short-lived credentials from AWS STS for each request. You can then use a lightweight library like[aws4fetch](https://developers.cloudflare.com/r2/examples/aws/aws4fetch/)


to sign AWS requests at the edge.


The downside to this approach is that it reintroduces the problem workload identity is meant to solve. Trust must still be bootstrapped from outside the Worker.


### Fastify


On a Fastify server, the typical setup is a **.env** file or environment variables sourced at startup:


These get loaded at boot, live in the process environment for the entire lifetime of the server, and end up in different places.


If you're running Fastify on EKS, the fix requires almost no code change. Instead of loading long-lived credentials from environment variables, you[configure IAM Roles](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html)


for Service Accounts (IRSA) and let the AWS SDK automatically exchange the pod's web identity for temporary credentials.


#### Step 1: Set up IAM roles for service accounts on EKS


Associate your pod's Kubernetes service account with an IAM role.


#### Step 2: Remove static credentials from your code


With IRSA configured, the AWS SDK automatically resolves credentials using its default provider chain. You don't need to pass a credential provider or modify your client setup at all:


The AWS SDK also handles credential refresh automatically using the pod’s web identity via its default credential provider chain, so you don’t need to manage the token lifecycle.


## When OIDC is not enough


OIDC solves the problem of authenticating your workload to cloud providers. But not every secret your edge function needs fits that model. Third-party API keys such as Stripe, Resend, Twilio, and OpenAI don't have an STS endpoint for token exchange. They issue a static key, and that's what they accept. The same is true for some database connection strings, webhook signing secrets, and any credentials from services that haven't adopted workload identity.


For these, the answer isn't to go back to **.env** files and **wrangler secret put** . It's to centralize them in a[secrets manager](https://www.doppler.com/demo)


that provides governance, auditability, and rotation in one place. Using a centralized manager instead of platform environment variables gives you:


- **Single point of change:** A secret updated in your secrets manager propagates to every environment that references it. No more updating the same key in five different dashboards and hoping you didn't miss one.
- **Audit trails:** You can see who accessed or changed a secret and when. With platform environment variables, that history doesn't exist.
- **Access scoping:** Developers can be granted read access to staging secrets without ever touching production values. Platform dashboards are typically all-or-nothing.
- **Rotation without redeployment:** Your secrets manager can update a secret and push it to your workloads without requiring a full redeploy, which is the friction that keeps most teams from rotating at all.
- **A consistent pattern across platforms:** Whether you're on Vercel, Cloudflare, EKS, or a GitHub Actions runner, the secrets come from the same place through the same interface.


The right architecture for secrets management at the edge combines OIDC for cloud-provider access with a centralized secrets manager for everything else that still depends on static credentials.


## Wrapping up


The instinct to reach for a static API key is understandable. It's fast, familiar, and easy to ship. But at the edge, that same instinct becomes a liability because secrets are replicated globally, and without traditional boundaries, a leaked credential can't be contained.


OIDC and Workload Identity Federation eliminate this class of problem entirely. There's no secret to rotate because there is no secret. There are no credentials to leak because none are stored. The exposure window collapses from the entire lifetime of your deployment to 15 minutes, and even then, it yields nothing reusable.


If you're managing secrets across Kubernetes workloads or CI/CD pipelines beyond the edge, the same OIDC principles apply, but the operational overhead of configuring trust policies across every environment adds up fast. This guide on[secrets management for ephemeral environments](https://www.doppler.com/blog/secrets-management-best-practices-for-ephemeral-environments)


walks through how to centralize that layer so workloads authenticate with their native identities while secrets are pulled from a single consistent location.
