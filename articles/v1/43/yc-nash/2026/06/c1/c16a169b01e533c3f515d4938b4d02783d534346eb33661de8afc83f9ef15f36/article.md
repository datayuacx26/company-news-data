---
schema_version: "1.0.0"
document_id: "c16a169b01e533c3f515d4938b4d02783d534346eb33661de8afc83f9ef15f36"
company_key: "yc-nash"
company: "Nash"
source_id: "yc-nash-news-import-75b8b0139b8c"
canonical_url: "https://www.nash.ai/blog/everyone-ships"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-24T12:02:46.184316+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:99d59bf32d298c469fa3ce4c171527e574fa1fbc47c32ccb05b641f9670be445"
---

# At Nash, Everyone Ships

Nash keeps deliveries moving for retailers and fleets nationwide. We built an in-house agent so the people closest to that work can ship the code themselves, not only the engineers. This is what changed once everyone could ship.


Nash moves deliveries for thousands of businesses, national grocery chains and local fleets alike, in real time and around the clock. When something breaks in that flow, the person who notices first is rarely an engineer. A support lead sees exactly why a delivery is stuck. A customer-success manager knows which orders were mispriced, and why. For most of our history, all they could do was write it up, file a ticket, and wait. The fix was often a single line, but it still had to wait its turn behind everything else in the queue.


One of our values is that everyone ships. For most of our history, that value ran ahead of our tooling. Engineers shipped; everyone else filed a ticket and described the problem.


So we built Carl.


Carl


Carl APP


Nash engineering agent


This is the very beginning of your direct message history with **Carl** .


**Carl** APP


2:14 PM


Describe a bug or a change. I’ll read the codebase, write the fix, run the tests, and open a pull request.


Carl is our in-house agent for shipping code. You describe a bug or a change in plain English, from Slack, Linear, the web, or Claude Code, and Carl reads the codebase, writes the fix, runs the tests, and opens a pull request. A human reviews it and merges it. The pull request is attributed to the person who asked, not to the bot.


We built the first version in a week, starting from a fork of Cole Murray’s open-source background-agents project and shaping it to how Nash works. We did not stand up a platform team to get there. We composed one out of pieces we already had: sandboxed cloud machines to run the agent safely, Claude to drive it, and the tools our team already lived in. Carl’s first pull request landed on February 13. By the end of its first full month it was opening hundreds, 446 in March alone.


The clearest sign it worked is that we used Carl to build Carl. In its first week, our daily ship log hit 62, then 74 pull requests merged in a single day.


0


Pull requests opened


0


Merged to production


0


Repositories touched


0


Human co-authored


The part we had not predicted was who would use it. People opening pull requests stopped being only engineers.


A support manager traced a database bug that was quietly erasing a key field on roughly eighty thousand rows, then had Carl ship the one-line fix, the test, and the ticket to track it. A customer-success manager removed a billing bug in minutes. A non-engineering executive went from “write me a test script” to shipping a real API feature. None of them waited for an engineer to find time. Describing the problem was most of the work of fixing it.


The numbers caught up to the stories. Since February, Carl has opened more than 1,300 pull requests across nineteen of our repositories, over 700 of them merged, with a median time from open to merge of a little over two hours. Almost half the non-engineering org has shipped code through Carl, and 97% of what Carl merges carries a human co-author who signed off on it (the other 3% are routine package bumps or test enhancements). The people shipping now come from support, customer success, operations, sales, and product.


Before Carl


→


After Carl


Engineering


Support


Customer Success


Operations


Leadership


Our engineering meetings changed character. They used to be triage: which small bugs to prioritize, what to cut, who had capacity to take them (or do we give them all to the on-call engineer?). Now they are review: how to evaluate the work arriving from everyone closest to the problems. We made that flip on purpose. Carl opens its pull requests as drafts, attributed to the human who asked, so an engineer’s job is to review, with careful attention to the plan and root cause of the session or PR, and merge rather than write every small fix by hand. As one customer-success manager asked mid-thread, after Carl built her change:


> So this is awaiting review by eng now, correct?


Carl reviews code, too. It has left nearly 1,900 reviews on our pull requests, the human-written ones included, and does so in an automated fashion. It also has judgment about what it should not do: alongside the foundation model’s underlying constitution and guardrails, we have found and tuned open-source guardrail skills so Carl knows its limits.


## How we built Carl


Carl is an agent runtime wrapped in a control plane that gives it three things a chat tool does not have: a real machine to work in, durable memory, and guardrails. Below is how each piece works, and some of what broke while we built it.


### Every session runs in its own disposable machine


When you send Carl a prompt, the control plane spins up a fresh Modal sandbox, an ephemeral microVM, for that session and nothing else, with a two-hour ceiling on the run. Inside it, a supervisor process boots the agent runtime (OpenCode), syncs repositories, and opens a bridge back to the control plane. Carl writes the code, runs the tests, and stages a pull request entirely inside that machine. When the session goes quiet for about ten minutes we snapshot the filesystem, uncommitted work and all, and tear the machine down, so we never pay to keep idle VMs warm. The next follow-up message restores the snapshot and skips the clone.


