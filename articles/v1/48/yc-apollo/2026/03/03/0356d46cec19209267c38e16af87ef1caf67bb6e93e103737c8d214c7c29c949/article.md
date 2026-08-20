---
schema_version: "1.0.0"
document_id: "0356d46cec19209267c38e16af87ef1caf67bb6e93e103737c8d214c7c29c949"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-we-taught-an-ai-to-nitpick-docs"
published_at: "2026-03-16T12:33:22+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:b0247ddd452bd78c92e60cf7e4eead8206a2367adbc5b3d4ac1907ce098937a7"
---

# How We Taught an AI to Nitpick Docs

Over the last couple years, I’ve iterated on what we at Apollo call the “AI Librarian.” It’s a suite of AI tools that sits on top of our documentation platform. From among those tools, the one I’m most proud of (at least for now) is the AI Style Guide Review. This tool intelligently and contextually applies our style guide, then brings the results right to the authors in GitHub.


Let me tell you: I got a lot wrong at first. In this post, I explain the three things I did to finally get it right:


- Restructured the style guide so AI could make sense of it
- Wired the review into our existing workflow
- Made it fast enough and cheap enough to run on every commit


But before I dive into the nitty-gritty details, let’s talk about the problem.


## Scaling style


The problem we had was all too common for documentation teams in the software-development industry: Apollo’s docs were written by dozens of engineers across many teams, and although we had a style guide with clear rules about voice, tense, terminology, and formatting, enforcing the guidelines was almost impossible. Writers ended up reviewing changes with fast-approaching deadlines and, most often, had to spend their time fixing blatant typos instead of making substantive changes that actually made the writing good.


Some tools, like Vale, existed to help address that problem, but they were limited to traditional static analysis. They caught the patterns I told them to catch and nothing else. We needed something that could understand context, not simply match a regex. We wanted something that could ask, “Does this sentence read well?” and actually answer the question correctly. So I turned to AI.


## Starting up an AI editor


My first attempt turned out how you’d probably expect. I took our existing style guide (the one written for humans), concatenated it with a prompt that said “review this diff against these rules,” and pointed it at incoming doc changes. The output was bad—not catastrophically bad, but chatty and full of false-positives that effectively made the output useless. The model flagged issues that weren’t violations while missing obvious ones. It hallucinated rules that didn’t exist, and it gave vague feedback like “consider rewording for clarity” without saying what was wrong or how to fix it.


What I soon came to realize was that the issue wasn’t with the AI, it was with the style guide. The guide was written to *explain* rules to *people* who could infer patterns from explanations. AI can’t do that—at least, not in the same way. When you tell a human “avoid passive voice,” they apply years of linguistic knowledge. When you tell a model the same thing, it pattern-matches inconsistently.


We had to change the guide to match the way the AI thinks, so we rewrote the style guide as a pattern library. Every rule became a set of concrete do-don’t pairs, with logic described explicitly:


```text
### Verb Tense and Voice


*   **Present Tense:** Favor the present tense.
Do this: "The client then sends a request to the server."
Don't do this: "The client will then send a request to the server."
Reason: Future tense is longer and rarely provides more clarity.


*   **Active Voice:** Favor the active voice for clarity and brevity.
Do this: "Apollo Studio manages your graph."
Don't do this: "Your graph is managed by Apollo Studio."
Reason: Passive voice is less direct and longer.


*   **Passive Voice Exceptions:** Passive voice is acceptable to
de-emphasize a subject or emphasize an object.
Do this: "Over 50 conflicts were found in the file."
Don't do this: "You created over 50 conflicts in the file."
Reason: Placing responsibility on the reader might be discouraging.
```


We applied the pattern across the entire guide: voice, headings, product names, changesets. Here’s an example of how we encoded the opinionated Apollo voice:


```text
### Voice


The Apollo voice is:
*   Approachable
*   Positive
*   Encouraging
*   Helpful
*   Opinionated/Authoritative


Opinionated voice prescribes a specific "happy path" to accomplish a goal.


Do this: "To achieve optimal performance, configure your server with X."
Don't do this: "You can configure your server with X, Y, or Z."
Reason: This is unopinionated and lays out options rather than prescribing a path.
```


And if you’re wondering—yes—I used AI to help generate the initial rewrite. But our docs team reviewed and edited every example. The difference in output quality was immediately obvious. Same model. Same prompt structure. Wildly better results.


If there’s one takeaway for me from this entire project, it’s this: **the way you encode knowledge for an AI can matter more than the model you choose.** A mediocre model with a well-structured prompt often outperforms a frontier model with a vague prompt.


## Getting AI into our workflow


