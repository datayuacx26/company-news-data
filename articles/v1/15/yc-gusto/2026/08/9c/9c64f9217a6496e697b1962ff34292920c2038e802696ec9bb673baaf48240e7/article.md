---
schema_version: "1.0.0"
document_id: "9c64f9217a6496e697b1962ff34292920c2038e802696ec9bb673baaf48240e7"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-engineering-rss"
canonical_url: "https://engineering.gusto.com/eval-driven-design-systems-part-2-5b989d492281"
published_at: "2026-08-03T18:02:56+00:00"
first_seen_at: "2026-08-03T20:17:48.268393+00:00"
fetched_at: "2026-08-03T20:38:58.255627+00:00"
content_hash: "sha256:af8a28516df3594b3771a8eb997ea6033c4a924ff92016862fbbba78f6787179"
---

# Eval-Driven Design Systems (Part 2)

# Eval-Driven Design Systems (Part 2)


[Jaywritescode](https://medium.com/@jaywritescode?source=post_page---byline--5b989d492281---------------------------------------)


5 min read


·


2 hours ago


--


Press enter or click to view image in full size


From plausible to production-ready: schema-grounded code replaces hallucinated props with verified, type-safe components.


In our[previous post](https://engineering.gusto.com/eval-driven-design-systems-8f781dc2dacb) , we treated Zod schemas as agent-readable contracts. But a schema is just a hypothesis; without tests, you’re shipping vibes. This post dives into how we operationalized these schemas, set up a testing suite on[Braintrust](https://braintrust.dev/) , and started scoring AI output against our design system.


We set up an experiment with 167 hand-written prompts across 25 components, plus a second dataset built from real engineer requests. Each prompt is a plain-English UI request (e.g., “a primary action button that confirms saving”). The model receives the component’s schema and its` .meta()` payload and returns JSX.


## Scoring every prop


We use three deterministic static analysis scorers:


- ` prop-validity` : Does every prop exist on the schema? It’s fractional, not pass/fail: an output with one invented prop gets partial credit. Unknown tags count as hallucinations.
- ` enum-validity` : For enum-typed props, is the value legal?` variant="large"` is a common failure mode that` prop-validity` misses because the prop exists, but the value is invalid.
- ` import-match` : Does the import statement exactly match the schema’s definition? This prevents broken builds from invalid import paths.


```text
export const propValidity: EvalScorer<EvalInput, string, string> = ({ input, output }) => {    const schema = readComponentSchema(input.schema);    const jsx = parseJsx(output);     let validCount = 0;    let totalCount = 0;     for (const element of jsx.elements) {      if (!isComponentTag(element.tagName)) continue;       if (element.tagName !== schema.componentName) {        totalCount += 1; // unknown component tag counts against the score        continue;      }       totalCount += 1;      validCount += 1; // using the right component at all is worth something            for (const attr of element.attributes) {        totalCount += 1;        if (schema.knownProps.has(attr.name) || isCommonHtmlAttr(attr.name)) {          validCount += 1;        }      }    }     return { name: 'prop_validity', score: totalCount === 0 ? 0 : validCount / totalCount };  };
```


We add a second layer of human ratings: **intent** (did the model solve the problem?) and **idiom** (is this how a fluent author would write it?). Mechanically perfect code can still be the wrong component; splitting these axes helps us identify specific failure modes. Every rating includes a rationale, which we use to seed our future LLM-as-judge scorers.


## From guesses to real requests


The 167 prompts above are good prompts. We wrote them carefully, covering every variation we could think of for each component. They’re also, structurally, a guess: we were guessing what builders actually ask for, then checking the model’s answer against our own guess.


We’re starting to close that loop. Every call our AI assistant makes into the design system’s tooling is instrumented to capture the engineer’s request verbatim, the literal text of the ask, not a summary of it. Calls that belong to the same request (one ask can trigger several tool calls) are grouped into a single unit, so we get one real-world example per request instead of a pile of disconnected calls.


A periodic job pulls a window of that production traffic, regroups it into requests, and lands each one as an eval row: the verbatim prompt, paired with whichever components the assistant actually reached for. There’s no hand-written “correct” JSX attached to these rows, and there doesn’t need to be one. prop-validity, enum-validity, and import-match don’t compare output to a golden answer; they check it against the schema. A real request can be scored the moment it’s captured.


That distinction matters more than it sounds like it should. The eval set can grow from usage instead of from our imagination, without anyone hand-labeling a row.


This part is still young. The real-request dataset runs as its own track today, separate from the long-running synthetic suite. Folding the two together, and using real requests to find the prompt phrasings our synthetic set never thought to write, is next.


## What we measured


The most useful way to talk about results is to show specific failures, not aggregate percentages.


Aggregate scores swing around based on model versions, dataset additions, and schema revisions, in ways that don’t always mean what they seem to mean. The failure categories don’t change. They’re the same now as they were six months ago, just less frequent.


## Get Jaywritescode’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Here’s a prop-validity failure:


```text
import { Button } from '@gusto/design-system';  - <Button variant="primary" intent="submit">Save changes</Button>  + <Button variant="primary">Save changes</Button>
```


intent doesn’t exist on the schema. prop-validity flags this below 1.0. A model that reads the schema doesn’t reach for the prop in the first place. We see this fix on every component with more than three props, across every model we’ve tried.


Now here’s a failure the deterministic scorers can’t catch. Prompt: “add a button to confirm and continue the payroll submission.”


```text
// What the model wrote  <Button variant="primary">Confirm and continue</Button>  // What a design-system-fluent author would write  <Button variant="primary">Continue</Button>
```


Both compile. Both score 100% on prop-validity, enum-validity, and import-match. The difference is in the content guidelines: The content guidelines say one verb per button, three words max. “Confirm and continue” is two verbs and three words. A static scorer doesn’t catch this. The human rating for idiom does. The rationale (“two verbs, the second is redundant given the surrounding flow”) becomes the seed for the LLM-as-judge scorer we’re building next.


The deterministic scorers caught the loud failures first. The soft failures are where the next layer of work lives.


## For your design system


The core idea: make your design system legible to AI, then measure whether AI is actually using it correctly. Treat both halves as production code.


Here’s the smallest version of this that works:


1. Pick your five most-used components.
2. Write a schema for each: prop types, three validated examples, the exact import statement. Add accessibility and content rules where they matter. Yes, the import statement too. Trust us on that one.
3. Expose them however your team’s AI tools read context: MCP, system prompts, structured docs in your assistant’s prompt template.
4. Write ten prompts per component. Plain English. The kinds of requests your engineers actually make.
5. Score with one deterministic check. prop-validity (does every prop in the output exist on the schema?) is the easiest place to start.
6. Run it whenever schemas change, or on a cadence. Read the failures, not just the aggregate.
7. Once you have real usage, mine it. Capture the literal request text wherever your AI tooling touches the design system, and feed it back into the same scorers.


Design-system teams have always kept patterns. Part of the job now is making those patterns machine-readable. It’s the same kind of work that made accessibility table stakes, just applied to a new audience. The audience writing your UI changed. The design system still owns the contract.


*Schemas tell models what’s possible. Evals tell you they listened.*


[Jay Johnson](https://medium.com/@jaywritescode) *works on the Builder Enablement team at Gusto. The team maintains the internal design system powering Gusto’s web products, including the schemas and evals described in this post.*


*If you maintain a design system and want to make it AI-legible, we’re hiring on Builder Enablement and platform teams:*[gusto.com/about/careers](https://gusto.com/about/careers)
