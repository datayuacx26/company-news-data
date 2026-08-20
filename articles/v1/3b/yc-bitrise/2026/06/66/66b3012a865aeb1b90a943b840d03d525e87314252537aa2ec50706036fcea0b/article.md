---
schema_version: "1.0.0"
document_id: "66b3012a865aeb1b90a943b840d03d525e87314252537aa2ec50706036fcea0b"
company_key: "yc-bitrise"
company: "Bitrise"
source_id: "yc-bitrise-news-import-b747fb40f1b3"
canonical_url: "https://bitrise.io/blog/post/bitrise-remote-dev-environments-beta-launch"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-24T03:43:06.304736+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:f75e5e7eddb9d5dd0ba223538751dc477b6cd02edf623b9e2f8d9fd21cde3ce6"
---

# The environment tax: why we built Bitrise Remote Dev Environments

*AI-assisted builds grew 164x in a year. The rate teams ship to users fell by a fifth.* ***Arpad Kun, VP of Engineering and Infrastructure at Bitrise*** *, on the gap that opened up in between, and what his team built to close it.*


## **The day a developer's agent outran her tooling**


A staff engineer on one of the teams we work with recently told me about her Tuesday. By lunch she had produced three features worth of code, most of it drafted by an agent running in her editor while she reviewed and steered. Builds and tests passed on her laptop, but then unexpectedly failed in CI. All of that morning’s productivity suddenly hit a speedbump.


So where is the problem? It’s not the staff engineer and it’s not her AI tooling. The new bottleneck that’s preventing her from doing her best work is an environment not suitable for AI-driven productivity.


That Tuesday is a preview. AI Agents are about to do a lot more coding and every change they produce still has to be built and tested somewhere. Our own internal data confirms that AI-assisted builds grew 164 times year over year, while the rate teams actually shipped changes to users dropped by more than a fifth. Code generation stopped being the constraint, so the constraint moved downstream, into building, testing, reviewing, and the environment all of it runs in.


