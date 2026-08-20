---
schema_version: "1.0.0"
document_id: "7c792d3a17e9f986389d53018325fb7c094b2800825d66b77c369cef56a2e58b"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/from-zero-to-secure"
published_at: "2024-02-28T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:15c514c8a6ec2bf03323fd9fbf6906c8eebaa8d1e082613d564e42de0a8bc1b0"
---

# From Zero To Secure

Last updated on Feb 28, 2024


Picture this: You work in application security and have been tasked with adding automated security checks in your team's development pipeline. Security automation is always a good thing, so it's exciting! The problem, though, is that your team has never had any code analysis tools running on the code base, so there'd be a mountain of existing technical debt. You also cannot block development when you introduce a new check in the pull-request workflow, as developers will need some time getting used to these checks and definitely wouldn't appreciate all their new changes being blocked.


If this situation sounds familiar, it's because it is! A recent survey of application security teams showed that the biggest roadblock in the implementation of a new automated security in the pull request workflow (fancily called "shifting-left") is concerns around a progressive rollout without disrupting development.


It's true. Implementing Static Application Security Testing, or SAST, for the first time can be a daunting task for engineering and security teams. The problem becomes substantially amplified with the size of the team and the age of the code base.


When we build DeepSource Enterprise, this was one of our primary problems to solve for. Large engineering organizations with hundreds of developers, such as Visa, Ancestry, and Sainsbury's trust DeepSource to ship clean and secure code across hundreds (and in some cases, thousands) of repositories. Here are 5 things DeepSource has that allowed these companies to have a progressive rollout.


## Baseline Analysis


When implementing DeepSource's SAST analysis for the first time on a repository, it establishes a baseline of existing security issues found in the code. This baseline tracks the default branch (usually` master` or` main` ), and automatically updates every time new code is merged.


This means two things: all existing issues in your code base are there for you to triage and slowly chip away at on your repository's dashboard; and all new pull-requests created only block on new issues that are currently being introduced in them.


For instance, if some's changing a function in a file that has, say, 35 existing security but only introducing one issue in the function they're changing, the pull request will be blocked with only that one issue.


Baseline Analysis makes it risk-free for you to implement SAST on a repository today without having to block development on the repository due to existing technical or security debt.


## Configurable Quality Gates


Sometimes you're still not ready to start blocking pull-requests. Especially in very old codebases or when your team is on a tight timeline shipping things, introducing a new blocking check is risky.


On DeepSource, you can disable the check from blocking entirely, but still report any issues detected in the pull-request. This is a middle-ground solution that helps you start building awareness about security among developers in your team without asking them to take action right away.


Then, once you're ready in a few weeks (or months), just flip the switch and make security checks mandatory on the pull-requests.


## Zero-config Workflow Integration


DeepSource runs all analysis in it's own runtime, without any dependency to your CI pipelines. Unlike several other SAST tools that require adding a new build step, setting up analysis with DeepSource requires simply adding a` .deepsource.toml` configuration in the repository's root.


With DeepSource, there's zero risk of changes to your CI set up increasing the total build time or having to maintain yet another CI pipeline. If your team has a complex build pipeline, this makes life so much easier.


By prioritizing developer experience and offering a seamless integration, DeepSource enables you to not just implement SAST workflows in your team risk-free, but also helps you spark a healthy collaboration between developers and security teams — slowly shifting the responsibility of writing secure code to both the teams.


If your team is looking to implement SAST for the first time, we'd love to show you how we can help.[Please reach out to us](https://deepsource.com/schedule-demo) !
