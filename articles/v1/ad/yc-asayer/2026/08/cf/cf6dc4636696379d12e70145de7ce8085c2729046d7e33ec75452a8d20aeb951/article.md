---
schema_version: "1.0.0"
document_id: "cf6dc4636696379d12e70145de7ce8085c2729046d7e33ec75452a8d20aeb951"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/text-expanders-git-prs-ai-prompts/"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T15:07:39.607497+00:00"
fetched_at: "2026-08-03T16:10:47.640296+00:00"
content_hash: "sha256:d10d8f1af6dcc36dd0da531df9ebebce6589250a8b6a9bf066e8fb9939f08e11"
---

# Using Text Expanders for Git, PRs, and AI Prompts

A text expander turns a short trigger like` :cm` into a full block of text anywhere you type — commit messages, PR descriptions, LLM prompts — so you stop retyping the same boilerplate dozens of times a day. The dev-friendly pick is[Espanso](https://espanso.org/) , a free, open-source, cross-platform expander written in Rust whose entire config is plain YAML you can version in git. This article is a copy-pasteable cookbook: real Espanso snippets for git incantations, PR skeletons, and reusable AI prompts, plus the three dynamic levers — shell output, clipboard injection, and cursor placement — that turn static text into context-aware tooling.


## Key Takeaways


- Espanso is a free, open-source, cross-platform text expander written in Rust; install it on macOS with` brew install --cask espanso` (macOS 11+), on Linux via the` .deb` package or AppImage, and on Windows from the official installer.
- To make a commit snippet context-aware, inject the current branch with a shell variable running` git rev-parse --abbrev-ref HEAD` , so a trigger always expands with the branch you are actually on.
- Espanso positions the cursor after expansion with the` $|$` hint, but you may define only one hint per match, and it can misbehave in auto-indenting editors — so keep PR skeletons in the browser description box.
- Espanso has no native “selected text” variable: copy the code first, then trigger a snippet that reads` {{clipboard}}` to wrap a highlighted function in an LLM prompt.
- Because the config is plain YAML, you can version the whole directory in a git repo and share a team’s snippets as a private Espanso package.


## Why a text expander belongs in your dev workflow


A text expander is any tool that watches your keystrokes and replaces a defined trigger with a longer, optionally dynamic block of text, system-wide. If your job involves retyping the same templated text over and over — conventional commits, PR descriptions, recurring prompts — it earns its keep immediately. Most people first meet expanders through email signatures; the payoff for developers is much larger because so much of the terminal-and-GitHub loop is boilerplate.


Three tools dominate:


Tool Platforms Strength


[Espanso](https://espanso.org/) Cross-platform, open-source Plain-YAML config, scriptable through shell and dynamic variables — the pick here


[Raycast Snippets](https://www.raycast.com/) macOS-centric Launcher feature with dynamic placeholders


[TextExpander](https://textexpander.com/) Cross-platform (paid) Team-oriented, shared fill-in groups


Pick Espanso when you want a scriptable, git-versionable config that travels across machines.


Install it and register the background service:


```text
# macOS (macOS 11+)
brew   install   --cask   espanso
espanso   service   register
espanso   start
```


On Linux,[the official install docs](https://espanso.org/docs/install/linux/) recommend the` .deb` package (` wget` it, then` sudo apt install ./espanso-debian-x11-amd64.deb` ) or the AppImage, followed by` espanso service register` and` espanso start` ; Wayland support is experimental. Windows uses the installer from espanso.org. Edit snippets any time with` espanso edit` and reload with` espanso restart` .


## Git snippets: expand the incantations you retype


Store git triggers in a match file and let` :cm` scaffold a[Conventional Commits](https://www.conventionalcommits.org/) message or` :undo` expand a command you never remember. Here is a static starter set:


```text
# ~/.config/espanso/match/git.yml
matches  :
-   trigger  :   "  :cm  "
replace  :   "  feat($|$):   "
-   trigger  :   "  :undo  "
replace  :   "  git reset --soft HEAD~1  "
-   trigger  :   "  :unstage  "
replace  :   "  git restore --staged .  "
-   trigger  :   "  :wip  "
replace  :   "  chore: wip [skip ci]  "
```


The` $|$` in` :cm` drops the cursor between the parentheses so you type the scope and keep going.


Now make a snippet aware of context. Espanso’s[shell extension](https://espanso.org/docs/matches/extensions/) runs a command and inserts its output, so you can pull the current branch into the text:


```text
-   trigger  :   "  :br  "
replace  :   "  {{branch}}  "
vars  :
-   name  :   branch
type  :   shell
params  :
cmd  :   "  git rev-parse --abbrev-ref HEAD  "
```


Type` :br` in a commit body or PR comment and it expands to whatever branch` HEAD` points at — no copy-paste, no stale branch name.


## PR templates: a fill-in-the-blanks skeleton


A` :pr` trigger can expand to a full Summary / Changes / Testing / Screenshots skeleton with the cursor dropped into the first field via` $|$` , turning a blank PR box into a form:


```text
-   trigger  :   "  :pr  "
replace  :   |
## Summary
$|$


## Changes
-


## Testing
-


## Screenshots
```


Espanso[supports the $|$ cursor hint](https://espanso.org/docs/matches/basics/) , but you may define only one per match — additional hints are ignored — and the docs warn it can misbehave in multiline expansions because Espanso simulates left-arrow key-presses, which fights auto-indenting code editors. Trigger` :pr` in the GitHub description textarea, not your IDE, and it behaves predictably.


## AI prompt snippets: reusable LLM scaffolds


Treat prompts like commit templates: define` :review` ,` :explain` , and` :tests` once and stop rewriting them. The static versions are just text:


```text
# ~/.config/espanso/match/ai.yml
matches  :
-   trigger  :   "  :explain  "
replace  :   "  Explain the following code line by line, then note any bugs or edge cases:  \n\n  "
-   trigger  :   "  :tests  "
replace  :   "  Write unit tests for the following code. Cover happy path and edge cases:  \n\n  "
```


The differentiating move is injecting code you just copied. Espanso has no native “selected text” variable, so the pattern is copy-first, then trigger a snippet that reads` {{clipboard}}` via the clipboard extension:


```text
-   trigger  :   "  :review  "
replace  :   "  Review this code for bugs, security, and readability:  \n\n  ```  \n  {{clipboard}}  \n  ```  "
vars  :
-   name  :   clipboard
type  :   clipboard
```


Select a function, press copy, type` :review` , and you get a fenced code-review prompt ready to paste into any model. Power users can simulate the copy inside a shell var —[the community workaround](https://github.com/espanso/espanso/discussions/1885) runs` xdotool key --clearmodifiers ctrl+c` before reading the clipboard — but the two-keystroke copy-then-trigger flow is simpler and portable.


## How Espanso’s dynamic variables work


A dynamic snippet reads its value from an extension —` type: date` for timestamps,` type: shell` for command output, and` type: clipboard` for` {{clipboard}}` — and Espanso evaluates variables top-to-bottom, so later variables can consume earlier ones. Per the[variables docs](https://espanso.org/docs/matches/variables/) , the shell and script extensions receive the current scope as environment variables, which is why a shell var can read a clipboard var defined above it.


That ordering unlocks composition. A date field for a changelog entry:


```text
-   trigger  :   "  :today  "
replace  :   "  {{today}}  "
vars  :
-   name  :   today
type  :   date
params  :
format  :   "  %Y-%m-%d  "
```


Espanso 2.3.0[added a tz parameter](https://github.com/espanso/espanso/releases/tag/v2.3.0) to the date extension for generating times in any IANA timezone. Combine shell, clipboard, and date vars in one match and you have a genuine micro-tool: pipe clipboard content through a formatter, stamp it, and drop the cursor where you continue typing.


## Version your config in git and share it


Because Espanso config is plain YAML — run` espanso path` to find the directory (on Linux,` ~/.config/espanso` ) — you can` git init` the whole folder, push it, and clone it onto every machine you work on. Your snippets become a versioned, reviewable artifact instead of a per-laptop accident.


For teams, publish shared commit and PR snippets as a package. Espanso[installs packages from any git repository](https://espanso.org/docs/packages/external-packages/) with the` --git` option, fetching straight from your host:


```text
espanso   install   team-snippets   --git   https://github.com/your-org/espanso-snippets   --external
```


Everyone gets the same` :cm` ,` :pr` , and` :review` conventions with one command. TextExpander offers a comparable capability through shared groups; Espanso does it with the git tooling you already run.


Start with three files —` git.yml` ,` ai.yml` , and a PR skeleton — commit them, and add a snippet every time you catch yourself retyping something. The config grows with your habits, and the copy-then-` :trigger` clipboard pattern is the one move that turns a static expander into a context-aware part of your git and AI workflow.


## FAQs


How is Espanso different from Raycast Snippets and TextExpander?


Espanso is free, open-source, cross-platform, and configured in plain YAML you can version in git and script through shell and dynamic variables. Raycast Snippets is a macOS-centric launcher feature with dynamic placeholders. TextExpander is a paid, cross-platform product oriented toward teams and shared fill-in groups. Choose Espanso when you want a scriptable, git-versionable config that travels across every machine you work on.


Can I place the cursor in more than one spot after a snippet expands?


No. Espanso allows only one cursor hint per match; any additional $|$ hints in the same replacement are ignored. The docs also warn the hint can misbehave in multiline expansions because Espanso simulates left-arrow key-presses, which fights auto-indenting code editors. For PR skeletons, trigger the snippet in the browser description textarea rather than your IDE so the single cursor lands predictably.


Does the shell extension run inside my project directory so git commands work?


Yes. Espanso's shell extension runs the command in your current working context, so type: shell with cmd running git rev-parse --abbrev-ref HEAD returns the branch of whatever repository you are typing in. The extension inserts the command's standard output into the expansion, letting a single trigger like :br resolve to the live branch name with no copy-paste and no stale value.


Where does Espanso store its config so I can version it in git?


Run espanso path to print the config directory rather than hardcoding a location, since it differs by operating system. On Linux it is typically ~/.config/espanso, while macOS uses an Application Support path. The directory holds plain YAML match files, so you can git init the whole folder, push it, and clone it onto other machines. Edit files with espanso edit and reload with espanso restart.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
