---
schema_version: "1.0.0"
document_id: "2dbe9ebb3012ff1dcca52e3465c805b2b4054ebfff5ff7d6f00407474e899bf5"
company_key: "yc-readme"
company: "ReadMe"
source_id: "yc-readme-news-import-ecdc4511c006"
canonical_url: "https://readme.com/blog/cli"
published_at: null
first_seen_at: "2026-07-22T11:00:35.158559+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:0cb9cf43a697631c79218110b0b8ff19dab43d3ad3253361b186a5144509451f"
---

# The ReadMe CLI Tool

As we've transitioned ReadMe docs to being 100% backed by Git, we needed our own file- and folder-based format for docs. We have that format documented, but what was sorely needed was a way to make sure everything was in the right format before pushing, both locally and in your CI.


Plus, a lot of people are using Claude Code or Codex to write docs! Having a CLI tool means the AI can ask questions, understand the format, lint at the end, and make sure everything is ready to go live.


To use the CLI, just run` npx @readme/cli` .


## Lint your docs


Linting will run through all your docs and check that your frontmatter is valid and pages are where they're supposed to be.


The ReadMe CLI


v1.0.0


npx @readme/cli


~/projects/my-docs


$


npx @readme/cli lint


✗


2 errors and 19 warnings in 346 files


24 custom blocks · 18 custom pages · 151 docs · 10 recipes · 143 reference


docs/Getting Started/enterprise-guides.md


●


Invalid frontmatter


: "titl"


is not a valid property, did you mean "title"


?


docs/Getting Started/quickstart.md


●


Broken link


: "doc:initial-project-configuration"


does not match any page


docs/AI/AskAI.md


●


Unknown property


: "category"


is not a known frontmatter field (use x-category for custom metadata)


docs/Getting Started/_order.yaml


●


Missing from _order.yaml


: enterprise-guides


──────────────────────────────


Run npx @readme/cli lint --fix


to automatically fix some of these.


💡 Tip:


Set up GitHub Actions to lint your docs on every PR!


└ npx @readme/cli setup


It searches for:


- Broken links
- Duplicate slugs
- Empty pages
- Invalid YAML
- Invalid MDX-ish
- Broken recipes
- Content that won't be shown
- Unknown properties
- Misspelled properties
- Folder structure
- Sidebar nesting limits
- Badly formed slugs
- Missing _order.yaml
- Stale _order.yaml
- Missing MDX components


Don't worry about fixing it all yourself, though. You can run with` --fix` , and we'll do our best for anything deterministic. For things that are more complicated, you can route it through Claude, which can make sure things are valid.


## Keep OpenAPI specs in sync


There's also OpenAPI Spec files in each repo, which are linked to Markdown pages. Sometimes, as the OAS file changes, the docs get out of date. The linter can make sure things stay in sync, and pages are created for any new endpoints.


This runs as part of the linter, but it can also be accessed directly:


The ReadMe CLI


v1.0.0


npx @readme/cli


~/projects/my-docs


$


npx @readme/cli oas:sync


●


Developer Metrics API


(developer-metrics-api.json · 15 endpoints)


●


ReadMe API


(readme-api-v2-beta.json · 54 endpoints)


+


Added GET /v2/projects/{id}/insights


~


Updated POST /v2/auth/tokens (request body)


●


Legacy API


(readme-api.json · 36 endpoints)


~


Updated ReadMeConfig/my-requests.md (title, excerpt)


-


Deleted DELETE /v1/legacy/cache


✓


Synced: 1 added, 1 deleted, 2 updated.


## Lint on every PR


If you use a CI tool, you can easily make the linting run on every push. You can set it up manually, or we have helper scripts to set it up for the following tools:


GitHub


` npx @readme/cli setup:github`


GitHub (Blacksmith)


` npx @readme/cli setup:github --blacksmith`


GitLab


` npx @readme/cli setup:gitlab`


Bitbucket


` npx @readme/cli setup:bitbucket`


RWX


` npx @readme/cli setup:rwx`


CircleCI


` npx @readme/cli setup:circleci`


Once it's set up, the linter runs against your docs on every PR and posts the results as a comment. Anything deterministic can be autofixed by running the linter with` --fix` .


