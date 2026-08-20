---
schema_version: "1.0.0"
document_id: "f4e1e5d3a8f19bd0c9b7fa489eeed789a9890674a819254423a7de8ab3266bab"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/from-60-repos-to-one-how-wix-tackled-monorepo-migration-part3"
published_at: "2025-10-26T11:52:04+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:8f7fb5e04305e75bbc250886eedaa7eaa7c614c97e686ac9dfd421b52f0c4459"
---

# From 60 Repos to One: How Wix Tackled Monorepo Migration - Part3

Please notice: This is Part 3 of a series of articles by


**Zachi Nachshon** :


-


Read the


[Part 1](https://www.wix.engineering/post/from-60-repos-to-one-how-wix-tackled-monorepo-migration-part-1) to learn how Wix began migrating 60 codebases into a 22GB backend monorepo, and what we learned along the way.


-


Read the


[Part 2](https://www.wix.engineering/post/from-60-repos-to-one-how-wix-tackled-monorepo-migration-part-2) to learn how Wix reduced build durations, optimized target selection with a graph-based approach, and streamlined monorepo checks.


###


The Local Development Bottleneck


When working with a 22GB monorepo locally, we encountered several challenges, including:


-


**Git performance issues:** Slow git commands due to the size of the monorepo


-


**Memory constraints:** Insufficient memory to build the monorepo locally


-


**IDE performance issues:** Slowness and freezes due to the large number of files


No developer wants to wait hours to clone a monorepo or spend time navigating unrelated code. This problem led us to build


**Tapas** , a custom tool designed to streamline local development within the monorepo.


####


What is Tapas?


Tapas is a CLI utility and framework used to create and load dynamic plugin extensions. Its runtime loads pre-selected plugins that were opted-in by the user, adding their commands into Tapas's main CLI menu.


####


Why Create Tapas?


1.


Reduce the amount of CLI utilities / unmanaged-scripts created in a variety of languages in an organization.


2.


Increased developer velocity by minimizing context switching with a controlled, documented, and tested CLI process for local and CI.


3.


Eliminate the need for undocumented and unreliable scripts that depend on environment variables.


4.


Install and use only the plugins that are relevant to the user's needs.


The all-in-one CLI provides standard features such as config management, self-updating, metrics reporting, built-in flag modifiers, and auto-completions. Most importantly, it can be extended with user-created plugins for specific domains.


One of such plugins is the


*Monorepo* plugin.


####


Tapas: Monorepo plugin key features:


-


**Performance:** Enables a smooth and performant experience when working on a Bazel monorepo of any size.


-


**Flexible Approach:** Allows developers to choose between working on the entire monorepo or a partially buildable state


-


**IDE Integration:** Seamless integration with IntelliJ and other local development tools.


-


**Side by side with current virtual monorepo:** Ability to synchronize local monorepo changes back to the virtual monorepo (current production environment).


Tapas bridges the gap between Bazel and Git sparse checkout


. It recognizes the build scope needed for the user's specified targets and the build infrastructure required for a valid Bazel workspace. By integrating these elements, Tapas provides users with a fully buildable local monorepo that is a fraction of its actual size.


There are two work modes in which Tapas operates:


1.


**Focused Mode:**


Targets specific, business-logic-oriented Bazel targets, resulting in a smaller and more performant scope.


2.


**Full Clone Mode:**


This mode covers the entire monorepo, making it larger and less performant, but suitable for infra-oriented tasks.


Focused mode is split into two methods:


**Something to build** , which includes buildable and testable Bazel target scopes that the user plans to work with, and


**Something to see** , which includes non-buildable sources that are used for local reference.


For example, a developer might want to work on specific buildable and testable Bazel targets from the dev-center and dev-center-platform folder paths (which are currently two separate VMR repositories). Developers might also want specific targets from server-infra to be locally visible for reference.


The Tapas monorepo plugin lets users focus on specific Bazel targets within the monorepo under a team/company domain. This enables users to work on only a manageable portion of the entire repository, such as using only 17% of the monorepo locally, as seen in the image below.


By reducing the working set to a fragment of the entire monorepo, developers experience dramatically faster IDE operations, reduced memory consumption, quicker searches and shorter local build times, essentially providing a lightweight development experience while maintaining all the integration benefits of a monorepo.


Tapas is utilized locally in a variety of ways, and here are a few examples:


1.


**Focus on a new monorepo Bazel target scope**


```text
tapas monorepo focus on //my/fresh/bazel/target/...
```


1.


**Focus on additional monorepo Bazel target scope**


```text
tapas monorepo focus also //my/additional/bazel/target/...
```


1.


**Focus directly from the** **.bazelproject** **view file used by the IntelliJ Bazel plugin**


```text
tapas monorepo focus from intellij
```


Using this approach, we let developers work in their favorite IDE, making it more comfortable for them.


Tapas became essential for enabling productive local development in a monorepo that would otherwise feel overwhelming.


####


What We Learned


Migrating to a monorepo isn’t for everyone, and it comes with its own challenges. For developers considering a similar path, here’s what we’ve learned:


**Start small, but think big** . Plan carefully, and don’t underestimate the work involved. Investing in the right tools makes all the difference but any team undertaking this transition will need tools tailored to their own workflows.


**Scaling CI/CD pipelines is critical** . Build times and developer experience are everything, and incremental, parallel builds were key to keeping our workflows efficient.


For us, this wasn’t just about adopting a monorepo; it was about improving the way we build software every day.


###


Conclusion: Building for the Future


For the developers at Wix, moving to a monorepo is about more than consolidating repositories. It was about building a foundation that could scale as we grew, enabling faster development, smoother collaboration, and more efficient deployments.


If you’re struggling with the complexity of distributed repositories, consider what a monorepo could mean for your team. For us, it has and will transform the way we work and set us up for future growth.


---


Watch the full talk ״


From 60 Repos to One: How Wix Tackled Monorepo Migration like a Tech Giant",


by Zachi Nachshon to hear more about our journey to a monorepo:


**This post was written by Zachi Nachshon**


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/subscribe)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)


-


Listen to our podcast on


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) or
