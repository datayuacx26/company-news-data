---
schema_version: "1.0.0"
document_id: "b2a93edb63b82ab3dbc08d2e3579636030764b28de261b961d05b0dedf310117"
company_key: "yc-buildjet"
company: "BuildJet"
source_id: "yc-buildjet-news-import-6b2f4e2d5236"
canonical_url: "https://buildjet.com/for-github-actions/blog/we-are-shutting-down"
published_at: null
first_seen_at: "2026-07-24T23:34:21.347694+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:14e24fa1e3a73fa84168e742fa574b3b8d7c9a62d2a59c26370a722823bcc85e"
---

# We're shutting down BuildJet for GitHub Actions

We're announcing that **BuildJet for GitHub Actions is shutting down** . As of March 31st, 2026, runners using the BuildJet for GitHub Actions service will no longer be available. You will need to migrate to GitHub's hosted runners before this date.


# Timeline


**Effective today (February 6th, 2026)**


- New signups are no longer accepted
- All concurrency subscriptions are now free - your concurrencylimits remain unchanged
- Pay-as-you-go usage billing continues as normal, until March 31st, 2026


**March 31st, 2026**


- BuildJet stops running jobs - migrate your workflows before this date


# How to migrate# Runners


Update your workflow files to use GitHub's hosted runners:


```text
yaml  1   jobs  :
2       build  :
-          runs-on  :   buildjet  -  2vcpu  -  ubuntu  -  2204
+          runs-on  :   ubuntu  -  latest
5         steps  :
6           -     uses  :   actions/checkout@v4


```


For ARM workflows, GitHub now offers native ARM runners:


```text
yaml  1   jobs  :
2       build  :
-          runs-on  :   buildjet  -  2vcpu  -  ubuntu  -  2204  -  arm
+          runs-on  :   ubuntu  -  24.04  -  arm
5         steps  :
6           -     uses  :   actions/checkout@v4


```


For teams requiring more powerful runners, see[GitHub-hosted runners](https://docs.github.com/en/actions/reference/runners/github-hosted-runners) for available options, or consider[larger runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-larger-runners) and self-hosted runners.


# BuildJet Cache


BuildJet Cache is interoperable with` actions/cache` . Simply swap the action:


```text
yaml  1   steps  :
2       -     uses  :   actions/checkout@v4
-        -     uses  :   buildjet/cache@v4
+        -     uses  :   actions/cache@v4
5         with  :
6           path  :   ~/.npm
7           key  :   $  {  {   runner.os   }  }  -  node  -  $  {  {   hashFiles('  **/package-lock.json')     }  }


```


# Why we're shutting down


When we launched BuildJet, GitHub Actions had significant performance limitations that made CI/CD painfully slow for many teams. Since then, GitHub has made substantial improvements - faster hardware, larger runner options, and native ARM runner support. The gap we set out to fill has largely closed.


We've decided to focus our efforts elsewhere. Thank you to everyone who trusted BuildJet with their builds over the years.
