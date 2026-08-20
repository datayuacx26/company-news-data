---
schema_version: "1.0.0"
document_id: "4cbf6d55fc6d1ac139c01a53af4ec7d545e1d2277c40b2a649880aeccfd7b2c3"
company_key: "yc-manufact"
company: "Manufact (formerly mcp-use)"
source_id: "yc-manufact-news-import-914a697bc101"
canonical_url: "https://manufact.com/blog/10000-github-stars"
published_at: null
first_seen_at: "2026-07-22T03:18:57.853834+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:cd439f5698af4c42cbbd2baf26bdf1508f83287b666e828ab9a0faf024a528b4"
---

# The road to 10,000 stars

mcp-use was born on March 28, 2025. The MCP protocol was released just a few months before and it was starting to gain traction. The first clients that supported MCP were Cursor and Claude Desktop, where you could connect your MCP server by pasting a JSON configuration file.


The most popular MCP server at the time was[Playwright](https://github.com/microsoft/playwright-mcp) , that gave agents access to Chrome. Other popular MCP servers were all development oriented, docs, GitHub etc. We immediately saw the bigger picture. At the time MCP was a developer only product, but the ability to give agents access to external systems by just plugging in an external component could have much bigger impact.


If this was going to be true, proprietary closed source applications could not be the only way to use MCP servers.


So we wrote mcp-use, it was a simple library that allowed you to connect any MCP server to any LLM in six lines of code. Fun fact: initially the library was called pymcp, then mcpeer, it landed on mcp-use just after (inspired by our friends at[browser-use](https://browser-use.com/) ).


As every open source project, mcp-use started with a README file and relentless posting on Reddit. Demo after demo, we started collecting some interest and feedback. The first demo that went "viral" was the one that showed how to control a curtain with MCP. It showed that MCP is much more than a way to connect your coding agents to documentation.


The first README The first Reddit post Controlling curtains with MCP


The initial demos are always hard. The product is so simple that it seems like it is not worth launching. The first updates are so small that it seems hard to think that anybody will care.


Fortunately, we did not stop, and at some point[LangChain](https://www.langchain.com/) picked up on it and decided to shout it out, giving massive visibility. The post went viral and so our repo.
