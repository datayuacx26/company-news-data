---
schema_version: "1.0.0"
document_id: "cfee82f646f4cf62fb91c081c223beea3e4101da9251027e9dfc095c7ae317b9"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/offload"
published_at: "2026-03-17T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:fa4b22b84d7f045b9de16750851bccb9c5b729d83ce7b335d14eec5d051f2e50"
---

# Offload: We made agents run our test suite 6x faster

Has this ever happened to you? You’re running 6 parallel agents on a mature codebase where the tests take about 12 minutes to run. Next thing you know, you’re waiting over an hour for each agent cycle to complete, and test flakes and timeouts are driving you wild.


We developed **Offload** to solve for this.


Offload “outsources” your test suite to run on 50 to 200+ cheap, lightweight sandboxes. Implemented in Rust, Offload leverages Modal’s on-demand serverless infrastructure to free your computer from the constraints of your existing test suite.


We found that **our agents can run a real integration test suite with Offload more than 6 times faster** .


Utilizing Offload enabled a 6x speedup for our integration tests over our baseline. In contrast, adding more xdist workers was already producing sub-linear speedup.


## We’re testing the limits of parallelism


In October 2025, Imbue launched[Sculptor, our coding agent environment](https://imbue.com/sculptor) designed for parallelism and safety. We’ve become heavy users of the tool, and we grew accustomed to coordinating multiple coding agents in parallel. As we adapted to increasing model capabilities and shared our learnings, we noticed recurring patterns.


Improving model capabilities enabled us to drive agents for longer by providing **higher-level requirements and deeper challenges** . Our engineers have been competing to outdo each other in how many hours of continuous turns they could achieve without human feedback, while still obtaining code we felt proud of. We also set up background agents to propose automated fixes to the codebase.


Naturally, our software projects grew in features and code size. Accustomed as we were to hand-crafting code, we had established intuitions and high standards that we weren’t willing to compromise on. The goal was to balance ensuring that generated code was **understandable, maintainable and verified** , while going as fast as possible.


### Parallel agents need guardrails


As we challenged agents with greater complexity and scope, we discovered that **externally-enforced standards are critical** . We consider tools such as typecheckers, style guides, linting and ratchets indispensable to ensuring high-quality code produced by the agents. We also rely heavily on[Vet](https://github.com/imbue-ai/vet) , our recently open-sourced automated reviewer to catch mistakes in generated code.


Our projects also rely on a **comprehensive suite of integration tests** . The cost to generate these tests has dropped. This enables the specification of every meaningful, user-visible attribute of software with tests. With tests, our armies of agents can modify software with confidence and cohesion intact.


It turns out that **AI-generated software projects want to have large integration test suites** .


## The testing bottleneck


If your experience is similar to ours, this is where you ran into the testing bottleneck. You want to stay in flow, setting up tasks for parallel agents and reviewing and merging their results. Before demanding your attention, you want the agent to verify as much as it can—including all the integration tests.


To expand on the example in the introduction, the Sculptor integration suite comprises **345 Playwright tests** . When we run these tests locally with` pytest` and the standard 3` xdist` workers it takes ~12 minutes per run. Adding on more parallel workers increases the contention for resources, slowing down results and risking flaky test runs.


The bundled skill enables the agent to leverage Offload naturally to validate its changes.


One solution to reduce flakiness is to globally lock integration test executions. Permit agents to generate and iterate on code in parallel using the safer unit testing suite, but force them to queue for an integration testing checkmark before showing the code to a user.


In either case you find your momentum hobbled. While you would rather be approving completed pull requests, you’re stuck with growing test execution times or watching stalled agents await their turn for testing. **Your workflow that started off parallel has become serialized** waiting for integration tests to complete.


## Introducing Offload


Offload is an **open-source tool (released under MIT license)** aimed at bringing remote test execution infrastructure to your software project. Our tool is implemented in Rust to take advantage of[tokio](https://tokio.rs/) ’s support for asynchronous tasks. Offload has out-of-the-box support for **Pytest, Cargo-Nextest and Vitest** , and it is designed to easily be extensible for other testing libraries. Crucially, Offload wants to meet your project exactly where it is by integrating with your existing test framework and test suite.


We provide an **onboarding agent skill** to set your project up. If you prefer to configure it manually, Offload relies on a few conventions and a simple configuration file that you write and check into your codebase.


When invoked, Offload prepares and caches an image of your code and dependencies on Modal’s infrastructure. Your tests then run on **“burst compute”** , effectively spinning up as many lightweight sandboxes as you requested. Offload intelligently schedules your tests in batches to minimize the total runtime of your suite.


We also thought about the **ergonomics of Offload for debugging** within the agent loop. For example, Offload supports retrieving the logs of specific failures via test ID, in order to avoid blowing up the context of the agent.


Offload also protects you from flakiness in your test suite in three main ways.


**Isolation eliminates bugs caused by parallelism.** Correctly using local parallel test execution frameworks like` xdist` takes additional disciplined engineering effort. Both your system and test suite need to be designed for parallelism, regardless of your original requirements. In contrast, with Offload you can say goodbye to incidental problems such as port collisions or tempfile data races.


**Remote execution avoids local performance variance.** Because all your test batches are running with dedicated resources, you completely sidestep the problem of performance degradation due to contention. Since a common Playwright test pattern is to assert a condition resolves to true before a deadline elapses, performance slowdowns can turn into test failures. This is a class of failures that is also excluded.


**Preemptive retries handle genuinely flaky tests.** Sometimes, the tests we write are genuinely flaky. I cannot cast the first stone. Offload has a mechanism to annotate flaky tests, and request that they be preemptively rerun in advance multiple times. This avoids having to rerun the entire suite for a single flaky failure.


## Offload in practice


**We cut the execution time of the integration test suite in Sculptor by 6x!** Additionally, because we run the tests on remote workers, this frees substantial local resources. By giving you the ability to run tests without burdening your local system, Offload increases both your cycle iteration speed and overall throughput.


The primary impact of utilizing Offload should be that **more parallel agents will be able to complete their work faster** . This reduces the amount of time that you spend waiting for work to complete, and keeps you in flow.


Another project at Imbue sees more modest gains from deploying Offload.


Of course, not every project benefits the same amount. The amount of speedup achievable will depend on factors such as the total duration of running the test suite and the distribution of lengths of the tests. **We found that it’s hard to see a measurable speedup if your test suite takes less than 2 minutes in total.**


At the same time, Offload also **reduces flakiness in integration pipelines** by providing isolation for test batches. Because this saves you (or a hapless agent) from having to re-run a large test suite, this saves quite a bit of time.


Another important impact is that this allows us to more effectively **manage CI costs** , since we can utilize accurate-to-the-second billing on Modal, rather than needing beefy CI workers on our projects. A complete run of Sculptor’s 345-test integration suite costs about **8 cents on Modal** .


## Get started with Offload


If you’re deploying parallel agents on a repo with an integration testing suite that takes more than a couple of minutes to run, Offload might belong in your toolbox.


Get started with Offload by running the following command to install the “offload-onboard” skill:


```text
sh
```


You can then ask your agent to install Offload for you directly by invoking the offload-onboard skill:


```text
sh
```


Alternatively, you can download the latest version of Offload from[crates.io](https://crates.io/crates/offload) with:


```text
sh
```


While we recommend the onboarding skill, Offload’s configuration is simple. An` offload.toml` might look like:


```text
toml
```


And finally, we open-sourced Offload on[GitHub](https://github.com/imbue-ai/offload) . Share feedback with us there, or join our[Discord](https://discord.com/channels/1391837726583820409/1483337125762957342) server to chat with our team.
