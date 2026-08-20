---
schema_version: "1.0.0"
document_id: "062d71fd20d018d9d3d02af714061de8d82b43fcf224f005cace4b4683ec2859"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-d854f87d935e"
canonical_url: "https://www.porter.run/changelog/api-audit-logs-cli-and-browser-session-management-short-lived-cloud-credentials-and-secure-cloud-access-for-workloads"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-25T19:39:33.596384+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:aa65dfafdfd6042ba8e63d0a1f86d90bae6445f36f487c6e51b03d79bf07833f"
---

# API Audit Logs, CLI and Browser Session Management, Short-lived Cloud Credentials, and Secure Cloud Access for Workloads

## **API Audit Logs 2.0**


A queryable record of all project-wide activity against the Porter API (including from the dashboard). Results can be filtered based on user, time, action, and resource. This is now the default view under the “Security” tab.


API audit logs are now available.


## ‍


## ‍ **CLI and Browser Session Management**


The Porter CLI now authenticates with short-lived sessions. You can also monitor and revoke all active browser/CLI sessions from one place. Admins can manage these sessions team-wide.


*Action required:* users on older versions of the CLI should upgrade to the latest version (` brew upgrade porter` , or` /bin/bash -c "$(curl -fsSL <https://install.porter.run>)"` ).


Dashboard and CLI sessions can be monitored and revoked.


## **Short-lived Cloud Credentials**


Porter now accesses your cloud through session-based, auto-expiring tokens instead of long-lived access keys. This is already live for AWS and GCP. Support for Azure will be released shortly and all customers using Azure will be notified.


## **Secure Cloud Access for Workloads**


**‍** Your applications can now receive scoped cloud permissions natively, with no persistent access keys to provision or rotate. More information is available in our[docs](https://docs.porter.run/applications/configure/secure-cloud-access) .
