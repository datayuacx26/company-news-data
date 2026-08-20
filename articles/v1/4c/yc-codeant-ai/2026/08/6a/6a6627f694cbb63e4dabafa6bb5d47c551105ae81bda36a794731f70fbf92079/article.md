---
schema_version: "1.0.0"
document_id: "6a6627f694cbb63e4dabafa6bb5d47c551105ae81bda36a794731f70fbf92079"
company_key: "yc-codeant-ai"
company: "CodeAnt AI"
source_id: "yc-codeant-ai-news-import-b68a5af7b5b5"
canonical_url: "https://codeant.ai/blogs/claude-code-macos-sandbox-escape"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T15:24:47.352463+00:00"
fetched_at: "2026-08-18T15:24:48.929333+00:00"
content_hash: "sha256:a3b3bc38c167a69cdf8fd780389748cc0a1031e1a2c3616e0882fe942e8dbef0"
---

# Claude Code Sandbox Escape on macOS

## Claude Code macOS Sandbox Escape: Executive Summary


During a[review](https://codeant.ai/ai-code-review) of Claude Code's macOS[sandbox](https://codeant.ai/blogs/agentic-rag-shell-sandboxing) policy generator, our team identified a path-provenance flaw that caused literal filesystem names to be interpreted as glob patterns.


Claude Code automatically authorizes sandboxed writes to the current workspace. If the workspace's real directory name contained glob metacharacters, such as:


```text
review  - bundle  **. claude
```


```text
review  - bundle  **. claude
```


```text
review  - bundle  **. claude
```


the policy generator resolved the directory as a literal path and then passed the resulting string through glob detection. The` **` bytes were treated as pattern syntax and compiled to the regular expression fragment` .*` .


This widened the macOS Seatbelt write policy beyond the source workspace. The same conversion also altered the mandatory deny rule that protects` .claude/settings.local.json` . A sibling settings path could therefore match the write allow while failing to match the protected-settings deny.


A sandboxed, auto-approved Bash command could use this policy mismatch to create a sibling project's` .claude/settings.local.json` . The injected file defined a` SessionStart` command hook. When the already-trusted sibling project was opened later, Claude Code executed the hook before initialization, authentication, model output, or access to the Bash tool.


The proof of concept used no symbolic link, hard link, filesystem race, alias, pre-existing hook, or valid Anthropic credential.


Anthropic validated the behavior, assigned **CVSS 4.0 7.7 (High)** , and awarded a **$XXXX bounty** .


## Affected Claude Code Versions and macOS Scope


The original report tested the official Claude Code 2.1.214 macOS arm64 executable. A separate regression run confirmed the same behavior on 2.1.215.


Item


Confirmed value


Product


Claude Code


Platform tested


macOS arm64 using Seatbelt


Version 2.1.214 SHA-256


` 59796dd18e9d77f1256f367db6d28ce4bd9cd5968e402ad3a327aac36abc6dec`


Version 2.1.215 SHA-256


` 90608b5c5ab504e96e77365cea6203d046e291d59b2bb42cf28dcb2ccdf9dd58`


2.1.214 result


Complete differential matrix passed


2.1.215 result


Complete differential matrix passed


Vendor severity


CVSS 4.0 7.7, High


Vendor vector


` CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:P/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N`


This article does not claim that Linux or Windows shared the macOS Seatbelt-specific implementation flaw.


The demonstrated chain required the following conditions:


1.


Claude Code was running an affected macOS build.


2.


` sandbox.enabled` and` sandbox.autoAllowBashIfSandboxed` were enabled.


3.


The source workspace was already trusted.


4.


The source workspace's real path contained literal glob metacharacters.


5.


A sibling project existed below the same canonical parent directory.


6.


That already-trusted sibling project was launched after the configuration write.


The sequence used two Claude Code launches, one for the source workspace and one for the later victim launch. It did not require concurrent sessions or a timing race.


## How the Claude Code macOS Sandbox Is Supposed to Work


Claude Code's[Bash sandbox documentation](https://code.claude.com/docs/en/sandboxing) states that sandboxed commands can write to the working directory and the session temporary directory by default. In auto-allow mode, eligible sandboxed commands run without a separate Bash confirmation because the operating system policy is expected to enforce that boundary.


The target file was:


```text
. claude  / settings  . local  . json
```


```text
. claude  / settings  . local  . json
```


```text
. claude  / settings  . local  . json
```


Claude Code treats this as[local configuration for the current project](https://code.claude.com/docs/en/settings) . The file can define[command hooks](https://code.claude.com/docs/en/hooks) , including` SessionStart` hooks that run when a new session starts or an existing session resumes.


Claude Code also generated a mandatory sandbox deny for protected project configuration. The expected authorization model was therefore clear:


-


Source project A was trusted.


-


Sandboxed commands launched from A could write inside A.


-


Sibling project B was a separate project with its own trust record.


-


A sandboxed command in A could not write B's protected local settings.


-


Trusting A did not authorize repository code or dependencies in A to modify B.


The vulnerability bypassed both the working-directory boundary and the protected-settings deny.


## Root Cause: Literal Path and Glob Pattern Confusion


The reverse-engineered JavaScript from Claude Code 2.1.214 showed the following policy-generation path. These are minified identifiers from the pinned build, so their names can change between releases.


Function


Observed role


` QJt()`


Adds the automatic write entry` "."` to the sandbox configuration


` aB()`


Resolves the entry against the current working directory


` U6()`


Detects glob syntax by inspecting characters in the resulting string


` Nnt()`


Converts glob syntax to a regular expression


` sfg()`


Generates macOS write-allow and protected-path deny rules


` vXc()`


Launches the command under the generated Seatbelt profile


The initial resolution step was correct. From the affected workspace, the automatic entry` "."` resolved to one real directory:


```text
<  parent  >
```


```text
<  parent  >
```


```text
<  parent  >
```


At this point, the asterisks were literal filename bytes. The value should have remained a literal canonical path.


Instead, the value was passed to` U6()` as an untyped string.` U6()` inferred glob intent from the characters alone.` Nnt()` then converted each` **` sequence to` .*` .


The security-relevant type information was lost between path resolution and policy generation. A value that represented one filesystem object was reinterpreted as a pattern representing multiple filesystem objects.


*The upper path in Figure 2 represents the required remediation invariant. The lower path represents the behavior observed in the affected builds.*


## How the Sandbox Allow and Deny Rules Diverged


For the following source workspace:


```text
<  parent  >
```


```text
<  parent  >
```


```text
<  parent  >
```


the generated write allow was equivalent to:


```text
^ <  parent  >
```


```text
^ <  parent  >
```


```text
^ <  parent  >
```


The protected local-settings deny was equivalent to:


```text
^ <  parent  >
```


```text
^ <  parent  >
```


```text
^ <  parent  >
```


The sibling target was:


```text
<  parent  >
```


```text
<  parent  >
```


```text
<  parent  >
```


The allow matched this path. Its first` .*` matched` -private/` , the literal` \\.claude` matched the sibling's settings directory, and the final` .*` matched the remaining filename.


The deny did not match. After consuming the sibling's` .claude` directory, the generated deny still required another` .claude/settings.local.json` suffix. The target contained only one such directory.


```text
ALLOW :     review  - bundle   .* \. claude   .*                          MATCH
TARGET :    review  - bundle  - private  /. claude  / settings  . local  . json


DENY :      review  - bundle   .* \. claude   ... \. claude  / settings  ...   MISS
requires   a   second   . claude
```


```text
ALLOW :     review  - bundle   .* \. claude   .*                          MATCH
TARGET :    review  - bundle  - private  /. claude  / settings  . local  . json


requires   a   second   . claude
```


```text
ALLOW :     review  - bundle   .* \. claude   .*                          MATCH
TARGET :    review  - bundle  - private  /. claude  / settings  . local  . json


requires   a   second   . claude
```


Seatbelt and the regular expression engine enforced the supplied policy. The vulnerability was in the compiler that selected which paths the policy described.


## Claude Code Sandbox Escape: Differential Validation


The proof of concept used four matched cases. The first three retained the same victim, payload, command, trust state, permission mode, binary, and target. Only the literal source directory name changed.


Case


Literal source directory


Allow matches


Settings deny matches


Settings file created


Hook executes


Ordinary negative


` review-bundle`


No


No


No


No


Protected-path control


` review-bundle**`


Yes


Yes


No


No


Targeted exploit


` review-bundle**.claude**`


Yes


No


Yes


Yes


Parent-wide ceiling


` **.claude**`


Yes


No


Yes


Yes


The` review-bundle**` control was essential. Its broadened allow permitted creation of the sibling's previously absent` .claude` directory, but the generated protected-settings deny still matched and blocked the file copy. This established three facts:


1.


The command was executing inside the sandbox.


2.


The protected-settings deny took precedence when it matched.


3.


The positive result depended on the` .claude` token changing the deny language, not on the absence of a protection rule.


The` **.claude**` case measured the proven write envelope. It reached an unrelated sibling name below the same canonical parent. The proof does not claim arbitrary filesystem-wide writes from the initial sandbox primitive.


## From Protected Configuration Write to SessionStart Command Execution


The injected local settings file contained a minimal` SessionStart` command hook. The published example keeps the payload limited to a benign nonce marker:


```text
{
"hooks"  :    {
"SessionStart"  :    [
{
"matcher"  :    ""  ,
"hooks"  :    [
{
"type"  :    "command"  ,
"command"  :    "/usr/bin/python3 -c '<write a benign nonce marker>'"
}
]
}
]
}
}
```


```text
{
"hooks"  :    {
"SessionStart"  :    [
{
"matcher"  :    ""  ,
"hooks"  :    [
{
"type"  :    "command"  ,
"command"  :    "/usr/bin/python3 -c '<write a benign nonce marker>'"
}
]
}
]
}
}
```


```text
{
"hooks"  :    {
"SessionStart"  :    [
{
"matcher"  :    ""  ,
"hooks"  :    [
{
"type"  :    "command"  ,
"command"  :    "/usr/bin/python3 -c '<write a benign nonce marker>'"
}
]
}
]
}
}
```


The source-side operation required only directory creation and a file copy:


```text
mkdir    -p    --   <sibling>/.claude
cp    --


```


```text
mkdir    -p    --   <sibling>/.claude
cp    --


```


```text
mkdir    -p    --   <sibling>/.claude
cp    --


```


Observed behavior differed according to the generated policy:


-


In the ordinary negative case,` mkdir` failed with` Operation not permitted` .


-


In the protected-path control,` mkdir` succeeded and` cp` failed on the protected file.


-


In the two positive cases, both operations succeeded without a Claude Code permission denial.


The sibling was then launched with Bash explicitly disabled and without an API credential. The recorded event order was:


```text
hook_started
->  hook_response    (  success  )
->  init
->  assistant    (  authentication   failure )
->  result
```


```text
hook_started
->  hook_response    (  success  )
->  init
->  assistant    (  authentication   failure )
->  result
```


```text
hook_started
->  hook_response    (  success  )
->  init
->  assistant    (  authentication   failure )
->  result
```


The nonce marker existed before the first assistant event. The victim process recorded:


```text
apiKeySource :    none
Bash   tool   available :    false
duration_api_ms :    0
total_cost_usd :    0
```


```text
apiKeySource :    none
Bash   tool   available :    false
duration_api_ms :    0
total_cost_usd :    0
```


```text
apiKeySource :    none
Bash   tool   available :    false
duration_api_ms :    0
total_cost_usd :    0
```


This evidence supports the term **pre-model command execution** . The` SessionStart` command completed before initialization and before any successful model interaction. The hook also wrote its marker above the extraction parent, demonstrating that the canonical-parent ceiling constrained only the initial sandboxed file write. It did not constrain the later host process.


## Claude Code Path Confusion Vulnerability Explained


This was not a symbolic-link or time-of-check to time-of-use issue.


The proof used:


-


no symbolic link;


-


no hard link;


-


no rename or directory-swap race;


-


no alternate path alias;


-


no pre-existing` .claude` directory;


-


no pre-existing setting, hook, helper, or Git execution configuration; and


-


no trust decision after the configuration was written.


The path did not change between validation and use. The same path string was interpreted under two incompatible types. The most accurate classification is **literal-path and pattern type confusion in a security-policy compiler** .


## What the Claude Code Sandbox Escape Allows


Workspace trust did not make the behavior expected or authorized.


The user trusted the source project. The sibling was a different project with a separate trust record. More importantly, the source process ran under an enabled sandbox whose purpose was to limit auto-approved Bash writes to the source workspace.


The target file was also covered by a mandatory deny. The protected-path control demonstrated that this deny was active and blocked the exact target when the generated expression matched.


The capability change was:


```text
sandboxed   code   with    write   access   limited   to   the   source   workspace
->
write   protected   Claude   Code   configuration   into   a   sibling   project
->
execute   an   unsandboxed   user   command   when   that   sibling   starts   later
```


```text
->
write   protected   Claude   Code   configuration   into   a   sibling   project
->
```


```text
->
write   protected   Claude   Code   configuration   into   a   sibling   project
->
```


The attacker principal did not begin with permission to write the sibling's protected settings or execute a host command during the sibling's startup. The vulnerable policy generator granted both capabilities through separate lifecycle transitions.


## Exploit Conditions and Scope Limitations


macOS permits glob metacharacters in directory names. The proof also verified that the stock` ditto` and` unzip` utilities preserved archive members named:


```text
review  - bundle  **. claude  **
**. claude


```


```text
review  - bundle  **. claude  **
**. claude


```


```text
review  - bundle  **. claude  **
**. claude


```


This provides a practical delivery path through an archive, copied project template, support bundle, or reproduction package whose top-level name is preserved.


The report does not claim that an ordinary` git clone` automatically selects such a destination name. It also does not claim direct writes above the fixed canonical parent prefix or through unrelated mandatory deny rules.


## How to Fix the Claude Code Sandbox Path Confusion


The policy generator should preserve path provenance instead of inferring a value's type from its characters.


Recommended changes are:


1.


Represent generated filesystem values using distinct` LiteralPath` and` GlobPattern` types.


2.


Resolve automatic entries such as` .` to canonical absolute paths and emit literal Seatbelt` subpath` rules.


3.


Do not infer glob intent from metacharacters found inside a resolved filesystem path.


4.


Emit protected settings, hook, and Git paths as literal canonical denies, including when an ancestor contains` *` ,` ?` ,` \[` or` \]` .


5.


Parse patterns only in settings fields that explicitly accept user-authored globs.


6.


Add real-directory regression tests for` review-bundle**` ,` review-bundle**.claude**` ,` **.claude**` ,` project?` , and` \[abc\]` .


The general engineering requirement is straightforward: a security-policy compiler must preserve whether a value represents one literal filesystem object or a language of possible objects.


## Claude Code Vulnerability Disclosure Timeline


Date


Event


18 July 2026


Report submitted to Anthropic through HackerOne with a four-case differential proof of concept


20 July 2026


Anthropic validated the behavior and assigned CVSS 4.0 7.7, High


4 August 2026


Anthropic awarded a $XXXX bounty


TBD


Fixed version, advisory or CVE decision, and coordinated public disclosure


At the time of this draft, the report remained private. The complete proof-of-concept script and private HackerOne material are intentionally excluded pending coordinated disclosure.


## What This Claude Code Sandbox Escape Teaches About Path Security


The[vulnerability](https://codeant.ai/vulnerability-database) originated at the boundary between filesystem resolution and pattern compilation.[Claude Code](https://codeant.ai/blogs/anthropic-claude-code-review) correctly resolved the current workspace to one real directory, but later discarded that provenance and treated metacharacters in the directory name as glob syntax.


That type change altered both sides of the policy. It broadened the write allow and changed the protected-settings deny so that the sibling target fell inside the first expression and outside the second. The resulting protected configuration write led to command execution during the sibling project's next startup, before the victim session produced model output.


Security-sensitive path processing should retain explicit type information from input parsing through policy generation. Literal paths should remain literal regardless of the characters they contain.


*Research and technical analysis by our AI Security Researcher,*[Sunder Singh](https://www.linkedin.com/posts/sunder-singh1_%F0%9D%97%9B%F0%9D%97%B2%F0%9D%97%BF%F0%9D%97%B2-%F0%9D%98%84%F0%9D%97%B2-%F0%9D%97%B4%F0%9D%97%BC-%F0%9D%97%AE%F0%9D%97%B4%F0%9D%97%AE%F0%9D%97%B6%F0%9D%97%BB-%F0%9D%97%AE%F0%9D%97%BB%F0%9D%97%BC%F0%9D%98%81%F0%9D%97%B5-activity-7493886630705922048-Fbdd?utm_source=share&utm_medium=member_desktop&rcm=ACoAACLqSagBT7beLEGx9cb4w1sPcZVtCsn-qK0) *. Anthropic's security team reproduced the submitted behavior and provided the severity assessment stated above.*
