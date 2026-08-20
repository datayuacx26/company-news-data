---
schema_version: "1.0.0"
document_id: "5f4b27393364913d924f8fcb0636cedfb2e04e942760508544a24b4d1d7e52fb"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-june-4-2026"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-24T03:05:55.735939+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:95d0ceb6670d5f2c40e2ddbc3f1689b1ee594cfee8db33c811899e990875e2a9"
---

# Changelog - June 4, 2026

This week’s changelog covers releases from May 28 through May 29, including faster project tag updates, sturdier large-upload processing, and more reliable GitHub pull request scans.


## Top 3 features


### 1. Faster project tag editing from the Projects list


Corgea now lets teams update project tags more quickly from the Projects list, with an inline add action, cleaner chip overflow handling, and a themed typeahead for selecting existing tags. That makes it much easier to keep repository organization current without leaving the main project inventory view.


Those tags are not just cosmetic. Corgea’s docs show that project tags can be used to scope Blocking Rules and PR Scan & Comment Rules, so faster tag editing also makes it easier to apply security automation by team, environment, service tier, or application group. In practice, this means teams can keep policy coverage aligned with how repositories are organized, without maintaining long manual project lists.


### 2. More reliable processing for large scan uploads


Corgea improved scan result ingestion reliability for large uploads, reducing memory-related failures during batch processing. This is especially useful for bigger codebases and larger third-party scan imports, where a single upload can contain a substantial amount of source or finding data.


The docs already steer larger projects toward the Corgea CLI and upload-based workflows when web uploads are not a fit, including projects above the 200MB Dropsite limit. This release strengthens that high-volume ingestion path so teams using CLI uploads or report imports can move scans through Corgea more consistently.


### 3. More reliable GitHub App pull request scanning


Corgea fixed pull request scan handling for organization-owned GitHub App installations, so PR scans continue reliably when the GitHub App is installed at the organization level instead of being tied to an individual user. That closes an important edge case for teams that standardize GitHub App access through shared org ownership.


This matters because the GitHub App sits at the center of Corgea’s pull request workflow. According to the docs, the app is what enables PR-triggered scans, security comments on changed lines, check runs, and fix pull request creation. Improving reliability for org-owned installations helps keep those security workflows dependable in the GitHub setups most companies actually use.


## More features and improvements


- Improved GitHub pull request scan handling when GitHub temporarily returns missing pull request data, so scans fail more gracefully and retry on the next push.
- Fixed incremental scan handling when stored fix data is missing, so follow-up scans and reachability updates can continue without failing.
