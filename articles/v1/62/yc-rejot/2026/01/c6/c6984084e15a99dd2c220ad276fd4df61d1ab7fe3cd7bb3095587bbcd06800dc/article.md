---
schema_version: "1.0.0"
document_id: "c6984084e15a99dd2c220ad276fd4df61d1ab7fe3cd7bb3095587bbcd06800dc"
company_key: "yc-rejot"
company: "ReJot"
source_id: "yc-rejot-news-import-01598ccac029"
canonical_url: "https://rejot.dev/blog/plain-text-knowledge-management/"
published_at: "2026-01-26T00:00:00+00:00"
first_seen_at: "2026-08-19T05:15:08.229837+00:00"
fetched_at: "2026-08-19T05:15:11.343066+00:00"
content_hash: "sha256:bb04fc72cce8c8d0c3419e34741c2f6b6c7336dbbcaad8a6b08cd78469b44d50"
---

# A knowledge management system inspired by plain-text accounting

A little while ago I had the displeasure of working with GNUCash, a desktop application for double-entry bookkeeping. I know it works for many people, but I never got the hang of it. I always felt like I was fighting the application rather than doing bookkeeping.


I’ve known about plain-text accounting for a while, but never got around to trying it out. I finally did, and it was a revelation. Especially with AI.


## Plain-text accounting refresher


> Plain text accounting is a way of doing bookkeeping and accounting with plain text files and scriptable, command-line-friendly software, such as Ledger, hledger, or Beancount.


Basically, it allows you to record transactions in a text file. A transaction looks something like this:


```text
2026-01-01 * "Some Payment" ^id-for-payment
description: "A payment I did because of reasons"
bill: "bills/relevant-bill.jpeg"
Expenses:Category:Account                          369.10 EUR
Liabilities:Accounts-Payable                      -369.10 EUR
```


AI makes it super easy to either edit transactions directly, or create scripts to process them.


## Agent feedback loop


I do a lot of agentic coding, so I know that one of the most important things is to have a good feedback loop. Beancount contains the` bean-check` utility that allows you to check the consistency of your transactions.


Other than that, Beancount doesn’t do that much. The beauty is its simplicity. It is very extensible via its scripting API. In the example above, “bill” is not something that is part of Beancount, but I decided to use it as a metadata field. A quick script to check if all bills are present improves the feedback loop.


## Vibe note-taking (has no feedback loop)


For a long time I’ve been trying to get more structured about my knowledge and note-taking. Lately, I’ve started using AI to help me with this. I simply open my agent harness of choice for that day and start speaking. Then I make the AI extract structured information from my notes.


To do this, I had a simple` AGENTS.md` file that described the approximate structure of the information I wanted.


The problem: **there is no feedback loop** .


One of the most important things in (personal) knowledge management is linking. Creating connections helps you think and find hidden patterns. Without validation, AI is not good at this. It likes to make stuff up.


## Codebases vs knowledge bases


When you think about it, knowledge bases and codebases have a lot in common. They are both just folders of text files with relationships between them.


Both compound when the structure is good; both decay when it is not.


So, let’s create the unit-test for knowledge1 .


## Thalo: a “programming” language for knowledge


Entertaining these ideas, I ended up building a “programming” language for knowledge. Or maybe it’s a compiler? Or a linter?


Anyway, the language has two main parts:


- **Entities** : define the types of knowledge you want to track
- **Entries** : create structured entries with metadata, links, tags, and content sections


In the example below, I’ve defined a simple “opinion” entity and an entry that uses it.


```text
2026-01-08T15:00Z create opinion "Plain text wins" ^plain-text #pkm
confidence: "high"


# Claim
Your notes should be plain text.


# Reasoning
- Plain text is portable
- AI works best with file systems
- grep > proprietary search


2026-01-08T15:00Z create opinion "Double-entry bookkeeping is a good idea" ^double-entry #pkm
confidence: "high"
related: ^plain-tekst


# Claim
Double-entry bookkeeping is a good idea. Especially using plain-text
accounting tools.
```


```text
2026-01-08T14:30Z define-entity opinion "Formed stances"
# Metadata
confidence: "high" | "medium" | "low"
related?: link[] ; "Related entries"


# Sections
Claim ; "Core opinion"
Reasoning? ; "Supporting points"
```


As you can see, I mistyped the related reference in the second entry. The checker catches this. Removing the typo makes this error go away. Removing metadata fields or changing them to the wrong type reveals other errors.


