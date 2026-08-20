---
schema_version: "1.0.0"
document_id: "f79b7819cb56a34dd8d2065a3ad858c85b473ad776ef3c9b0bcbc751bee42443"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/autogpt-cve-2026-30950-session-hijack"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T22:13:02.720629+00:00"
content_hash: "sha256:9252bbd55c93e38a95ca826f41d1398d8043793cbf91d606a7509b5135f7b7fe"
---

# CVE-2026-30950 Allows Chat Session Hijacking In AutoGPT

## Summary


ZeroPath Research discovered an authenticated IDOR vulnerability in[AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) that allows any logged-in user to hijack any other user's chat session with a single PATCH request. The attacker needs only the target` session_id` — there's no requirement to share an organization, agent, or prior access with the victim.


After the hijack, the attacker reads the full conversation history of the session, and the original owner is locked out of their own data. Chat sessions in AutoGPT carry conversation history with the agent, credentials metadata, agent execution context, and any files or sensitive content the user pasted into the chat.


The issue is tracked as[CVE-2026-30950](https://www.cve.org/CVERecord?id=CVE-2026-30950) and[GHSA-q58p-v9r9-7gqj](https://github.com/Significant-Gravitas/AutoGPT/security/advisories/GHSA-q58p-v9r9-7gqj) , and was patched in` autogpt-platform-backend` version 0.6.51. MITRE assigned a CVSS 3.1 base score of 7.1 (high).


## Impacted Software


Vulnerable Versions Patched Versions


>= 0.6.36, < 0.6.51 >= 0.6.51


## Timeline


- 2026-03-06 — Issue reported to AutoGPT maintainers (private security advisory[GHSA-q58p-v9r9-7gqj](https://github.com/Significant-Gravitas/AutoGPT/security/advisories/GHSA-q58p-v9r9-7gqj) created)
- 2026-03-08 — Fix committed as[eca7b5e793](https://github.com/Significant-Gravitas/AutoGPT/commit/eca7b5e793) and shipped in` autogpt-platform-beta-v0.6.51`
- 2026-05-11 —[GHSA-q58p-v9r9-7gqj](https://github.com/Significant-Gravitas/AutoGPT/security/advisories/GHSA-q58p-v9r9-7gqj) published by the AutoGPT maintainers
- 2026-05-18 —[CVE-2026-30950](https://www.cve.org/CVERecord?id=CVE-2026-30950) published


## AutoGPT


### Background


[AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) is a workflow automation platform for building, deploying, and managing continuous AI agents. The platform's backend (` autogpt-platform-backend` ) is a FastAPI service that brokers chat sessions between end users and agents — each session carries the message history, tool invocations, and credentials context for one conversation.


### Chat sessions and ownership


Each chat session is a database object with a` session_id` (UUIDv4) and a` user_id` representing the owner. Sessions are cached in Redis (12-hour TTL) and read with a` get_chat_session(session_id, user_id)` helper that performs ownership validation when` user_id` is supplied. The helper has a dual mode: when called with` user_id=None` , the docstring explicitly notes this is "admin/system access" and the ownership check is skipped — meant for internal lookups that aren't tied to a particular caller.


The platform also exposes an` assign-user` endpoint, designed to let a freshly-logged-in user claim an anonymous session they created before authenticating. That endpoint is where the bypass lives.


## CVE-2026-30950: Hijacking any user's chat session with one PATCH


### Practical Impact


A logged-in AutoGPT user is having a private conversation with the copilot — agent output, customer details, credentials they've pasted into the chat. A completely unrelated user, signed into the same instance with no shared organization, agent, or prior access, only needs the victim's` session_id` to take the conversation over. That id is a UUIDv4, so it's not brute-forceable, but it leaks routinely through referer headers, shared links, support tickets, server logs, and screen shares.


One PATCH request later, the attacker reads the victim's entire chat history, and the victim is silently locked out of their own session — same URL, same JWT, same browser, but the chat area never loads.


The walkthrough below uses two real browser sessions against AutoGPT at the last vulnerable tag (` autogpt-platform-beta-v0.6.50` ). The green banner above each viewport is the victim, the red banner is the attacker.


#### 1. The victim's private chat


The victim is using the copilot normally. The session contains a plain-text mention of a leaked Stripe key, the rotation status, the customer name, and a draft of the customer-facing incident notification:


Nothing about this view is unusual — it's a routine internal AI workflow. The point of the walkthrough is what an unrelated user can do with the URL of this session.


#### 2. The attacker has no access (pre-exploit)


The attacker logs into the same AutoGPT instance with their own account and opens` /copilot` . The sidebar reads "No conversations yet" — expected behavior of a properly isolated multi-tenant app:


The attacker then tries to load the victim's session directly by guessing the URL —` /copilot?sessionId=<victim_session_uuid>` . Pre-exploit, the backend correctly rejects the read; the UI shows a loading spinner and the input is disabled:


So far, so good — sessions are isolated by ownership at read time.


#### 3. The exploit — one PATCH request


From the attacker's already-authenticated session, a single PATCH:


```text
curl   -X PATCH   \
-H   "Authorization: Bearer <attacker_jwt>"     \
http://  <  host  >  /api/chat/sessions/  <  victim_session_id  >  /assign-user
# HTTP/1.1 200 OK
# {"status":"ok"}
```


The endpoint accepts the request, flips the session owner in the Redis cache to the attacker, and returns 200. No interaction from the victim is required.


#### 4. The attacker reads the victim's private messages


The attacker reloads the exact URL that was blocked in step 2. This time the per-session API read (cache hit) returns` userId == attacker` , the ownership filter matches, and the full chat history renders:


The leaked key, customer name, and draft email — all visible to a user who had nothing to do with the session moments ago. This is the confidentiality impact that gives the CVE its` C:H` rating.


#### 5. The attacker's sidebar is still empty (cache-only hijack)


Worth pointing out: the attacker's session list (left sidebar) is **still** empty even after the hijack. The sidebar is rendered from the LIST endpoint, which reads straight from Postgres — and the database row's` userId` was never updated:


For the attacker this doesn't matter — they already have the URL and the read works. But it also explains why a victim won't necessarily notice anything is wrong from their own sidebar.


#### 6. The victim is locked out of their own session


The victim's sidebar still lists their session (DB unchanged). They click it — same URL they used five minutes ago. The per-session API call hits the cache, which now says` userId == attacker` , the ownership filter rejects the victim's JWT, and the chat area never loads:


This is the availability impact (` A:L` ): the victim can no longer access their own conversation history. The chat area renders the same loading-spinner state the attacker had in step 2 — a frustrating "is it broken?" UX with no surfaced error message — until the cache entry expires.


### The Details: Bypassing ownership checks by passing` None` to the data layer


#### Overview


The` PATCH /api/chat/sessions/{session_id}/assign-user` endpoint authenticates the caller but performs no check that the caller currently owns the target session. The service function it calls deliberately passes` user_id=None` to the data accessor, which the accessor treats as a privileged system lookup and returns the session regardless of who owns it. The service then overwrites` session.user_id` with the caller's ID and persists the change to Redis and the database.


#### Preconditions


- An authenticated account on the target AutoGPT instance (any standard signed-up user works).
- Knowledge of the victim's` session_id` . Session IDs are UUIDv4, so they're not brute-forceable, but they appear in URLs, referer headers, server logs, shared links, support tickets, and screen shares.


#### The three layers of the bypass


The vulnerability is established once at each of three layers, and each layer's behavior on its own looks defensible — it's the composition that breaks.


**Layer 1 — the route handler.** In` autogpt_platform/backend/backend/api/features/chat/routes.py` , around lines 753–776, the endpoint requires a valid JWT but does not check that the caller owns the session referenced in the URL:


```text
@router  .  patch  (
"/sessions/{session_id}/assign-user"  ,
dependencies  =  [  Security  (  auth  .  requires_user  )  ]  ,
status_code  =  200  ,
)
async     def     session_assign_user  (
session_id  :     str  ,
user_id  :   Annotated  [  str  ,   Security  (  auth  .  get_user_id  )  ]  ,
)     -  >     dict  :
await   chat_service  .  assign_user_to_session  (  session_id  ,   user_id  )      # ZP: no ownership check on session_id
return     {  "status"  :     "ok"  }
```


` session_id` is taken directly from the URL path and is attacker-controlled.` user_id` comes from the caller's JWT and is the attacker's own ID. Both flow into the service layer with no further gating.


**Layer 2 — the service function.** In` autogpt_platform/backend/backend/copilot/service.py` , around lines 291–303,` assign_user_to_session` fetches the session by ID with` user_id=None` , then overwrites the owner:


```text
async     def     assign_user_to_session  (  session_id  :     str  ,   user_id  :     str  )     -  >   ChatSessionInfo  :
session   =     await   get_chat_session  (  session_id  ,     None  )      # ZP: None disables ownership filter in the data layer
if     not   session  :
raise   NotFoundError  (  f"Session   {  session_id  }   not found"  )
session  .  user_id   =   user_id
session   =     await   upsert_chat_session  (  session  )
return   session
```


Other endpoints in the same file (` GET /sessions/{session_id}` ,` DELETE /sessions/{session_id}` ) correctly forward the authenticated` user_id` to` get_chat_session` , so the ownership check fires. This endpoint is the outlier.


**Layer 3 — the data accessor.** In` autogpt_platform/backend/backend/copilot/model.py` , around lines 355–366, the ownership check short-circuits when` user_id is None` :


```text
session   =     await   _get_session_from_cache  (  session_id  )
if   session  :
if   user_id   is     not     None     and   session  .  user_id   !=   user_id  :      # ZP: bypassed when user_id is None
logger  .  warning  (  f"Session   {  session_id  }   user id mismatch"  )
return     None
return   session
```


The same check exists on the database fallback path a few lines below. The docstring on` get_chat_session` documents` user_id=None` as "admin/system access," so from this function's perspective the behavior is by design — it just trusts that callers pass` None` only when they have already authorized the request themselves.


#### Persistence and lockout


After the service overwrites` session.user_id` ,` upsert_chat_session` writes the modified session to both the database and Redis. The Redis write (` cache_chat_session` in` model.py` ) serializes the full session object including the attacker's` user_id` and stores it with a 12-hour TTL. From that point on:


- The attacker calling` GET /api/chat/sessions/{session_id}` passes the now-cached ownership check and receives the full message history.
- The victim calling the same endpoint fails the ownership check (the cached` session.user_id` no longer matches their JWT subject) and receives 404 — the same lockout symptom you'd see if the session had been deleted.


A note on the DB layer:` db.update_chat_session` does not update the` userId` column directly, so initially the database row retains the original owner while the Redis cache is poisoned. However, any subsequent upsert of the session (for example, when the attacker sends a follow-up message and the session is re-saved end-to-end) writes a session record carrying the attacker's` user_id` , making the change durable beyond the 12-hour cache TTL.


## POC


[Full working POC](https://github.com/ZeroPathAI/autogpt-CVE-2026-30950-poc) with a Docker Compose setup that boots AutoGPT at the last vulnerable tag (` autogpt-platform-beta-v0.6.50` ), provisions an attacker and a victim account, and runs the single-PATCH hijack end-to-end. The POC verifies the attacker has no pre-exploit access, fires the request, then confirms ownership has transferred and the victim is locked out.


## Mitigation


- Upgrade` autogpt-platform-backend` to 0.6.51 or later. The fix is in commit[eca7b5e7](https://github.com/Significant-Gravitas/AutoGPT/commit/eca7b5e79370c34ed75e80badb824023d7d8629d) .
- If you cannot upgrade immediately, treat` PATCH /api/chat/sessions/{session_id}/assign-user` as an authenticated-but-unauthorized endpoint — block it at your gateway, or accept that any authenticated user can hijack any session whose ID they can obtain.


## Takeaways


The bug here isn't in any single function — it's in how three functions agree on responsibility. The route handler trusts the service to authorize. The service passes` None` and trusts the data layer not to expose anything dangerous. The data layer treats` None` as a privileged sentinel and trusts callers to only use it from system code. None of those individual decisions is obviously wrong if you only look at one file at a time, and that's exactly why this kind of finding survives review.


What makes it interesting from a tooling angle is that it's a pure semantic bug — there's no taint flowing into a dangerous sink, no string concatenation, no missing escape. Every value at every step is the "right" type. The flaw is that an authorization sentinel (` user_id=None` meaning "skip the check") was reachable from a code path that hadn't done the check yet. That's the kind of cross-function policy violation that's well-suited to LLM-driven analysis of how authorization decisions actually compose across a codebase, rather than pattern matching on individual sinks.
