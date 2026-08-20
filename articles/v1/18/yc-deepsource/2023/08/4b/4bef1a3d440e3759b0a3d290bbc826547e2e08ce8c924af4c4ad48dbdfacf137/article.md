---
schema_version: "1.0.0"
document_id: "4bef1a3d440e3759b0a3d290bbc826547e2e08ce8c924af4c4ad48dbdfacf137"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/introducing-deepsource-for-kotlin"
published_at: "2023-08-05T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:796d75ab5403dfbe031da05a5b1ced6c9d6701382cb4c907aea7e5a943525c98"
---

# Introducing, DeepSource for Kotlin

Last updated on Aug 5, 2023


We're excited to launch our official Analyzer for Kotlin today. Kotlin has been one of the most widely requested programming languages that our users have asked us to support. This beta release is our first step towards building the most sophisticated static analysis experience available for Kotlin.


According to Google, about 60% of all Android developers use Kotlin as their primary language. The language's wild popularity also comes from the fact that it is easy to learn and incredibly fun to develop with. Several large enterprises have adopted Kotlin to build applications at scale — Google, Netflix, Amazon, Uber, and Pinterest are among the largest names that use Kotlin in production.


Kotlin is a compiled language and unlike Java, it comes with null-safety built in. Even though Kotlin has stricter null-safety guarantees, the language itself cannot save the developer from bad programming patterns that lead to inefficient or vulnerable code. Using static analysis on your Kotlin code can prevent these obvious errors as well as more sophisticated issues creeping into your production codebase.


## Meet the Kotlin Analyzer


At DeepSource, we strive to build the fastest and most reliable static analysis experience. The all-new Kotlin Analyzer has been built to be fast and guarantees less than 5% false positives in the results. If you're already a DeepSource user, just add the following lines in your **.deepsource.toml** file, and you'd be good to go:


```text
[[  analyzers  ]]
name =   "kotlin"


```


You can also add the Kotlin analyzer to it by heading to the[configuration](https://docs.deepsource.com/docs/repository-view-repository-settings#configuration) section of the settings tab for that repository's dashboard. Make sure you don't accidentally modify the existing configuration.


If you're new to DeepSource, get started by creating your free account[here](https://deepsource.com/signup) . Follow the steps shown[here](https://deepsource.com/blog/setup-static-analysis-python) , select the repositories you want to analyze, enable the Kotlin analyzer on them, and in a couple of minutes, you'll see any Kotlin issues raised in your code!


## 75+ issues prevented in your code


In this release, the Kotlin analyzer can detect 75+ bug risks, anti-patterns, performance issues, etc. in your source code. We're working on adding more checks as part of our upcoming releases which should be available to you automatically. The analyzer is also designed to integrate seamlessly with[detekt](https://detekt.dev/) , which is a widely used open-source static analysis tool in the Kotlin community. We understand the importance of compatibility with existing tools, and that is why we respect any detekt configuration you may have set up in your project root.


Explore all issues that we can detect in the Directory[here](https://app.deepsource.com/directory/analyzers/kotlin/issues) .


## Format your code with ktlint


We've also added the most popular Kotlin formatting tool, **ktlint** as a transformer. You can activate **ktlint** on any repository where the Kotlin analyzer is also enabled by either going to the[configuration](https://docs.deepsource.com/docs/repository-view-repository-settings#configuration) section in that repository's settings tab and regenerating the configuration (Make sure you don't accidentally overwrite already existing configuration!) or by pasting the following code at the end of the **.deepsource.toml** file:


```text
[[  transformers  ]]
name =   "ktlint"


```


Once you've committed the change to your configuration, you should see **ktlint** running on the repository.


## Get started with the Kotlin Analyzer


The Kotlin Analyzer is available to all DeepSource users now. Add the relevant sections to your existing project on DeepSource, or[get started](https://deepsource.com/signup) if you're a new user. We're constantly improving the Analyzer and you will see several new issues, Autofix™ capability, and more soon.


As always, we're looking forward to hearing from you.
