---
schema_version: "1.0.0"
document_id: "993d8d6812f95350717c9cb5c3735b82179a3ace2b6a98ab0cac5e2e24adb641"
company_key: "godaddy-inc-class-a-common-stock"
company: "GoDaddy Inc."
source_id: "godaddy-inc-class-a-common-stock-news-import-cf537cccbea7"
canonical_url: "https://www.godaddy.com/resources/news/logs-as-runtime-documentation"
published_at: "2026-06-29T06:15:00+00:00"
first_seen_at: "2026-07-21T21:50:09.149998+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:e719bbb68a9eff8cf4ef503894bfad3bbdc17a07854a81a11b9f8ba89cc27859"
---

# Logs As Runtime Documentation

## Key takeaways


- Structured logs convert runtime ambiguity into evidence, reducing the exploration tax paid by both humans and AI assistants during debugging.
- Logging is most valuable not as a debugging sidecar but as runtime documentation that answers questions static source code never can.
- The upfront cost of consistent, scoped, filterable logging is small compared to the token and time savings it generates across every future debugging session.


## Background


As software developers, we build things that solve problems. Many of the things we build go through multiple iterations only to be scrapped for whatever reason. The product we worked on, an iOS chat application that lets users connect to an AI agent via MCP, ended up going to the software development graveyard. Building it, however, provided us with some real actionable insights.


What we learned is that logging was doing more than helping us fix bugs. We used Claude Code to navigate the codebase, follow notification flows, and refactor patterns throughout the project, and what became clear early on was that our real cost was not any single bug but session spend: tokens burned on file reads, exploration, and wrong proposals whenever runtime behavior was invisible. Structured logs acted as runtime documentation, cutting estimated token spend by roughly 40% (~29k–31k tokens saved) across the specific issues we ran into: tracing notification flows, diagnosing a startup race condition, answering lifecycle questions, and reconstructing event timelines. When logs answer which branch ran, what values were live, how long steps took, and what happened next, both people and the assistant guess less.


Instead of reconstructing behavior from scattered files, we could read what the system was actually doing: branch choices, values, timings, handoffs. Logging stopped being a sidecar and became something we reached for constantly.


## The problem we cared about: token spend, not any one defect


The chat interface we built let customers start a conversation with an AI agent to perform tasks like buying or managing a domain, creating a logo, or drafting a business or marketing plan. We prototyped several approaches to make this work on iOS. One was A2UI, where the AI agent drives the user interface directly, generating and updating UI components in response to conversation context rather than relying on hand-coded screens. Another was MCP-UI, where the agent uses the MCP to render output inside an embedded web view. Our logo concept used MCP-UI to ask the user a series of questions through chat, determine what kind of logo they wanted, and render the result. We scoped the prototype to proving the concept worked end to end, which it did, surfaced as a native iOS alert displaying the generated image rather than a full save-to-album flow.


Streaming responses between the agent and the client was an early concern. We experimented with both SSE and WebSockets as alternatives and confirmed both were viable. The details matter less than the fact that response delays were real and we noticed them.


Unlike desktop users, mobile users background apps. They multitask. To keep them in the loop when the agent responded, we prototyped local notifications with the ability to reply directly to the agent from the notification. That gave us something useful but also gave us something to debug: notification routing, a startup race, and noisy local notifications firing at the wrong time.


That is where Claude Code came in, and where the economics of AI-assisted debugging became hard to ignore. When Claude Code cannot see the device, it pays for certainty in tokens: wide reads across coordinators and delegates, follow-up questions, speculative refactors, and sometimes whole approaches that a single log line would have ruled out. We started estimating savings per incident and summed them. The headline is not precision to the last token but that ambiguity is expensive, and logs cheaply convert ambiguity into evidence.


The sections that follow are not “the problems” in a narrative sense; they are examples of what we avoided paying for once runtime output was legible.


## The logging setup


Many ways exist to structure logging. In the following code example, we used a` Loggable` protocol backed by OSLog, with scoped loggers per domain and emoji prefixes for quick scanning in Console.app. The exact code matters less than the principles: stable categories, predictable formatting, and messages you can filter and interpret when you are already under pressure.


```text
extension AiroNativeVoiceViewController: Loggable {
public static var logger: Logger { AiroLogger.ui
}
public static var emoji: StaticString { "📱" }
}
```


` AiroLogger` defines subsystem-scoped categories:` general` ,` ui` ,` network` ,` query` , and` repository` .


` nonisolated` on the protocol requirements keeps logging callable from any isolation context: actors,` @MainActor` types, background work. No warnings or extra` await` required.
That consistency lowered friction enough that logging actually showed up everywhere we needed it. Categories sped up filtering; emoji made repeating patterns easy to spot in a wall of output rather than one undifferentiated stream.


Emoji were partly taste, partly ergonomics. For neurodivergent team members, they mattered in a specific way: a long console dump is uniform text, which is hard to scan and easy to get lost in. Stable emoji prefixes behave like landmarks. You can jump to “the 📬 block” without reading every line. That is not a substitute for filters, but it pairs well with them and cuts the attention cost of parsing dense output. Filterable, visually structured logs cost less per incident.