A tool that produces good output but lives in the wrong place is a tool nobody uses. Our docs platform already had a custom deploy preview system, so I wired the style guide review into the same pipeline. Now, when a commit is pushed, the build forks: one path produces the deploy preview, and the other runs the style-guide analysis and surfaces results as GitHub status checks and annotations.


PullRequestReviewer orchestrates the whole flow. When a PR event comes in, the reviewer creates a GitHub check run, parses the diff to extract changed .mdx lines with their real line numbers, and hands the lines off to the review engine:


```text
const statusWriter = new StatusWriter(octokit, pr.head);
const check = await statusWriter.create("AI Style Review");


// Parse the diff to extract changed lines with real line numbers
const files: ChangeRequestInput = new Map();
for await (const file of pr.getFiles()) {
if (!filename.endsWith(".mdx")) continue;


for (const line of patch?.split("\n") ?? []) {
if (line.startsWith("@@")) {
// Parse hunk header: @@ -oldStart,oldLines +newStart,newLines @@
const hunkMatch = line.match(/@@ -\d+(?:,\d+)? \+(\d+)(?:,\d+)? @@/);
if (hunkMatch) realLineNumber = parseInt(hunkMatch[1], 10);
continue;
}
if (line.startsWith("+")) {
files.get(filename)?.push({
lineNumber: realLineNumber,
content: line.slice(1), // Strip the '+' prefix
});
}
}
}
```


Results land as annotations on the GitHub check run, with each suggestion mapped to a specific file and line:


