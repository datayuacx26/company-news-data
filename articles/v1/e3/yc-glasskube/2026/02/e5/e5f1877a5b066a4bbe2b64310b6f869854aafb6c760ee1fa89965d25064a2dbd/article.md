---
schema_version: "1.0.0"
document_id: "e5f1877a5b066a4bbe2b64310b6f869854aafb6c760ee1fa89965d25064a2dbd"
company_key: "yc-glasskube"
company: "Glasskube"
source_id: "yc-glasskube-news-import-f40b83a58804"
canonical_url: "https://distr.sh/blog/distr-alerts/"
published_at: "2026-02-16T00:00:00+00:00"
first_seen_at: "2026-07-21T21:44:16.826534+00:00"
fetched_at: "2026-07-28T21:57:41.767482+00:00"
content_hash: "sha256:6c3f31419bb4b06a4132718f0084596bf4e4d31139da1dcf3e2bdb28afde205e"
---

# Distr Now Alerts You When Deployments Go Wrong

## The Visibility Gap


For your own application, you’ve got this figured out. Grafana alerts, PagerDuty, whatever. Something breaks in production, you know about it in minutes.


For[self-hosted customer deployments](https://distr.sh/glossary/self-hosted-software/) ? Nothing. Radio silence unless they open a ticket.


You have all this sophisticated monitoring for your own infrastructure, but the moment your software crosses over to customer-controlled environments, you’re flying blind. You find out things are broken when your customer tells you they’re broken. Sometimes days later.


You can’t SSH into every customer deployment and tail logs. That doesn’t scale, and frankly, your customers probably wouldn’t let you even if you wanted to.


## What We Built


Alerts in Distr work for both sides:


Distr lets you configure alerts in the[Vendor portal](https://distr.sh/docs/vendor-portal/) . Set them once, they apply everywhere. Or get granular if you want.


For customers that manage and update the[deployments](https://distr.sh/docs/agents/) themselves, you can enable alerts for them as well so they can configure their own alerts in the[Customer portal](https://distr.sh/docs/platform/customer-portal/) .


This dual approach matters because not all customer relationships look the same. Some of your customers want you monitoring their deployments. They’d rather you catch issues first and fix them proactively. Others prefer to run their own ops and want full control over who gets notified when.


For example, you’ll get alerted if an update fails or gets stuck looping, if we lose contact with a deployment (network issues, dead instance, whatever), or if your app’s in a crash loop. We filter out normal restarts from successful updates though, so you won’t get spammed about routine operations.


The details on setting this up are in the[alerts documentation](https://distr.sh/docs/agents/alerts/) .


## How It Helps


The main thing is you catch issues before your customer does. That’s worth a lot.


We had one customer who didn’t realize their deployments were in a crash loop for two days because the app was restarting fast enough that it looked “up” when they checked. With alerts, that would’ve been caught in the first 10 minutes.


## Getting Started


If you’re already using Distr, alerts are available now. Vendor settings for your side, customer portal for theirs. Takes about 30 seconds to configure.


If you’re not using Distr yet and you’re dealing with[software distribution](https://distr.sh/glossary/software-distribution-platform/) to[on-prem](https://distr.sh/glossary/on-premises-definition/) or customer-controlled infrastructure… well, we should probably talk.[Try it here](https://distr.sh/onboarding) .


## Join the Conversation


We’d love to hear your thoughts on this feature. Share your feedback, ideas, or questions:


- [Join the discussion](https://github.com/distr-sh/distr/discussions)
- [Book a demo call](https://cal.glasskube.com/team/gk/demo?duration=30)
