---
schema_version: "1.0.0"
document_id: "15a1fcbe3eb3d57a40eafd6ff7cdbc32eabf9ba9cbc5b1bd138bcc6cfe7a1bf1"
company_key: "yc-tempo-2"
company: "Tempo"
source_id: "yc-tempo-2-news-import-0ac7902f4a3f"
canonical_url: "https://docs.tempo.new/changelog/insiders"
published_at: null
first_seen_at: "2026-08-10T03:56:47.119811+00:00"
fetched_at: "2026-08-10T03:56:48.804539+00:00"
content_hash: "sha256:2abb5e57732be67c596de755c6848b91a4eb239afccb9a8e552dc05557e1d38d"
---

# Insiders updates

Tempo Insiders builds ship ahead of the stable channel, so these notes are incremental: each entry covers only what landed since the previous Insiders build. Every stable release bundles the builds that preceded it — see[Product updates](https://docs.tempo.new/changelog) for the merged notes.


​


Workspaces


Canvas


Chat


Platform


August 10, 2026


**Clicking between workspaces no longer freezes the app for half a second.**


### ​


New


- Canvas: resize multiple selected elements proportionally at once.
- Hold Shift while dragging a canvas element to lock movement to one axis.
- Chat: links now show an affordance to open in your external browser.
- Confirm before quitting or reloading the app, with an option to reset from Settings.


### ​


Improved


- Switching workspaces is snappy again — the 500ms click freeze is gone.
- Design agents now narrate what they’re doing on the canvas.


### ​


Fixed


- PR badges in workspaces now always open in your external browser.
- Issues: the “Awaiting Response” status now shows on the issue card.
- Cmd+L now applies only to the focused browser tab.
- Home no longer shows workspace chrome, the count badge is correct, and already-completed setup steps are detected.
- The live canvas stays stable even when a probe is missed.
- Chat: fast mode now sticks for new chats.


This release is largely about responsiveness. Switching between workspaces used to lock up the app for about half a second on every click — that freeze is now gone, and canvas rescans are scoped to just the workspace that changed, so things stay quick.


The canvas picks up two editing improvements: you can resize a multi-selection proportionally, and holding Shift while dragging now constrains movement to a single axis. Design agents will also narrate their actions as they work on the canvas.


Smaller fixes round things out. PR badges and chat links now reliably open in your external browser, Cmd+L is scoped to the focused tab, issue cards show “Awaiting Response,” and Home drops the leftover workspace chrome while correcting its count badge and setup-step detection. You can now also confirm before quitting or reloading, and reset that prompt from Settings.


​


Canvas


Chat


Workspaces


August 7, 2026


**Canvas AI-mode gets a round of reliability fixes alongside a new DeepSeek chat model.**


### ​


New


- Added DeepSeek V4 Pro to the chat model catalog.


### ​


Improved


- Canvas AI mode: gestures coalesce, subtree scoping, and working-list controls behave more predictably.


### ​


Fixed


- Revealing layers no longer scrolls the entire workspace.
- Drags with no target in loop-rendered content now correctly enter AI mode.
- Panel sizes are contained again instead of overflowing.
- Style panel no longer emits Tailwind classes in projects that don’t use Tailwind.
- Dev server now runs managed commands under the project’s declared Node version.


This release focuses on making the **canvas** more dependable, especially in AI mode. Gestures now coalesce cleanly, edits stay scoped to the right subtree, and the working-list controls behave as expected. We also fixed a case where target-less drags in loop-rendered content failed to enter AI mode.


A few layout and workspace issues are resolved: revealing layers no longer scrolls the whole workspace, and panel sizes are properly contained again. The style panel now avoids emitting Tailwind classes in projects that aren’t set up for Tailwind.


On the chat side, **DeepSeek V4 Pro** is now available in the model catalog. The dev server also correctly uses each project’s declared Node version when running managed commands.


​


Platform


Issues


Canvas


Chat


August 7, 2026


**A new Home tab with a Getting Started checklist lands alongside global back/forward navigation.**


### ​


New


- Home tab with a Getting Started checklist that tracks setup progress.
- Global back/forward history in the app header.
- Pull requests and Lines changed metric tabs on the org Insights tab.
- Issues filter dropdown with search and creator/agent dimensions.
- Right-click menu on the chat’s linked-issue card.
- Open PR pills as in-app browser tabs in the right split.
- Links from a Browser tab now open in new tabs beside the current one.
- Local HTML files auto-reload in Browser tabs.


### ​


Improved


- Right-panel state now persists across Issues, Canvas, and Docs.
- Chats reopen instantly by rendering the latest messages first.
- Workspace Git status badges update live instead of every 30 seconds.
- Cleaner spacing around tool calls in AI chat.


### ​


Fixed


- Label-grouped boards no longer hide name-backed issues.
- Emoji corruption and “Server Error” bricking in doc and issue bodies.
- Issue bodies no longer get blank paragraphs written on open.
- Apple Silicon users get the correct notarized DMG instead of the Intel installer.
- Updater verifies the staged bundle’s signature before installing.
- Linked artifacts on web share pages are now clickable.
- Canvas previews now capture reliably when an agent finishes a session.
- Reloading a browser view shows a loading state instead of a black screen.
- Split-view divider drags keep working across webviews and iframes.
- Fuzzy search treats separators consistently for better matches.


This release opens with a new **Home tab** built around a Getting Started checklist, so setup progress is visible in one place — with completion reported directly by agents. Navigation also gets easier: the app header now has **global back/forward history** , and Browser tabs behave more like a real browser, opening links in adjacent tabs and auto-reloading local HTML.


**Issues** gains a redesigned filter dropdown with search and creator/agent dimensions, a right-click menu on linked-issue cards, and fixes for label-grouped boards that were hiding some issues. Editors no longer write stray blank paragraphs into issue bodies, and doc/issue content is protected from emoji corruption and “Server Error” states.


On the platform side, the macOS installer is now notarized and correctly targets Apple Silicon, and the updater verifies a staged bundle’s signature before installing. Shared web pages list linked artifacts that are actually clickable, and Git status badges update live rather than on a 30-second sweep.


**Canvas** and **Chat** get steadier: canvas previews capture reliably when agents finish, chats reopen instantly by rendering recent messages first, and split-view divider drags keep working across webviews and iframes.


​


Canvas


Chat


Agents


August 5, 2026


**A round of Canvas editing fixes and more reliable chat.**


### ​


Improved


- Codex-synthesized MCP tool-call approvals are accepted automatically.


### ​


Fixed


- Cmd+B bolds text again instead of toggling the sidebar when an editor is focused.
- Renaming a chat no longer applies to whichever chat you’re currently viewing.
- Queued chat messages now send once async mini-turns finish.
- ESC returns to the Select tool after arming a tool from a toolbar dropdown.
- Alignment-grid clicks in the style panel no longer overwrite the axis they show.
- Nested Effects color popovers stay open when you release a drag.
- Browser-tab canvas and visible-region element captures render correctly.
- Expo canvases render again via a Metro recovery adapter.


This release focuses on **Canvas** stability and everyday editing. ESC now reliably drops you back to the Select tool after picking a tool from a toolbar dropdown, alignment-grid clicks in the style panel stop rewriting the axis they display, and nested Effects color popovers stay open through a drag. Rendering also improved: browser-tab canvases and visible-region captures come through correctly, and Expo canvases load again.


For **Chat** , Cmd+B bolds text again when an editor is focused instead of toggling the sidebar, renaming a chat now targets the right chat rather than the one you’re viewing, and messages queued during async mini-turns send once those turns settle.


On the **Agents** side, MCP tool-call approvals synthesized by Codex are now accepted automatically, so those flows run without extra prompts.


​


Canvas


Chat


Agents


Issues


August 5, 2026


**Canvas AI Mode lets you stage edits on locked elements and apply them from chat.**


### ​


New


- Canvas AI Mode: stage edits on uneditable elements and apply them via chat.
- Agent work now shows up in the issue activity feed.
- Artifact links now point at a specific branch instead of a workspace.
- /audit-tdd is now a built-in slash command.
- Chat http(s) links open in a split-view Browser tab; Cmd+click opens the OS browser.
- Confirm before quitting or reloading, with a reset in Settings.
- Nav shortcut hints (Ctrl-N) return, revealed after a 1.5s Ctrl hold.


### ​


Improved


- Double-click a selection outline or handle to shrink it to hug its content.
- Browser tab now handles Cmd+R/W/T plus Chrome-style Cmd+L and Cmd+F.
- Agent templates updated to Software Factory guide v9.


### ​


Fixed


- AI chat sessions no longer go unresponsive: queued messages survive, late answers get a grace period, and live sessions aren’t replaced by dead ones.
- Remote Control: answering a chat question on one machine no longer freezes the other.
- Mirrored chat now shows the full turn instead of only the tail while in flight.
- Idle agent runs are no longer killed while still healthy, and assignments release when a run is truly abandoned.
- Remote workspaces no longer sink to the bottom and ignore sort order.
- Dimension labels stay readable on rotated or flipped selections.
- Shared fixed-size storyboards render at their canvas box, and screenshots no longer show another storyboard’s pixels.
- Pressing a storyboard body now interacts with its content instead of dragging the storyboard.
- Fill on the flex main axis now takes the leftover space.
- The outage toast no longer fires for users who are actually online.


The headline of this release is **Canvas AI Mode** . You can now stage edits on elements that aren’t directly editable and apply them through chat, closing the loop between the canvas and AI. Alongside it, canvas selections got more precise: double-click an outline or handle to hug content, dimension labels stay readable when you rotate or flip, and shared storyboards render and screenshot correctly.


AI chat reliability saw a major push. Sessions that used to go unresponsive now hold onto queued messages, give late answers a grace period, and never swap a live session for a dead one. Remote Control and mirrored chat also got fixes so answering a question on one machine no longer freezes another, and in-flight turns show in full.


Agents are more legible and more stable: their work now appears in the issue activity feed, artifact links point at a specific branch, healthy runs are no longer reaped, and abandoned runs release their assignments cleanly. Templates are updated to Software Factory guide v9.


Rounding things out, the Browser tab gains standard Chrome-style shortcuts, chat links open in split view, and the app now confirms before quitting or reloading (resettable in Settings).


​


Canvas


Chat


Platform


Agents


August 3, 2026


**Canvas reliability fixes plus new chat tab shortcuts and a restyled MCP settings tab.**


### ​


New


- Added keyboard shortcuts for opening new chat tabs.


### ​


Improved


- Restyled the MCP tab to match the settings page and design canvas.


### ​


Fixed


- Fixed stale storyboard previews caused by transport stalls and share re-measuring.
- Fixed hex color commits and shading fallback in the style panel.
- Cleared leftover insets when moving an element back into normal flow.
- Prevented canvas dev server hangs from silently failing.
- Stopped Windows native file-watcher crashes with polling and process isolation.
- The ‘Remote Control Enabled’ toggle now respects the remote-control flag.


This release focuses on canvas stability. Storyboard previews no longer go stale when the transport stalls, and shared items re-measure their intrinsic size correctly. The style panel also picks up several fixes: hex-mode color commits now apply reliably, shading falls back gracefully where the paint worklet isn’t available, and stale insets are cleared when you move an element back into normal flow.


Chat gains keyboard shortcuts for opening new tabs, and the MCP tab has been restyled to match the settings page and design canvas.


On the platform side, Windows users should see fewer crashes thanks to a polling default and supervised utility-process isolation for the native file watcher. Canvas dev server connections are also hardened against silent hangs, and the ‘Remote Control Enabled’ toggle now correctly respects its feature flag.


​


Canvas


Platform


July 31, 2026


**Canvas selection and effects fixes, plus more reliable remote control connections**


### ​


Improved


- Remote control connections are more reliable, with clearer error messages when they fail.


### ​


Fixed


- Collapsed elements now keep their selection color on the frame.
- New effects are added to the top of the effects stack.


This release focuses on polish across the canvas. Selecting a collapsed element now correctly keeps the selection color on its frame, so it’s easier to see what you’ve picked. When you add a new effect in the style panel, it now lands at the top of the effects stack instead of the bottom, matching what most people expect.


We also made remote control connections more dependable. The relay is now provisioned automatically, and when something does go wrong the failure is surfaced with a legible message instead of failing silently.


​


Platform


Canvas


Chat


Workspaces


July 31, 2026


**The free tier now runs on GPT-5.6 Luna with a spend cap instead of a prompt limit.**


### ​


New


- Free tier moves to GPT-5.6 Luna, capped on spend rather than number of prompts.
- Shift+A now preserves grid/wrap layout when converting in place.
- Added a light-mode pass across the BPM design system.


### ​


Improved


- Radius handles now appear near selection corners on canvas.
- Canvas type-checking runs off the main thread for a more responsive app.
- First turn in a workspace no longer depends on route, with view state kept per workspace.


### ​


Fixed


- Copy/paste of layers now works inside HTML storyboards.
- The justify-content control now always shows on flex containers.
- Browser element screenshots now capture at full fidelity.
- Canvas share links now unfurl as human-readable previews in Slack.
- The canvas homepage now re-scopes to the active org when you switch orgs.
- “New Canvas” now gives clear feedback on un-initialized projects.
- Long AI builds no longer look frozen during silent patch gaps.
- AI chat now recovers cleanly when a reconnect produces no turn.
- A hung auth refresh can no longer wedge the app.
- Failed language-server starts no longer crash the app.
- Quitting the app no longer surfaces a crash from open AI sessions.
- Crash evidence is now preserved across the restart a bug report requires.
- Starfield animation now runs at 60 Hz on app and sign-in screens.
- Removed the redundant status line under Get Started in onboarding.


The headline change is on billing: the free tier now runs on **GPT-5.6 Luna** and is limited by spend rather than a fixed prompt count, so you can work in the way that fits your usage.


Canvas gets several quality-of-life improvements. Radius handles now show up near selection corners,` Shift+A` keeps grid and wrap layout when converting in place, and copy/paste finally works for layers inside HTML storyboards. Type-checking now runs off the main thread, keeping the app responsive on larger projects.


AI chat and workspaces are more reliable. Long builds no longer appear frozen during silent patch gaps, sessions recover when a reconnect yields no turn, and the first turn in a workspace is now route-independent with view state tracked per workspace.


Platform stability work rounds out the release: a hung auth refresh can no longer wedge the app, failed language-server starts and app-quit no longer trigger crashes, and crash evidence now survives the restart needed to file a bug report. Slack link previews and a new light mode for the design system are also included.


​


Canvas


July 30, 2026


**Clicking a folder in All Canvases now opens it in a modal instead of drilling in place.**


### ​


Fixed


- All Canvases: clicking a folder opens the modal instead of drilling in place.


Opening folders from the All Canvases view now behaves as expected. Selecting a folder brings up the modal view rather than replacing the current view with an in-place drill-in, so you stay oriented and can browse folder contents without losing your place.


​


Agents


July 30, 2026


**Custom Agents are now org-scoped with Private/Public and edit-permission controls.**


### ​


New


- Custom Agents can be shared org-wide as Private/Public with View-only or Anyone-can-edit access.
- Issues: split display mode for triaging agent-generated PRs.
- Chat: toggle the sidebar with Cmd+B.


### ​


Improved


- Updater now offers a retry before falling back to a manual download.
- Shared links now name the browser tab after the shared artifact.
- Onboarding: the “Get Started” button shows a loading state.


### ​


Fixed


- Fixed “Update didn’t apply” errors and the broken manual-download link.
- Restored downloads and auto-updates that were failing due to a GitHub rate-limit outage.
- Canvas: drawn frames no longer render as 0x0 in newly created files.
- Canvas: undo history no longer breaks after landed writes.
- Canvas: storyboard failures now surface clearly instead of failing silently.
- Style panel: repo-image fill previews now resolve correctly.
- Landing page: scroll reveal animations now fire reliably in Firefox.


The headline change in this release is **org-scoped Custom Agents** . You can now share agents across your organization as either Private or Public, and choose whether others get View-only access or can edit them. Alongside this, Issues gains a **split display mode** built for triaging agent-generated PRs, and Chat picks up a **Cmd+B shortcut** to toggle the sidebar.


Updates are more reliable this round. We fixed the “Update didn’t apply” failures, repaired the manual-download link, and the updater now offers a **retry** before pushing you to a manual download. We also resolved a GitHub rate-limit outage that was blocking downloads and auto-updates entirely.


Canvas received several fixes: newly created files no longer show drawn frames as 0x0, undo history stays intact after landed writes, and storyboard failures now surface properly. Smaller polish includes shared links naming the browser tab after the artifact, a loading state on the onboarding “Get Started” button, correct repo-image fill previews in the style panel, and reliable scroll animations on the landing page in Firefox.


​


Agents


Chat


July 29, 2026


**Chat now shows rich agent cards and lets you open agents in tabs or split view.**


### ​


New


- Rich agent cards in chat, with the option to open agents in tabs or split view.


### ​


Fixed


- Codex Apps are pre-approved so plugin tools work in AI chat.
- Canvas no longer misclassifies aliases and builtins as npm dependencies.
- Canvas installs dependencies using the correct package manager.
- Small SVG shapes keep their intended dimensions on canvas.
- Multi-select drag groups stay rigid when dropped onto a frame.
- Transient registration hiccups no longer show up as render-host errors.
- You stay in settings after deleting a project.
- Issue create-modal now seeds the body without losing content, and the collab provider is StrictMode-safe.


Chat gets a meaningful upgrade this release. Agents now appear as rich cards, and you can open them in their own tab or in split view so you can work alongside them instead of scrolling through a single thread. AI chat plugin tools also work reliably now that Codex Apps are pre-approved.


Canvas received several fixes around dependencies and shapes. Dependency preflight no longer mistakes path aliases and builtins for npm packages, dependencies install with the correct package manager, and small SVG shapes keep their intended size. Multi-select drag groups also stay rigid when dropped onto a frame, and transient registration failures no longer surface as render-host errors.


Elsewhere, deleting a project now keeps you in settings instead of bouncing you elsewhere, and the issue create modal seeds its body without dropping content while staying stable under React StrictMode.


​


Agents


Issues


July 29, 2026


**Re-assigning an agent to an issue now continues its existing workspace.**


### ​


Fixed


- Re-assigning an agent to an issue continues its existing workspace instead of starting over.
- Restored the animated /book-form funnel on the landing page.


Agent hand-offs are smoother in this release. When you re-assign an agent to an issue, it now picks up its existing workspace and keeps its context, rather than starting fresh.


We also fixed the landing page, restoring the animated **/book-form** funnel that had stopped working.


​


Canvas


July 28, 2026


**HTML storyboards are editable again, and canvases no longer disappear from the homepage.**


### ​


New


- HTML storyboards support editing again, with flat-JSX capture retained.
- Kimi K3 now runs directly on Tempo Inference in AI chat.
- Delete is now available in the canvas selection context menu.


### ​


Improved


- Homepage and workspace loading is faster with fewer git-related stalls.


### ​


Fixed


- Canvases no longer disappear from the homepage.
- Agent routing now works to a teammate’s agent.
- Agent route selection is no longer wiped.
- “Fix with AI” now pre-fills the chat composer.
- AI chat now sends the queued message on the async settle path.
- AI tool calls now use the workspace’s org instead of the window’s active org.
- Wrapper components now stay visible in the layer tree.


HTML storyboards are back: you can edit them again while flat-JSX capture keeps working as before. On the homepage, a rebuilt canvas index means canvases stop vanishing, and the list loads noticeably faster after cutting the underlying index from roughly 29,500 rows to under 600.


Agent workflows got more reliable. Routing work to a teammate’s agent now succeeds, agent route selections are no longer wiped out, and AI tool calls correctly use the workspace’s org. AI chat now delivers queued messages even on the async settle path, and “Fix with AI” properly pre-fills the composer.


Canvas editing picks up a delete action in the selection context menu, wrapper components stay put in the layer tree, and Kimi K3 is now available directly through Tempo Inference. Under the hood, several performance fixes reduce git-related freezes and spawn storms, making the homepage and workspaces feel smoother.


​


Canvas


July 28, 2026


**A new all-canvases homepage lands alongside live component previews in the Assets tab.**


### ​


New


- All-canvases homepage replaces the old per-workspace view.
- Assets tab now shows live component previews with add-to-canvas.
- Chat file links are now clickable.
- Agents show Runs-on as a Properties row with per-agent Remote Control opt-in.
- Font-size shortcuts: Cmd/Ctrl+Shift+. and +,
- File tree no longer auto-expands and now shows gitignored entries.
- Landing page redesigned around a software-factory focus.


### ​


Improved


- Faster panning on large canvases.
- General canvas rendering and interaction performance.


### ​


Fixed


- Storyboard moves no longer revert after being placed.
- Multi-element and multiselect drags now commit atomically and snap as a group.
- Same-named canvases no longer collide, now addressed by path.
- Fixed several canvas loading hangs and broken storyboard loads.
- No more default-sized outline flash when starting a draw gesture.
- Fast release-while-moving draws now commit instead of cancelling.
- Alignment snapping while drawing storyboards and frames.
- Canvas previews now fetch by rendered path for accuracy.
- Design context sidebar now matches the chat sidebar.
- Slack: unsubscribing from a thread no longer mutes the whole channel.


This release centers on the canvas. The workspace now has a single **all-canvases homepage** built on the designer’s own code, replacing the older per-workspace view, and the **Assets tab** gains live component previews you can drop straight onto the canvas.


Editing is steadier: storyboard moves no longer revert, multi-element drags commit atomically and snap as a group, and same-named canvases no longer collide. We also fixed several loading hangs, the draw-gesture outline flash, and alignment snapping while drawing.


Beyond the canvas, chat file links are now clickable, agents expose a Runs-on property with per-agent Remote Control, and the file tree stops auto-expanding while surfacing gitignored entries. New font-size shortcuts (Cmd/Ctrl+Shift+. and +,) speed up text edits, and a Slack fix ensures unsubscribing from a thread no longer mutes an entire channel.


​


Workspaces


Platform


July 25, 2026


**Remote Control lets you run workspaces on another machine and mirror and control them from this one.**


### ​


New


- Remote Control: run workspaces on another machine, then mirror and control them here.
- Opus 5 is now available and set as the default model (Opus 5 1M).
- Selection-aware undo/redo on the canvas.
- Portal content is now editable in projected storyboards.
- Custom Agent template MCP tools.
- Redesigned issue and doc share pages (Notion-style, mobile + desktop).
- Mobile layout for the canvas share page.
- Keyboard-shortcut hints and press-to-flash on the canvas right-click menu.
- Added a product docs page for the Agents feature.


### ​


Improved


- Issue filter dropdown rebuilt on the design system dropdown.
- Style panel Tailwind class and color search now ignores separators.
- Faster canvas homepage source-repo sync that keeps local state on-device.
- Removed the close warning on share pages.
- Click the branch name in the app header to copy it.


### ​


Fixed


- Pinned workspaces keep a stable order and drag/drop lands on the target.
- Interrupted chats are no longer counted as running agents.
- Projected storyboards no longer lose their head.
- Share page shows a loading state instead of an error while the canvas loads.
- Canvas chat session dropdown now scrolls with a visible scrollbar.
- Toggling Tempo Inference now tears down the running server.
- Diagnostics export now keeps a chat tab’s prior-session events.


The headline of this release is **Remote Control** . You can now run workspaces on a separate machine and mirror and control them from your current one, so heavier or remote setups stay fully usable from wherever you’re working.


On the model side, **Opus 5** is now available and set as the default (Opus 5 1M). Sonnet 4.6 has been retired. Canvas also gets meaningful upgrades: **selection-aware undo/redo** , editable portal content in projected storyboards, and keyboard-shortcut hints on the right-click menu.


Sharing is cleaner too, with a Notion-style redesign of the issue and doc share pages across mobile and desktop, a mobile layout for canvas share pages, and a loading state instead of an error while a canvas loads.


Rounding things out are fixes to pinned workspace ordering and drag/drop, more accurate running-agent counts, branch-name copy from the app header, and improved Tailwind class and color search in the style panel.


​


Workspaces


Canvas


July 23, 2026


**Tempo now includes an embedded browser with tabs and element context.**


### ​


New


- Embedded browser tabs with element context for AI chat
- Tempo Web Clipper: capture any web page as an HTML storyboard
- Canvas comments now render image attachments
- Renaming a chat can also rename its workspace branch
- Chat-view center tabs stay mounted when switching tabs
- OpenCode model picker now offers Kimi K3 (replacing K2.6)
- Design-system generation adds build-mode choice and app shell


### ​


Improved


- Auto-update now surfaces stuck or failed updates instead of failing silently
- Canvas: wrap selections that span multiple parents
- Faster background git/GitHub work with capped concurrency


### ​


Fixed


- Opening an issue no longer auto-opens the AI chat panel
- Archive-toast Undo no longer races or fails silently
- Storyboard preview cache now survives switching workspaces
- Removed blank-frame flash when undoing/redoing auto-layout wraps
- Hover and click now reach frames inside a selected storyboard
- Storyboard drags no longer snap back on subframe focus steal
- Canvas paths containing spaces now work
- Fixed zombie Custom Agent runs after a failed submission
- Completed agent workspaces now route correctly
- Commit action now preserves the selected model
- Sub-chat split context is now preserved
- CLI return button stays above the terminal
- CLI mode reloads terminal history on close
- Child processes no longer show mac Dock icons or Windows consoles
- Free-tier quota limits now show in the banner instead of failing quietly
- Bug-report error text is now readable


This release brings a built-in **embedded browser** to Tempo, complete with tabs and element context that AI chat can reference — plus the **Tempo Web Clipper** , which captures any web page as an editable HTML storyboard. Canvas comments can now carry image attachments end to end.


Canvas gets several quality improvements: you can wrap selections that span multiple parents, storyboard preview caches survive switching workspaces, and long-standing interaction issues are fixed — no more blank-frame flash on auto-layout undo/redo, drags snapping back on focus steal, or being unable to hover and click frames inside a selected storyboard. Paths with spaces now work too.


Chat and agents are more reliable: renaming a chat can also rename its workspace branch, center tabs stay mounted across switches, the commit action keeps your chosen model, and sub-chat split context is preserved. Zombie Custom Agent runs after failed submissions and mis-routed completed agent workspaces are both fixed. The OpenCode picker now offers **Kimi K3** .


Rounding things out, auto-update now clearly reports when it’s stuck or failed, quota limits surface in a banner, opening an issue no longer hijacks the AI chat panel, and background git/GitHub work runs faster with capped concurrency.


​


Canvas


July 17, 2026


**Canvas gains Shift+A full alignment inference and smarter spacing for overlapping stacks.**


### ​


New


- Canvas: Shift+A infers and applies full alignment across selected elements.
- Canvas: negative gaps are inferred for uniform overlapping stacks.


### ​


Fixed


- Canvas: you can now edit text nodes whose formatted content includes line breaks.
- Canvas: undo now survives file drift instead of failing.
- Canvas: portal-child layers are no longer pruned during live updates.
- AI chat: interrupting a response now cold-resumes cleanly instead of reusing a flaky process.
- Style panel: multi-element edits now commit or cancel as one atomic change.
- Workspaces: chat continuity is preserved across archive and restore.
- Auth: the transient reconnect banner no longer intercepts clicks.
- Auth: sessions recover more reliably after a refresh or auth failure.
- Windows: no more false “Git is required” errors on slow-to-start machines.


This release focuses on the canvas. **Shift+A** now infers and applies full alignment across your selection in one step, and overlapping stacks with uniform spacing correctly infer negative gaps. Editing is more reliable too: text nodes containing line breaks are now editable, undo holds up even when the underlying file has drifted, and portal-child layers stay put during live updates.


AI chat handles interruptions better — stopping a response now starts a clean session rather than reusing a subprocess that could hang. The style panel treats multi-element edits as a single atomic change, so a cancel no longer leaves things half-applied.


A batch of stability fixes rounds things out: chat history survives archiving and restoring a workspace, sign-in recovers more gracefully after a refresh or auth failure, the reconnect banner no longer swallows clicks, and Windows users won’t hit a false “Git is required” block on slower machines.


​


Agents


Platform


July 16, 2026


**Fixed crashes affecting agent follow-ups and diagnostics.**


### ​


Fixed


- Prevented crashes during agent follow-ups and diagnostics.
- AI chat now keeps a stopped first message until a session is created.


This release focuses on stability for AI-powered workflows. We fixed crashes that could occur during agent follow-up actions and while diagnostics were running, so long or interrupted sessions should now hold up more reliably.


We also fixed an issue in AI chat where stopping your very first turn could lose that message. Your message is now preserved until a session exists, so nothing disappears when you start and pause a new conversation.


​


Agents


July 15, 2026


**Agent Templates gallery lands to end the Custom Agents cold start.**


### ​


New


- Agent Templates gallery to jump-start custom agents.
- GLM 5.2 free and paid combined into one quota-aware model picker row.
- Gradient and image fills now available on the bottom fill layer.
- Live-preview typography by hovering over font options.
- Pinned workspaces now grouped into a flat “Pinned” section.


### ​


Improved


- Archived workspaces no longer background-track git/PR status.
- Per-workspace git ahead/behind checks now run off the main thread for a smoother UI.


### ​


Fixed


- Create agents instantly — fixes infinite-loading Slack trigger.
- Fixed canvas selection, copy, text-formatting, and transform interactions.
- Dragging fill alpha to 0 no longer deletes the fill row.
- Share/preview captures no longer silently save partial results.
- You can now share canvases nested at any depth under the canvases root.
- Share-preview comment pins now stay synced with canvas pan and zoom.
- AI chat keeps the interrupted notice in history and clears the sidebar dot on resume.
- Streaming indicator now anchors to the transcript tail instead of the last assistant reply.
- Clipped out-of-flow content no longer inflates storyboard autosize.
- Recover workspaces that got stranded in “materializing” by orphaned directories.
- Handle missing workspace paths gracefully at startup.


This release makes agents much easier to start with. The new **Agent Templates gallery** removes the blank-slate problem for custom agents, and agents are now created instantly instead of opening a draft view — which also fixes the infinite-loading Slack trigger.


Canvas and the style panel get a round of attention. You can now add **gradient and image fills** on the bottom fill layer, and dragging a fill’s alpha to 0 no longer deletes the row. Hovering over font options gives you a **live typography preview** . Sharing is more reliable too: canvases nested at any depth can be shared, captures never save silently partial results, and share-preview comment pins stay locked to the canvas as you pan and zoom.


Workspaces are more resilient and responsive. Workspaces stranded in “materializing” by orphaned directories can now recover, missing workspace paths are handled at startup, and git status tracking runs off the main thread — with archived workspaces skipped entirely — for a smoother overall experience.


AI chat also improves: the interrupted notice now persists in history and the sidebar dot clears on resume, while the streaming indicator anchors to the end of the transcript.


​


Chat


Platform


July 15, 2026


**Custom artifacts and inline pills now open in split view instead of the browser.**


### ​


New


- Search issues by their human-readable issue ID.
- Open custom artifacts and inline artifact pills in split view.
- Add “Copy as Markdown” to the authenticated member doc-share view.
- Cmd+R reloads the open canvas instead of the whole page.
- Manual reload button added to canvas toolbars.
- Issue detail actions menu now includes Create workspace and Copy ID.
- Shift+A infers flex-wrap for multi-row arrangements on canvas.
- Queued agent runs are now visible when you hit the concurrency cap.
- One-click workspace archive snapshots uncommitted work before cleanup.


### ​


Improved


- AI can now find and list design-system assets when you ask about your design system.
- Reuse an idle AI terminal for repeated AI-run commands.
- Faster app response by cutting full-tree re-renders from duplicate queries.
- Abandoned-run notices now render inline instead of taking over the whole area.


### ​


Fixed


- Auto storyboard/devserver diagnostic reports no longer attach your chat transcript.
- Stopped the connection dropping every ~5 minutes on token refresh.
- Fixed mid-session 401 errors on managed Tempo Inference.
- Split-view doc scroll position is preserved when switching workspaces.
- Assignee dropdown no longer shifts position while adding assignees.
- Diff pane no longer shows a branch-added file as a full-file add in uncommitted mode.
- Opening a canvas card no longer hijacks the chat into the right split.
- Canvas Open now shows a loading state and can open linked canvases from share view.
- Freeform canvases no longer land in the wrong place on empty legacy roots.
- Selection-overlay chrome stays rigid under skew and scale transforms.
- Radius corner is chosen by drag direction when handles stack at max.
- Recovered and surfaced dead-route storyboard live sessions.
- Cleared stale devserver hints and lost host-registration hangs on canvas.
- Restored GPT-5.6 workspace naming and kept acknowledged follow-ups alive.
- Hid the cross-provider “open in new” arrow on the draft model picker.
- Org-scoped views re-scope on org switch instead of flashing the previous org.
- Outer app window no longer scrolls out of the viewport.
- Clearer error when a bundled Claude binary won’t launch.
- Fixed crashes from null-path workspaces and diagnostics-dir startup failures.


This release leans into a smoother split-view workflow. Custom artifacts and inline artifact pills now open directly in split view instead of bouncing you out to the browser, and split-view docs keep their scroll position when you switch workspaces.


Canvas gets a batch of practical improvements: **Cmd+R** reloads the open canvas (with a new manual reload button in the toolbar too), **Shift+A** now infers flex-wrap for multi-row arrangements, selection chrome stays rigid under skew and scale, and several placement, loading-state, and stuck-session issues are fixed.


Issues and agents are more workable: search by human-readable issue ID, use **Create workspace** and **Copy ID** from the issue detail menu, and see agent runs that are queued behind the concurrency cap. The assignee dropdown also stays put while you add people.


Stability and privacy see real fixes: the connection no longer flaps every few minutes on token refresh, managed Tempo Inference stops throwing mid-session 401s, and automatic storyboard/devserver diagnostic reports no longer include your chat transcript. AI can now find and list your design-system assets when you ask about them, and general responsiveness improves thanks to fewer redundant re-renders.


​


Platform


July 14, 2026


**Fixed app freezes, idle CPU churn, and renderer crashes for a smoother, more stable workspace.**


### ​


New


- Canvas: Shift+A infers a CSS grid for clean 2D arrangements.


### ​


Improved


- Canvas: Shift+A keeps outlier children absolutely positioned.
- Workspace asset index is now limited to design-system declarations.


### ​


Fixed


- App no longer freezes for 1-2s, churns CPU while idle, or bloats renderer memory.
- Renderer no longer hard-crashes when nearing its memory limit.
- Abandoned agent runs now show in their chat instead of spinning forever.
- Setup-script terminals now run in their own workspace.
- AI chat now warns when /compact does nothing.


This release focuses on stability and responsiveness. We tracked down and fixed the 1-2 second app freezes, idle CPU churn, and renderer memory bloat that many of you were hitting during longer sessions. The renderer also no longer hard-crashes when it approaches its memory limit, so heavy workspaces should stay up.


On the canvas, **Shift+A** got smarter: it now infers a CSS grid for clean two-dimensional layouts, while keeping outlier children absolutely positioned so nothing jumps out of place.


We also cleaned up a few rough edges. Abandoned agent runs now report back in their own chat instead of leaving an infinite spinner, setup-script terminals run in their own workspace, and AI chat warns you when` /compact` has nothing to do rather than silently no-op’ing. Finally, the workspace asset index is now scoped to design-system declarations.


​


Workspaces


July 13, 2026


**Archive workspaces to free up worktrees while keeping the branch.**


### ​


New


- Archive a workspace to remove its worktree while keeping the branch.
- Shift+A converts a selected container to Auto Layout in place.
- Option+Shift+A removes Auto Layout from a container.
- Admin coupon-code dashboard at auth.tempo.build/admin/coupons.


### ​


Improved


- Auto Layout wrap now sorts children into visual flow order.
- Auto Layout infers per-side padding from children edge offsets.


### ​


Fixed


- Restored the Codex CLI slash-command flow on Codex 0.144.1 + GPT-5.6.
- Subagent progress now renders correctly from the opencode 1.17.13 wire.
- MCP tools fall back to unpinned resolution across docs, comments, agents, and scripts.


This release adds **workspace archiving** : you can now archive a workspace to remove its worktree and reclaim space while preserving the underlying branch, so nothing is lost when you step away from a piece of work.


Auto Layout on the canvas gets faster and more predictable. **Shift+A** converts a selected container into an Auto Layout frame in place, and **Option+Shift+A** removes it again. Wrapping now reorders children into their visual flow, and padding is inferred per-side from where children actually sit against the edges.


On the AI side, the Codex CLI slash-command flow works again on Codex 0.144.1 with GPT-5.6, and subagent progress renders correctly against the latest opencode wire format. MCP tools also now fall back to unpinned resolution across docs, comments, agents, and scripts. Admins get a new coupon-code dashboard at` auth.tempo.build/admin/coupons` .


​


Platform


July 13, 2026


**Windows users no longer see stray console windows pop up while Tempo runs.**


### ​


Fixed


- Hidden background console windows that could flash open on Windows.


On Windows, Tempo spawns helper processes to keep everything running smoothly. Previously these could briefly open visible console windows, cluttering your screen and taskbar.


Those windows are now suppressed, so background work stays out of your way and the app feels cleaner during normal use.


​


Chat


July 10, 2026


**AI chat gains the GPT-5.6 (Sol/Terra/Luna) Codex models and retires GPT-5.4 Codex.**


### ​


New


- AI chat adds GPT-5.6 Sol, Terra, and Luna Codex models; GPT-5.4 Codex is sunset.
- New design-system slash commands plus an example buttons canvas.


### ​


Improved


- AI chat streaming is smoother with an event-driven, frame-coalesced pump.
- Billing retries Stripe checkout actions after a dropped connection, with no duplicate toast.
- Per-model credit toggles now respect the master Tempo Inference switch.
- Provider request failures show a specific error tag instead of a generic “unknown”.
- “Publish failed” toasts now include the underlying git error.


### ​


Fixed


- Canvas comments now land correctly when tapped on mobile Safari.
- Issue boards no longer crash from a stale saved organization id.
- Editing an issue with a malformed label id no longer crashes.
- Moving issue groups tolerates a missing assignee or label set.
- Canvas now renders all visible route storyboards, not just one.
- Drawing over a text storyboard creates a sibling instead of overwriting.
- Duplicated canvas previews keep the copy’s annotations.
- Text being edited on canvas no longer shows an unwanted outline.
- Doc suggestions resolve against the current document, not a stale state.
- The “Comment resolved” toast now dismisses on its own.
- “Session expired” only appears once connectivity is confirmed.
- Agents view shows a loading state instead of “No agents yet” while loading.
- Toasted background actions no longer crash the app.
- Bug report screen recording asks for permission up front to avoid a capture leak.
- AI can reference issues by a bare number (e.g. “932”).


This release brings the **GPT-5.6 Codex** family to AI chat: the Sol, Terra, and Luna variants are now available, while the older GPT-5.4 Codex model has been retired. AI chat also streams more smoothly thanks to an event-driven pump, and provider errors now report a specific cause instead of a generic “unknown” message.


Canvas gets several fixes: all visible route storyboards render, drawing over a text storyboard creates a sibling rather than overwriting it, duplicated previews keep their annotations, and text being edited no longer shows a stray outline. On the design side, a new slash-command suite and an example buttons canvas make it easier to work with your design system.


Issues and docs are more resilient — boards no longer crash on stale saved state or malformed label ids, group moves tolerate missing assignees or labels, and doc suggestions apply against the current document. Billing now retries Stripe checkout actions after a dropped connection without duplicate toasts, and per-model credit toggles respect the master Tempo Inference switch. Rounding things out: canvas comments work on mobile Safari, the “Comment resolved” toast auto-dismisses, “Session expired” waits for confirmed connectivity, and background actions no longer crash the app.


​


Agents


July 10, 2026


**Stuck agent runs now recover automatically instead of hanging.**


### ​


Fixed


- Agent runs whose turn never started now self-heal and retry instead of stalling.


Agent runs that got stuck before their turn was dispatched would previously hang indefinitely. A new startup watchdog detects these stalled runs and automatically retries them, so work continues without manual intervention.


If you’ve hit agents that seemed to freeze right after starting, they should now recover on their own.


​


Chat


Canvas


Workspaces


Agents


July 9, 2026


**Fixes across Slack, AI chat, canvas, and workspaces plus lower per-visit memory use.**


### ​


Improved


- Slack agent posts are now titled “{User}‘s {Agent name}”.
- Workspaces use less memory by no longer keeping a file-tree copy per visit.


### ​


Fixed


- Split-view ”+” now opens the new-tab dropdown instead of the old overlay.
- Shared view.tempo.new canvases stay interactive.
- Duplicate canvas comment popovers no longer appear.
- The active canvas comment toolbar toggle works correctly.
- Slack DM replies to stale installs are flagged to reconnect instead of dropped.
- AI chat no longer gets stuck loading when a session ends mid-stream.
- AI chat mini-turns now finalize with a correct completion status.
- Issue↔workspace links survive branch renames.
- Kanban cards drag correctly inside scrolled virtualized columns.


This release focuses on stability across the app. In **Slack** , agent posts are now clearly titled “{User}‘s {Agent name}”, and DM replies to stale installations are flagged for reconnection instead of being silently dropped.


**Canvas** gets several fixes: shared` view.tempo.new` canvases stay interactive, duplicate comment popovers are gone, and the active comment toolbar toggle behaves correctly.


**AI chat** no longer hangs in a loading state when a session ends mid-stream, and mini-turns now finalize with an accurate completion status.


For **workspaces** , the split-view ”+” opens the new-tab dropdown, issue-to-workspace links survive branch renames, and memory use is lower now that a full file-tree copy is no longer kept per visit. Kanban cards also drag correctly inside scrolled, virtualized columns.


​


Platform


July 9, 2026


**You can now set granular, per-model spending controls for Tempo Inference credits.**


### ​


New


- Set per-model credit controls for Tempo Inference.
- Subscribe/unsubscribe to Slack threads from agents across all providers.


### ​


Improved


- Draw tools now emit Tailwind classes so previews match your project styling.
- Faster board loads via virtualized kanban swimlane rows.
- Unified agent chat context menus.
- Local workspace canvases render immediately without waiting on the cloud index.
- Clearer Slack trigger connection state.


### ​


Fixed


- Board edits now update the UI instantly.
- Agent create flow shows the real GitHub/webhook URL and secret.
- Canvas comment popover stays anchored in split view and on tab switch.
- Copy usable canvas comment thread links.
- Storyboard previews render with the project’s global CSS.
- Drawn-primitive storyboards are labeled after their root element.
- Overflowing text stays visible in intrinsic text storyboards.
- Preview auto-capture no longer thrashes the live render pool.
- Promoted workspace chat is now protected from archiving.
- Removed duplicate context chip tooltips.
- MCP status converges correctly after CLI reauth.
- Hardened built-in MCP tools and improved connectivity probing.
- Resolved workspace branch PR collision allocation.
- On Windows, git is resolved on PATH with a clear error when missing.
- Improved app stability around window menus and shutdown edge cases.


This release adds **per-model controls for Tempo Inference credits** , so you can decide exactly how spend is allocated across models instead of managing a single pooled limit.


Canvas gets a lot of attention. Draw tools now emit Tailwind classes, and drawn or pasted storyboard previews render with your project’s global CSS, so what you see on the canvas matches your real styling. Comment popovers stay anchored correctly in split view and when switching tabs, thread links are now copyable, and local workspace canvases load right away without waiting on the cloud index.


Issues feel snappier: board edits update the UI instantly, and virtualized kanban swimlanes speed up board loading. Agents can now subscribe and unsubscribe to Slack threads across all providers, and the create flow shows the real GitHub/webhook URL and secret.


A range of fixes improve day-to-day reliability, including steadier MCP status after CLI reauth, better connectivity probing, cleaner git handling on Windows, and general app stability improvements.


​


Platform


July 9, 2026


**Fixed a crash that prevented the packaged desktop app from starting.**


### ​


Fixed


- Desktop app no longer crashes on launch due to a missing dependency.


This release fixes a startup crash in the packaged desktop app. A required dependency (` css-tree` ) wasn’t being bundled, causing the main process to crash before the app could open.


If you were unable to launch Tempo after installing a recent build, updating to this version should resolve the issue.


​


Platform


July 9, 2026


​


Platform


Issues


July 9, 2026


**Share links now use human-readable, Linear-style URLs.**


### ​


New


- Share issues with readable URLs based on your org slug.
- Browse and edit a project’s CSS custom properties from the canvas.
- Manage files in the Design tab’s canvases tree.
- Collapsible date-group headers in the chat workspace list.
- Issue context cards are now collapsible and remember their state.
- Agent runs and new workspaces auto-link to tickets with live loading indicators.
- First-run empty state added for Agents.
- Tempo Inference now works from the CLI via the chat-mode server.


### ​


Improved


- Renamed “Custom Agents” to “Agents” across the UI.
- Restored the “What are Agents?” help dialog.


### ​


Fixed


- Slack agents no longer self-trigger on their own MCP posts.
- Issue, doc, and canvas context pills now open as center tabs on the agents route.
- Fixed an app crash caused by the collapsed tab rail.
- Blocking questions are no longer revoked by in-turn subagent activity.
- Agents MCP now loads reliably on cold-start sessions.
- Doc body reflows in narrow/split view instead of clipping.
- Custom stage titles now display correctly on the issue share page.
- Touch pan and pinch-zoom now work on the mobile share preview.


Sharing gets a big upgrade: issue share links are now human-readable, Linear-style URLs built from your org slug instead of opaque IDs. The mobile share preview also supports touch pan and pinch-zoom, and shared issues now show the correct custom stage titles.


Canvas gains new depth. You can browse and edit a project’s CSS custom properties directly from the canvas, and the Design tab’s canvases tree now supports full file management.


Issues and chat feel more organized. Context cards are collapsible with persisted state, agent runs and new workspaces auto-link to tickets with live loading indicators, and the chat workspace list has collapsible date-group headers.


Agents are cleaner and more reliable: “Custom Agents” is now simply “Agents,” the “What are Agents?” help dialog is back, and there’s a proper first-run empty state. Under the hood, Slack agents no longer self-trigger, the Agents MCP loads reliably on cold-start, and blocking questions survive subagent activity. Tempo Inference now also works from the CLI.


A handful of stability fixes round things out, including a resolved app crash from the collapsed tab rail and doc bodies that reflow instead of clip in narrow or split views.


​


Agents


Chat


July 8, 2026


**Slack bot posts now show who triggered them and long AI turns no longer drop out with 401s.**


### ​


Improved


- Slack bot posts are attributed to the triggering user (“Shippy (Name)”) with a composited avatar.
- First-party Tempo MCP tools are now approved and available in Codex.
- CLI TUI now shows the correct model and provider tier (Zen vs Go).
- Color picks now commit canonical —color-* variables in the style panel.


### ​


Fixed


- Long Tempo Inference turns in Codex no longer fail with 401 errors mid-run.
- Canvas rectangles keep a consistent stroke width when zooming and no longer clip.
- Canvas zoom readout has a fixed width so the pill stops shifting around.
- Tempo projects now heal unsatisfiable tempo-sdk version pins before install.


This release focuses on making AI and integrations more reliable. Long Tempo Inference turns in Codex previously risked cutting out with a 401 partway through; a token-refresh fix keeps those sessions alive. First-party Tempo MCP tools are now approved in Codex, and the CLI TUI correctly reflects the model and provider tier you’re on (Zen vs Go).


Slack integration got clearer: bot posts are now attributed to the person who triggered them, showing “Shippy (Name)” alongside a composited avatar so it’s obvious who kicked off an action.


On the canvas, drawn rectangles now hold a steady stroke width while zooming and won’t clip at their edges, and the zoom readout uses a fixed width so the pill no longer jitters. Color selections in the style panel commit canonical` --color-*` variables, and project setup can now heal unsatisfiable tempo-sdk pins before installing.


​


Chat


July 8, 2026


**You can now surface archived chats with a per-project Status filter.**


### ​


New


- Filter chats by status to view archived conversations per project.
- Set a per-account Figma personal access token to paste image fills.


### ​


Improved


- The /mcp slash command is now scoped to Claude, where it’s supported.


### ​


Fixed


- Long Tempo Inference turns in opencode no longer fail with 401 errors.
- Codex now recovers when resuming a session with no rollout found.
- Canvas undo now keeps DOM and layer state in sync.
- Legacy storyboard autosize now accounts for out-of-flow content.
- Resolved macOS launch issues loading the app and OpenTUI binaries.


Archived chats are now easier to find. A new per-project **Status** filter lets you surface archived conversations without digging, so nothing gets permanently buried once it leaves your active list.


For design work, you can now set a **per-account Figma personal access token** , which unblocks pasting image fills directly from Figma.


This release also focuses on AI chat reliability. Long Tempo Inference turns in opencode no longer time out with 401 errors, Codex gracefully falls back when a session can’t be resumed, and the` /mcp` command is now limited to Claude where it actually works.


On the canvas side, undo now keeps the DOM and layer state aligned, and legacy storyboard autosize correctly includes out-of-flow content. macOS users should also see fewer launch problems loading the app and its bundled TUI binaries.


​


Agents


July 8, 2026


**Slack integration gains full bot-capability parity with 13 tools, including DMing anyone.**


### ​


New


- Slack MCP now supports 13 tools, including DMing anyone via slack_send_dm.


### ​


Improved


- MCP-posted Slack threads stay in their workspace and route inbound DM replies.


### ​


Fixed


- Custom Agents actions column (settings/status/⋮) stays visible.


Slack support takes a big step forward with full bot-capability parity. You now get 13 tools, including the ability to DM anyone directly with` slack_send_dm` .


Agent conversations in Slack are more reliable too: threads posted via MCP continue in their original workspace, and inbound DM replies are routed correctly.


On the settings side, the Custom Agents actions column (settings, status, and the ⋮ menu) now stays visible so you can always reach those controls.


​


Canvas


July 8, 2026


**Canvas migration is more reliable and now recovers from page-config errors automatically.**


### ​


Fixed


- Legacy canvases migrate more reliably and self-heal from page-config errors.


Opening older canvases is now more dependable. We hardened the migration path for legacy canvases so they convert cleanly to the current format.


When a canvas hits a page-config error, Tempo now detects and repairs it automatically instead of leaving the canvas in a broken state, so you’re less likely to run into load failures on existing boards.


​


Agents


July 8, 2026


**Tempo agents now work end-to-end with Slack—posting as the Tempo bot, continuing threads in-workspace, and supporting multiple workspaces.**


### ​


New


- AI can post to Slack as the Tempo bot, with support for multiple Slack workspaces.
- Slack threads now continue in the same Tempo workspace via thread-aware routing.
- Daily designer-bug Slack report added.
- Pin workspaces to the top of the sidebar.
- Right-click menu on board column headers, Linear-style.
- Figma-parity marquee selection on the canvas.
- Bare issue references in chat autolink to clickable chips.


### ​


Improved


- Custom Agents tab: full page, click-to-sort, Last run column, and richer trigger cards.
- Create-flow trigger modal offers all issue filters and a real assignee list.
- Claude MCP servers get a reconnect/authenticate action and a scope badge.
- Help menu slimmed to Documentation and Community (Discord).
- Tailwind class editing moved into a dedicated “Classes” style-panel row.
- OpenCode skips permission prompts and surfaces subagent tool calls in the Agent accordion.


### ​


Fixed


- Cron trigger schedules now display in your local timezone.
- Private Slack channels now appear in the trigger picker, and the reconnect spinner clears.
- Slack file_share messages can now trigger custom agents.
- Hosted-artifact preview card stays visible in AI chat.
- Tempo Inference is no longer shown as active without an active plan or usable credits.
- Issue create-modal ”+” pre-selection no longer hits a race condition.
- OpenCode free-tier binding rebuilds on resume so glm-5.2-tempo-free resolves.


This release focuses heavily on connecting Tempo agents to Slack. The AI can now post to Slack as the Tempo bot, and integrations work across multiple Slack workspaces. Threads started in Slack continue in the same Tempo workspace thanks to thread-aware trigger routing, private channels show up in the trigger picker, and` file_share` messages can kick off custom agents. A new daily designer-bug Slack report is also included.


The Custom Agents experience got a broad polish: a full-page layout, click-to-sort columns, a Last run column, and richer trigger cards. The create-flow trigger modal now offers all issue filters and a real assignee list, and cron schedules display in your local timezone.


On the canvas, marquee selection now matches Figma’s behavior, with unified measurement typography and consistent guide colors. Elsewhere, you can pin workspaces to the top of the sidebar, right-click board column headers for a Linear-style menu, and bare issue references in chat turn into clickable chips.


Several fixes round things out, including a corrected Tempo Inference status that no longer appears active without a plan or credits, a race-free issue create modal, a persistent hosted-artifact preview card, and reconnect/scope improvements for Claude MCP servers.


​


Agents


Issues


July 7, 2026


**Agents can now fire on issue assignment with per-property filters on issue triggers.**


### ​


New


- New agent trigger: issue.assigned, plus per-property filters on issue triggers
- MCP tools to get share links for issues and docs (members only)
- Canvas keyboard shortcuts to reorder layers (Figma-style \[ \], Cmd+\[ \])
- Draggable gap handles on zero-gap seams in canvas
- Press I to activate the eyedropper while a color picker is open
- Bug reports now capture Issues/kanban board state
- Slack triggers can target private channels; broader Slack bot scopes


### ​


Improved


- Faster chat: hidden chat tabs no longer re-render on every token
- New Canvas opens instantly with a loading state and no spinner delay
- Hosted-artifact previews now embed inline in chat


### ​


Fixed


- Agent-created chats can now be renamed everywhere
- Kanban Stage picker options now flip correctly when Status filter is “is not”
- Cmd+Z right after a style change no longer loses the undo
- tempo:// deep-links work again and emoji survive markdown round-trips
- Canvas comments toolbar and pins now render in chat split view
- Fixed stuck snap guides and draw gestures swallowed near storyboard chrome
- Enter now selects the highlighted variable in style-panel pickers
- Doc/issue bodies no longer keep leading/trailing blank paragraphs or squished tables
- New workspaces no longer reuse a pushed branch name, fixing stale PR badges


Agents gain a new **issue.assigned** trigger and **per-property filters** on issue triggers, so you can scope automations to exactly the issues you care about. Slack integration also improves: triggers can now target private channels and the bot has broader scopes.


Canvas gets a batch of workflow upgrades — Figma-style keyboard shortcuts for reordering layers (` \[` ,` \]` , and` Cmd+\[` ), draggable gap handles on zero-gap seams, and a New Canvas that opens instantly with a proper loading state. Several long-standing canvas annoyances are fixed too, including stuck snap guides, dropped draw gestures near storyboard chrome, a lost undo after style changes, and comment toolbars/pins not showing in chat split view.


Docs and issues render more cleanly:` tempo://` deep-links work again, emoji survive markdown round-trips, tables no longer squish, and stray blank paragraphs are trimmed. Chat also feels snappier — hidden tabs stop re-rendering on every token — and hosted-artifact previews now embed inline. Rounding things out, agent-created chats are renameable everywhere, and new MCP tools let members fetch share links for issues and docs.


​


Platform


July 7, 2026


**Promo codes can now be scoped to specific models.**


### ​


New


- Promo codes can be applied to specific models in billing.


### ​


Fixed


- Triggered-run chats now name correctly, restore selection, and search files in the Open modal.
- Style-panel scrubbing now wraps at the screen edge instead of stopping.
- Chat loading shimmer in the sidebar now stays in sync with other animations.


Billing now supports **model-scoped promo codes** , so discounts can be tied to specific models rather than applied broadly.


Agent-triggered runs got several fixes: chats are named correctly, the workspace shell and prior selection are restored, and you can now search files directly from the Open modal.


We also smoothed out a few interaction and visual rough edges — scrubbing values in the style panel now wraps continuously at the screen edge, and the sidebar’s chat loading shimmer stays aligned with the rest of the interface’s animations.


​


Issues


Chat


July 6, 2026


**Attach selected tickets to the issues-tab AI chat with Cmd+L.**


### ​


New


- Cmd+L attaches selected tickets as context to the issues-tab AI chat.
- Added Hyblock Capital case-study video on the landing page.
- Added work-in-motion video showcase to /book and /agent-plus.


### ​


Fixed


- Issue cards from AI chat now show the real title instead of raw content.
- The “You’re offline” toast now clears once connectivity is confirmed.
- Sub-agent turns in issues now resolve the correct tool context.


The issues tab gains a faster way to bring work into AI chat: select one or more tickets and press **Cmd+L** to attach them as context, so the assistant can reason about exactly what you’re looking at.


Several AI chat and connectivity issues are fixed. Issue cards generated from chat now display their real title rather than raw content-block output, and sub-agent turns resolve tool context correctly. The stale “You’re offline” toast now disappears once the connection is verified.


The landing pages also get new video content, including a Hyblock Capital case study and a work-in-motion showcase on the /book and /agent-plus pages.


​


Canvas


July 4, 2026


**Canvas comments now create and close instantly on submit.**


### ​


Improved


- Layers panel header buttons now show hover tooltips.


### ​


Fixed


- Canvas comments create and close immediately when you submit.


Submitting a comment on the canvas now creates it and closes the input in one step, so you no longer have to wait or click again.


The layers panel header buttons now display hover tooltips, making it clearer what each button does.


​


Chat


Issues


Docs


July 4, 2026


**You can now @-mention tickets and docs in every chat surface.**


### ​


New


- @-mention tickets and docs in any chat.
- Redeem promo codes in Settings for billing.
- Archive chats instead of deleting them.
- Background-task pill now surfaces scheduled (Cron) jobs.
- Double-click an option in Ask a Question to select and advance.


### ​


Improved


- File/folder move and rename now ask for confirmation, with smoother drag-and-drop.
- Clickable links in comments now open in your browser.
- Agent trigger changes now show instantly.
- Agents tab shows PR badges for agent workspaces.
- Bulk issue actions now apply as a single, faster update.


### ​


Fixed


- AI chat memory compaction no longer fails (404 errors).
- Top-level folders once again expand by default in the file tree.
- Image attachments in AI chat now send reliably.
- Longer connection window prevents AI chat handshake timeouts.
- Questions asked on async chats now correctly wait for your input.
- Issue-linked canvas Open modal now shows the link’s branch, not just main.
- Shift+A now wraps component instances at the correct call site.
- The Mixed corner-radius field is now clickable to edit or clear.
- New agents start disabled, and deleting one cancels in-flight runs.
- Agent workspaces no longer clutter the canvas homepage.
- Comment tool stays active after creating a comment.
- List-view group ”+” now pre-fills the clicked group’s properties.
- No more stray error echo after stopping an AI response.


The headline of this release is deeper AI chat integration: you can now **@-mention tickets and docs in any chat surface** , so pulling context into a conversation is a lot faster. Scheduled (Cron) jobs also appear in the background-task pill, and questions asked on async chats now correctly wait for your input.


We fixed a significant reliability issue where AI chat memory compaction was failing outright — that’s resolved, alongside more dependable image attachment delivery and a longer connection window that prevents handshake timeouts.


Workspaces got some polish: moving or renaming files and folders now asks for confirmation, drag-and-drop feels smoother, and agent workspaces stay off the canvas homepage while showing PR badges in the Agents tab. New agents also start disabled by default, and deleting one cancels any in-flight runs.


Rounding things out, you can **archive chats** instead of deleting them, **redeem promo codes** in Settings, click links in comments to open them, and the file tree once again expands top-level folders by default.


​


Issues


July 3, 2026


**Issues now open and create near-instantly with optimistic updates.**


### ​


New


- Draw a frame on the canvas to absorb enclosed elements and storyboards.
- Find in Canvas: search text with Cmd/Ctrl+F.
- Cmd/Ctrl+A selects all visible tickets in a view.
- Right-click to edit ticket properties in the list view.
- Create issues instantly with an optimistic card that rolls back on failure.
- New “PR Status” workspace sort follows the progress pipeline.
- Run custom AI actions from the Commit menu.
- Chat view left sidebar now shows only the layers panel.
- Free tier now uses GLM 5.2 (via Fireworks) instead of GPT-5.4-mini.


### ​


Improved


- Redesigned and polished public share pages.
- Issues tab opens nearly instantly.
- Faster AI chat responses by caching inference tokens.


### ​


Fixed


- Default views now load on every board, not just empty ones.
- Share links now authenticate correctly.
- Create modal defaults to the first visible stage instead of a hidden one.
- Optimistic issue creation no longer duplicates entries in Recent artifacts.
- Issue deletion works reliably across all delete paths.
- Column ”+” now inherits its column’s grouping.
- Comment composer avatar no longer shows ”?” for non-assignee viewers.
- Swimlanes hide correctly for co-assignees filtered out of a view.
- Multi-assignee swimlane drag stays lane-scoped and reassigns the source lane.
- Storyboards no longer stay stuck larger than their content.
- Shift+A infers arrangement direction and skips absolute pinning in flex parents.
- Component instances drag into frames by their call site.
- Agents grid “Active” now reflects live streaming chats.
- Expired chat notice now covers cached-message sessions.


Issues get a big responsiveness boost this release. The Issues tab now opens nearly instantly, and creating an issue is optimistic — the card appears and the modal closes right away, rolling back only if the save fails. We also fixed several rough edges around views, deletion, grouping, swimlanes, and duplicate entries in Recent artifacts.


The canvas gains new tools: draw a frame to absorb the elements and storyboards inside it, and search on-canvas text with Find in Canvas (Cmd/Ctrl+F). Alongside these, we fixed storyboards getting stuck larger than their content, improved Shift+A arrangement behavior, and made component instances drag into frames by their call site.


Public share pages have been redesigned and polished, and share links now authenticate correctly. AI chat is faster thanks to cached inference tokens, and the free tier now runs on GLM 5.2. Rounding things out: a new “PR Status” workspace sort, custom AI actions in the Commit menu, and quality-of-life fixes across issues and agents.


​


Canvas


July 2, 2026


**Paste Figma designs into the canvas as editable HTML storyboards.**


### ​


New


- Paste Figma designs as editable HTML storyboards.
- SVG shape draw tools with a fill/stroke style panel on the canvas.
- Set a user-defined System Prompt injected into every AI conversation.
- Custom Agents can pick a reasoning effort paired with the model.
- Sonnet 5 now runs natively on Bedrock / Tempo Inference.
- Workspace PR badges now show a ✕ indicator for failing checks.
- AskUserQuestion cards are keyboard-focusable, with route-wide Cmd+L focus.


### ​


Improved


- Collapsed zero-size elements are now framed by their child bounds.
- Kanban drag no longer re-renders every card on each slot change.


### ​


Fixed


- Dragging a multi-assigned card between columns keeps its assignees.
- Edit labels from the kanban right-click menu, plus fixed create-on-subset.
- Canvas comments now appear in-app on the current working branch.
- Shared links hold members briefly during auth instead of denying access.
- Auth recovers from Clerk session-not-found with a grace period and retry.
- Color-picker variable swatches now show in unthemed realms.
- Workspace drag-reorder snap-back and a bulk-update crash are resolved.
- Sidebar tree no longer jitters from placeholder-height mismatches.
- Scaffolded projects handle spaced/Windows paths correctly.
- Windows dev server stops orphaning the Next.js dev lock.


This release brings design and canvas work closer together. You can now **paste Figma designs directly onto the canvas as editable HTML storyboards** , and draw with new **SVG shape tools** backed by a dedicated fill/stroke style panel.


AI chat gets more control: define a **System Prompt** that’s applied to every conversation, choose a **reasoning effort** for Custom Agents, and run **Sonnet 5 natively** on Bedrock / Tempo Inference. AskUserQuestion cards are now keyboard-focusable, and Cmd+L focuses chat from anywhere.


Issues and workspaces are steadier too. Kanban drag is faster and no longer clears assignees on multi-assigned cards, labels are editable straight from the right-click menu, and PR badges flag failing checks. A batch of fixes also smooths out canvas comments on working branches, auth edge cases on shared links, color-picker swatches, sidebar scroll jitter, and Windows path and dev-server issues.


​


Issues


July 2, 2026


**Open ticket details no longer collapse when saved views load late.**


### ​


New


- Custom Agents: early local-first prototype available as a draft preview.


### ​


Fixed


- Issues: an open ticket’s detail view stays open when the saved-views query resolves late.


This release fixes an annoying issue in the **Issues** view: previously, if the saved-views query finished loading after you’d already opened a ticket, the detail panel would collapse out from under you. Open tickets now stay open regardless of when saved views land.


We’re also starting work on **Custom Agents** . This first cut is a local-first prototype and still a draft, so expect rough edges as we iterate on the design and capabilities.


​


Chat


Platform


July 1, 2026


**Add Sonnet 5 support and in-app prompt top-ups.**


### ​


New


- Buy more prompts with in-app top-up purchases.
- Sonnet 5 available as a model (Tempo Inference falls back to 4.6).
- Canvas: share links via the new canvas_share tool, with auto-shown screenshot.
- Canvas: “Paste here” on the backdrop drops content at the cursor.
- Canvas: live size pill while drawing an element or storyboard.
- AI chat: Ctrl-U / Ctrl-D half-page scroll in a focused chat.
- AI chat: fuzzy-match slash command picker instead of prefix-only.
- Command palette: run project scripts from Cmd+Shift+P.
- Org avatar now shows the unread / streaming chat indicator.


### ​


Improved


- Free tier raised to 20 requests/day and 60/month.
- Bulk create-from-issues: instant tab switch, all rows, loading, and model picker.
- Canvas: new dropdowns, avatars, and badges.


### ​


Fixed


- Issues: create result now opens the detail view; recovers from corrupt reads.
- Issues: board scroll preserved on detail nav; no card flash on view/org switch.
- Issues: long titles wrap in the new-issue modal instead of overflowing.
- Chat: input is focused when opening or switching chats.
- Canvas: pasted images are written next to the real source file.
- Style panel: Clip content row always shows, including on storyboard root.
- Workspaces: only the branch’s own commits count as unpushed.


This release expands your model options and gives you a way to keep going when you run low. **Sonnet 5** is now selectable, with Tempo Inference falling back to 4.6, and you can grab more prompts through an **in-app top-up** without leaving the app. The free tier also gets more room: 20 requests per day and 60 per month.


The **canvas** picks up several additions: a` canvas_share` tool that generates a share link and shows a screenshot, a “Paste here” action that drops content right at your cursor, and a live size pill while you draw. Pasted images now save next to their real source file, and there are refreshed dropdowns, avatars, and badges.


**AI chat** gains vim-style Ctrl-U / Ctrl-D scrolling and a fuzzy slash-command picker, and the input now focuses automatically when you open or switch chats. The org avatar reflects unread and streaming chat activity.


On the **issues and workspaces** side, creating an issue now opens its detail view, board scroll is preserved during navigation, long titles wrap in the new-issue modal, and bulk create-from-issues is faster with instant tab switching, full rows, a loading state, and a model picker. Unpushed commit counts now include only the branch’s own commits.


​


Platform


Issues


Docs


June 30, 2026


**Universal Cmd+P search now spans every artifact in your workspace.**


### ​


New


- Cmd+P opens a universal search across all artifacts.
- @mention teammates in issues and docs.
- Revamped ticket page for issues.
- Git GUI controls in the workspace Changes tab.
- Mint share links directly on view.tempo.new.
- Create a separate workspace for each multi-selected issue.
- OpenCode models now available in the chat model picker.


### ​


Improved


- Redesigned changelog modal with accordion releases and canvas.
- Click context chips in chat to open the artifact they reference.
- AI-chat artifact cards are now clickable in side chats.
- Chat session dropdown shows loading and unread status.
- “Sort by Stage” now follows your org’s custom pipeline order.
- Unified Open modal and polished the ”+” new-tab dropdown.
- Added header-button tooltips and right-aligned the Add workspace tip.


### ​


Fixed


- Fixed a dead open button and stray warning on issue-linked workspaces.
- To-do (task list) bodies now save when creating a ticket.
- Sidebar chat input stays editable while a new workspace spins up.
- Chat list no longer jumps around during streaming.
- Share-preview comment pins now track canvas pan and zoom.


**Search just got faster.** Press` Cmd+P` anywhere to search across every artifact in your workspace—docs, issues, canvases, and chats—from one place.


Collaboration also gets a boost: you can now **@mention teammates** in issues and docs, share links can be minted right on view.tempo.new, and you can spin up a dedicated workspace for each issue you multi-select. The ticket page has been fully revamped, and the Changes tab now includes Git GUI controls.


Chat is more connected, too. Context chips and artifact cards in side chats are clickable, the session dropdown surfaces loading and unread status, and OpenCode models are available in the model picker.


We also fixed a batch of rough edges: a dead open button on issue-linked workspaces, task-list bodies dropping when creating tickets, the sidebar chat input locking up during workspace setup, the chat list jittering while streaming, and comment pins drifting on shared canvas previews.


​


Issues


June 29, 2026


**Issues now support multiple assignees per ticket.**


### ​


New


- Issues: assign multiple people to a single ticket.
- Sidebar chat: pick reasoning effort with a new effort picker.
- Bug reports: attach screen recordings, files, and images.
- Canvas Playground: hands-on tour walking through every canvas action.
- Canvas: Ungroup gesture plus group/ungroup keyboard shortcuts.


### ​


Improved


- Canvas: context-linking now requires a pushed canvas branch.
- Canvas: instant previews carry source CSS, removing the flash on load.
- Canvas: Ungroup and Explode now keep instant previews stable.


### ​


Fixed


- Chat: attached images no longer disappear from bubbles when nav context is present.
- Auth: smoother handling of expired sessions and cleaner connection toasts.
- Issues: kanban board now stays scoped to the active org after switching orgs.
- Workspaces: right-click menu actions no longer deselect the current chat/workspace.
- Issues, MCP, and kanban board: assorted fixes.


The headline change this release is **multiple assignees per issue** — you can now share ownership of a ticket across several people instead of picking just one.


Canvas gets a lot of attention: a new **Canvas Playground** gives you a guided, hands-on tour of every canvas action, and you can now **ungroup with a gesture** or use dedicated group/ungroup keyboard shortcuts. Instant previews are steadier too, carrying source CSS to avoid the load flash and staying stable through Ungroup and Explode.


Chat picks up an **effort picker** in the sidebar so you can control reasoning effort, and attached images no longer vanish from bubbles when navigation context is present. **Bug reports** can now include screen recordings plus file and image attachments.


On the fixes side, expired sessions are handled more gracefully with cleaner connection toasts, the kanban board correctly scopes to your active org after switching, and right-click menu actions keep your current chat or workspace selected.


​


Platform


June 26, 2026


**Connection outages now surface as a persistent toast so you know when you’re offline.**


### ​


New


- Connection outages show a persistent toast with a confirmation before appearing.


### ​


Fixed


- Canvas: reordering elements now keeps sibling positions correct after a move.


When your connection drops, Tempo now shows a persistent toast so you’re aware the workspace is offline, instead of failing silently. The notice waits for confirmation before appearing to avoid false alarms from brief network blips.


On the design canvas, reordering elements no longer leaves siblings with stale positions. Move operations now apply real element-aligned edits, so the layout stays accurate after a reorder.


​


Canvas


June 25, 2026


**Canvas elements and storyboards can now be nudged with arrow keys.**


### ​


New


- Arrow-key positioning for absolute elements and storyboards on the canvas.
- ”+” picker in right-panel chat to attach Docs, Canvases, and Issues.


### ​


Improved


- Style panel shows “Mixed” when a multi-selection has differing values.
- Sending a message bumps a workspace up the Recent list immediately.


### ​


Fixed


- Newly created wrapper is auto-selected after Wrap in Frame / Auto Layout.
- Doc body now always renders by waiting for the first sync before mounting.
- Shared canvases keep their styling instead of loading unstyled.
- Style panel class dropdown now populates on Vite v3 host projects.
- Frames no longer collapse to a default size on the legacy sizing path.
- AI-generated issue titles no longer show raw HTML entities.
- AI chat keeps the correct issue context instead of losing it.


This release focuses on the canvas and AI chat. You can now nudge absolute elements and storyboards with the arrow keys for precise positioning, and the right-panel chat gains a ”+” picker for attaching Docs, Canvases, and Issues directly to a conversation.


The style panel is clearer for multi-selections: when selected elements disagree on a value, it now shows “Mixed” instead of a single misleading value. Workspaces also reorder in the Recent list as soon as you send a message, not just when a turn finishes.


Several fixes improve day-to-day reliability. Docs now wait for the first sync before mounting so the body always renders, shared canvases keep their styling, and the style panel’s class dropdown populates correctly on Vite v3 hosts. We also fixed frame collapsing on the legacy sizing path, auto-selection of the new wrapper after Wrap in Frame / Auto Layout, garbled HTML entities in AI-generated issue titles, and lost issue context in AI chat.


​


Canvas


June 25, 2026


**Canvas now snaps to equal spacing between objects.**


### ​


New


- Gap snapping aligns objects to equal spacing as you move them.
- Add a workspace directly from the issues/docs chat picker.


### ​


Improved


- Duplicate-and-drag now snaps consistently and releases the cursor cleanly.


### ​


Fixed


- Text-only storyboards render crisply with no stray marquee or Line Break layers.
- Linked docs from an issue now open in the Docs tab.
- Search auto-expands a parent when only a child matches.


Canvas gets smarter alignment with **gap snapping** , which helps you line objects up with equal spacing as you drag them. Duplicate-and-drag also behaves more predictably now: it snaps the same way as a normal drag and no longer leaves the cursor stuck to the copy.


Text-only storyboards are cleaner too — iframes render crisply, and you’ll no longer see a stray marquee selection or unwanted Line Break layers.


A few workflow fixes round things out: you can now add a workspace right from the issues/docs chat picker, linked docs open in the Docs tab from an issue’s context, and search will automatically expand a parent item when the match is only on one of its children.


​


Canvas


June 24, 2026


**Edit styles across multiple elements at once on the canvas.**


### ​


New


- Edit styles for multiple selected elements at once.
- Switch existing Claude chats to Max effort via respawn.
- AI chat errors now upload a diagnostic automatically to speed up fixes.


### ​


Improved


- Slash-command menu is keyboard-navigable with a reachable Show-more.
- Kanban boards are faster with virtualized card lists and smoother drag.
- Kanban board uses a cleaner overlay scrollbar.


### ​


Fixed


- Right-click now shows the system spell-check correction menu.
- Default issue stages no longer use stages your org has hidden.
- Intrinsic-sized storyboards no longer collapse on auto-width roots.


The style panel now supports editing multiple selected elements together, so you can adjust shared properties across a group in one pass instead of element by element.


AI chat gets two upgrades: you can switch existing Claude chats to Max effort through a respawn, and when a chat errors, a diagnostic is uploaded automatically so issues can be tracked down without extra steps. The slash-command menu was rebuilt to be fully keyboard-navigable, including a reachable Show-more.


Issues see a round of fixes and performance work. Kanban boards now virtualize their card lists for smoother scrolling and dragging, use a cleaner overlay scrollbar, and respect any built-in stages your org has hidden when creating default stages.


On the canvas, intrinsic-sized storyboards no longer collapse on auto-width roots. We also restored the system spell-check correction menu on right-click in text fields.


​


Chat


June 24, 2026


**You can now restore an AI chat to a previous point in the conversation.**


### ​


New


- Restore an AI chat back to any earlier point in the conversation.


### ​


Improved


- Run and setup scripts are now stored in the database, configurable per org, user, and project.
- Smoother chat performance during heavy cross-device activity.
- Faster rendering of user message bubbles in chat.


### ​


Fixed


- What’s New modal links now stay within the correct release channel.


The headline change in this release is the ability to **restore an AI chat to a previous point** in the conversation. If a chat goes in the wrong direction, you can roll back to an earlier message and pick up from there.


Chat also feels faster. We stopped cross-device event floods from bogging down the renderer, and user message bubbles now render more efficiently, so the chat stays responsive even during busy sessions.


Run and setup scripts have moved to the database, so they can be configured at the org level as well as per user and per project. We also fixed the What’s New modal so its links stay within the correct release channel.


​


Canvas


Chat


Issues


June 24, 2026


**Smoother drag-and-drop and faster AI chat.**


### ​


Improved


- Refined drag-and-drop behavior across the workspace.
- AI chat feels more responsive, with less lag when adding context.


​


Canvas


June 24, 2026


**Canvas comments arrive with pins, threads, @-mentions, and resolve/reopen.**


### ​


New


- Canvas comments: pins, threads, sidebar, @-mentions, and resolve/reopen.
- Set a per-canvas background color on the Design tab.
- Zoom control added to the canvas sidebar.
- Delete tickets with a confirmation step; delete dialogs unified across issues.
- Issues list view now nests stage groups under their parent status.
- Run /review directly in chat instead of the CLI terminal.


### ​


Improved


- Clearer empty states for the Design sidebar, Issues views, and Docs.
- Recent artifacts show clearer visual hierarchy and accurate origin labels.
- Connection issues now distinguish ‘internet down’ from ‘service down’ instead of a blank screen.
- Chat artifact summary rows and plan-open behavior refined.
- Project and workspace order plus selection now stay in sync across chat and the design homepage.
- Restyled changelog modal with clearer headings and lighter chrome.
- Bolder, easier-to-scan sidebar titles.


### ​


Fixed


- Mention picker no longer leaks raw email addresses; pasted-image thumbnails show in the collapsed composer.
- Links are now clickable in the docs and issues editor.
- Kanban drag-and-drop rebuilt for more reliable moves.
- Color picker now preserves alpha (transparency) when editing styles.
- Hid orphaned commit and right-sidebar buttons in empty and canvas-only states.
- Chat spinner in the nav rail is now scoped to the active org and reflects all workspaces.
