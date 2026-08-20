---
schema_version: "1.0.0"
document_id: "7e3f8d9bf53a1c959d59ebe731041d395f2a3f4cb18d7ee1055797beb2cce6f1"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/support-for-bitbucket-data-center"
published_at: "2024-02-28T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:7ae18caa00bb9436695bcd0c7f00a4ffc337add484077d835fc6280576727cff"
---

# Support for Bitbucket Data Center

Last updated on Feb 28, 2024


Atlassian's support for its Server products[ended on Feb 15, 2024](https://www.atlassian.com/migration/assess/journey-to-cloud) . If your organization used Bitbucket Server, the self-hosted version, you would need to migrate either to Bitbucket Cloud or Bitbucket Data Center, their refreshed self-hosted offering.


## How we're helping our customers


We shipped our integration with Bitbucket Data Center in January 2024, making the transition easy for our customers who were either still in the planning phase or very early in the migration process.


If you've switched to Bitbucket Cloud, you can use DeepSource Cloud or Enterprise Cloud. For customers of Bitbucket Data Center who are self-hosting, you can also self-host DeepSource Enterprise Server with first-class support for Bitbucket's new APIs.


DeepSource will automatically recognize existing configuration files (` .deepsource.toml` ) in each repository and activate the analysis, so switching is easy. If you're also using DeepSource for tracking code coverage, you will need to update the[Data Source Name (DSN)](https://docs.deepsource.com/docs/analyzers-test-coverage#set-up-deepsource_dsn-environment-variable) for each repository in your CI system. For teams with a large number of repositories, you can retrieve this from[the API](https://docs.deepsource.com/docs/apis-repository#repository) .


## New customers looking to switch


If your current code health platform (static analysis, SAST, IaC analysis, or code coverage) doesn't support Bitbucket Data Center, we'd love to help![Sign up](https://deepsource.com/signup) for a free account and try out the Bitbucket integration for yourself, or[talk to sales](https://deepsource.com/contact/sales) !
