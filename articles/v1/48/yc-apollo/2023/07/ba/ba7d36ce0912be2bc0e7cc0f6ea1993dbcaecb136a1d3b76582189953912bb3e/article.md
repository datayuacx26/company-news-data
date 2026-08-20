---
schema_version: "1.0.0"
document_id: "ba7d36ce0912be2bc0e7cc0f6ea1993dbcaecb136a1d3b76582189953912bb3e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-apollo-kotlin-leverages-gradle-enterprise-to-rev-up-build-times"
published_at: "2023-07-17T09:43:11+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:ed62dffd60da61a215632bee2924c8a5b01496934f882fc3ac8ae3933c101169"
---

# How Apollo Kotlin leverages Gradle Enterprise to Rev Up Build Times

If you are using Apollo Kotlin, you are very very (very) likely using the Gradle Build Tool. It plays a central part in Apollo Kotlin. It’s the build system that downloads your schema, generates your Kotlin models, wires everything together, and makes sure you’re not running your tasks too many times. This is done using the` com.apollographql.apollo3` Gradle plugin ([download](https://plugins.gradle.org/plugin/com.apollographql.apollo3) ,[source](https://github.com/apollographql/apollo-kotlin/tree/dac7678081759edb21bd93bbba78e1d7348bc942/libraries/apollo-gradle-plugin) ) amongst other things.


We, the maintainers of Apollo Kotlin, are also using the Gradle Build Tool daily to build the libraries and run the integration tests. The build has grown substantially over time. It now includes 75 modules and more than 2400 tests and benchmarks on different platforms like Android, iOS, Linux, and the Web. Managing such a build can be challenging at times and having to wait for Github Actions before merging a PR is never fun…


So when the opportunity arose to join the “Revved up by Gradle Enterprise”[OSS initiative](https://gradle.com/enterprise-customers/oss-projects/) three months ago, we didn’t think twice! We jumped on board and started optimising our builds. This post tells the story of how we integrated[Gradle Enterprise](https://gradle.com/) , our findings, and what it may look like if you integrate it in your project.


## Gradle Enterprise


Gradle Enterprise is a software platform for boosting developer productivity and improving the developer experience. If you use the free Build Scan® service, you are already experiencing some of the benefits. The version available as part of Gradle Enterprise adds[more features](https://gradle.com/pricing/) like unlimited and automatic build scan invocations and data retention, data privacy, cross build analysis (aka Build Scan comparisons), and Gradle Enterprise REST API support. In addition to build and test performance acceleration technologies (like Remote Build Cache discussed here) and the aforementioned Build Scan service for making troubleshooting failed builds more efficient, Gradle Enterprise provides failure analytic tools to improve toolchain reliability, including flaky test management capabilities.


Integrating Gradle Enterprise is done through a[Gradle plugin](https://docs.gradle.com/enterprise/gradle-plugin/) . In the Apollo Kotlin case, the configuration boils down to this:


```text
plugins {
id("com.gradle.enterprise") version "3.13.4"
}


gradleEnterprise {
server = "https://ge.apollographql.com"


buildScan {
publishIfAuthenticated()


capture {
taskInputFiles = true
}
}
}
```


Note: we’re using[a Gradle Enterprise instance dedicated to Apollo](https://ge.apollographql.com/) . This post isn’t a deep dive so I’ll skip the configuration details, but it’s overall very easy. For more information, you can check the comprehensive[plugin documentation](https://docs.gradle.com/enterprise/gradle-plugin/) .


## The remote build cache


Gradle Build Tool avoids re-executing the same work in several ways:


1. [Up-to-date checks](https://docs.gradle.org/current/userguide/incremental_build.html) , also called incremental build (not to be confused with[incremental task execution](https://docs.gradle.org/current/userguide/custom_tasks.html#incremental_tasks) ), avoids executing tasks if no input has changed. For example: if you didn’t touch any` .kt` file, you shouldn’t need to run` compileKotlin` again.
2. [Local build cache](https://docs.gradle.org/current/userguide/build_cache.html#sec:build_cache_configure_local) reuses the results of previous task executions stored locally on your machine: if you remove your` build` directory,` compileKotlin` will fetch the` .class` files from your local build cache instead of recompiling them, which is usually much faster.
3. [Remote build cache](https://docs.gradle.org/current/userguide/build_cache.html#sec:build_cache_configure_remote) reuses the results of previous runs stored on a remote machine: if you do a clean checkout on a new machine but someone else somewhere in the world already built your project, you can reuse their outputs.


That last point is particularly interesting. A finely-tuned remote cache can save your developers hours of build time. For an example, Apollo Kotlin’s` shadowR8Jar` task is one of the longest tasks,[routinely taking more than one minute](https://ge.apollographql.com/s/nafcok5otqjwe/timeline?details=qpotk5ayimdi4) to execute. Not everyone needs to recompile it though. If you’re working on other parts of the build, you can reuse the jar that was built in CI from the latest push to` main` .


Enabling the build cache is a few lines in your` settings.gradle.kts` file:


```text
buildCache {
remote(gradleEnterprise.buildCache) {
enabled = true
push = isCI
}
}
```


## Optimizing the build


In order for up-to-date checks (incremental builds), local build cache and remote build cache to work correctly, you’ll need to make sure your build respects certain conditions:


1. Tasks inputs and outputs to be configured correctly.
2. Consistent tasks inputs across local runs: for an example when the project is in different directories.
3. Consistent tasks inputs across remote runs: for an example CPU architecture, JDK version and/or environment variables should not change your tasks inputs (unless required obviously)


It turns out we had all three kind of issues in the Apollo Kotlin repo.


Fortunately, Gradle ships useful[build validation scripts](https://github.com/gradle/gradle-enterprise-build-validation-scripts/blob/main/Gradle.md#installation) that help diagnose such issues. With the scripts and the awesome guidance from[Nelson Osacky](https://github.com/runningcode) ,[Tyler Bertrand](https://github.com/tylerbertrand) and other Gradle team members 💙, troubleshooting is a lot easier.


We found a bunch of issues, both in the project and in dependencies and below are a few examples. This post isn’t the place to list them all but they are a good sample of what you may find in a real life project:


### 1.` java.net.URL` serializes differently depending if` .hashCode()` was called


[GitHub issue](https://github.com/gradle/gradle/issues/24979)


[Gradle Enterprise inputs comparison](https://ge.apollographql.com/c/42lopdlukymuu/ixecxntor7p2w/task-inputs?cacheability=cacheable,overlapping-outputs,validation-failure&task-text=dokka#ftw6b756lcrbe-dokkasourcesets.main$0.externaldocumentationlinks.jdk$2.url)


[Workaround](https://github.com/adamko-dev/dokkatoo/pull/65)


This one literally blew my mind 🤯. You would assume an URL to be serializable to a String like` "https://..."` . Turns out` java.net.URL` is not!` hashCode` () is doing network IO and depending on whether the DNS was resolved or not will serialize to a different value. There is a[8-year-old bug in the JDK](https://bugs.openjdk.org/browse/JDK-8075619) that is not fixable now but that was a fun ride. This was fixed by using` java.net.URI` instead. Many thanks[aSemy](https://github.com/aSemy) for working around this 💙.


### 2.` apollo-compiler:test` and` apollo-gradle-plugin:test` contain absolute paths


[Gradle Enterprise inputs comparison](https://ge.apollographql.com/c/akstifgsdtzli/4dnomw3hfzivq/task-inputs?cacheability=cacheable,overlapping-outputs,validation-failure#mch5vbuoyljvq-$1)


[Fix](https://github.com/apollographql/apollo-kotlin/pull/4899)


When creating inputs programmatically, the[default path sensitivity is set to Absolute](https://github.com/gradle/gradle/issues/23456) . This is a problem because running the same tasks from different directories cannot reuse previous results therefore creating local build cache misses. This was fixed by specifying the path sensitivity explicitly.


### 3. Apollo jars have host JDK version in` Built-By` attribute


[Gradle Enterprise inputs comparison](https://ge.apollographql.com/c/hsu4evocb27qw/2vtgyzwwzgmig/task-inputs?cacheability=cacheable&expanded=WyJoajJsMnF3bG8zcXVxLWxpYnJhcmllcyIsInNzb3ZhbmpsNzQ1c2ktc3RhYmxlY2xhc3NwYXRoIl0#change-ssovanjl745si-stableClasspath-0-0)


[Fix](https://github.com/apollographql/apollo-kotlin/pull/4961)


For historical reasons, the Apollo Kotlin jars contained a` Built-By` attribute in the jar manifest. While this is fine when running the same JDK, different CI machines have different JDK versions creating remote build cache misses. This was fixed by removing this` Built-By` attribute


## Collaboration is easier


One thing that stands out with Gradle Enterprise is how easy it is to link to diagnostics. Almost every item in the UI is clickable with an anchor link pointing exactly to where the problem is.


Want to link to a given task that takes a long time?[This is possible](https://ge.apollographql.com/s/nafcok5otqjwe/timeline?details=qpotk5ayimdi4) :


Want to link to a given line in the logs?[This is also possible](https://ge.apollographql.com/s/hvad7y7pgpbxm/console-log?page=2#L1634) :


Want to outline a cache miss that is due to different inputs?[You can link to inputs comparisons](https://ge.apollographql.com/c/akstifgsdtzli/4dnomw3hfzivq/task-inputs?cacheability=cacheable,overlapping-outputs,validation-failure#mch5vbuoyljvq-$1) :


All of this is especially useful when working with other open source projects as it makes troubleshooting and communication a lot easier.[gradle-intellij-plugin/issues/1376](https://github.com/JetBrains/gradle-intellij-plugin/issues/1376) is a good example where sending a link saves a lot of writing time and allows the issue to be resolved quickly.


## Current state


Three months into our journey, our mean build time in CI went from ~28min to ~4min 🎉 ([trend](https://ge.apollographql.com/scans/trends?search.startTimeMax=1688594015772&search.startTimeMin=1680732000000&search.tags=CI&search.timeZoneId=Europe/Paris&trends.disabledDistributions=WyJidWlsZC10aW1lOnA1Xzk1IiwiYnVpbGQtdGltZTpwMjVfNzUiLCJhdm9pZGFuY2Utc2F2aW5nczpwMjVfNzUiLCJhdm9pZGFuY2Utc2F2aW5nczpwNV85NSIsImF2b2lkYW5jZS1zYXZpbmdzOmRpc3RyaWJ1dGlvbi5wNTAiLCJidWlsZC10aW1lOmRpc3RyaWJ1dGlvbi5wNTAiXQ&trends.section=buildTime&trends.timeResolution=week) )


Of course this depends on several factors and if we do a lot of low-level changes that invalidate the build cache, that time may go up again. But we’ve gained confidence and visibility in our build. Plus the time savings are nice too!


Another side effect of this work is that we can apply the findings to the Apollo Gradle Plugin itself and make sure the Apollo tools integrate nicely in the ecosystem.


## What’s next


The journey is not over. We’ve seen a lot of improvements already but there is still room for optimization. We are eager to enable the[configuration cache](https://docs.gradle.org/current/userguide/configuration_cache.html) and look at[predictive test selection](https://gradle.com/gradle-enterprise-solutions/predictive-test-selection/) to see what additional savings we can get.


It’s an exciting time to be in the devtools space. If you’re an Apollo contributor, let us know if the remote build cache made anything faster for you. If you’re an Apollo user,[let us know what](https://github.com/apollographql/apollo-kotlin/issues/new/choose) we can do to make your experience better. In all cases, I hope this gave a good overview of what Gradle Enterprise can do to boost productivity by providing real life examples.


## Many thanks to Gradle


We would like to thank Gradle for selecting us to participate in their “Revved Up by Gradle Enterprise” OSS partner program. In particular, many thanks to[Nelson Osacky](https://github.com/runningcode) and[Tyler Bertrand](https://github.com/tylerbertrand) who have been super supportive during the journey and continue to give insights and guidance on how to make our builds even faster.


Written by


Martin Bonnin


[Read more by Martin Bonnin](https://www.apollographql.com/blog/author/martin)
