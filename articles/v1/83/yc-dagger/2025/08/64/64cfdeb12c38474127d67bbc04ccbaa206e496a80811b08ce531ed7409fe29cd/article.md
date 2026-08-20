---
schema_version: "1.0.0"
document_id: "64cfdeb12c38474127d67bbc04ccbaa206e496a80811b08ce531ed7409fe29cd"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/evolution-of-ci/"
published_at: "2025-08-01T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:9451102bb32b1bf63daf1059dd8c15ef66d4f0699d03a27c05f790e3bfc2a13c"
---

# The Evolution of CI in the Age of AI

Imagine the project you work on became the most popular on Github overnight.You have thousands of eager contributors. The number of maintainers is still the same size, but you now have 1,000 PRs a day to review and merge in.


On one hand, you want the contributions, but on the other hand, you’re drowning in them. You need help. You have to adapt.


This is what introducing coding agents looks like. *gasp*


Before we dive into all the ways AI is going to make your life harder, let’s zoom out a bit. The problems we need to solve are not new problems, they’re only exacerbated by coding agents. Lets have a look at the problems from two different perspectives: the contributor and the reviewer.


### The Contributor


For a lot of teams today, the contribution flow looks roughly like this:


1. Dev receives task
2. Dev writes code
3. Dev opens pull/merge request
4. CI happens
5. Maintainer reviews
6. Dev merges


Each step along the way is a feedback gate for the dev. The feedback stages are basically mini bosses in the world’s most mundane video game. If they get knocked down by the CI boss’s lint attack, they go back to writing code and try again. Eventually, through trial and error, they fight their way through each boss and beat the game by merging the code.


A critical part of this process is that the dev learned from the feedback along the way and it will carry forward to their next contribution. They’ll always remember that one cool tip the reviewer gave them. Maybe they’ll even start running linters locally before pushing! This is great for the dev, but it doesn’t scale. How do you take the new knowledge this dev gained and give it to ALL contributors?


### The Reviewer


Know the classic stereotype of the tired OSS maintainer burned out on reviewing half-assed contributions? That’s all of us, we just don’t know it yet.


When a project starts maybe you’re just hacking on it solo, or maybe there’s a few of you. As you build the project, you see it all assembled piece by piece. A cryptic bug pops up, and you know exactly where it probably came from. Then the team grows, and the new contributors aren’t yet experts on the entire codebase, but they know they can go ask Fabian about the database or Portia about the authorization middleware. The ratio of contributors to “qualified reviewers” starts to get out of balance. Then the team grows more, as well as the complexity of the codebase; Fabian and Portia have moved on and there’s no longer “that one expert” to go ask for help. The ability to contribute to the project has slowed to a fraction of what it once was.


Maybe that story sounds familiar. Maybe you can point out where in that timeline you’re currently at. This process is only accelerated by the introduction of AI contributors. As the ratio between contributors and “qualified reviewers” grows, those reviewers quickly become the bottleneck to shipping code.


The reviewer is the final boss to merging code. They’re responsible for passing their expertise to the contributor, updating their own context on the changed codebase, and finally making sure the contribution actually solves the problem it set out to. For a lot of teams the reviewer is also making sure the coding style adheres to the team’s preferences, the code is not malicious or introducing security issues, docs have been added/updated, tests have been added/updated, and the list goes on. This does not scale today or in a world with AI contributors.


### It’s hopeless!


So we’re all doomed right? Well, maybe not, or else I probably wouldn’t have written all of this. These problems have existed since the dawn of engineering so of course there are solutions! Many of the best software engineering teams already follow these processes, but the rest of us don’t because we just don’t feel the pain yet.


### Sharing the expertise


Developer documentation. Pretty obvious, right? In fact, the first thing you usually do when you’re introducing an AI contributor to your codebase is write a bunch of rules files and markdown to tell it how to solve problems. These are literally developer docs. Instead of spending all of your time writing rules files, you should put effort into building really great developer documentation that benefits **all contributors** .


