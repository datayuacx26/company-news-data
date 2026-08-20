---
schema_version: "1.0.0"
document_id: "c73781a108e2c6b1a10dc2fd1b9285932c13a8356cd365d41ef03bd6fa8ac6f4"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/how-to-protect-your-mcp-server-with-fastly/"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:b44e4e8ca2bc9e668ac6764edd0ef8ca18d5cce2958a615d35b4b0b64a9f8288"
---

# How to protect your MCP Server with Fastly

As organizations rapidly adopt AI agents and agentic workflows, Model Context Protocol (MCP) servers are becoming a foundational integration layer between AI models and enterprise systems. While they accelerate development and simplify tool integration, they also introduce a new class of security challenges. Unlike traditional API traffic that’s typically deterministic and narrowly scoped, MCP traffic enables dynamic tool discovery, natural language-driven interactions, and orchestration across trust boundaries. These characteristics expand the attack surface and require security controls that extend beyond conventional API protection.


When a major global publisher approached Fastly asking for help securing their MCP server backbone connecting their AI agents with real-time financial data and tools, our team sprang into action to build a prototype that any organization can follow the architecture of to protect theirs. The following outlines the process and outcomes they achieved, providing a practical reference for organizations working to secure their own MCP environments.


## Fastly MCP Protection Prototype


To protect their MCP server, the Fastly prototype needed to adhere to three key requirements:


-


**Detect and mitigate AI-specific attack vectors** – OWASP and AI-specific attacks targeting the MCP server must be mitigated


-


**Ensure production-scale performance** – Sub-100ms latency is mandatory and the solution must handle at least 100Mbps throughput


-


**Allow authentication complexity** – A mix of IP-based, token-based, and mTLS authentication models must be allowed without false positives


To address these requirements, the prototype combined the flexibility and customization of multiple existing Fastly capabilities. While we won’t dive into the details of their unique implementation (but are happy to help your organization mirror the security outcomes privately), each of these solutions provided the customer with another layer of protection for their MCP server.


Here’s how the prototype paired them together to solve the customer’s requirements:


-


[Fastly’s Global Network](https://www.fastly.com/products/cdn) provides more than just 578 Tbps global capacity and performance benefits, enabling customers to implement custom traffic logic as it enters the network. This configuration flexibility allowed rules to be implemented as intended MCP traffic hit our network that automatically blocked unexpected content types like xml and restrict HTTP methods.


-


[Fastly Bot Management](https://www.fastly.com/products/bot-management) **** provides visibility into the automated traffic heading to the MCP server, allowing rules to be created to block unwanted automation clients, bots with bad fingerprints, and generally malicious bot traffic.


-


[Fastly Next-Gen WAF](https://www.fastly.com/products/web-application-api-protection) **** provides the ability to mitigate OWASP attacks and LLM-specific attacks like encoded prompt injection and jailbreak phrases. The prototype leverages its rate limiting capabilities to incorporate policies against IPs or API keys that exceeded expected norms.


-


[Fastly Media Shield](https://www.fastly.com/products/streaming-media/media-shield) **** provides a designated Fastly POP as anchor for all MCP server traffic before it ultimately goes to origin. This enables the prototype to reduce origin load while speeding up connections by reducing the time required for costly multi-roundtrip handshakes.


### Key results


With these four solutions implemented and tuned for protecting the customer’s MCP server, the team ran a 10-week trial to see how it stacked up against the customer’s requirements and found it far exceeded them.


Requirement


Desired Metric


Result


Latency (average)


< 100ms


**Sub 50 ms**


Upload throughput


100 Mbps


**Achieved**


Max payload size


200MB+


**500MB+ without timeouts**


Authentication Flexibility


IP-based, token-based, and mTLS model acceptance


**Achieved**


## Fastly for AI Infrastructure


Fastly's platform is uniquely positioned in the request path to secure and accelerate AI agent infrastructure because it was purpose-built for the edge where performance and security must coexist.


-


**Performance without compromise:** Inspection adds negligible latency; Fastly's global scale and resilience keeps AI agents performant


-


**Programmable security:** Network and Next Gen-WAF rules can be tuned for the novel patterns of MCP and agentic traffic


-


**Real-time visibility:** Security teams see everything, in near real-time, without waiting for batch log exports


-


**AI-native threat mitigation:** Policies for prompt injection, tool abuse, and bot-driven enumeration can be easily created and paired with out-of-the-box OWASP-style attack protections


As AI and associated technologies continue to rise in adoption, Fastly’s MCP server protection prototype is just one example of how Fastly can help your business confidently adopt emerging technologies without increasing risk.[Contact us](https://www.fastly.com/contact-sales) if you’d like to learn more about the prototype or how we can help your business in the AI era.
