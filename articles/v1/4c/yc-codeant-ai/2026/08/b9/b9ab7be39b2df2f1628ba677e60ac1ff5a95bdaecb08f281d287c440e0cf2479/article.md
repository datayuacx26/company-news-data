---
schema_version: "1.0.0"
document_id: "b9ab7be39b2df2f1628ba677e60ac1ff5a95bdaecb08f281d287c440e0cf2479"
company_key: "yc-codeant-ai"
company: "CodeAnt AI"
source_id: "yc-codeant-ai-news-import-b68a5af7b5b5"
canonical_url: "https://codeant.ai/blogs/ai-code-review-ssrf-authorization-bypass-apache-superset"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T02:05:34.248566+00:00"
fetched_at: "2026-08-18T02:05:35.079628+00:00"
content_hash: "sha256:c6a13d46c17412b66be9ca60942302f48c8194b0c049470ab2602f18575cbf29"
---

# AI Code Review for Security: Finding SSRF and Auth Bypasses

Most of what an automated reviewer does on a[pull request](https://codeant.ai/blogs/top-pull-request-automation-tools) is unremarkable. Naming, a missing null guard, a test that asserts the wrong half of a tuple.


This article is about the other kind. Two findings on[Apache Superset](https://github.com/apache/superset) , a project sitting at 74.3k GitHub stars and roughly 400 open pull requests at the time of writing, where the security control was present, executed, and passed.


Both bugs were in code that was being written to *close* a security gap. That is the part worth your time.


> **What contextual review solves here:** a scanner asks whether a security check exists on a path. These two bugs need a different question, which is what the check asserts about.
>
>
> Specifically, whether the object it inspects is the same object the code touches two lines later. That gap is invisible in a diff unless you hold both halves in the same frame.


The two pull requests are public.[#42930](https://github.com/apache/superset/pull/42930) is open as of August 17, 2026, and[#42936](https://github.com/apache/superset/pull/42936) merged on August 11, 2026.


Every claim below traces to a commit you can read.


## What Is SSRF, and How Does a Proxy Hop Defeat Peer Validation?


Server-side request forgery is a request your server makes on an attacker's behalf, to a destination the attacker picks and you would never allow directly. The classic prize is an internal address that trusts anything originating inside your network.


Superset PR #42930 added a defense against exactly this on dataset imports from data URLs. New helpers in` superset/utils/network.py` and a pair of peer-validating connection classes in` superset/commands/dataset/importers/v1/utils.py` .


The design is genuinely good. Instead of trusting a hostname allowlist, which DNS can re-point between validation and fetch, it inspects the socket after the connection is up and rejects unsafe peer addresses on every hop.


### How does SSRF typically exploit APIs from inside a trusted service?


Hostname-based defenses fail because the name is not the thing you connect to. A name resolves at connect time, and an attacker who controls the DNS record can hand you a public address during validation and a private one a millisecond later.


Checking the connected peer closes that door. You stop reasoning about what a name claims and start reasoning about the address your socket actually reached.


That is a real improvement over an allowlist, and it is why the finding that follows is interesting rather than embarrassing.


### Why the allowlist and the peer check both passed


` urllib` installs a default` ProxyHandler` . When an HTTP or HTTPS proxy is configured in the environment, the connection socket is established to the proxy, not to the destination.


The peer check inspected that socket. It saw the proxy's public address, found nothing unsafe about it, and returned cleanly.


The proxy then did what proxies do. It resolved the destination the caller asked for and fetched it, including addresses the peer check was written to forbid.[CodeAnt's](https://codeant.ai/) review comment on the pull request stated it plainly.


The maintainer's reply was short. Proxies disabled on the guarded path, so the connection goes straight to the destination and the peer check validates the real target.


### How to prevent SSRF when an egress proxy is in the path


The shipped fix is commit`[3431995](https://github.com/apache/superset/commit/343199578e3951f87265b5aecdbc6401485070d2)` , which disables proxy usage on the import fetch so peer validation applies to the destination.


A follow-up commit,`[243164f](https://github.com/apache/superset/commit/243164f54886455c4bd1b02599fc4ecc34c85a82)` , added a unit test asserting that` load_data` 's opener disables proxies when internal data URLs are not permitted, and recorded the behaviour change in` UPDATING.md` .


That second commit exists because the human reviewer on the PR raised the operational cost. Deployments that need an egress proxy to reach legitimate external URLs are affected by this, and a release note is the honest way to ship it.


If your own service validates a peer address, resolved IP, or TLS certificate before a fetch, the question to answer is which hop you are inspecting. Any intermediary between your socket and the destination means the two are different machines.


## How Does an Authorization Bypass Survive a Broken Access Control Check?


The second finding lives in PR #42936, a smaller pull request closing two authorization gaps. The relevant half concerns dataset repointing.


Superset lets an editor change a dataset's` database_id` . The original code verified ownership of the dataset and stopped there, which meant editorship of one dataset implied reach into any database connection in the deployment.


The PR fixed that by routing the change through` security_manager.raise_for_access` with the resolved` Database` object, mirroring a check the create path already performed. Correct instinct, and the check was now in the diff.


### Why the access check ran against a stale table


The legacy` /datasource/save/` view called` raise_for_access` against the dataset's *current*` table_name` ,` schema` , and` catalog` .


Then` update_from_object` ran, and applied whatever` table_name` ,` schema` , and` catalog` the request supplied.


So a caller passes the authorization check against a table they are legitimately allowed to touch, and the write repoints the dataset at one they are not. The check is real, it runs, and it grants permission for a state that no longer exists by the time the write lands.


CodeAnt's review comment landed on` superset/views/datasource/views.py` . The resulting commit,`[abaec3c](https://github.com/apache/superset/commit/abaec3cf4207594a2a067af30e98d7621ff0a8ef)` , is titled "check access against requested table, not stale one" and constructs the` Table` object from the request payload before the access check instead of after it.


### How this differs from a classic IDOR vulnerability


An insecure direct object reference is the absence of a check. You pass an ID that belongs to someone else, nothing verifies ownership, you get their record.


This is the presence of a check aimed at the wrong record. The distinction matters because it changes what finds the bug.


IDOR / missing check


Wrong-subject check


What the diff shows


No authorization call on the path


An authorization call, correctly written


What a scanner reports


Missing access control on a mutating route


Nothing


What the fix changes


Adds a check


Moves or re-parameterises an existing check


What catches it


Route coverage rules, access-control linting


Reading the call site alongside what runs after it


Both land under[OWASP A01 Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/) . Only one of them shows up in tooling built to detect an omission.


### What the merged fix changed


PR #42936 merged as commit` 5248367` on August 11, 2026, after a human reviewer walked both halves and confirmed the repoint path now runs the target-database check before reassigning` database_id` .


The other half of that PR, an ownership guard on` TabStateView.delete_query` , was authored by a Superset contributor before the automated review ran. Worth naming, because the point of this article collapses if attribution gets loose.


## What Do These Two Bugs Have in Common?


One is a network-layer SSRF. The other is application-layer authorization. Different files, different subsystems, different threat models.


They are the same bug.


### The anatomy of wrong-subject validation


In both cases a guard asserts a property about an object, and the code then acts on a different object.


The peer check asserted that the proxy's address was safe. The fetch went to the destination.


The access check asserted that the current table was permitted. The write applied the requested table.


Nothing is missing from either diff. The assertion is simply about something else, which is why reading the guard in isolation tells you nothing.


### Why race condition and TOCTOU intuitions apply without threads


Developers reach for[time-of-check to time-of-use](https://cwe.mitre.org/data/definitions/367.html) reasoning when they see threads, locks, or filesystem races. Neither of these bugs has any of that.


The gap is not temporal in the concurrency sense. It is a gap between *what was measured* and *what was used* , and a single-threaded request handler produces it just as reliably as a race.


If you already audit for TOCTOU, the same instinct works here. Ask what changed between the check and the use, and treat "the request payload got applied" as a change.


## Why Does SAST Miss This, and How Is That Different From DAST?


Static analysis works on structure. It asks whether a control exists on a path, whether tainted data reaches a sanitizer, whether a call graph has a hole in it.


Both of these paths have a sanitizer. Both have an authorization call. Taint reaches a control in each case, the control is a real one, and the analysis terminates clean.


### SAST vs DAST vs contextual review


Each approach can see a different slice of this, and none of them is a superset of the others.


SAST


DAST


Contextual code review


Sees the control exists


Yes


No


Yes


Sees what the control asserts about


No


No


Yes


Sees what executes after the control


Partially


Only through behaviour


Yes


Needs the vulnerable path reached


No


Yes


No


Catches the SSRF proxy hop


No


Only with a proxy configured in the test environment


Yes


Catches the stale-table repoint


No


Only with a crafted two-table payload


Yes


Dynamic testing could in principle find both. It would need an egress proxy configured in the test environment for one, and a crafted two-table payload for the other.


That is a lot of setup for a bug you do not yet know exists.


### What automated code review has to model to see it


To flag the SSRF, a reviewer needs to know that` urllib` installs a default` ProxyHandler` , that a proxy terminates the socket, and that the peer check therefore describes an intermediary. That is library semantics, not syntax.


To flag the repoint, a reviewer needs to read` raise_for_access` and` update_from_object` as a sequence, and notice that the second reads fields the first did not consider.


Both require holding two locations in mind and reasoning about what one asserts regarding the other. That is the specific thing[CodeAnt AI](https://www.codeant.ai/) is built to do on a pull request, and it is also, for what it is worth, exactly what a strong human reviewer does on a good day.


## What Does Secure Code Review Look Like in an Open Source Repo Like Superset?


Superset carries around 400 open pull requests at any given moment. No maintainer reads all of them closely, and the ones that get the deepest attention are usually the ones that look risky.


A hardening PR does not look risky. It is a security fix, written by a member, adding controls that were missing.


That is precisely where a fresh pair of eyes earns its keep, because everyone reviewing it is predisposed to read the presence of a check as the end of the question.


### Signal quality is the whole game


A second automated reviewer ran on PR #42930 and posted three additional suggestions on the same diff.


The maintainer worked through them and rejected two as misreads, with a traced explanation of why the code was correct as written. That is the tax a noisy reviewer charges, and it is paid in maintainer attention, which is the scarcest resource an open source project has.


A finding is only worth posting if it survives the maintainer reading the code around it. Both findings described in this article did, and both produced a commit.


### How AI code review works on pull requests in practice


The mechanics are unglamorous. The reviewer runs on the diff, has the surrounding files available, posts inline comments on the lines it can defend, and the maintainer accepts or argues.


No gate, no blocking status check, no severity dashboard anyone has to triage. A comment either changes the code or it does not.


## How Do You Test for Wrong-Subject Validation in Your Own Code?


This class does not need a tool to start hunting. It needs a question applied to code you already have.


For every security check in your codebase, name the object it asserts about, then name the object the next twenty lines use. Where those differ, you have a candidate.


### Five patterns worth grepping for


Each of these produces the same shape as the two Superset bugs.


-


**A check on a resolved value, then a re-resolution.** DNS lookups, symlink resolution, and redirect following all re-resolve after validation. Validate the final hop or pin the resolved value.


-


**A check on current state, then a write from request state.** Any` check(entity) ... entity.update(**payload)` sequence. Build the check's subject from the payload.


-


**A check on a socket or connection, with an intermediary in the path.** Proxies, service meshes, and connection pools all mean the peer is not the destination.


-


**A check on one field of a composite key.** Verifying` database_id` while` schema` and` catalog` travel freely in the same request is a partial assertion.


-


**A check inside a cached function.** If the cache key omits the requesting principal, the first caller's authorization result is served to the second.


### How to test for SSRF behind a proxy


Configure` HTTP_PROXY` and` HTTPS_PROXY` in a test environment and re-run your existing SSRF test suite.


Tests written against a direct connection pass whether or not the proxy path is guarded, which makes a proxied run the cheapest coverage you can add.


If the proxied run passes an address your direct run rejects, you have reproduced this exact bug.


## Conclusion: AI Code Review Needs Context, Not Just Pattern Matching


The important lesson from these[vulnerabilities](https://codeant.ai/vulnerability-database) is not simply that AI can find security bugs. It is **how** it finds them.


An SSRF vulnerability can look like an ordinary URL-handling flow until you trace where the value comes from, where it ends up, and what validation happens in between. An authorization bypass can look like a legitimate permission check until you determine **whose identity is actually being validated against which resource** .


That is where contextual[AI code review](https://codeant.ai/ai-code-review) becomes useful.


Instead of looking for isolated patterns, it can reason across data flow, function relationships, validation logic, and authorization context to identify security issues that depend on how the application actually works.


For engineering teams, the goal isn't to replace every existing security control. It's to catch these vulnerabilities while the code is still being changed, with enough context for developers to understand and fix them.


If you want to see what contextual[AI code review](https://codeant.ai/blogs/best-ai-code-review-tools) can find in your own codebase,[try CodeAnt AI's AI-powered code review](https://codeant.ai/ai-code-review) and see how it surfaces security issues with the surrounding code context.
