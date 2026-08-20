---
schema_version: "1.0.0"
document_id: "d3d918760bf04969689cb0164ec98964495dc348f9cdb80304df27c7bfa06f9c"
company_key: "yc-brex"
company: "Brex"
source_id: "yc-brex-news-import-5eb786253ae8"
canonical_url: "https://www.brex.com/journal/terminal-ui-became-an-agent-interface"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-21T11:29:48.523536+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:88e13d19ce77a77b23b5432c0b74308586d36bc4bbbf9cc2fcf7e5fe76f444e3"
---

# When your infrastructure tool's best interface is a web page, you have an agent problem

## **The tab that never closes**


Every infrastructure engineer at Brex has the same browser tab: Terraform Cloud.


You write Terraform in your editor, push a branch, open a pull request, and then you leave the terminal. Open the browser, navigate to the workspace, wait for the plan to load, scroll through hundreds of lines of output, try to figure out which of the 47 resource changes actually matter, decide whether to approve, and then, back to the terminal. Dozens of times a day.


Terraform Cloud isn't bad. It solves real problems: centralized state, remote execution, and policy enforcement. But its interface was built for a world where humans are the only consumers of infrastructure data, and where clicking through a web UI is an acceptable cost of doing business.


That assumption is breaking down.


## **The agent can't click "Confirm & Apply"**


We started using LLMs at Brex to help reason about infrastructure changes: identify risks, explain errors, and suggest fixes. The first obstacle was the data, not the model.


Terraform Cloud has an API and an MCP server, but those tools return raw data: terminal output for plan logs, unprocessed JSON blobs, and JSONAPI metadata. No parsing, no structuring, no extraction of what actually changed or why something failed. This isn't unique to Terraform. It's a pattern across the ecosystem. MCP servers and APIs give you raw data. The intelligence that makes that data useful (parsing resource diffs, highlighting what changed, surfacing diagnostics) lives in the web UI or nowhere. The API gives you the raw ingredients. The webpage shows you the meal. Agents need the meal.


The model can reason about the data, but you're just giving it the wrong format. So we built Terraview.


## **Building for humans made us build for machines**


Terraview started as a terminal UI for reviewing Terraform plans. Engineers spend their day in the terminal using tools like Neovim, git, Claude Code, and the Terraform CLI; we wanted plan review to happen there too. Built with Go and the BubbleTea TUI library, it fetches data from the Terraform Cloud API and renders it as an interactive, keyboard-driven tree: workspaces, runs, resource diffs, attribute changes, diagnostics, state versions.


But here's what we didn't expect: the work required to make infrastructure data navigable for humans (parsing plan output into structured diffs, filtering noise, organizing by action type, and extracting diagnostics) turned out to be exactly what was needed to make it consumable for LLMs. The same parsing pipeline that powers the interactive tree can output structured JSON. The same state model that tracks what's on screen can produce snapshots that an LLM can read. The art of building a good TUI is the art of building a strong context for agents.


We leaned into that, and it shaped every design decision from then on.


## **Two audiences, one data pipeline**


The principle is simple: every feature has a human mode and a machine mode, and they share the same code.


**Structured JSON mode** skips the TUI entirely and outputs the same parsed, filtered data to stdout. Not megabytes of raw plan JSON with every attribute of every resource, just what matters: resource diffs, attribute changes, diagnostics with severity and file locations, drift detection, and apply status. This is what solves the context bloat problem. An LLM gets a focused, pre-parsed view that fits in a context window and can be queried with jq or reasoned about directly.


**Playbooks** drive the TUI headlessly via scripts with no terminal required. The real program runs, processing real messages and rendering real views, but a script controls the input. Deterministic, reproducible, fully automated. This is how we test the TUI in CI and how LLM agents can programmatically exercise it.


**Event journals** record every meaningful state transition as structured JSONL, with view snapshots capturing the screen at each moment. When we debug Terraview with an LLM, we hand it these artifacts. The model sees the exact sequence of events and can pinpoint where things went wrong.


**Agent integration** lets you go from reviewing a problem in the TUI to getting LLM help without context-switching. Select the relevant resources, press a key, and Terraview builds a curated prompt from the selection and pipes it to your agent. You go from "I see the problem" to "the agent sees the problem" in a single keystroke.


None of this required maintaining two products. BubbleTea's functional architecture means there's one state, one message loop, one render function. Swap the input source and the output sink, and the same program serves a different audience.


## **Building for the world we're heading into**


Terraview started because we were impatient. We believed there was a better way to review infrastructure changes and didn't want to wait for someone else to build it. But every challenge taught us something new. When raw logs bloated the context window, we learned that machine-readable output is a design requirement. When we couldn't debug TUI sessions with an LLM, we learned that observability needs to be designed for the tools doing the observing. Each problem pushed us to aim higher than where we started.


The result is a tool we take personal ownership of. One that reflects our bar for quality and our commitment to the engineers who depend on it. We're sharing what we learned because solving this once and keeping it to ourselves would be a waste.


Terraform Cloud is representative of an entire class of SaaS products: monitoring dashboards, CI/CD platforms, and cloud consoles. They all have the same shape: rich data trapped behind a human-only interface, with APIs that return raw data without the parsing, structuring, and queriability that make it useful. The gap between what SaaS interfaces offer and what AI-augmented workflows require is real, and it's growing. Closing it means building tools where structured, machine-readable output is a first-class concern, not an afterthought bolted onto a human interface.


Every SaaS tool your team depends on will eventually need this layer. We chose not to wait.