That gap is why we built[Bitrise Remote Dev Environments](https://bitrise.io/platform/remote-dev-environments?utm_medium=other&utm_source=blog&utm_campaign=rde_announcement_rde-beta-launch_all_2026-06-30&utm_content=blog-product-page) .


## **Everyone is paying an environment tax. The currency just differs**


Every engineer needs a stable and consistent environment to do their best work, and increasingly the AI agents working alongside them need one too.


If the environment drifts or just is not available anymore (older Xcode versions, etc.) or if the compute capacity is lacking there is a price to be paid. We can think of this as an “environment tax” that each contributor pays differently:


- **An agent** pays in tokens. Set it loose on a build or test failure caused by a slight environment drift or a mismatched dependency version, and it can burn enormous amounts of tokens chasing a trivial problem: why the code that ran on a laptop fails in CI, why something that built a minute ago doesn't build now. Those tokens buy nothing. They go to re-deriving a setup that should have been correct on arrival.


- **A human** pays in something scarcer: their attention. Every time you stop solving the problem and start solving your machine, the flow state goes, the hour after lunch lost to reproducing a CI failure locally, or a local failure you can't reproduce in CI. And it's compute, too. More than one team has told me they can't take a video call while a build runs, because the laptop is pinned and the fans are screaming. Development has changed. We run things in parallel now, steering several agents at once, and a single laptop was never meant to carry that load.


We treat these as two problems, an AI infrastructure problem and a developer experience problem. They are one problem, with one root: the place code gets written is not the place it gets built and tested. That gap is where the tax lives.


## **Go tax-free with Bitrise RDE**


Today we're opening[Bitrise Remote Dev Environments](https://bitrise.io/platform/remote-dev-environments?utm_medium=other&utm_source=blog&utm_campaign=rde_announcement_rde-beta-launch_all_2026-06-30&utm_content=blog-product-page) in beta. It runs on the exact same virtual machines, stacks, and cache infrastructure as your Bitrise CI and Build Hub builds. Not a similar setup. The same one.


We’ve built it so that your agents can do their best work. You can use Claude Code, Codex, or your favorite coding agent, or connect from VS Code, Cursor, JetBrains, or SSH, and write code inside the environment that is going to build and test it in the CI phase. Essentially humans and/or agents can work and run the same verifications in the same environment as the CI environment.


Bitrise RDE


That one change is what makes the tax disappear. "Works on my machine" stays true for a structural reason: your laptop and your CI are two environments that happen to agree most of the time, and every time they disagree, someone pays. Write code in the same place you build it and that mismatch has nowhere left to live. It stops. The agent stops spending tokens re-deriving the setup. The engineer gets the afternoon back.


In addition, by starting multiple VMs, same or different stack versions, you can horizontally scale yourself and your agents.


## **The same engine, expanded sideways**


Bitrise CI and Build Hub already build and test at scale on real macOS, on Apple Silicon, with the full Xcode toolchain and build caching for Bazel, Gradle, and Xcode. RDE is that same engine, expanded sideways so a human or an agent can build and iterate inside it. You inherit the caches, the machine classes, and the toolchain your builds already use. There is nothing left to drift.


It also scales the way cloud infrastructure scales, not the way laptops scale. We have heard from teams who bought extra Mac minis just to have somewhere to run agents in parallel, which is a strange place to end up: a closet of hardware doing cloud-shaped work. The engine runs as many environments as you need, pre-warmed and ready. Archive one when you are done, restore it later with its disk intact, and pick up Monday where you left off Friday without paying for idle compute.


## **Built for software engineers and agents**


Every coding agent needs a build environment and feedback loop, and it shouldn't be an engineer's computer just because it was the most convenient to set up initially. A laptop was never going to be that place, and a fresh VM per task configured for purpose pays the setup tax every time.


RDE gives an agent an environment that is correct from the first second. Claude Code and Codex come pre-installed and run on your own API keys. You orchestrate parallel agents through our open-source MCP server, so connecting a fleet is a documented step, not a weekend project. And because RDE runs on the infrastructure your security team already reviewed for Bitrise, that approval should already cover it.


## **Join our beta now**


RDE is open as a self-serve beta. Spin up an environment, connect your editor or your agent, run real work in it, and tell us where it breaks. The feedback from this beta is what GA gets built on.


[Try RDE now](https://app.bitrise.io/dev-environments?utm_medium=other&utm_source=blog&utm_campaign=rde_announcement_rde-beta-launch_all_2026-06-30&utm_content=blog-product-cta)


Need help with setup or want to learn more? Read our[documentation.](https://docs.bitrise.io/en/bitrise-rde?utm_medium=other&utm_source=blog&utm_campaign=rde_announcement_rde-beta-launch_all_2026-06-30&utm_content=blog-documentation)


Want to shape what this becomes before the full release? That conversation is happening in the[#rde Slack community](https://join.slack.com/t/bitrise/shared_invite/zt-3ycxzv91u-hsp5uw9wcromjndijchjfw?utm_medium=other&utm_source=blog&utm_campaign=rde_announcement_rde-beta-launch_all_2026-06-30&utm_content=blog-slack-invite) .


### **FAQ**


**What is a Remote Dev Environment?**


An on-demand cloud machine you develop inside, running on the same virtual machine stacks as your Bitrise CI and Build Hub builds. You run Claude Code, Codex, connect from VS Code, Cursor, JetBrains, or SSH, VNC.


**How is this different from a generic cloud dev box?**


Generic dev boxes give you a machine that is similar to your build environment that you maintain. RDE gives you the same one with no effort, if you use[Bitrise CI](https://bitrise.io/platform/bitrise-ci?utm_medium=other&utm_source=blog&utm_campaign=rde_announcement_rde-beta-launch_all_2026-06-30&utm_content=blog-ci-link) or[Build Hub](https://bitrise.io/platform/build-hub?utm_medium=other&utm_source=blog&utm_campaign=rde_announcement_rde-beta-launch_all_2026-06-30&utm_content=blog-bh-link) for Github Actions. That is what removes the "works on my machine" class of bug instead of just reducing it.


**Can AI agents use it?**


Yes. You can pre-install and configure Claude Code or Codex with your own API keys, and you can orchestrate parallel agents through the open-source MCP server with OAuth. You bring your AI, we provide the compute power, environment and customization.


**Does it run real macOS for iOS work?**


Yes. Real macOS on Apple Silicon with the full Xcode toolchain, Simulator, and the Apple tools your projects already require.


**What does the beta cost and commit me to?**


It is a free self-serve beta you can try yourself. Spin environments up, archive them when done, and restore later.
