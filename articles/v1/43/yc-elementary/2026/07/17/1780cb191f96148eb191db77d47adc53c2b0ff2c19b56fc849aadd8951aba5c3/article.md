---
schema_version: "1.0.0"
document_id: "1780cb191f96148eb191db77d47adc53c2b0ff2c19b56fc849aadd8951aba5c3"
company_key: "yc-elementary"
company: "Elementary"
source_id: "yc-elementary-news-import-909d1fa3bc2c"
canonical_url: "https://www.elementary-data.com/post/security-incident-report-malicious-release-of-elementary-oss-python-cli-v0-23-3"
published_at: null
first_seen_at: "2026-07-25T02:31:24.735806+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:040abd8b9d878c352a6488177ba4af829c773c9eaa680423557dd525bac2184b"
---

# Security Incident Report: Malicious release of Elementary OSS Python CLI v0.23.3

## **Summary (TL;DR)**


On April 24, 2026 at 22:20 UTC, a release of the Elementary Open Source Python CLI, version **0.23.3,** contained malicious code. It was published to PyPI, and to a Docker image pushed to our registry.
‍ **These artifacts were not produced by the Elementary team.**


The attacker opened a PR with malicious code, and exploited a script-injection vulnerability in one of our GitHub Actions workflows to publish it as release 0.23.3.
‍ **Users who installed 0.23.3, or who pulled and ran the affected Docker image, should assume that any credentials accessible to the environment where it ran may have been exposed. ‍**
‍ **Elementary Cloud and the Elementary dbt package and were not affected, and no other versions of the CLI were affected.**


## **Timeline**


All timestamps are in UTC.


- **April 24, 22:10** — Attacker posts a crafted comment on a pull request. The comment body is interpolated into a shell context by a vulnerable Github Action workflow, executing arbitrary code on our CI runner with access to workflow secrets.
- **April 24, 22:13–22:17** — Attacker uses our GITHUB_TOKEN to create three branches and three pull requests as preparation for the release.
- **April 24, 22:20** — Attacker triggers our release.yml workflow via workflow_dispatch.
- **April 24, 22:20:47** — Malicious elementary-data 0.23.3 is published to PyPI.
- **April 24, 22:24** — Malicious Docker image is pushed to our registry.
- **April 25, 6:18** — A[Github issue](https://github.com/elementary-data/elementary/issues/2205) reporting the malicious release is opened by[crisperik](https://github.com/crisperik) , and[Henri-Maxime Ducoulombier](https://github.com/H-Max) reaches out to[report on our community Slack](https://elementary-community.slack.com/archives/C02CTC89LAX/p1777098043136579) .
- **April 25, 8:14 -** Elementary initiated security investigation and response.
- **April 25, 8:51-11:51** - Elementary team conducted the following urgent remediation actions:


- Removed 0.23.3 from GitHub, PyPi, and the Docker image from the registry.
- Removed vulnerable Github Actions, deleted the attacker PRs and branches.
- Rotated CI credentials (Github,PyPi, etc.), and as additional safety measure also credentials to additional services. Implemented new authentication methods where possible.
- Reported the attacker account to GitHub.
- Released 0.23.4, a clean safe version.


## **Who was impacted?**


Only users who downloaded and executed version 0.23.3 of the Open Source Python CLI.


There is no impact at all on all Elementary Cloud users, the Elementary dbt package, and all other versions of the CLI.


## **What should you do?**


**If you did not install version 0.23.3 of the Python CLI and did not pull our Docker image during the affected window (April 24 22:10 to April 25 9:45 UTC) no action is required.**


All other versions of the CLI, the Elementary dbt package, and Elementary Cloud were not affected.


If you installed elementary-data 0.23.3, please take the following steps:


1. Check your installed version:


` pip show elementary-data | grep Version`


2. If the version is 0.23.3, uninstall it and replace with the safe version:


` pip uninstall elementary-data`


` pip install elementary-data==0.23.4`


In your requirements and lockfiles, pin explicitly to elementary-data==0.23.4.


3. Delete your cache files to avoid any artifacts.


4.Check for the malware's marker file on any machine where the CLI may have run: If this file is present, the payload executed on that machine.


` macOS / Linux: /tmp/.trinny-security-update`


` Windows: %TEMP%\\\\.trinny-security-update`


5. Rotate any credentials that were accessible from the environment where 0.23.3 ran - dbt profiles, warehouse credentials, cloud provider keys, API tokens, SSH keys, and the contents of any .env files. CI/CD runners are especially exposed because they typically have broad sets of secrets mounted at runtime.


6. Contact your security team, to hunt for unauthorized usage of exposed credentials. The relevant IOCs are at the bottom of this post.


## **Actions and Remediations by Elementary**


In our response to protect our users and our systems, these are the measures we have **already taken** :


- Removed the malicious 0.23.3 release from Github, PyPI, Docker registry, and released a safe 0.23.4 version.
- Rotated the PyPI publish token, Github token, Docker registry credentials, and all other secrets that were available to the affected workflows.
- Removed the vulnerable Github Action workflow, and audited every other GitHub Actions workflow in our organization for the same class of script-injection vulnerability.
- As additional safety measure, revoked all existing user access tokens to Github, PyPi, AWS and other critical services, and created new access tokens. Where possible, we moved to OIDC authentication.
- Hardened our open source release flow and permissions model, including a search of additional vulnerabilities in Github Actions.
- Reported the attacker account and the externally hosted payload URL through GitHub's and the hosting provider's abuse channels.
- Validated the attack only impacted a specific version of Elementary open source Python CLI, and that there was no impact to Elementary Cloud or open source dbt package.


These are ongoing measures we are **currently taking** :


- Collaborating with a leading cybersecurity firm,[Wiz](https://www.wiz.io/) , to conduct a thorough investigation and fortify our defenses against potential future attacks.
- Hardening additional workflows, also unrelated to the CI or release process.
- Enhancing the monitoring and alerting systems to detect and respond to unusual activities swiftly.


## **Conclusion**


We deeply regret the disruption and concern this incident has caused our community. The trust you place in our open-source tooling is something we take seriously, and we are committed to being transparent about what happened, what we know, and what we are doing about it.


We will update as our investigation progresses, and share measures we are taking to prevent it from happening again.


Should you have any concerns or questions, or have indicators of compromise to share, please do not hesitate to contact us atsecurity@elementary-data.com .


Sincerely,


The Elementary Team


‍


‍


### Acknowledgements


- We want to thank two members of the Elementary community who were quick in notifying us of the incident:[crisperik](https://github.com/crisperik) who identified that the v0.23.3 release contained malicious code (similar to the recent litellm compromise) and opened issue #2205, and[Henri-Maxime Ducoulombier](https://github.com/H-Max) who posted to the Elementary community Slack.
- We also want to credit and thank[smiotani-aeyesec](https://github.com/smiotani-aeyesec) ,[nekros1xx](https://github.com/nekros1xx) and[darryk10](https://github.com/darryk10) for their security advisories.
- We thank the research team at Wiz for their rapid response and professional support.


### Indicators of Compromise


Compromised PyPI package: elementary-data==0.23.3
Injection file: elementary.pth
C2 / exfiltration domain: igotnofriendsonlineorirl-imgonnakmslmao.skyhanni.cloud
Persistent execution marker: $TMPDIR/.trinny-security-update


‍
