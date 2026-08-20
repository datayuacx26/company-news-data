---
schema_version: "1.0.0"
document_id: "9af911907c2b5b1ed8ca695975a100db14ce99cdbb2cf544b6c2ed46c61d9e1d"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/community-analyzers"
published_at: "2024-01-03T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:c9bcec66085c0d5221b49d41a38ecaf620b15cb578f22e3843bbe850c492403c"
---

# Introducing, Community Analyzers

Last updated on Jan 3, 2024


Today, we are excited to announce a significant addition to DeepSource – Community Analyzers, broadening the technologies and languages supported by DeepSource. Community Analyzers are third-party, open-source static analyzers that you can now leverage within DeepSource.


## What are Community Analyzers?


Community Analyzers are open-source third-party static analyzers that are executed as part of your existing CI pipeline and the results are reported to DeepSource using the[OASIS standard](https://docs.oasis-open.org/sarif/sarif/v2.1.0/errata01/os/sarif-v2.1.0-errata01-os-complete.html#_Toc141790657) SARIF (Static Analysis Results Interchange Format) format. Unlike our Core analyzers, Community Analyzers do not run on DeepSource's infrastructure. This approach ensures that you can utilize DeepSource's powerful analysis features and broadens the horizon of technologies and languages you can now analyze using DeepSource. You are no longer limited to the analyzers we provide natively.


The initial release adds support for Kube Linter, Dart Analyze, Slither and AWS CloudFormation Linter, with support for more to come in the near future. To simplify your onboarding experience, all analyzers have pre-configured ready-to-use CI snippets tailored for popular CI providers like Azure Pipelines, GitHub Actions, Circle CI, and more.


## Centralized management & seamless CI integration


Despite running the analyzers externally, you get to manage all issues in one centralized dashboard in DeepSource. This means you can track and address code health issues across different languages and tools without juggling multiple platforms. Moreover, you can leverage all of DeepSource's features with these analyzers. This includes:


- Quality Gates: Set quality standards for your codebase and ensure they are met before merging code.
- Issue Diffing: Only see newly introduced issues in a pull request compared to the main branch.
- Ignore Rules: Fine-tune what issues to track and what to ignore based on your project's needs.


## Getting started


Leveraging Community Analyzers is a straightforward process, mirroring the usage of Core Analyzers, with just one extra step. All supported Community Analyzers can be found in the[Analyzer Directory](https://deepsource.com/directory) . After enabling the analyzer in the repository's` .deepsource.toml` configuration file, use one of the CI configuration snippets which is pre-configured to execute the analyzer and report the results in SARIF format back to DeepSource. Refer to the[documentation](https://docs.deepsource.com/docs/community-analyzers) for detailed setup instructions.


## Contributing new analyzers


All community analyzers supported by DeepSource are maintained in an[open-source repository](https://github.com/deepsourcecorp/community-analyzers) . This opens up new avenues for community contributions. If you would like to add support for a static analyzer on DeepSource, consider opening a pull request with the analyzer's metadata. Refer to the[contribution instructions](https://github.com/deepsourcecorp/community-analyzers?tab=readme-ov-file#adding-a-new-analyzer) for more info.


Community analyzers are available for DeepSource Cloud users across all the plans today, and will be available for Server customers by the end of this month.
