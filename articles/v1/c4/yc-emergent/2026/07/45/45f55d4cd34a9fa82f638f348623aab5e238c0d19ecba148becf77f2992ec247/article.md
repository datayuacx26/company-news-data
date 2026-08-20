---
schema_version: "1.0.0"
document_id: "45f55d4cd34a9fa82f638f348623aab5e238c0d19ecba148becf77f2992ec247"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-fc8f7e880d65"
canonical_url: "https://emergent.sh/blog/super-deployer-ai-app-deployment-success"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-21T18:00:00.368523+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:a55b9218bf88537052ced507f7aaa137c9620763115e22566484c0ff464da806"
---

# We Had an 85% Deployment Success Rate. Here's the Assumption We Broke to Fix It.

Before we built Super Deployer, roughly 85% of apps that pushed beyond our standard dependency set simply failed to deploy. Not gracefully, not with a clear error users could act on. They failed, a support ticket got filed, an engineer looked at it, and sometimes there was a fix. Sometimes there wasn't.


Eighty-five percent. That's where we started.


## The Assumption AI Breaks


**‍** There is an assumption baked into every deployment system built before 2022: the person who wrote the code also knows what it needs to run.


In the pre-AI world, that's not an assumption at all. It's just how software works. A developer builds a Python app that uses FFmpeg and a CUDA library. They write the Dockerfile. They know to add apt-get install ffmpeg. They know what the base image needs. The Kubernetes manifest reflects decisions made by a human who understood the full dependency graph before they typed the first line of code.


Emergent breaks this assumption completely.


When our platform builds an app from a prompt, the agent writes whatever code solves the problem. It doesn't ask "will this dependency be available in the deployment environment?" It asks "does this code work?" Those are not the same questions. The gap between them was costing us 15% of our deployment attempts.


Our original pipeline used static, template-based manifests on an individual framework basis: Expo for React Native, a standard Node template, a Python template. We kept pre-installed dependencies minimal to control pod sizes. That was a reasonable engineering decision in a world where developers defined their own runtime requirements. It was a structural failure in a world where an agent might decide your app needs Ollama running locally for inference, FFmpeg for video processing, and Redis for caching because the problem you gave it required all three.


We couldn't solve this by making the templates smarter. The surface area of "what an AI agent might write" is functionally unbounded. **The right solution was something different.**


## Decision 1: Don't Touch the Happy Path. Build a Fallback.


The first instinct was to expand our standard manifests, add more pre-installed dependencies, ship larger base images, and make the templates more flexible. The problem was that any template comprehensive enough to cover all possible AI-generated dependencies would be enormous, slow to pull, and still incomplete. For example, adding Ollama support for the 2% of apps that need it makes the other 98% slower and larger. It added complexity to a path that was already working.


The better framing: deployment failure is a minority event. Most apps deploy fine on the standard pipeline. What we needed was a fallback path that only activated when the standard pipeline failed, and that had no constraints on what it could handle.


**Super Deployer** is not a replacement for the standard pipeline. It's a second-pass agent that fires only on failure. The standard path is unchanged: fast, lightweight, predictable. If it succeeds, Super Deployer never runs. If it fails, the user sees "this is taking a while, sit back and relax," and Super Deployer takes over silently.


The key architectural insight is that a fallback can afford to be expensive, slow, and per-app specific, because it only runs when something is already broken. You don't optimize a fallback for speed. You optimize it for success rate, and you accept the latency cost.


## Decision 2: Write a Manifest From Scratch, for Each App, From Its Own Code.


Standard manifests are templates with variables. Super Deployer doesn't use templates.


When triggered, it reads two inputs: the full application code and the error logs from the failed standard pipeline attempt. From these it generates a completely custom Kubernetes manifest: not an instantiation of a template, but a manifest written from first principles by an agent that has read the code and understands why the previous attempt failed.


The practical difference shows up most clearly at the system dependency layer. Standard package managers (pip, npm, yarn) install application-level dependencies. They can't touch the operating system. If your app needs libopencv, libavcodec, a specific CUDA version, or ffmpeg built with custom flags, those dependencies have to be installed at the system level: apt-get, custom initContainer build steps, sometimes a different base image entirely. The standard manifest has no mechanism for this. Super Deployer reads the code, identifies OS-layer requirements, and writes the installation steps directly into the manifest.


Pod sizing works the same way. A standard CRUD API runs fine at 512MB. An app running an Ollama instance for local inference needs substantially more. Super Deployer reads the code, sees the inference call, and provisions accordingly. It also writes app-specific health checks rather than generic process probes, because a web app that's "running" but hasn't finished loading its model isn't actually ready to serve traffic.


