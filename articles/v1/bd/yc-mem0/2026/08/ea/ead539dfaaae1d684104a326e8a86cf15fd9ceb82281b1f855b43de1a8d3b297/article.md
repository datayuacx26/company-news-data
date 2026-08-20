---
schema_version: "1.0.0"
document_id: "ead539dfaaae1d684104a326e8a86cf15fd9ceb82281b1f855b43de1a8d3b297"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/cross-session-identity-resolution-in-agent-memory"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T18:30:59.228146+00:00"
fetched_at: "2026-08-04T18:43:13.169222+00:00"
content_hash: "sha256:152f9abcbb837bc099264666f69436bf27d42a00cbf86d5a0abb848bc86e0aa8"
---

# Cross-Session Identity Resolution in Agent Memory

Say your friend likes a dark development environment and has it set on their laptop. One day you urgently log into your own mail on their laptop, and because your eyesight is weak, you switch to a light background.


Ideally the system keeps both settings separately, keyed to each mail id. But if the assistant scopes memory to the *laptop* instead of the *person* , both preferences pile into one bucket and the last change wins. Your light quietly overwrites your friend's dark. When your friend logs back in, they get a light screen they never chose. That's not a storage bug, it's an identity bug across sessions.


This is where[Mem0](https://mem0.ai/) helps. Each person's facts and preferences are saved under their own` user_id` , so your light lives under your id and your friend's dark under theirs, no collision. Once your friend logs in with their id, the assistant is looking under the right identity and serves their dark background, not yours.


## **Memory scopes to an id, not to a person**


Every memory layer partitions memory by an identifier. Mem0, for instance, scopes across four keys:` user_id` for the persistent account,` agent_id` for the agent's persona,` app_id` for the product surface, and` run_id` for a single session or ticket. You write under an id, you read back under the same id.


```text
mem  . add  (  "I prefer dark mode and terse code reviews."  ,    user_id  = "user_abc"  )
mem  . search  (  "how does this user like reviews?"  ,    user_id  = "user_abc"  )
```


```text
```


```text
```


This works perfectly, and that's exactly what hides the problem. The store hands back whatever lives under the id you gave it. It has no opinion about whether` user_abc` is the right id for the human actually typing right now. Passing a` user_id` isn't the same as knowing who the user is. One is a lookup key. The other is a decision, and the store doesn't make it for you. Mem0's identifiers scope memory, they don't resolve or merge identities across sessions, and honestly that's the right split: you don't want your memory store guessing who you are. That guessing is a layer you own.


So persistent user identity for an agent isn't a field you set once. It's a mapping you keep maintaining, from every messy signal a real person shows up with to the one canonical identity their memory should hang on.


## **Two people, one computer**


Here's the failure in its most concrete form. One shared machine, two people.


—> Checkout complete code on[GitHub](https://github.com/aashidutt-mem0/Cross-Session-Identity-Resolution-in-Agent-Memory) <—


Monday, Person A signs in and tells the assistant they write Python and want a dark editor background. Thursday, Person B signs in on the same machine and says dark hurts their eyes, they want light. Saturday, Person A comes back. What background do they get?


The whole answer hangs on one decision you made when you wrote that memory: which id you scoped it to.


Scope it to the machine and both people write into the same bucket. A's dark and B's light end up in one place, and B's was the last write in. A comes back to a light editor they never asked for. That's not a preferences glitch. It's one person's setting bleeding into another's, on the exact store that was supposed to make the thing feel personal.


Scope it to the person and A's preference lives under A's id, B's under B's, and the two never touch.


```text
# wrong: one bucket for the shared machine
mem  . add  (  "I prefer a dark editor background."  ,     user_id  = "shared-pc"  )      # Person A
mem  . add  (  "I prefer a light editor background."  ,    user_id  = "shared-pc"  )      # Person B, overwrites


# right: one bucket per person
mem  . add  (  "I prefer a dark editor background."  ,     user_id  = "user_abc"  )       # Person A
mem  . add  (  "I prefer a light editor background."  ,    user_id  = "user_def"  )       # Person B
```


```text
# wrong: one bucket for the shared machine


# right: one bucket per person
```


```text
# wrong: one bucket for the shared machine


# right: one bucket per person
```


When A comes back, the only thing deciding dark or light is the id you search under:


```text
mem  . search  (  "editor background?"  ,    filters  = {  "user_id"  :  "shared-pc"  }  )     # -> light, B's, contaminated
mem  . search  (  "editor background?"  ,    filters  = {  "user_id"  :  "user_abc"  }  )      # -> dark, A's own
```


```text
```


```text
```


Same store, same two facts. That one scope key is the whole difference between a personal agent and a leaky one. Scoping to the user, which is exactly what Mem0's` user_id` gives you, is what keeps A on dark and B on light. This is identity resolution doing its most basic job: settling "who is this" before it ever gets to "what do they like."


> **Wanna give it a try? Get a**[Mem0 API Key](https://app.mem0.ai/dashboard) **and try it yourself.**


## **What identity resolution actually has to do**


Fellegi and Sunter framed[identity resolution](https://www.zingg.ai/post/fellegi-sunter-model-limitations-modern-entity-resolution) back in 1969 as a decision with three outcomes for every candidate pair, link, don't link, or send to review, chosen to keep the error rates on both sides in check. In practice it splits into deterministic matching (the emails are identical, link them) and probabilistic matching (same device, same name, close enough, score it).


You've almost certainly used the productized version without calling it that. Every customer-data platform, Segment, Mixpanel, all of them, runs an identity graph that stitches an anonymous device to a known account the second there's enough signal. Same deterministic-then-probabilistic machinery. Agent memory just inherited the problem one level down, at the user, and it shows up as three jobs.


-


**Unify:** The same human keeps arriving under different signals: a guest token, then an email, then an SSO subject, then a second device. They all need to resolve to one identity, or the memory scatters into fragments that each look like a brand-new user.


-


**Isolate:** Two different humans must never land on the same identity. A shared family tablet, a generic` support@` inbox, one too-eager merge, and suddenly one person's memory is scoped to someone else's id. In a memory system that isn't a glitch, it's a leak.


-


**Merge:** When you find out two ids were the same person all along, maybe a guest who just signed in, maybe a duplicate account nobody caught, you have to fold one memory graph into the other and retire the empty one, without dropping anything and without counting it twice.


Get these right and memory feels personal the way people actually want. Get unify wrong and the agent has amnesia. Get isolate wrong and you've got a confidentiality breach.


## **Fuzzy match**


If every returning user politely logged in with the same email every time, this would be a lookup, not a problem. What is they don't.? What if they browse anonymously and buy later? or They start on web and finish on mobile? or They sign up once with a work email and again through SSO? Most of the signal you get is partial, and every interesting decision lives in the gap between "obviously the same person" and "obviously not."


That gap is where you need scoring. Weight each signal, add up what an incoming session shares with a known identity, and check the total against two thresholds:


-


Above the **link** threshold: confident enough to unify on the spot.


-


Below the **no-link** threshold: treat as a new person.


-


In between: **review** . Don't auto-merge. That middle band isn't a failure, it's your safety valve.


A verified email is deterministic and should link on its own. A shared device is much weaker, because a shared device is exactly how two people get confused for each other, so it should drop a candidate into review, not link it. Name plus device together can clear the bar; device alone shouldn't. Concretely:


```text
W_EMAIL  ,    W_DEVICE  ,    W_NAME   =  1.0  ,    0.5  ,    0.3
LINK  ,    REVIEW   =  0.8  ,    0.4


def    resolve  (  signals  ,    profile  )  :
if    signals  . get  (  "email"  )    in    profile  [  "emails"  ]  :
return    "link"                             # deterministic, score 1.0
score   =  0.0
if    signals  . get  (  "device"  )    in    profile  [  "devices"  ]  :  score   +=  W_DEVICE
if    signals  . get  (  "name"  )     ==  profile  [  "name"  ]  :     score   +=  W_NAME
if    score   >=  LINK  :    return    "link"              # e.g. device + name
if    score   >=  REVIEW  :  return    "review"            # e.g. device alone: hold, don't merge
return    "new"
```


```text
W_EMAIL  ,    W_DEVICE  ,    W_NAME   =  1.0  ,    0.5  ,    0.3
LINK  ,    REVIEW   =  0.8  ,    0.4


def    resolve  (  signals  ,    profile  )  :
if    signals  . get  (  "email"  )    in    profile  [  "emails"  ]  :
return    "link"                             # deterministic, score 1.0
score   =  0.0
if    score   >=  LINK  :    return    "link"              # e.g. device + name
return    "new"
```


```text
W_EMAIL  ,    W_DEVICE  ,    W_NAME   =  1.0  ,    0.5  ,    0.3
LINK  ,    REVIEW   =  0.8  ,    0.4


def    resolve  (  signals  ,    profile  )  :
if    signals  . get  (  "email"  )    in    profile  [  "emails"  ]  :
return    "link"                             # deterministic, score 1.0
score   =  0.0
if    score   >=  LINK  :    return    "link"              # e.g. device + name
return    "new"
```


That's Fellegi-Sunter in fifteen lines, and it's the whole difference between recognizing an anonymous returning user and either ignoring them or handing them someone else's memory. Tune the weights and thresholds however you like. The shape is the point: deterministic signals link outright, fuzzy signals climb toward a threshold, and there's a review band sitting in the middle for everything ambiguous.


## **The two failure modes have very different blast radii**


The review band earns its place because the two ways to be wrong don't cost the same.


-


Fragmentation is the loud, annoying one. The returning user gets treated as new, re-states everything, and walks away sure your memory feature doesn't work. You lose the retention story that justified building memory in the first place. Bad, but quiet.


-


Collision is the scary one. When identity resolution collapses two people onto one id, one user's private context surfaces inside another user's session. Preferences are the harmless end of that. The harmful end is the health detail, the salary number, the relationship an agent inferred from one person's messages and then served to someone else, because both were typing into the same under-specified identity. That's the leak that turns a wrong merge into a security incident instead of a papercut.


So the thresholds aren't arbitrary. A false merge costs far more than a missed one, so you set the link bar high, let the review band catch the ambiguous cases, and eat the occasional missed unification as the cheap mistake. An aggressive resolver chasing a slick returning-user demo optimizes the cheap failure and walks straight into the expensive one.


## **The test: deterministic, fuzzy, review, isolate, merge**


So I wrote a small demo that drops this resolver in front of Mem0 and checks every job against real stored memory, not assertions.


***Five moves:***


-


**Deterministic unify:** Person A signs in on web with a verified email, stores a preference, then comes back on mobile and logs in with that same email. Deterministic match, so they link to their canonical id and the preference is just there.


-


**Fuzzy unify:** Same human, but this time an anonymous web session with no email at all, only the shared web device and a name off a form. Device plus name clears the link threshold, so the resolver recognizes them without a login. I check recall two ways: naive, treating the raw session as its own id (empty), and resolved, after the fuzzy link (the preference comes back). That gap is the entire payoff of fuzzy resolution.


-


**Review, withheld:** A session turns up with nothing but the shared web device. That lands in the review band, and the resolver refuses to auto-merge, because that device could just as easily be a second person on the same machine. This is the exact case an aggressive resolver leaks on.


-


**Isolate:** A second human, Person B, resolves to their own id with the opposite preference. I check that Person B's id never returns Person A's memory, and I assert the leakage count is zero instead of eyeballing it.


-


**Merge duplicate graphs:** Person A turns out to have two accounts, a web signup and a separate SSO signup under a different email, each with its own memory graph. A dedup pass scores the SSO account against the existing one, matches on shared device and name, and folds the two graphs into one. I check the count before and after and confirm nothing got stranded under the retired id.


The only thing changing between the amnesiac agent and the one that knows its user is that resolver in the middle. Same store, same data, same model.


## **Results**


Run on gpt-4o-mini with a local embedder or via API, here's what the resolver actually did:


**Check**


**Result**


Fuzzy recall, naive (raw session id)


0 memories


Fuzzy recall, resolved (fuzzy link, device + name, score 0.8)


1 memory, the preference recovered


Shared-device-only session (score 0.5)


held for review, not linked


Cross-user leakage hits


0


Duplicate-graph merge


1 memory moved, canonical went 1 to 2, 0 stranded


The row that matters is the fuzzy pair. Same human, same stored preference, and the only variable is whether an anonymous session with no email got resolved before the search ran. Naive came back empty. Resolved returned the preference. That empty cell is every returning user your agent is quietly failing to recognize just because they didn't log in first, and the resolver closed it on a device-plus-name match with no login at all.


The rest is the safety side holding up. The device-only session scored 0.5 and stayed in review instead of auto-linking, which is the case that leaks. Person B resolved to their own id and saw only their own preference, none of Person A's. And when the duplicate SSO account got merged, its pricing memory moved into the canonical identity with nothing left behind, so a later search for that person returns both memories.


## **Doing the fuzzy match responsibly**


Probabilistic matching is the job, but it's also where you can quietly hurt a real user. Three rules keep it honest.


-


**Let deterministic signals link, and make the fuzzy ones earn it.** A verified email or an authenticated SSO subject can link on its own. A device, an IP, a name, a fingerprint, none of those should ever link alone; they accumulate toward a threshold and send the ambiguous ones to review. Tuning isn't optional here. A wrong link is a data leak, so put the bar where false merges are rare and let the review band soak up the rest.


-


**Treat a merge like a write to your most valuable record.** Mem0 has no move operation, so a merge reads the memories under the source id, re-adds them under the target, and clears the source:


```text
def    merge  (  from_id  ,    into_id  )  :
for    m    in    mem  . get_all  (  user_id  = from_id  )  [  "results"  ]  :
mem  . add  (  m  [  "memory"  ]  ,    user_id  = into_id  )
mem  . delete_all  (  user_id  = from_id  )
```


```text
def    merge  (  from_id  ,    into_id  )  :
for    m    in    mem  . get_all  (  user_id  = from_id  )  [  "results"  ]  :
mem  . add  (  m  [  "memory"  ]  ,    user_id  = into_id  )
mem  . delete_all  (  user_id  = from_id  )
```


```text
def    merge  (  from_id  ,    into_id  )  :
for    m    in    mem  . get_all  (  user_id  = from_id  )  [  "results"  ]  :
mem  . add  (  m  [  "memory"  ]  ,    user_id  = into_id  )
mem  . delete_all  (  user_id  = from_id  )
```


Keep provenance on what you moved and which signals justified it, so a bad merge can be traced and undone instead of quietly poisoning a real account. A merge touches the record with a person's name on it. Handle it like that.


-


**Keep anonymous memory provisional.** Before a session resolves to anyone, scope its memory to a session id (` run_id` ) and treat it as tentative. Promote it into a real identity only once a match clears the link threshold. If it never does, it expires with the session instead of contaminating someone's account.


### **Further reading**


-


Binette and Steorts,[(Almost) All of Entity Resolution](https://arxiv.org/abs/2008.04443) , on the deterministic-vs-probabilistic matching problem this all descends from.


-


Fellegi and Sunter, "[A Theory for Record Linkage,](https://www.tandfonline.com/doi/abs/10.1080/01621459.1969.10501049) " *JASA* 64(328), 1969, the original decision-theoretic model.


-


Rasmussen et al.,[Zep: A Temporal Knowledge Graph Architecture for Agent Memory](https://arxiv.org/abs/2501.13956) , on graph-based memory that dedups entity nodes inside the store.


-


New America OTI,[AI Agents and Memory](https://www.newamerica.org/oti/briefs/ai-agents-and-memory/) , on the privacy stakes when memory crosses sessions and services.


Mem0 is an intelligent, open-source memory layer designed for LLMs and AI agents to provide long-term, personalized, and context-aware interactions across sessions.


Get your free API Key here:[app.mem0.ai](https://app.mem0.ai/)


If you are an agent, sign up using` mem0 init --agent --json` , or self-host Mem0 from our open-source[GitHub repository](https://github.com/mem0ai/mem0) .


## **Frequently asked questions**


### Q. Isn't this what` user_id` is for?


` user_id` is the destination, not the map. It tells the store where to read and write. It doesn't tell you that the anonymous mobile session is the person who was on web, or that the SSO signup is a duplicate of an account you already have. Working out which` user_id` a given human maps to, across signals and sessions, is the part the store leaves to you.


### **Q. Can I match an anonymous session to a logged-in user without a login?**


Yes, and that's the whole point of fuzzy resolution. Score the anonymous session's signals (device, name, behavioral fingerprint) against your known identities. Clears the link threshold, unify it; lands in review, hold it; too weak, keep it separate. You get the returning user back without forcing a login, which is exactly the retention you were leaving on the table.


### **Q. Why not just merge aggressively so returning users always get recognized?**


Because the two failure modes aren't symmetric. Failing to unify costs a good experience. Wrongly merging two people costs a data leak. An aggressive resolver optimizes the cheap failure and walks into the expensive one. Set the link bar high, send the ambiguous cases to review, and take the occasional missed unification as the safe error.


### **Q. Two accounts turned out to be the same person. How do I merge their memory?**


Confirm the match on a strong signal first (ideally a verified email, or a review-band case a human approved), then fold one graph into the other: read every memory under the duplicate id, re-add it under the canonical one, retire the duplicate. Keep provenance so the merge stays auditable and reversible. Step five of the demo does exactly this.


### **Q. How is identity resolution different from just isolating tenants?**


Tenant isolation (app or org scoping) keeps companies apart. Identity resolution works inside a tenant, keeping individual people correctly unified and separated. You need both. Nailing tenant isolation does nothing to stop two users inside the same app from colliding on an under-specified id.


### **Q. Does the memory framework matter for this?**


Not much. Mem0, a graph store, your own vectors, whatever you use, the store scopes by an id and you own the mapping to it. The resolution layer sits above the store and ports across all of them.
