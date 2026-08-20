---
schema_version: "1.0.0"
document_id: "2eb4684ceb5206f0688276cd7925b221469c2b0aa58083c0035c211073f199de"
company_key: "dropbox-inc-class-a-common-stock"
company: "Dropbox Inc."
source_id: "dropbox-inc-class-a-common-stock-rss-58b41d3bcd8e"
canonical_url: "https://dropbox.tech/infrastructure/reducing-our-monorepo-size-to-improve-developer-velocity"
published_at: "2026-03-25T17:00:00+00:00"
first_seen_at: "2026-07-20T23:21:44.950471+00:00"
fetched_at: "2026-07-28T22:00:16.885857+00:00"
content_hash: "sha256:607fae0a8b17de6b301462e03846fe13c3606e5c710bc8da3ef597cc941813f0"
---

# Reducing our monorepo size to improve developer velocity

At Dropbox, almost every product change flows through a single place: our server monorepo. A monorepo is a single, shared Git repository that contains many services and libraries used across the company. Instead of splitting code across dozens of smaller repositories, we keep a large portion of our backend infrastructure in one place. That architecture makes cross-service development easier, but it also means the repository sits at the center of nearly everything we build.


Building[AI-powered features](https://dropbox.tech/machine-learning/building-dash-rag-multi-step-ai-agents-business-users) at Dropbox often requires small changes across ranking systems, retrieval pipelines,[evaluation logic](https://dropbox.tech/machine-learning/practical-blueprint-evaluating-conversational-ai-at-scale-dash) , and UI surfaces. All of that work moves through the same engineering loop: pull the latest code, build and test it, get it reviewed, merge it, and ship it. Over time, we began to notice that this loop was getting slower. Our monorepo had grown to 87GB; downloading a full copy of the codebase (or “cloning” the repository) took more than an hour, and many continuous integration (CI) jobs were repeatedly paying that cost. We were also approaching GitHub’s 100GB repository size limit, which introduced real operational risk.


In this post, we’ll share how we reduced the repository from 87GB to 20GB (a 77% reduction), cutting the time required to clone the repository to under 15 minutes. We’ll also explain what was driving the growth and what we learned about maintaining a large monorepo at scale.


Dropbox Dash: AI that understands your work


Dash knows your context, your team, and your work, so your team can stay organized, easily find and share knowledge, and keep projects secure, all from one place. And soon, Dash is coming to Dropbox.


[Learn more →](https://dash.dropbox.com/?utm=blogs)


## When repository size becomes a real problem


To understand why repository size matters, it helps to look at how engineers actually work. The first time someone sets up their development environment, they clone the repository, meaning they download a full copy of the codebase and its history to their machine. After that initial setup, daily work is less intensive. Engineers fetch and pull incremental updates rather than redownloading everything. But that first clone is unavoidable, and when the repository reached 87GB, it regularly took more than an hour.


That cost didn’t just affect onboarding. Many continuous integration jobs—automated build and test workflows that run on every code change—begin from a fresh clone. That meant our CI pipelines were repeatedly incurring the same overhead. Internal systems that synchronize the repository were also handling significantly more data than before, which increased the likelihood of timeouts and degraded performance.


At the same time, the repository was growing steadily, typically by 20 to 60MB per day, with occasional spikes above 150MB. At that rate, we were on track to hit the GitHub Enterprise Cloud (GHEC) **** 100GB repository size hard limit within months. The issue wasn’t simply that we had a large codebase. The growth rate itself didn’t match what we would expect from normal development activity, even at Dropbox’s scale. That suggested the problem wasn’t just what we were storing, but how it was being stored.


## When compression backfires


At first, we looked for the usual causes of repository bloat: large binaries, accidentally committed dependencies, or generated files that didn’t belong in version control. None of those explained what we were seeing. The growth pattern pointed somewhere less obvious: Git’s delta compression.


Git doesn’t store every version of every file as a complete copy. Instead, it tries to save space by storing the differences between similar files. When multiple versions of a file exist, Git keeps one full version and represents the others as deltas, or “diffs,” against it. In most repositories, this works extremely well and keeps storage efficient.


The issue was how Git decides which files are similar enough to compare. By default, it uses a heuristic based on only the last 16 characters of the file path when pairing files for delta compression. In many codebases, that’s good enough. Files with similar names often contain related content. Our internationalization (i18n) files, however, followed this structure:


i18n/metaserver/\[language\]/LC_MESSAGES/\[filename\].po


The language code appears earlier in the path, not in the final 16 characters. As a result, Git was often computing deltas between files in different languages instead of within the same language. A small update to one translation file might be compared against an unrelated file in another language. Instead of producing a compact delta, Git generated a much larger one.


Routine translation updates were therefore creating disproportionately large pack files. Nothing about the content was unusual. The problem was the interaction between our directory structure and Git’s compression heuristic. Once we understood that mismatch, the rapid growth of the repository finally made sense.


### Testing a fix locally


Once we suspected that delta pairing was the root cause, we looked for ways to influence how Git grouped files during compression. We found an experimental flag called --path-walk


that changes how Git selects candidates for delta comparison. Instead of relying on the last 16 characters of a path, it walks the full directory structure, which keeps related files closer together.


We ran a local repack—essentially asking Git to reorganize and recompress the objects in the repository—using this flag. The results were immediate. The repository shrank from the low-80GB range to the low-20GB range. That confirmed our hypothesis: the issue wasn’t the volume of data, but how it was being packed.


However, that success exposed a new constraint. GitHub told us that --path-walk


was not compatible with certain server-side optimizations they rely on, including features like bitmaps and delta islands that make cloning and fetching fast. Even though the fix worked locally, it wouldn’t work in production.


We needed a solution that achieved the same size reduction while remaining compatible with GitHub’s infrastructure. That meant working within the parameters GitHub could safely support, rather than relying on an experimental client-side flag.


### Why we couldn't do this alone


Our local experiments proved that better packing could dramatically reduce the repository size. But there was a critical limitation: you can’t repack a repository locally, push it to GitHub, and expect those improvements to persist.


GitHub constructs transfer packs dynamically on the server based on what each client is missing. That means the server’s own packing strategy determines clone and fetch sizes. Even if a local mirror is perfectly optimized, GitHub will rebuild the pack during transfer using its own configuration. To permanently reduce repository size and improve performance, the repack had to be executed on GitHub’s servers.


```text
$ git clone --mirror git@github.com:dropbox-internal/server.git server_mirror
performance: 2795.152366000 s


$ du -sh server_mirror
84G     server_mirror


$ git repack -adf --depth=250 --window=250
performance: 31205.079533000 s (~9h)


$ du -sh server_mirror
20G     server_mirror
```


We shared our findings with GitHub Support and worked with them on a solution that would be compatible with their infrastructure. Instead of relying on experimental flags, they recommended a more aggressive repack using tuned window and depth parameters. These settings control how thoroughly Git searches for similar objects and how many layers of deltas it allows. Higher values increase compute time during repacking but can significantly improve compression.


We tested the approach on a mirrored clone of the repository. The repack took roughly nine hours to complete, but the result was clear: the repository shrank from 84GB to 20GB. Because this method aligned with GitHub’s server-side optimizations, it could be executed safely in production.


## Rolling it out without breaking anything


Repacking a repository changes how billions of objects are physically organized on disk. It doesn’t alter the contents of the code, but it does change the structure underlying every clone, fetch, and push. Given how central the monorepo is to our development workflow, we treated this like any other production infrastructure change.


Before touching the live repository, we created a test mirror and had GitHub perform the repack there first. We monitored fetch duration distributions, push success rates, and API latency to ensure the new pack structure didn’t introduce regressions. The mirror dropped from 78GB to 18GB, and while there was minor movement at the tail of fetch latency, it was well within the tradeoff we were willing to make for a fourfold size reduction. We didn’t observe stability issues.


With that validation in place, GitHub rolled out the production repack gradually over the course of a week. They updated one replica per day, beginning with read-write replicas and reserving buffer time at the end of the week in case a rollback was needed. This phased approach ensured that if anything unexpected surfaced, they could revert safely.


The final result was substantial. The repository shrank from 87GB to 20GB, and clone times dropped from over an hour to under 15 minutes in many cases. New engineers no longer begin onboarding with a long wait. CI pipelines start faster and run more reliably. Internal services that synchronize the repository are less prone to timeouts. And by moving well below GitHub’s 100GB limit, we reduced the risk of platform-level performance degradation during high-traffic periods.


Just as importantly, the system remained stable throughout the rollout. Fetch duration, push success rates, and API latency all stayed within expected ranges. The improvements held without introducing new operational risk.


Project data size dropped significantly and has remained stable since.


## What we learned


Beyond the size reduction itself, this project reinforced a few broader lessons about maintaining large-scale infrastructure. The following three mattered most:


**Growth isn’t just about commit volume** When we first noticed the repository ballooning, the instinct was to look at what was being added: large files, unused dependencies, generated artifacts. But the root cause had nothing to do with the content of our commits. It was about how our directory structure interacted with Git’s compression heuristics. Our i18n paths encouraged Git to compute deltas across different languages rather than within the same language. Routine translation updates were therefore creating oversized pack files. The growth was structural, not behavioral.


Tools embed assumptions. When your usage patterns diverge from those assumptions, performance can degrade quietly over time. In our case, Git’s 16-character path heuristic worked as designed. It just didn’t work well with our repository structure. Understanding those internal mechanics was what allowed us to diagnose the issue correctly.


**Some fixes require working with your platform provider** We were able to identify the root cause and even validate a fix locally. But because GitHub determines how repositories are packed and transferred, a local repack wasn’t enough. The solution had to align with GitHub’s server-side infrastructure.


That meant bringing clear data to GitHub, testing collaboratively, and working within supported parameters. When your system depends on a managed platform, some problems live at the boundary between your code and theirs. Having strong relationships and a shared debugging process makes a meaningful difference.


**Treat repo health like production infrastructure** A repository repack changes the physical structure of billions of objects. Even though the code itself doesn’t change, every engineer and every automated system interacts with that underlying structure. We approached this project the same way we would approach any production infrastructure change: test on a mirror, measure real-world impact, roll out gradually, and maintain a rollback path.


Repositories can feel like passive storage, something that simply grows over time. At scale, they are not passive. They are critical infrastructure that directly affects developer velocity and CI reliability. As part of this work, we built a recurring stats job that tracks key health indicators for the monorepo and feeds them into an internal dashboard. It monitors things like overall repository size, how quickly that size is growing, how long a fresh clone takes, and how storage is distributed across different parts of the codebase. If growth starts accelerating again or clone times begin creeping up, we'll see it early rather than discovering it when engineers start feeling the pain. Monitoring growth trends and investigating anomalies early is part of running a healthy engineering organization.


## What’s next


Reducing the repository from 87GB to 20GB had an immediate impact on how we build. New engineers can get started in minutes instead of waiting through a lengthy initial clone. CI pipelines spin up faster and run more reliably. Teams working on AI features—where progress often comes from many small, iterative changes across multiple services—feel that improvement in every development cycle.


The investigation also led to structural changes designed to prevent the same issue from resurfacing. We updated our i18n workflow to align more closely with how Git’s packing algorithm groups files, reducing the likelihood of pathological delta pairing in the future. Just as importantly, we now have better visibility into repository growth trends and a clearer understanding of what “normal” looks like.


More broadly, this project gave us a repeatable playbook. When growth accelerates unexpectedly, we know how to investigate at the compression layer, how to validate fixes safely, and how to work across platform boundaries when necessary. Monorepos will continue to grow as products evolve, but growth doesn’t have to mean friction. With the right tooling and discipline, it can remain invisible to the engineers who rely on it every day.


*Acknowledgments: Samm Desmond, Genghis Chau*


~ ~ ~


*If building innovative products, experiences, and infrastructure excites you, come build the future with us! Visit*[jobs.dropbox.com](https://jobs.dropbox.com/) *to see our open roles.*