github-actions


Bot


commented on Feb 20


⋯


### ReadMe Docs Lint


File


Message


` my-requests.md`


**Error:** Duplicate slug "my-requests", also exists in reference/ReadMeConfig


` AskAI.md`


**Warning:** Unknown frontmatter property "category" (use x-category for custom metadata)


` LLMstxt.md`


` upgrading-to-rdme10.md`


**Warning:** Redirect page has body content: pages with a link URL won't display their content


` create-a-component.md`


**Warning:** Snippet out of range: "" references line 14 but "javascript" block has only 13 lines


💡


*Tip:* Run` npx @readme/cli lint --fix` locally to automatically fix some of these issues.


## Preview your docs locally


Currently in beta, you can run a server locally and see what your docs look like. It'll hot-reload, so you can see changes as they're happening. Over the next few weeks, as we get more stable, this will come out of beta.


## Working with AI


We built the CLI to work well with AI. When you run it inside Claude Code or Codex, it adapts its output for an agent rather than a human, which means you can use it for both drafting new pages from scratch and cleaning up linting errors across an existing repo.


The CLI has deep knowledge of the folder structure, docs formats and built-in MDX components. It can handle validation, OAS syncing, and structural rules.


Writing from scratch works best when you give Claude a rough outline and let` @readme/cli lint` enforce the house style as it goes. You can tell Claude what to write, or add existing unformatted docs into the repo and tell it to format them. Each page it writes gets linted on save, and Claude will iterate against the warnings until the file is clean. Because the CLI knows about ReadMe-specific things like custom blocks, frontmatter requirements, and link integrity, you don't need to babysit conventions you'd otherwise have to spell out.


Prompt


I want to document \[feature/endpoint\]. The source of truth is \[path to spec / code / brief\]. Draft new pages under the appropriate folder (docs/, reference/, etc.), then run` npx @readme/cli lint` and fix any errors or warnings until it passes clean. Match the tone and structure of existing pages in the same folder.


Fixing linting errors is even simpler. It's exactly the kind of work AI is great at. After you run` npx @readme/cli lint` yourself, you can let Claude fix the issues. The` --fix` flag handles the auto-fixable stuff, and everything else (broken links, missing frontmatter, malformed custom blocks, vague headings) that needs judgment can be handled by AI. Re-run lint after each batch of fixes so the agent can confirm progress and catch any regressions it introduced.


Prompt


Run` npx @readme/cli lint` and fix every error and warning it reports. Start with` npx @readme/cli lint --fix` to handle the auto-fixable ones, then work through the rest manually. Re-run lint after each file to verify the fix landed and didn't break anything else. Don't change content beyond what's needed to resolve each issue.


## The old rdme tool


We also have an older CLI called[rdme](https://docs.readme.com/main/docs/rdme) , from before our docs were backed by Git. It does things like upload files to ReadMe via our API, which has since been replaced by Git bidirectional sync. The old one is still available, though, for folks who've been using it.


## Try it out!


Run` npx @readme/cli help` any time to see everything the CLI can do:


The ReadMe CLI


v1.0.0


npx @readme/cli


~/projects/my-docs


$


npx @readme/cli help


Usage: npx @readme/cli \[options\] \[command\]


Options:


-v, --version


output the version number


--no-check


Skip ReadMe project validation checks


-h, --help


display help for command


Linting Commands:


lint


Lint and validate your ReadMe docs


setup:github


Set up GitHub Actions to lint your docs on every PR


OAS Tooling Commands:


oas:sync


Sync reference pages with OpenAPI specs


oas:validate


Validate OpenAPI spec files


Other Commands:


versions


List all doc versions and their branches


dev


Start a local dev server to preview your docs \[beta\]


play


Visit your pet


help


display help for command


💡 Tip:


your pet misses you! Run


npx @readme/cli play


to check in.


The CLI tool is all open source, so check it out on[GitHub](https://github.com/readmeio/cli) . Please[file an issue](https://github.com/readmeio/cli/issues) if you hit a bug or have a feature request.


Happy writing!


*P.S. I've also heard rumors that you can make a little friend if you run` npx @readme/cli play` .*
