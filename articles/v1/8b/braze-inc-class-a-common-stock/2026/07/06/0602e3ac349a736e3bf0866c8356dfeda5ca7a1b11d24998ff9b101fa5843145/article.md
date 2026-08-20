---
schema_version: "1.0.0"
document_id: "0602e3ac349a736e3bf0866c8356dfeda5ca7a1b11d24998ff9b101fa5843145"
company_key: "braze-inc-class-a-common-stock"
company: "Braze Inc."
source_id: "braze-inc-class-a-common-stock-news-import-06f37ae7f1b6"
canonical_url: "https://www.braze.com/resources/articles/braze-mcp-server"
published_at: "2026-07-23T16:09:10.388+00:00"
first_seen_at: "2026-07-23T18:01:56.201300+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:95c444b4938af4c39c92c8e624e02fc512d618b41487e934e82f59bfb5bf389d"
---

# The Braze MCP Server: Unlock cross-workspace agility and conversational insights

***TL;DR***


*Braze is introducing its remote MCP Server, making it possible for brands to securely connect external AI tools (like Claude, ChatGPT, and Cursor) directly to their Braze environment. This unlocks an intuitive, conversational interface supporting seamless operations across workspaces, accelerated campaign production, and natural language data queries.*


***Key takeaways***


- *The Braze MCP Server is now in early access*
- *Brands leveraging the Braze MCP Server can use their preferred AI tools (e.g. Claude, ChatGPT) to manage their Braze environment, allowing AI to support more holistic connections between Braze and other layers of their marketing stack*
- *This powerful new tool can support data queries, workflow orchestration, and a range of highly specific industry use cases across retail, financial services, and beyond*


Managing lifecycle marketing programs across an enterprise organization requires structural flexibility. You may be managing multiple Braze workspaces in order to work across distinct business units, independent regional teams, or to separate staging environments from production. While this structure keeps data organized, it can introduce friction when your teams need to share assets or analyze performance holistically.


What if you could bypass the complexity entirely? Imagine exploring your Canvas, campaign, and customer engagement data via simple, everyday conversation—with no SQL, no tech support, and instant answers.


The Braze Model Context Protocol (MCP) Server (now available in Early Access) bridges these gaps by securely connecting external AI tools (like Claude, ChatGPT, and Cursor) directly to your Braze environment. This unlocks an intuitive, conversational interface that allows you to operate seamlessly across workspaces, accelerate campaign production, and query your data using natural language.


## Powering your entire AI ecosystem: Inside and outside Braze


