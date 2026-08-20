---
schema_version: "1.0.0"
document_id: "c57e1557b33e6e4d454444ad480db556f3b75f2e1f1b852c52b71f4f961239f6"
company_key: "tucows-inc-class-a-common-stock"
company: "Tucows Inc."
source_id: "tucows-inc-class-a-common-stock-rss-e99aab447650"
canonical_url: "https://storiesfromtheherd.com/the-case-for-terraform-tofu-merge-queue-d2f31392eade"
published_at: "2025-07-17T14:36:17+00:00"
first_seen_at: "2026-07-20T23:20:12.168287+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:65dc412bf6d8916e2ff6d5101198e8db8450d3189bde7ca1c9588fc391976ec2"
---

# Streamlining Infrastructure-as-Code: The Case for Terraform/Tofu Merge Queues

# Streamlining Infrastructure-as-Code: The Case for Terraform/Tofu Merge Queues


## Can we settle the debate on when to apply infrastructure-as-code changes: ***before*** or after merge?


[Rishav Dhar](https://medium.com/@rishavd?source=post_page---byline--d2f31392eade---------------------------------------)


5 min read


·


Jun 26, 2025


--


Every infrastructure-as-code practitioner eventually asks themselves the same question:


> *Should I apply Terraform/Tofu changes* ***before*** *or* ***after*** *merging a pull request?*


In the courtroom of DevOps, it’s a hung jury. And the stakes? Our infrastructure’s stability and team’s sanity. In this blog post, we’ll evaluate the case **for** and **against** each method of applying infrastructure changes, and present our closing argument in favour of merge **queues** . Order in the repo!


## Case #1: Apply after merge


In this model, a provisional` plan` is generated in a pull request (PR) → reviewed/approved by peers → merged into the main branch → runs` apply` .


### ➕ For apply-after-merge


- **Simplicity reigns** : Ideal for large teams using GitOps framework where the main branch is the single source of truth.
- **Tooling-light** : Straightforward continuous-integration/continuous-deployment (CI/CD) pipeline, promoting self-service via automation.


### ➖ Against apply-after-merge


- **Flaky apply** : A successful` plan` can fail to` apply` for a number of reasons (e.g., resource conflicts or quota exhaustion), rendering the “source of truth” undeployable until subsequent PR fixes are applied.
- **Drift risk** : Running` apply -auto-approve` after merge does not necessarily reflect the latest plan output, leading to unexpected changes due to configuration drift since the last plan step.


### Verdict


Simple, but brittle. We fast-track merges at the expense of provisioning confidence, which can be mitigated by staging deployments.


## Case #2: Apply before merge


As before, a provisional` plan` is generated in a PR → reviewed/approved by peers → triggered` apply` in the same PR branch → merged when successful.


### ➕ For apply-before-merge


- **Fail fast** : Early detection of unexpected errors, like quota issues and naming collisions, are addressed before merge.
- **Healthy main** : The main branch is stable by design, always reflecting a deployable state.


### ➖ Against apply-before-merge


- **CI/CD overhead** : Multiple PRs that need to` apply` get in each other’s way, usually needing to manually serialize runs and require additional tooling integration. For example,[Atlantis](https://www.runatlantis.io/) manages PR-locks and triggers` apply` via comment-driven automation.
- **Anti-GitOps** : Not only does the main branch fall behind the current state, it’s often unclear which of the lingering PRs represent the deployed state: leading to more time spent rebasing branches to avoid conflicts.


### Verdict


Safe, but sluggish. We trade developer-velocity for infrastructure safety: not always gracefully.


Press enter or click to view image in full size


## Case #3: Apply with merge queue


GitHub-native since[2023](https://github.blog/news-insights/product-news/github-merge-queue-is-generally-available/) , merge queues improve developer experience by ensuring each PR in the queue is compatible and passing status checks before deploying changes to production at scale.


## Get Rishav Dhar’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


While it’s capable of batching and parallelization, we’re just interested in its ability to` apply` changes to infrastructure as if merged (like` apply-after-merge` ), and only finalizing the merge if successful. Otherwise, the PR remains open for further changes (like` apply-before-merge` ), with the option to rollback to the last healthy deployment from main.


### ➕ For apply-with-merge-queue


- **Source of truth** : While staying up-to-date, the main branch is also representative of the actual deployed state at all times, as` apply` failures are rejected from the merge queue.
- **Native locking** : Instead of trying to control concurrency at the PR level, leverage Terraform/Tofu’s built-in state-locking to prevent simultaneous writes to a backend hosted on any major cloud platform.
- **Self-serviceable** : Merge queue seamlessly integrates with existing GitHub Action workflows, avoiding the need for additional tooling and associated maintenance overhead.


### ➖ Against apply-with-merge-queue


- **Edge cases** : While` apply` failure can automatically trigger` apply -auto-approve` from the main branch, successful rollback isn’t guaranteed. For example, when introducing a new provider.
- **Workflow ownership** : Requires bespoke workflow configuration to: reuse plan output, configure recovery behaviour (if any), and communicate infrastructure changes clearly and securely.


[Enhance Terraform/Tofu Automation with GitHub Action Using OP5dev/TF-via-PR enables low-code workflows to init, plan and apply IaC changes — batteries included. medium.com](https://medium.com/@rishavd/enhance-terraform-tofu-automation-with-github-action-1a775c8b6b61?source=post_page-----d2f31392eade---------------------------------------)


### Verdict


Automated, predictable, and scalable. But we can’t judge on your behalf; it’s up to you to weigh the evidence and decide for yourself. Try our working[GitHub Action workflow](https://github.com/OP5dev/TF-via-PR/blob/main/.github/workflows/test_aws.yaml) here and see how you can implement this into your development process. Our example includes: matrix strategy, concurrency, rollback and even notify-on-failure. To integrate with GitHub environments, use` gh-readonly-queue/main/pr-*` as the branch name pattern and curate it to best fit your use case.


Exhibit: Branch protection rule for merge queue (don’t forget to require status checks!)


## Closing Arguments


The case for merge queue isn’t just strong, it’s compelling. It combines the best of both worlds, while addressing weaknesses of both pre- and post-merge workflows. If you’re ready to pass judgement in favour of stability, consistency and automation: merge queue is your winning case.


### Sources


- *GitHub Blog* , Will Smythe & Lawrence Gripper.[How GitHub uses merge queue to ship hundreds of changes every day.](https://github.blog/engineering/engineering-principles/how-github-uses-merge-queue-to-ship-hundreds-of-changes-every-day/)
- *Terramate* , Sören Martius.[Mastering Terraform Workflows: apply-before-merge vs apply-after-merge.](https://terramate.io/rethinking-iac/mastering-terraform-workflows-apply-before-merge-vs-apply-after-merge/)
- *IaC Insights* , Matt Gowie.[Should you apply before or after merging?](https://newsletter.masterpoint.io/p/apply-merging)


Press enter or click to view image in full size


Author: Rishav Dhar, Senior Engineer II in Platform DevOps at Wavelo.


## About Tucows


We do a lot, but at our core, we’re in the business of keeping people connected and keeping the Internet open. We’re made up of three companies: Tucows Domains, Ting, and Wavelo.


- As[Tucows Domains](http://tucowsdomain.com/) , we help people find their place online as the world’s largest domain name wholesaler and the third-largest domain registrar globally.
- As[Ting Internet](http://tinginternet.com/) , we deliver high-speed fiber internet service to communities across the United States.
- As[Wavelo](http://wavelo.com/) , we believe the future of telecom is event driven. We build telecom billing and operations software for Mobile Virtual Network Operators and Fiber Internet Services.


#JoinTheHerd at[https://www.tucows.com/careers/](https://www.tucows.com/careers/overview)
