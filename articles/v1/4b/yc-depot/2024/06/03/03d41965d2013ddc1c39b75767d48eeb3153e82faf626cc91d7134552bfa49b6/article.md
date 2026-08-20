---
schema_version: "1.0.0"
document_id: "03d41965d2013ddc1c39b75767d48eeb3153e82faf626cc91d7134552bfa49b6"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/project-build-timeouts"
published_at: "2024-06-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:fe1f47134f1df24b59d4963329dbac0636601155077de94bf8a9554d29a94d78"
---

# Now available: Project build timeouts

We have two big new features now available inside Depot! You can now configure a timeout for your Docker image builds by setting a project-level build timeout. In addition, you can cancel individual builds from within Depot.


## Project level build timeouts


Now, inside your container build projects, there is a new` Build Timeout` setting that allows you to configure how long a build can run before it is automatically canceled. You enter the maximum number of minutes you're willing to wait for an image build, and once your build goes over that, it will be automatically canceled.


This is a great way to ensure that your builds don't run longer than expected and prevent any runaway builds from consuming resources.


## Cancel builds


Not all builds are invoked by a CI provider and thus are easily cancelable. Sometimes, you need to cancel a build running inside of Depot that was invoked via code or our[public API](https://depot.dev/docs/api/overview) . For those cases, we've now rolled out the ability to cancel individual builds. You can click on any running build and cancel it from the UI.


## What's next


There are more and more use cases for our Docker image build acceleration. We have folks using` depot build` in both their CI & local environments to speed up their builds. We also have more and more platform use cases where folks want to build Docker images with the power of Depot via our API.


These new features help all those use cases by making it easy to control and manage your builds. If you have other things that you'd like to see in this area, hop into our[Discord Community](https://discord.gg/MMPqYSgDCg) and let us know.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