Super Deployer also handles externally-brought code, not just apps built on Emergent. If a user imports an existing project, Super Deployer can help deploy it. The agent doesn't care whether it generated the code or not. It reads what's there and produces a manifest for what it finds.


## Decision 3: Self-Loop on Failure. Don't Escalate.


Super Deployer doesn't write one manifest and wait to see what happens. It deploys, reads its own output, and if it fails, **it revises and retries up to 3 times.**


Each iteration feeds the full error output from the previous attempt back into the agent's context. The agent reasons about what went wrong, produces a revised manifest, and tries again. The loop continues until the deployment succeeds or the iteration cap is reached.


Getting the retry logic right was harder than the architecture. The failure mode you want to avoid is the agent producing cosmetically different manifests each iteration without genuinely advancing toward a solution. The prompt design has to push the agent toward genuine hypothesis revision: "the previous approach failed because XYZ; here is why, and here is a structurally different approach." This took real iteration to get right.


In the worst cases, such as apps with unusual dependency graphs, multiple services that need coordination, or framework configurations we hadn't seen before, Super Deployer's retry loop took longer than usual. We were initially uncomfortable with this.


But the relevant comparison isn't "a slow retry loop versus instant." It's "a slow retry loop versus a support ticket that might take 24 hours and require a senior engineer." In that framing, even the worst cases are extremely fast. And across the full distribution of Super Deployer deployments, the median is well below the ceiling. It's the edge cases that push it.


## How We Built It


We started with a week of manual failure analysis. No code, no agent design. Every deployment failure during that week, we triaged by hand: what broke, why, what category of failure it represented. After a week, the distribution was clear. A small number of failure categories accounted for the vast majority of failures. The long tail was genuinely long, but the top failure categories had clear patterns.


We then applied an 80/20 decision: identify the failure category causing the most failures, solve it manually a few times to confirm the fix was reproducible, then encode it into the agent. "Encode" means two things in practice: either a prompt update that teaches the agent to recognize and handle that pattern ("when you see this error signature, consider installing these system libraries"), or a new tool call the agent can invoke ("here is a library registry you can query to find the correct system package name for a given Python module").


After each category is addressed, we run evals and regression-test prior categories. Agents that improve in one dimension sometimes regress in another. A prompt change that makes the agent better at handling system dependencies can make it more aggressive about pod sizing in ways that break apps that were previously deploying fine. The regression suite is not optional.


**Version 1 of Super Deployer was live one week after we started building it.** It wasn't polished (the retry logic was less sophisticated and the manifest generation made more errors), but it shipped and the numbers immediately improved.


## The Numbers


Deployment success rate for apps with non-standard dependencies, before Super Deployer: ~85%. After: 98.5–99%.


The other number that matters: the break-and-fix cycle that dominated before Super Deployer is now rare. That loop had a real cost in engineering time and user trust.


## What This Gets Wrong


The longer deployment times is a real UX cost for edge cases, and "sit back and relax" stops being reassuring after 90 minutes. We need better communication of progress: not just "it's still working," but "it's on attempt 2, the previous failure was X, here's what it's trying now." That would make Super Deployer feel less like a black box and more like a process with a visible trajectory.


The retry loop is currently opaque to users. The agent is doing real diagnostic work each iteration (reading its own errors, revising its model of what the deployment needs), but none of that is surfaced. A deployment tracer would let users see what the agent is actually doing, which also happens to be the thing most likely to make them trust it. It would also surface the rare cases where Super Deployer is genuinely stuck, so a human can intervene intelligently rather than waiting for the iteration cap.


Super Deployer is still internal. Users have no direct access; it runs as a silent backend fallback. We are considering surfacing it to let users trigger it explicitly, inspect what it's attempting, and understand their deployment environment. The agent is good enough to show, but the interface around it isn't there yet.


## Every CI/CD Pipeline Has a Pre-AI Assumption


The assumption that "code author equals deployment author" is not a problem specific to Emergent. It is going to break for every team that lets AI write a meaningful fraction of their production code.


If your CI/CD pipeline assumes that the person who wrote the code also understands the runtime environment, you have a pre-AI assumption in your stack. That assumption held because it was structurally enforced: a human had to write both the code and the manifest, so they were the same human, and they knew both sides. When an AI agent writes the code, that structural link breaks. The agent that writes the code has no context about the deployment environment. The deployment environment has no context about what the code needs. Something has to bridge that gap.


Static manifests, pre-baked templates, fixed base images: these are artifacts of a model where every dependency decision was made deliberately by the person writing the code. In a world where AI writes the code, you need something that can infer deployment requirements from the code itself, handle errors it's never seen before, and iterate until it gets to working.


The gap is an inference problem. **The right tool for it is an agent.**
