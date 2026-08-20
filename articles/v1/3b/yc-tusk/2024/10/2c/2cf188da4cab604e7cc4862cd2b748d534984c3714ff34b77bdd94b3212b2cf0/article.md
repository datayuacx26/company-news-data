---
schema_version: "1.0.0"
document_id: "2cf188da4cab604e7cc4862cd2b748d534984c3714ff34b77bdd94b3212b2cf0"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/introducing-llm-powered-browser-testing"
published_at: "2024-10-03T09:00:01+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:7dd5fe3107698abdc4aac8285aca496d25803c88e36cc792cb49993ac8cf9804"
---

# Introducing LLM-powered browser testing

# Tusk + Momentic


We've got some exciting news, folks.[Tusk](https://usetusk.ai/) is partnering with[Momentic](https://momentic.ai/) to bring AI testing superpowers to our AI coding agent.


We’re proud to announce the launch of **Automated Browser Testing** today. Tusk is now the **first AI coding agent capable of autonomously testing its changes** and iterating on a PR like a human engineer would.


## How It Works


After generating a pull request for an issue, Tusk will:


- Generate the steps required to verify that the code changes are correct
- Use Momentic’s AI testing platform to execute the browser test on your PR’s preview environment


Our AI agent emulates a frontend engineer by **testing its changes on a browser** , iterating based on the test result, and **leaving screenshots and videos** as proof of completion in the PR. In doing so, we make reviewing Tusk's PRs easier while improving the quality of the code that Tusk generates.


## More Reliability, More Throughput


We ran a beta of Automated Browser Testing with a small group of companies. We saw a **50% reduction in engineering time** spent reviewing Tusk’s PRs. Internally, it has allowed us to **2x the number of Tusk PRs merged** in a sprint.


To use Automated Browser Testing, record a video showing how to get to the location of the UI change and/or how to replicate a bug. You can also narrate over the video so that you don’t need to write a detailed issue description.


Tusk uses the video’s visual and audio context, along with the console logs, to help it reliably browse through your web app to test the change that was made. For more details, check out our[documentation](https://docs.usetusk.ai/browser-testing) .


## Partnership with Momentic


Automated Browser Testing builds on top of **Momentic’s SOTA web agent** . We can provide instructions in natural language and replicate user flows live in a browser on-demand.


We’re excited to partner with the Momentic team for this key milestone. Get the power of Tusk’s code generation with the **peace of mind** that every PR is validated in a real, live browser. Just like how you’d test your own changes.


---


## Try It Out


Interested in experiencing Tusk’s ticket to PR workflow with testing? Sign up at[https://app.usetusk.ai/](https://app.usetusk.ai/) and reach out tofounders@usetusk.ai for early access to this new feature.


To learn more about Momentic, reach out tosales@momentic.ai .