On the[rules page](https://thalo.rejot.dev/rules) you’ll find an overview of all checker rules.


## Why I think this is a good idea


Obviously, I think this is a good idea. Otherwise, I wouldn’t have built it. Unlike other knowledge management systems, Thalo is completely plain-text. This means you can use it with any editor, any version control system, and any AI. It’s also open source.


This means your data is really your data: completely portable2 . You can do what you want with it:


- Use AI, or don’t.
- Use a version control system, or live dangerously.
- Script it to your needs (there is a[simple, local API](https://thalo.rejot.dev/docs/scripting) ), or just use the built-in rules.


## How I use it (which might be different from you)


Thalo doesn’t really dictate how you use it, but I do want to highlight some ways I’m using it. The entities defined in my knowledge base are the following (and no, I will not elaborate on all of them):


```text
journal, opinion, reference, lore, goal, x-post, event-attendance, git-commit,
conversation, conversation-message, conversation-summary, telegram-webhook-update
```


I do want to highlight a couple of workflows that I’m using (in no particular order):


### Stream of consciousness dump


This is the workflow I discussed earlier. Basically I speak to the AI and I have it extract journal entries, opinions, facts, and insights from my stream of consciousness.


1. Stream of consciousness dump into a journal file
2. AI extracts structured facts and insights
3. Thalo validates the extracted entries against your schema


### Agentic search


This doesn’t need a whole lot of explanation. These days, agents are good at search. They use the file system and simple shell tools to find information.


I hooked my knowledge base up to a Telegram bot and an agent built with` pi`3 . It answers questions about me, my work, my projects, etc. It also allows me to create new entries from my phone.


### Knowledge extraction from existing content


Similar to the stream of consciousness dump, but for existing content. I input data as references. Could be anything I have created: my CV, blog posts, websites, etc. It helps me organize my existing work.


What I found to be a multiplier is letting the AI ask questions about the entries I’ve added. It helps me discover new connections and insights.


The corporate-looking figure below illustrates this workflow:


A continuous loop of knowledge refinement.


### Commit processing


This has been my personal favorite so far. I process git commits into a living record of work. I track what I built, learned, and struggled with.


I have a simple script to ingest git commits from other repositories on my machine. Since` ^links` are forced to be unique, I can use them as an idempotency key. Every evening, I run the script and ingest the commits.


My` git-commit` entity contains a` status` metadata field that can be` unprocessed` ,` processed` , or` skipped` . All commits start unprocessed, I then use an LLM to actually process them by reading the commit message and the full diff. It then extracts interesting information.


This snippet gives you an idea:


```text
2026-01-22T09:54Z create git-commit "feat(thalo): add incremental parsing and visitor-based checker" ^commit-0580776
hash: "0580776c926e27c2f6e88e88b805ce0b5b82a8f4"
short-hash: "0580776"
repository: ^repo-thalo
author: "Wilco Kruijer <wilcokruijer@gmail.com>"
authored-date: 2026-01-09
processing-status: "processed"
processed-date: 2026-01-22
files-changed: 57
insertions: 9388
deletions: 1413


# Summary
Major refactoring introducing incremental parsing infrastructure, visitor-based checker rules, and
pre-computed workspace index for efficient LSP updates.


# SE Concepts
- Incremental Parsing: Only re-parse changed portions of text
- Visitor Pattern: Rules implement enter/exit hooks for node types
- Workspace Index: Materialized view of cross-file data
- Invalidation: Targeted updates based on change impact


# Insights
- Pre-computing workspace-level data amortizes cost of cross-file checks
- Single-pass visitor with dependencies is more efficient than multiple traversals
- ~8x more insertions than deletions reflects new infrastructure scale


# Project Improvements
- Incremental parsing enables fast re-checking on edits
- Single-pass visitor execution reduces redundant tree traversals
- WorkspaceIndex pre-computes schemas, link definitions for cross-file checks
- LSP invalidation targets only affected documents


# Files Changed
- packages/thalo/src/model/document.ts: New Document class for incremental editing
- packages/thalo/src/model/line-index.ts: LineIndex for offset/position conversion
- packages/thalo/src/checker/visitor.ts: RuleVisitor pattern for single-pass checking
- packages/thalo/src/checker/workspace-index.ts: Pre-computed cross-file data
- packages/thalo/src/checker/rules/*.ts: All 27 rules migrated to visitor pattern
- packages/thalo-lsp/src/server.ts: Use InvalidationResult for targeted updates
- packages/thalo/src/semantic/analyzer.ts: Enhanced semantic analysis


# Original Message
feat(thalo): add incremental parsing and visitor-based checker


- Add Document and LineIndex classes for incremental text editing
- Introduce RuleVisitor pattern for single-pass rule execution
- Add WorkspaceIndex for pre-computed cross-file data
- Migrate all 27 rules to visitor pattern with dependencies
- Extract root-level parse errors (ERROR nodes) as diagnostics
- Update LSP server to use InvalidationResult for targeted updates
- Add comprehensive tests for new infrastructure


```


```text
2026-01-15T10:00Z define-entity git-commit "A processed git commit with extracted insights" ^git-commit
# Metadata
hash: string ; "Full 40-character commit hash"
short-hash: string ; "Short 7-character hash for display"
repository: link ; "Link to the git-repository reference"
author: string ; "Commit author name and email"
authored-date: datetime ; "When the commit was authored (use YYYY-MM-DD format)"
processing-status: "unprocessed" | "in-progress" | "processed" | "skipped" = "unprocessed"
processed-date?: datetime ; "When the commit was analyzed (use YYYY-MM-DD format)"
files-changed?: number ; "Number of files modified"
insertions?: number ; "Lines added"
deletions?: number ; "Lines removed"


# Sections
Original Message ; "The verbatim commit message"
Summary? ; "Clear 1-2 sentence summary of what this commit does"
Files Changed? ; "List of files modified with brief change descriptions"
Project Improvements? ; "What improvements this commit brings to the project"
SE Concepts? ; "Software engineering concepts demonstrated or applied"
Insights? ; "General lessons or patterns learned from analyzing this commit"
```


## Design decisions


Thalo is young and pre-1.0, so these design decisions may change over time. I wanted to highlight a couple design choices that might be interesting.


Based on feedback, these might change.


### Data is (im)mutable


Schrödinger’s mutability. One of my goals was to have data be **mutable** , since working in cooperation with AI is a lot easier when it can go back and change any data. On the other hand, another goal was to have full **change-tracking and provenance** . These things are in some way orthogonal.


I’ve chosen to go with a hybrid solution. My recommended way to use Thalo is inside a Git repository. This way, you can freely edit any entries, and we can use Git to track changes.


When you don’t use Git, you can use the timestamp-based checkpointing mode. To this end, we have a twin directive to` create` :` update` . This directive points at a pre-existing entry and overrides any content with the new content4 .


Both the scripting API and the` query` CLI command support a` since` option. This allows you to query entries that have changed since a given checkpoint (i.e. commit hash or timestamp).


### Simple query syntax


Thalo includes a small query language that allows you to query your data. It’s inspired by SQL, but it’s much simpler. It supports filtering by entity type, tags, links, and metadata.


Some examples:


```text
-- Querying tags directly
lore   where   #career


-- Querying links directly
lore   where   ^  self


-- Querying metadata (repository being of type link)
git  -commit   where   repository   =   ^repo  -  fragno


-- Combinations
lore   where   #career   and   type   =   "fact"
```


What I think might end up being a better choice is using the checkpoint system to idempotently ingest entries into a SQLite database. This would give the user a lot more flexibility and power, and us a lot less implementation work.


### Markdown interoperability


Thalo is designed to coexist with Markdown. You can embed Thalo code blocks inside Markdown files using fenced code blocks with the` thalo` language identifier.


The Thalo CLI, LSP, VSCode extension, and Prettier plugin all support this.


### Tooling


Any good programming language needs an ecosystem. Even a *“programming”* language.


Thalo was built using Tree-Sitter5 , and consists of a number of components:


- **CLI** :` thalo check` is the main way of working with Thalo.
- **LSP** : Language Server Protocol implementation. It provides features like go-to-definition, find references, and semantic highlighting.
- **Prettier** : Prettier plugin for` .thalo` files.
- **VSCode** : VSCode/Cursor extension for syntax highlighting, formatting, and language server features.
- **Scripting API** : Programmatic access to Thalo.


There’s also a couple of *WIP* components such as a merge driver for Git and a GitHub action. But these were vibe-coded and have not yet been tested. So really they don’t deserve to be named.


On the Thalo site you can also find a[slideshow demo](https://thalo.rejot.dev/demo) , the[playground](https://thalo.rejot.dev/playground) , and the[list of checker rules](https://thalo.rejot.dev/rules) .


## Quickstart and end


Thalo can be installed using your preferred NPM-compatible package manager:


```text
npm   install   -g   @rejot-dev/thalo-cli


# Initialize your knowledge base (creates basic entities.thalo and AGENTS.md files)
thalo   init


# Validate your entries
thalo   check
```


If you want to stay updated, you may do the following things:


- Follow me on X:[@WilcoKr](https://x.com/WilcoKr)
- Star the repo:[github.com/rejot-dev/thalo](https://github.com/rejot-dev/thalo)


And please let me know if there are any design decisions that you would’ve made differently. Thanks!


## Footnotes


1.


This section was very much inspired by this X post:[Ralph gave coding quality gates](https://x.com/arscontexta/status/2015437189115486354)↩


2.


[Malleable software](https://www.inkandswitch.com/essay/malleable-software/) : Restoring user agency in a world of locked-down apps↩


3.


[Pi](https://github.com/badlogic/pi-mono) is a framework for building (coding) agents that is very extensible.↩


4.


To be quite honest, there’s a decent chance we might end up removing the` update` directive.↩


5.


[Tree-Sitter](https://tree-sitter.github.io/tree-sitter/) is a parser generator tool and library for constructing parsers and lexers for programming languages.↩
