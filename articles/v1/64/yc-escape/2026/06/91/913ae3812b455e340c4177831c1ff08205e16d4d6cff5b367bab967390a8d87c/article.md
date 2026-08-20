---
schema_version: "1.0.0"
document_id: "913ae3812b455e340c4177831c1ff08205e16d4d6cff5b367bab967390a8d87c"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/how-ai-pentesting-found-an-unauthenticated-rce-and-walked-around-the-waf/"
published_at: "2026-06-30T15:25:47+00:00"
first_seen_at: "2026-07-20T23:23:55.281376+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:e65616d6ee2c4db016a6b6768aeba7778ce9049ca1b7839d7d90b351df747902"
---

# Two Critical Vulnerabilities, One AI Pentester: How Cascade Found an Unauthenticated RCE and Walked Around the WAF

## TL;DR


We pointed **Cascade** ,[Escape's AI pentesting solution](https://escape.tech/product/ai-pentesting) , at a single Spring + JSP customer portal. It came back with two findings that are typically difficult for traditional Dynamic Application Security Testing (DAST) scanners to detect:


- **Unauthenticated RCE via SpEL injection.** A` ref` request parameter was dropped, unsanitized, into a` spring:eval` expression inside a global JSP template. Because the JSP engine substitutes the value *before* SpEL parses it, an attacker controls the expression itself, not just a string inside it. Evaluated under a full` StandardEvaluationContext` , that becomes` Runtime.exec()` from an unauthenticated request, with the command output reflected straight back into the page. Full pre-auth server compromise from a single GET.
- **A WAF bypassed by obfuscation.** A Web Application Firewall blocked the clean RCE payload, so Cascade rebuilt the exploit character by character (` class.forName` ,` .charAt()` ,` toString(int)` ). The request that finally landed held no literal` java.lang.Runtime` and no` exec` , which left the WAF with no rule to catch it, even though the expression it evaluated did exactly what the blocked one would have.


The two vulnerabilities were discovered in the same application, each found by an agent that reads the source code and then attacks the running app to confirm the vulnerability is real. Both vulnerability classes are well documented, so the part worth your attention is the reasoning that took an AI pentester from source to working exploit, which is exactly where periodic manual tests and pattern-matching scanners tend to come up short.


Below, we break down each finding and how Cascade got there.


## Meet Cascade: AI Pentesting That Reads Code *and* Attacks


Most scanners work the same way: throw payloads at endpoints, match the responses against a signature database, report what matched. DAST is good at exactly this. It's fast, it scales, and it's reliable for known patterns, which is what you want for regression testing on every release.


What DAST doesn't do is read code. It has no way of knowing that a request parameter gets substituted into a template expression before the parser ever sees it, or that a WAF blocking` java.lang.Runtime` is useless against an expression that spells out the same string at runtime. To find that kind of vulnerability you have to reason about how the application is actually wired together, and that's outside what a signature database can do.


[Cascade](https://escape.tech/blog/introducing-cascade-the-multi-agent-penetration-testing/) is Escape's[autonomous pentesting](https://escape.tech/blog/agentic-pentesting/) engine, built as a swarm of specialized agents that pass work between them:


- The **Orchestrator** runs the engagement, it decides what's worth attacking and spins up the other agents as it goes.
- **Coverage agents** map the application and read the source where they can get it.
- **Exploitation agents** are each spawned against one hypothesis, "this` ref` parameter reaches a SpEL expression, go test it."
- A separate **Reporter agent** reproduces every finding before it's written up, so nothing surfaces that hasn't actually been proven.


Cascade's agentic architecture


The agents share a knowledge store, so what one of them learns shapes what the others test next. When source code is in scope, Cascade uses it: it follows the data from input to sink, spots the dangerous evaluation patterns, then attacks the live app to confirm the hypothesis actually holds. (Black-box mode gets surprisingly close, but white-box is where findings like the two below come from.)


Both vulnerabilities in this article started with source-code inspection. Cascade read the JSP template, followed the` ref` parameter to a` spring:eval` call, and built the exploit from there. When the clean payload hit the WAF and bounced, it adapted the payload, re-encoded it, and got through on its own.


A scanner recognizes what it has already seen. A pentester has to work out what it hasn't, and that's the gap Cascade is built to close.


## Finding #1 — Unauthenticated RCE via SpEL Injection in a Global JSP Template


**Severity:** Critical
**Auth required:** None
**Detected by WAF:** No (clean payload)


### What SpEL injection is


Spring Expression Language (SpEL) is the expression engine baked into the Spring framework. It can walk object graphs, call methods, instantiate classes, and reach static methods like` T(java.lang.Runtime).getRuntime().exec()` . That reach is the whole point of the language, and it's also exactly why letting attacker-controlled data into an evaluator is so dangerous.


It gets worse when the evaluator runs under a` StandardEvaluationContext` , Spring's default. That context puts no restrictions on type references (` T(...)` ) or method calls, so an injected expression can do anything the JVM can. The application here used that default.


### The root cause


Every page on the portal rendered through a shared global JSP template, including the` og:image` and` og:description` Open Graph meta tags that social-media crawlers read. The template built those values with a Spring` <spring:eval>` tag, and it dropped the raw` ref` GET parameter straight into the expression:


```text
<spring:eval expression="'x' + ref + 'y'" var="metaValue" />


```


The detail that makes this exploitable is the order of operations. JSP substitutes` ${ref}` *before* SpEL parses anything. So by the time the SpEL engine reads the expression, the attacker's input is already part of the expression source, not sitting safely inside a quoted literal, but woven into the code that's about to run.


### How Cascade found it and exploited it


Cascade's attack chain


Cascade's coverage agent read the template source and followed` ref` from the request through to the` <spring:eval>` tag. A Spring expression evaluator with user input concatenated directly into it is a textbook SpEL sink, and that's how it got flagged.


What happened next is the part a fuzzer can't do. The exploitation agent didn't pull a payload off a list, it looked at the shape of the actual expression first:


- the expression is a string concatenation,` 'x' + ref + 'y'`
- JSP substitutes the parameter *before* SpEL parses it
- so the injection point sits inside a string literal, which means you need a quote to break out, a subexpression to inject, and a reopen to keep the expression valid


The` x')+(...)+('"` wrapper falls directly out of that analysis. Drop a generic` T(java.lang.Runtime).getRuntime().exec('id')` into the parameter raw and you get a parse error, not a shell; it only works once you've understood the literal it's landing in. Cascade then confirmed execution by finding the` uid=...` output in the response and handed the verified finding to the reporter, start to finish without a human touching it.


### Impact


Critical, pre-auth. Any unauthenticated visitor can run arbitrary OS commands on the server, and because the template is global, every page on the application is a viable entry point.


## Finding #2 — WAF Bypass via Obfuscated SpEL Payload


**Severity:** Critical
**Auth required:** None
**Detected by WAF:** No (obfuscated payload)


### The wall


Put a WAF in front of the app and the clean payload above stops working. The WAF scans the request for known-dangerous strings;` java.lang.Runtime` ,` getRuntime` ,` exec` ,` Scanner` and drops anything that matches. On its own, that looks like a fix.


It isn't, and the reason is a mismatch in what each layer actually sees. The WAF reads the raw request bytes. SpEL reads the expression *after* it evaluates. So if you can make the expression produce those dangerous strings at runtime without ever writing them in the request, there's nothing left for the WAF to match.


### The technique: runtime string reconstruction


SpEL lets you call` toString(int)` on a` Character` to turn an ASCII code point into its character. Chain enough of those together and you can spell out any class or method name using nothing but integers.


The building block Cascade used:` (2.toString()+2).charAt(0)` yields the character` '2'` as a` Character` object, and` Character.toString(int)` (reached as` .class.toString(int)` ) maps a code point to its character. From there it's just concatenation:


```text
(2.toString()+2).charAt(0).class.toString(106)  →  'j'
(2.toString()+2).charAt(0).class.toString(97)   →  'a'
(2.toString()+2).charAt(0).class.toString(118)  →  'v'
(2.toString()+2).charAt(0).class.toString(97)   →  'a'
(2.toString()+2).charAt(0).class.toString(46)   →  '.'
(2.toString()+2).charAt(0).class.toString(108)  →  'l'
(2.toString()+2).charAt(0).class.toString(97)   →  'a'
(2.toString()+2).charAt(0).class.toString(110)  →  'n'
(2.toString()+2).charAt(0).class.toString(103)  →  'g'
(2.toString()+2).charAt(0).class.toString(46)   →  '.'
(2.toString()+2).charAt(0).class.toString(82)   →  'R'
(2.toString()+2).charAt(0).class.toString(117)  →  'u'
(2.toString()+2).charAt(0).class.toString(110)  →  'n'
(2.toString()+2).charAt(0).class.toString(116)  →  't'
(2.toString()+2).charAt(0).class.toString(105)  →  'i'
(2.toString()+2).charAt(0).class.toString(109)  →  'm'
(2.toString()+2).charAt(0).class.toString(101)  →  'e'
// → "java.lang.Runtime" — assembled entirely inside the SpEL evaluator, never written in the HTTP request


```


The full payload assembles` java.lang.Runtime` and` getRuntime` this way, feeds them into` Class.forName(...).getMethod(...)` , invokes the method to get the runtime instance, calls` exec("id")` , and reads the output through a` Scanner` — all of it driven by chained integer-to-character lookups.


```text
curl --path-as-is -sk -X GET \
"https://$DOMAIN/?ref=$PAYLOAD"


```


To the WAF this is a request full of integer literals and` .charAt()` calls, and none of it trips a rule. To the SpEL engine it's` java.lang.Runtime` , and it runs` exec` .


### How Cascade adapted


When the clean payload got blocked, Cascade treated the WAF as a puzzle to probe rather than a dead end, testing what the evaluator would still accept at each step.


**Confirm SpEL is still live.** A harmless` ${2*3}` comes back as` 6` . So the WAF isn't blocking evaluation, only specific patterns — the sink is still reachable.


**Get a class reference without` T()` .**` T(java.lang.Runtime)` is blocked, but` ${2.class}` returns` class java.lang.Integer` . That's a valid` Class` object pulled from an integer literal, with no banned keyword anywhere, and it's the way into reflection.


**Build strings without quotes.** Quote characters are blocked too, so a literal like` "java.lang.Runtime"` is off the table. The way around it is` Character.toString(int)` , reached through` (2.toString()+2).charAt(0).class.toString(int)` — integers and method calls on objects already in scope, no quotes, no keywords. The piece that mattered most on this target:` 2.class.forName(...)` worked directly, calling` forName` on the` Integer` class reference to load any class from a string we'd built ourselves.


**Load the class and walk to the runtime.** With string construction working, Cascade spelled out` java.lang.Runtime` , passed it to` 2.class.forName(...)` , then reached` getRuntime` the same way — method name assembled as a string, invoked through` getMethod(...).invoke(null)` . Still no` T()` , no literals, no quotes in the request.


**Execute and read the output.**` exec("id")` runs on the reflected runtime instance, the output goes through a` Scanner` with the same` \\A` delimiter trick from the clean payload, and the result comes back in the rendered HTML.


The WAF never saw` java.lang.Runtime` . What it saw was integer literals,` .charAt()` calls, and` .class.toString()` chains, with nothing in its ruleset to catch any of it. Every dangerous string only existed after evaluation, inside the engine, where the WAF couldn't reach.


### The lesson


A WAF is not a fix for an injection vulnerability. It raises the bar — someone firing a static payload list at the endpoint gets stopped — but anyone who understands the execution model underneath, human or AI, can rebuild the attack out of integers and slip past it.


## What this says about AI-powered pentesting


Neither of these vulnerabilities is exotic. SpEL injection is a known class, and WAF bypass through string reconstruction is a documented technique. The interesting part is *how* they surfaced, and what that says about the tools most teams lean on.


Start with[DAST](https://escape.tech/blog/top-dast-tools/) . Pointed at the same parameter, a scanner fuzzes` ref` with a payload list, and most SpEL lists carry something like` T(java.lang.Runtime).getRuntime().exec('id')` — raw, unquoted, blind to context. Against an expression that expects a string literal in a specific spot, that payload throws a parse error instead of a shell. The scanner sees a non-200, marks the parameter clean, and moves on. Cascade read the template first, understood how the expression was assembled, and shaped a payload to fit. The difference isn't the size of the wordlist; it's that there was no wordlist involved.


A skilled human doing source review would very likely catch this too, and we're not pretending otherwise. The problem is cadence. A manual pentest happens once or twice a year against a snapshot of the app, and a template like this one can ship in a routine deploy and sit untouched for months between engagements. Cascade runs continuously and keeps what it learns from one session to the next — the template, the parameter flows, the evaluation context — so a new endpoint or parameter gets tested on the next run rather than at the next contract renewal.


The bypass is where the difference is hardest to argue with. A scanner that gets blocked logs "blocked" and stops there. Cascade rebuilt the dangerous strings out of integers, regenerated the payload, and confirmed it went through — blocked, work out why, adapt, retry. A static payload list won't do that, and a human attacker will, which is the whole problem: most applications are defended against the payload list and wide open to the person.


And that points at the thing that probably matters most to whoever owns this app. The WAF was giving the team a false read on its own coverage. From the outside, blocked traffic looks like protection works. But until something actively tries to get around the control, there's no way to know whether it's stopping real attacks or only the careless ones — and that's the question Cascade was built to answer in practice rather than on a slide.


## Conclusion


Two critical vulnerabilities on one application, neither needing authentication, both found by an agent that read the source, reasoned about evaluation order, and kept going when a firewall got in the way. Neither one came off a shelf: the RCE was derived from reading the template, and the bypass was assembled live, once the WAF had shown which patterns it would reject. Those are the findings that tend to fall through the cracks between scheduled pentests and signature-based scanners.


If your security program leans on annual pentests and DAST alone, it's worth asking what's sitting undetected in the months between engagements.


That's what Escape AI pentesting is built for: continuous, context-aware testing that reasons about your application the way a skilled attacker would, not the way a signature list does.[You can book a walkthrough here.](https://escape.tech/book-a-demo)


💡 **Want to learn further?** Explore these guides to learn more about novel vulnerabilities, optimize your workflows, and explore alternatives to existing solutions:


- [Fixing Vulnerabilities Directly in your IDE with Escape MCP](https://escape.tech/blog/fixing-vulnerabilities-directly-in-your-ide/)
- [Apple's App Store Source Map Leak: A Preventable Vulnerability We Found in 70% of Organizations](https://escape.tech/blog/apple-app-store-source-map-leak/)
- [How to Implement Multi-User Testing in DAST: Real-World Examples](https://escape.tech/blog/multi-user-dast-testing-real-world-examples/)
- [Top XBOW Alternatives](https://escape.tech/blog/xbow-alternatives/)
- [How Escape AI Pentesting Exploited SSRF in LiteLLM](https://escape.tech/blog/how-escape-exploited-ssrf-in-litellm/)
