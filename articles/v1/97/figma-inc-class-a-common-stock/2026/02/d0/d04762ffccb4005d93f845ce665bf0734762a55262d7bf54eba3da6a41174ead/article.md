---
schema_version: "1.0.0"
document_id: "d04762ffccb4005d93f845ce665bf0734762a55262d7bf54eba3da6a41174ead"
company_key: "figma-inc-class-a-common-stock"
company: "Figma Inc."
source_id: "figma-inc-class-a-common-stock-rss-45373d72e79e"
canonical_url: "https://www.figma.com/blog/introducing-codex-to-figma/"
published_at: "2026-02-26T05:00:00+00:00"
first_seen_at: "2026-07-25T04:33:00.636596+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:828d079380853ea6a90e1a2e76efff8b50917d1c8941a2be5642070e11b1e0cb"
---

# Building frontend UIs with Codex and Figma

###### Connect Figma to other MCP clients


Codex is one of our MCP partners. See all supported agentic tools in our[MCP catalog.](https://www.figma.com/mcp-catalog/?fuid=1590478787962654948)


Starting today, you can generate Figma Design files from Codex using the[Figma MCP server](https://www.figma.com/blog/introducing-figma-mcp-server/)


### [Introducing our MCP server: Bringing Figma into your workflow](https://www.figma.com/blog/introducing-figma-mcp-server/)


[Today we’re announcing the beta release of the Figma MCP server, which brings Figma directly into the developer workflow to help LLMs achieve design-informed code generation.](https://www.figma.com/blog/introducing-figma-mcp-server/)


. The MCP server is built to support movement in both directions, bringing working UI onto the canvas and back into code just as easily—so you can build on your best ideas, not just your first.


The Codex desktop application is purpose-built for agentic coding. It provides a focused interface for managing multiple agents in parallel across projects, tracking progress without losing context, and integrating external tools.


That kind of fluidity feels familiar. In Figma, teams move just as easily. The canvas is designed for exploration and iteration. It’s a space where the best ideas have room to take shape. By connecting the Figma canvas to Codex, that spirit of exploration extends directly into the development workflow—unlocking a powerful new way for users to build everything from prototypes to production applications.


## Starting an app from a design


One of the core use cases of the Figma MCP server is retrieving context from Figma files and using that context in code generation. The Figma MCP server can capture information from Figma Design, Figma Make, and FigJam files and pass it to Codex as part of the building process.


To get started, open the Figma file that you plan to build your application from. Choose a frame by right clicking and selecting “Copy as” and “Copy link to selection.”


These selection URLs are linked directly to a frame or node on the Figma canvas. They could be a single element or a collection of components, but essentially it’s the source data that an agent will use for code generation. Selections can come from Figma Design, Figma Make, or FigJam files. Once you have the URL, open Codex and select either a new project or existing one. From here you can instruct Codex with a prompt like:` Help me implement this Figma design in code, use my existing design system components as much as possible.`


Beyond extracting design information, there are a number of other[helpful tools](https://developers.figma.com/docs/figma-mcp-server/tools-and-prompts/) available in the Figma MCP server.


Prompts like this will instruct the agent to call the` get_design_context` tool from the Figma MCP server. This tool helps extract critical design information from Figma files like layouts, styles, and component information and then provides that context to Codex for code generation.


## From code to canvas


After iterating in code, you can bring your design back into the canvas to compare flows, explore alternatives, and validate your assumptions. The Figma MCP server makes it easy to bring those screens back into Figma without having to recreate them manually from the browser. With the` generate_figma_design` tool, you can turn a live, running interface into fully editable Figma frames in seconds—bringing real, functioning UI directly onto the canvas for deeper explorations and collaboration.


First you’ll need to render the UI of your application. This can be done either locally or via a publicly available web server. From there, ask Codex to generate a new Figma Design file.


Codex will then walk you through the following steps:


1. Decide to either create a new Figma file or use an existing one.
2. Determine which workspace to place the file in.
3. Setup the application for UI capture.
4. Open a new browser session of your application.


When the application reloads, you’ll see a new tool bar at the top of the page with the following options:


- **Entire screen:** Capture the render of the entire screen currently displayed and send it to the Figma file.
- **Select element:** Choose a specific component or element on the page to capture.
- **Open file:** Open the Figma file to inspect your new design layers.


After you’ve captured all the information that you want to transfer to Figma, you can choose to either open the file or return to Codex. Codex will have the Figma file URL waiting for you.


## There and back again, an MCP story


Now that you’ve built your app and set up your design file, you’re ready to iterate. Here you can take full advantage of what the Figma canvas has to offer, including:


- Adding design system components
- Updating styles, fonts, and colors to variables
- Adjusting layouts and adding annotation instructions
- Crafting various interactions and empty states
- Collaborating on multiple variations and explorations


Once you’ve completed refining the UI, you can follow the same steps that we outlined at the[beginning of this blog](https://www.figma.com/blog/introducing-codex-to-figma/#starting-an-app-from-a-design) to pull those changes back into your application via the MCP server.


When code and the canvas are connected, you can move fluidly between execution and exploration. This roundtrip process unlocks the true power of the Figma MCP server with Codex—the ability to start from anywhere to craft meaningful, high-quality application experiences without compromising on speed.


To learn more about the Figma MCP server, review our[developer docs](https://developers.figma.com/docs/figma-mcp-server/tools-and-prompts/#generate_figma_design) or get started by installing the[Figma MCP server](https://help.figma.com/hc/en-us/articles/32132100833559) directly in the Codex desktop application.


Yarden is a Product Manager at Figma focused on developer tools across design, code, and AI. She’s passionate about enabling agent-powered workflows that expand teams and redefine how builders move from idea to production.


[LinkedIn](https://www.linkedin.com/in/yarden-chanel-katz-7b4ab3b0/)
