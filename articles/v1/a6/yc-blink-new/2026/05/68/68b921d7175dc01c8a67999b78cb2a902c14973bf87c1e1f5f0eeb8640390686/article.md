---
schema_version: "1.0.0"
document_id: "68b921d7175dc01c8a67999b78cb2a902c14973bf87c1e1f5f0eeb8640390686"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-hooks-guide"
published_at: "2026-05-24T12:34:15+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:310152d0a98f94a354f2a00feef48f6e85ed038a42380f6cfebf241fb3407c28"
---

# Claude Code Hooks: Auto-Approve Tools, Get Notifications, and Run Linters

## 5 Hook Recipes Ready to Paste


### Recipe 1 — Desktop notification when Claude pauses


```text
{
"hooks"  : {
"Notification"  : [
{
"hooks"  : [
{
"type"  :   "command"  ,
"command"  : [  "osascript"  ,   "-e"  ,   "display notification   \"  Claude needs your input  \"   with title   \"  Claude Code  \"  "  ]
}
]
}
]
}
}
```


The` Notification` hook requires no` matcher` — it fires on every pause event. The` osascript` call triggers a native macOS notification. For a cross-platform bell, return` {"terminalSequence": "\\u0007"}` from any hook script instead.


### Recipe 2 — Auto-approve file reads (skip the permission prompt)


```text
{
"hooks"  : {
"PreToolUse"  : [
{
"matcher"  :   "Read"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  : [  "node"  ,   "-e"  ,   "process.stdout.write(JSON.stringify({decision:'approve'}))"  ],
"timeout"  :   1000
}
]
}
]
}
}
```


The hook writes` {"decision":"approve"}` to stdout. Claude reads that output and skips the permission dialog. Using the` string\[\]` exec form spawns` node` directly — no shell, no quoting issues.


### Recipe 3 — Run Prettier after every file write


```text
{
"hooks"  : {
"PostToolUse"  : [
{
"matcher"  :   "Write"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  : [  "sh"  ,   "-c"  ,   "prettier --write   \"  $CLAUDE_TOOL_INPUT_FILE_PATH  \"   2>/dev/null || true"  ],
"timeout"  :   10000
}
]
}
]
}
}
```


` $CLAUDE_TOOL_INPUT_FILE_PATH` holds the path Claude just wrote. The` || true` prevents the hook from failing when Prettier finds no matching parser.` PostToolUse` fires after the tool completes, so the file already exists on disk.


### Recipe 4 — Block writes to protected files


```text
{
"hooks"  : {
"PreToolUse"  : [
{
"matcher"  :   "Write"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  : [  "sh"  ,   "-c"  ,   "if echo   \"  $CLAUDE_TOOL_INPUT_FILE_PATH  \"   | grep -q 'credentials  \\  |secrets  \\  |  \\  .env$'; then echo '{  \"  decision  \"  :  \"  block  \"  ,  \"  reason  \"  :  \"  Cannot write to sensitive files  \"  }'; else echo '{  \"  decision  \"  :  \"  approve  \"  }'; fi"  ],
"timeout"  :   2000
}
]
}
]
}
}
```


The script checks whether the target path matches` credentials` ,` secrets` , or` .env` . A match returns a block decision with a readable reason. Claude receives that reason and can try a different path without session interruption.


### Recipe 5 — Log all Bash commands to a file


```text
{
"hooks"  : {
"PostToolUse"  : [
{
"matcher"  :   "Bash"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  : [  "sh"  ,   "-c"  ,   "echo   \"  $(date): $CLAUDE_TOOL_INPUT_COMMAND  \"   >> ~/.claude/bash-history.log"  ]
}
]
}
]
}
}
```


` $CLAUDE_TOOL_INPUT_COMMAND` holds the exact command Claude passed to the Bash tool. This recipe appends every command with a timestamp to` ~/.claude/bash-history.log` . Review the log after a long session to audit exactly what the agent ran.


The Claude Code hook lifecycle: PreToolUse runs before a tool, PostToolUse runs after, and Notification fires when Claude waits for input


Blink


## The New string\[\] Exec Form (Safer Hooks)


