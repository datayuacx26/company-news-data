---
schema_version: "1.0.0"
document_id: "19e3266c9f616fc6d5c12f3677eca51a4603da81c54c18c394fdc83c51626f45"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/remote-build-caching-secret-to-software-builds"
published_at: "2025-01-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:64df0805076bdc8a603c09f43d10f9db3915567300cb6ff1abd8406a05d2e1bc"
---

# Remote build caching: The secret to lightning-fast builds

Slow builds suck. It’s a rather uncontroversial take.


They can send cloud costs through the roof, kill a software engineer’s productivity and developer experience, and seriously hamper a team’s ability to iterate, innovate, and resolve incidents.


There’s plenty of anecdotal evidence supporting the negative impacts of slow build times. In fact, it’s been a meme in the software engineering community for as long as memes have existed. And as a growing number of tools and technologies have enabled software developers to be more productive and to ship more code, there still exists this jammed conveyor belt that is the build pipeline.


[Build faster. Waste less time. Depot accelerates Docker image builds and GitHub Actions workflows. Easily integrate with your existing CI provider and dev workflows to save hours of build time. Get Started →](https://depot.dev/start)


## Slow builds make for bad days at work


In the[2024 Stack Overflow Survey](https://survey.stackoverflow.co/2024/professional-developers#developer-experience-frustration) , nearly 33% of respondents noted *complexity of tech stack for build* as a common frustration. There’s also a growing body of empirical research supporting the potential for slow build times to negatively impact productivity and developer experience. Scientists who research questions around developer experience and productivity[recently asked developers which tasks they most wanted to automate](https://www.microsoft.com/en-us/research/uploads/prod/2024/11/Time-Warp-Developer-Productivity-Study.pdf) , and it should come as no surprise that devs mentioned wanting “faster and reliable build systems to swiftly integrate and manage new code changes into existing codebases.”


Another scientific study explored[factors that contribute to a “bad day” at work](https://arxiv.org/pdf/2410.18379) , and 56.8% of coded responses fell into one of the following categories:


- Engineering system friction (14.7%)
- Blocked or stuck (12.3%)
- Poor productivity (10.5%)
- Builds of tests slow/broken (7.9%)
- Interruptions or randomizations (7%)
- Lacking focus time (4.4%)


In our experience, waiting for slow builds to finish can contribute to each of these. Slow builds create friction in deployment pipelines. They make developers feel blocked or stuck. Waiting for slow builds to finish gets in the way of focus time and frustratingly interrupts workflow. And all of this wasted time can easily impact how productive devs feel overall.


## So then, how do we go faster?


It’s interesting that in the year 2025, we’re still bogged down by such a fundamental problem as slow build pipelines. Certainly, a large majority of folks have adopted CI tools that help automate these pipelines, and in fact, new build tools are coming online all the time that focus on improving the old ways of compiling & building artifacts. All of these things are moving the needle in the right direction.


We see this with Depot today:[Building Docker images on native CPUs](https://depot.dev/products/container-builds) with instant, shared caching can make a build up to 40x faster. We’ve rethought what a GitHub Actions runner looks like, and have brought Depot tech directly into the runners (like our accelerated caching & ramdisks for faster disk access) to[make entire CI workflows up to 10x faster](https://depot.dev/products/github-actions) .


Others are discovering new ways of creating their CI pipelines and infrastructure in native programming languages like Dagger and AWS CDK. Developers are taking every step they can to make their builds faster, if for no other reason than to save their own sanity.


What’s the next essential step forward? We believe it’s[remote caching](https://depot.dev/docs/cache/overview) . In our view, it's a capability that every development team should consider regardless of size, composition, and domain. Remote caching enables teams to reuse build and test outputs and share them seamlessly across machines and environments, offering a massive efficiency boost.


But remote caching isn’t just about speeding up builds. We think it has the potential to revolutionize the entire development process.


## Stages of build automation and optimization


As we’ve learned from our experience as engineers and in working with other companies, there’s a predictable pattern to how build automation and optimization unfolds within an organization. Most teams start with shifting their builds into a centralized place and writing workflows that accomplish the needs they have at the time. Having a single pane of glass and a single place to automate builds saves time.


These early stages focus on optimizing for code being built and deployed, checks happening on pull requests, and tests being run automatically. Teams then often invest time and attention into figuring out which portions of those workflows can be parallelized so more tasks can be checked off simultaneously.


Next comes thinking about caching. Why? Well, if you can leverage the results of a previous build, you can save time on the current build. No matter the company size, structure, or tools involved, this potential benefit is universal. It’s all about optimizing the time needed to move an idea through its representation in code and ultimately into production.


But, we believe this journey often starts after the code is committed. There’s a missing link to ultimately maximize the efficiency of the entire build pipeline by looking at where the code is being written, locally on a developer's machine.


If we can optimize the build time there and inside of a CI environment, we exponentially accelerate builds everywhere. This is where remote caching comes in.


## How does remote caching work?


Caching eliminates redundant work and drastically cuts build times. Remote caching takes it to another level by making those reusable pieces of work shared across your entire team and between your local machine & CI.


By using remote caching, developers get back valuable time in their day that allows them to move more quickly. Folks using Depot Cache have slashed their build times in half simply by moving their cache to a remote service that automatically shares it across their team. This gives them back time to focus on high value items and keeps them in a flow state, rather than constantly having to context switch while waiting for a slow build to finish.


## Looking under the hood of Depot Cache


Implementing a remote cache from the ground up is a huge time investment. We did the work so that you don’t have to. With Depot Cache, you point your tool at it with a simple config change and it just works.


[We integrate with build tools](https://depot.dev/docs/cache/overview#supported-tools) that support remote caching like Bazel, Go, Turborepo, sccache, Pants, and Gradle, so that they store and retrieve build artifacts from Depot's remote cache. That cache can then be used from local development environments, CI/CD systems, or anywhere else you run your builds.


This speeds up your builds and tests by orders of magnitude, especially for large codebases, as those builds and tests become incremental. Instead of always having to rebuild from scratch, only the parts of your codebase that have changed are rebuilt, and only affected tests are re-run.


What about latency?


Latency is something we’ve thought a lot about with remote caching. Our solution with Depot Cache is to have the remote cache service be backed by a global CDN. So when you run a build in Oregon, we pull your cache artifacts from a cache node closest to you. When you run inside of our GitHub Actions runners, we route through our fast networks to pull your cache from a node closest to your runner. The best way to combat latency is with data locality.


## Faster builds, happier devs


In our experience, the only people who don’t mind slow builds are the people who don’t love getting shit done, and we think they’re pretty rare in the software world. Remote caching significantly enhances developer experience by reducing the time spent waiting for builds to complete. Faster builds mean developers can focus on what they do best — architecting and writing and refining code — without unnecessary interruptions. This boosts productivity *and* minimizes frustration caused by those disruptively long build times.


In short, remote caching helps developers get shit done and stay happy and satisfied in the process.


## Avoid a conveyor belt build-up with Depot Cache


Slow build times can become a critical bottleneck that depletes developer focus and hinders workflow efficiency. We think there’s a direct correlation between faster build processes and improved developer productivity and developer experience.


Remote caching is the next frontier in acceleration for teams ready to take their build optimization to the next level. Just think of what you’ll unlock for your developers when you give them more time to focus on coding, and less time to be frustrated by slow builds.


If you'd like to try out Depot Cache to speed up your own builds, we[offer a free 7-day trial](https://depot.dev/sign-up) that you can use to get started.


## Related posts


- [How to build Arm and multi-architecture containers today](https://depot.dev/blog/building-arm-containers)
- [Comparing GitHub Actions and Depot runners for 2x faster builds](https://depot.dev/blog/comparing-github-actions-and-depot-runners-for-2x-faster-builds)
- [Uncovering Disk I/O Bottlenecks in GitHub Actions](https://depot.dev/blog/uncovering-disk-io-bottlenecks-github-actions-ci)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
