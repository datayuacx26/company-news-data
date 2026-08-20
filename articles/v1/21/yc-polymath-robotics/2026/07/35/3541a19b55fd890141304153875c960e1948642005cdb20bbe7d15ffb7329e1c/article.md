---
schema_version: "1.0.0"
document_id: "3541a19b55fd890141304153875c960e1948642005cdb20bbe7d15ffb7329e1c"
company_key: "yc-polymath-robotics"
company: "Polymath Robotics"
source_id: "yc-polymath-robotics-news-import-0eb697031937"
canonical_url: "https://www.polymathrobotics.com/blog/the-polymath-code-standard"
published_at: null
first_seen_at: "2026-07-24T03:33:00.269851+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:3458e4eeadb9c46bd455f88da2bb546751eebd86d6f108577fbc5bcdbbf0d239"
---

# Polymath Robotics Blog | The Polymath Code Standard: How We Maintain Consistent Style Across Every Repository

At Polymath Robotics, we write Python, C++, bash, YAML, CMake, Dockerfiles, and more — spread across dozens of repositories public and private.


Keeping all of that consistently formatted and free of basic syntax and logical errors is something we've invested in deliberately.


Our internal “Polymath Engineering Standard” covers a range of topics starting low level at the formatting and basic linting level, to architectural guidelines for program structure, naming, best practice usage patterns for commonly used frameworks, and aspirational direction for application of static analysis tools and runtime monitoring.


In this post I’ll walk through how we automate away the basic linting architecture so it’s easily used and updated everywhere we write code, freeing up time and energy for higher level concerns.


## Why?


Having a consistent linting standard for an organization has two major benefits:


1. Any developer jumping in to unfamiliar code encounters a consistent and familiar visual style, which enables understanding and focus on the logic and/or content.
2. Neither writers nor reviewers ever get distracted by formatting disagreements, corrections, or preventable basic usage errors


Putting together the definition of our standard, we adopted the following axioms to guide it:


### The specifics don’t matter, deciding does


The specific formatting choices you make matter far less than making *some* choices and enforcing them consistently. Whether you use 80 or 120-character line lengths, single or double quotes, tabs or spaces, these are all defensible. What's not defensible is having authors, code blocks, files, packages, and repositories each make these decisions independently, because that inconsistency has real costs.


The clearest benefit shows up in code review. When reviewers don't have to think about style violations or formatting preferences, they can focus on architecture and logic. And that is where human attention is actually valuable.


### A manual process won’t work


Automated tooling should handle all the mechanical stuff. Any formatting standard must be easily applied automatically because the friction introduced by massaging formatting by hand is a serious drain on productivity and motivation, pulling developers out of their flow.


There's also a practical safety implication. Static analysis and linters catch a whole class of real bugs: uninitialized variables, incorrect type usage, unreachable code, and more. When your test coverage isn't complete (and it rarely is), these tools are a baseline defense.


## Where we started


Early on, our repositories had no shared standard. Different engineers had different preferences, formatting was inconsistent, and basic errors that linters would have caught were showing up in review or worse, during expensive on-vehicle testing sessions.


The first steps were clear enough - establish a standard and configure tools to enforce it. We settled on a set of tools


