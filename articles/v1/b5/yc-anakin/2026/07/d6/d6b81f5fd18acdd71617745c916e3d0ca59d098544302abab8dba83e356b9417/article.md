---
schema_version: "1.0.0"
document_id: "d6b81f5fd18acdd71617745c916e3d0ca59d098544302abab8dba83e356b9417"
company_key: "yc-anakin"
company: "Anakin"
source_id: "yc-anakin-news-import-edbd07d03db6"
canonical_url: "https://anakin.io/blog/revoke-wire-access-what-each-deletes"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-08-05T00:49:53.361744+00:00"
fetched_at: "2026-08-05T03:48:37.411401+00:00"
content_hash: "sha256:4a93f10d0f7f2a75dee5c17d72851aceb73208b537ba50b196e644678c2b7145"
---

# 5 Ways to Revoke Wire's Access - What Each One Actually Deletes

Most vendors document how to connect. Far fewer document how to disconnect, and almost none tell you precisely what survives when you do. This is the other half: how to revoke API access once you've granted it, and what actually gets deleted.


That's backwards. Access you can't withdraw isn't access you granted - it's access you lost. So here is every way to cut Wire off, ordered from the smallest to the largest, with exactly what each one removes.


## The five levers


**1. Delete a session.** Removes the stored session for one account. The identity and its vault binding stay. The next task fails with` AUTH_EXPIRED` until someone logs in again.


**2. Delete an identity.** Removes one connected account and every session under it. Other identities on the same vault are untouched.


**3. Disconnect the source, keep identities.** The vault connection goes. Bound sessions keep working until they expire, then stop refreshing - and each one is flagged "source disconnected" so nothing fails silently. This is the migration path, not the offboarding one.


**4. Disconnect and delete the identities.** The vault connection and every identity sourced from it, removed together in one atomic operation. Nothing half-deleted if something fails partway.


**5. Revoke in your own vault.** The nuclear option, and the only one that doesn't require Wire to cooperate. Delete the Service Account or pull the role assignment. Effective instantly, from a system we don't control.


## Why the vault lever is the important one


Levers 1-4 are Wire doing what you asked. Lever 5 is you removing the ability to ask.


That's a meaningful difference in a security review, because it doesn't depend on our software behaving correctly, our API being reachable, or our company still existing. You granted a scoped credential from a system you own; you can withdraw it there, and no code of ours participates.


What Wire does do is *notice* . The next time a read comes back rejected, the source is marked` revoked` rather than retried into a loop, and identities depending on it surface as broken instead of quietly returning stale data. Revocation you can see is worth more than revocation you have to trust.


## The destructive one has its own address


A design detail worth surfacing, because it's the kind of thing that only matters once.


Levers 3 and 4 differ by a single word in their outcome and enormously in consequence - one keeps your identities, one deletes them. The obvious implementation is a flag: same endpoint,` ?delete_identities=true` .


We didn't do that. The destructive variant is a **separate URL** , so deleting a customer's identities cannot happen through a stale bookmark, a copy-pasted query string, or a retried request that picked up the wrong parameter. You have to call the endpoint that does the destructive thing, on purpose.


> The safe path and the irreversible path should not be one typo apart.


## What "delete" means here


Worth being exact, because the word does a lot of work in vendor documentation.


A delete in Wire removes the row. It is not a status flag, not a soft-delete with a hidden restore, not a tombstone that a support engineer can undo. The encrypted session is gone; there is nothing left to decrypt.


Two consequences follow, and one of them is a genuine limitation:


- **Deletion is immediate.** No queue, no nightly job, no eventual consistency window.
- **Deletion is something you trigger.** There is no background sweep that removes old sessions on a schedule. Sessions become useless on their own - they expire, and an expired session authenticates nothing - but the encrypted row persists until someone deletes it or replaces it by logging in again.


If your policy requires stored material to be purged on a fixed clock, that's an explicit step today, not something to assume. We'd rather say so than let you infer a sweep that doesn't run.


## The one thing none of these reach


A task that is already running.


Wire loads a session at the start of a job and holds it for that job's duration, so revoking partway through stops the *next* task rather than the current one. The window is usually seconds - occasionally a minute or two on a slow site - and after it, every subsequent task fails.


If that window matters to you, the move is to sign out everywhere on the site itself. That invalidates the session at the source, including the copy a running job is holding, which is the only action that reaches inside a job already in flight.


## An offboarding checklist


When someone leaves, or an engagement ends, or you're just closing an account:


- **Revoke in your vault first.** It's immediate and it doesn't wait on us.
- **Then disconnect and delete the identities** (lever 4) to remove the stored sessions rather than leaving them to expire.
- **Check for other sources.** Multiple vault connections per account are supported, and a dev vault is easy to forget.
- **Account for imported sessions.** A session imported from a browser isn't backed by a vault at all, so revoking in your vault doesn't touch it. Those only go away when you delete them in Wire - worth checking for explicitly, because they're the ones a vault-first offboarding misses.
- **Sign out everywhere on the site itself** if it offers it. That invalidates sessions at the source - including any we still hold - and is the only step that reaches sessions outside our system.


None of this requires a support ticket, and none of it requires our help. That's the point.


**Reference.** Every lever described here is a single API call, and all of them are yours to make without asking us. The[Azure Key Vault guide](https://anakin.io/blog/azure-key-vault-wire-rbac-scoping) walks through revoking at the vault itself.


→[API reference · Identities & credentials](https://anakin.io/docs/documentation)


---


[Back to blog](https://anakin.io/blog)
