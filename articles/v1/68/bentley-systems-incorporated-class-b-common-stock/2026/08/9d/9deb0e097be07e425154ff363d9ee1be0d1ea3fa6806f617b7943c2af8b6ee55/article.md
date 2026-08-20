---
schema_version: "1.0.0"
document_id: "9deb0e097be07e425154ff363d9ee1be0d1ea3fa6806f617b7943c2af8b6ee55"
company_key: "bentley-systems-incorporated-class-b-common-stock"
company: "Bentley Systems Incorporated"
source_id: "bentley-systems-incorporated-class-b-common-stock-news-import-dc7c98e657e9"
canonical_url: "https://blog.bentley.com/insights/no-code-no-problem-engineers-software/"
published_at: "2026-08-14T16:05:53+00:00"
first_seen_at: "2026-08-19T19:42:18.523348+00:00"
fetched_at: "2026-08-19T19:42:21.196640+00:00"
content_hash: "sha256:c4f8e581934b04eee6c2c3136d639d7881bcbc9f2344596501c9c1f6623f5317"
---

# No Code, No Problem: Engineers Are Learning to Talk to Their Software

When visitors come to Quebec City, Louis-Martin Losier likes to show them the Quebec Bridge. Losier works on design software at Bentley Systems, the global tech giant developing solutions for infrastructure engineers, and the bridge is a marvel of his profession. With the longest cantilever span on Earth, the bridge spans the mighty St. Lawrence River just west of the city’s picturesque old town. When it opened more than a century ago, it was hailed as the eighth wonder of the world. “This huge bridge was a great engineering achievement,” Losier says.


