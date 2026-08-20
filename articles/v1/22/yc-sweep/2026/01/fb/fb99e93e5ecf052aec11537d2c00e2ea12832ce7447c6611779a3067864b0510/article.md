---
schema_version: "1.0.0"
document_id: "fb99e93e5ecf052aec11537d2c00e2ea12832ce7447c6611779a3067864b0510"
company_key: "yc-sweep"
company: "Sweep"
source_id: "yc-sweep-rss-0fc78e2214cd"
canonical_url: "https://blog.sweep.dev/posts/read-file"
published_at: "2026-01-30T00:00:00+00:00"
first_seen_at: "2026-07-24T02:57:34.319784+00:00"
fetched_at: "2026-07-28T22:22:54.577865+00:00"
content_hash: "sha256:b4ab228b6dc035e2a6f468101ee8dee8ef79d043db83f32efb67315ea409c782"
---

# A better way for coding agents to read files

[posts](https://blog.sweep.dev/posts/autocomplete-context) A better way for coding agents to read files


# A better way for coding agents to read files


Veronica


•


January 30, 2026


*We decreased token usage for ultra-large files by 90%.*


---


When coding agents need to understand a codebase, they typically use a combination of searching and reading files to find the relevant parts. This works fine for small files, but becomes prohibitively expensive for large ones.


Consider a scenario where an agent needs to understand a 5,000-line file to add a new endpoint. Reading the entire file consumes 40,000+ tokens - that’s $0.20 with Claude Opus. But the cost isn’t just financial: you’re also waiting for the model to process all those tokens, adding latency to every response. Worse, as the context window fills with irrelevant code, you get context rot - the model’s attention degrades, and it becomes more likely to miss important details or make mistakes. Over an entire agent trajectory, these problems compound significantly.


## The Problem with Line-Based File Reading


Typical agents use start and end line numbers to read files. The tool interface looks like this:


```text
{
"tool"  :   "read_file"  ,
"parameters"  : {
"path"  :   "src/services/order_processor.py"  ,
"start_line"  :   100  ,
"end_line"  :   150
}
}
```


This approach has a fundamental flaw: it forces the LLM to make blind decisions about which parts of a file are relevant before seeing what those sections contain.


Let’s say you have a 3,000-line file` order_processor.py` and the agent needs to understand the` calculate_shipping_cost` function. The agent has two bad options:


1. **Read the entire file** - Costs 25,000+ tokens, most of which are irrelevant
2. **Guess line numbers** - Search for “calculate_shipping_cost” to find the start line - but the end line is unknown.


Here’s what this looks like in practice:


```text
# The agent finds the function starts at line 1247
def   calculate_shipping_cost  (order: Order, destination: Address) -> Decimal:
# But how many lines should it read?
# Too few: misses important logic
# Too many: wastes tokens on unrelated code
```


We found that coding agents either:


- Over-read which wastes tokens
- Under-read and miss critical context, requiring a second read (which wastes time and makes you pay for two requests to the underlying model)


## Agents aren’t trained to use extra tools


The initial solution we tried was giving the agent a separate tool (like` get_file_outline` ) that returns the structure without the full content. In theory, the agent could peek at the outline first, then read specific sections as needed. The sequence of tool calls would look something like:


```text
User: write tests for calculate_shipping_cost in order_processor.py


Assistant: I'll write tests for calculate_shipping_cost. Let me first examine the file to understand its structure and functionality.
{
"get_file_outline"  : {
"file_path"  :   "order_processor.py"
}
}
{
"read_file"  : {
"file_path"  :   "order_processor.py"  ,
"start_line"  :   100  ,
"end_line"  :   150
}
}
```


In practice, agents struggle to use tools like this as they’re trained to read files directly. The agent hasn’t been trained to internally make a “should I peek first?” decision, so it just calls` read_file` directly without using the outline tool.


We tried to fix this with elaborate prompting (instructing the agent to always check the outline first), but this approach doesn’t work well because it’s fighting the model’s instincts and adding latency to every file read, even for small files that don’t need it. Determining whether to use the outline tool is simple. If the number of tokens is over a certain threshold we can show the overview, otherwise allow the read file call to go through normally.


## Our Solution


The solution that actually works is invisible to the agent: wrap the existing` read_file` tool so that it directly returns an outline when the file is too large.


From the agent’s perspective, it can call` read_file` , and it gets back something useful. The wrapper handles the complexity:


1. If the file is small enough, return the full contents as normal
2. If it’s too large, return a structural outline with line numbers. The agent can then request specific line ranges to dig deeper.


This works with the model’s training rather than against it. The agent just reads files, and large files come back as navigable outlines.


## Finding the right preview format


We experimented with several formats to show the LLM. First we showed a given symbol outline like so:


\[


...


5


nested


items


,


read


lines


298


-


558


for


details


\]


Click or hover to highlight the tokens


This is a good start, but we found a more token efficient way to show this. The format below is 9 tokens while the original was 15 tokens (saving approximately 30% over the entire outline):


(


5


children


)


\[


298


:


558


\]


Click or hover to highlight the tokens


This symbol outline contains classes, functions, and properties with their visibility modifiers and line ranges. This gives the agent exactly what it needs to navigate: the shape of the code and where to look for details.


The full format looks like this:


```text
# File Structure Outline
[public] class MyClass [1:150]
[private] fun initialize [10:25]
[public] property config [30]
[protected] fun processData (3 children) [40:95]
[public] fun getResults [100:145]
```


The agent can immediately see that` processData` is the meatiest function and request lines 40-95 if that’s where it needs to look.


Here’s another example using` editor.rs` from Zed. The entire file is ~207,000 tokens using the[OpenAI tokenizer](https://platform.openai.com/tokenizer) , while our outline is only 3,694 tokens (saving 98.2%):


File Outline (click to select)


📄 Full File (all 0 lines)


Full File (0 lines, 0 tokens)


File Preview (lines 1-28553, 0 tokens)


## Adaptive depth for very large files


We can optimize the outline generation even further. Instead of showing the full outline, we can cap the depth adaptively based on file size. We start with a greedy approach and only reduce depth when necessary:


1. Generate the full outline with unlimited depth
2. Check the size. If it’s under 10,000 tokens, we’re done. If not, regenerate with depth capped at 10
3. Keep reducing depth by 1 until it fits, stopping at depth 1 (top-level symbols only)


```text
// Depth unlimited - everything shown
[public] class OrderProcessor [1:800]
[private] class ValidationContext [50:150]
[private] fun validateItem [60:85]
[private] fun validateQuantity [90:120]
[public] property errors [125]
[public] fun processOrder [200:400]
[private] fun applyDiscounts [250:300]
[private] fun calculateTax [305:350]
...


// Depth 2 - nested classes collapsed
[public] class OrderProcessor [1:800]
[private] class ValidationContext (3 children) [50:150]
[public] fun processOrder (2 children) [200:400]
...


// Depth 1 - only top-level symbols
[public] class OrderProcessor (12 children) [1:800]
[private] fun helperFunction [850:900]
...
```


The` (N children)` markers are important. They tell the agent there’s structure it isn’t seeing. Combined with line ranges, the agent can decide whether to request` read_file` on lines 50-150 to explore` ValidationContext` , or whether the top-level view is enough for its current task.


This approach means small-to-medium files get full structural detail, while massive files still return something useful instead of truncating or erroring out.


## The results


After implementing this approach in Sweep Agent, we saw:


- **90% reduction in token usage** for files larger than 2,000 lines
- **Faster response times** - Fewer tokens means faster generation and lower latency
- **Reduced context rot** - Smaller, more focused context windows keep the agent’s attention sharp
- **Less compaction and truncation** needed for large files


Our agent now navigates large files efficiently without any special training. By using the outline, it’s able to be much more surgical in its file reads, identifying the relevant sections and requesting only what it needs. Small files still return in full, while large files are now much more manageable.


If you’re using JetBrains IDEs, you can[download our plugin](https://plugins.jetbrains.com/plugin/26860-sweep-ai) to try this out.
