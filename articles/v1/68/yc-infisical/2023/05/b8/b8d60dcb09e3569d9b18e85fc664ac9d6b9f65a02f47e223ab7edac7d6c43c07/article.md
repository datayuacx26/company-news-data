---
schema_version: "1.0.0"
document_id: "b8d60dcb09e3569d9b18e85fc664ac9d6b9f65a02f47e223ab7edac7d6c43c07"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/how-to-minimize-risk-from-secret-leaks-and-establish-robust-breach-prevention"
published_at: "2023-05-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:f913d941b68704f4da885e7cab9f9b063dfbe47849b2e9071aa4b686ae3b4e6d"
---

# How to Minimize Risk from Secret Leaks and Establish Robust Breach Prevention

Secret leaks in both open source and proprietary code repositories are reaching alarming levels. Almost every day, there are attack reports targeting companies that had their credentials, tokens, or API-keys keys discovered within code repositories.


According to the recent[report of the state of secret sprawl](https://blog.gitguardian.com/the-state-of-secrets-sprawl-2023/) by GitGuardian, in 2022, there were over 10 million secrets leaked across GitHub repositories, while 1 in 10 developers has exposed a secret at least once. In fact, platforms like GitHub and GitLab have now become a target for malicious actors who are setting up bots to automatically crawl repositories. You can test it yourself – if you commit a secret to a public git repository, it will be picked by bots within a few seconds, and you won’t even realize until you get a gigantic bill for OpenAI or some other service.


According to another report by ReversingLabs, they have identified over 112,000 exposed secrets within packages in the npm repository, 30,000 in the Python Package Index (PyPI), and 10,000 in the RubyGems open source repository. The affected packages primarily center around popular platforms like Microsoft's Azure, Google Cloud, GitHub, Slack, and Salesforce. Some other types of leaked credentials include OAuth tokens, JSON Web Tokens (JWT), and numerous others.


The risks are obvious! The main question is how to prevent becoming a victim and how to effectively address situations where sensitive information unintentionally ends up in public repositories or becomes exposed. Luckily, software development teams have a number of options when it comes to preventing/remediating secret leaks and effectively managing risks in both short and long term.


## Immediate Prevention and Remediation


Tools for secret scanning and preventing leaks are widely available. For example, GitHub provides secrets detection them as part of their Advanced Security Package. In additional there is a number of free open-source tools available such as Infisical that specifically search for hardcoded authentication tokens, environment variables, api keys, encryption keys, etc.


To directly prevent secrets from leaking into git, developers should consider setting up pre-commit hooks that will block developers' commits if any hardcoded secrets are detected in those. In for any reason, there already are hardcoded secrets within the codebase, developers should consider setting up continuous monitoring systems which will automatically notify them about leaks that occurred (e.g., via email, Slack, or a tool like PagerDuty).


It's worth noting that secrets scanning tools can often generate a high number of false positives. They may reveal numerous leaked credentials, including placeholders or decoys that hold no value for potential cyber adversaries. For this reason, tools like Infisical employ a number of features that help prevent false positives (e.g., setting up custom rules for certain types of secrets or verifying if a secret is indeed legitimate).


## Rotate exposed secrets


Even when secrets leaks are promptly addressed, they still warrant concern. Malicious actors do not need extensive time to discover and exploit stolen secrets. For example, in the CircleCI secrets leak, which lasted only a few weeks, a significant number of customers became victim to attacks after their secrets and credentials were stolen from private code repositories hosted on the CircleCI platform, according to the company's statement.


For this reason, it is important to immediately rotate the secrets that had a potential to be exposed. If a secret is obtained from a 3rd-party service, it is recommended to immediately reach out to them notifying them of potential impact and inquiring for activity logs related to any specific secret. Lastly, it is crucial for organizations to take the obtained activity logs and determine whether the activity that has occurred is authorized, suspicious or malicious.


## Manage your secrets going forward


Regardless of whether the exposure of development secrets is discovered through an audit or in response to a leak, your organization will inevitably need to progress beyond the initial phase of situational awareness and crisis management. Subsequently, the emphasis should shift towards responsibly managing development secrets moving forward. In addition to what was discussed above, you should consider doing the following:


### 1. Promote developer best practices — and monitor compliance


Due to the demanding nature of modern, agile development organizations, developers often face strong incentives to overlook issues like stored secrets, which may seem like distant risks to them.


It is crucial for DevOps organizations to acknowledge these negative incentives and encourage best practices that mitigate the security risks associated with secrets. Even seasoned developers, when faced with delivery pressure, can inadvertently make errors or lapses in judgment that lead to the exposure of credentials, tokens, and other sensitive development secrets.


### 2. Establish end-to-end security


Lastly, it is important to ensure that efforts to detect and prevent secret leaks go beyond mere code scanning and encompass the entire development pipeline. There may be misconfigurations in CI/CD tooling or oversight by developers that lead to the inclusion of secrets during the final builds, even when DevOps teams have taken precautions to safeguard them.


Furthermore, organizations should consider implementing binary analysis as an integral part of the standard build-and-release pipeline. This practice enhances visibility into leaked secrets, extending beyond examining raw code to include software packages and containers that are ready for release. By doing so, this capability can uncover secrets that have been inadvertently disclosed due to build or packaging errors, which may otherwise go unnoticed during a code audit.


### 3. Adopt a holistic secret management solution


[Secrets management](https://infisical.com/blog/secrets-management-complete-guide) is a fairly fractured industry. Products like Infisical will help you fight[secret sprawl](https://infisical.com/blog/what-is-secret-sprawl) and unify different parts of secrets management into a single tool. From here on, you will be able to:


- automatically provision secrets for local development;
- establish secrets detection and leak prevention policies;
- set up continuous monitoring for hard-coded secrets in your code;
- orchestrate secrets across your CI/CD and production infrastructure;
- set up access management and proper approval request workflows.


It takes just 5 minutes to get started, and you can learn more here.
