---
schema_version: "1.0.0"
document_id: "bd11048bd5afde8c3c1093987dafa31a4a96f6b81d2705de576919611f0118ad"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/review-ai-generated-code-difftastic/"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T07:50:16.961097+00:00"
fetched_at: "2026-08-05T07:50:18.082714+00:00"
content_hash: "sha256:6d5649821b7c79090bec169eb50ee5abc1f4390aa7418709d2ec3757314b737b"
---

# Reviewing AI-Generated Code with Difftastic

AI coding tools routinely bundle a real logic change with wholesale reformatting in a single commit, and a line-based` git diff` drowns the one behavior-changing line in a screen of red and green.[Difftastic](https://difftastic.wilfred.me.uk/) is a structural diff tool that parses code into syntax trees and compares the trees instead of the text, so pure reformatting, re-wrapping, and whitespace churn register as *no change* — leaving only the edits that alter behavior. This article shows what difftastic is, how to wire it into git, and a concrete workflow for reviewing the commits an agent hands you.


## Key Takeaways


- Difftastic diffs the abstract syntax tree via[tree-sitter](https://tree-sitter.github.io/tree-sitter/) , so reformatting and line-wrapping show as no change while a single new argument still shows as a change.
- Difftastic supports[over 30 languages](https://github.com/Wilfred/difftastic) and falls back to a line-oriented, word-highlighted diff for unrecognized extensions or files with parse errors.
- Wire it in with` \[diff\] external = difft` in` ~/.gitconfig` , then read an agent’s commit with` git show <commit> --ext-diff` . The recommended git configuration changed in difftastic 0.61 (October 2024), so copy it fresh from the manual.
- Difftastic is an external diff tool, not a git pager, so pair it with[delta](https://github.com/dandavison/delta) for everyday viewing and reach for difftastic on reformatting-heavy AI diffs.
- ` difft --check-only --exit-code` gates a commit on the question “did this change behavior, or just reformatting?”


## The problem: AI diffs bury the line that matters


Agents and assistants like Cursor, GitHub Copilot, and Claude Code frequently reformat a whole function — or re-wrap a long call across several lines — in the same commit that changes one condition. A line-oriented diff has no way to tell the difference: it marks every touched line as changed. You get a wall of red and green, and the one edit that alters runtime behavior is somewhere inside it. Reviewing AI-generated code is exactly the situation where signal-to-noise matters most, because the reformatting is machine-generated churn and the behavior change is what you’re accountable for merging.


## What is difftastic?


Difftastic is a structural diff that compares files based on their syntax rather than their text. It parses each version with tree-sitter, builds an[abstract syntax tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree) , and diffs the trees. The practical consequence: if you reformat code so it now spans multiple lines, difftastic shows only what actually changed, and a moved or renamed block is reported as a move rather than a delete-plus-add.


Difftastic is written in Rust, created by Wilfred Hughes, and open source under the MIT license. It supports over 30 programming languages through tree-sitter parsers — not the “50+” some reviews claim. When a file has an unrecognized extension, it degrades gracefully to a line-oriented diff with word highlighting, so nothing breaks on languages it doesn’t parse.


## Install difftastic and wire it into git


Install the` difft` binary via a package manager or` cargo` , then register it as git’s external diff:


```text
# macOS
brew   install   difftastic


# Rust toolchain (needs Rust 1.85+ as of 0.69)
cargo   install   difftastic
```


For a one-off, pass the config inline so you don’t change anything permanently:


```text
git   -c   diff.external=difft   diff
git   -c   diff.external=difft   show   --ext-diff
```


To make difftastic your default, add this to` ~/.gitconfig` ,[straight from the manual](https://difftastic.wilfred.me.uk/git.html) :


```text
[  diff  ]
external   = difft
```


This changes` git diff` to use difftastic; other subcommands then only need the` --ext-diff` flag, e.g.` git show --ext-diff` and` git log -p --ext-diff` . The[recommended configuration changed in difftastic 0.61](https://github.com/Wilfred/difftastic/releases/tag/0.61.0) (October 2024), so if you’re copying an older blog snippet, replace it with the current manual version. Prefer inline output over the two-column default with` --display inline` :


```text
difft   old.js   new.js   --display   inline
```


## Why does difftastic win for AI-code review?


Structural diffing focuses on changed syntax nodes and eliminates the noise from non-essential edits. The clearest demonstration is the reformat-plus-one-argument case. Suppose an agent takes a one-line function signature and, because it now exceeds your line-length limit, a formatter wraps it across multiple lines — *and* the agent adds one argument:


```text
// before
function   createUser  (  name  ,   email  ) {}


// after
function   createUser  (
name  ,
email  ,
role  ,
) {}
```


A line diff highlights the entire block, because every line changed textually. Difftastic highlights only the new` role` argument — the single edit that changes behavior. That is precisely the kind of subtle addition an agent can slip past a reviewer who is skimming a noisy diff. The same logic applies to extractions: when an agent pulls a block into a new helper, difftastic flags the new function name as added instead of showing the wrapper and the body as one tangled delete-plus-add.


## A commit-review workflow for agent output


To read only the semantic changes an agent produced, target its exact commit:


```text
git   show   <  agent-commi  t  >   --ext-diff
# or review the whole branch
git   log   -p   --ext-diff   main..agent-branch
```


Scan for three things: additions and removals difftastic actually flags as changes; blocks reported as moves or renames rather than rewrites; and any hunk that reverts to a plain line diff — that’s the signal difftastic hit an unrecognized extension or a parse error. By default, difftastic falls back to a line-oriented diff whenever parse errors are encountered, a conservative choice so it never claims two syntactically different files are the same. That matters for AI output mid-refactor, which may not parse cleanly; raise the tolerance with` export DFT_PARSE_ERROR_LIMIT=20` if a nearly-valid file falls back unnecessarily.


For automation, difftastic can check whether two files share the same AST without rendering a diff:


```text
difft   --check-only   --exit-code   before.js   after.js
```


This sets the exit code to 0 when there are no syntactic changes and 1 when there are — a fast gate for a pre-merge hook or CI step that answers “did the agent change behavior, or just reformat?”


## Limits, and pairing difftastic with delta


Be honest about the trade-offs. Difftastic scales relatively poorly on files with a large number of changes and can use a lot of memory, so treat it as a review tool for specific commits, not a wholesale replacement for` git diff` . Its side-by-side display is usually good but can occasionally confuse. It’s an external diff tool, not a git pager, and it does not render inside the GitHub PR Files tab or most IDE diff panes — it’s a local CLI.


The practical answer is to run both. Keep a fast text pager for everyday viewing and switch to difftastic for the gnarly, reformatting-heavy diffs.


Tool Diff type Works as pager? Best for


difftastic Structural (AST) No (external diff only) Reformatting-heavy AI diffs


delta Text, syntax-highlighted Yes Everyday diffs


diff-so-fancy Text, prettified Yes Lightweight cleanup


Delta and[diff-so-fancy](https://github.com/so-fancy/diff-so-fancy) are text-based; neither does AST diffing. Use delta as your default pager, and reach for difftastic when the diff is drowning in formatting churn.


A clean structural diff is a pre-merge filter: it catches the behavior-changing line before you approve. The changes that still slip through — the subtle logic edit a noisy AI diff hid — are the ones you end up reconstructing after they ship, and watching a session replay of the broken interaction is one of the more direct ways to surface that class of regression. Configure` \[diff\] external = difft` today, then review your next agent commit with` git show <commit> --ext-diff` and read only what changed.


## FAQs


What is the difference between difftastic and delta?


Difftastic is a structural diff that parses code with tree-sitter and compares abstract syntax trees, so reformatting and line-wrapping register as no change. Delta is a text-based, syntax-highlighted pager that still diffs lines. Difftastic cannot act as a git pager and only works as an external diff tool, while delta works as a pager for everyday viewing. Use delta by default and difftastic for reformatting-heavy AI diffs.


What happens when difftastic encounters a file it cannot parse?


Difftastic falls back to a line-oriented diff with word highlighting in two cases: when a file has an unrecognized extension, and when it hits parse errors. The parse-error fallback is a deliberate, conservative choice so difftastic never claims that two syntactically different files are the same. This matters for AI output mid-refactor that may not parse cleanly. Raise the tolerance with the DFT_PARSE_ERROR_LIMIT environment variable when a nearly-valid file falls back unnecessarily.


Can difftastic show diffs inside the GitHub PR Files tab?


No. Difftastic is a local command-line tool that works as git's external diff tool, not as a rendering layer inside the GitHub PR Files tab or most IDE diff panes. You run it through commands like git show with the --ext-diff flag or git log -p --ext-diff. Third-party integrations exist for some tools like magit, but PR-tab and IDE support is limited, so difftastic is best treated as a terminal-based review step.


How do I check whether an AI commit changed behavior or just reformatted code?


Run difft with the --check-only and --exit-code flags on the two versions of a file. Difftastic checks whether the files share the same abstract syntax tree without rendering a diff, setting the exit code to 0 when there are no syntactic changes and 1 when there are. This is much faster than a full diff and makes a clean gate for a pre-merge hook or CI step that distinguishes real behavior changes from pure reformatting.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
