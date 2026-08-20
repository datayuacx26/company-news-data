---
schema_version: "1.0.0"
document_id: "735b354a0f8769bad9d253dea68a6e94160f120b6bbc58b1fe420e9e63e3c96c"
company_key: "pinterest-inc-class-a-common-stock"
company: "Pinterest Inc."
source_id: "pinterest-inc-class-a-common-stock-rss-45b43224fdd9"
canonical_url: "https://medium.com/pinterest-engineering/an-engineers-guide-to-better-ai-skills-implementing-a-testing-process-to-optimize-agent-a000c9c9abcd"
published_at: "2026-05-12T16:01:00+00:00"
first_seen_at: "2026-07-20T04:35:24.920473+00:00"
fetched_at: "2026-07-28T22:13:25.865061+00:00"
content_hash: "sha256:1c5fe0fd867d396613313efc95e9efd68845b154ebeed5e095a27c775430b593"
---

# An Engineer’s Guide to Better AI Skills: Implementing a Testing Process to Optimize Agent…

# **An Engineer’s Guide to Better AI Skills: Implementing a Testing Process to Optimize Agent Performance in Any Repository or Skill**


[Pinterest Engineering](https://medium.com/@Pinterest_Engineering?source=post_page---byline--a000c9c9abcd---------------------------------------)


4 min read


·


May 12, 2026


--


Author: Daniel Reed


Press enter or click to view image in full size


The tech industry is currently seeing a massive overhaul in the way we work and many are enjoying the benefits of AI agents, particularly when automating engineer workflows and serving domain-specific knowledge. However, relying on agents to consistently invoke a custom skill can be surprisingly unreliable at times.


When adopting a new skill intended to help agents write code for Pinterest’s iOS architecture (I’ll call it rx-mvvm) we discovered that sometimes our knowledge skill wasn’t being loaded into our agents. To address this, we conducted a series of tests on Pin-agent (an internal fork of OpenAI’s Codex) and Claude Code to quantify the reliability of skill invocation and identify some best practices to maximize performance. This was a direct result of observing agents struggling to meet the skills bar during architectural reviews. We found that by applying different techniques we could track and drastically improve skill invocation rates on both tested agents.


**How to Build A Skill Test Harness**


Building a reliable test harness for agent skill invocation requires three key components working in concert. The Core Tool is a Bash script that orchestrates automated testing by piping prompts to your agent and capturing verbose output logs. The core execution is simple:


```text
if echo "$prompt" | claude --print --verbose --output-format stream-json > "$log_file" 2>&1; then      command_success=true  fi
```


The script runs all test cases in sequence, collecting logs for later analysis. We ran the entire suite multiple times to account for the nondeterministic nature of agents. Prompts were categorized into two categories defined as arrays:


*Positive Cases* — 15 prompts covering the full spectrum of skill domains:


```text
CORE_PROMPTS=(      "load the rx-mvvm-architecture skill"      "check if this follows rx-mvvm patterns"      # ... 13 more cases  )
```


*Negative Cases* — 5 general programming prompts designed to expose false positives:


```text
EDGE_PROMPTS=(      "fix this Swift compilation error"      "write unit tests for this View"      "refactor this function"      # ... 2 more cases  )
```


We then use log parsing heuristics on the json output logfiles to detect skill invocation by searching for telltale patterns in the JSON-streamed debug output.


```text
skill_invoked_claude() {      local log_file="$1"       if grep -q '"name":"Skill"' "$log_file" && grep -q '"command":"rx-mvvm-architecture"' "$log_file"; then          return 0      elif grep -q 'Launching skill: rx-mvvm-architecture' "$log_file"; then          return 0      else          return 1      fi  }
```


The script finally tallies successes across both categories and computes three key metrics with clear formulas:


```text
CORE_SUCCESS_RATE=$(awk "BEGIN {printf \"%.1f\", ($CORE_SKILL_INVOKED / $CORE_TOTAL) * 100}")  EDGE_FALSE_POSITIVE_RATE=$(awk "BEGIN {printf \"%.1f\", ($EDGE_SKILL_INVOKED / $EDGE_TOTAL) * 100}")  OVERALL_ACCURACY=$(awk "BEGIN {printf \"%.1f\", ($TOTAL_CORRECT / $TOTAL_TESTS) * 100}")
```


**What we learned: optimizations**


Our initial “vanilla” testing revealed that neither agent could guarantee 100% skill invocation, particularly when engineers used terse or ambiguous prompts. The baseline performance was an overall accuracy of 73% for Codex and 62% for Claude. This low reliability is unacceptable for critical engineering workflows.


Our research confirmed that the performance of both tools can be dramatically improved, with the increase being much greater for Codex than for Claude. We found there were many ways to improve skill invocation rates:


- Frontmatter description:
— Including more contextual information (like architectural components) in the skill description in the frontmatter YAML (the section at the top) is a great way to improve performance.
— This gave us measurable gains that were agnostic to agent choice
- Aggressive Language:
— Applying aggressive, all caps commands like “YOU MUST LOAD THIS SKILL IF” in the frontmatter is another way to signal importance
— I personally think this is a little silly, not to mention ugly
- [AGENTS.md](http://agents.md/) :
— Adding a table of skills to the[AGENTS.md](http://agents.md/) file, along with reasons to choose to use them is another optional way to improve skill loading
— Teams will want to balance this against the desire to save tokens in their context window by keeping their[AGENTS.md](http://agents.md/) files small.
- Combination:
— Applying multiple techniques concurrently is a way to compound the gains, but only if you’re a Codex user. We didn’t see these gains matched while using Claude code.
— We also were surprised to find that asking the agents to improve on our additions did not further improve our invocation rates– it actually went down a bit.


Below is a table detailing what we found in our runs. For Codex, we used GPT 5.2-codex and for Claude we were using Opus 4.5.
(100 tests = 5 runs * (15 “positive” + 5 “negative”) tests)


Press enter or click to view image in full size


**Conclusion**


I would be in remiss if I didn’t say that the test prompts we are using are intentionally terse — they’re meant to catch edge cases. This isn’t an indictment of agent skills, models or harnesses. Every single test case during every single run on both agents loaded the skill when the prompt explicitly said ‘load this skill’. The primary method of reliable skill invocation is a good plan, verbose instruction and clear intent from the developer.


The overarching lesson we learned through this process was that not only is it possible to empirically test how often we were loading the skills we expected, it’s something we should encourage, adopt and improve upon so that our agentic AI coding tools become more effective. However, even with a fully optimized skill the engineers working with AI have a responsibility to use high quality and thorough prompts. Teams should follow both of these rules to unlock the full potential of AI agents for domain specific work.
