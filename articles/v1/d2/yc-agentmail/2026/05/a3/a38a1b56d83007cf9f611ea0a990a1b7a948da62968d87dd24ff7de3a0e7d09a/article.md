---
schema_version: "1.0.0"
document_id: "a38a1b56d83007cf9f611ea0a990a1b7a948da62968d87dd24ff7de3a0e7d09a"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/html-vs-markdown-for-ai-agents"
published_at: "2026-05-25T00:00:00+00:00"
first_seen_at: "2026-07-21T05:07:06.081239+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:b0de4317041509ac726306c0ee9ee68ba9c56dc537da1a7e7d6485fe3634d4d3"
---

# HTML vs Markdown for AI agents

The format an AI agent uses to deliver its output is more consequential than it sounds. It determines how much of that output a human actually consumes, how much the agent costs to generate it, how easily the result can be audited, and how well it survives downstream tooling. None of those properties are trivial in production.


This is the central question that became contested when an engineer on Anthropic's Claude Code team[published a piece](https://thariqs.github.io/html-effectiveness/) arguing HTML, not Markdown, should be the default format for AI agent output, and Andrej Karpathy[publicly backed](https://x.com/karpathy/status/2053872850101285137) the recommendation. The conversation that followed surfaced a structural question underneath the format choice: what's the right substrate for agent-to-human communication?


This piece is the place to learn the full picture: where the debate came from, what HTML and Markdown actually are, the real pros and cons, how to decide between them, and what the broader trajectory of agent output formats looks like.


**Contents**


- Background: HTML, Markdown, and how this debate started
- The problem this debate is reacting to
- Why Markdown won the default for AI agents
- The two camps
- A practical rubric
- The broader pattern: agent-native protocols
- Where this is heading
- FAQ


## Background: HTML, Markdown, and how this debate started


HTML and Markdown are both markup languages, but they were designed at different times for different purposes.


**HTML (HyperText Markup Language)** was created by Tim Berners-Lee at CERN in 1990 as the foundation of the web. It is expressive: tags for structure, CSS for styling, SVG for graphics, embedded JavaScript for interactivity, spatial layouts. It renders natively in every browser. It was designed for humans to read web documents in browsers.


**Markdown** was created by John Gruber in 2004 as a lightweight alternative for writing web content. The design goal was readability: a Markdown file should be publishable as-is in plain text without looking like it has been marked up. Simple syntax (` #` for headers,` **` for bold,` \[text\](url)` for links) converts cleanly to HTML. Markdown is the default format for documentation in most developer tools today.


Both formats predate AI agents entirely. Neither was designed with a machine producer in mind.


The current debate about which one AI agents should use as a default became public when an engineer on Anthropic's Claude Code team[published a piece](https://thariqs.github.io/html-effectiveness/) arguing HTML should replace Markdown as the standard output format for AI agent work. The post drew widespread attention. Andrej Karpathy[publicly backed](https://x.com/karpathy/status/2053872850101285137) the recommendation. The disagreement that followed (a[notable counter-piece](https://kurtis-redux.medium.com/the-unreasonable-ineffectiveness-of-html-5bd01ae1e879) argued HTML "chases visual gloss at the expense of source readability, security, ecosystem compatibility, and reviewability") sharpened the structural question agents are now forcing: what's the right substrate for agent-to-human communication given that agents produce more output than humans can read?


## The problem this debate is reacting to


