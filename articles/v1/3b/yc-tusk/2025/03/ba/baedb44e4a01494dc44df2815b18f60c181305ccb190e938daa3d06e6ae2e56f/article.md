---
schema_version: "1.0.0"
document_id: "baedb44e4a01494dc44df2815b18f60c181305ccb190e938daa3d06e6ae2e56f"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/deeplearning-case-study"
published_at: "2025-03-25T09:00:01+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:c119e5b8728941978df1fdf5243f6bfbec4a1675be0d4ee74087cf1c1eeecbb8"
---

# How DeepLearning.AI Used Tusk To Prevent Bugs in PRs

57.1%


Tusk test suites incorporated into PRs


42.9%


PRs with edge cases prevented


4


Tests generated per PR on average


## Introduction


[DeepLearning.AI](https://deeplearning.ai/) is the premier education platform for starting and advancing a career in AI and machine learning. Founded by **Andrew Ng** , global leader in AI, DeepLearning.AI is making a world-class AI education accessible to people around the globe.


Being pioneers of the current AI wave, DeepLearning.AI’s engineering team is a forward-thinking organization that has embraced AI-native products for making their developer experience more efficient and fulfilling.


## Problem


"Tusk has been an integral part of our CI/CD pipeline since it gives our engineers a sense of security when pushing code. We’re a fast-moving team that cares deeply about quality, so it helps to get the best of both worlds."


**Joe Chen** – Head of Engineering at DeepLearning.AI


The DeepLearning.AI engineering team first reached out to Tusk when they were looking for an LLM-powered solution that could improve the testing portion of their SDLC.


As a course management product **serving millions of visitors every month** , platform reliability was a high priority for the team. Joe Chen, Head of Engineering at DeepLearning.AI, wanted a way to improve code coverage for their backend since they had legacy services that lacked unit tests.


Being a startup, the engineering team had to contend with tradeoffs between shipping velocity and code quality. They had already implemented code review tools, but realized that better code coverage would provide more **peace of mind** when merging PRs.


## Solution


DeepLearning.AI's engineering team worked with the Tusk team to integrate Tusk’s test generation agent into their CI/CD pipeline.


Onboarding took Joe **one minute of work** to authorize the GitHub app and sync their backend repo. The Tusk team then handled the heavy lifting of setting up the test container required for Tusk to self-run its generated tests.


Within a day, Tusk was live and the DeepLearning.AI engineering team started seeing the agent’s unit test generation output within their PRs.


Anytime an engineer raised a PR or pushed a commit to a PR’s branch, Tusk would generate backend unit tests for the code changes while taking into account their codebase and business context.


Because Tusk functioned as a non-blocking PR check, Joe was able to embed the tool into the engineering team’s workflow in a way that was complementary to their existing branch permissions.


## Results


"Edge cases that are not covered can cause the most critical issues. By using Tusk, our team gains an additional guardian to protect our product from the threats of edge cases."


**Amo Chen** – Senior Backend Engineer at DeepLearning.AI


As part of a white-glove implementation, the Tusk team offered to run the test generation agent retroactively on a subset of the backend team’s recent PRs to provide a sample of Tusk's output.


When reviewing this test generation sample, Amo Chen, Senior Backend Engineer at DeepLearning.AI, found that Tusk had surfaced a failing test for an edge case that would actually cause a bug.


Amo was able to push a hotfix for the bug before users started encountering and creating customer support tickets for the issue.


Once the Tusk PR check was enabled, Tusk would go on to save their backend engineers from causing bugs **2-3 times in a given month** . With Tusk covering their blind spots, Joe’s engineering team could continue to ship fast with greater peace of mind.


### In the first 2 months


- **57.1%** of Tusk’s test suites were incorporated by engineers into PRs
- **42.9%** of testable PRs had edge cases identified by Tusk, preventing potential regressions before deployment
- **4 verified tests** generated on average per PR


Going off of the success of their Tusk implementation, Joe and the DeepLearning.AI team have synced more repos to Tusk. The future looks even brighter as Tusk now helps to generate more Python-based unit tests as well as frontend unit tests for the team.


---


## Curious?


Tusk is an AI agent that generates unit and integration tests for PRs. If you’d like to use Tusk to increase code coverage while preventing regressions,[book a demo](https://cal.com/team/tusk/demo) with us.


If you’re a software engineer or product manager interested in specializing in Machine Learning or AI Engineering, create an account at[DeepLearning.AI](https://deeplearning.ai/) and enroll in a course today.


---


TRY TUSK NOW


## AI test generation with codebase context for quality-obsessed teams.


Cover your blind spots, catch verified critical bugs, merge PRs 25% faster with peace of mind.
