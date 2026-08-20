---
schema_version: "1.0.0"
document_id: "8733378f5dec6b5d69301427af3a666e96840c182c47c7e60aae95c24b798378"
company_key: "yc-meticulous"
company: "Meticulous"
source_id: "yc-meticulous-news-import-624e920f0172"
canonical_url: "https://www.meticulous.ai/blog/speed-up-your-frontend-monorepo-build"
published_at: "2024-01-16T00:00:00+00:00"
first_seen_at: "2026-07-25T14:57:24.607504+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:c5f54b432163ad729c21eefd33f5ce60c895d090400f6bff03931212ee17bb24"
---

# How to Speed Up Your Frontend Monorepo Build

In today's fast-paced development environment, speed and efficiency are crucial for a successful software development process. A monorepo is a popular strategy for organizing and managing codebases, especially in large-scale projects. It offers several advantages, such as simplified dependency management, code sharing, and collaboration.


However, as the size of your frontend monorepo grows, build times can become a bottleneck, slowing down your development process and negatively impacting developer productivity. In this blog post, we'll explore how to speed up both your local and continuous integration (CI) builds using[Turborepo](https://turbo.build/repo) and[Turbocache](http://turbocache.build/) . Not only will these techniques lead to faster development cycles but will also reduce your CI costs.


## Getting started with Turborepo


Turborepo is a high-performance build system specifically designed for monorepos, optimizing and parallelizing tasks while leveraging caching. Let's dive into how to set up Turborepo for your project.


If you have already set-up Turborepo, you can skip to the next section.


To get started with Turborepo, you need to install it as a global dependency:


```text
npm install -g turbo


```


Once Turborepo is installed, add a root turbo.json to your monorepo with your desired tasks. This configuration sets up a pipeline with three tasks: build, test, and lint. You may reference the[configuration options](https://turbo.build/repo/docs/reference/configuration) on the Turbo docs to fit your monorepo.


```text
{
"$schema": "https://turbo.build/schema.json",
"pipeline": {
"build": {
"outputs": [".next/**", "!.next/cache/**"],
"dependsOn": ["^build"]
},
"test": {
"dependsOn": ["build"]
},
"lint": {}
}
}


```


You can confirm your setup by running:


```text
turbo lint build test


```


## Speeding up your CI builds with Turbocache


Turbocache is a remote caching service that can significantly speed up your CI builds. It is designed to work with Turborepo and can be used with any CI provider, although this blog post will focus on GitHub Actions. Along with caching, Turbocache also provides a dashboard to monitor your builds as well as analytics to help you optimize your builds for better cacheability.


To get started, you need to create an account at[turbocache.build](https://turbocache.build/) . Once you create an account, you will need to create an API token. Store this API token in your repository's secrets as TURBO_TOKEN.


Update your Github Action workflow to use Turbocache. Your updated workflow should look similar to this:


```text
name: CI


env:
# Turborepo
TURBO_API: "https://cache.turbocache.build"
TURBO_TOKEN: ${{ secrets.TURBO_TOKEN }}
TURBO_TEAM: "meticulous_ci" # you may change this to your team name
TURBO_RUN_SUMMARY: true


jobs:
# ...
build:
# ...
steps:
- uses: turbocache-build/turbocache-action@v1
- uses: actions/checkout@v3
# ...
- run: npm install
- run: npx turbo lint build test


```


For up-to-date instructions, please refer to the[Turbocache documentation](https://turbocache.build/docs) .


--


In this blog post, we've explored how to speed up your frontend monorepo builds using[Turborepo](https://turbo.build/repo) and[Turbocache](http://turbocache.build/) . By implementing these tools, you can optimize your local and CI builds, leading to faster development cycles and reduced CI costs. Don't let slow build times hold you back!


## Meticulous


Meticulous creates and maintains an exhaustive suite of e2e ui tests with **zero** developer effort.


This quote from the CTO of Traba sums the product up best: "Meticulous has fundamentally changed the way we approach frontend testing in our web applications, fully eliminating the need to write any frontend tests. The software gives us confidence that every change will be completely regression tested, allowing us to ship more quickly with significantly fewer bugs in our code. The platform is easy to use and reduces the barrier to entry for backend-focused devs to contribute to our frontend codebase."


This[post](https://www.meticulous.ai/blog/lessons-from-a-decade) from our CTO (formerly lead of Palantir's main engineering group) sets out the context of why exhaustive testing can double engineering velocity. Learn more about the product[here.](https://www.meticulous.ai/)
