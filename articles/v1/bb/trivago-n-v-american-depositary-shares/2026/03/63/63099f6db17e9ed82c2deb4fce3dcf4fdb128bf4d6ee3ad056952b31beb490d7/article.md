---
schema_version: "1.0.0"
document_id: "63099f6db17e9ed82c2deb4fce3dcf4fdb128bf4d6ee3ad056952b31beb490d7"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2026-03-13-from-a-long-list-to-a-clear-signal-baseline-driven-accessibility-reporting/"
published_at: "2026-03-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:2b3b7e93fac068ae5f55234610c6be9f12fac715c6887c590ba58ea7f5cae1b9"
---

# From a long list to a clear signal: baseline-driven accessibility reporting

The output of most automated accessibility tests is a long list of violations. This format, while comprehensive, makes it difficult to distinguish new issues introduced in a feature branch from long-standing technical debt. It doesn’t clearly show if accessibility is improving, and it doesn’t help prevent regressions.


With regulations like the[European Accessibility Act (EAA)](https://commission.europa.eu/strategy-and-policy/policies/justice-and-fundamental-rights/disability/european-accessibility-act-eaa_en) making digital accessibility a legal requirement, teams need a more effective process than simply reviewing an ever-growing list.


Most automated accessibility testing tools answer “What’s wrong right now?” but they fall short on “What did we just fix?” and, more importantly, “What did we just break?” We addressed this gap by extending our in-house *test automation framework* to support baseline-driven reporting, turning raw test output into actionable information. Our solution is packaged as an extension to the framework that can be enabled on demand.


# The problem with isolated reports


Analyzing accessibility reports in isolation creates several practical problems:


- New vs. existing issues: A single list mixes regressions with pre-existing debt. A critical new issue can easily get lost among dozens of older, lower-priority ones.
- No clear proof of progress: A reduction in the total violation count doesn’t tell the full story. It’s impossible to know if you fixed 30 issues and introduced three new ones, or if you just fixed 27.
- *Developer pressure* : Large violation lists create friction. The task can seem so large that it’s tempting to defer the work, which allows new regressions to merge into the main codebase.


The fundamental issue is a lack of context. The report shows the current state but provides no information about the change from a previous state.


# Our approach: comparing against a baseline


Our solution is to stop analyzing reports in isolation and instead compare every new test run against a “baseline”, a snapshot of the known issues on the main branch. Our internal tool for this builds on the open-source[axe-core-maven-html-selenium](https://github.com/dequelabs/axe-core-maven-html/tree/develop/selenium) project, extending it with clear reporting and baseline logic. *We chose it* as the underlying[axe-core](https://github.com/dequelabs/axe-core) *is a proven* open source solution for reporting accessibility issues. Also, our test framework is based on Selenium, so they can work together hand in hand.


The core of our baseline comparison lies in how we uniquely identify each accessibility violation. For every detected issue, we generate a stable “fingerprint” by hashing a combination of the reported accessibility ruleId (e.g., color-contrast), the problematic html snippet, the CSS target selector, and the impact level (e.g., critical, moderate). *We also do some further pre-processing to avoid false positives or negatives if just one of these things only slightly changes but the affected web element should be considered the same.* This fingerprinting ensures that the same violation, even if slightly shifted in the DOM or having a different text value on the website, can be consistently tracked across different test runs.


The comparison organizes the output into three distinct categories:


- New issues: Violations found in the feature branch that do not exist in the baseline.
- Fixed issues: Violations that were in the baseline but are no longer present in the feature branch.
- Persisting issues: Problems that exist with identical fingerprints in both the baseline and the feature branch.


# How it works in practice: Integrating into CI/CD with GitHub Actions


Our entire process is seamlessly integrated into our CI/CD pipeline using GitHub Actions, providing developers with immediate feedback within their pull requests.


## Baseline generation


A dedicated GitHub Actions workflow runs on every merge to the main branch. This job executes a fixed set of accessibility test scenarios against the deployed staging environment of our main branch. The accessibility test results, in the form of a baseline.json file that axe-core produces and the associated comprehensive HTML reports including screenshots of all affected web elements, are then pushed to a special GCP bucket. This makes sure that the baseline is always fresh.


## Comparison in pull requests


When a developer opens a pull request for a feature branch, another GitHub Actions workflow is automatically triggered. This workflow deploys the feature branch to a temporary staging environment and then runs the same tests that are used on the baseline against it. Crucially, during this run, the extension fetches the latest baseline.json and the screenshots so it can perform the comparison.


Multiple artifacts are generated here:


- A raw JSON output of the test results against the feature branch.
- The screenshots for the affected elements of the feature branch.
- The baseline report from the baseline.json file with the screenshots that already existed in the baseline report.


We regenerate the baseline report so it has the exact look and feel of the current version of our reporting extension. Also, it makes it easier to integrate this into our test report navigation. This makes it possible to not only see the current, fixed and new issues but also look at the baseline report without having to leave the current report.


Like the baseline, all generated artifacts are pushed to a GCP bucket and stored so they can be seamlessly linked to the GitHub status overview. This status is marked as failed as soon as there are any new accessibility issues introduced with this pull request.


## Some more technical details


The *accessibility* framework extension uses our test framework’s plugin and extension mechanism so it can be extended, improved and fixed without touching the core framework. This makes it possible to iterate quickly if there are issues such as false positives or negatives, or if visual adjustments have to be made.


The above flow is simplified since there can be multiple reports generated per test run. Each report has a dedicated title and subdirectory specified. This information is then used within the baseline difference calculation to make sure that each of the sub reports are compared to their respective baseline.


After all reports are generated, an overview page is created that displays the overview of available sub reports. This is also the stage where needed CSS and JavaScript are added for the visual appearance of the report pages.


Reports can also be generated without a baseline in case there is no baseline report available. This happens every time the baseline itself is generated but also if there is something wrong on the infrastructure side. In that case, only a current snapshot report is generated which still gives a good picture about the a11y state of the application under test but without the comparison metrics.


## A focused report


Developers can click the “a11y test report” link directly in the GitHub Pull Request comments or checks. On the overview, it is immediately apparent if new issues were introduces or old issues were fixed.


Drilling down into one of these sub reports, there is a detailed overview of issues, their severity, which concrete elements are affected and steps to fix it. It is also possible to copy a locator targeting the exact element on the page for further manual or automated checks to verify the issue. For better visibility, each accessibility rule violation also includes a screenshot and badges indicating the ruleset that each issue is defined in.


This draws attention to the most critical information which makes the developer’s immediate task more obvious: investigate and resolve any regressions introduced by their changes. Our current system is designed to flag all new issues, treating them as potential regressions that should be addressed before merging. Persisting issues, which are already tracked, are not flagged and can be addressed as part of scheduled accessibility improvements.


# Keeping a realistic perspective


It’s important to note that this report does not replace thorough manual checks. Automated solutions are powerful, but industry estimates suggest they can only detect around 30-50% of all possible accessibility issues. Many violations, especially those related to complex user interactions, screen reader behavior, or cognitive load, still require human testing.


We see this automated report as one important piece of a larger accessibility puzzle, not a complete solution on its own.


# Conclusion


The tools used to find problems are only one part of an effective accessibility strategy. The process built around those tools is just as important.


For us, the key was not finding more issues, but adding context to the ones we found. By separating new problems from old ones, accessibility became a manageable part of our daily development work. It enables developers to fix regressions proactively and helps teams to incrementally improve their accessibility posture. While a dedicated dashboard for long-term metrics isn’t yet in place, it could be a useful overview for the future. However, the continuous generation of the baseline itself already provides a clear snapshot of the current state of accessibility debt, giving us a good indication of our progress.


Regarding persisting issues that remain in the baseline, our philosophy is clear: both the development and QA teams are trained on spotting these issues, and there should be no issues that are deliberately left unsolved. The sooner issues are tackled, the easier it is for subsequent issues to be fixed, promoting a continuous improvement cycle rather than accumulating technical debt.


When evaluating your own accessibility process, these questions can be a useful starting point:


- How can we tell if a change made things worse?
- What is the smallest, most immediate action a developer can take?
- How do we make progress visible and sustainable?


The goal should be a system that helps your team solve problems incrementally, not just one that generates a list of them.
