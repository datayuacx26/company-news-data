---
schema_version: "1.0.0"
document_id: "3259a65a7d703b003e40cc997d8987fb8dcb76b8a839dbd9caa7b2c9ae244fca"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/github-actions-oss-fork-builds"
published_at: "2023-07-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:78d70af2f69be05396f10c0e8de65963d2e484878a7c123fdf99882de9174084"
---

# Run Depot builds on forks of open-source projects with GitHub Actions

Today we're excited to announce that you can now build Docker images inside PRs opened from forks of your open-source repositories with Depot and GitHub Actions! It allows contributors to your projects to leverage Depot's build performance without sharing your build secrets with forked repositories.


[Following Depot AI, this is the third announcement for Drop Week #01](https://depot.dev/drop-week)


## The problem


GitHub Actions doesn't issue tokens or allow access to secrets for pull requests from open-source forks (specifically, for the` pull_request` event). It is a security mechanism that prevents malicious workflows from stealing those secrets.


But if you couldn't access secrets or issue tokens, there was no way to authenticate with your Depot project, and so no way to run your Docker image builds using Depot.


Today, we see projects use workarounds like this these:


1. If the pull request originated internally (i.e., not a fork), run the Docker image build on Depot
2. If the pull request was external (from a fork), run the Docker image build with` buildx` locally in the workflow
3. Or opt not to build Docker images at all for external PRs to avoid the complexity


Building with` buildx` locally is slow, and it's *especially* slow when building multi-platform images due to the necessary emulation of Arm images. Effectively, maintainers have to choose between build performance and security.


The complexity is annoying, and we wanted to offer something better.


## Depot authentication for forked repositories


We've developed a solution that allows us to offer access to Depot build infrastructure for these external pull request workflows:


1. Issue OIDC tokens from our own issuer to authenticate the workflow, validating all the details the workflow claims about itself
2. Route these "untrusted" workflows to ephemeral build infrastructure, separate from your main layer cache


### OIDC tokens for untrusted workflows


We've built a new authentication mechanism for workflows that run for untrusted` pull_request` events from open-source forks. We are running an[OIDC issuer](https://actions-public-oidc.depot.dev/) that receives claim requests from these workflows, validates their identity, and issues short-lived OIDC tokens describing their details.


The authentication flow goes like this:


1. The GitHub Actions workflow makes a "claim request" to the OIDC issuer with details about the workflow (e.g., the repository, the workflow ID, the run ID, etc.)
2. The OIDC issuer responds with a unique "challenge code" for the workflow to print periodically in the workflow logs
3. While the workflow prints the challenge code, the OIDC issuer watches the build logs using the GitHub API and validates that the claiming workflow is printing the correct challenge code
4. Once the issuer has validated the challenge code, it returns a new OIDC token to the workflow, encapsulating the details about the workflow run inside the token claims


Once the OIDC token is issued, the` depot` CLI can[exchange it for a build token](https://depot.dev/docs/cli/authentication#oidc-trust-relationships) as it would for other GitHub Actions workflows.


We have[open-sourced the OIDC issuer](https://github.com/depot/actions-public-oidc) ! Contributions are welcome.


### Ephemeral builders


Identifying the originating workflow is only the first step. Since these workflows come from open-source forks, they can potentially execute arbitrary code, so we need to isolate them from your main layer cache. Otherwise, they could potentially read that cache's content, or even modify existing cache entries.


So, when open-source maintainers opt to allow Depot builds from forked PRs, those builds run using a new type of builder called an ephemeral builder. Ephemeral builders don't have access to your main project cache. And they also don't have a cache of their own; on shutdown, all files are destroyed.


Ephemeral builders effectively isolate untrusted builds into their own build infrastructure, but still allows them to leverage the performance of Depot builders, especially for multi-platform builds.


## Allowing Depot builds from forked PRs


If you have an open-source project, you can enable access to` pull_request` builds from your project settings under Trust Relationships:


Additionally, we recommend you require approval on GitHub Actions workflows that want to run when a pull request is opened from a public forked repository. This will allow you to validate that the changes are safe before running the workflow on Depot builders. See these[GitHub docs](https://docs.github.com/en/actions/managing-workflow-runs/approving-workflow-runs-from-public-forks) for more information.


## Conclusion


Our new authentication flow and ephemeral builders allow you to leverage Depot for your Docker image builds on pull requests coming from forked repositories.


We're excited to see how this helps open-source maintainers and contributors build and test their Docker images faster and more securely.


Pop into our[Community Discord](https://discord.gg/MMPqYSgDCg) and let us know what you think!


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
