---
schema_version: "1.0.0"
document_id: "aed068ba6cd246cd22924f0c56db91af0ecd4032b267c84658a56f895faaae61"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/signadot-operator-cli-hit-1-0-now-with-istio-ambient-support-and-enhanced-local-dev/"
published_at: "2025-07-02T15:30:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:0a80fe8e6d9c49ddea63b55d717fdd0f6e940cdd5f22d665a0ce77d2fa65724b"
---

# Signadot Operator & CLI Hit 1.0: Now with Istio Ambient Support and Enhanced Local Dev

We’ve been listening closely to our community, and this month’s updates are a direct result of your feedback. Our focus has been twofold: hardening the core platform and bridging the gaps in the local development experience. With that, we’re excited to announce that our Operator and CLI are officially graduating to version 1.0.


This isn’t a release that introduces breaking changes, but rather a statement of stability. After being used to create tens of thousands of sandboxes for customers across the world, our core components have proven their reliability. The 1.0 milestone signifies that they are battle-tested and ready for prime time, now with long-awaited support for Istio’s ambient mesh and a supercharged local development workflow.


## Operator 1.0: Finally, Sandboxes without the Sidecar Tax


While service meshes are powerful, sidecars can be a drag. They add complexity, consume resources, and can be a pain to manage. Istio’s ambient mode has been on everyone’s radar as a potential solution—the promise of a mesh without the per-pod overhead.


With our Operator 1.0 release, that promise becomes a reality for Signadot users. We now added experimental support for sandbox routing in Istio ambient mode. This is a game-changer. You get all the benefits of Signadot’s request-level isolation for ephemeral environments without the sidecar tax. It’s a cleaner, more efficient way to give your developers the high-fidelity, isolated testing they need. Check out the[release notes](https://www.signadot.com/docs/release-notes#operator-v100) for the details.


## CLI 1.0: Closing the Gap Between Local and Cluster


“But it works on my machine!” is the oldest excuse in the book, but it’s often a symptom of a real problem: a mismatch between local and cluster configurations. True high-fidelity local development means more than just running your service’s code; it means running it with the same environment variables, secrets, and configmaps it uses in the cloud.


Our 1.0 CLI release is all about closing that gap. You can now fetch secrets and configmaps from your Kubernetes cluster and inject them, along with other environment variables, directly into your local development environment. This makes` signadot local` an even more powerful tool for catching issues early, ensuring that when it works on your machine, it will work in the cluster, too. The[docs on local sandboxes](https://www.signadot.com/docs/reference/cli/sandbox) have the full rundown.


## And One More Thing… Our Browser Extension is Now Open Source


In the spirit of transparency and community, we’ve also open-sourced our browser extension. This little tool makes it easy to route traffic to your sandboxes, and now you can see exactly how it works, contribute your own improvements, and help shape its future. You can find the code and join the fun over at our[GitHub repository](https://github.com/signadot/browser-extension) .


We build these tools to solve the real-world problems we see teams struggling with every day. Take these new releases for a spin and let us know how they’re working for you.