## Where logs saved tokens (and time)


Each example in the following sections is the same story in miniature. Without logs, we would have paid an exploration tax: extra reads and back-and-forth until something in source or a guess matched reality. With logs, the runtime told the story first.


### Notification flow


Notifications could arrive as a suggested reply action, a deeplink push, or a local notification deeplink. Without logs, mapping the branches would have meant walking` PushNotificationManager` ,` AppNavigationCoordinator` , and the` UNUserNotificationCenterDelegate` implementation: slow for humans and token-heavy for an assistant.


One line at the top of` didReceive` :


```text
AiroKit.AiroLogger.general.debug(
"📬 Notification response received:
actionIdentifier=\(response.actionIdentifier),
categoryIdentifier=\(response.notification.request.content.categoryIdentifier)"
)
```


From that single line the assistant could see the path taken, confirm that the if/else branches were mutually exclusive (airo reply vs. deeplink, not both), and identify the` AIRO_REPLY_` prefix on` actionIdentifier` as the real decision point. That headed off a bad suggestion: adding guard logic for both handlers firing simultaneously, which the design did not need.
Further down, coordinator logs spelled out the reply path:


```text
// OverAppDelegate
AiroKit.AiroLogger.general.debug(
"📬 OverAppDelegate handleAiroNotificationReply:
text=\(action.replyText.prefix(50)), conversationId=\
(action.payload.conversationId), customerId=\
(action.payload.customerId), ventureId=\
(action.payload.ventureId)"
)
// AppNavigationCoordinator
AiroKit.AiroLogger.general.error("📬 submitAiroReply
failed: \(error.localizedDescription)")
```


Between those two sites we could follow the reply dispatch chain without opening` OverAppDelegate` ,` AppNavigationCoordinator` , or their dependency graphs first.


**Estimated saving: ~8,000 tokens** (no explore pass, no deep coordinator read).


### The startup race


Notifications could arrive before the Airo view controller finished loading. The first approach used` setPendingReply` with sleep-based retries:


```text
public func setPendingReply(_ text: String) {
pendingReply = text
Self.logDebug("setPendingReply: Set pending reply
[text=\(text.prefix(50))]")
Task {
try? await Task.sleep(for:
.milliseconds(500))
if bareChatModel.conversationId != nil {
Self.logDebug("Submitting pending reply")
await bareChatModel.submitPrompt(text)
pendingReply = nil
} else {
Self.logWarning("No conversation ID yet,
will retry")
try? await Task.sleep(for:
.milliseconds(1000))
// ...
}
}
}
```


In Console.app, “No conversation ID yet, will retry” appeared constantly. Even on fast startups the retry could still lose the race. The logs revealed something broader than a flaky timer:` setPendingReply` was running before the view controller had finished loading, which raised the real question of whether the reply path should go through the UI layer at all.


We stopped polishing retries and removed the mismatch. Notification replies now bypass the view controller and submit directly to the service:


```text
func submitAiroReply(text: String, conversationId: String, customerId: String, ventureId: String, shouldOpenAiro: Bool) {
guard let service = airoService else { return }
Task {
do {
try await service.submitNotificationReply(
conversationId: conversationId,
ventureId: ventureId,
userMessage: text,
customerId: customerId,
workflow: "airo-v2"
)
if shouldOpenAiro {
await showAiro(.inAppReply(ventureId: ventureId, customerId: customerId, conversationId: conversationId))
}
} catch {
AiroKit.AiroLogger.general.error("📬
submitAiroReply failed: \
(error.localizedDescription)")
}
}
}
```


Where a view controller is in play for the in-app case, an async` init` sets the conversation before the initializer returns:


```text
public init(_ service: AiroService, source: Source, ...) async throws {
chatModel = try ChatModel(service: service, ...)
super.init(nibName: nil, bundle: nil)
if let conversationId = source.conversationId {
chatModel.setActiveConversation(serverId: conversationId)  // synchronous, no race
}
}
```


` ChatModel` uses a state machine under` @MainActor` , which serialises mutations without a separate actor:


```text
@MainActor @Observable final class ChatModel {
enum State {
case idle(previousConversationIdForHandoff: String?)
case bootstrapping(previousConversationId: String?)
case ready(conversationId: String)
}
func submitReply(_ text: String, toConversation conversationId: String) async {
guard case .ready(conversationId) = state else {
AiroLogger.general.error("💬 Cannot submit - conversation \(conversationId) not active")
return
}
await doSubmit(reply: text, conversationId: conversationId)
}
}
```


The token saving here was not "we fixed a race" but that we **did not spend tokens** polishing two dead ends: more retry logic and a separate actor wrapper. Logs surfaced the real design question early.


**Estimated saving: ~12,000 tokens** (avoided dead-end refactors and review loops, not just fewer debugger minutes).


### Fewer lifecycle questions


Bootstrap logs in` ChatModel` meant we did not burn cycles on basic lifecycle questions: “when is the conversation available?”, “what starts the SSE stream?” These sound small but cost real tokens when the assistant has to read` ChatModel` and its callers to answer them.


