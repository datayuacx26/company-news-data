---
schema_version: "1.0.0"
document_id: "65790243240e2da35635337299a9b0cb595f8282036f71749d7ad612f5d45995"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-hooks"
published_at: "2026-05-10T01:06:23+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:07.653203+00:00"
content_hash: "sha256:548efee6ac175f3f85c4afb8650d8970f57e760f5bae00eb68e005987816be95"
---

# Claude Code Hooks: Automate Commands at Every Stage of Agentic Tasks

## 5 Hook Configurations You Can Use Today


### 1. Auto-format code after every edit


The most common hook. Runs Prettier after every file edit, so formatting is always consistent — no manual` prettier --write` needed.


Add to` .claude/settings.json` in your project root:


```text
{
"hooks"  : {
"PostToolUse"  : [
{
"matcher"  :   "Edit|Write|MultiEdit"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  :   "jq -r '.tool_input.file_path' | xargs npx prettier --write 2>/dev/null || true"
}
]
}
]
}
}
```


The hook extracts the file path from the JSON input using` jq` , then passes it to Prettier. The` || true` prevents a non-zero exit if the file type isn't supported by Prettier.


**Requires:**` jq` installed (` brew install jq` or` apt-get install jq` )


### 2. Run tests after code changes


Run your test suite after every file edit. If tests fail, Claude sees the output and can fix the issue in the same turn.


```text
{
"hooks"  : {
"PostToolUse"  : [
{
"matcher"  :   "Edit|Write|MultiEdit"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  :   "npm test -- --bail 2>&1 | head -50"
}
]
}
]
}
}
```


` --bail` stops after the first test failure.` head -50` limits output so Claude's context doesn't fill up with test output. Adjust the test command for your stack (` bun test` ,` pytest` ,` cargo test` , etc.).


The` PostToolUse` hook receives test output in stdout but doesn't feed it back to Claude automatically. For Claude to act on test failures, use a` Stop` hook instead — it fires once per turn after Claude finishes, giving you control over what context Claude sees next.


### 3. Block dangerous commands


Block specific Bash commands before they execute. Claude receives your error message as feedback and adjusts.


```text
{
"hooks"  : {
"PreToolUse"  : [
{
"matcher"  :   "Bash"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  :   "python3 -c   \"\n  import sys, json  \n  data = json.load(sys.stdin)  \n  cmd = data.get('tool_input', {}).get('command', '')  \n  banned = ['rm -rf /', 'DROP TABLE', 'force-push', '--force main']  \n  for b in banned:  \n      if b in cmd:  \n          print(f'Blocked: command contains   \\\"  {b}  \\\"  ', file=sys.stderr)  \n          sys.exit(2)  \n\"  "
}
]
}
]
}
}
```


Exit code 2 blocks the command. Claude receives the stderr message and will either try a different approach or explain why it needs the blocked command.


For production use, move the check to a script file instead of inline Python:


```text
#!/bin/bash
# .claude/hooks/block-dangerous.sh
INPUT  =  $(  cat  )
CMD  =  $(  echo   "  $INPUT  "   |   jq   -r   '.tool_input.command // empty'  )


BLOCKED  =  (  "rm -rf /"   "DROP TABLE"   "git push --force main"   "chmod 777"  )


for   pattern   in   "${  BLOCKED  [  @  ]}"  ;   do
if   [[   "  $CMD  "   ==   *  "  $pattern  "  *   ]];   then
echo   "Blocked: '  $pattern  ' is not allowed in this project"   >&2
exit   2
fi
done


exit   0
```


Register it:


```text
{
"hooks"  : {
"PreToolUse"  : [
{
"matcher"  :   "Bash"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  :   "  \"  $CLAUDE_PROJECT_DIR  \"  /.claude/hooks/block-dangerous.sh"
}
]
}
]
}
}
```


### 4. Desktop notification when Claude needs input


Get a desktop notification whenever Claude finishes a task and is waiting for your next prompt. Useful when Claude is working on a long task and you've switched to another window.


**macOS:**


```text
{
"hooks"  : {
"Notification"  : [
{
"matcher"  :   "idle_prompt"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  :   "osascript -e 'display notification   \"  Claude is ready  \"   with title   \"  Claude Code  \"  '"
}
]
}
]
}
}
```


**Linux:**


```text
{
"hooks"  : {
"Notification"  : [
{
"matcher"  :   "idle_prompt"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  :   "notify-send 'Claude Code' 'Claude is ready for your next prompt'"
}
]
}
]
}
}
```


The` idle_prompt` matcher fires specifically when Claude is done and waiting — not on permission prompts or other notification types. Other matcher values:` permission_prompt` ,` auth_success` ,` elicitation_dialog` .


### 5. Re-inject context after compaction


When Claude's context window fills up, Claude Code compacts the conversation. Important details can get lost. A` SessionStart` hook with a` compact` matcher re-injects key context after every compaction.


