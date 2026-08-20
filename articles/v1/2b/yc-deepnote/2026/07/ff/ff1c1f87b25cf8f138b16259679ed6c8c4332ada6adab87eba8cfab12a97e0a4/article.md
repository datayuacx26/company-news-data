---
schema_version: "1.0.0"
document_id: "ff1c1f87b25cf8f138b16259679ed6c8c4332ada6adab87eba8cfab12a97e0a4"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-1c599340c4cd"
canonical_url: "https://deepnote.com/blog/why-we-built-a-new-notebook"
published_at: null
first_seen_at: "2026-07-21T16:01:05.618995+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:9e730ee68ff4ce867042aa280dcaf4ca2cb2b92c6676c5b060dc990a3f27d72c"
---

# Deep dive: why we built a new notebook format

[← Back to all posts](https://deepnote.com/blog)


Yesterday, we open-sourced Deepnote, and the response was overwhelming: trending on Hacker News, a flood of stars on[GitHub](https://github.com/deepnote/deepnote/) , and thousands of new contributors joining our community on[LinkedIn](https://www.linkedin.com/company/deepnote/) . We got strong feedback from the community to share more about the rationale for us open-sourcing Deepnote after 7 years of building it, so we're publishing a technical deep dive on what led us to this decision, why we believe that notebooks need a new standard, and how we went about building the new standard.


## Prior art


In 2011, IPython Notebook (later known as **Jupyter** ) changed everything. It pioneered the idea of an interactive computational notebook1,2.


Building a format that survives a decade of innovation is extremely hard. Straight up impressive.


In 2014, the Jupyter team nailed it. Today, the[4th major revision](https://ipython.org/ipython-doc/3/api/generated/IPython.nbformat.v4.html) of the .ipynb format powers millions of workflows.


However...


- It was created pre-cloud


- It was created pre-collaboration


- It was created pre-AI


And it shows. What we have is:


- **Difficult collaboration** : there’s no space for comments, reviews, or permissions.


- **JSON makes even minor edits look big** , leading to noisy diffs and painful human reviews.


- **No project structure** : each notebook is isolated, even when they’re part of the same analysis


- **No context** : outputs are embedded in JSON, producing noisy diffs and making code review painful


- **No extensibility** : when Jupyter launched, there were only two cell types: markdown and code. A more helpful approach is having rich text blocks, interactive inputs, and SQL blocks.


- No good way to **securely store secrets** .


When it comes to formats, you either die a hero or live long enough to see yourself become a villain.


What began as a document for individuals to analyze data and write supporting narratives is now expected to serve **AI agents, distributed teams, and production pipelines** . We've heard consistent feedback from the community of hundreds of thousands of people that we need a format ready for that future.


## Projects, not files


In Jupyter, the notebook is the smallest unit of work. In Deepnote, it’s the **project** .


A project can contain multiple notebooks, shared integrations, and environment settings.


Think of it like a spreadsheet with multiple tabs — one for data cleaning and EDA, one for feature engineering, one for the app that visualizes results (yes, we can turn notebooks into apps). These pieces only make sense together. This means that there is metadata on the project level (integrations, permissions), not just at the notebook level.


We could have used a` .zip` file to bundle multiple files, but we wanted the format to remain human-readable at all times, unlike a binary file.


To solve this, we had to go for our own format. That's why Deepnote’s` .deepnote` file ties them into a single, human-readable YAML document. Clean, version-control-friendly structure that anyone can review and edit.


## Introducing the deepnote format


Every time a new player comes along and[introduces a new format](https://xkcd.com/927/) , there's a hesitation ('Do *we really need it?* '). At first, we really tried to get around the limitations listed above, but we didn't succeed.


That's why we designed` .deepnote` with a few non-negotiables:


- **Human-readable YAML** , not machine-optimized JSON


- **Support for securely connecting** to data sources and other integrations


- **Multiple notebooks per project** , with shared dependencies and integrations


- **Extensible block types,** Deepnote already has 23 of them (SQL as first-class citizen, charts, inputs, KPIs, text)


- **Verifiable schema** , ensuring metadata integrity and forward compatibility


- **Language-agnostic** , supporting Python, SQL, TypeScript, and sometimes even in the same notebook.


Notebooks today serve a much broader audience — not just developers, but analysts, PMs, and now, AI agents.


By going open source under the **Apache 2.0 license** , we want to give the community an open standard to build upon.


We kept the backward compatibility, and you can convert any` .ipynb` to` .deepnote` instantly with:


```text
npx @deepnote/convert notebook.ipynb
```


…and open it locally in VS Code, Cursor, Windsurf, or JupyterLab.


### Why not` .ipynb` ?


We get this question often.


For the past 7 years, we’ve been building on top of the` .ipynb` format. But we were constantly running into limitations of the format. With every new idea and every new feature, we kept Frankensteining the metadata field. This wasn’t sustainable.


Our vision for notebooks has grown beyond the Jupyter notebooks as we know them today. The changes we are proposing are radical and not compatible with the original assumptions of in .ipynb.


We don't use JSON. There's more than just one notebook. We introduced block types that have no equivalent in Jupyter, a reactive execution model, and a project-based structure that simply doesn’t fit the` .ipynb` schema. Maintaining backward compatibility became increasingly complex.


Porting this into .ipynb wouldn't be pretty. And if we were the maintainers of .ipynb, I don't think we would be merging it either.


We tried—and are still adamant about maintaining full compatibility—but over time, the paths diverged.


Instead, we built a library to convert notebooks between these two formats, giving us the best of both worlds: an empty canvas ready for the next decade and backwards compatibility with the existing work.


### Why not` .py` ?


We also get this question often.


Python is the dominant language today and will continue to be for a while, but we see that notebooks are **no longer just about Python** . So why lock ourselves into just one ecosystem?


Increasingly, we see new workflows and new expectations:


- SQL notebooks for analysts


- Rich text (headings, paragraphs, bullet points, to-do lists) and chart-only notebooks with interactive elements for data storytelling


- TypeScript notebooks for AI agents


- TypeScript is now emerging as a meaningful language in the AI world.


` .py` can’t express interactivity, visualization, or multi-language workflows. For examples, demos, and hobby projects, this would be great. For production-grade, multi-language projects, this quickly breaks apart. Notebooks have evolved far beyond scripts, and their format should reflect that.


We need something ready for the next decade:


- ready for new block types


- ready for AI agents


- ready for backward compatibility (.ipynb is already on 4th major version)


Data exploration doesn’t need Python. And with` .py` , we would be stuck with Python scripts.


## Why do we need a new notebook?


### AI‑native and agentic notebooks


The next decade of notebooks won’t be single‑player documents. They’ll be **co‑authored with AI agents** that read, reason over, and safely modify your work. The format must:


- Provide **typed blocks with stable IDs** and a dependency graph so agents can target, rerun, and validate exactly what changed.


- Support **multi‑language projects** (Python, SQL, R)


- Expose **structured metadata** (inputs, outputs, lineage) that an assistant can consume, not scrape from JSON.


- Preserve **reviewability** (clean diffs, comments) so human owners can accept or revert agent edits.


### Cloud‑native by default


Modern notebooks need to run anywhere—from your favorite IDE to your preferred cloud. The format should:


- Capture **project‑level settings** (Python version, custom image, requirements, secrets, integrations) for reproducible runs across machines.


- Work **offline in your IDE** and **scale in the cloud** on larger compute with the same file, while respecting organization settings.


- Encode **permissions and governance hooks** (owners, viewers, execution roles) so collaboration isn’t bolted on later.


### More than a notebook: data apps, scheduled runs, API endpoints


Teams don’t stop at exploration. The same project should become **data apps** , **scheduled jobs** , and **API endpoints** without a rewrite. Practically, that means:


- **Apps** : UI‑centric notebooks with input and chart blocks that publish as shareable applications.


- **Jobs** : Parameterized runs scheduled with notifications and retries.


- **Endpoints** : Deterministic blocks promoted to HTTP handlers (GET/POST) for lightweight inferencing.


> Example (illustrative) .deepnote excerpt showing project‑level intent captured alongside notebooks:


```text
project:
notebooks:
- id: "nb-clean"
name: "01 - Cleaning"
- id: "nb-app"
name: "App"
integrations:
- id: "snowflake-prod"
type: "snowflake"
settings:
environment:
pythonVersion: "3.11"
customImage: "org/data:2025-10-01"
requirements:
- "pandas>=2.0.0"


schedules:
- name: "daily-refresh"
cron: "0 6 * * *"
notebookId: "nb-clean"


apps:
- name: "Sales Dashboard"
notebookId: "nb-app"


endpoints:
- name: "score"
notebookId: "nb-app"
blockId: "block-score"
```


## Looking ahead


The` .deepnote` format and its integration with popular IDEs are just the beginning. Very soon, a standalone notebook experience will come, powered by an AI notebook agent. In the longer term, we’re building towards a world where notebooks are not just for exploration, but also for **data apps, APIs, and autonomous workflows** — all powered by open, transparent formats.


We're building a truly universal computational medium for collaboration between humans and AI agents.


**Check out our**[GitHub repo](https://github.com/deepnote/deepnote/) **and try Deepnote Open Source today.**


### Footnotes


1. While we, of course, recognize Wolfram/ Mathematica as the inventors of the medium in the late eighties, and ideas going back all the way to literate programming, this post is focused more on the current dominant standard for notebooks — Jupyter.


2. See:[Literal programming](https://en.wikipedia.org/wiki/Literal_(computer_programming)) .


Jakub Jurovych


CEO @ Deepnote


Follow Jakub on[LinkedIn](https://www.linkedin.com/in/jakubjurovych/)
