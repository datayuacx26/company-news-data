---
schema_version: "1.0.0"
document_id: "88d8cdf4db54bf3f348e986191c4b97742388e38f93c1567159a4462807d438b"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-news-import-53e8505a9d97"
canonical_url: "https://about.gitlab.com/blog/curl-removed-from-omnibus-gitlab-fips-packages-in-19-0/"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-25T06:37:24.143169+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:94d4e698148b9dc22319da0a29c6211feaeabd8f28f828683022d3310564b6b9"
---

# curl removed from Omnibus-GitLab FIPS packages in 19.0

Starting with Omnibus-GitLab 19.0 (and the subsequent patch release to existing supported versions), FIPS packages will no longer include a GitLab-built version of curl. Instead, they will use the curl package provided by the customer’s Linux distribution, in the same way that FIPS packages already use the distribution's OpenSSL.


## Why is this change happening?


This change is necessary because curl 8.18.0 deprecated compilation against OpenSSL 1.x, which prevents us from continuing our previous approach on Amazon Linux 2 and AlmaLinux 8 (affecting RHEL 8 customers). GitLab provides most dependencies for Omnibus-GitLab, but in FIPS packages we link to the distribution's cryptographic libraries rather than bundling our own — and we are now extending that model to curl.


For maintainability and security reasons, we are applying this change to all FIPS packages, including distributions with OpenSSL 3.0 or later. All FIPS customers are affected.


## What do I need to do?


### GitLab Self-Managed


GitLab 19.0 will be available starting on May 21, 2026.


> Learn more about the[release schedule](https://docs.gitlab.com/releases/) .


Starting with the 19.0 Omnibus-GitLab FIPS package, the bundled curl will be removed and replaced with the curl provided by the customer's Linux distribution. The customer's GitLab instance will continue to work as expected. This change has no other impact and doesn't require any immediate action.


## Important implication


GitLab will no longer be responsible for shipping security updates to curl specifically in FIPS packages, and it will be up to the customer to keep their own OS's curl up to date to receive fixes/security patches. Scanner findings for curl will now reflect the host OS package rather than a GitLab-bundled version. This is consistent with how OpenSSL is already handled in FIPS environments.


## What do I do if I still have problems?


If you need assistance, please open an issue in the[omnibus-gitlab issue tracker](https://gitlab.com/gitlab-org/omnibus-gitlab/-/issues/new?issue&issuable_template=Bug) .
