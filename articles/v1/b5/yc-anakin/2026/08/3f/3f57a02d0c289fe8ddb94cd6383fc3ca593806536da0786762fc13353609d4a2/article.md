---
schema_version: "1.0.0"
document_id: "3f57a02d0c289fe8ddb94cd6383fc3ca593806536da0786762fc13353609d4a2"
company_key: "yc-anakin"
company: "Anakin"
source_id: "yc-anakin-news-import-edbd07d03db6"
canonical_url: "https://anakin.io/blog/prove-password-never-reaches-log"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T00:49:53.361744+00:00"
fetched_at: "2026-08-05T01:38:03.459815+00:00"
content_hash: "sha256:785fbe85cff066d066bfdc2076a0c57d822f67b1b4d5a9bb5b8a9077ba398d42"
---

# How We Prove a Password Never Reaches a Log Line

Every vendor security page says they don't log credentials. It's the easiest sentence in the document to write and one of the hardest to keep true, because nobody breaks it on purpose.


Here's how it actually breaks.


An engineer is chasing a bug in a vault integration at four in the afternoon. The connection object isn't behaving. They add one line - log the struct, see what's in it - reproduce the bug, fix it, and ship. The debug line goes with it. It's one line, it looked harmless, and the reviewer had eleven other files to read.


That struct had a password field. It's now in your log aggregator, replicated, indexed, and retained for as long as your retention policy says. Nobody did anything malicious. Secrets in logs are never the plan; the policy said don't log them and everyone agreed with it.


> A rule that depends on every engineer remembering it, forever, under deadline, is not a control. It's a hope.


## Make the type refuse


So we don't rely on remembering. Every type in Wire that holds cleartext - vault tokens, resolved credentials, connection blobs - renders **redacted** , by construction. Log it and you get a placeholder. The debug line above prints nothing sensitive, because the object has no representation that reveals itself.


Which sounds like a solved problem, and isn't quite - because Go, like most languages, has several ways to turn an object into a string, and they don't all go through the same door.


The obvious ones respect a custom string method.` %#v` - the syntax representation - does not. It reaches past your formatting and prints the raw struct. So does a value nested inside a slice, or a map, or another struct, or wrapped in an error, depending on how it's reached. Implement the obvious method, test the obvious case, and you've covered maybe half the paths a value can take to a log line.


## The test


So there's a single test file whose entire job is to answer one question: *can a secret reach a log line?*


It takes each type that holds cleartext, fills it with a recognisable canary value, and renders it every way a value can plausibly end up in a log:


- Every formatting verb, including the ones that bypass custom formatting
- Inside a slice
- Inside a map
- As a field of another struct
- Wrapped in an error
- Serialised to JSON


Then it asserts the canary appears in **none** of them.


Add a field that holds a secret and forget to redact it, and the test fails before the code merges. Not in review, where a human has to notice. Not in production, where a customer's credential is already in CloudWatch. In CI, on the commit that introduced it.


The threat this defends against is completely mundane, which is exactly why it needs a machine watching for it. Nobody is going to deliberately log a password. Someone is absolutely going to add a debug line at four in the afternoon.


## Three more properties in the same spirit


The pattern generalises: prefer controls the system enforces over rules people follow.


**Buffers are zeroed after use.** When a credential is decrypted to be used, the plaintext buffer is overwritten once it's consumed rather than left for the garbage collector. It narrows the window in which a secret exists in process memory - which matters for anything that captures memory, including a crash dump.


**A failed connection saves nothing.** Connect a vault with a bad token and no row is written. Not written-then-marked-invalid - not written. There's no partial state to clean up, and no half-configured connection to be confused by later. Either the credential worked at the moment you gave it to us, or it doesn't exist here.


**A reference can't point outside its scope.** This one is a real attack, not a hypothetical. A credential binding names where to read a secret from. If that reference were taken at face value, a crafted one could aim a customer's own credential at a location of the caller's choosing - using their access to read something they never granted. So the scope is checked on every resolution. A reference pointing outside the vaults the connection was granted is rejected before any call is made, and there's a test named for exactly that behaviour so nobody refactors it away.


## Why we'd rather show you this


Security pages are a genre, and the genre has a problem: every one of them says the same things. Encryption at rest. Least privilege. No credentials in logs. The words are identical whether the engineering behind them is thorough or aspirational, which makes them almost useless for telling the two apart.


A test is different. A test either passes or it doesn't, it runs on every commit, and it fails on the engineer who broke it rather than on the customer who discovers it.


It also fails on us. That's the part worth stating plainly: these controls exist because we assume our own engineers will make ordinary mistakes under ordinary pressure. Building on that assumption produces better systems than building on the assumption that everyone will remember the policy.


**Ask us anything.** Security review, questionnaire, or an architecture question this didn't answer - we'd rather have the conversation than send you a PDF.


→[Talk to our team](https://anakin.io/products/wire) ·[Docs](https://anakin.io/docs/documentation)


---


[Back to blog](https://anakin.io/blog)