```text
{
"hooks"  : {
"SessionStart"  : [
{
"matcher"  :   "compact"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  :   "echo 'CONTEXT RESTORED: Use Bun (not npm). Run bun test before committing. Database is Postgres via Blink. Current branch: feature/auth-refactor.'"
}
]
}
]
}
}
```


Any text your command writes to stdout gets injected into Claude's context. Replace the` echo` with a script that reads your project's current state — recent git commits, active issues, whatever Claude needs to stay on track.


## Combining Multiple Hooks


You can stack multiple hooks under a single event, and configure multiple events in the same file:


```text
{
"hooks"  : {
"PostToolUse"  : [
{
"matcher"  :   "Edit|Write|MultiEdit"  ,
"hooks"  : [
{
"type"  :   "command"  ,
}
]
}
],
"PreToolUse"  : [
{
"matcher"  :   "Bash"  ,
"hooks"  : [
{
"type"  :   "command"  ,
}
]
}
],
"Notification"  : [
{
"matcher"  :   "idle_prompt"  ,
"hooks"  : [
{
"type"  :   "command"  ,
}
]
}
]
}
}
```


Multiple hooks under the same event run in parallel. For` PreToolUse` decisions, Claude Code picks the most restrictive result — if any hook returns exit 2, the tool call is blocked.


## Hook Scope and Location


Location Scope Committed to repo?


` ~/.claude/settings.json` All projects No — local only


` .claude/settings.json` Single project Yes — can be shared


` .claude/settings.local.json` Single project No — gitignored


Use` ~/.claude/settings.json` for personal preferences (notification style, blocked commands you always want). Use` .claude/settings.json` in your project for team-shared automation (test runner, formatter).


Run` /hooks` inside Claude Code to see all configured hooks and which events have active configurations.


## Troubleshooting Common Issues


**Hook not firing:** Run` /hooks` to confirm the hook is registered. Check that your matcher matches the tool name exactly — matchers are case-sensitive (` Edit` not` edit` ).


**"command not found" errors:** Use absolute paths or` $CLAUDE_PROJECT_DIR` to reference scripts. The hook runs in a non-interactive shell that may not have your shell aliases or PATH configured.


**Shell profile output breaking JSON:** If your` .zshrc` or` .bashrc` contains` echo` statements that run unconditionally, they'll prepend to your hook's JSON output and break parsing. Wrap them:` if \[\[ $- == *i* \]\]; then echo "Shell ready"; fi`


**Stop hook running forever:** If you have a` Stop` hook that continues the conversation, check for the` stop_hook_active` field in the input JSON and exit early if it's` true` — otherwise your hook creates an infinite loop.


## Build Claude Code Hooks Into Your App With Claude Code or Cursor


Add[Blink](https://blink.new/) as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up Claude Code hooks to auto-run tests and format code after every edit, then build and deploy the app on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Yes. A` PreToolUse` hook can exit with code 2 to block any tool call — including Bash commands, file edits, or reads. Claude receives your stderr message as feedback and can adjust its approach. This is the mechanism for enforcing project rules that the LLM can't override.[Blink](https://blink.new/) uses a similar principle: infrastructure rules are enforced at the platform level, not left to the AI to remember.


Yes, and this is intentional.` PreToolUse` hooks fire before any permission-mode check. A hook that blocks a command will block it even in` bypassPermissions` mode or with` --dangerously-skip-permissions` . Hooks can tighten restrictions but cannot loosen them past what permission rules allow.


Claude Code v1.x supports 25+ hook event types as of 2026, including` PreToolUse` ,` PostToolUse` ,` Stop` ,` SessionStart` ,` Notification` ,` PreCompact` ,` PostCompact` ,` SubagentStop` ,` PermissionRequest` ,` CwdChanged` ,` FileChanged` ,` ConfigChange` , and more. The full reference is at[docs.claude.com](https://docs.claude.com/en/docs/claude-code/hooks-guide) . For production applications using Claude Code as an agentic backend, also consider[Blink Cloud](https://blink.new/cloud) — it adds a database, auth, and deploy layer that hooks can interact with.


Yes. Hooks in` ~/.claude/settings.json` apply to all your projects. Hooks in` .claude/settings.json` apply only to that project and can be committed to the repo so your whole team gets the same automation. You can layer both — project hooks run alongside global hooks.


Yes. Hooks run as shell commands with access to your environment. For injecting environment variables into Claude's context (not just your hook's execution environment), use` CLAUDE_ENV_FILE` — write a shell script to that path and Claude Code sources it before every Bash command. This is how` direnv` integration works with the` CwdChanged` hook. For apps that need environment management across multiple environments,[Blink](https://blink.new/) handles env vars at the platform level.


Hooks in` .claude/settings.json` can be committed to your repo and shared with your team — this is the intended use case for project-level automation like formatters and test runners. Hooks that reference secrets or personal paths should go in` .claude/settings.local.json` (gitignored) or` ~/.claude/settings.json` . Treat hook commands like you would any other shell script in your repo: review them before committing.