```text
await statusWriter.complete(check, {
conclusion,
output: {
title: "Style Review Completed",
summary: `The pull request has ${changes.length} style issues.`,
annotations: changes.map((change) => ({
annotation_level: change.severity,
path: change.file,
start_line: change.lineNumber,
end_line: change.lineNumber,
message: `${change.reason}\n\n\`\`\`suggestion\n${change.suggestion}\n\`\`\``,
})),
},
});
```


### Why not PR reviews?


I initially tried using GitHub Pull Request reviews to deliver suggestions. On paper, it’s the perfect UX. Engineers get inline suggestions they can accept with one click. In practice, on large documentation changes with dozens of suggestions, the PR review becomes so noisy it visibly degrades the performance of the “Files changed” tab. GitHub’s UI just isn’t built for that volume of automated feedback.


Annotations solved the noise problem but created a new one: they didn’t support GitHub’s suggestion blocks. Engineers could see exactly what was wrong, but they had to manually implement the fix, when they should just have to select an **Accept** button. That’s a real UX downgrade, and there’s no way around it with what GitHub provides today.


## Building the dashboard


What I *could* control was the experience outside of GitHub. We already had an internal-only view of our docs site for other tooling, so I built a dashboard on top of it. Each review got its own page with comprehensive observability: cost breakdown, token usage, duration, and every suggestion grouped by file.


Each suggestion showed a visual diff between the original line and the proposed fix, color-coded by severity:


Now engineers can pick the suggestions they want, then choose between two paths. For local iteration, they generate a patch. It’s a curl | sh one-liner that fetches a server-generated script and applies it:


```text
const curlCommand = useMemo(() => {
const url = new URL(signedPatchBaseUrl);
const encoded = encodeSuggestionsUrlSafe(selectedIndices, suggestions.length);
url.searchParams.set("s", encoded);
return `curl -sL "${url}" | sh`;
}, [selectedIndices, signedPatchBaseUrl]);
```


The selection is encoded as a compact binary bitfield. Each suggestion is one bit, packed into bytes and base64-encoded for URL safety:


```text
/**
* Format:
* - 16-bit unsigned int (big-endian) for total suggestion count
* - 1 bit per suggestion index (1 = selected, 0 = not selected)
* - Zero-padded to the byte boundary
* - Base64url encoded
*
* Example: 12 suggestions with indices 1, 2, 4, 6, 8, 9 selected
* - Bytes: [0x00, 0x0C, 0b01101010, 0b11000000]
* - Base64: "AAxqwA"
*/
export function encodeSuggestions(
selectedIndices: number[],
totalCount: number
): string {
const buffer = new ArrayBuffer(2 + Math.ceil(totalCount / 8));
const view = new DataView(buffer);
const uint8 = new Uint8Array(buffer);


view.setUint16(0, totalCount, false); // Big-endian count header


const selectedSet = new Set(selectedIndices);
for (let i = 0; i < totalCount; i++) {
if (selectedSet.has(i)) {
const byteIndex = 2 + Math.floor(i / 8);
const bitIndex = 7 - (i % 8); // MSB first
uint8[byteIndex]! |= 1 << bitIndex;
}
}
return uint8ToBase64(uint8);
}
```


For faster iteration, there’s a **Commit to PR** button that applies selected suggestions as an atomic Git commit directly to the PR branch—no local checkout needed:


```text
const handleCommitToPr = async () => {
const response = await fetch(
`${BASE_URL}/internal-api/ai-review/${reviewId}/commit`,
{
method: "POST",
headers: { "Content-Type": "application/json" },
body: JSON.stringify({ suggestionIndices: indices }),
}
);
// On success: "Committed 5 suggestions! Commit SHA: a1b2c3d4"
};
```


Behind the scenes, SuggestionCommitter validates that the PR branch hasn’t moved (SHA check), fetches current file contents, applies the line replacements, and creates a single multi-file commit through the GitHub Git Data API:


```text
export class SuggestionCommitter {
async commit(options: SuggestionCommitOptions) {
// Step 1: Validate PR state (SHA hasn't changed)
const prValidation = await this.prClient.validateForCommit(
prNumber, expectedSha
);


// Step 2: Group changes by file
const changesByFile = this.groupSuggestionsByFile(suggestions);


// Step 3: Validate content still matches
const validation = await this.fileClient.validateContent(
currentHeadSha, contentValidations
);


// Step 4: Fetch files and apply replacements
const fileContents = await this.fileClient.getMultipleFiles(
filePaths, currentHeadSha
);


// Step 5: Create atomic multi-file commit
await this.gitCommitClient.createMultiFileCommit({
branch: branchRef,
parentSha: currentHeadSha,
files: updatedFiles,
message: commitMessage,
});
}
}
```


It’s not a one-click acceptance on GitHub, but it’s good enough for engineers to actually use it.


## Making it fast and cheap at the same time


Here’s where things got really interesting. Netlify background functions (and AWS Lambda more broadly) are limited to 15 minutes of execution time. For a typical commit touching a handful of files, the review finished in a couple of minutes. But documentation doesn’t always change in small increments. A major release might touch hundreds of files with thousands of changed lines. On those commits, we were hitting the 15-minute wall. And even when we didn’t time out, nobody wanted to wait that long for results.


Parallelism was the solution: split the work across multiple concurrent calls. But parallelism multiplied cost, and the style-guide prompt is big. Really big. Every parallel call needed the full style guide as input context, and input tokens aren’t free.


So I ran the numbers on our historical usage and three optimizations became clear:


### 1. Input caching


The style guide and system prompt are identical across every call. CacheManager hashes the style guide content, checks for an existing cache, and creates one if needed. Subsequent calls reference the cache by name instead of including the full text:


```text
export class CacheManager {
async getStyleGuideCache(model: string): Promise<string> {
const currentHash = this.getStyleGuideHash();


// If we already have a valid, unexpired cache, return it
if (this.cachedContentName && this.styleGuideHash === currentHash) {
const cache = await this.client.caches.get({
name: this.cachedContentName,
});
if (new Date(cache.expireTime) > new Date()) {
return cache.name;
}
}


// Create new cache with style guide as system instruction
return this.createStyleGuideCache(model, currentHash, displayName);
}


private async createStyleGuideCache(model, hash, displayName) {
const styleGuideText = fs.readFileSync(STYLE_GUIDE_PATH, "utf-8");


const systemInstruction = [
"You are an expert technical writer reviewing a documentation PR.",
"",
"Here is our style guide:",
"<style-guide>",
styleGuideText,
"</style-guide>",
"",
"If it looks like you are inside a code snippet, do not provide feedback.",
].join("\n");


const cache = await this.client.caches.create({
model,
config: { displayName, ttl: "3600s", systemInstruction },
});
return cache.name;
}
}
```


That alone cut costs dramatically for parallel workloads. You pay for the style guide tokens once, then dramatically reduce the per-call cost.


### 2. Line-level granularity


Instead of sending the model a full-file diff, I send it one line at a time, with a few surrounding lines for context. Each line gets a structured prompt, with section-heading detection and code-block awareness:


```text
const CONTEXT_LINES_BEFORE = 5;
const CONTEXT_LINES_AFTER = 5;
export const REVIEW_MODEL = "gemini-3-flash-preview";


// Lines are pre-filtered to skip trivial content
private shouldSkipLine(line: LineWithContext): SkipReason {
if (line.content === "" || line.content.trim() === "") return "empty";
if (line.isInsideCodeBlock) return "code-block";
if (/^import\s+/.test(line.content)) return "import-statement";
if (line.content === "---") return "frontmatter";
if (/^<!--.*-->$/.test(line.content.trim())) return "html-comment";
return null; // This line needs review
}