Session lifecycle


01 · Spawn


Fresh microVM


A Modal sandbox for this session only, with a two-hour ceiling.


02 · Boot


Runtime & repos


OpenCode boots, repositories sync, a bridge opens home.


03 · Work


Write, test, stage


Carl produces the change and the pull request entirely in-VM.


04 · Snapshot


Freeze at idle


About ten quiet minutes, then the filesystem is frozen and the machine torn down.


05 · Restore


Resume on reply


The next message restores the snapshot and skips the clone.


Idle VMs are never kept warm, so a paused session costs nothing.


Isolation is the point. Secrets arrive as environment variables only at spawn, with a precedence rule that lets a system variable override a user one but never the reverse, so a repository secret can never shadow the session’s own identity. The model keys are injected live and never written into a snapshot. Even the shell is fenced: Carl cannot open a pull request by hand, it has to go through one audited tool path, so every PR runs through the same reviewed gate. Pull requests that fundamentally change an aspect of a feature get labeled “Needs PM review”, others simply “bug fix”.


Our first attempt at fast cold starts taught us something. We pre-baked repositories into the machine image with shallow clones to save time, and it worked until the agent tried to push a branch. Pushes failed intermittently with` did not receive expected object` , and the obvious fixes did not hold. A shallow clone leaves the local object store incomplete in a way that quietly defeats Git’s pack negotiation on push. The real fix was a full clone in the prebake and a` git repack` to rebuild pack state. A speed optimization had been quietly breaking the one thing the agent existed to do.


### The control plane is a swarm of single-session actors


Each session is its own Durable Object on Cloudflare’s edge, with its own embedded SQLite database. That makes a session a single-writer actor that owns its state and its live connections, so hundreds can run at once without contending on a shared database. Because these objects hibernate when idle and lose their in-memory state, every fact that matters lives in SQLite, and the live connections are rebuilt from the platform on wake.


Pure functions decide the whole lifecycle: when to spawn a machine, when to restore a snapshot, when to call a session idle. They take the current state and the clock and return a decision, so we can unit-test the policy without standing up any infrastructure. The same control plane decides which model to run, too: a cheap classifier sends a quick question to a smaller model and a code change to the strongest one, a split we designed to cut chat-session cost by about a quarter.


Little of this was the first cut. A 2,800-line hand-rolled router was migrated onto a framework and split into domain modules, and a pair of thousand-line-plus files were carved into focused services as the system grew.


Sessions, live


**0** writing now


Each cell is one session: its own machine, its own SQLite, a single writer. Hundreds run at once, and the idle ones hibernate until the next message.


Two failures shaped this more than any design doc. A realtime interface invites you to refetch on every event, and ours did, with no debounce, until a single active session was firing thousands of requests a minute at our own API and tripping our host’s abuse protection against us. A separate state mismatch once drove one client into a twenty-four-thousand-request retry storm. The fix was unglamorous and correct: collapse bursts of events into one refetch, and hold data a little longer before calling it stale.


We also learned that idle is a slippery word for an agent. Our first inactivity timer measured wall-clock silence and cheerfully killed sandboxes in the middle of a long task, because an agent thinking hard looks exactly like an agent doing nothing.


### What Carl is allowed to touch


Carl cannot call a task done until the work holds together. Shipping is gated: the tests, the type checker, and the linter all have to pass inside the sandbox, and Carl has to run a structured self-review across five lenses: standards, bug-catching, the change’s history, security and data-flow, and the ways code fails silently. It scores each finding from 0 to 100 and drops anything under 80. Only then can it stage the result.


Shipping is gated


Gate 01 · Build


Tests, type checker, and linter all pass inside the sandbox.


Gate 02 · Self-review


Five lenses: standards, bug-catching, history, security and data-flow, and silent failures.


Each finding is scored 0 to 100. Anything under 80 is dropped.


Gate 03 · Draft


A draft pull request opens, bot-authored, with the person who asked as co-author.


Gate 04 · Human


A person publishes and merges. Carl can stage, never ship.


The result is always a draft. Every Carl pull request opens in draft, authored by a dedicated bot identity, with the person who asked attributed as co-author, and a human publishes and merges it. Carl also knows the edge of its own authority: a change that adds a subsystem or reaches across many parts of the codebase makes it stop and ask for a human owner rather than press ahead. And a short list of rules is welded into its prompt that Carl cannot edit: every query is scoped to a single organization, and credentials are never logged, committed, or repeated back.


### Review at volume


Carl reviews pull requests, its own and other people’s, and a review is its own small pipeline rather than a single prompt. It routes twenty-eight domain-specific review profiles by the files a change touches, sends a scout through to flag suspect spots, then has reviewers verify each candidate with a quoted line of code as evidence. Findings come back through one structured tool call, never parsed out of free text, and a single review is capped at fifteen inline comments, so it stays focused instead of burying the author.