The Braze MCP Server expands the[BrazeAI](https://www.braze.com/product/brazeai) ™ ecosystem, offering a powerful external counterpart to your in-product assistant, the[BrazeAI Operator](https://www.braze.com/resources/articles/brazeai-operator-product-spotlight) ™. Both are designed to accelerate your workflow, but they operate at different scales to support your day-to-day execution.


- **The BrazeAI Operator™:** Your native, in-product AI assistant that helps you go from idea to execution. Instead of having to figure things out manually, you can simply ask for help and get guidance. Acting both as a guide and a doer, the Operator understands the exact context you are currently viewing within Braze, helping you answer questions, optimize content, and build directly inside Braze.
- **The Braze MCP Server:** An ecosystem-wide interface that sits outside of the Braze dashboard within your preferred AI client, powered by the industry-standard open-source Model Context Protocol. It operates at a grander scale, allowing you to access assets, query data, and seamlessly orchestrate workflows across workspaces using natural language prompts.


By combining both tools, you can create a frictionless production pipeline. For example, you can use the Braze MCP Server to find and copy a successful campaign from a global workspace and duplicate it into your regional workspace. Once the asset is copied, you can access Operator in the regional workspace to help you edit the template, personalize for regional audiences, and launch a net-new campaign catered to your specific local audience.


## Move from insights to action with conversational data access


The ultimate value of the Braze MCP Server lies in how quickly it can transform raw data into real, strategic impact. Whether you’re reviewing performance or prepping for an upcoming meeting, it helps you discover and act on insights in seconds.


The Braze MCP Server helps you summarize campaign and Canvas performance to surface the key highlights, trends, and outliers that shape smarter marketing strategies. Beyond performance tracking, it streamlines your operations by spotting redundancies—like unused segments or duplicate attributes—and providing practical cleanup recommendations to keep your data healthy and clean.


Finally, it takes the stress out of preparation by easily generating charts, graphs, and strategic recommendations, helping you walk into your next meeting with the CMO armed with digestible, high-impact information that makes it possible for your team to confidently drive decisions forward.


Curious what that looks like in the day-to-day use cases?


- **Retail brands:** Evaluate cross-region promotion performance during holiday sales. Cross-reference loyalty segments between your North America and EMEA audiences to identify which region had the highest conversion rate on cart-abandonment campaigns.
- **Financial services:** Track multi-product onboarding journeys. Review user progression throughout the credit card application onboarding process and compare it directly to a premium checking account onboarding segment to spotlight conversion drop-off points.
- **Gaming brands:** Optimize live-ops and player lifecycle events. Pull daily active user (DAU) retention metrics from a recent seasonal event and easily compare those numbers against your baseline to see the lift in re-engagement.


## Fluid asset migration across workspaces


The Braze MCP Server allows you to share and move content between different areas of your organization. During a collaborative session, you can direct the MCP server to navigate to a specific workspace, locate a high-performing email template, and migrate the HTML or campaign structure directly into an entirely different workspace.


This approach offers several operational advantages:


- **Accelerate production cycles:** Move tested assets from staging environments directly into production workspaces with a single prompt.
- **Scale winning strategies:** Easily replicate successful campaigns and[Braze Content Blocks](https://www.braze.com/resources/articles/braze-content-blocks) built by one regional team across other organizational workspaces.
- **Maintain design consistency:** Pull centralized media assets or templates into any active workspace without manual downloads or uploads.


## Marketing on the Move


Modern marketing moves fast, and waiting until you're back at your computer isn't always an option. Whether you’re heading to the gate, in transit to a meeting, or grabbing a coffee between calls, the Remote MCP Server keeps you connected to your Braze environment directly from your mobile device.


Because MCP connects seamlessly to mobile AI apps like Claude, you can instantly inspect segments, verify campaign configurations, and pull key metrics wherever you are. Imagine you’re en route to a flight and need a quick, last-minute check on a segment built for an upcoming campaign.


- **Ask naturally from your phone:** "Can you show me the details for the Flash Sale test segment?"
- **Get instant, clear context:** Your AI client checks Braze via the MCP Server and instantly breaks down key details—such as segment filters, creation dates, applied tags, and assigned workspace permissions.
- **Follow up on the fly:** Simply reply with "Yes, pull its audience size and analytics," and receive real-time, actionable numbers in seconds while walking to your terminal.


## Enterprise-grade security and zero-friction setup


Data security is foundational to how the Braze MCP Server operates. Built completely on[OAuth standards](https://oauth.net/2/) , the server matches the security protocols of your existing enterprise dashboard.


- **Instant syncs:** You log in to the MCP server using your standard Braze credentials—including your company’s SSO or SAML configurations. All workspace settings and access levels sync automatically upon authorization.
- **Mirrored permissions:** The Braze MCP Server inherits the same user permissions, feature scopes, and workspace access assigned to your Braze account. The assistant cannot see or alter anything that you cannot access yourself.
- **No technical overhead:** There are no packages to install, no configuration files to manage, and no API keys to generate.


## Pro-tips for better conversational insights


To get the most out of your Model Context Protocol server, we recommend that brands keep these internal best practices in mind:


**Tip**


**How to apply it**


**Example**


*Be specific*


Avoid vague queries. Prompts with rich detail return the most accurate results.


"July 2026 campaign performance" instead of "recent performance."


*Dig deeper*


Follow up on initial prompts to guide the conversation and prioritize overall business impact.


"What are the actionable recommendations based on these drop-off rates?"


*Manage up*


Half of a marketer's job is showcasing impact internally. Use MCP to help shape your internal storytelling.


“Create a slide summary for my CMO to highlight revenue impact based on the 2026 Summer Sales Campaign.”


## Elevate your marketing workflow


The Braze MCP Server turns your preferred external AI assistant (e.g. Claude, ChatGPT, Copilot, Codex, and Cursor) into an active partner that understands the structure of your Braze environment and the depth of your data. By removing technical boundaries between environments, your teams can focus on scaling creative ideas and discovering growth opportunities in real time.


[Learn more](https://www.braze.com/docs/user_guide/brazeai/mcp_server) to get started today.
