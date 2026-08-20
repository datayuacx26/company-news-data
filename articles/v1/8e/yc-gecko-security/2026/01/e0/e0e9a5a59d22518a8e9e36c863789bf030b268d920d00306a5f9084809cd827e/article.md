---
schema_version: "1.0.0"
document_id: "e0e9a5a59d22518a8e9e36c863789bf030b268d920d00306a5f9084809cd827e"
company_key: "yc-gecko-security"
company: "Gecko Security"
source_id: "yc-gecko-security-news-import-02715d60c1a6"
canonical_url: "https://www.gecko.security/blog/why-static-analysis-struggles-with-business-logic"
published_at: "2026-01-27T00:00:00+00:00"
first_seen_at: "2026-07-21T21:22:16.875303+00:00"
fetched_at: "2026-07-28T22:23:06.268498+00:00"
content_hash: "sha256:9e1340d5f13c0b9fb481e16c149ffa8f5be5139a37b57c44f8a59f293a3a4938"
---

# Why Static Analysis Struggles with Business Logic Vulnerabilities

## Introduction


If you've spent time with static analysis tools, you've probably noticed they excel at finding certain vulnerability classes while completely missing others. SQL injection, XSS, and path traversal get caught reliably. Authentication bypasses, broken access control, and privilege escalation slip through almost universally.


This pattern isn't random. It isn't something that can be fixed by tuning rules or adding more signatures. It reflects a fundamental architectural constraint in how most static analysis tools represent code.


Most SAST scanners, including many of the newer tools that have AI capabilities, are built on Abstract Syntax Trees (ASTs). ASTs provide a syntactically complete representation of source code, but they're semantically empty in ways that matter for security analysis. Understanding this limitation helps explain why entire classes of vulnerabilities remain invisible to static analysis, and what would need to change to address them.


## What ASTs Actually Provide


When a SAST tool parses code into an AST, it constructs a tree structure representing the syntax. Function declarations, variable assignments, method calls, control flow statements. For a simple function, the AST might look something like this:


yaml


```text
FunctionDeclaration:    "get_order"
├──    Parameters:   [ order_id  ,  current_user  ]
├──    Body:
│      ├──    MethodCall:    "order_service.fetch"
│      │      └──    Arguments:   [ order_id  ]
│      └──    ReturnStatement


```


This representation captures the structure of the code within that file, but it doesn't capture several things that matter for security analysis:


- Where is` order_service.fetch` actually defined?
- What type does it return?
- Is` current_user` ever used for authorization anywhere in the call chain?
- What other code paths lead to` order_service.fetch` , and do any of them include permission checks?


These questions require information that ASTs don't contain. An AST tells you the syntactic structure of code in a single file, but nothing about what that code actually does or how it connects to the rest of the system. For vulnerability classes that depend on understanding relationships between components, this limitation is fundamental.


## Why This Matters for Business Logic Vulnerabilities


Business logic vulnerabilities differ from injection vulnerabilities in an important way. Injection vulnerabilities are typically about something being present that shouldn't be, like unsanitized input reaching a dangerous sink. Business logic vulnerabilities are often about something being absent that should be present. A missing authorization check, a validation that doesn't happen, an assumption that doesn't hold.


