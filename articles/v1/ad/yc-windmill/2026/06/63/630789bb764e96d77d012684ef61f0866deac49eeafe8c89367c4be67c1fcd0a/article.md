---
schema_version: "1.0.0"
document_id: "630789bb764e96d77d012684ef61f0866deac49eeafe8c89367c4be67c1fcd0a"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/sandboxed-container-runtime"
published_at: "2026-06-05T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:f468d7f4aefe526de7614e9cd15c9f6ffa9ea7ffa0eff5f252fba3660aa9c080"
---

# Sandboxed daemonless container runtime

### [Sandboxed daemonless container runtime](https://www.windmill.dev/changelog/sandboxed-container-runtime)


Scripts


[Enterprise](https://www.windmill.dev/pricing)


[v1.719.0](https://github.com/windmill-labs/windmill/releases/tag/v1.719.0)


[Docs](https://www.windmill.dev/docs/advanced/docker)


Bash scripts can now run any container image with the` # sandbox <image>` annotation. The image rootfs is pulled with crane and run chrooted inside the job's own nsjail, so it is daemonless, needs no Docker socket or dind sidecar, and is safe to run for untrusted multi-tenant code. Docker scripts are now allowed on Windmill Cloud.


#### New features


- Run any container image from a bash script with \`# sandbox <image>\` — the body runs inside the image, sandboxed.
- Daemonless: image rootfs is pulled with crane and run chrooted in the job nsjail, no Docker socket or dind sidecar.
- Inherits the job confinement: no host filesystem, the jail own /proc, unprivileged worker uid, the job network.
- Instance settings for pull policy, per-image and cache size caps, default registry, and private-registry auth.
- Now available on Windmill Cloud since containers run sandboxed.
