---
schema_version: "1.0.0"
document_id: "cd4a0ab1abe318006e32a1cf1291f8e8725925f964f47b1f79a4d7bcbd6a1255"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/deepsource-sca"
published_at: "2025-04-08T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:9f56ba4a111bb783162aff7b23b84b959b58b2cb833fca18b9fdc74f9905fa0c"
---

# Introducing, DeepSource SCA

Last updated on Apr 8, 2025


We are pleased to announce that DeepSource's latest offering,[Software Composition Analysis (SCA)](https://deepsource.com/platform/sca) is now generally available to all workspaces on DeepSource Cloud. If you're an existing customer, head over to a new` Dependencies` tab that will show up in your repository dashboard and enable analysis on one or more targets — the free plan includes three targets, and you can upgrade to our new[SCA Premium](https://deepsource.com/pricing) plan anytime.


## What is DeepSource SCA?


- DeepSource SCA helps you secure your open-source dependencies by continuously analyzing your package manifests and lock files (called "targets") and showing you all vulnerabilities in the versions you're using, based on sources like[National Vulnerability Database](https://nvd.nist.gov/) and others.
- We use a proprietary algorithm that works on our static analysis engine for deep dependency analysis and correlation with source code to determine reachability of these vulnerabilities in your first-party code. This way, you see all these vulnerabilities in proper context.
- Our multi-variate auto-remediation engine finds all possible upgrade paths for affected packages, so you can indentify the safest way to update without breaking your code.
- Finally, our proprietary Dynamic Risk engine helps you define custom weighting startegies for vulnerabilities dependning on organization's context, so you can go beyond the rigidity of using only CVSS and EPSS scores.


## How is it priced?


DeepSource SCA is priced **per target** , where each target is a combination of a manifest file and a lock file in your repository. Our core offering, SCA Premium, is priced at $8 per target/month billed yearly, and $10 per target/month billed monthly.


Unlike traditional SCA products that usually have opaque pricing and gate on features, we're priced transparently — you only pay for what you're using. You can learn more about our pricing philosophy[here](https://deepsource.com/blog/sca-pricing) .


## Getting started with DeepSource SCA


If you're an existing DeepSource customer, you can get started with DeepSource SCA from your dashboard right away.


If you're not a customer yet,[sign up](https://deepsource.com/signup) with the free plan, or reach out to[sales](https://deepsource.com/contact/sales) .
