---
schema_version: "1.0.0"
document_id: "4a48be1a6e2bc9b8ad73b6be01426be13165bcaba54330fd80189ceabd283e05"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/insights/best-practices/quarterly-fraud-update-june-2026"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:a3ce0457bb3735ca69947d20966c5a253f385a1c23f8578d2b719872eeb451eb"
---

# June 2026 Fraud Update: Securing Your Secrets and CI/CD from the Supply Chain

Hello again, builders! We’re back to break down the latest fraud and abuse trends we’ve been tackling throughout Q2 2026. A wave of credential harvesting attacks targeting supply chain pipelines has had a wide impact across the tech industry. We’re also seeing familiar pressure on ISVs, as threat actors continue to target their customers. While Twilio works behind the scenes to keep your data locked down, we want to arm you with the insights and steps you need to keep your own customers just as safe.


## Supply chain attacks on the rise


Since March, Twilio has been monitoring a number of supply chain attack campaigns, many led by the threat group[TeamPCP](https://malpedia.caad.fkie.fraunhofer.de/actor/teampcp) . The goal of these attacks is primarily to harvest credentials at scale by targeting company supply chain software. The common tactic across these attacks is that they leverage[GitHub Actions workflows](https://docs.github.com/en/actions/concepts/workflows-and-actions/workflows) to inject[info stealer malware](https://www.malwarebytes.com/blog/threats/info-stealers) into tens of thousands of build pipelines across thousands of companies. When companies include and run these infected package workflows in their own build pipelines, the malware can exfiltrate highly sensitive data such as Twilio Auth tokens and API keys found in environment variables and source code.


Some of the attacks that Twilio has been tracking include:


**March 19, Trivy Scanner Compromise:** Bypassing authentication measures by using credentials that weren’t rotated during an[earlier containment](https://www.aquasec.com/blog/trivy-supply-chain-attack-what-you-need-to-know/) , the threat actor was able to inject malicious code into 75 separate official version tags of Aqua Security's[Trivy GitHub Action](https://github.com/aquasecurity/trivy-action) . Once an affected version of that Action was run against a customer’s own pipeline, the Action would harvest any secrets found in environment variables.


**May 12, Mini Shai-Hulud:** Using CI/CD cache poisoning and[OIDC token hijacking](https://auth0.com/blog/refresh-token-security-detecting-hijacking-and-misuse-with-auth0/) to bypass traditional code artifact[provenance checks](https://docs.github.com/en/actions/how-tos/secure-your-work/use-artifact-attestations/use-artifact-attestations) and infect highly-trusted development packages (172 in total) such as TanStack, UiPath, and others. Once installed on a system, it actively sweeps for specific high-value secrets and attempts to self-propagate.


**May 18, Megalodon:** Hitting over 5,500 downstream GitHub repositories in a single six-hour window, pre-harevested credentials were again used to inject malware into GitHub Actions workflows. Less sophisticated than Mini Shai-Halud, it does not self-propagate but was deployed broadly in a very small window.


This quarter, our security teams have been laser-focused on securing Twilio’s own supply chain. But security is a team sport, so to help you protect your own stack from these same threats, here are two areas where you can harden your supply chain and protect your Twilio secrets today:


### Secure your deployment workflows


- Don’t reference GitHub Actions in your pipelines by[version tag](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository#creating-a-release) (e.g., trivy-action@v0.28.0). Reference the immutable commit SHA instead.
- Limit the checking out or execution of untrusted code from a pull request within a[pull_request_target](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#pull_request_target) workflow; use the unprivileged[pull_request](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#pull_request) event instead.
- Limit outbound internet access from[CI/CD runners](https://docs.gitlab.com/runner/security/#network-segmentation) . Route all egress traffic through a secure proxy that enforces an allowlist or only use an internal artifact repository that limits and scans dependency packages.
- Enforce[cryptographic commit signing](https://docs.github.com/en/authentication/managing-commit-signature-verification/about-commit-signature-verification) (GPG, SSH, or S/MIME). Configure your repository branch protection rules to reject unsigned commits entirely.
- Block direct pushes to main branches. Require mandatory peer reviews from designated[CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners) for any structural pipeline changes (such as files matching .github/workflows/).


### Secure your access keys and limit blast radius


- Never upload your credentials in plain text to a Git repository or write your credentials into your application code.
- Enforce credential rotation across all secrets in your organization and MFA everywhere possible.
- Make sure developers don’t store long-lived, plaintext privileged[Twilio API keys](https://www.twilio.com/docs/iam/api-keys) or[GitHub Personal Access Tokens (PATs)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) in .env files or local shell histories.
- In your GitHub Action yaml file, only add secrets to the execution step in your pipeline that requires them. Avoid global scoping.


As a best practice, keep your integrations secure by vetting third-party tools and vendors before you connect them. Always stick to the principle of least privilege, granting only the exact access needed to get the job done. Finally, run regular audits to revoke access for any stale integrations or users.


## ISV End User Account Takeovers


Similar to previous quarters, we continue to observe threat actors systematically targeting the end users of[Independent Software Vendors (ISVs)](https://help.twilio.com/articles/4402930862747) . Attackers use many methods to breach systems, ranging from brute-force tactics and stolen passwords to advanced session hijacking. Based on our customer interactions, API Auth token or key compromises are the entry point in more than 98% of cases. The downstream damage these actors can inflict is substantial. Once bad actors compromise an end-user account, they exploit your platform's trusted access to initiate unauthorized traffic, often accumulating massive charges and damaging your sending reputation across communication networks.


A multi-layered defense is the most effective way to secure your operations. By integrating Twilio's programmatic end-user isolation using[subaccounts](https://www.twilio.com/docs/iam/api/subaccounts) and automated containment[triggers](https://www.twilio.com/docs/usage/api/usage-trigger) with continuous behavioral detection and mandatory supplemental authentication for critical actions, you can build a more secure, multi-account Twilio organization.


In the next section, we’ll cover how you can use secret managers to more easily implement secret security at scale.


## Secrets management


Twilio invests heavily in security features and capabilities across the Twilio platform to keep you and your customers safe. But remember, **even the best security features and protocols in the world fail if you don’t keep your secrets a secret** .


Twilio recognizes that keeping secrets safe is a challenge many enterprises face as the sheer amount of secrets required to operate modern infrastructure has grown significantly in the past decade. This challenge is conventionally referred to as “Secrets Sprawl” and it should be taken seriously. This is especially true for APIs, which often rely on single factor authentication.


In order to meet this challenge, Twilio is encouraging all customers to take the following steps, which will improve not only their Twilio account security posture but their entire enterprise security posture:


1. Adopt a secrets manager1 and use it to store and manage your Twilio account secrets. These tools enable your enterprise to use secrets while minimizing access to them.
2. Inject your Twilio API secrets into your production and development environments through your deployment pipelines. This reduces the risk of inadvertent leaks and enables zero-downtime secrets rotation.
3. Rotate your secrets regularly. Best practice is to rotate static secrets once every 90 days, but rotating them at all is better than nothing, so rotate manually as frequently as you can while you automate.


Hopefully, these insights can help your team to drive more secure outcomes for your deployments on Twilio. Until next time, *cheers from Twilio!*


---


1 Cloud Provider Secrets Manager Options:[AWS Secrets Manager](https://aws.amazon.com/secrets-manager/) ,[Azure Key Vault](https://azure.microsoft.com/en-us/products/key-vault/) ,[GCP Secret Manager](https://docs.cloud.google.com/secret-manager/docs)
Cloud Agnostic Secrets Manager Options:[HashiCorp Vault](https://www.hashicorp.com/en/products/vault) ,[Doppler](https://www.doppler.com/try-doppler-free) ,[Cloudflare Secrets Store](https://developers.cloudflare.com/secrets-store/) ,[1Password Secrets Management for Developers](https://1password.com/developers/secrets-management)
