---
schema_version: "1.0.0"
document_id: "a40ccfa715329de4aeed745d7b9a27315c1f5b4d5d8cae071f8cc09afe7b7b86"
company_key: "yc-revyl"
company: "Revyl"
source_id: "yc-revyl-news-import-bdee0e34c3b5"
canonical_url: "https://revyl.com/blog/mobile-infra-for-coding-agents/"
published_at: "2026-02-13T00:00:00+00:00"
first_seen_at: "2026-07-25T21:41:44.227724+00:00"
fetched_at: "2026-07-28T22:20:29.370610+00:00"
content_hash: "sha256:6ac559f77e99680a6fb69d300f687b3356b9679272e8c4b6c40e2c3c33b01f46"
---

# Building device infrastructure for mobile AI agents

Every major tech company is building coding agents. The frontier is moving from “write code” to “interact with the product.” But the moment your agent needs to tap a button on a real iPhone, you hit an infrastructure problem that has nothing to do with AI.


We know this because we built it before. At Uber, we created[DragonCrawl](https://www.uber.com/blog/generative-ai-for-high-quality-mobile-testing/) — the first AI-powered mobile testing system to run in production at scale. It executed Uber’s core trip flow across 85 cities with 99%+ stability, zero maintenance, on every Android code change. The models were the easy part. The infrastructure to actually run them against real devices was where we spent most of our time.


Now we’re building Revyl, and the problem is the same. There’s been a lot of progress on getting AI to reason about what to do on a screen. But getting it a real device to act on — with low-latency streaming, clean state, and verified execution — is a different kind of hard. That’s the infrastructure iceberg.


• • •


## The iceberg


We wanted to get to a single CLI command: tell the device what to do in natural language, and have it happen. Here’s what that actually required.


revyl device tap —target “the login button”


Dedicated device fleet management


Layered host provisioning, runtime isolation, and capacity orchestration


Device boot, clean-state erasure, port allocation


H.264 WebRTC streaming at 60 fps


Multi-model AI grounding with self-correction


Native action execution + visual verification


Distributed orchestration with sticky device routing


OpenTelemetry observability across every layer


One command on top. Eight layers of infrastructure underneath, each with its own failure modes, each harder than you’d expect.


• • •


## The seven layers


Here’s what we actually had to build. Each of these is its own project, with its own set of surprises.


01


Dedicated Device Fleet


We run a dedicated device compute fleet. Provisioning turned out to be one of the hardest parts: the platform has to continuously reconcile host state, runtime images, isolation boundaries, telemetry, rollout safety, and recovery behavior across a moving OS and toolchain surface. The result is a control plane that can absorb churn, drain capacity, recover failed workers, and roll forward quickly without exposing that complexity to the agent.


Hard


Emulators leak resources. Platform updates can shift low-level assumptions underneath you. Someone has to be on-call for hardware failures at 2am.


02


Device Provisioning


Android emulators cold-boot in 1-5 minutes. iOS simulators need to be erased between runs for hermetic isolation, then booted, then connected to a companion daemon. We added pre-warming to cut first-task latency by 30-60 seconds, but getting it right meant handling all the silent failures.


Hard


Boot processes hang without error. Port conflicts across concurrent sessions produce cryptic failures. Clean-state guarantees require careful lifecycle management that’s easy to get subtly wrong.


03


Real-Time Video Streaming


We stream H.264 video at 60 fps from real devices via WebRTC, anywhere in the world. A local ring buffer lets the AI “watch” what just happened. The pipelines are self-healing — they auto-restart on codec failures without losing the session. Getting here required a lot of time with GStreamer.


Hard


Codec negotiation, WHIP/WHEP handshakes, resolution mapping between logical and physical pixels. One misconfigured buffer adds multi-second latency. One codec mismatch kills the stream silently.


04


AI Element Grounding


”Tap the login button” needs to become precise pixel coordinates. We use a reasoning model to understand intent and a vision model to locate the element on screen. When grounding fails, the system retries with expanded search regions. No CSS selectors, no accessibility IDs — just the screenshot and the instruction.


Hard


Vision models hallucinate coordinates. Screen densities vary wildly across devices. Dynamic UIs shift elements between the moment we capture and the moment we execute. Getting to production-grade accuracy took months of iteration.


05


Action Execution & Verification


The loop is: execute, wait, verify. Every action is visually confirmed before we move on. We support the full mobile vocabulary — tap, swipe, type, long press, pinch, drag, scroll — using platform-native APIs (UIAutomator2 for Android, XCTest/IDB for iOS) with tiered retry logic.


Hard


ADB commands hang without warning. XCTest runners timeout silently. Keyboard state is unpredictable across OS versions. Animation timing varies per device, so “wait for the screen to settle” is harder than it sounds.


06


Parallel Orchestration


We needed to go from 1 session to 10,000+ concurrent sessions. Dedicated device pools handle platform-specific capacity. A workflow orchestrator runs suites in parallel with automatic capacity management. Every session gets a clean, isolated device.


Hard


Devices are stateful — you can’t just spin up a container. We had to build sticky routing, distributed concurrency control, pre-warming to avoid 3-5 minute cold boots, and queue-depth autoscaling. All of it custom.


07


Observability


We instrumented everything with OpenTelemetry: grounding latency, click accuracy, action time, streaming health, provisioning phases. Every session gets a full video recording for post-mortem review.


Hard


When something fails, you’re tracing across LLM, device, streaming, and orchestrator simultaneously. Building the observability layer was as much work as the infrastructure itself.


• • •


## The interface


Seven layers of complexity, one CLI:


# AI-grounded device actions — no selectors, no coordinates


$


revyl device


tap


—target


“the login button”


Verified. Step complete.


$


revyl device


type


—target


“email field”


—text


“user@company.com ”


Verified. Step complete.


$


revyl device


swipe


—target


“the feed”


—direction


up


Verified. Step complete.


# Expose as MCP tools for any coding agent


$


revyl mcp


MCP server running. Agent can call device actions directly.


The CLI is the primary interface. You say what to do in natural language, and we handle the screenshot, grounding, execution, and verification loop behind it.


We also expose everything as an MCP server —` revyl mcp` turns the device into a tool that any agent framework can call directly, whether it’s Claude, GPT, or something custom. The live video stream is always available for continuous perception-action loops.


• • •


## Performance


We optimized for the tight loop an AI agent needs: capture the screen, ground the instruction, execute the action, verify the result. Here’s where we ended up compared to other approaches.


State Polling Latency


Revyl


~50ms


In-house


~400ms


BrowserStack


~650ms


Sauce Labs


~700ms


Time to First Device Action


Revyl


~8s


BrowserStack


~45s


Sauce Labs


~60s


In-house


~180s


Max Concurrent Device Sessions


Revyl


10,000+


Sauce Labs


~50


BrowserStack


~25


• • •


## Numbers


60


FPS streaming


Multi-stage


fleet provisioning


10,000+


Parallel sessions


~50ms


Polling latency


3


Compute tiers


0


Selectors required


## Architecture


The full request path:


Your Agent


→


Revyl API


→


Orchestrator


→


iOS Device Pool


Android Runtime Pool


→


Device Action


→


WebRTC Stream


→


Verified Result


Your agent talks to our API. We provision the device, stream the screen, ground the instruction, execute, verify, and return the result. You focus on the intelligence.


• • •


## Try it


$


curl -fsSL[https://revyl.com/install.sh](https://revyl.com/install.sh) | sh


or


$


brew install RevylAI/tap/revyl


[View on GitHub →](https://github.com/RevylAI/revyl-cli)


If you’re building an agent that needs to interact with real mobile devices, we’d like to help. Reach out atanam@revyl.ai or[book a demo](https://scheduler.zoom.us/daniele-proano-8qxkpf/revyl-demo) .
