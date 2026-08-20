---
schema_version: "1.0.0"
document_id: "ea9f1b25b275aac2e7e30955ef11e250abeb37f5edcfb3a3ccc2d105cba5fe10"
company_key: "yc-superglue"
company: "superglue"
source_id: "yc-superglue-news-import-11c072c5544e"
canonical_url: "https://superglue.ai/blog/ai-editing/"
published_at: "2026-01-22T00:00:00+00:00"
first_seen_at: "2026-07-22T15:16:40.020846+00:00"
fetched_at: "2026-07-28T22:23:15.131655+00:00"
content_hash: "sha256:3f0b3c778a2267ec9c58712f39b50e03d20e11283a6a35326531dacc0bc7f09d"
---

# JSON Editing with LLMs - Getting it right

At **superglue** , we integrate systems and allow you to build tools on top of these systems, their APIs, Databases, and more. Under the hood, every tool we run is defined by an internal JSON configuration.


To improve the creating as well as editing experience of these tools, we built an agentic editing experience. You can manually tweak API headers, or you can simply chat with the superglue agent: *"Update the body to include a timestamp."* , but also *"Check out the last error and fix the auth issue"* or *"Edit the final transform step and remove the emails from my return type"* .


It sounds straightforward, but figuring out how the agent actually applies those edits turned into a surprising engineering challenge. We went through three distinct architectural iterations before getting it right.


## Attempt 1: The "Rewrite Everything" Approach


Our first design was the simplest: Structured Output.


We fed the LLM the current JSON state and the user's request, and asked it to generate the **entire** new JSON object. We would then just replace the current tool with the new one.


Imagine a simplified version of our tool config:


Original Config


```text
{
"url": "www.api.test.com",
"body": "function(x) { return \"example\" }",
"header": {
"content-type": "application/json"
}
}
```


The API call failed because the content type is wrong and api.test.com expects this to be plain text.


The LLM would then output:


LLM Output


```text
{
"url": "www.api.test.com",
"body": "function(x) { return \"example\" }",
"header": {
"content-type": "text/plain"
}
}
```


It worked, technically. But for production use, it was flawed.


1. **Hallucination/Forgetfulness:** The LLM would often "forget" keys that it deemed unimportant to the current task, resulting in data loss.
2. **Drift:** It would occasionally rewrite parts of the config we didn't ask it to touch.
3. **Performance:** Regenerating a massive configuration object just to change one boolean value is inefficient.


## Attempt 2: The "Cursor" Approach (Search & Replace)


We looked around at the state of the art and took inspiration from AI code editors like Cursor. Their model is generally to treat the file as a text buffer. The LLM returns a` search_string` (to locate the context) and a` replace_string` .


We rebuilt our edit tool to follow this schema. We treated our JSON config as a flat text file.


The same example JSON as before and the same problem, the agent tool would output:


Agent Output


```text
{"search_string": "application/json", "replacement_string": "text/plain"}
```


We would then take this and perform a simple replace operation on the JSON object.


Performance improved immediately. Edits were granular. We weren't re-generating the whole world anymore; we were just swapping out specific blocks of text.


While this worked for simple key-value pairs, we started seeing a spike in errors where the agent couldn't locate the string it wanted to replace. The culprit: **stringified JavaScript** . Because we are an integration company, we store a lot of code snippets inside JSON strings.


```text
"body": "function(x) { return \"example\" }"
```


To an LLM, writing a search string for this is a nightmare of escaping. The LLM might output a search string that looks semantically correct, but if it misses a single escaped quote (` \\"` ) or a newline character (` \\n` ), the exact string match fails. We spent too much time trying to get the LLM to understand the exact whitespace of our serialized JSON.


## Attempt 3: JSON Patch (The Solution)


We realized we were trying to solve a structural problem with text-processing tools. We didn't need to search through a text file, instead, we needed to modify a data structure. Unlike Cursor, which has to handle Python, XML, and Markdown files, we can rely on the fact that we only ever make edits on JSON objects with a fixed structure.


With a narrower problem, searching for a specific solution led us to: **JSON Patch (RFC 6902)** .


If you aren't familiar, JSON Patch is a standard format for describing changes to a JSON document. It uses simple operations:` add` ,` remove` ,` replace` ,` move` , and` copy` .


Instead of asking the LLM to "find this text and write this text," we ask it to generate a patch operation. For example, when the user requests *"Change the content type to text/plain"* , the agent outputs:


JSON Patch


```text
[
{ "op": "replace", "path": "/header/content-type", "value": "text/plain" }
]
```


**Why this works for us:**


1. **No "Search" required:** We don't need to match string content. We only need the` path` .
2. **Escaping handled:** We bypass the stringification issue entirely. The LLM generates the *value* of the code, and the JSON Patch library handles the insertion.
3. **Determinism:** It is much harder for the LLM to hallucinate when it is constrained to specific paths.


## The Trade-off: Schema Dependency


No solution is perfect. The downside of JSON Patch is that the precision of the edit is tied to the granularity of your JSON structure.If you have a nested object, JSON Patch is surgical.But if you store a massive configuration as a single string value under one key, JSON Patch is blunt—you have to replace the whole string.


However, for our use case, with a rather granularly typed object structure, this works quite well and it forces us to continue having a good structure.


## The Result


The "Playground Agent" is now the most robust it has ever been. It handles complex tools, nested headers, and stringified code blocks without breaking syntax or losing data.


Here is a look at the final UI. You can see the agent proposing a fix, which we render as a diff. The user can then` Accept` or` Reject` the specific patch.


Your browser does not support the video tag.


If you are interested in the implementation details or want to see how we set up the patch logic, check out our[open-source repository](https://github.com/superglue-ai/superglue) .