Take the authentication bypass we recently found in[Cal.com](https://www.gecko.security/blog/caldotcom-broken-access-controls#account-takeover-via-organization-invite-token) . The vulnerability required chaining three separate bugs across different files:


1. Username validation skipped users who belonged to organizations
2. Email validation only searched within the attacker's organization scope
3. A global database upsert matched on email, overwriting the victim's credentials


No single file contained the vulnerability. Each function looked reasonable when examined in isolation. The bug existed only in the interaction between them, a three-step chain where each step's flawed assumption enabled the next.


An AST-based scanner examining any of these files individually would see a function that queries the database, conditional logic that checks organization membership, and an upsert operation with proper error handling. All syntactically valid. All part of a critical authentication bypass that the scanner can't detect.


## The Role of Taint Analysis


If you're familiar with SAST internals, taint analysis is the natural solution to this limitation. Serious tools don't just parse ASTs; they track how data flows through the program to identify when untrusted input reaches dangerous operations.


Good taint analysis tools already have sophisticated symbol resolution. Tools like CodeQL construct a full semantic database with types, call graphs, and cross-file information, because you can't do meaningful inter-procedural taint tracking otherwise. But better symbol resolution doesn't fix the underlying problem, because taint analysis is designed to answer a specific question, and that question isn't the one business logic vulnerabilities pose.


Taint analysis asks whether untrusted data reaches a dangerous operation without sanitization. This works well for injection vulnerabilities like SQL injection and XSS, which are fundamentally data-flow-to-dangerous-sink problems. But taint analysis can't tell you whether the authorization logic is correct.


The Cal.com authentication bypass illustrates this. The taint trace showed` request.email` flowing through` usernameCheckForSignup()` , then through` validateUsername()` , then to` prisma.user.upsert()` . From a taint perspective this looks correct. Input comes from the request, passes through validation functions, and reaches the database.


The bug was inside the validation function:


typescript


```text
if   (userIsAMemberOfAnOrg) {
// Skip validation entirely, leave available: true
}


```


Taint analysis doesn't evaluate whether conditional branches implement correct security policy. It tracks what flows through the program, not whether the logic is right. This is a fundamental limitation. Business logic vulnerabilities aren't data-flow questions. They're questions about correctness, about whether the code does what it should rather than just whether data flows where it shouldn't.


## Comparing Analysis Approaches


Question Taint Analysis Semantic Model


Does user input reach the database? Yes Yes


Is input sanitized before SQL execution? Yes Yes


Is there an authorization check in this path? No Yes


Is the authorization logic actually correct? No Yes


Does user context get dropped between layers? No Yes


Should this parameter be checked but isn't? No Yes


This is why adding an LLM on top of taint analysis doesn't suddenly enable business logic detection. The taint trace for the Cal.com bug looks clean. There's nothing for the LLM to flag because the underlying model doesn't surface the actual problem.


## What Finding Business Logic Vulnerabilities Requires


The gap isn't in parsing or tracing. It's in reasoning about correctness. To find business logic vulnerabilities, you need a system that can:


- Build accurate call chains across files and repos. Know that` order_service.get()` in repo A calls` handlers.get()` in repo B.
- Track parameter flow through the chain. See that` user` is available at the entry point but never passed downstream.
- Reason about what's missing. Identify that there's no point where` order.user_id` is checked against` current_user.id` .
- Evaluate conditional logic. Determine whether that` if (userIsAMemberOfAnOrg)` branch implements correct security policy.


This is where the code representation matters. Feed an LLM incomplete or inaccurate code structure and it's guessing. Give it compiler-accurate symbol resolution, type information, and full call chains, and it can actually reason about whether the code is correct.


## The Problem Compounds at Scale


In a small monolithic codebase, you might get away with AST-level analysis if your call chains are short and the entire codebase fits in context. But modern applications are increasingly multi-repo, with shared libraries, internal packages, and microservices in separate repositories. They're polyglot, with Python services calling Go services calling JavaScript frontends. And they're distributed, with API calls, message queues, and gRPC connections that don't appear in any single AST.


Consider a typical authorization pattern across services:


python


```text
# api-gateway/routes.py:
@app.get( "/orders/{order_id}"   )
def    get_order  ( order_id:  str  , user: User = Depends( get_current_user  )  ):
return   order_service.get(order_id)   # Does this check user owns order?


```


python


```text
# order-service/handlers.py (different repo):
def    get  ( order_id:  str   ):
return   repo.find_by_id(order_id)   # No user context here at all


```


The AST for each file is complete. But the vulnerability, that user authorization happens at the gateway but isn't enforced at the service layer, is invisible to any tool that can't connect these two files across repository boundaries.


AST-based tools can't resolve that` order_service.get` maps to` handlers.py:get()` . They can't trace that the` user` parameter is never passed downstream. They can't determine that` find_by_id` returns data without ownership validation.


## How Language Servers Approach This Differently


IDEs solved the cross-file resolution problem years ago. When you command-click on a function in VS Code, it doesn't pattern-match the function name. It uses a language server to resolve the actual definition across files and dependencies.


Language servers build a semantic index of code. Every symbol, its type, where it's defined, where it's referenced, and how it connects to everything else. This is the same information compilers use, and it's accurate, complete, and cross-file by design.


With a semantic index, you can answer questions like:


- Does any path to this database query include an authorization check?
- Which API endpoints expose user data without validating ownership?
- Where does this user input end up?


## Extending Semantic Analysis to Microservices


Semantic indexing solves cross-file analysis within a repository. For microservices, you need an additional layer to link services through their API contracts.


Most services define their interfaces somewhere, whether in OpenAPI specs, protobuf schemas, AsyncAPI definitions, or route decorators in the code itself. By parsing these contracts and mapping HTTP clients to the REST endpoints they call, message publishers to subscribers on those topics, and gRPC clients to service definitions, you can extend the semantic graph across service boundaries.


When you trace a call chain, it doesn't stop at` http.post("/api/orders")` . It continues into the service that handles that endpoint.


This is how you can detect vulnerabilities like:


- IDOR across services (user validated at gateway, not at data service)
- Data leakage through event streams (PII published to message queues)
- Privilege escalation via service-to-service calls (internal APIs that assume trusted callers)


## What This Means in Practice


AI coding assistants figured this out already. Copilot, Cursor, and similar tools use semantic indexing through IDE integrations or systems like GitHub's stack graphs because AST-level understanding isn't enough to write useful code. The same applies to finding vulnerabilities in it.


At Gecko, we built our scanner on language server indexing rather than AST parsing. The semantic index provides the ground truth of how code actually connects across files, repositories, and services.


On top of that foundation, we use LLMs to do what they're suited for. Reasoning about developer intent, identifying security-relevant patterns, and generating targeted test cases. The LLM isn't guessing about code structure. It's reasoning about security using accurate information about how the code actually behaves.


The result is that we can find vulnerabilities like the Cal.com authentication bypass. Multi-step chains where each individual function looks correct, but the interaction between them creates an exploitable flaw.


If you're dealing with business logic vulnerabilities that your current tools miss, or you're trying to get visibility across a microservice architecture, you can[try Gecko for free](https://app.gecko.security/) and see what it finds.
