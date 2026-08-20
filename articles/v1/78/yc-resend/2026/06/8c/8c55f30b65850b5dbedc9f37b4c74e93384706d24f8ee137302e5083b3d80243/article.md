---
schema_version: "1.0.0"
document_id: "8c55f30b65850b5dbedc9f37b4c74e93384706d24f8ee137302e5083b3d80243"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/vercel-marketplace-integration"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:7e006fe278b85900f0af6ceb1da26715f1c65d6d0397d905fa39ccc48e53c9cc"
---

# Vercel Marketplace Integration

Today, we're excited to announce the Vercel Marketplace Integration. **Create and manage a Resend account** directly from the Vercel dashboard or the Vercel CLI.


Many builders, creators, and developers today deploy projects on Vercel. The integration makes it easy to get started with Resend without leaving your Vercel workflow and centralizes your billing within Vercel.


Email for developers, now one click away.


## What's new


- Create a Resend account directly from the Vercel dashboard or the Vercel CLI
- A Resend API key is added to your Vercel project's environment variables
- DNS for Resend is automatically configured through Vercel
- Billing is managed through Vercel


## Prerequisites


Before you begin, you'll need two things:


1. A Vercel account with a project.
2. A domain purchased through Vercel.


## How to get started


The integration connects Resend to an existing domain purchased through Vercel.


### 1. Install the integration


You can install the integration from the Vercel dashboard or the Vercel CLI.


To install **from the dashboard** , navigate to the[Resend Integration page](https://vercel.com/marketplace/resend) .


To install using the **Vercel CLI** , run:


```text
vc i resend   -m     domain  =  example.com
```


### 2. Authorize the integration


Walk through the authorization process:


- Connect your Vercel domain.
- Select your desired Resend plan.
- Authorize the integration.


### 3. Connect and verify


The integration can automatically add the required DNS records to start sending from your new Resend account. To direct the integration, follow these steps:


First, connect the integration to a Vercel project.


Finally, verify your domain within the Resend dashboard.


## Empowering developers


Our goal is to empower developers to ship quickly with confidence, taking care of the busy work so they can get back to building the projects they love.


This new integration makes it even easier for Vercel users to get started with Resend, and we can’t wait to see what you do with it.


For more help, view[our Vercel Marketplace Integration docs](https://resend.com/docs/guides/vercel-marketplace-integration) .