This year, working from the comfort of[Bentley’s new office here in Quebec City](https://blog.bentley.com/insights/bentley-systems-picks-quebec-city-for-its-first-tech-hub-to-bring-ai-to-infrastructure/) , Losier showed how any engineer could[recreate a design as intricate as the Quebec Bridge with the help of AI](https://blog.bentley.com/insights/quebec-bridge-rises-ai-recreates-engineering-wonder/) . He connected an AI agent to Bentley’s MicroStation engineering software and instructed it, in plain English, to draw the geometry of the bridge. Next, he used Bentley’s structural analysis solution called STAAD to check the result. The structure originally took 17 years to build, but Losier faithfully reconstructed it in days, reviewing the design step by step.


## Flattening the learning curve


The technology behind the experiment is the Model Context Protocol, or MCP, an open standard that lets an AI assistant operate other software. Anthropic created MCP, the Linux Foundation now governs it, and any AI assistant can adopt it. Instead of offering advice from a chat window, the AI works inside the engineering software, flattening the steep learning curve that has kept many engineers from using their software to its full potential.


Bentley has already developed MCP servers for several of its software solutions, and more are in the works. In early August, the company held the largest webinar in Bentley’s history to explain the technology to the engineers who will use it. The event, called “What MCP Means for You: Simpler, Faster Workflows in Bentley Applications and Beyond,” drew more than 2,000 registrations. ([Watch the MCP webinar.](https://eventhub.bentley.com/wcc/eh/2541253/lp/5429416/what-mcp-means-for-you-simpler-faster-workflows-in-bentley-applications-and-beyond) )


It was Bentley’s second session on the subject. The first, which drew more than 1,000 engineers, showed how AI coding tools like Claude and Codex were[lowering the barrier to automation](https://blog.bentley.com/insights/from-code-to-command-how-ai-is-rewiring-the-way-engineers-design-infrastructure/) and explained how MCP would eventually make the coding step unnecessary. The second webinar answered the question that followed the first: What does AI-powered engineering without coding look like in an ordinary workday?


MicroStation MCP Server and AI is used to design a noise barrier wall. This is the future of engineering—where your intent is the only input required.


## Decoding AI's alphabet soup


The answer began with definitions. Host Casey Aldridge, Bentley’s senior director of customer and community marketing, billed the hour as “MCP 101.” Excitement around Bentley’s new MCP servers has quickly spread, she said, but many users are still sifting through the “AI alphabet soup” of new terminology.


The back-to-basics approach reflects what Bentley hears from its software users: They want AI to work inside the tools, standards, and processes they already rely on. But before they let it work there, they must be able to check its results and trust it. That’s because in infrastructure, a design that is 90% right is wrong and leaves the engineer with 100% of the liability.[Bentley CTO Julien Moutte recently asked](https://blog.bentley.com/insights/would-you-cross-that-bridge-ai-and-the-trust-problem-in-infrastructure/) whether anyone would drive across a bridge that is “hopefully” designed right. (We think you know the answer.)


MCP’s answer is to connect probabilistic, creative AI to deterministic, trusted software that does the math. The AI proposes ideas, the software calculates, and the engineer, who remains in the loop, checks and approves the results. For Bentley, “it’s not simply about adding AI for the sake of it,” Aldridge said. It’s about connecting AI to the trusted analysis and simulation tools engineers already use, “while keeping engineering judgment firmly at the center.”


## Like a universal power adapter


Presenter Annik Carson, who works as a senior applied AI scientist at Bentley, broke the AI vocabulary into three pieces. AI, in this conversation, means out-of-the-box large language models like Claude and ChatGPT. They are trained on enormous amounts of data and can discuss almost anything, but they cannot see or understand an engineer’s project. They know only, Carson said, what you tell them in the chat interface window. On the software side, an API—short for application programming interface—is a menu of commands to operate the software programmatically, without a person clicking through dialogue boxes. MCP is the bridge between the two, connecting the AI we all use and the APIs that software uses. It allows engineers to prompt AI in plain language, and the AI to communicate with the software and drive it.


“Think of MCP like a universal power adapter,” Carson said. The AI is the gadget you bring on a trip, and each software product is a foreign wall socket. MCP makes any plug fit. Claude, Gemini, GitHub Copilot, and Bentley’s own Copilot all work with an MCP-enabled product the same way.


Connected this way, the AI becomes, in Carson’s words, a coworker, “creating, updating, orchestrating workflows across these different systems.” In practice, that means engineers work from a single interface while the AI assistant gathers what it needs from multiple products.


## An AI design informed by industry standards


Next up was Eduardo Mendonça, a senior application engineer at Bentley. He showed what that process looks like in MicroStation. His example came from a real request: A contractor wanted an early view of the design, along with the cost of noise barriers on a road, before a specialist firm took over the project. The goal, Mendonça stressed, was not a finished design but a quick first draft good enough to price and discuss the project.


The AI workflow was built by Mendonça’s colleague Stuart Milne, who earlier this year used a similar process to[recreate London’s iconic Gherkin tower](https://blog.bentley.com/insights/ai-recreated-the-gherkin-london-tower/) . Milne gave Claude two MCP connections: one to MicroStation and one to NotebookLM, a Google research tool preloaded with road noise barrier standards. Prompted in plain English, Claude studied the standards, analyzed the roads and buildings already in the MicroStation model, and worked out where barriers were needed before drawing anything. “The assistant is not simply generating text or suggestions,” Mendonça said. “Through MCP, it can use capabilities exposed by the MicroStation API to interact directly with the model.”


The barriers appeared, almost instantly, along with Claude’s accounting of its work. Claude described how many barriers it created, which design rules it applied, and where height requirements were met and where they fell short. It produced cross-sections, plan views, and a recommendation for transparent acrylic paneling based on local standards, leaving the engineer to validate or reject the proposal.


## The whole project, in one conversation


Karim Rashad, who leads Bentley’s structural solution engineering team, showed the same idea in STAAD.Pro. An API called OpenSTAAD has existed for years, but unlocking it used to require programming. Now the AI assistant does the programming.


Rashad asked Claude to review a finished analysis of a train station roof and summarize its material properties, utilization, stress, and displacement. The report came back in about two minutes. Then he raised the stakes, asking the assistant to combine five successive STAAD models spanning the roof’s design history. The result was a 13-page illustrated report tracing how engineers, aided by AI, had refined the structure. The original design used 232.5 tons of steel; the final version was nearly 40% lighter. The report documented the decisions behind each stage. “This is also great for managers who are trying to review different stages of a project,” Rashad said.


## “Anything you can imagine”


The demos set off a stream of follow-up questions, and the presenters spent the final 20 minutes answering them. The most common one: How can I try this myself? Bentley publishes its[finished MCP servers on GitHub](https://github.com/bentleysystems) , and it[maintains a page with updates](https://www.bentley.com/lp/bentley-mcp-servers/) and information on early access to servers still in development. Users should also check their software version: The STAAD MCP server, for example, requires STAAD.Pro 2025 or later. Setup, one presenter said, took about 20 minutes from download to first prompt.


On security, Carson said Bentley’s MCP servers log only basic metadata, such as call volume and latency, and track no user data, model content, tool calls, or any part of the conversation between user and agent. “Your data should remain your data,” Aldridge stressed. “You shouldn’t have to give up ownership or control of project information to take advantage of these AI-powered workflows.”


On model choice, any AI agent works with Bentley’s MCP servers, so no firm is locked into a single vendor. Users have noticed that different models produce different results, and some engineers already prefer one model for planning and another for execution.
Many questions tried to gauge what’s possible. Can MCP check code compliance? Draw 2D plan views? Work from a point cloud? The answer each time was yes, with a caveat the presenters repeated throughout the webinar: Results depend on the quality of the prompt and the structure of the input model. “Anything that you can imagine you could perform using the MicroStation API,” Mendonça said, “you can do it through MCP.”


## FAQ:


**


**


What is the Model Context Protocol in engineering software?


The Model Context Protocol (MCP) is an open standard that connects AI assistants directly to software application programming interfaces (APIs). Functioning like a universal adapter, MCP allows engineers to drive complex tools like MicroStation and STAAD using plain-language prompts instead of navigating dense software menus or writing custom code.


**


**


How does an MCP workflow maintain accuracy and safety in infrastructure design?


MCP pairs probabilistic AI models with deterministic engineering software that accurately handles structural calculations. This setup keeps the engineer firmly in the loop to validate each stage of the design, ensuring that creative AI proposals are rigorously tested by trusted analysis tools before approval.


**


**


How does Bentley protect project data and privacy when using MCP servers?


Bentley’s MCP servers only record basic operational metadata, such as latency and call volume. The system does not track, capture, or store model content, proprietary project files, tool calls, or the conversational prompts exchanged between the engineer and the AI agent.