LLMs produce more than humans can read. An[HBR study](https://hbr.org/2026/03/when-using-ai-leads-to-brain-fry) earlier this year found that workers under high AI oversight reported 19% more information overload, 14% more mental effort, and 33% more decision fatigue. The format an agent produces in is the lever that decides how much of that output a human actually consumes.


That format is now contested.


## Why Markdown won the default for AI agents


Markdown won the default for three practical reasons. The major AI coding tools ship with it as the standard output for plans, specs, and documentation. It uses about 68% fewer tokens than HTML. It performs well on machine-comprehension benchmarks where token efficiency matters: RAG pipelines and large-document tasks see meaningful accuracy improvements when ingesting Markdown over raw HTML, because the lower token overhead leaves more context budget for actual reasoning. (Research on raw structural understanding of tables is more mixed; HTML can outperform Markdown in some controlled benchmarks when context budget isn't the constraint.) Infrastructure providers (Cloudflare among them) have shipped Markdown-conversion features specifically for AI agent ingestion. And Markdown can be edited in any text editor with clean version-control diffs.


All those reasons assume the human is going to read and edit the file. That assumption is breaking down. Most readers don't finish a 100-line Markdown document. Markdown isn't using the visual bandwidth the brain has available.


## The two camps


The fight is over what to do about that gap. Two structurally different answers.


**HTML.** Roughly 30% of the human cortex is dedicated to visual processing. Markdown gives it almost nothing to work with: bold, italics, headers, bullets, tables. HTML gives it tables, CSS, SVG, embedded scripts, interactive components, spatial layouts.


The pros are real. Information density. Visual navigation. Shareability via URL. Two-way interaction (sliders, buttons, copy-as-JSON exports).


The use cases it unlocks are broad. Implementation plans with embedded mockups and live code snippets. Code review artifacts that render actual diffs with inline annotations and color-coded severity. Research reports with SVG flowcharts and tabbed navigation. Interactive prototypes where sliders tune parameters and a button exports the result back into the coding session. Custom one-off editors for triaging tickets, tuning prompts, picking values that are painful to express in plain text (colors, easing curves, cron schedules, regexes). The output stops being a document the human reads and becomes a small, transient interface the human can act on.


Production has followed the argument. Anthropic shipped Claude Artifacts (interactive HTML rendered on demand inside the Claude interface). OpenAI added HTML and React rendering to ChatGPT Canvas.[Salesforce's Agentforce](https://engineering.salesforce.com/how-agentforce-converts-llm-responses-into-structured-ui-for-ai-agents-across-4m-sessions/) processes over 4M agent sessions across 133K+ agents, converting LLM text to structured UI components using its Adaptive Response Formats. This isn't a fringe preference. It's where the platforms with the most volume have already landed for agent-to-human delivery.


The cons are also real. HTML costs 2-3x more tokens for clean content and 8-10x with CSS and JavaScript, and takes 2-4x longer to generate.[One developer's published cost model](https://x.com/aibuilderclub_/status/2054450951369437356) put HTML output at roughly $11K/year against $6.6K for Markdown at modest scale (a few hundred files, a few dozen sessions per day), with the explicit caveat that exact numbers depend on model pricing and implementation. Agent-generated JavaScript becomes runnable code in the reader's browser, which is a real XSS exposure: reading text becomes running code, with all the security implications that entails. HTML diffs are noisy. Editing requires tools.


The deeper concern in the Markdown camp is about source legibility. HTML's raw source is hostile to read, so if humans only consume rendered output, the ability to audit what the agent actually wrote degrades. If reviewability is what makes an artifact serious, anything that can only be reviewed after rendering is structurally weaker than something legible in its source.


**Markdown.** It's the lingua franca of developer collaboration platforms: GitHub, GitLab, Notion, Discord, and most chat surfaces with Markdown-style rich text. It's greppable, diffable, version-controllable. A plain-text Markdown file written today is more likely to survive format migrations and link rot than equivalent HTML with CSS and JavaScript dependencies, although the picture isn't perfect (CommonMark, MDX, and GitHub Flavored Markdown have created their own dialect fragmentation). The diagram limitation is overstated, too: modern Markdown integrates with Mermaid and PlantUML for declarative diagrams without the bloat of HTML/SVG source. And its source stays legible whether rendered or not.


The cons: the core visual toolkit is bold, italics, headers, bullets, and basic tables, and anything beyond that falls back to ASCII art or requires the diagram tooling above. No native interactivity. No native two-way exchange. And the 100-line problem the debate started with.


### At a glance


HTML Markdown


Token cost 2-3x more (8-10x with CSS/JS) 68% fewer tokens


Machine comprehension Stronger on structural understanding Better on RAG and large-context ingestion


Interactivity Native (sliders, buttons, exports) None natively


Security risk XSS via agent-generated JS Minimal


Source legibility Hostile to read raw Legible rendered or raw


Version control Noisy diffs Clean diffs


Ecosystem support Native browsers GitHub, Notion, Discord


Best for The session The archive


## A practical rubric


The most useful framings in the debate aren't picking sides. They're splitting the use case.


HTML for files meant to be opened, scanned once, shared, and discarded. Markdown for files meant to be filed, searched, edited, and cited later. HTML wins the session. Markdown wins the archive.


The frame holds up in practice. An autonomous coding agent producing a one-off code review artifact is different from one producing a spec that lives in version control for two years. The format question should follow the artifact's intended lifespan, not be defaulted across the board.


Extending the rubric to the full surface area an agent talks across:


- **Browser-rendered one-time artifact** (review, dashboard, prototype, demo): HTML.
- **Repository file or document edited and cited later** : Markdown.
- **Asynchronous message to a human** (email, Slack, chat): the channel's native rendering (HTML for email; Markdown-as-rich-text for chat).
- **Agent-to-agent payload** : structured JSON or a protocol like MCP, not a human format. Optimize for parseability and schema validation.
- **Surface with a structured renderer** (Adaptive Cards, A2UI, custom UI components): the structured protocol, not raw markup. Let the platform translate.


Defaulting one format across all five of these is what produces friction. The format that wins is the one the surface itself wants.


## The broader pattern: agent-native protocols


Both HTML and Markdown were built for humans. Markdown was created in 2004 to be human-readable in plain text. HTML was created in 1990 for humans to read web documents in browsers. Both predate LLM agents entirely. The whole debate is over which of two existing human formats agents should default to.


The agent-native protocols emerging in parallel point the same direction. A short tour of what's converging, at varying maturity levels:


- [MCP (Model Context Protocol)](https://www.anthropic.com/news/model-context-protocol) from Anthropic standardizes how agents talk to tools and data sources. Production-grade and widely adopted across the major model providers.
- [Adaptive Cards](https://adaptivecards.io/) from Microsoft is a platform-agnostic format for rich interactive content. Mature and shipping in Copilot Studio and elsewhere.
- [A2UI](https://developers.googleblog.com/introducing-a2ui-an-open-project-for-agent-driven-interfaces/) from Google standardizes how agents render UI surfaces through structured JSON. Currently in public preview.
- **A2A** (Agent-to-Agent communication) and **AG-UI** (real-time agent-to-user interaction) are emerging in the same space, less production-tested but converging on the same pattern.


What they share is the underlying shape. Agents emit structured payloads in agent-native wire formats. The platform translates those payloads into the interfaces humans already use: cards, text fields, buttons, dropdowns, chat surfaces, documents. The wire format is new. The surface humans see is not. None of these systems asks humans to learn anything new.


That's the signal in the format debate. The format war is what shows up in feeds. The deeper trajectory is that every layer of the agent-to-human and agent-to-agent stack is getting agent-native protocols underneath, and rendering to human-native surfaces on top. Format follows surface. Surface follows the human's existing workflow.


## Where this is heading


The HTML vs Markdown question is a useful reflection point on where agentic infrastructure is heading next. The game is becoming one of efficiency. Which path gets the right output in front of a human fastest, at the lowest cost, with the least friction between the agent and the surface the human already trusts.


The progression that's visible if you zoom out: raw text was the baseline. Markdown added enough structure for readability while staying lightweight. HTML added information density and interactivity at the cost of complexity and tokens. The next step is structured agent-native protocols rendering into existing human surfaces, with the agent never producing presentational markup directly. Each step in the progression trades efficiency for comprehension and capability. The trajectory is consistent.


The pattern across all of it: agents that win are the ones that fit into the surfaces humans already use. Not new interfaces for humans to learn. Not new formats for them to adopt. The browsers, documents, dashboards, calendars, chat surfaces, and design tools they already live in. This is the thesis behind[AgentMail](https://www.agentmail.to/) , and it's the broader pattern visible across every layer of the agent stack.


If you're building agents, the most useful thing you can do isn't pick a side in the format war. It's reduce the friction between your agent and the surfaces the human is already using. HTML when the surface is a browser and the goal is a rich, scannable artifact. Markdown when the surface is a repository and the goal is a file that survives the year. Structured protocols when something exists to render them. The win is making sure your agent shows up wherever the human already is.


The next wave of agentic infrastructure isn't a new format or a new interface. It's the unglamorous work of making agents fluent in everything that's already there.


For more on how this applies to autonomous coding agents specifically, see[What it takes for AI coding agents to be truly autonomous](https://www.agentmail.to/blog/autonomous-coding-agents) . For the broader case on identity as the substrate that lets agents operate inside existing interfaces, see[Email as Identity for AI Agents](https://www.agentmail.to/blog/email-as-identity-for-ai-agents) .


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
