---
schema_version: "1.0.0"
document_id: "103b4f3c94cf3525576ec235651a2e9df446a35033523e90d91f9001edf524ea"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-1c599340c4cd"
canonical_url: "https://deepnote.com/blog/jupyter-ai-guide"
published_at: null
first_seen_at: "2026-07-26T08:11:39.657869+00:00"
fetched_at: "2026-07-28T21:16:48.751829+00:00"
content_hash: "sha256:d007b69789e9eec6be463e76adf3ff7457e4d1919e14dd9381a7a46d1214daeb"
---

# Jupyter AI: How to use AI tools in your notebooks

[← Back to all posts](https://deepnote.com/blog)


This piece will help readers decide if Jupyter AI is the right assistant for their notebook workflow. It will start with what the project actually is, when it was introduced, and who is driving it. Then it will show how the chat UI and %%ai magic command work in practice, the experience, and where it stalls. The close will compare that flow with Deepnote and a real notebook example to judge which tool gets to a usable result faster.


Jupyter AI is a solid option if notebooks are where work starts and ends. This piece is a grounded look at how it works in practice, where it delivers, and where it still leaves gaps when the output needs to move beyond a notebook.


## What is Jupyter AI?


Jupyter AI is an open-source project under the JupyterLab organization that adds generative AI features to notebook workflows. In JupyterLab, it provides a native chat interface. Across notebook environments that support the IPython kernel, it also provides the` %%ai` magic for prompt-driven generation inside cells. The project is currently listed as under incubation and was publicly announced at JupyterCon on May 10, 2023, as part of a broader set of AWS-led Jupyter contributions.


> **v3 just rolled out:** Jupyter AI v3 adds a more *agent-oriented* workflow in JupyterLab, where you install at least one agent separately, create chats directly in the workspace, and collaborate with agents through the chat panel. See the latest getting started guide[here](https://jupyter-ai.readthedocs.io/en/v3/getting-started.html) .


## Jupyter AI features


Jupyter AI’s feature set is more substantial than just “chat plus magic commands.” At a high level, it gives notebook users two main ways to work with AI inside Jupyter:


-


A native chat panel inside JupyterLab


-


The` %%ai` magic for working directly inside notebook cells


The chat panel is useful when you want to ask for code, explanations, refactors, or debugging help without leaving the notebook workspace. The` %%ai` magic is useful when you want prompting to live directly alongside the code and outputs it produced. It's worth noting that the chat panel requires JupyterLab specifically, while` %%ai` works in *any* IPython kernel environment -- if you're still deciding between the two interfaces,[this comparison](https://deepnote.com/compare/jupyter-notebook-vs-jupyterlab) is worth reading before you commit to a setup.


In practice, this makes Jupyter AI feel less like a single built-in assistant and more like a JupyterLab chat framework for working with installable agents and notebook-aware tools.


**A chat UI that fits notebook workflows**


The chat interface is designed for longer, more contextual notebook work, not just one-off prompts.


A few details stand out:


-


You can create and name separate chats (you can reopen them later like other documents, use multiple chats at once, and keep separate threads of work without relying on one long conversation)


-


Chat history can be saved as` .chat` files


-


Files can be attached as context (you can drag and drop files or notebook cells into chat)


-


The interface supports[personas](https://jupyter-ai.readthedocs.io/en/v3/developers/entry_points_api/personas_group.html) , including the default Jupyternaut persona


That sounds small, but it matters. If one notebook contains multiple experiments or lines of inquiry, named chats help keep context from bleeding across them. The` .chat` files also make the interaction feel less disposable. Instead of a prompt box you use once and forget, the chat becomes part of the working environment.


**Cell-level prompting**


The other half of the experience is the` %%ai` magic. This lets you interact with models directly inside notebook cells, which is one of the more compelling parts of the product.


Instead of switching to a separate assistant, you can keep the prompt, the generated code, and the resulting output all in the same notebook flow. That makes the notebook feel more like a reproducible generative AI workspace, not just a place where model output gets pasted after the fact.


**Model flexibility**


Jupyter AI also supports a wide range of model providers, which makes it more flexible than a single-model assistant.


The[docs list](https://jupyter-ai.readthedocs.io/en/v3/) support across providers such as Anthropic, Cohere, Gemini, Hugging Face, Mistral, NVIDIA, OpenAI, Ollama, SageMaker, and Bedrock, among others. Its model layer is also starting to extend through integrations such as[LiteLLM](https://jupyter-ai.readthedocs.io/en/v3/users/jupyternaut/index.html#model-selection) , which is worth knowing if you want broader provider coverage without tying the workflow to a single model backend.


For teams that already have a preferred provider and need to stay inside a specific cloud stack, or want more control over what models they use, this flexibility matters.


Jupyter AI v3 also adds some *notebook-native* workflows that are easy to miss if you only think of it as a chat sidebar. It now highlights[code-toolbar shortcuts](https://jupyter-ai.readthedocs.io/en/v3/users/index.html#code-toolbar-shortcuts) for AI-generated code, including copying code, inserting it as a new notebook cell above or below the active cell, and replacing the active cell entirely. That makes the assistant feel more integrated into notebook editing, not just adjacent to it.


## How to use Jupyter AI


Jupyter AI does not ship with an agent by default. You need to install at least one agent first, and Jupyter AI will then detect the agents available in your environment. The official docs walk through the chat interface, how to create and name chats, and how to attach files as prompt context with` @file:` .


This is the default chat layout you see the moment you open Jupyter AI. It matters because the chat panel is where most of the interaction happens, so the UI you see here sets the pace. It anchors what a first‑time user should expect.


You can also switch between various models (and model providers) via the Jupyternaut settings:


This shows how separate chats are created and named to keep threads clean. That detail matters when one notebook supports multiple experiments or separate questions. It keeps context from bleeding across unrelated prompts.


This is the key feature for attaching context to a prompt. It is the difference between a generic answer and code that actually fits the notebook. For anything non‑trivial, this is what keeps results usable. You can also drag notebook cells directly into the chat input, use the paperclip picker, or attach files by path with` @file:` .


This is what an initial response looks like in practice. It shows the typical output shape: explanation plus code that can be run in the next cell. This is the baseline experience most users will get first.


You can also *follow‑up/iterate* based on your initial prompt to tailor it according to your needs.


## Where it actually works well


Jupyter AI is strongest when the notebook is the real workspace, not just a temporary scratchpad. Its biggest advantage is simple: **everything stays in one place** .


Instead of bouncing between a notebook and a separate AI tool, you can:


-


Ask for code


-


Run it immediately


-


Inspect the output


-


Fix errors in place


-


Ask follow-up questions with the notebook itself as context


For notebook-heavy work, proximity is useful in a very practical way. You do not have to copy code into another tab, re-paste it back into JupyterLab, rerun it, and then explain the error all over again. The assistant sits next to the code, the output, and the execution environment.


That makes Jupyter AI especially useful for tasks like:


-


Drafting small functions


-


Refactoring notebook code


-


Debugging errors


-


Explaining existing cells


-


Looking up syntax or library patterns while staying in flow


****


Jupyter AI works best when the problem is small enough to stay bounded and notebook-native.


That usually means things like:


-


A solo analyst exploring a dataset


-


A developer building an API call from documentation


-


A notebook user debugging code in place


-


A learner trying to understand what a block of Python is doing


This is also how the[DeepLearning.AI course](https://www.deeplearning.ai/short-courses/jupyter-ai-coding-in-notebooks/) frames it. The examples stay close to the notebook and close to execution. One project builds a small book research assistant from API docs. Another walks through stock data analysis and visualization. The pattern is consistent: local, iterative work that stays inside the notebook.


**In practice, Jupyter AI works best when...**


-


The notebook is the main workspace


-


The audience is technical or notebook-native


-


The goal is faster iteration, not publishing


-


The task is code generation, debugging, explanation, or light analysis


That is a narrower promise than “AI for all data work,” but it is also the one Jupyter AI delivers on most reliably. It strengthens *notebook-first* workflows by giving agents direct notebook-aware capabilities through the Jupyter MCP server. Agents can answer questions about the active notebook or active cell, create and edit notebooks, run code cells through the kernel, and open other files in JupyterLab. That makes the assistant more useful for real in-notebook work than a model that only replies with text.


## Jupyter AI use cases


Jupyter AI is most useful when the work already fits the shape of a notebook. It is not trying to replace the whole workflow around analysis. It is trying to help you move faster inside the environment you are already using.


The clearest use cases are the practical ones:


-


Generating starter code


-


Refactoring existing notebook code


-


Explaining logic in plain English


-


Debugging errors without leaving the notebook


-


Prototyping small analyses before formalizing them elsewhere


These are not giant platform-scale workflows. They are smaller, everyday tasks that come up constantly for developers, analysts, and learners.


What the course examples emphasize


The[DeepLearning.AI course](https://www.deeplearning.ai/short-courses/jupyter-ai-coding-in-notebooks/) leans into exactly those kinds of tasks. It shows Jupyter AI being used to:


-


Produce API request code from documentation


-


Build a small book research assistant


-


Analyze and visualize stock-market data


-


Generate, explain, and refine notebook code in place


That is a good clue about the product’s sweet spot. The examples stay close to the notebook, close to the code, and close to execution.


**Where context makes the difference**


Jupyter AI also gets much more useful when you give it something real to work from.


That can be:


-


An attached file


-


Notebook content


-


API documentation


-


A code snippet that needs to be explained or fixed


This is where it moves beyond generic code generation. If you want help calling an unfamiliar API, explaining a notebook-specific bug, or writing code that matches the structure of your existing notebook, context is what makes the output usable.


In other words, it works best when the goal is to help you move faster inside a notebook, not replace everything that happens before or after the notebook.


## Jupyter AI best practices


Jupyter AI works best when you treat it like a coding partner, not an autopilot. The quality of the output depends a lot on the quality of the context you give it, and the notebook itself can either help that process or quietly undermine it.


**Start with context, not just prompts**


The first rule is simple: give Jupyter AI enough context to produce runnable code instead of plausible-looking text.


That can mean attaching:


-


A file with` @file`


-


Relevant notebook cells


-


API documentation


-


A code snippet you want explained or fixed


The model performs much better when it has something concrete to work from, especially in cases like the Open Library API exercise, where the documentation is what makes the generated request code actually usable.


**Keep the notebook reproducible**


Jupyter AI can help you write code faster, but it does not protect you from bad notebook hygiene.


A few habits matter more when AI is involved:


-


Run cells top to bottom


-


Restart the kernel and run all before you trust the result


-


Avoid relying on hidden state from earlier manual runs


-


Check that variables, imports, and outputs still work in order


This is basic notebook discipline, but it becomes more important when the assistant is generating or editing code for you. A response can look correct and still fail once the notebook is run cleanly from start to finish.


Jupyter AI is strongest in notebooks that stay organized. If one notebook is doing 5 jobs at once, the assistant has more room to misunderstand what belongs where.


A better pattern is:


-


One notebook, one clear task


-


Markdown cells that explain the flow


-


Repeated logic moved into functions or external scripts


-


Cleanup once the exploratory phase is over


Jupyter AI is good at prototyping, debugging, and explanation. But the more complex the notebook gets, the more structure starts to matter.


AI-generated code should be treated as a draft, not a final answer. Review it before execution, especially when it touches:


-


Credentials


-


File paths


-


External APIs


-


Model provider configuration


-


Anything that writes, deletes, or overwrites data


The model providers may require credentials and that the chat interface sends data to third-party hosted models, so you need to understand both privacy and billing implications before using it casually.


**Use model flexibility carefully**


Jupyter AI supports multiple providers and lets you configure models through Jupyternaut settings, which is useful if your team already has a preferred model stack. For AWS users, the docs include a dedicated[Bedrock setup guide](https://jupyter-ai.readthedocs.io/en/v3/users/bedrock.html) that walks through enabling model access and authenticating through` boto3` .


That flexibility is useful, but it also means you should know which model you are using, what it costs, and what data is being sent where.


It is also worth remembering that agents are now a more explicit part of the setup model. Since Jupyter AI can work with multiple installable agents, plus notebook tools and even custom MCP servers, it is worth being deliberate about what capabilities you enable in a shared environment.


## Where Jupyter AI can feel limiting


Jupyter AI is helpful inside the notebook, but it does not solve what happens after the notebook.


If your audience is other notebook users, that is usually fine. If your audience is a product manager, an executive, or anyone who wants a clean interactive result without opening JupyterLab, you still need another layer. Teams that are already running JupyterLab across multiple users often find themselves evaluating JupyterHub for shared access --[that comparison is here](https://deepnote.com/compare/jupyterhub-vs-jupyterlab) if that's the direction you're headed.


**The notebook is still the boundary**


Jupyter AI can help you:


-


Write code


-


Explain code


-


Refactor code


-


Fix errors in place


What it does not do is turn a working notebook into a polished, shareable product on its own.


That becomes a real limitation when the output needs to be:


-


Easy to share with non-technical teammates


-


Clean enough for review


-


Interactive without exposing notebook internals


-


Ready for someone who does not care how the code works


**The cleanup still belongs to you**


Once an analysis works, the notebook usually still needs work.


Someone has to:


-


Remove scratch cells


-


Clean up test output


-


Organize the notebook into a readable flow


-


Decide what another person should and should not see


Jupyter AI can help with parts of that, but it does not automatically turn exploratory work into a finished deliverable.


****


The clearest limitation is not coding help. It is workflow support. Jupyter AI is strong when the problem is “help me work inside this notebook.”


It is much weaker when the problem becomes “help me turn this into something others can use.”


That does not make it a weak product. It just means its center of gravity is still the notebook, not the broader lifecycle around the notebook. The new v3 workflow also makes Jupyter AI feel more capable inside JupyterLab, but it does not change the basic boundary of the product. Agents can read files, write files, run shell commands, and interact with notebooks through the MCP server, but that still happens inside a notebook-centered environment. It improves notebook execution and iteration. It does not remove the need for a separate publishing or stakeholder-facing layer.


## Deepnote Agent


Deepnote Agent is designed to make notebook work faster, clearer, and easier to share. You can ask questions in plain language, and Agent can explain code, suggest changes, or make direct edits to the notebook. The key difference is that it separates **Ask** mode from **Edit** mode. In Ask mode, it explains or suggests without changing anything. In Edit mode, it makes notebook changes with a visible plan, live updates, and a before-and-after diff view. That makes iteration easier to review and reduces the risk of silent changes.


For teams with security or compliance requirements, Deepnote also supports[Bring Your Own Key (BYOK) and custom model configuration](https://deepnote.com/docs/ai-custom-models) , allowing you to connect your own provider credentials instead of routing everything through a shared managed key.


This is the Deepnote Agent entry point. It shows where the assistant lives inside the workflow.


You get to see the diff viewer of the changes made in various cell blocks by the AI agent:


This shows how session history is retained in the agent view. It matters because long‑running notebook work needs clear thread separation. It also matters for auditability and for picking up work later. It also supports collaboration when multiple people touch the same notebook.


The` @deepnote/convert` package provides a command-line tool and programmatic API for converting between Jupyter notebooks and Deepnote's open-source format. I ran the conversion step using:


```text
npm @deepnote/convert titanic-tutorial.ipynb
```


The official Deepnote extension for working with Deepnote notebooks locally in VS Code (and similar IDEs such as Cursor, Antigravity, etc.).


Imagine a churn analysis where the notebook needs to be shared live with a teammate, reviewed together, and then shown as a simple app to a stakeholder. In Deepnote, you can[collaborate](https://deepnote.com/docs/comments) in the same runtime, then publish the notebook as an app without leaving the workspace. The assistant can also help with the same prompt, but the key difference is the full flow from analysis to a shareable end result.


If the notebook itself is the final product, Jupyter AI can be enough. If the output needs to be a clean, shareable artifact, Deepnote fits better.


This is the published churn web app view readers can interact with. It makes the difference between a notebook artifact and something others can use without opening JupyterLab. It also sets a visual baseline for the rest of the comparison.


**Jupyter AI vs Deepnote Agent**


Both tools bring AI assistance into notebook workflows, but they're built on different assumptions about the scope of that assistance and those assumptions show up quickly once you move past code generation.


The other meaningful difference is in how edits are reviewed. Jupyter AI v3 agents can make direct changes to notebooks -- inserting cells, writing files, running commands, etc. with a permission system that requests approval before executing. Deepnote Agent separates Ask mode from Edit mode explicitly: Ask mode explains and suggests without changing anything, while Edit mode produces a visible plan with live updates and a before-and-after diff for each affected cell block. The approval step in Jupyter AI happens before the change; the diff view in Deepnote lets you inspect what actually changed afterward.


****


Agents can directly edit notebooks; permission approval required before changes execute


**Jupyter AI** **Deepnote Agent**


**Best for** Users working inside JupyterLab Teams that need collaboration, review, and output beyond the notebook


**Context scope** Active notebook + attached files Full project: notebooks, SQL, datasets, integrations


**Edit model** Ask mode and Edit mode with diff view


**Model flexibility** Many providers, self-hostable BYOK and custom model config available


**Open source** Yes No (open format; platform is managed)


## Where Jupyter AI fits


Jupyter AI makes the most sense when your work lives in JupyterLab and the notebook is both the workspace and the output. It is useful for generating code, debugging faster, explaining logic, and iterating without leaving the environment. v3 makes that workflow more capable by leaning further into installable agents, persistent chats, notebook actions, and notebook-aware tools. But once the job shifts from “help me work inside this notebook” to “help me turn this into something other people can use,” the limits show up quickly. If your workflow ends at the notebook, Jupyter AI is a strong fit. If it needs to end in collaboration, review, or a shareable artifact, you probably need something beyond the notebook itself.


## References


- [Jupyter AI documentation](https://jupyter-ai.readthedocs.io/en/v3/)


- [Jupyter AI user guide (chat interface images)](https://jupyter-ai.readthedocs.io/en/v3/users/index.html)


- [Jupyter AI GitHub repository](https://github.com/jupyterlab/jupyter-ai)


- [AWS announcement of Jupyter AI (May 10, 2023)](https://aws.amazon.com/blogs/machine-learning/announcing-new-jupyter-contributions-by-aws-to-democratize-generative-ai-and-scale-ml-workloads/)


- [Jupyter AI PyPI (incubation note)](https://pypi.org/project/jupyter-ai/3.0.0b9/)


- [DeepLearning.AI short course: Jupyter AI — AI Coding in Notebooks](https://learn.deeplearning.ai/courses/jupyter-ai-coding-in-notebooks/information)


- [Jupyter AI bedrock guide](https://jupyter-ai.readthedocs.io/en/v3/users/bedrock.html)


- [Deepnote documentation](https://deepnote.com/docs)


- [Deepnote explore churn notebook](https://deepnote.com/explore/customer-churn-ml-playbook)


Srihari Thyagarajan


Technical Writer


Follow Srihari on[Twitter](https://x.com/hari_leo03) ,


[LinkedIn](https://www.linkedin.com/in/srihari-thyagarajan/)


and[GitHub](https://github.com/Haleshot)
