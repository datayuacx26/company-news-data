---
schema_version: "1.0.0"
document_id: "4801ab72741aa1190553694834695c0109598022b1d313ae3bcfb615a300d77a"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/dagger-0-16/"
published_at: "2025-02-20T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:4f09ddd3070cbb9f993dfd1ab9a7a0bb47ddc730bf91ed0c76e941f694731e06"
---

# Dagger 0.16: 1Password and Hashicorp Vault Integrations, Performance Boost, and More

We’re excited to announce the release of Dagger 0.16, packed with features and improvements that simplify your workflows, enhance security, and provide a smoother overall experience. Whether you’re just getting started or a seasoned Daggernaut, there’s something here for everyone. Let’s dive in!


#### Secure Secrets Management in Dagger: Now with HashiCorp Vault and 1Password


Dagger 0.16 brings a major upgrade for teams that need secure, seamless secrets management: you can now fetch secrets directly from HashiCorp Vault and 1Password inside your pipelines. No more manually loading secrets into your environment. Dagger now integrates with the tools you already trust to keep your credentials safe.


**What This Means for You**


**Fetch Secrets When You Need Them**


Instead of preloading secrets before running a pipeline, you can now securely retrieve them at any stage of execution. Credentials, tokens, API keys—Dagger pulls them only when needed, reducing the risk of accidental exposure.


**Works with the Tools You Already Use**


If your team relies on Vault for enterprise-grade security or 1Password for team collaboration, Dagger now plugs right in. No extra steps—just seamless, built-in support.


**Less Setup, More Security**


With this update, secrets stay secure and up to date, with less configuration and no hardcoded values. As always, secrets are kept out of logs and cache.


This is just the beginning! We’d love to hear which secret managers you want next—drop a request in[GitHub](https://github.com/dagger/dagger/issues) and help shape the future of secrets management in Dagger.


Check out the demo below to see it in action, or dive into our docs[here](https://docs.dagger.io/features/secrets/) :


#### Performance Improvements


**Faster Module Loading** Loading of modules (the` load module` step shown in progress output) is now faster in many cases. In particular:


- Cache utilization of module loading is greatly improved, which can decrease load times by up to a factor of 10 when re-calling functions after changing source code in a Daggerized repo.
- Less extraneous files are loaded from the source repository.


Users of modules with large numbers of dependencies or in large git repositories are expected to see the most immediate benefit.


For some concrete numbers, here are` load module` times for the` dagger-dev` module in Dagger’s repository under different scenarios:


These improvements in cache utilization are also set-up for future improvements not only in` load module` times but also function call times more generally.


There are a few obscure breaking API changes along for the ride with these improvements, but are not expected to impact the vast majority of users. However, v0.16.0 engines will as a result require CLIs to also be upgraded to at least v0.16.0 in order to connect.


Read more, including about details on breaking changes:[#9505](https://github.com/dagger/dagger/pull/9505)


#### Boosting Developer Productivity


We’ve introduced several updates to streamline your development workflows and save time, including:


**Effortless Dependency Updates with**` **dagger update**`


Managing dependencies is now easier than ever with the new` dagger update` command. This feature streamlines updates to your` dagger.json` file, keeping your dependencies current without manual effort.


Read more:[#8839](https://github.com/dagger/dagger/pull/8839)


**New Engine Config Mechanism with**` **engine.json**`


v0.16 introduces significant enhancements to engine configuration by introducing a new custom engine configuration file,` engine.json` specifically for Dagger. This new configuration file does not entirely replace the existing BuildKit-style` engine.toml` format but is designed to be the preferred method for editing configurations moving forward. By creating a custom configuration format, we can simplify the format, and additionally add Dagger-specific fields that we need. As part of this, we now no longer need to ship a default config as part of the engine image – the engine starts out of the box with reasonable defaults.


Additionally, it’s now much easier to inject this new config - instead of needing to manually start and manage a new engine, the default docker provisioner now pulls this config file from` ~/.config/dagger/engine.json` and automatically injects it in.


This update enables several key improvements:


- Allows for a cleaner and more relevant configuration by eliminating unnecessary items related to multiple OCI/containerd workers and other BuildKit-specific settings.
- Introduces custom properties for the engine, moving away from the reliance on numerous environment variables.
- Provides an opportunity to document the new configuration format using JSON schema, enhancing the quality of the documentation in the admin guide (see our[new engine configuration docs](https://docs.dagger.io/configuration/engine) )


The new custom configuration file includes features such as custom garbage collection policies, log level configurations, and security settings, all of which take precedence over the engine’s default settings.


Read more:[#9022](https://github.com/dagger/dagger/pull/9022)


**Support for Floating-Point Numbers in the Engine**


Dagger now supports floating-point (` float64` ) numbers inside the engine, enabling more precise calculations in pipelines that require numeric operations.


Read more:[#9396](https://github.com/dagger/dagger/pull/9396)


Along with all of the improvements mentioned above, we merged many bug fixes that simplify your overall Dagger experience. You can see all of the changes made in our last few patch releases[here](https://github.com/dagger/dagger/releases) .


#### What’s Next?


This release reflects feedback from our incredible community of Daggernauts. Your input helps us prioritize features and fixes, so keep it coming!


If you have feature requests, feedback, or ideas, join us on[Discord](https://discord.com/invite/dagger) or submit a[GitHub issue](https://github.com/dagger/dagger/issues) . We’re already hard at work on the next release, and we can’t wait to share what’s coming next.
