---
schema_version: "1.0.0"
document_id: "55893aaeeeadd5dda24fd3609b5a7acf8e3b95bd93385bf377cf9d0bce2b7895"
company_key: "yc-inkeep"
company: "Inkeep"
source_id: "yc-inkeep-rss-006a915c529f"
canonical_url: "https://inkeep.com/blog/anthropic-openai-mcp-apps-extension"
published_at: "2025-11-25T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:13.852300+00:00"
fetched_at: "2026-07-28T22:25:07.688192+00:00"
content_hash: "sha256:00e1485f38f19eda9c738702571d6e79cec9b48e5473c47cb844b5c813f0f314"
---

# Anthropic and OpenAI Join Forces to Standardize Interactive AI Interfaces with MCP Apps Extension

In a rare display of collaboration between AI competitors, Anthropic and OpenAI have partnered with community maintainers to release the MCP Apps Extension (SEP-1865), a new specification that brings standardized interactive user interface capabilities to the Model Context Protocol. The announcement, made on November 21, 2025, addresses what the MCP team calls "one of the most requested features from the MCP community" and signals a maturing ecosystem where industry leaders are prioritizing interoperability over proprietary advantage.


The specification was authored jointly by MCP Core Maintainers from both OpenAI and Anthropic, alongside the creators of MCP-UI and leaders of the MCP UI Community Working Group. For tech leaders evaluating AI infrastructure investments, this collaboration carries implications that extend well beyond the technical details.


## The Problem: Fragmentation and Developer Burden


Until now, MCP servers have been limited to exchanging text and structured data with host applications. While sufficient for many use cases, this constraint creates significant friction when tools need to present visual information or collect complex user input.


Consider a practical scenario: a data visualization MCP server that returns chart data as JSON. Under the current model, the host application must interpret that data and render it appropriately. Every new data type or visualization requirement means additional development work on the client side. As requirements expand to include interactive elements, such as collecting multiple related settings from users, the complexity compounds rapidly.


The MCP community has developed creative workarounds, but the resulting patchwork of different implementations and conventions has made it increasingly difficult for servers to work consistently across clients. This fragmentation translates directly to increased development costs, slower time-to-market, and accumulating technical debt. The MCP team acknowledged this challenge directly: "This lack of standardization creates a real risk of ecosystem fragmentation - something we're working to proactively prevent."


## A Model for Industry Collaboration


The path to this specification demonstrates how AI infrastructure standards may evolve going forward. The MCP-UI project, created by Ido Salomon and Liad Yosef and maintained by a dedicated community, first pioneered the vision of agentic applications with interactive interfaces. The project proved that rich user interfaces could work as first-class MCP resources, fitting naturally within the protocol's architecture.


That community-driven innovation attracted enterprise attention. Major organizations including Postman, Shopify, Hugging Face, ElevenLabs, and Goose adopted MCP-UI's approach, validating the concept at scale. Separately, OpenAI's Apps SDK demonstrated demand for rich UI experiences within conversational AI interfaces, enabling developers to build interactive applications inside ChatGPT using MCP as its foundation.


Rather than allowing these parallel efforts to diverge into incompatible standards, Anthropic, OpenAI, and the MCP-UI team chose collaboration. The result is an official MCP extension that incorporates lessons from both commercial implementations and community innovation. For tech leaders, this pattern - community experimentation followed by enterprise validation and collaborative standardization - may preview how foundational AI infrastructure will develop across the industry.


## What the Specification Delivers


The MCP Apps Extension introduces several key capabilities designed to address the fragmentation problem while maintaining the security and reliability that enterprise deployments require.


**Pre-declared UI resources** use a dedicated` ui://` URI scheme and are referenced in tool metadata, making interfaces predictable and auditable. This approach ensures that hosts can understand what UI capabilities a server provides before any interaction occurs.


**HTML-first implementation** means the initial specification supports` text/html` content rendered in sandboxed iframes. This design choice prioritizes universal compatibility and leverages existing web development skills rather than requiring teams to learn new frameworks.


**Security-first architecture** incorporates multiple protection layers including iframe sandboxing, predeclared templates, auditable messages, and user consent mechanisms. For organizations with strict security requirements, these safeguards are built into the foundation rather than bolted on afterward.


**Bidirectional communication** between embedded interfaces and host applications uses the existing MCP JSON-RPC protocol over postMessage, adding interactive capabilities without introducing new complexity to the communication layer.


**Backward compatibility** ensures that MCP Apps is an optional extension. Existing implementations continue to function without modification, and organizations can adopt the new capabilities at their own pace.


The MCP team frames the broader vision clearly: "The MCP Apps Extension is starting to look like an agentic app runtime: a foundation for novel interactions between AI models, users, and applications." The specification is intentionally lean, establishing core patterns that can expand based on real-world feedback.


## Strategic Implications for Tech Leaders


**For organizations already building on MCP** , this announcement warrants close attention. The expansion from protocol to platform increases MCP's strategic value, and early engagement with UI capabilities may provide competitive advantages as the ecosystem matures. Teams should monitor the specification's evolution and consider participating in the feedback process to ensure their use cases are represented.


**For organizations evaluating AI infrastructure options** , the collaboration model itself is informative. Multi-company investment in an open standard suggests longevity and stability. The participation of both Anthropic and OpenAI - direct competitors in many markets - reduces concerns about vendor lock-in and signals confidence in MCP's long-term viability.


**For the broader industry** , this development suggests that AI infrastructure is trending toward interoperability rather than fragmentation. The willingness of major players to collaborate on foundational layers, while competing on applications and services built atop those layers, mirrors patterns seen in successful technology ecosystems from cloud computing to mobile platforms.


## What Comes Next


The MCP Apps Extension is currently a proposal open for community input. Tech leaders interested in shaping the specification can review the full details in SEP-1865, submit feedback through GitHub Issues, and join discussions in the #ui-wg channel on the MCP Contributors Discord.


For those ready to experiment, prototype implementations are available for testing. The specification team is actively seeking real-world feedback to inform the standard's evolution.


This announcement marks more than a technical milestone. It represents an inflection point where competing AI organizations chose ecosystem health over short-term proprietary advantage. The collaborative model established here may well become the template for how the AI industry builds its foundational infrastructure in the years ahead.


*This article is based on the official announcement from the Model Context Protocol team published November 21, 2025. For technical implementation details, consult the SEP-1865 specification directly.*
