---
schema_version: "1.0.0"
document_id: "4c015b933b93a3b23e6f01b375d6bcd61f13f67ccde81784d3f4526b0b2f821c"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/critical-rce-in-serena/"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T00:39:58.153065+00:00"
fetched_at: "2026-08-18T00:40:01.438152+00:00"
content_hash: "sha256:1837ef4b9ae4c59514ed33847967abd3667996d72aa91153655d4ba7fd4a1c29"
---

# Critical remote code execution in Serena, a popular MCP coding agent

Serena, one of the most widely used AI coding agents, ran attacker-supplied code the moment a developer opened a project. GitLab's Threat Research Group found a critical server-side template injection ([GHSA-pp25-4cg4-qcr9](https://github.com/oraios/serena/security/advisories/GHSA-pp25-4cg4-qcr9) , CVE pending) that executes arbitrary code in the Serena process. Anyone on serena-agent 1.6.1 or earlier should update to 1.7.0 now.


A threat actor can exploit this by hiding a malicious` .serena/project.yml` file into a repository it controls and getting a developer to open it via the Serena Model Context Protocol (MCP) server. The flaw bypasses` trusted_project_path_patterns` , the control Serena built specifically to stop untrusted repositories from running code. We reported it privately on August 1, 2026, and the maintainers shipped a fix eight days later.


It is an early example of a risk class that will spread as MCP servers become embedded in the software development lifecycle.


## Why MCP servers are a different kind of attack surface


Most developer tools touch a codebase in a narrow way: A linter reads files, a formatter writes them, and a test runner executes a specific harness. Conversely, MCP servers hand a large language model (LLM) a general-purpose interface to the developer's environment, covering file system access, shell execution, language server queries, and sometimes the network. The same reach that makes them useful makes compromising the server process severe.


CI/CD pipelines run in isolated environments with scoped credentials. MCP servers run on the developer's own machine, in the developer's own user context, with access to everything the developer can reach: SSH keys, cloud provider credentials,` .env` files, browser sessions, and internal network resources. Compromise the server process and you compromise the developer's entire local environment.


Developers point these servers at repositories they did not write all the time: evaluating a new open-source library, triaging a bug report, or reviewing a contributor's branch. If the server processes anything from that repository before the developer inspects it, the attacker gets a window. Serena is explicit about this threat and ships a trust model to prevent untrusted repositories from executing code. The trust model had a gap.


## How the vulnerability works


### What Serena is


Serena describes itself as "the IDE for your agent." It connects Claude, Cursor, Copilot, and other AI assistants to a local codebase over MCP, providing semantic code navigation, refactoring, and editing tools that go beyond what a raw file system gives an LLM. Developers run it as a local MCP server and point their assistant at it:


```text
serena start-mcp-server --project /path/to/repo


```


Because Serena operates on arbitrary local repositories with the user's full OS privileges, it ships a trust gate.` trusted_project_path_patterns` controls which project paths count as trusted, and only trusted projects can use privileged features like` activation_command` , which runs a shell command on project open. The intended guarantee: opening an untrusted repository is safe.


### The sink: An unsandboxed template engine


Serena lets each project define custom modes, named configurations with a` prompt` field that gets injected into the LLM's system prompt when the project is active. Serena renders these prompts as Jinja2 templates, and the rendering is the problem. In` src/interprompt/jinja_template.py` :


```text
# line 22
self  ._env   =   jinja2.Environment()


```


` jinja2.Environment()` is a plain, unsandboxed environment. It exposes built-in globals whose attributes chain up through Python's object graph to` os` and` subprocess` . The techniques for reaching arbitrary code execution from that starting point are well documented and require no specialized knowledge. Jinja2 ships a` SandboxedEnvironment` for exactly this reason, but it was not used here.


### The delivery: Project configuration as untrusted input


A project's` .serena/project.yml` supports an` added_modes` field, listing additional modes to activate when the project opens. Serena's mode loader treats any name containing a path separator or ending in` .yml` as a filesystem path to load directly:


```text
# context_mode.py, lines 29-30
def   looks_like_yaml_path  (s:   str  ) ->   bool  :
return   os.sep   in   s   or   (os.altsep   and   os.altsep   in   s)   or   s.lower().endswith((  ".yml"  ,   ".yaml"  ))


# context_mode.py, lines 130-135
@  classmethod
def   load  (cls, name_or_path:   str   |   Path) -> Self:
if   isinstance  (name_or_path, Path)   or   looks_like_yaml_path(  str  (name_or_path)):
return   cls  .from_yaml(name_or_path)
...


```


A path-like entry in` added_modes` causes Serena to load that file from the repository, read its` prompt` field verbatim, and pass it to the unsandboxed renderer.` yaml.safe_load` is used correctly and blocks YAML deserialization gadgets, but it has no bearing on a plain string inside the` prompt` field. The YAML parser sees harmless text. The injection happens later, when that string is compiled as a Jinja2 template.


### The trust control bypass


` is_trusted()` , Serena's mechanism for preventing untrusted projects from executing code, gates two features:` activation_command` , the shell command run on project open, and` ls_specific_settings` , project-scoped tool overrides. The mode-loading and prompt-rendering path is never checked against` is_trusted()` . The` added_modes` list from a project's` .serena/project.yml` is processed without validation, and the mode loader applies no allowlist and no trust check before it loads the file and hands the` prompt` field to the template engine.


We confirmed the bypass empirically. We ran the same end-to-end test with` trusted_project_path_patterns` set to empty, so no project counts as trusted, stricter than any default configuration. Two things happened:


