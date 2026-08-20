---
schema_version: "1.0.0"
document_id: "7d4cf10d9b62812733e6d28a3419fccc442353e47a52c3a9f985ef00ef64a136"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/turbo-builders"
published_at: "2023-02-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:29fa624188f9b79f8443d52ec0ce762ef22dd81aac59f44ad49444f7b1308253"
---

# Turbo Builders are now available

Since launching in July, we have focused on becoming the fastest place to build Docker images. We are constantly evaluating different ideas for making image builds faster, and we are excited to announce that we have rolled out a significant upgrade across our entire provisioning system.


Starting today, all Depot-hosted projects above our free tier will run on larger builder machines that we are calling Turbo Builders. These machines are 4x the size of our previous builder machines and come with **16 CPUs & 32 GB of memory** . They are available for both Intel and Arm builds without any additional configuration needed on your part.


We are already seeing a massive performance gain in our own benchmarks. For example, we have seen our multi-platform[Mastodon benchmark](https://depot.dev/benchmark/mastodon) get as high as **53x faster** compared to Docker inside of GitHub Actions when running on Turbo Builders.


You should see a nice performance boost for image builds that can leverage multiple CPUs and additional memory on top of your already fast builds. We are excited to see what you build with these new machines. Be sure to pop into our[Discord Community](https://discord.gg/MMPqYSgDCg) and let us know what you think!


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
