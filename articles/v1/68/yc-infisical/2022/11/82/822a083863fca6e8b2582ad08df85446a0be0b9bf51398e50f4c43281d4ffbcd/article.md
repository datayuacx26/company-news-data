---
schema_version: "1.0.0"
document_id: "822a083863fca6e8b2582ad08df85446a0be0b9bf51398e50f4c43281d4ffbcd"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/astrazeneca-leaked-secrets-api-keys"
published_at: "2022-11-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:e1f08277ea69a1b744eb03569a86eaebbaab9499ea24cab2b854c5afe97737f7"
---

# Astrazeneca leaked its secrets. What should you know?

In this blog, we touch on what happened at AstraZeneca, what the broader context is around leaking secrets, and remedies for what you can do as a developer to not run into the same problem.


## AstraZeneca


Yesterday, AstraZeneca confirmed that they leaked sensitive credentials to GitHub, exposing patient data connected to their Salesforce cloud environment. The underlying cause—Human error from exposing developer credentials (known as secrets) online and forgetting about it for more than a year.


Although the leaked credentials only gave access to AstraZeneca’s test environments with limited patient data, the whole situation serves a valuable lesson.


## Leaks


As rookie as it sounds, exposing secrets happens to companies of all sizes from Samsung to Clearview AI to even Insomnia Cookies due to human error. For less tech-savvy readers, here’s how it happens:


Sometimes, developers hardcode secrets which means embedding exact values into source code; this is a bad practice that should be avoided at all costs. Other times, developers forget to git-ignore .env files designed to store their secrets during development. These files can be used but should only be used locally. In both cases, developers end up committing secrets to public repositories. Then all it takes is for a bad actor to scoop up your secrets with a trained program to start exploiting systems.


## Solutions


Now there are a few debatable ways to solve this issue:


The least ideal way would be to encrypt and commit .env files to public (ideally private) repositories. Even with strong encryption, this is method is suboptimal because it implies baking secrets permanently into version control. A better way would be to use a .gitignore file to tell git to not commit .env files to source control. This method is most-commonly used but falls short when developers forget to git-ignore their files to begin with. The ideal way would be to use a secure secret manager. These platforms/programs work to store and inject secrets back into developers’ programs, thus avoiding any leaks entirely. The only challenge here might be to set up the secret manager to operate well with a company’s workflow and infrastructure.


## Secret Managers


So, why are we still defaulting to .env files and not adopting secret managers everywhere? The short answer is that .env files are much simpler and free to use than secret managers that can come with a steep learning curve and some dollars. That said, depending on what secret manager you use, there can be great benefits worth considering:


- Injection: Secret managers can inject secrets directly into programs in development and production, addressing the problem with .env files that get leaked.
- Truth & Reliability: Being a source of truth for a company’s secrets, the right secrets can easily be synced to developers of that company during development. This means producing more reliable software that doesn’t crash due to missing secrets and avoiding sending secrets over Slack to stay in sync (the typical case with .env files).
- Rotation: Secrets can easily be rotated and updated across an entire infrastructure.
- Permissions: Secret managers can permission secrets to the right parties for different stages of a software lifecycle.
- Rollback & Logging: Secret managers can version secrets and rollback to previous versions if necessary. They can store logs of who accessed which secrets.


While there are many secret manager options out there like Hashicorp Vault, AWS Secret Manager, GCP Secret Manager, we, of course, advocate for Infisical. It’s simplicity makes it suitable for both personal and enterprise use-cases, and it’s also completely free for teams under 5 people. You can check it out[here](https://www.infisical.com/) .


That said, the AstraZeneca case highlights the need to make safer development practices around maintaining secrets more widespread so that secrets stop getting leaked, and I hope this article shed light on how to improve developers’ secret management practices.


If you have any questions or would like to discuss how you can improve secret management practices, feel free to shoot me an email attony@infisical.com !
