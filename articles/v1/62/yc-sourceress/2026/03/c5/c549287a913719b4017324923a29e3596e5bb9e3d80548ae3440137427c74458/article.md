---
schema_version: "1.0.0"
document_id: "c549287a913719b4017324923a29e3596e5bb9e3d80548ae3440137427c74458"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/vet"
published_at: "2026-03-05T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:4d3894cb0d4f1986c838119a43e311466654aa63af35ebf7976e0325f2e484e5"
---

# Vet: Prevent coding agents from making mistakes

One line. That’s all it takes to set up and run Vet, a fast and local code review tool built by our team. It’s concise where other tools are verbose, and it catches more relevant issues.


` curl -fsSL https://raw.githubusercontent.com/imbue-ai/vet/main/install-skill.sh | bash`


When you’re using an agent to write code, you ask for a feature, the agent hits a wall, and silently swaps in fake data instead of telling you. You ask it to write tests, it tells you they pass, but it never ran them. You may not notice until later, or you may not notice at all. With the Vet skill your agent will run Vet after writing code, finding the issue for the agent to remedy.


Vet verifies your coding agent’s work by considering your conversation history to ensure the agent’s actions align with your requests.


It can review entire PRs, too. Vet’s reviews detect logic errors, unhandled edge cases, and deviations from stated goals with high precision.


Here’s a short video introduction:


Setting up Vet takes one line because it uses your existing API keys. It’s open source, works with local models, and has zero telemetry. Run from the CLI, CI, or as an agent skill.


Get started on[GitHub](https://github.com/imbue-ai/vet) | Build with us on[Discord](https://discord.gg/sBAVvHPUTE)