The last stage is where most of the noise gets cut. It re-reads only the flagged code, ignores the author’s framing, and actively tries to disprove the bug, then runs every survivor through a rubric that drops anything a linter would already catch, anything generic, and anything it cannot pin to a specific line. The bar is precision over recall: a reviewer that cries wolf gets muted, so Carl would rather stay silent than add noise. When two reviewers disagree, the more cautious severity stands until the mechanism is genuinely disproved, and “the description says it is fine” does not count.


Review · precision over recall


01 · Route


28 profiles


Domain reviewers picked by the files a change touches.


02 · Scout


Flag suspects


A first pass marks the spots worth a closer look.


03 · Verify


Quote the evidence


Every candidate needs a specific line of code to survive.


04 · Cut


≤ 15 comments


A rubric drops the generic and the unpinnable; the rest of the noise stays silent.


Two choices keep it honest. Carl’s own pull requests are reviewed by a model from a different provider, because a reviewer from a different lineage catches what a sibling model waves through. And the reviewer that runs in the open is comment-only: it can flag a problem or request a change, but it can never approve a teammate’s code. The bot gets a voice in review, but not the final say.


### Skills that get sharper over time


Carl does not keep its know-how in one giant prompt. Everything Carl knows how to do is a self-contained skill: shipping a change, reviewing a pull request, triaging the day’s quality reports, cutting a release, writing the release notes. Each one is a short playbook plus the references it needs for that single job. The skills live in a shared library, and every sandbox pulls the current, pinned set on startup and installs it, so each session begins with the same vetted toolkit instead of whatever the model happened to remember.


Packaging capability this way means the library improves without touching the agent. The review pipeline above started life as a single prompt and grew, over many iterations, into the staged reviewer with profiles for twenty-eight kinds of change, and every session since has picked up the sharper version automatically. The skills that anyone can run are also deliberately defanged: our build refuses to ship the public review core unless it provably cannot reach production data or write anything on its own, so a shared skill stays limited to reading and commenting.


### Work that runs on its own


Not all of Carl’s work starts with a person. A lot of it is scheduled or triggered. Underneath, a recurring job is just a row in a table: a schedule, a prompt, a model, and the repositories it runs against. A single platform timer wakes every fifteen minutes, finds the routines that are due, and starts a session for each, the same kind of session a human would start, with no human in the loop. Because a routine is data rather than code, someone can add or change an automation without shipping a deploy.


That substrate runs a lot of Nash’s quiet maintenance:


- A **daily quality routine** reads the morning’s report, cross-references it against open and recently merged pull requests, pulls the matching error traces, and proposes the one or two fixes most worth doing.
- Another keeps **dependencies current** .
- Another **rotates reviewers** onto every open pull request and posts the day’s review queue.


Carl also tends its own work:


- When one of its pull requests **fails CI** , an event spins up a session seeded with the failing output to fix it, bounded so it tries a few times and then asks for help rather than running forever.
- When a human **requests changes** , a session works through each comment and replies on the thread.
- About an hour after a pull request opens, the **cross-provider review** runs on its own.


The pattern we like most is the one where Carl improves Carl. A scheduled routine reads recent activity and posts a ranked list of improvement ideas to a channel; react to one with a number, and that reaction kicks off a build session to go do it. The backlog grooms itself, and the next improvement is one emoji away.


### A couple more things that broke


Two more failures are worth keeping, for the principles they left behind:


- We hand-rolled the scheduler at first, and a small parsing gap meant an hourly job quietly ran once a day while fixed-time jobs drifted off their hour. **The lesson is an old one: do not hand-roll cron.**
- The cheap model we use to guess which repository a request means kept returning the right answer wrapped in a slightly malformed field, which our strict parser threw away whole, forcing someone to re-pick a repository they had just named. **Be strict about the one field that carries the decision, forgiving about the rest** , and when you genuinely have no signal, widen the options instead of narrowing them.


None of these pieces is the story on its own; they compound. A sharper skill lifts every session that uses it, a routine puts that skill to work on a schedule, and Carl’s own reviews and self-improvement routines fold each lesson back into the library. The more we build with this tooling, the more it improves on its own, and each lesson we feed back makes the next session a little better than the last.


The person who understands a problem should be able to fix it. Give them a teammate that already knows the code, and the work stops waiting in a queue. We built less than we expected to, composing existing pieces instead of a platform from scratch, and spent our week on the parts that were ours.


Carl is not magic, and we would not trust a post that claimed otherwise. A little over half of what it proposes gets merged. The rest a human closes, redirects, or rewrites. It still needs good context, some “taste,” and it does its best work when the person asking knows what “done” looks like.


It also created new problems, which is how you know it mattered. When anyone can ship, the bottleneck moves to review, particularly plan review (did the agent ship the fix in the right way): holding the bar high on every pull request now takes more of our engineers’ attention than writing the fixes ever did. And when implementation gets cheap, the harder question is what to build. More of our energy now goes to product direction, planning, and vision, to deciding which problems are worth Carl’s reach, than to the mechanics of shipping. Those are better problems, and they are the ones we are working on next.


At Nash, everyone ships.
