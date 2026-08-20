---
schema_version: "1.0.0"
document_id: "0e358be1895b0eba47056f5c487900cdd0633b976993a5c14a7ec9881a1168e8"
company_key: "yc-gecko-security"
company: "Gecko Security"
source_id: "yc-gecko-security-news-import-02715d60c1a6"
canonical_url: "https://www.gecko.security/blog/rce-in-your-test-suite-ai-agent-skills-bypass-skill-scanners"
published_at: "2026-03-11T00:00:00+00:00"
first_seen_at: "2026-07-21T21:22:16.875303+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:8598f12c98215cee3c46de309d38081f37315aa32c0f8bf63638e1255df20277"
---

# RCE in Your Test Suite: AI Agent Skills and the Attack Vector Skill Scanners Miss

Over the past few months, AI agent skills have become a standard part of developer tooling. Claude Code, Cursor, Codex CLI, and Gemini CLI all support the same basic concept: a` SKILL.md` file with YAML frontmatter and markdown instructions that tells the agent how to do something specific. Skills can be installed from public marketplaces like[ClawHub](https://clawhub.ai/) and[skills.sh](https://skills.sh/) , and shared across a team by committing them to the repo.


The install command looks like this:


bash


```text
npx skills add owner/repo-name


```


That command clones the skill repository and copies its contents into` .agents/skills/<skill-name>/` inside your project. Claude Code then gets a symlink at` .claude/skills/` , Cursor at` .cursor/skills/` , and so on across 37 supported agents.


A typical skill directory looks like:


```text
my-skill/
├── SKILL.md
├── scripts/
│   └── helper.sh
└── references/
└── docs.md


```


Security research to date has focused on what happens inside` SKILL.md` and the scripts that agents are instructed to run. Snyk's[ToxicSkills](https://snyk.io/blog/toxicskills-malicious-ai-agent-skills-clawhub/) study found 13.4% of skills on ClawHub had critical security issues, Cisco built an open-source scanner with LLM-based analysis, and VirusTotal added skills to their analysis pipeline. All of it looks at what the agent reads and what the agent is told to execute.


---


## How the installer actually works


The skill installer copies the entire skill directory verbatim into your repo, including any scripts, configs, or test files the author chose to bundle. The only exclusions are` .git` ,` metadata.json` , and files prefixed with` _` . Every other file lands on disk inside your repo, inside` .agents/skills/<skill-name>/` .


Now consider what your test runner does.


Both Jest and Vitest discover test files using recursive glob patterns. Jest's default is:


```text
**/__tests__ /**/  *. [jt]  s?(x)
**/?(*.)+(spec|test). [jt]  s?(x)


```


Vitest's default is:


bash


```text
**/*.{ test  ,spec}.?(c|m)[jt]s?(x)


```


These patterns match anywhere in the project tree, including inside` .agents/skills/` .


The critical piece is how these runners handle dot-prefixed directories. Most glob libraries default to` dot: false` , meaning` **/*.test.js` would skip any directory starting with` .` . That would accidentally protect you here.


But both Jest (v29+) and Vitest (v0.25.3+) override that default and pass` dot: true` to their underlying glob engines, based on source code analysis of both runners. Jest's maintainers explicitly added this after developer requests to support tests in hidden directories. Vitest fixed a regression in November 2022 ([PR #2359](https://github.com/vitest-dev/vitest/pull/2359) ) that was preventing test discovery in dot-directories and added a regression test to prevent it breaking again.


Neither runner excludes` .agents/` ,` .claude/` , or` .cursor/` from their discovery paths. Jest's` testPathIgnorePatterns` defaults to` \["/node_modules/"\]` . Vitest's` exclude` list covers` .git` ,` .idea` ,` .cache` ,` .output` , and` .temp` . That is it.


---


## The attack chain


Here is the full scenario from attacker to developer machine:


**Step 1.** An attacker publishes a skill to ClawHub called` github-pr-reviewer` . The` SKILL.md` is clean and descriptive. It passes every existing scanner because scanners look at` SKILL.md` content.


The skill also includes:


```text
github-pr-reviewer/
├── SKILL.md
└── tests/
└── reviewer.test.ts    < -- the payload


```


**Step 2.** A developer finds the skill on ClawHub. It has good documentation and no warnings from any scanner. They run:


bash


```text
npx skills add attacker/github-pr-reviewer


```


The installer copies everything into` .agents/skills/github-pr-reviewer/` . The file` tests/reviewer.test.ts` is now sitting in the repo.


**Step 3.** The developer runs their test suite. This might be` npm test` ,` npx vitest` , an IDE auto-run on save, or the CI pipeline running on push.


The test runner discovers` reviewer.test.ts` via its recursive glob, treats it as a first-class test file, and executes it.


**Step 4.** The payload runs with full local permissions: filesystem read access, all environment variables, shell.


---


## What the payload looks like


The file looks like a legitimate test. A` beforeAll` block runs before any assertions, silently, regardless of whether the actual test cases pass or fail.


Conceptually the structure is:


typescript


```text
// reviewer.test.ts


beforeAll  ( async   () => {
// Read process.env — contains GitHub tokens, NPM_TOKEN,
// AWS credentials, internal service keys in CI


// Read .env from working directory


// Read ~/.ssh/ private keys


// Read ~/.aws/credentials


// Read ~/.npmrc — often contains registry auth tokens


// POST everything to an attacker-controlled endpoint
// Fails silently to avoid any visible error output
});


// The test suite itself is real enough to pass review
describe  ( 'PRReviewService'  ,  () =>   {
it  ( 'initialises'  ,  () =>   {
expect  ( true  ). toBe  ( true  );
});
});


```


The` beforeAll` runs during test setup phase. Nothing in the test output indicates anything happened. In CI,` process.env` contains deployment tokens, secrets, and whatever cloud credentials the runner has access to.


---


## Why skill scanners do not catch this


Every current skill scanner operates with the same assumption: the threat lives in` SKILL.md` and in scripts that the agent is instructed to run.


These tools look for prompt injection patterns in markdown, shell commands embedded in skill instructions, suspicious network calls in bundled scripts, and data exfiltration in agent-invoked code. None of them flag` *.test.ts` files because test files are not part of the agent execution surface. They are not referenced in` SKILL.md` . The agent never touches them.


A scanner reviewing the skill for agent-side threats will see a clean` SKILL.md` and a test file and move on.


The skills ecosystem has been framed as an agent security problem. The actual problem is that skills are repo artifacts, and everything in the repo becomes part of the developer toolchain. The agent is not needed.


---


## The supply chain amplification


This gets worse when you consider that` .agents/skills/` is designed to be committed to version control.


The skills ecosystem explicitly expects you to commit skills to your repo so teammates can share them. GitHub's official` .gitignore` templates do not include` .agents/` . Once the malicious test file lands in the repo, it propagates to every developer who clones and runs tests, every CI pipeline on every branch, and every fork.


A single` npx skills add` from a malicious source creates a persistent compromise. The skill could be removed from ClawHub the next day. The file is already in your git history.


For an attacker, the optimal target is an open source project with an active contributor base. Any contributor who runs the test suite triggers the payload, and they have no reason to suspect it.


---


## Other surfaces worth noting


Test files are the highest risk because of the recursive glob with` dot: true` , but a few other auto-execution surfaces are worth being aware of:


**ESLint configs:** ESLint config files are JavaScript, not JSON. If a skill placed an` eslint.config.js` somewhere ESLint would discover it, it executes during linting. The current installer keeps files within the skill's own directory, which limits this, but the boundary is worth watching.


**conftest.py:** Python projects using pytest auto-execute any` conftest.py` during test collection. The same vector applies to Python repos if a skill bundles one.


Test files remain the most reliable vector because they require the fewest assumptions about project layout and execute in the most predictable context.


---


## Mitigations


**For developers using skills now:**


Add` .agents/` to your test runner's ignore list.


Jest (` jest.config.js` ):


javascript


```text
module  . exports   = {
testPathIgnorePatterns  : [ '/node_modules/'  ,  '/\\.agents/'  ],
};


```


Vitest (` vitest.config.ts` ):


typescript


```text
import   { defineConfig, configDefaults }  from    'vitest/config'  ;


export    default    defineConfig  ({
test  : {
exclude  : [...configDefaults. exclude  ,  '**/.agents/**'  ],
},
});


```


One line, add it now, whether or not you currently use skills.


**For the` npx skills add` installer:**


The installer should use a strict allowlist of permitted file types before copying to disk.` SKILL.md` , contents of` scripts/` ,` references/` , and` assets/` are reasonable inclusions. Files matching` *.test.*` ,` *.spec.*` ,` __tests__/` , and` *.config.*` should be excluded by default, with an explicit opt-in flag for anyone with a legitimate reason.


**For skill registries:**


ClawHub and skills.sh should flag skills that bundle test files or build configs. This is a trivial static check that no current scanner performs.


**For CI pipelines:**


Add the exclusion above directly to your CI config as a hardening step independent of whatever the project-level config says.


---


The skills ecosystem is repeating the early npm playbook, except without the decade of accumulated incidents that eventually forced package registries to take supply chain security seriously. The threat model everyone is building scanners around assumes the agent is the execution environment. It is not. The repo is, and everything that lands in it gets treated as trusted by the tools that already run there.
