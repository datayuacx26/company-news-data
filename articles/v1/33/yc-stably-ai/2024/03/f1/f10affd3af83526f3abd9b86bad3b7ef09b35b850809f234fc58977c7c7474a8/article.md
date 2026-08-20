---
schema_version: "1.0.0"
document_id: "f10affd3af83526f3abd9b86bad3b7ef09b35b850809f234fc58977c7c7474a8"
company_key: "yc-stably-ai"
company: "Stably AI"
source_id: "yc-stably-ai-news-import-4d9fcc9f146f"
canonical_url: "https://www.stably.ai/blog/how-to-use-concurrency-in-github-actions"
published_at: "2024-03-25T00:00:00+00:00"
first_seen_at: "2026-07-22T14:42:49.723725+00:00"
fetched_at: "2026-07-28T22:26:05.625622+00:00"
content_hash: "sha256:990be9ddf4f2aacfd32136f12ac5b0408007210d1543fb5182e78f920724c8a5"
---

# How to use concurrency in GitHub Actions

The concurrency feature in GitHub Actions can be quite powerful, even though many of you are unaware of it. This feature can help solve these two problems in a clean way:


1. **Concurrent deployments** : Suppose there is an auto-deploy job/workflow after merging a PR on the main branch. When two developers merge their PR into main, this leads to two concurrent deploy-to-production jobs on different commits, leading to inconsistencies and outages.
2. **Wasteful spend** : Suppose a developer pushes “commit 1” to their PR and quickly discovers that they forgot to add a comment. They immediately push another commit, “commit 2” to the PR. Jobs running on “commit 1” are still being billed, only to be obviated by the jobs running on “commit 2”.


The concurrency feature in Actions helps you address exactly these problems. The big picture idea is pretty simple: You add a “key” to a workflow or job such that


1. It runs exactly one workflow or job at any given time per unique key
2. It runs the workflow or job with the latest commit


The following snippet can be specified at a workflow or the job level.


```text
concurrency: group: ${{ github.workflow }}
cancel-in-progress: true


```


Breaking this snippet down:


- You specify a group i.e., the “key” I mentioned before to identify the workflow (or job) run uniquely
- You set cancel-in-progress to true to cancel any ongoing jobs in favor of new jobs that will be run on the latest commit This solves 1) and 2) but let’s make it even better.


Consider this example: suppose you use the above concurrency state for a workflow that runs your unit tests. Let’s say Alice pushes a commit to a branch “Alice test” and the unit test jobs begin. While they’re running, suppose Bob pushes a commit to a branch “Bob test”. Since unit test jobs from both Alices and Bob's branches, have the same “key,” Alice’s job would be canceled midway, and only Bob’s job would run to completion.


The recommended solution is to make the “key” include the PR name so it only runs the latest commit within the context of a given PR, and does not hamper runs on other PRs in the repository. github.head_ref is the source branch of the pull request in a workflow run.


```text
concurrency: group: ${{ github.workflow }}-${{ github.head_ref }}
cancel-in-progress: true


```


[Here’s the official Github page](https://docs.github.com/en/enterprise-cloud@latest/actions/using-jobs/using-concurrency) on using concurrency. In our experience, smart use of concurrency is one of the most effective ways of reducing your GitHub Actions bill. Companies can often cut 10% of their spending by effectively leveraging the concurrency feature.