```text
AiroLogger.general.debug("💬 Bootstrap: fetching most recent conversation")
let fetchStart = Date()
if let serverId = try? await service.fetchMostRecentConversation(ventureId: ventureId) {
let fetchElapsed = Date().timeIntervalSince(fetchStart)
AiroLogger.general.debug("💬 Bootstrap: fetch completed in \(String(format: "%.3f", fetchElapsed))s")
state = .ready(conversationId: serverId)
} else {
let fetchElapsed = Date().timeIntervalSince(fetchStart)
AiroLogger.general.debug("💬 Bootstrap: no recent conversation found (took \(String(format: "%.3f", fetchElapsed))s), creating new")
// ...
}
AiroLogger.general.debug("💬 Bootstrap: reconciling session state")
await reconcileOpenSessionState()
AiroLogger.general.debug("💬 Bootstrap: reconcile complete")
```


Sequencing was visible at a glance. Timings like` fetch completed in 0.342s` separated slow network bootstrap from logic bugs, something source alone cannot give you.


**Estimated saving: ~5,000 tokens.**


### Local notifications


We sent local notifications when a prompt or stream completed. That was useful when the user was away from Airo but noisy and often wrong when they were already in the Airo view.


Static analysis finds call sites. Logs showed whether scheduling matched user context. The following illustrates a representative sequence:


```text
🎯 Airo button tapped at timestamp 1777293980.918
🎯 User fetch took 0.005s
🎯 Venture fetch took 0.028s
🎯 Pre-showAiro setup took 0.080s total
🎯 showAiro started
🎯 Cookie refresh took 0.008s
🎯 Creating AiroNativeVoiceViewController
🎯 ViewController created in 0.006s
🎯 Presenting view controller at timestamp 1777293981.012
📱 Airo view did load
🎯 Present call returned in 0.002s
🎯 showAiro total time: 0.016s
📐 ChatContentView.body building
💬 Bootstrap: fetching most recent conversation
──── (165 lines hidden) ────────────────────────────────────────────
✅ Loaded message 6EEB96FB-87BE-4FE5-B622-B4CACF75F1C1 with 1 content items
Item[0]: source=mcpUI, hasResource=true, resource=[html.count=0, uri=logo-ui://65e4925a-aa91-478b-b8cd-22edb5908485]
🌐 generating HTML (length=0) [async]
🌐 HTML generation completed in 0.000s
🏗️ LazyMessageContentView: rendering 1 loaded content items for message 6EEB96FB-87BE-4FE5-B622-B4CACF75F1C1
🎨 McpUiResourceHostWebView.onAppear for resourceUri=...
──── (20 lines hidden) ─────────────────────────────────────────────
🌐 loading HTML with invalid baseURL
🌐 starting bridge connection [resourceUri= … ]
🌐 HTML empty, fetching from proxy [uri= … ]
🍪 Starting cookie refresh for SSE credentials
```


Tap, presentation, Airo view did load, bootstrap, and rendering were already underway. If a local notification still fired afterward, we were looking at a suppression problem tied to visible state, not a vague “notification bug.” Timestamps also ruled out the comforting story about startup noise; the user was already in session.


**Estimated saving: ~4,000–6,000 tokens** (no manual timeline reconstruction across presentation, bootstrap, rendering, and scheduling).


### Onboarding


Onboarding into a large codebase means docs, source, and hallway context. Logs add another path: the system in motion. A new developer may not yet know every entry point, but structured, scoped logs show flows as they happen, which shortens the gap between “I found the files” and “I see what it does.”


Compared with a wiki, logs stay current. Compared with diagrams, they are concrete. For assistants, that means fewer speculative reads and fewer clarifying turns: the same token economics as debugging. For humans, less guesswork.


## Filtering and emoji


Subsystem filters (` ui` ,` network` ,` repository` ) cut noise. Consistent markers (📬 for notifications, 💬 for chat) let the eye jump to the right thread under load. Less scrolling, less parsing fatigue, and visual anchors that help when dense logs are not one flat slab of text, including for neurodivergent readers.


## Totals


The following table lists our estimated total token savings for each action:


What we avoided paying for Est. tokens saved


Notification flow file tracing / deep coordinator reads ~8,000


Dead-end refactors (retries, actor wrapper) surfaced early by logs ~12,000


Lifecycle and sequencing questions ~5,000


Manual timeline reconstruction (presentation + bootstrap + scheduling) ~4,000–6,000


**Sum** **~29,000–31,000**


That sum is a rough estimate, not a measured figure, but grounded in the examples above. ~40% is a fair headline if you want a single number.


## Closing


We set out to ship a prototype. What stuck was a lesson about economics: AI-assisted debugging is cheap when the model gets ground truth quickly, and expensive when it has to infer control flow from a sprawling tree of files.


Logs answer questions the runtime always knows better than a static repository:


- which branch ran
- how long it took
- which values were live
- what happened next


Every one of those answers in a log line is a file read or clarifying question the assistant, or a human, might not need. The upfront cost of good logging is small. The return shows up every time someone has to understand what the code actually did, on a device or in production.


Treat logs as runtime documentation, not debug exhaust.
