---
schema_version: "1.0.0"
document_id: "936c3ef8d89a6892c1cebac4ce481a90ec9364c42cef32147af15b81aff503d1"
company_key: "yc-osmosis"
company: "Osmosis"
source_id: "yc-osmosis-news-import-de01a8838bba"
canonical_url: "https://osmosis.ai/blogs/exploring-foundation-models-tool-use-efficacy"
published_at: "2025-10-09T00:00:00+00:00"
first_seen_at: "2026-07-25T18:22:44.798253+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:650edb6d1c58700f988f229732e267c4ac5998d66532fcc88946c569b07a8791"
---

# Exploring Foundation Models' Tool-Use Efficacy

Model Context Protocol (MCP) is an open protocol launched by Anthropic to standardize the way LLMs use external tools. AI agents use MCP to enable multi-turn workflows, where an LLM (via products like[Claude Desktop](https://claude.ai/download) or[Cursor](https://docs.cursor.com/en/tools/mcp) ) can select and coordinate between tools in multiple MCP servers. Since its introduction, MCP has quickly become the de facto standard for tool integrations with LLMs.


There are now thousands of official and unofficial MCP servers, each with dozens of tools! While more MCP choices are great for the tool integration ecosystem, sometimes having too many options is a curse. Products like Cursor often limit how many tools you can provide to an LLM, so you are forced to select which tools you want to utilize the most.


At Osmosis, we perform real-time reinforcement fine-tuning with tools. Given the limitations of popular LLM products, we wanted to analyze the robustness of multi-tool use in various foundation models to see if we could improve upon multi-tool use in the future.


At the start of our experiments, we surmised that the foundation models would be relatively close in tool-use capabilities - at the very least no large disparities. We set out to determine the impact of tools on model performance with:


- [A set of questions](https://github.com/Osmosis-AI/tool-project/blob/main/experiment/pretrain_knowledge_tests.json) that can be answered with or without tools (e.g. “What is a Dutch oven?”)
- [A set of questions](https://github.com/Osmosis-AI/tool-project/blob/main/experiment/tool_use_tests.json) that require the use of tools (e.g. “What is the current weather in New York City?”)


For the former, we tested models with and without access to tools - for the latter, we tested models with just the relevant tools, and with all tools available. We tested the most popular closed-source models (OpenAI o3, OpenAI GPT-5, Claude Sonnet 4, and Gemini Pro 2.5) against some of the most recent SOTA open-source models (Kimi K2 and gpt-oss-120b). We ran each model five times for each question.


It was difficult to fully assess the relationship between tool quantity and performance due to ‘unforced errors’ like failed tool-calling (i.e. authentication, rate-limits, etc.) and models simply deciding to not use tools.


We frequently saw models fail to use tools when it was necessary:


```text
{
"query"  :  "List the first 5 emails in my Gmail inbox"  ,
"model"  :  "openai/o3"  ,
"experiment"  :  "test_tool_usage_info_gathering_with_all_mcp_servers"  ,
"response"  :  "It looks like I need a query parameter (for example, “in:inbox”) and a maximum number of results to retrieve. Could you please confirm that you’d like me to retrieve the first 5 messages from your Inbox?"  ,
}


```


Or use too many and/or irrelevant tools for tasks:


```text
{
"query"  :  "In a hydraulic press operation, the output piston cannot"  ,
"model"  :  "anthropic/claude-sonnet-4"  ,
"success"  :  true  ,
"response"  :  "It seems like your question about the hydraulic press operation was cut off. You mentioned \"the output piston cannot\" but didn't complete the thought.\n\nCould you please finish your question? For example, are you asking about:\n- The output piston cannot move?\n- The output piston cannot generate enough force?\n- The output piston cannot return to its original position?\n- Something else?\n\nOnce you provide the complete question, I'll be happy to help explain the hydraulic press operation and troubleshoot any issues you're experiencing."  ,
"metrics"  : {
"tools_used_list"  : [
"get_me"  ,
"get_issue"  ,
"list_issues"  ,
"search_issues"  ,
"create_issue"  ,
"update_issue"
],
"input_tokens"  :  806496  ,
"output_tokens"  :  4196
},
"error"  :  null  ,
"evaluation"  :  "failure"
}


```


We saw that GPT-5 performed best out of tested models - but interestingly, we saw significant drops in performance on the same question as we changed the configuration from giving the model access to all tools, to just the relevant tools for a specific problem (e.g. Google and Perplexity tools for browsing).


For GPT-5, we speculate there was a skew during its post-training process that favored retrieving its own state / user identities. GPT-5 could potentially generalize better by starting with its 'comfort zone' - with the opposite being worse and out of distribution. We observed that GPT-5 frequently used` get_me` (Slack tool) in unnecessary situations:


```text
{
"query"  :  "Who is the best basketball player of all time?"  ,
"model"  :  "openai/gpt-5"  ,
"experiment"  :  "test_tool_usage_browsing_with_all_mcp_servers"  ,
"response"  :  "Short answer: Most fans and analysts still pick Michael Jordan.\n\nWhy it’s debated:\n- Michael Jordan: unmatched peak (6–0 in Finals, 6 Finals MVPs, 5 MVPs, 10 scoring titles, elite defense).\n- LeBron James: longevity and versatility GOAT (all-time points leader, 4 titles, 4 MVPs, record All-NBAs, elite across roles).\n- Kareem Abdul-Jabbar: résumé GOAT (6 MVPs—the most, 6 titles, decades of dominance, unstoppable skyhook).\n- Bill Russell/Wilt Chamberlain: rings (Russell’s 11) and record-shattering stats (Wilt) in earlier eras.\n\nIf you tell me your criteria (peak dominance, longevity, titles, two-way impact, era strength), I can give a tailored pick. My default pick: Jordan for peak dominance and two-way impact on the biggest stage."  ,
"error"  :  null  ,
"metrics"  : {
"tools_used_list"  : [
"get_me"  ,
"get_tag"
],
"input_tokens"  :  31152  ,
"output_tokens"  :  769  ,
"total_tokens"  :  31921  ,
}
}


```


Access to tools is a crucial piece to scaling AI agents towards more complex use cases. Any company that builds AI agents will need to use tools. While post-training on tool-use is now common for new model releases, there are still many shortcomings for these models - and addressing edge cases is difficult for generalized models.


At Osmosis, we’ve been helping companies train open-source models to beat state-of-the-art foundation models with better latency at a fraction of the cost. By focusing on a particular domain, use case, or set of tools, it’s possible to achieve significantly better reliability than what’s available out-of-the-box. We already support multi-turn tool training, which allows AI agents to learn the same tools they’d use in real production settings. If you’re interested in learning more,[reach out](https://osmosis.ai/cdn-cgi/l/email-protection#452e2436203c052a36282a362c366b242c) !


---


[Full Evaluation Framework](https://github.com/Osmosis-AI/tool-project)
