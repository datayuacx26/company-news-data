---
schema_version: "1.0.0"
document_id: "056251864993bbf85cabc9fc180f4761533a54ceccde88fbfc67d3383df9fd06"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-in-cursor-origin"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-17T20:10:48.552705+00:00"
fetched_at: "2026-08-17T20:10:51.956634+00:00"
content_hash: "sha256:75ef464ee65f1d4f360fa291bb02bafde77a0ac7744539156fb2e007b8b68232"
---

# Depot CI is now available in Origin

[Origin](https://cursor.com/docs/origin) enters early beta today, and you can now connect Depot to an Origin namespace to run your CI on Depot. Bring your existing GitHub Actions workflows and get faster and more reliable CI jobs without needing GitHub.


## What you get


[Depot CI](https://depot.dev/products/ci) is our own programmable CI engine. It speaks GitHub Actions, so the workflows you already have keep working, but everything underneath is ours: the orchestrator, the compute, the caching, and the APIs.


**Connect from Origin or Depot** : you connect Depot from your repository's **Apps** tab in Origin, and Origin sends you to Depot to finish up. Brand new to Depot? Create your account and your organization right there in the flow. Already use Depot? Sign in and pick the organization to connect.


**Your existing workflows, converted for you** :` depot ci migrate workflows` reads the workflows in` .github/workflows/` and copies them into` .depot/workflows/` , applying Depot CI compatibility fixes as it goes. It shows you which workflows it can take as-is, and anything it can't translate cleanly gets flagged for review. No need to rewrite YAML by hand.


**A command to import your secrets and variables** : import your GitHub secrets and variables with` depot ci migrate secrets-and-vars` , so your migrated workflows run right away. You can manage secrets and variables afterward from the[dashboard or the CLI](https://depot.dev/docs/ci/how-to-guides/manage-secrets-and-variables) .


**Runs triggered by Origin events** : pushes and pull requests in Origin-hosted repositories trigger Depot CI runs. No separate webhook setup, no GitHub round trip.


**Status checks back on the pull request** : CI job results are visible in Origin, inline with your pull requests. Configure your repo with branch protection rules to enforce that CI passes before merging.


**Everything else Depot CI already does** : logs, CPU and memory metrics,[SSH into a job](https://depot.dev/docs/ci/how-to-guides/debug-with-ssh) , retries of individual failures,[custom runner images](https://depot.dev/docs/ci/how-to-guides/custom-images) , and a first-class API and CLI so agents can trigger runs and read results without a git event.


Get a faster and more reliable CI experience without GitHub, and without having to rewrite your existing workflows for another provider. Depot CI connects directly to your Origin repository and is fully compatible with GitHub Actions. We run your workflows on our own infrastructure for better performance and reliability.


If you already use Depot but haven't tried Depot CI yet, the quickstart gets your existing workflows running in minutes.


Quickstart


## Getting started


You'll need a Cursor Pro, Teams, or Enterprise plan with Origin access. Navigate to the[Origin UI](https://cursor.com/codebase) to create a new repo, install the Depot app, and connect it to your Depot account. Don't have a Depot account? We'll help you create one during installation.


### Create a new repo


At launch, Depot works only on Origin-hosted repos, not on repos that are mirrored from GitHub to Origin.


From[cursor.com/codebase](https://cursor.com/codebase) , click **New repo** or **+ New** and follow the steps to create a new Origin repo.


### Connect Depot from Origin


1. In Origin, navigate to the[Depot app](https://cursor.com/codebase/settings/apps/depot) .
2. Review the permissions we ask for and install the app.
3. Origin sends you to Depot to complete the installation. Create a Depot account or sign in if you already have one.
4. Choose the Depot organization to connect. If you need a new one, we'll help with that too.


That's it. You'll start on the Depot CI page in your dashboard, ready for your first run.


Organization owners can also connect from the Depot side: in your[organization settings](https://depot.dev/orgs/_/settings) , under **Origin** , click **Connect to Origin** to kick off the process. You'll get redirected to the Origin UI, where you can confirm the installation before heading back to the Depot dashboard.


### Migrate your workflows


Migration runs from your terminal, in a checkout of the repository you connected. Depot shows you these steps on the last screen of the connect flow, and again on the Depot CI page until your first run. You'll need to be an owner of your Depot organization and be able to push a commit to the GitHub repo from which you copied your source code.


1. Install the Depot CLI on the machine where your repository is checked out.
2. Run` depot login` and choose the organization you connected to Origin.
3. From the repository root, run` depot ci migrate workflows` . It shows you the workflows whose triggers Depot CI supports, asks whether you want to migrate the rest, and then writes converted copies into` .depot/workflows/` . Anything it can't translate cleanly comes through as a disabled job flagged for review.
4. Run` depot ci migrate secrets-and-vars` . It reads the secrets and variables your workflows reference, commits a one-shot import workflow to a temporary branch, and hands you the` git push` command. You have five minutes to push it. The workflow runs once, imports the values into Depot CI, and deletes its own branch.
5. Push` .depot/workflows/` and merge it into your default branch to activate the workflows.


### See your first run


Runs show up on the[Depot CI page](https://depot.dev/orgs/_/workflows) in your dashboard, or you can list them from the CLI:


```text
depot   ci   run   list
depot   ci   status   <  run-i  d  >
```


## Why we did this


We've spent the past three years making the delivery loop faster, one primitive at a time: container builds, then GitHub Actions runners, then caching and registries, then[our own CI engine](https://depot.dev/blog/now-available-depot-ci) . The reason is always the same. AI has moved the bottleneck from writing code to integrating it, and every slow step between commit and production is now the thing holding a team back.


Origin is Cursor's answer to the top of that loop, and it puts code hosting right next to where the code is being written. The question then becomes: What runs my tests? We'd rather that answer be a CI engine built for this volume of code than a queue you wait in.


Depot CI is fully API-driven, which means an agent working in Cursor can trigger a run, read the logs, and act on a failure without a human clicking through a dashboard.


## Pricing


All Depot plans include a bucket of Depot CI minutes. After that, you pay for what you use. See plans and Depot CI pricing on the[pricing page](https://depot.dev/pricing) .


## Learn more


We're using this early beta release to tighten our integration with Origin as much as possible, and we want to hear how it's going for you. Tell us what's working and what you'd like next in our[Discord community](https://discord.gg/MMPqYSgDCg) , or reach out through[Depot support](https://depot.dev/help) .


- [Cursor changelog for Origin early beta](https://cursor.com/changelog/origin-code-hosting)
- [Depot CI overview](https://depot.dev/docs/ci/overview)
- [GitHub Actions compatibility](https://depot.dev/docs/ci/compatibility)


## Related posts


- [Now available: Depot CI](https://depot.dev/blog/now-available-depot-ci)
- [Depot is now available in Stripe Projects](https://depot.dev/blog/stripe-projects)
- [What we need from CI for agentic engineering](https://depot.dev/blog/ci-for-agentic-engineering)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
