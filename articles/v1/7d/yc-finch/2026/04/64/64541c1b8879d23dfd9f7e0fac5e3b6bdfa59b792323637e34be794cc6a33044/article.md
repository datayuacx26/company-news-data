---
schema_version: "1.0.0"
document_id: "64541c1b8879d23dfd9f7e0fac5e3b6bdfa59b792323637e34be794cc6a33044"
company_key: "yc-finch"
company: "Finch"
source_id: "yc-finch-news-import-d853ae2d7ef6"
canonical_url: "https://www.tryfinch.com/blog/under-the-hood-auth-fragmentation-and-connection-health"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-24T08:01:05.068281+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:9874886c4c2d22709bacb8cf307040724e457ddb6641c7d6877ec1fd6271883e"
---

# Under the Hood: Auth Fragmentation and Connection Health

*This post is part of Under the Hood, a series from Finch's product team on the real engineering and product challenges behind unifying payroll data at scale.*


Before data can flow through any API, an employer has to authorize access to their system. The way employers grant access to their payroll data varies dramatically from one provider to the next, and the challenge of managing that variance at scale is often overlooked.


Authentication is something the Finch product team thinks about constantly—both in terms of establishing the initial employer connection and in maintaining it over time. In this blog, I’ll explain how my team approaches the employer auth flow, how we make the process as smooth as possible for employers, and the steps we take to make sure those connections stay healthy.


## Auth methods: OAuth, API keys, and everything in between


There’s virtually no standardization across payroll providers, and auth methods are no exception. Payroll providers have a wide spectrum of rules and logic for authorizing data access.


Some providers offer OAuth, the gold standard for third-party authorization. It’s clean, well-documented, and relatively straightforward to implement. But OAuth is the exception, not the rule. Across the payroll ecosystem, we encounter providers that require API keys provisioned through a developer portal, others that use dedicated service user accounts with specific permission scopes, and still others that gate API access behind marketplace partnership agreements that can take weeks or months to secure.


This means there’s no single auth flow you can build once and reuse. At Finch, we maintain a library of provider-specific authentication logic—hundreds of bespoke implementations, each reflecting the particular way a given provider handles authorization. Every provider’s integration is different, and it starts with the authentication layer.


## Finch Connect: Making auth accessible for every employer


That’s a high-level look at establishing the initial connection, but just as important is the employer experience and keeping connections healthy.


The person connecting their employer’s payroll system to your app might be an HR admin, an office manager, or a small business owner who’s never configured a third-party integration before. The auth experience has to work for all of them, regardless of how comfortable they are with the underlying technology.


We’re especially mindful of the many different employer personas, which is why we have[Finch Connect](https://demo.tryfinch.com/) : an embedded auth flow that walks the employer through the connection process, specifically tailored to each provider. For some systems, that means a straightforward OAuth redirect. For others, it means step-by-step instructions, video walkthroughs, and guided flows that walk the employer through creating a service account or configuring permissions. The underlying auth method might be complex, but the employer’s experience shouldn’t reflect that complexity.


We measure success here in concrete terms: conversion rates through each step of the Connect flow, time-to-authenticate, post-error drop-offs, and support ticket volume. When employers struggle with a specific provider’s setup, we can see it in those metrics and respond by rewriting instructions, adding a walkthrough video, or redesigning the flow for that provider entirely.


The goal is to abstract as much complexity away from the employer as the system allows. Regardless of whether the underlying connection is OAuth, credential-based, or uses API keys, the employer should feel guided, not confused.


## Keeping the connection alive


Once the connection is established, maintaining it is an ongoing engineering challenge. There are dozens of reasons a connection could be interrupted, from upstream changes (the provider deprecated an endpoint) to organizational shifts (the employee whose credentials were used to establish the connection leaves the company). Any one of these events is a potential disruption that could force the employer into a re-authentication flow if left unaddressed.


At Finch, we treat connection health as a formal operational discipline. We track connection lifespan across every provider and hold ourselves to a clear SLO with a dedicated on-call rotation. Engineers on the Connections team monitor auth session failure rates, data sync health, and provider-specific alerts daily. This proactive monitoring helps our team to catch most upstream changes at the provider level so we can absorb the impact before it surfaces for our users.


We also give our customers tools to monitor their own connections. Finch’s dashboard surfaces granular connection statuses, flagging when re-authentication is needed, whether permissions are insufficient, or whether an account setup step was missed. Any disruption, no matter the cause, impacts the employer’s experience. So while we handle the monitoring, maintenance, and remediation on the platform side, we also give our customers visibility into what’s happening to make the whole system more resilient.


The maintenance burden of self-built integrations is invisible until it suddenly becomes the thing that consumes your engineering team. Finch absorbs the work of keeping every connection for every provider healthy at scale.


## Why auth is the foundation of the unified model


All the benefits of a unified API, from[data normalization](https://www.tryfinch.com/blog/under-the-hood-challenge-and-value-of-unifying-payroll-data) to[deduction mapping](https://www.tryfinch.com/blog/under-the-hood-from-unifying-data-to-powering-workflows) to[opinionated data models](https://www.tryfinch.com/blog/under-the-hood-opinionated-data-modeling-case-study) , depend on the connection itself. Without a reliable connection to the employer’s payroll account, there’s no data to work with.


Finch’s investment in breadth of connectivity across every auth method, including systems with no API at all, is what makes the unified data model possible at scale. The auth layer is invisible to the end users, but it’s the first and hardest problem to solve.


You can learn more about our authentication methods in our[developer docs](https://developer.tryfinch.com/integrations/field-support) , or or[sign up for our sandbox](https://dashboard.tryfinch.com/signup?) to explore the API for yourself.
