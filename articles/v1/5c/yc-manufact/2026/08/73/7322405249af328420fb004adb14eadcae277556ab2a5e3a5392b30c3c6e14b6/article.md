---
schema_version: "1.0.0"
document_id: "7322405249af328420fb004adb14eadcae277556ab2a5e3a5392b30c3c6e14b6"
company_key: "yc-manufact"
company: "Manufact (formerly mcp-use)"
source_id: "yc-manufact-news-import-914a697bc101"
canonical_url: "https://manufact.com/blog/skills-over-mcp"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-12T09:14:28.343900+00:00"
fetched_at: "2026-08-12T09:14:29.185762+00:00"
content_hash: "sha256:f4d0ff0f2eafdb58647cb885cef4a092563ee5f7a947745f4db3cb117bfa8f7f"
---

# What are Skills over MCP? | Manufact Blog

What does an agent need to do useful work? MCP connects an agent to your tools, data, and services, such as Slack or Linear. A skill gives an agent a playbook for a specific task, such as drafting and sending a Slack message in your writing style.


Skills over MCP lets a service expose both through one connection: the actions an agent can take and the task knowledge behind each one.


Check out the Skills Extension SEP, as well as our mcp-use example on how to implement them.


- [SEP-2640: Skills Extension](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/2640)
- [mcp-use Skills over MCP example](https://github.com/mcp-use/mcp-use/tree/main/libraries/typescript/packages/server/examples/skills-over-mcp)


# What is Skills over MCP?


Skills over MCP follows the[Agent Skills specification](https://agentskills.io/specification) . A Skill is a directory that contains a SKILL.md file at its root. A Skill may also contain additional files and subdirectories that may contain references, scripts, or examples.


Here is an example of what a Skill looks like:


```text
skills/
├── purchasing/
│   ├── SKILL.md
│   ├── references/approval-policy.md
│   └── templates/purchase-order.md
└── refunds/
├── SKILL.md
├── references/policy.md
└── templates/confirmation.md
```


```text
---
name: purchasing
description: Create approved purchase orders with the create-purchase-order tool
---


# Purchasing


Before creating a purchase order, read the
[  approval policy  ]  (  references/approval-policy.md  )  . When the purchase is
approved, call   `  create-purchase-order  `   with the SKU and requested quantity.
Use   [  the order template  ]  (  templates/purchase-order.md  )   when confirming the
purchase.
```


An MCP Server serves Skills via its resources primitive. An MCP resource is just addressable content a server exposes by URI.


The server advertises support for Skills in it's initialization handshake:


```text
{
"capabilities"  : {
"extensions"  : {
"io.modelcontextprotocol/skills"  : {}
}
}
}
```


The agent lists these skills through a` skills


/


list


`


message which returns available Skills from the server:


```text
{
"skills"  : [
{
"uri"  :   "skill://purchasing/SKILL.md"  ,
"name"  :   "purchasing"  ,
"description"  :   "Create approved purchase orders with the create-purchase-order tool"  ,
"resources"  : [
{
"uri"  :   "skill://purchasing/references/approval-policy.md"  ,
"digest"  :   "sha256:9f36c8a14b72"
},
{
"uri"  :   "skill://purchasing/templates/purchase-order.md"  ,
"digest"  :   "sha256:4d7a2e90c615"
}
]
},
{
"uri"  :   "skill://refunds/SKILL.md"  ,
"name"  :   "refunds"  ,
"description"  :   "Check refund eligibility and use the refund-order tool safely"  ,
"resources"  : [
{
"uri"  :   "skill://refunds/references/policy.md"  ,
"digest"  :   "sha256:72ab5d3c8e19"
},
{
"uri"  :   "skill://refunds/templates/confirmation.md"  ,
"digest"  :   "sha256:c14f6b908a3d"
}
]
}
]
}
```


The Skills catalog returns a list of available skills, each with its related resources, as well as a digest for each file. The digest lets a host verify fetched content matches the catalog.


Here's the flow of how an MCP Client can read the Skills from a server:


Loading diagram...


# Serve Skills with mcp-use


mcp-use offers first class support for Skills over MCP. It's as easy as dropping in a` skills


/


`


folder and mcp-use automatically advertises and serves the Skills.


```text
shopping-mcp/
├── src/index.ts
└── skills/
└── purchasing/
├── SKILL.md
├── references/approval-policy.md
└── templates/purchase-order.md
```


And that's all you need in order to ship your Skills and MCP together!


Skills over MCP support is now available in[mcp-use v2.1.0](https://github.com/mcp-use/mcp-use) .


Try it now in the[mcp-use Inspector](https://inspector.manufact.com/inspector)