Great developer documentation is how we scale the lessons learned from the contribution mini bosses to all contributors. Did a contributor ask a question about how the authorization middleware is architected? Put it in the docs. Did you give a contributor feedback on how you prefer react components to be structured? Put it in the docs. Better yet, if an AI contributor receives feedback, it should update the docs itself!


With great developer documentation you’re no longer relying on a couple of expert humans to hold all of the knowledge of the codebase. Bootstrapping this in your project may sound daunting, so consider using a coding agent to help you get started! Generally the first thing they do when solving a problem is analyze the codebase to understand the architecture. It may not be anywhere near complete, but it’s a start!


### The bottleneck


So now your contributors have the Nintendo Power walkthrough and cheat codes to contribute to the project. As a reviewer, your job is now slightly easier, however the scaling problem has not been solved. We need to improve the automated tooling earlier in the feedback process for the contributor so that by the time the contribution is ready for a reviewer it is in a much more final state. This means that the reviewer can focus on whether the contribution solves the problem and stop pretending to be a human linter.


Leveling up the feedback loops requires workflow changes. As a former Hashicorporeal, the tao “workflows, not technologies” is engrained in me. Simply put, we solve these problems by defining a workflow. Not a “Github Actions workflow” or anything like that, but more broadly an idea of how a problem gets solved. That workflow is implemented by tools, but remember “technologies change, end goals stay the same.” I’ll mention a few of these workflows and some tools that can help implement them, but each team faces different challenges and I’m curious to hear what other workflows people have implemented in this space!


### Level up your workflows


**Linters** . Most of us probably have a linter in our project already, but we don’t spend much time maintaining it. In fact, we probably spend more time working around it with exceptions than we do improving it. If you’ve ever had a contribution nit picked on the tiniest details of coding style or even writing style in your documentation, it means your reviewers are doing the job of a linter. Invest some time in your linter. Choose one that can validate everything that matters to your project. Look at what’s popular for your stack, such as[golangci-lint](https://github.com/golangci/golangci-lint) ,[ruff](https://github.com/astral-sh/ruff) , or[ESLint](https://github.com/eslint/eslint) . A lot of these tools offer a configuration-free option, but it’s worth building a configuration that represents your project’s style. Successful implementation of a linter moves the code style feedback to the contributor’s environment for faster feedback loops and less work for reviewers.


**Tests** . Think about what your tests are solving for you: are you checking a box that you *have* tests, or do they provide real confidence in the state of your codebase? As a contributor, do you feel sure that you haven’t accidentally broken anything? Unit tests are great for making sure individual functions behave as expected in different scenarios, but integration tests will give you confidence that your application actually works when all the pieces are put together.


**Fast feedback loops** . It’s important that these tools are available to the developer so they can have a local feedback loop. The work you put into these tools shouldn’t be locked in your CI environment. Building great developer tooling means developers can easily run these tools the same exact way they’re running in CI. This is what[Dagger](https://dagger.io/) is built for. You can build fast feedback loops accessible to human contributors, coding agents, and everything in between.


**AI reviewers** . These are relatively new tools that can use LLMs to provide code reviews. Tools such as[Greptile](https://www.greptile.com/) ,[CodeRabbit](https://www.coderabbit.ai/) , or[Github Copilot](https://github.com/features/copilot) can look for a whole bunch of things in a contribution and leave a review just like a maintainer would. Some of them can even integrate with the processes mentioned earlier to give highly contextual feedback. **AI reviewers do not replace the maintainer review** , but they are an effective tool at giving earlier automated feedback to a contributor to achieve a more polished state by the time it reaches a maintainer.


### Preparing for the future


So we’re not doomed, but we have a lot of work to do! Set off and build a culture of great developer documentation, linters that represent your code style, tests that build trust, portable developer tooling for fast feedback, and AI reviewers as the final mini boss before facing a maintainer! Whether you’re preparing to roll out an army of coding agents, concerned about an incoming wave of AI slop, or ready to finally provide a better experience for your humans, these processes will help you ship faster now and into the future.Want to discuss this topic with me more? Come say hi on our[Discord](https://discord.gg/dagger-io) and check out my latest Agentic CI demo[here](https://dagger.io/deep-dives/agentic-ci) .
