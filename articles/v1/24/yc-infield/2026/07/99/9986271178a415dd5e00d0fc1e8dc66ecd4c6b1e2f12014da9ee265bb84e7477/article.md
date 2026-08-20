---
schema_version: "1.0.0"
document_id: "9986271178a415dd5e00d0fc1e8dc66ecd4c6b1e2f12014da9ee265bb84e7477"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/defining-a-dependency-management-policy"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:4061b882509861caec5ae6e6d781091587c8c2ca419c515b3e3a1d47557fab31"
---

# Defining a Dependency Management Policy

Infield keeps open source dependencies up to date. We do this by combining software and a team of expert developers who will fix breaking changes in order to upgrade your app. We’ve upgraded more than a thousand packages across millions of lines of code. In doing so we’ve come to learn what good (and unfortunately bad) dependency maintenance looks like.


Good dependency management means defining a policy, monitoring for compliance, and prioritizing continuous dependency upgrades as part of your ongoing maintenance cycles. Dependencies need clear owners responsible for their upkeep.


Define your dependency policy as a series of goals. For a typical SaaS web application we recommend:


1. Core frameworks and programming languages should be kept on their latest version. Aggressive teams will stay on the bleeding edge (running main in production) while more conservative ones will upgrade once the first patch version comes out of a new release.
2. Other direct dependencies should be upgraded when they become more than two years stale, measured as the lag between the release date of the version in use and the most recently released version (this is[https://libyear.com/](https://libyear.com/) ).
3. There should be no open CVEs against any of your dependencies.
4. Do not use any dependencies abandoned by their authors.


Monitoring for compliance means tracking various data sources - a security scanner for CVEs, github repos / mailing lists for abandonment, package registries for new releases, and framework web pages for major announcements. This can be done by technical project managers, but we’ve also seen teams write scripts that automatically send summary emails or create Jira tickets when they find work to be done.


Finally, good dependency policy means responsibility. Depending on your size, you might have one or two team members generally responsible for dependency upgrades. When multiple teams are responsible for the same app you need a way to track which dependencies are owned by which teams.


Besides the security and reliability dependency maintenance brings, rewarding this work helps build engineering culture. The best engineers want to work with modern tools.


‍
