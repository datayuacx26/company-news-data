---
schema_version: "1.0.0"
document_id: "a2b9fe379bbeed4de2dd50cea8a198c7d1920a86f685ab0158049abf6a29d183"
company_key: "uipath-inc-class-a-common-stock"
company: "UiPath Inc."
source_id: "uipath-inc-class-a-common-stock-rss-2f83a748bf9d"
canonical_url: "https://engineering.uipath.com/contract-based-test-automation-framework-fa01e0e1be60"
published_at: "2025-10-10T07:45:18+00:00"
first_seen_at: "2026-07-20T23:16:59.255384+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:44a5d47635343d546b406d8114bccbe982dd3972aa77a3e39c6f4a81b738342d"
---

# Contract-Based Test Automation Framework

# Contract-Based Test Automation Framework


[Bogdan Cucosel](https://medium.com/@bogdanutz?source=post_page---byline--fa01e0e1be60---------------------------------------)


5 min read


·


Oct 10, 2025


--


To keep UiPath shipping safely across UiPath Automation Cloud™ — our cloud offering and Automation Suite — our Kubernetes-based on-premises solution, we standardized on a **contract for end-to-end tests (Athena)** and ran them inside **ephemeral, hermetic environments (ETE)** . Around this core, we built shared **API clients** , a **declarative data-provisioning engine** , and a **flexible test automation framework** — so teams can write tests once and run them anywhere, with less flakiness and duplication.


## Background: Scale, topology, and the quality bar


The UiPath Platform™ spans multiple products, teams, and delivery models (on-premises, cloud, FedRAMP). Ensuring changes remain shippable across these surfaces — while deployment cadences differ — requires integrated testing that mirrors real usage, not isolated mocks.


## Where we started


- **Isolated tests.** Teams ran suites in non-integrated setups with mocked dependencies (great for unit speed, bad for catching contract gaps and corner cases).
- **Contention on shared environments.** “Always-on” test environments caused serialized runs, drift, and corruption during infra changes — plus unnecessary cost.
- **Flakiness from cadence mismatch.** Automation Cloud™ shipped bi-weekly, Automation Suite shipped less frequently — integration lag introduced infra-flakiness.
- **Polyglot drift.** Teams scripted their own stacks; tests weren’t portable across environments.
- **DIY data seeding.** Every team rebuilt API clients to prepare cross-component test data.


## Goals


- **Write once, run everywhere** (cloud rings, Automation Suite, ephemeral environment)
- **Reusable artifacts** with clear contracts
- **Short-lived, clean environments** for testing every change
- **Main branches always shippable** via left-shifted gates


## Key concepts: Glossary


- **System Under Test (SUT):** the newer payload of the component of the system we’re validating
- **End-to-End test:** exercise the system like a real user across components
- **Hermetic environment:** bundles the SUT and its last known good dependencies to remove external flakiness
- **Ephemeral environment:** created on-demand pre-merge, destroyed after tests
- **Left-shifted checks:** run quality gates before merging to release branches
- **Change lifecycle stages:** pre-merge, post-merge, stability, deploy, post-deploy/post-upgrade, synthetics


## Solution Part 1: Ephemeral Test Environment (ETE)


We create, patch, test, and destroy a full **Automation Suite** instance on demand. Implementation details are abstracted behind **frontend templates** (the contract) so teams can evolve internals while maintaining a stable interface. Typical PreMerge flow: build the component → deploy ETE → patch with the new build of SUT → run tests → collect logs → tear down.


**Keeping ETE fresh**


Nightly, we build and test major Automation Suite branches, and snapshot each one — failing snapshots are retained for investigation but not published — so consumers always pick a good base.


**Hermetic by design.**


## Get Bogdan Cucosel’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


We bake external dependencies into the snapshot (e.g., a **mini licensing server** ) to remove external calls that cause flakiness.


Press enter or click to view image in full size


ETE snapshot lifecycle (install → snapshot → deploy, snapshot → patch)


**What ETE brings**


- Integrated component tests in a clean, known-good state
- No shared-environments contention or drift
- No external dependencies → less flakiness and faster signal


## Solution Part 2: Athena — A contract for tests


**Athena** defines how a test **executor** invokes a test **implementer** . The executor provides SUT details (FQDN, credentials, test type, etc) — the implementor returns results in a standard format. Extras can also include a **random seed** for reproducible data, and lastly, the ability to **persist state across stages** (e.g., pre/post upgrade). The current packaging is a **Docker container** that each team publishes and targets to **UiPath Automation Cloud™** , **all ETE lifecycle stages** , and **Automation Suite** .


Athena contract (folders, entrypoints, variables)


**What Athena brings**


- **Write tests once, run anywhere** (consistent invocation across platforms)
- **Polyglot freedom, standardized execution**
- A stable surface for building tooling


## Solution Part 3: Shared API clients


To avoid every team re-implementing clients, each component **generates and publishes its own API client** in the build pipeline. PR checks prevent “forgot to bump/regenerate” errors — the **API version is embedded** in component code so we can determine compatibility from the running container. For critical components, we add **business clients** atop the API to hide async/complex flows from test authors.


**What Shared API Clients bring**


- Consistent interoperability across component
- Duplication removal across board.


## Solution Part 4: Declarative data provisioning


Developers describe *what* data they need — an execution engine that uses ‌shared clients to provision across components, adapting to **SUT version differences** and breaking API changes.


Execution *engine calling Identity/OMS/Licensing/OR clients from a simple declaration.*


## Solution Part 5: Test Automation framework flexibility


To support a wide range of testing needs, UiPath enables teams to choose the right framework for their scenario — whether it’s low-code, code-first, unit testing, or full integration validation. This flexibility ensures consistent, reliable automation across web, desktop, APIs, and applications.


- **UiPath Studio Coded automation tests / Low-code automation —** Code-first UI testing with extensions, activity packages, and Studio Web libraries for simpler API-based automation.
- **Wdio tests —** Browser automation and UI testing with WebdriverIO.
- **Playwright tests —** Fast, reliable cross-browser UI testing with Playwright.
- **XUnit tests —** .NET unit testing with the xUnit framework.
- **NSpec tests —** Behavior-driven testing for .NET applications.
- **API integration tests —** Automated validation of APIs and system integrations.


## How it all fits together


Across the lifecycle, we run **Athena-based tests** in the right environment:


- **Pre/Post-Merge:** build the component, deploy to **ETE** , run Athena.
- **Automation Cloud CD:** deploy to a ring, validate with Athena.
- **Automation Suite CI (AKS/EKS/GCP):** deploy the suite, run Athena for all components.


Press enter or click to view image in full size


*Pipeline diagram mapping Athena to ETE, cloud, and Automation Suite.*


## Challenges and lessons


- **Contract adoption:** Moving every team to publish an Athena container takes coordination.
- **Hermetic ≠ divergent:** snapshots must reflect reality without re-introducing shared environments flake.
- **Versioning hygiene:** automatic checks are essential to keep clients honest
- **Declarative beats imperative:** teams should declare what they want to do. The mechanism to do it should be central
- **Listen and generalize** : teams have different requirements — generalize and incrementally modify the contract