// Each line gets a focused prompt with context
private buildLineReviewPrompt(line: LineWithContext): string {
const parts = [`Review this single line from file: ${line.filename}`];


if (line.sectionHeading) {
parts.push(`Section: ${line.sectionHeading}`);
}


parts.push("Context before (do not review these):");
for (const ctx of line.contextBefore) {
parts.push(`  ${ctx.lineNumber}: ${ctx.content}`);
}


parts.push(">>> LINE TO REVIEW <<<");
parts.push(`  ${line.lineNumber}: ${line.content}`);
parts.push(">>> END LINE TO REVIEW <<<");


return parts.join("\n");
}
```


The review response uses structured output (a JSON schema that the model has to conform to), which constrains the output and makes it easier to parse:


```text
const response = await thread.generateJsonWithUsage(
g.object({
needsChange: g.boolean(),
suggestion: g.optional(
g.object({
newContent: g.string().description("The suggested new content"),
reason: g.string().description(
"Be succinct and direct. Do not reference 'the style guide'. " +
"To the reader, you ARE the style guide."
),
severity: g.enum("notice", "warning", "failure"),
})
),
})
);
```


Doing that made each individual call tiny—small enough that I could drop from Gemini 3 Pro to Gemini 3 Flash without losing quality, and further reducing cost.


### 3. Batched parallel execution


With cached prompts and a cheaper model, parallelism becomes affordable. Lines get batched and reviewed concurrently with a worker pool:


```text
const MAX_CONCURRENT_REVIEWS = 30;


// Review each line individually with concurrent execution
const lineResults = await this.runWithConcurrency(
linesWithContext,
MAX_CONCURRENT_REVIEWS,
(line) => this.reviewLine(line)
);


// Simple worker pool implementation
private async runWithConcurrency<T, R>(
items: T[],
limit: number,
task: (item: T) => Promise<R>
): Promise<R[]> {
const results: R[] = [];
let index = 0;


const workers = Array.from(
{ length: Math.min(limit, items.length) },
async () => {
while (index < items.length) {
const current = index++;
results[current] = await task(items[current]!);
}
}
);


await Promise.all(workers);
return results;
}
```


(A note of caution: most AI providers have dynamic rate limits, so handle 429s carefully—you will get them.)


The result: what used to take up to 15 minutes now finishes in under 60 seconds, even on our largest commits. Cost is calculated in real-time using live GCP pricing with sensible fallbacks:


```text
// Fallback pricing per million tokens (USD)
const FALLBACK_PRICING = {
"gemini-3-flash-preview": {
inputPricePerMillionTokens: 0.50,
outputPricePerMillionTokens: 3.00,
cachedInputPricePerMillionTokens: 0.05,
},
"gemini-3-pro-preview": {
inputPricePerMillionTokens: 2.00,
outputPricePerMillionTokens: 12.00,
cachedInputPricePerMillionTokens: 0.20,
},
};
```


At this point, the most expensive run has cost about ten cents. Most small changes have cost a fraction of one cent.


## Discovering an accuracy bonus


What I was most surprised by during this whole process was this: **Line-by-line review was significantly more accurate than bulk review.** When the AI analyzed an entire file, it seemed to lose focus. The AI would miss violations in one section while over-flagging in another. Narrowing the scope to a single line forced precision. False positives dropped. False negatives dropped. The quality improvement wasn’t marginal; it was a drastic improvement.


Every review also produces a comprehensive observability record: duration, token breakdown, cost, logs, and every suggestion stored for later analysis.


```text
export type AIReviewRecord = {
reviewId: string;
prNumber: number;
repo: string;
sha: string;
status: "running" | "completed" | "failed" | "timeout";
startedAt: string;
completedAt?: string;
duration?: number;
tokenUsage: TokenUsage;
cost: CostBreakdown;
linesReviewed: number;
changesFound: number;
filesReviewed: number;
model: string;
logs: LogEntry[];
suggestions?: StoredSuggestion[];
};
```


## If you’re building something like this


The implementation that worked for us wasn’t complicated: encode knowledge as examples, not explanations; surface results where engineers already work; and optimize for speed and cost at the same time. None of that is specific to documentation. I’m describing principles that apply to any situation in which you’re using AI to enforce standards at scale.


What’s more, the “non-AI” work mattered. I spent more time rewriting the style guide, picking the right GitHub integration surface, building a dashboard, and wiring up atomic commits through the Git Data API than I spent on anything related to the model itself. The work wasn’t glamorous, but it determined whether people changed their behavior based on what the AI told them.


The AI Librarian isn’t finished, but I built it relying on what is perhaps the least glamorous insight of all: the prompt can matter more than the model.


Written by


Daniel Abdelsamed


[Read more by Daniel Abdelsamed](https://www.apollographql.com/blog/author/daniel)
