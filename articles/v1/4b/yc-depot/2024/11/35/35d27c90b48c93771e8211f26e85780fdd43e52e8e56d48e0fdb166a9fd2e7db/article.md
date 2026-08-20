---
schema_version: "1.0.0"
document_id: "35d27c90b48c93771e8211f26e85780fdd43e52e8e56d48e0fdb166a9fd2e7db"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/github-actions-runner-settings"
published_at: "2024-11-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:1c05b730beb0d3c7542d77744b3c898b79f95ecc2cbebd5fc06a33ab4d7aa223"
---

# Now available: GitHub Actions Runners settings

Since launching our own GitHub Actions runners, we've been working on automatic configurations for things inside the runner. The idea is that we streamline integrations and provide better performance by offering the ability to have specific tools tuned right out of the box. We're excited to ship these initial settings, which you can now configure inside your Organization Settings in Depot.


## Automatically authenticate with Depot Registry


All Depot GitHub Actions runners launch pre-authenticated to our Depot Ephemeral Registry by **default** now. If you use our Docker image build acceleration inside of your GitHub Actions workflows via` depot build` or` depot/build-push-action` and save your images to our registry with the` save` flag, you can now pull those built images from` registry.depot.dev` without having to authenticate separately.


You can toggle the setting in your Organization Settings to disable automatic authentication.


## Enable the containerd layer store for Docker layers


We've now exposed a setting that enables the containerd layer store for Docker layers on your GitHub Actions runners. This setting is disabled by default, but you can enable it in your Organization Settings.


When enabled, the Depot GitHub Actions runners will default to using containerd as the layer store for Docker layers. This can provide better pull performance as the original layer store from Docker would have to download, verify, and convert from OCI to Docker. With containerd, the layers are already in the OCI format and can be used directly, reducing each layer unpack to download and verify.


## What's next


This is the first set of automatic settings we are surfacing for our GitHub Actions runners. We have more in the works and will be rolling them out in the coming weeks. If you have any feedback or suggestions for settings you'd like to see, please let us know in our[community Discord](https://discord.gg/MMPqYSgDCg) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
