---
schema_version: "1.0.0"
document_id: "06383dfecb18fe90a54ee8748d59c4e0d1011817cc4c94ab462e10a8945b4f7f"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/swift-static-analysis"
published_at: "2023-07-12T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:048d1e10535450a2ed636b5b33354880b4129df930affe048949d32b1b9ead78"
---

# Introducing, DeepSource for Swift

Last updated on Jul 12, 2023


Swift has rapidly risen to prominence as a preferred programming language for Apple's ecosystem. With 30m+ registered developers on Apple's development platform, it's the primary programming language for developing applications on iOS and macOS platforms, including iPhone, iPad, Mac, and other Apple devices.


Today, I'm excited to announce DeepSource's Swift Analyzer, with support for static analysis, Static Application Security Testing (SAST), code coverage, and Autofix™️. The Swift Analyzer is available to all users and teams on DeepSource Cloud and Enterprise, and coming to Enterprise Server in the next release.


If you're an existing DeepSource user, simply enable the Analyzer on one of your repositories. If you're not on DeepSource yet,[sign up here](https://deepsource.com/signup) or request for a[personalized demo for your team](https://deepsource.com/schedule-demo) .


## Write clean and secure Swift code


The Swift Analyzer detects 75+ issues as part of this release, with which it can flag potential bugs, performance bottlenecks, bad programming practices, and security vulnerabilities in your Swift code. We're working on adding more checks as part of our upcoming releases which should be available to you automatically. The analyzer is also designed to integrate seamlessly with SwiftLint, which is a widely used open-source linter in the Swift community. We understand the importance of compatibility with existing tools. This is why we respect any SwiftLint configuration you may have set up in your project.


Explore all issues that we can detect in the Directory[here](https://app.deepsource.com/directory/analyzers/swift/issues) .


## Autofix™️ for Swift


If you haven't seen Autofix™️ yet, here's the gist --- DeepSource can automatically create fixes for issues detected in your code and create pull-requests in a couple of clicks. We've built Autofix™️ ground-up for Swift, starting with 14 checks. This will enable you to resolve hundreds of occurrences of issues across your entire repository easily.


## Repository metrics for Swift


In addition to detecting issues in your Swift code, DeepSource also helps you understand your code health through several metrics, such as documentation coverage, number of external dependencies, and code coverage. With the Swift Analyzer, you get all of these metrics tracked out-of-the-box. You can set thresholds and configure quality gates to block pull-requests if any of these thresholds fail.


The *Documentation Coverage* metric calculates and displays the percentage of functions, classes, etc in your codebase that have been documented. Not only this, but it also flags the specific functions, classes, and other constructs in your codebase that is not documented, so that you can take action and document them.


The Swift analyzer also displays the number of dependencies that are currently being used by your Swift project. The analyzer currently supports[Swift Package Manager](https://www.swift.org/package-manager) . Support for more package managers is on the way!


## Getting started with Swift on DeepSource


If your repository is already active on DeepSource, you can enable the DeepSource Swift Analyzer by adding the below entry in your **.deepsource.toml** file.


```text
[[  analyzers  ]]
name =   "swift"


```


More information on how to configure the analyzer can be found[here](https://docs.deepsource.com/docs/analyzers-swift) .


If you're not on DeepSource yet,[sign up here](https://deepsource.com/signup) or request for a[personalized demo for your team](https://deepsource.com/contact/sales) . We're excited for you to try out DeepSource for Swift!