- [Ruff](https://docs.astral.sh/ruff) for Python
- [clang-format](https://clang.llvm.org/docs/ClangFormat.html) and[cpplint](https://github.com/cpplint/cpplint) for C++
- [shellcheck](https://www.shellcheck.net/) for shell scripts
- [yamlfmt](https://github.com/google/yamlfmt) for YAML
- [hadolint](https://github.com/hadolint/hadolint) for Dockerfiles
- etc.


We decided on configurations for each and decided to run them via[pre-commit](https://pre-commit.com/) hooks.


### Pre-commit, briefly


` pre-commit` is a framework for managing git hooks.


You define checks in a` .pre-commit-config.yaml` at the root of your repository, and` pre-commit` runs them before each commit. Each hook references an external repository at a specific version, and pre-commit handles installation and running automatically.


It’s a super useful tool, and solves the base problem of zero-effort enforcement of the standards. Instead of having to manually run tools, configure editors, or be caught out at CI time after already pushing code for review, your code gets checked and fixed up every time you commit.


It’s relieved the background thread I always had going in my brain that would try to format code correctly the first time! Now I’ll just leave bad formatting such as unsorted imports and wonky spacing, knowing that the next commit will fix it, and leaving me in flow instead of worrying about getting some comments lined up just right.


### An issue of growing complexity


As our standard grew more complex, so did the complexity of the` pre-commit` setup. Here it is in all its glory.


```text
---
# Pre-commit config for Polymath Code Standard - https://github.com/polymathrobotics/polymath_code_standard
# Copied from origin - DO NOT EDIT IN CONSUMING REPOSITORY
repos:
# Generally useful checks provided by pre-commit
-     repo:     https://github.com/pre-commit/pre-commit-hooks
rev:     v5.0.0
hooks:
-     id:     check-added-large-files
-     id:     check-ast
-     id:     check-case-conflict
-     id:     check-merge-conflict
-     id:     check-shebang-scripts-are-executable
-     id:     check-symlinks
-     id:     check-toml
-     id:     check-xml
-     id:     end-of-file-fixer
-     id:     forbid-submodules
-     id:     mixed-line-ending
-     id:     trailing-whitespace
# JSON (This one supports comments, vs the one in pre-commit-hooks repo does not)
-     repo:     https://gitlab.com/bmares/check-json5
rev:     v1.0.0
hooks:
-     id:     check-json5
# Python
-     repo:     https://github.com/astral-sh/ruff-pre-commit
rev:     v0.11.5
hooks:
-     id:     ruff-format
-     id:     ruff
args:   [  --fix  ]
# C++ formatting
-     repo:     https://github.com/pre-commit/mirrors-clang-format
rev:     v19.1.7
hooks:
-     id:     clang-format
args:   [  "--style=file:.config/clang-format"  ]
# C++ linting
-     repo:     https://github.com/cpplint/cpplint
rev:     2.0  .0
hooks:
-     id:     cpplint
args:   [  "--config=.cpplint.cfg"  ,   --quiet  ,   --output=sed  ]
# Markdown
-     repo:     https://github.com/jackdewinter/pymarkdown
rev:     v0.9.28
hooks:
-     id:     pymarkdown
args:   [  -d  ,   MD013  ,   fix  ]
# XML
-     repo:     https://github.com/emersonknapp/ament_xmllint
rev:     v0.1
hooks:
-     id:     ament_xmllint
# YAML
-     repo:     https://github.com/adrienverge/yamllint.git
rev:     v1.29.0
hooks:
-     id:     yamllint
args:   [  -d  ,   "{extends: default, rules: {line-length: {max: 256}, commas: false}}"  ]
# CMake
-     repo:     https://github.com/cmake-lint/cmake-lint
rev:     1.4  .3
hooks:
-     id:     cmakelint
args:   [  --linelength=140  ]
# Bash / Shell scripts
-     repo:     https://github.com/shellcheck-py/shellcheck-py
rev:     v0.10.0.1
hooks:
-     id:     shellcheck
args:   [  -e  ,   SC1091  ]
# Docker/Container files
-     repo:     https://github.com/AleksaC/hadolint-py
rev:     v2.12.1b3
hooks:
-     id:     hadolint
args:   [
--ignore  ,   SC1091  ,
--ignore  ,   DL3006  ,
--ignore  ,   DL3008  ,
]
-     repo:     https://github.com/Lucas-C/pre-commit-hooks
rev:     v1.5.5
hooks:
-     id:     insert-license
types_or:   [  python  ,   cmake  ,   shell  ]
name:     Copyright     headers     for     Python/CMake
args:   [
--license-filepath  ,   .config/copyright.txt  ,
--comment-style  ,   '#'  ,
--allow-past-years  ,
--no-extra-eol  ,
]
-     id:     insert-license
types_or:   [  c++  ,   c  ]
name:     Copyright     headers     for     C/C++
args:   [
--license-filepath  ,   .config/copyright.txt  ,
--comment-style  ,   '//'  ,
--allow-past-years  ,
]
# Ansible
-     repo:     https://github.com/ansible/ansible-lint
rev:     v25.2.1
hooks:
-     id:     ansible-lint
always_run:     false
files:     ^ansible/
additional_dependencies:
-     ansible
```


‍


This came saddled with a bundle of configuration files as well, their existence vs. commandline arguments and directory location each according to the quirks of the particular checker


- ` .config/`


- ` ansible-lint.yml` (2 lines)
- ` clang-format` (70 lines)
- ` copyright.txt` (2 lines)


- ` .cpplint.cfg` (19 lines)
- ` .ruff.toml` (21 lines)


That’s over 200 lines of configuration spread across 6 files, copy-pasted into dozens of repositories.


And it worked pretty well for a while, too! Right until we wanted to make iterative improvements to the standard: change a rule, add a new check, upgrade a tool version. We could have invoked tooling to synchronize to target repositories, but that had a secret complexity hiding right below the surface. The inevitable manual edits leading to merge conflicts, cleaning up outdated config files, keeping a centralized list of repositories that needed the synchronization, and so on.


The standard was only as consistent as your discipline in propagating changes, and in practice that discipline eroded quickly.


## How we do it now


Our solution to this problem is[polymath_code_standard](https://github.com/polymathrobotics/polymath_code_standard) , a single repository that bundles all of our tool configurations and exposes them as their pre-commit hooks. Many of these hooks are thin wrappers around the hooks we were already using, but bundles them together with arguments and config files.


So instead of pulling in ten different tool repos (and having pre-commit install ten different python virtual environments), consumer repos pull in just the one and it handles everything.


Here's what a representative repository's` .pre-commit-config.yaml` looks like now:


```text
# Apply Polymath Code Standard formatters and linters
repos:
-     repo:     git@github.com:polymathrobotics/polymath_code_standard.git
rev:     v2.0.0
hooks:
-     id:     polymath-general
-     id:     polymath-python
-     id:     polymath-cpp
-     id:     polymath-shell
-     id:     polymath-cmake
-     id:     polymath-docker
-     id:     polymath-markdown
-     id:     polymath-xml
-     id:     polymath-yaml
exclude:     ^ansible/      # Ansible has its own linting standard
-     id:     polymath-toml
-     id:     polymath-json
exclude:     package-lock.json
-     id:     polymath-copyright
args:   [  --license  ,   proprietary  ,   --copyright-org  ,   'Polymath Robotics, Inc.'  ]
-     id:     polymath-ansible
files:     ^ansible/
```


‍


That's it! No tool configuration, no accompanying files. The version pin controls everything.


When we improve the standard — add a lint rule, upgrade a tool, tighten a formatter setting — repositories adopt it by bumping` rev` .


## A peek inside


The repository is a Python package with a CLI entry point (` polymath_code_standard` ), and each checker is a subcommand of that.


```text
-     id:     polymath-python
entry:     polymath_code_standard     python
types:   [  python  ]
```


Each group runs its tools in sequence via subprocess and aggregates the results.


Tool configuration files — ruff settings, clang-format style, yamllint rules, and so on — are bundled inside the package and used via` importlib.resources` .


### Per-language hooks


We split the standard into separate hooks by language and concern:` polymath-general` ,` polymath-python` ,` polymath-cpp` ,` polymath-shell` ,` polymath-cmake` ,` polymath-docker` ,` polymath-markdown` ,` polymath-xml` ,` polymath-yaml` ,` polymath-toml` ,` polymath-json` ,` polymath-copyright` ,` polymath-ansible` .


There are two reasons for this split. First, pre-commit groups output by hook ID — keeping them separate means Python failures don't get buried in a wall of C++ output.


Second, repositories only enable the hooks they actually need.


A pure-Python service doesn't need the C++ hooks; a repository with no Dockerfiles can skip` polymath-docker` .


This keeps the output focused and allows for exposing an intentionally limited set of knobs to tweak for customizing behavior as needed for a repository. For example, in this one you can see the config skipping the` ansible/` directory when running the YAML check, because Ansible’s jinja-templating syntax has slightly different requirements and` ansible-lint` enforces a slightly different formatting that is more appropriate to the domain.


## Usage


Most of these hooks don't just report violations, they fix them.` ruff format` ,` clang-format` , and the copyright header insertion tool all modify files in place.


The first time you run` pre-commit run --all-files` on a repository that's adopting the standard, you'll typically get a batch of auto-fixed files to review and commit. After that initial pass, violations are caught (and often fixed) at commit time before they ever reach a PR. This is huge for adoption. "You need to go fix 847 formatting violations" is a really hard sell, but "Run this command, review the diff, commit" is much easier.


## And there it is


There is nothing groundbreaking here: bundle your configs, expose them through a versioned interface, let consumers pin and upgrade. However, we have solved a usability problem that doesn’t appear to have a standard solution in the community. And now, the standard actually stays consistent because updating it is cheap enough that it happens.


You may want to adopt our standard for some languages as-is, we think we’ve made some solid choices! Or, you may find value in following the pattern to bundle up your engineering team’s standard for easy distribution and enforcement.


The package is public and permissively licensed, you can find it at[github.com/polymathrobotics/polymath_code_standard](https://github.com/polymathrobotics/polymath_code_standard) , where the README gives more usage information.
