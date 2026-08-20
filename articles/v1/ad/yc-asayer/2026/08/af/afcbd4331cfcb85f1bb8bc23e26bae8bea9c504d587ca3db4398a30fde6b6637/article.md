---
schema_version: "1.0.0"
document_id: "afcbd4331cfcb85f1bb8bc23e26bae8bea9c504d587ca3db4398a30fde6b6637"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/mac-awake-overnight-agent-runs-caffeinate/"
published_at: "2026-08-01T00:00:00+00:00"
first_seen_at: "2026-08-01T09:43:21.725514+00:00"
fetched_at: "2026-08-01T09:43:22.946026+00:00"
content_hash: "sha256:98ea67b8fa59d885788554acb285f7085762ab30681fefdc0d2deeaeb655194e"
---

# Keep Your Mac Awake for Overnight Agent Runs With caffeinate

` caffeinate` is a command-line tool built into macOS that prevents your Mac from going to sleep while a task runs — and the single most useful form is` caffeinate -i <your command>` , which wraps a process, holds the system awake for as long as it runs, and releases automatically the moment it exits. If you closed your laptop lid or walked away and came back to find an overnight Claude Code, Codex, or long build job dead, this is the built-in, no-install fix. This article covers the flags that matter, the durable overnight recipe, how to verify the assertion is actually held, and the one hard limit` caffeinate` cannot cross.


## Key Takeaways


- ` caffeinate` ships with macOS (since OS X 10.8), needs no install and no admin rights, and creates an IOKit power-management assertion — the same mechanism apps like Xcode use to keep a Mac awake.
- On a laptop, use` -i` to prevent idle sleep;` -s` only works on AC power and is silently ignored on battery, and` -d` needlessly keeps the display lit.
- Wrapping a command (` caffeinate -i npm run build` ) releases the assertion automatically when the process exits, so you can’t forget to turn it off.
- ` caffeinate` alone will not keep a MacBook awake with the lid shut: closing the lid triggers clamshell sleep, which a software assertion cannot override on battery.
- Run overnight agents inside` tmux` so the assertion survives a closed or crashed Terminal window.


## What` caffeinate` is on the Mac (the 10-second answer)