The Claude Code[May 2026 release](https://code.claude.com/docs/en/whats-new/2026-w20) added the` string\[\]` exec form for hook commands. The old string form passes your command through a shell:


```text
"command"  :   "prettier --write   \"  $FILE  \"  "
```


The new array form spawns the binary directly:


```text
"command"  : [  "prettier"  ,   "--write"  ,   "$FILE"  ]
```


No shell interprets the array form. Three problems disappear: shell injection risk, quoting edge cases, and` .bashrc` interference.


The` .bashrc` interference is the most common failure mode. If your shell RC file prints anything on startup — a greeting, a conda prompt, a version line — hooks that run through` bash -c` mix that output into the JSON Claude reads. Claude cannot parse` Hello, Alex!\\n{"decision":"approve"}` . It ignores the decision entirely. The` string\[\]` form bypasses shell initialization files.


Use the string form only when you need shell features: pipes, redirects, or command substitution. For everything else, use` string\[\]` .


## continueOnBlock: Let Claude Handle Rejections


By default, when a` PreToolUse` hook returns` {"decision": "block"}` , Claude stops the current turn. The session waits for your next message.


` continueOnBlock` changes that behavior. Add it to the hook config:


```text
{
"type"  :   "command"  ,
"command"  : [  "sh"  ,   "-c"  ,   "..."  ],
"continueOnBlock"  :   true
}
```


When` continueOnBlock` is` true` , the block reason feeds back to Claude as tool output. The agent reads the rejection reason and keeps the turn going. It can reason about the block and try a different approach — no session pause, no manual intervention.


This is the right pattern for guardrails that should guide behavior rather than halt it. A block on a sensitive file write becomes: "That path is protected — use a different location."


` continueOnBlock` is a PostToolUse-only field in the hook config. It has no effect when placed on PreToolUse hooks.


## terminalSequence: Desktop Notifications Without a Terminal


Some Claude Code setups run without a controlling terminal — in CI, headless environments, or via IDE integrations. Standard` osascript` and` notify-send` calls work, but triggering a terminal bell requires the` terminalSequence` output field.


Return it from any hook script:


```text
{  "terminalSequence"  :   "  \u0007  "  }
```


` \\u0007` is the ASCII bell character. Any terminal listening to Claude Code's output will ring. No controlling terminal is required. This works cross-platform without OS-specific dependencies.


For notification workflows that must work on macOS, Linux, and headless alike, combine both:


```text
# Ring the bell everywhere; native notification on macOS only
if   command   -v   osascript   &  >  /dev/null;   then
osascript   -e   'display notification "Claude needs input" with title "Claude Code"'
fi
echo   '{"terminalSequence": "\u0007"}'
```


For full hook field reference, see the[official Claude Code hooks documentation](https://code.claude.com/docs/en/reference/hooks) .


## Build This Into Your App With Claude Code and Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up hooks for auto-approval of file reads, run Prettier on every write, and deploy the app on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


With hooks configured, Claude Code handles routine approvals and cleanup automatically — you only step in when there's a real decision to make


Blink


The hook must write valid JSON to stdout — exactly` {"decision":"block","reason":"..."}` . If your script prints anything else first (output from` .bashrc` , a debug line), Claude cannot parse the JSON and ignores the decision. Switch to the` string\[\]` exec form to bypass shell init files. Run` claude hooks list` to confirm the hook registered correctly.


Hooks run on the machine where Claude Code is installed, not on a remote server. If you run Claude Code over SSH or inside a container, hook commands execute in that environment. Ensure any binaries your hooks call —` prettier` ,` osascript` ,` node` — exist in that environment's PATH.


Run` claude hooks list` to confirm the hook registered. Add` >&2 echo "hook fired"` at the start of your command — Claude Code displays stderr in the session output. If you see JSON parse failures, check for` .bashrc` output polluting stdout and switch to the` string\[\]` exec form.


Claude Code injects tool-specific variables into every hook process.` CLAUDE_TOOL_INPUT_FILE_PATH` holds the file path for` Write` and` Read` operations.` CLAUDE_TOOL_INPUT_COMMAND` holds the command string for` Bash` operations. Inside` Stop` hooks,` stop_hook_active` is set to` true` — check this variable to prevent infinite loops where the hook triggers another stop.
