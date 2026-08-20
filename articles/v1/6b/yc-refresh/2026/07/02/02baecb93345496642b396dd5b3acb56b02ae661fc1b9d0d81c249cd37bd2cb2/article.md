---
schema_version: "1.0.0"
document_id: "02baecb93345496642b396dd5b3acb56b02ae661fc1b9d0d81c249cd37bd2cb2"
company_key: "yc-refresh"
company: "Refresh"
source_id: "yc-refresh-news-import-082559068c7a"
canonical_url: "https://refresh.dev/blog/trajectories-and-computer-1"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-08-01T00:34:43.117149+00:00"
fetched_at: "2026-08-01T00:34:44.545265+00:00"
content_hash: "sha256:6d733f1536b8eb2c7d3a8eeab2ca56521a60f2454a042cef1aaf17dbd287d72d"
---

# Introducing trajectories.sh and computer-1

Today, we're announcing two new tools for the computer-use agent ecosystem:


[computer-1](https://github.com/harbor-framework/harbor/tree/main/src/harbor/agents/computer_1) , an open-source, model-agnostic computer-use harness built inside Harbor


[trajectories.sh](https://www.trajectories.sh/) , a platform anyone can use to upload, inspect, and share agent trajectories


Together, they make it easier to evaluate computer-use agents across models, inspect their behavior, and share results with others.


## Building on the Harbor ecosystem


[Harbor](https://www.harborframework.com/) has become an important part of the open agent-evaluation ecosystem. It gives benchmark authors a shared way to package environments, define tasks, run agents, apply verifiers, and generate trajectories. Benchmarks such as[Terminal-Bench](https://www.tbench.ai/) , which[now runs on Harbor](https://www.harborframework.com/docs/migration) , have shown how much agents can accomplish through a terminal.


But many workflows still require graphical interaction. Agents may need to navigate websites, operate desktop applications, interpret visual layouts, or use software that does not expose a practical API or command-line interface.


Terminal interaction and graphical computer use are complementary. We built computer-1 to bring graphical computer-use evaluation into Harbor.


## Meet computer-1


computer-1 is an open-source computer-use harness built directly inside Harbor. It lets benchmark authors define a task once and evaluate it across multiple model providers without rebuilding the agent loop for each one. computer-1 currently supports:


Anthropic and Amazon Bedrock


Google Gemini


OpenAI


Other vision-language models through LiteLLM


Each provider uses different tool schemas, action formats, safety protocols, and execution loops. computer-1 places those differences behind a common Harbor interface. It standardizes:


One action schema


: Every vendor tool call parses into a single ComputerAction: click, type, key, scroll, drag, hold_key, one-shot zoom_region crops, and bash commands executed in the environment, plus the terminal set that ends an episode.


One coordinate space


: Each action records the space it arrived in — native desktop pixels, Gemini's 0–999 grid, or the model space Anthropic downscales to a 1568px long edge — so rescaling to real pixels is driven by that field rather than by matching on vendor names.


One turn loop


: Three provider styles sit behind one dispatch: strict-JSON chat completions over LiteLLM, native step providers that own their conversation state and emit one normalized turn at a time (Anthropic/Bedrock, Gemini), and self-driving providers that run the episode themselves (the OpenAI Responses API).


The same implementation can run in local Docker environments or scale to infrastructure such as[Daytona](https://www.daytona.io/) and[Modal](https://modal.com/) . This lets benchmark authors spend more time on tasks, environments, and verifiers instead of maintaining provider-specific orchestration.


## Example: navigating the Cambridge BEUDO dataset


In one evaluation, computer-1 was asked to find Cambridge's highest-energy covered property in the Building Energy Use Disclosure Ordinance dataset. The agent navigated an interactive map, inspected multiple records, compared energy-use data, and extracted the correct property.


AGENT RUN


Claude Opus 4.8 · 116 steps · verifier passed.[View the trajectory →](https://www.trajectories.sh/t/3e994c42-81db-45ee-a41f-f9a1c8373052?trial=cambridge-beudo-most-energy-2024__WihCYYd&step=77&phase=result)


The verifier shows whether the task passed. The trajectory shows how the agent completed it. That is where trajectories.sh comes in.


## Meet trajectories.sh


[trajectories.sh](https://www.trajectories.sh/) turns agent runs into interactive, multimodal timelines. Each model response and tool call is paired with the corresponding screen state. Instead of reading flat JSON logs or opening folders of screenshots, you can move through a run step by step.


A trajectory can show:


What the agent saw


Which action it selected


How the interface responded


What the model concluded


Where the run diverged


How the verifier scored the result


Computer-use behavior depends heavily on visual context. Page layouts, open windows, loading states, cursor position, and application state can all affect the outcome of a run. A text log alone often leaves that context out.


Harbor's own[Hub](https://hub.harborframework.com/) already hosts jobs, datasets, and tasks and summarizes each one across cost, tokens, and performance — the right view for the terminal tasks Harbor was built for first, where a run reads as text. trajectories.sh is the complement for computer use, where the artifact under review is a sequence of frames and the coordinate-level actions taken against them.


## Trajectories should be easy to share


A computer-use trace may include screenshots, prompts, model responses, tool calls, errors, metadata, and evaluation results. These runs are often stored in local folders, internal dashboards, or recordings that are difficult to share and inspect.


With trajectories.sh, a trajectory becomes a link. Researchers can publish representative benchmark runs. Engineers can share the trace behind a regression. Model developers can compare how different agents approach the same task.


Public and unlisted trajectory links with recorded trials are also designed to generate visual previews when shared on platforms such as X, Slack, Discord, or LinkedIn.


A trajectory link shared to iMessage.


The preview shows the task, model, and step count before you tap. Opening the link takes you straight into the run.


---


The preview is a playable GIF


Trajectories with recorded trials render as a playable GIF of the run itself — recipients see the agent moving through the task before they even open the link. Works anywhere links preview: iMessage, Slack, Discord, X, LinkedIn.


This gives people useful context before they open the run. A benchmark score tells you whether an agent succeeded. A trajectory helps explain why.


## Case study: NASA satellites


Consider a 131-step evaluation in which an agent was asked to navigate an interactive solar-system map and identify spacecraft that were active in July 2009 and farther from Earth than the Moon. The agent returned 14 spacecraft. The correct answer was 17.


AGENT RUN


Claude Opus 4.6 on NASA Eyes · 14 of 17 identified · verifier failed. Misclassifies LCROSS, omits SOHO and ACE.[View the full run here](https://www.trajectories.sh/t/43773570-970c-4ff6-9dd9-c80a2091b30e?trial=conduit__0381d63&step=41&phase=result)


Reviewing the run revealed two different problems.


First, the agent found LCROSS and confirmed that it was active during the relevant period. It then excluded it after reasoning:


“in the Earth-Moon system, so exclude.”


LCROSS should have been included. Second, the agent omitted SOHO and ACE because it never searched for them.


The final answer showed an undercount. The trajectory showed that one spacecraft was misclassified, while two others were never considered. Those failures call for different fixes. One concerns spatial reasoning; the other concerns search coverage.


## Built for the community


computer-1 is open source, and lives as a self-contained module inside the[Harbor repository](https://github.com/harbor-framework/harbor) . Researchers and developers can inspect the implementation, add providers, build benchmarks, create new actions, and contribute improvements.


[View computer-1 on GitHub →](https://github.com/harbor-framework/harbor/tree/main/src/harbor/agents/computer_1)


trajectories.sh is a hosted platform that anyone can use. It works with computer-1, but it is not limited to it. Uploads use Harbor's ATIF job format, so trajectories from other harnesses can be shared too once exported to it.


[Explore trajectories.sh →](https://www.trajectories.sh/)


We hope these tools make it easier to:


Build reproducible computer-use evaluations


Compare agents across providers


Debug long-horizon failures


Share representative runs


Publish trajectories alongside benchmark results


Learn from how other agents succeed and fail


## Get started


Install computer-1 through Harbor:


```text
uv tool install 'harbor[computer-1]'
```


Then publish a finished run:


```text
npx trajectories-sh upload trajectory ./my-run
```
