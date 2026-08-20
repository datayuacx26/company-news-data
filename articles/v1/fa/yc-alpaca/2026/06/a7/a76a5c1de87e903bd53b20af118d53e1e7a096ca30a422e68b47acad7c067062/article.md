---
schema_version: "1.0.0"
document_id: "a76a5c1de87e903bd53b20af118d53e1e7a096ca30a422e68b47acad7c067062"
company_key: "yc-alpaca"
company: "Alpaca"
source_id: "yc-alpaca-rss-4f5e7678c8d4"
canonical_url: "https://alpaca.markets/blog/alpaca-launches-skills-library-for-ai-agents/"
published_at: "2026-06-17T13:49:54+00:00"
first_seen_at: "2026-07-20T23:23:37.146111+00:00"
fetched_at: "2026-07-28T20:48:41.890548+00:00"
content_hash: "sha256:7ce9493885e798feaff67c7c6b0f4f11f4aaffae3f3f6e335e4336db216f326a"
---

# Alpaca Launches Skills Library for AI Agents

Today, we’re introducing the[Alpaca Skills Library](https://github.com/alpacahq/alpaca-skills/?ref=alpaca.markets) , an open-source collection of agent skills designed to help AI assistants run repeatable workflows with Alpaca's API, MCP Server, and CLI.


The first release is the[Trading API Backtesting Skill with CLI](https://github.com/alpacahq/alpaca-skills/tree/main/skills/trading-api/backtest?ref=alpaca.markets) . It helps your AI agent move from a strategy idea to a documented research run by guiding strategy formalization, historical data retrieval, benchmark comparison, assumption tracking, and reproducible reporting through Alpaca’s Trading CLI. Anyone can use it to transform strategy ideas into documented research runs with traceable data, structured artifacts, and decision-ready evidence.


Alpaca has long focused on API-first infrastructure for builders. As more people use AI assistants to write and run trading workflows, the Skills Library provides shared instructions, guardrails, and reporting standards so agents can produce more consistent results.


## **What is the Alpaca Skills Library?**


[Alpaca Skills Library](https://github.com/alpacahq/alpaca-skills/?ref=alpaca.markets) is an open-source workflow package for AI agents. Each skill is a SKILL.md file with step-by-step instructions your agent can follow when asked to complete a task, such as running a historical backtest or fetching market data through Alpaca’s Trading API and Market Data API.


If you use an AI coding assistant that supports skills (Claude Code, Codex, etc), you can add a skill from the repository and invoke it through natural language.


Skills are designed to help agents:


- **Formalize intent** : Translate a freeform strategy idea into precise rules before any code runs.
- **Fetch data consistently** : Get historical bars, quotes, calendars, and corporate actions through Alpaca’s Market Data API.
- **Report transparently** : Return benchmarks, assumptions, caveats, and artifact paths in a standard format.
- **Stay reproducible** : Save run folders, raw outputs, and data fingerprints so you can compare results across runs.


Now, you can browse skills on GitHub, install them in supported AI coding assistants, and contribute improvements or publish new skills for the community.


## **First in the Library: Trading API Backtesting Skill**


The[Trading API Backtesting Skill](https://github.com/alpacahq/alpaca-skills/tree/main/skills/trading-api/backtest?ref=alpaca.markets) is the first skill in the library. It helps your AI agent transform a start date, end date, and strategy concept into a structured historical backtesting workflow.


Before running a simulation, the skill instructs the agent to formalize the strategy, confirm assumptions with you, document fill models, indicator definitions, and benchmark choices. During the run, the agent fetches historical market data through Alpaca’s CLI, executes a local simulation script, and writes structured outputs such as notes.md, summary.json, report.md, trades.csv, and equity.csv.


Key capabilities include:


- Strategy formalization before execution
- Deterministic backtesting workflows
- Standardized benchmark comparisons
- Data fingerprints for reproducibility
- Transparent assumptions and disclosures
- Structured reports and performance summaries
- Optional paper trading validation workflows


## **How to Install Alpaca Skills**


Clone the[alpaca-skills repository](https://github.com/alpacahq/alpaca-skills?ref=alpaca.markets) . Each skill is a folder with a SKILL.md file inside. Put those folders where your coding assistant looks for skills.


- **Claude Code** : Add skill folders to` .claude/skills/` in your project, or to` ~/.claude/skills/` for all projects. Claude finds them on its own. You can type /skill-name to run one directly.
- **Codex** : Add skill folders to` .agents/skills/` in your project, or to` ~/.agents/skills/` for all projects. Codex finds them on its own. You can also type $skill-name to run one directly.
- **Cursor** : Add skill folders to` .cursor/skills/` in your project, or to` ~/.cursor/skills/` for all projects. Cursor also reads` .agents/skills/` . One option is to clone the whole repo to` ~/.agents/skills/alpaca` so every skill is available everywhere.


Each skill is plain Markdown. You can read and edit the instructions before you run a backtest or other task.


## **Contribute to Alpaca Skills Library**


Skills Library is built with Alpaca’s open-source community in mind. You can contribute new skills, improve existing workflows, and share specialized knowledge with other builders using Alpaca’s APIs, MCP Server, and CLI.


This is the first of many skills we plan to release. Future additions will cover trading, market data, research workflows, and agent-native automation.


## **Getting Started with AI Trading at Alpaca**


Over the last few months, we’ve released[Alpaca’s MCP Server V2](https://docs.alpaca.markets/us/docs/alpaca-mcp-server?ref=alpaca.markets) and[CLI](https://docs.alpaca.markets/us/docs/alpacas-cli?ref=alpaca.markets) , making it easier to start trading with Alpaca in natural language or by using your computer’s command-line tool.


To get started, see the latest API specs here:


- Trading API:[https://docs.alpaca.markets/us/openapi/trading-api.json](https://docs.alpaca.markets/openapi/trading-api.json?ref=alpaca.markets)
- Market Data API:[https://docs.alpaca.markets/us/openapi/market-data-api.json](https://docs.alpaca.markets/us/openapi/market-data-api.json?ref=alpaca.markets)


---


*The content of this article is for general informational purposes only.*


*Insights generated by our MCP server, CLI and connected AI agents are for educational/ informational purposes only and not investment advice. Backtests are hypothetical historical simulations and do not represent actual trading performance* . *Please conduct your own due diligence before making any decisions. All firms mentioned operate independently and are not liable for one another.*