` caffeinate` is a supported macOS command-line tool that creates power-management assertions to alter sleep behavior; it has shipped in` /usr/bin/caffeinate` since OS X 10.8 (2012), requires no installation and no admin privileges. Under the hood it calls IOKit’s[power-assertion API](https://developer.apple.com/documentation/iokit/1557134-iopmassertioncreatewithname) — a mechanism that dynamically requests a system behavior from the power management system — via` IOPMAssertionCreateWithName` , the same sanctioned path applications use to keep a Mac awake during active work.


The behavior that matters for a long-running job is the wrap. If a utility is specified, caffeinate creates the assertions on the utility’s behalf, and those assertions persist for the duration of the utility’s execution; otherwise, caffeinate creates the assertions directly, and they persist until caffeinate exits. The canonical[man-page example](https://www.unix.com/man_page/osx/8/caffeinate/) is` caffeinate -i make` : caffeinate forks a process, execs “make” in it, and holds an assertion that prevents idle sleep as long as that process is running.


## The` caffeinate` flags that matter


On a laptop, the one flag that matters is` caffeinate -i` : it prevents the system from idle-sleeping while allowing the display to dim, which is exactly what a background agent or build needs. The[full documented flag set](https://ss64.com/mac/caffeinate.html) is larger than most guides admit:


Flag Prevents Notes


` -i` Idle sleep The developer default; display can still turn off


` -d` Display sleep Keeps the screen lit — wastes power for a headless job


` -m` Disk idle sleep Rarely needed for CPU/network-bound agents


` -s` System sleep AC power only


` -u` Declares user activity One-shot; resets the idle timer


` -t N` — Auto-exits after N seconds


` -w pid` — Holds the assertion until the given PID exits


Two details competitors get wrong. First, the` -s` assertion is valid only when the system is running on AC power — on battery it silently no-ops, so it’s the wrong default for an unplugged laptop. Second, if the display is off,` -u` turns it on and prevents idle sleep, and if no timeout is specified with` -t` , that assertion defaults to a 5-second timeout. Wrapping a task is safer than a bare assertion because it can’t outlive the job:` caffeinate -i npm run build` prevents sleep during the build and releases the instant it finishes.


## The overnight-agent recipe


To keep[Claude Code](https://github.com/anthropics/claude-code) ,[Codex CLI](https://github.com/openai/codex) , or Aider alive overnight, wrap the agent’s launch command directly and run it inside a terminal multiplexer so nothing depends on your Terminal window staying open. Because the assertion lives inside the` caffeinate` process, it dies if Terminal closes or crashes —` tmux` gives you true durability:


```text
# Start a detached tmux session that holds the assertion for its whole life
tmux   new-session   -d   -s   agent   '  caffeinate -i claude  '
```


If the agent is already running in another pane, don’t restart it — tie a fresh assertion to its existing process ID with` -w` :


```text
# Find the PID, then hold the assertion until that process exits
pgrep   -f   claude
caffeinate   -i   -w   <  pi  d  >
```


Before you walk away from an 8-hour run, prove the assertion is actually held. In a second pane, run:


```text
pmset   -g   assertions
```


A` PreventUserIdleSystemSleep` entry in that output confirms macOS will not idle-sleep while your job runs; when the wrapped command exits, the line disappears. This one diagnostic is the difference between hoping and knowing your overnight run is protected.


## Can` caffeinate` keep a MacBook awake with the lid closed?


` caffeinate` alone will not keep a MacBook awake with the lid shut. Closing the lid triggers clamshell sleep, and a software power assertion cannot override it on battery power — full stop. This is the limit no keep-awake CLI can cross, and it’s why a job dies the moment you fold the laptop and unplug it.


Lid-closed operation works in exactly two supported cases. The first is Apple’s[clamshell mode](https://www.macworld.com/article/673295/how-to-use-macbook-with-lid-closed-stop-closed-mac-sleeping.html) : Apple supports using a MacBook with the lid closed by connecting an external display, keyboard, and mouse, but the setup has strict requirements — it won’t work without an external monitor, and it can introduce thermal trade-offs. Apple’s recommended setup connects an external display, a power adapter, and an external keyboard and mouse or trackpad. The second is the System Settings → Battery → Options toggle, “Prevent automatic sleeping when the display is off” — but this requires the computer to be[connected to a power source](https://support.gmhec.org/TDClient/47/middlebury/KB/ArticleDet?ID=567) , not running from its battery, to work as intended.


If you genuinely need battery power *and* a closed lid — a laptop in a bag running an agent —` caffeinate` is the wrong tool.[Amphetamine](https://apps.apple.com/us/app/amphetamine/id937984704) is built for that case: its documentation states that with Amphetamine, your Mac does not need a display, keyboard, mouse, or power adapter to use Closed-Display Mode, because it uses a[publicly-accessible API to disable Apple’s requirements](https://iffy.freshdesk.com/support/solutions/articles/48001077199-amphetamine-closed-display-mode) .


Here’s the whole picture as a truth table:


Scenario` caffeinate -i` Battery > Options Amphetamine


Lid open, idle, on battery Stays awake Stays awake Stays awake


Lid closed, on battery Sleeps Sleeps Stays awake


Lid closed, on AC + external display/keyboard Stays awake Stays awake Stays awake


Terminal window closes or crashes Assertion dies Unaffected Unaffected


Wrapped command finishes Auto-releases Unaffected Session continues


## Stopping it, and the gotchas


Stop` caffeinate` with Ctrl+C in the foreground, or let it exit on its own when the wrapped command finishes — the wrap pattern is deliberately hard to leave running by mistake. The failure modes are worth naming. A bare` caffeinate` with no wrapped utility and no` -t` runs until you kill it; leaving any Mac awake continuously[increases battery drain, raises component temperatures, and adds SSD writes over time](https://setapp.com/how-to/how-to-stop-mac-from-sleeping) , so scope the assertion to the job. And because the assertion is bound to the process, closing or crashing the Terminal drops it instantly — which is the whole argument for wrapping the agent in` tmux` rather than typing` caffeinate` and hoping the window stays open all night.


## When to reach for more


` caffeinate` is the right tool when your Mac is plugged in, or lid-open, and you want a specific overnight job protected with automatic cleanup. When you need battery-plus-closed-lid, hand off to Amphetamine and its session-based controls. And when a build or agent run is heavy enough to run for days, the cleaner answer is often to stop babysitting a laptop’s power state entirely and run the workload on an always-on remote box — a machine that never sleeps because it was never designed to. For the overnight-on-your-own-Mac case, wrap the command, verify the assertion with` pmset -g assertions` , and go to sleep.


## FAQs


What is the difference between caffeinate -i and caffeinate -d?


The -i flag prevents idle system sleep while still allowing the display to turn off, which is what a background agent or build needs and the correct default on a laptop. The -d flag prevents display sleep, keeping the screen lit and wasting power on a headless job. Use -i for overnight terminal work and reserve -d only when the display itself must stay on, such as a monitoring dashboard.


Does caffeinate need sudo or admin rights to run?


No. caffeinate needs no installation and no administrator privileges because it uses IOKit's public power-assertion API, the same sanctioned mechanism applications like Xcode use to keep a Mac awake. Any user can run it directly from Terminal. It ships built into macOS at /usr/bin/caffeinate and has done so since OS X 10.8, so there is nothing to install and no elevated permission to grant.


What happens to caffeinate if I close the Terminal window?


The assertion dies instantly. Because the power-management assertion lives inside the caffeinate process, closing or crashing the Terminal window terminates that process and drops the assertion, letting the Mac idle-sleep. To survive a closed or crashed window, run the job inside tmux with caffeinate wrapping the command inside the session, so the assertion is bound to a detached process that outlives your Terminal.


How do I keep caffeinate running for only a set number of hours?


Use the -t flag with a timeout in seconds. For example, caffeinate -i -t 28800 prevents idle sleep for exactly eight hours and then exits automatically, releasing the assertion. This is useful when you want a fixed window rather than tying the assertion to a specific command's lifetime. Without -t or a wrapped utility, a bare caffeinate runs until you manually stop it with Ctrl+C.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
