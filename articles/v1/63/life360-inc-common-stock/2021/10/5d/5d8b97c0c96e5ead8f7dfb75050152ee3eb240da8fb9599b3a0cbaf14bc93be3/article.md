---
schema_version: "1.0.0"
document_id: "5d8b97c0c96e5ead8f7dfb75050152ee3eb240da8fb9599b3a0cbaf14bc93be3"
company_key: "life360-inc-common-stock"
company: "Life360 Inc."
source_id: "life360-inc-common-stock-rss-25c02f0e1eb8"
canonical_url: "https://medium.com/life360-engineering/customizing-android-studio-with-the-intellij-plugin-sdk-part-1-8827e3ddd217"
published_at: "2021-10-01T21:57:28+00:00"
first_seen_at: "2026-07-20T04:36:48.728062+00:00"
fetched_at: "2026-08-20T00:22:26.884199+00:00"
content_hash: "sha256:65223dd71405079acc5265fc045ff89023d23debced40def691b7e900f146896"
---

# Customizing Android Studio with the IntelliJ Plugin SDK (Part 1)

#### There’s a Plugin For That


IDEs are a very powerful tool in today's world of software engineering. They help catch syntax errors, display static code analysis warnings, help refactor and migrate code, and even serve as code generators.


Some common uses for plugins are:


- Code generation (also known as templating)
- Adding additional IDE support for customized architectures
- Adding automation for repetitive tasks to make them less time consuming
- Creating a brand new language or Intellij IDE


Over the course of my tenure at Life360, I have written and supported two plugins for Android Studio. Both of them solved two very distinct problems but contributed to the ease of use and velocity for the Android team.


The first plugin came about due to our very customized design system. The system uses a JSON format and some syntactic sugar so that colors could in the future be pulled remotely from the backend. Native IDE support was hindered by diverging from the typical colors.xml approach, making it cumbersome to know if the purple or gray you were using was the purple or gray you really wanted. Setting out to bring back more native-like support for colors, this plugin helped engineers have a visual representation of a named color in the gutter by parsing the JSON and hex codes. The end result was this:


Color palette at time of writing pluginUsage by Engineer


The second plugin was more in-depth. Overall, the team moved the architecture for the app towards micro-services and well-defined, smaller pieces. We made the decision to have multiple repositories, but there was a need to solve the amount of engineering time it took to bring these new pieces online.


CI configuration, static analysis, test setup, documentation, publication, and sample apps were all major pieces that went into the majority of these repositories. That’s when I set out to incorporate our desired architecture into the new project wizard of Android Studio. At first, I investigated Free Maker ([https://freemarker.apache.org/](https://freemarker.apache.org/) ) and local files but eventually landed on the Intellij supported plugin way to add to the existing wizard. This methodology does not require local files to be in sync, only that the plugin is up-to-date. The end result was a quicker startup time and a more enjoyable engineering experience creating new pieces of our system.


Two additional options were added to the new project wizardPage 1 of the new kit wizardPage 2 of the new kit wizard


Although the wizard looks pretty, it is not the most important piece. The files generated are what really makes this a true time-saver. The files included are:


- A sample app complete with the navigation component and dagger setup
- Jenkinsfile for CI config
- KLint, Lint, CPD, and static analysis tools pre-configured
- GitHooks to keep non-passing commits from taking time and resources on CI
- A template for a CHANGELOG.md
- A README.md
- Config for Dokka to generate markdown documentation from KDocs
- Fastlane for easy-to-use commands and publication
- Jacoco and code coverage setup as well as example instrumentation and unit tests


The sample app, if burrowed down into, has a default single activity, multiple fragment setup with a top-level application that handles dagger injection. Proguard rules and a set of modules depending on the project type are also included and it strives to be a good living/working example.


Overall, the templating plugin took what normally would take an engineer about a month or two (2–3 sprints) of effort for configuration and initial setup and cut it down to ~5–10 minutes. This means more coding sooner!


The process took a lot of trial and error and I am hoping that through this series I can relay some lessons learned the hard way, show you some tricks, and dive more into how I used these technologies to solve problems for the larger team.


Below are some documentation primers, and in part 2, I’ll dive more into how plugins like these are implemented.


- Kotlin ([https://kotlinlang.org/](https://kotlinlang.org/) )
- Intellij SDK ([https://plugins.jetbrains.com/docs/intellij/welcome.html](https://plugins.jetbrains.com/docs/intellij/welcome.html) )
- Kotlin DSL ([https://docs.gradle.org/current/userguide/kotlin_dsl.html](https://docs.gradle.org/current/userguide/kotlin_dsl.html) )
- Plugin verifier ([https://github.com/JetBrains/intellij-plugin-verifier](https://github.com/JetBrains/intellij-plugin-verifier) )


### Come join us


Life360 is the first-ever family safety membership, offering protection on the go, on the road, and online. We are on a mission to redefine how safety is delivered to families worldwide — and there is so much more to do! We’re looking for talented people to join the team: check out our[jobs page](http://www.life360.com/jobs) .


---


[Customizing Android Studio with the IntelliJ Plugin SDK (Part 1)](https://medium.com/life360-engineering/customizing-android-studio-with-the-intellij-plugin-sdk-part-1-8827e3ddd217) was originally published in[Life360 Engineering](https://medium.com/life360-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