Feature Trust-gated Result on untrusted project


` activation_command` Yes Blocked


Template injection via` added_modes` No Executes


The feature whose entire purpose is to run a shell command is blocked. The template injection reaches the same outcome through a different path and runs freely. That makes it a protection mechanism failure (CWE-693). The trust model exists to prevent an untrusted repository from executing code, and this path defeats it while looking like ordinary project loading.


### The full call chain


```text
.serena/project.yml  (attacker-controlled, ships with the repo)
added_modes: ["./path/to/attacker-mode.yml"]
|
v
SerenaAgentMode.load()            context_mode.py:130-135
from_yaml(name_or_path)
prompt = <attacker-controlled string>
|
v
SerenaAgent._update_active_modes()    agent.py:1063
|
v
create_system_prompt()                agent.py:996
_format_prompt(mode.prompt)
JinjaTemplate(prompt).render()      arbitrary code execution


```


Code runs as a side effect of the ordinary project-open flow, before Serena serves its first request to the LLM.


### Exposure


Serena's reach in the developer ecosystem is substantial. The repository has 27.8k GitHub stars and 1.8k forks, and the` serena-agent` package records roughly 136,000 downloads per month on PyPI. It integrates with Claude Code, Cursor, VS Code, JetBrains IDEs, Claude Desktop, and OpenWebUI, covering the range of AI-assisted development environments in common use.


A realistic attack needs no infrastructure and no social engineering beyond the repository. An attacker publishes a library, a sample project, a CTF challenge, or a seemingly useful tool. A developer clones it, opens it with Serena, and the code runs. Given everything a developer's machine can reach, the payoff for the attacker is immediate. In a CI/CD setup where Serena processes submitted repositories automatically, no human interaction is required at all.


## Why new tools inherit old bugs


None of the patterns behind this bug are unique to Serena.


User-supplied data flowed into a template engine without sandboxing. Template injection is one of the oldest vulnerability classes in web security, and most template engines ship a sandbox or restricted mode to address it. In a young ecosystem, developers building configuration-driven templating features often reach for the plain variant by default. The pattern will show up in other MCP tools.


Project configuration files were treated as trusted input. A repository's configuration files are authored by the repository owner. In any threat model where the repository may be untrusted, those files are attacker-controlled input. The distinction is easy to overlook when you build a tool and the configuration files feel like part of the tool itself.


Trust gates covered some code paths but not others. Serena's trust model was designed correctly: The gate exists, it is implemented, and the features it covers are clearly identified. The gap was a code path added without being brought under the same gate. Keeping trust coverage complete as a codebase evolves takes deliberate review of every new path that processes project-supplied input, not only the paths that are obviously privileged.


## What this means for you


### If you use Serena


Update to serena-agent 1.7.0 now. The fix switches the template engine to` jinja2.sandbox.SandboxedEnvironment` , which closes the injection path. On Versions 1.6.1 and earlier, avoid opening repositories from untrusted sources.


### If you build MCP servers or AI coding tools


These practices should apply to any tool that reads a project directory for an LLM.


- Treat project configuration files as input from a possibly hostile repository owner. Validate them like an HTTP request body: check the structure and reject anything off-schema.
- Map every place a string becomes code or a command: template rendering, subprocess calls, plugin loading, dynamic imports. For each, know whether project input can reach it and what stops it.
- Make your trust gate cover every path, not one feature. Review it whenever you add a feature that reads project input.
- Test against a hostile repository before you ship. Build the most aggressive config you can, confirm nothing runs on open, and automate it.


### If you lead a security team


MCP servers are a new class of local attack surface running with your developers' full user context, so treat them like any other privileged tooling on those machines. Start by finding out which MCP servers are running in your environment and where. At scale, this may call for tooling and policy. From there, apply the same scrutiny to MCP server updates that you apply elsewhere: a vulnerability in an MCP server is a vulnerability on every machine that runs it. When you evaluate new MCP tools, ask the vendor about their trust model and how they test it against untrusted project inputs.


## Disclosure timeline


Date Event


2026-08-01 Vulnerability identified during research into AI coding agent attack surfaces


2026-08-01 Full advisory and proof of concept submitted to maintainers via GitHub private security advisory


2026-08-05 Report accepted by maintainers


2026-08-09 Fix shipped in serena-agent 1.7.0; public advisory published ([GHSA-pp25-4cg4-qcr9](https://github.com/oraios/serena/security/advisories/GHSA-pp25-4cg4-qcr9) )


2026-08-10 CVE requested with the GitHub CNA by the maintainers


We thank the Serena maintainers for their prompt and collaborative handling of the report.


## How GitLab can help


GitLab Duo Security Agent can help you audit your codebase for the same patterns. Questions like "does this project render user-controlled strings through a template engine?" or "are there configuration files in this repository that get passed to an execution context?" are a practical starting point.


## Looking ahead


The MCP ecosystem sits roughly where the npm ecosystem sat a decade ago: growing fast, adoption outpacing security scrutiny, and trust assumptions left implicit. The Serena finding is ordinary, and that is the point. It is the kind of issue that appears whenever a new technology matures faster than the security patterns around it, and MCP is maturing fast. We expect more of the same as MCP servers become standard developer infrastructure.


GitLab's Threat Research Group will keep assessing AI developer tooling and sharing findings as they are responsibly disclosed. We encourage researchers to apply the same scrutiny to MCP servers that the security community has long applied to browser extensions, IDE plugins, and CI/CD integrations: tools that run with significant privilege on developer machines and handle data from sources they do not fully control.
