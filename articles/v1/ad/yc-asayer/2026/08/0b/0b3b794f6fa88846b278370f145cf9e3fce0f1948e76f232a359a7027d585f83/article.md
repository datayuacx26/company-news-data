---
schema_version: "1.0.0"
document_id: "0b3b794f6fa88846b278370f145cf9e3fce0f1948e76f232a359a7027d585f83"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/recover-lost-commits-git-reflog/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T08:38:37.747115+00:00"
fetched_at: "2026-08-19T08:38:39.866328+00:00"
content_hash: "sha256:88dd783aee71177ed40b864ad2089838fcfa46eae543d288679ec5043d3a3d12"
---

# Recovering Lost Commits with Git Reflog

To recover a lost commit, run` git reflog` , copy the hash of the commit you want, and restore it with` git checkout -b recovered <hash>` .


You meant` HEAD~1` , typed` HEAD~2` , and watched three hours of work drop out of` git log` . Those few seconds between hitting enter and remembering the reflog exists are the worst in Git. When you run` git reset --hard` , botch a rebase, or delete a branch, your commits are almost never destroyed: they’re just orphaned, and their hashes are still recorded in your reflog. This guide gives you the fast recovery recipe for the three scenarios that cause the panic, then the hard limits worth knowing so you don’t lose the work a second time. Every command below is copy-pasteable.


## Key Takeaways


- ` git reflog` records every move of` HEAD` in your local repository (commit, checkout, reset, rebase, merge), so a “lost” commit is usually one` git reset --hard <hash>` away from being back.
- The safest recovery is` git checkout -b recovered <hash>` : rebuild the commit on a new branch and inspect it before touching your real branch.
- Reflog cannot recover uncommitted working-directory changes, because Git never recorded them in a ref in the first place.
- By default Git keeps reachable reflog entries for 90 days and unreachable ones for 30 days, so recover promptly and avoid running` git gc` until you have your commits back.
- The reflog is strictly local and never pushed, so it can rescue your own lost work but not a teammate’s unpushed commits.


## How does Git reflog work?


[Git’s reflog](https://git-scm.com/docs/git-reflog) is a local log of every position your branch tips and other refs have held. Every commit, checkout, reset, rebase, and merge moves` HEAD` and gets logged. So when a commit becomes “lost” (meaning no branch or tag points to it anymore) it is only *orphaned* , not deleted, and its hash still sits in the reflog waiting to be re-attached.


Run the command with no arguments to see the history of` HEAD` :


```text
git   reflog
```


```text
b58145c HEAD@{0}: reset: moving to HEAD~2
dd7c37e HEAD@{1}: commit: one more commit
b5b3286 HEAD@{2}: checkout: moving from main to feature
b58145c HEAD@{3}: commit: very important commit
```


Read each line as: **short hash** , the **` HEAD@{n}` index** (how many moves back that entry sits), and a **description** of the operation. In the output above,` HEAD@{0}` is the destructive` reset` that just happened, and` HEAD@{1}` (` dd7c37e` ) is the commit it moved away from, the one you want back. Under the hood,` git reflog show` runs the same thing as` git log -g --abbrev-commit --pretty=oneline` , and it takes any ref, so` git reflog show main` gives you one branch’s history.


## Recover from an accidental hard reset


A` git reset --hard` moves your branch pointer but leaves the old commit intact in the object store. Find it in the reflog, then re-point to it. The commit just before the reset is the one you lost, usually` HEAD@{1}` .


```text
git   reflog
# HEAD@{0}: reset: moving to HEAD~2
# HEAD@{1}: commit: one more commit   <-- this hash


git   checkout   -b   recovered   dd7c37e
```


The safest recovery is to inspect before you overwrite: run` git checkout -b recovered <hash>` to rebuild the commit on a new branch, verify it with` git log` , and only then decide whether to move your real branch. Once you’re sure, you can jump the branch directly:


```text
git   reset   --hard   dd7c37e       # or: git reset --hard HEAD@{1}
```


**Double-jeopardy warning:**` git reset --hard` discards any *current* uncommitted changes in your working tree. If your tree is dirty,` git stash` or use the new-branch route first. Otherwise the recovery command causes a second loss. Immediately after a reset you can also use` git reset --hard ORIG_HEAD` , because` git reset` records the branch’s previous tip in[ORIG_HEAD](https://git-scm.com/docs/git-reset) before it moves.


## Undo a bad rebase


A rebase rewrites history, and a mis-resolved conflict can silently drop commits. Reflog holds the pre-rebase tip. After a bad rebase,` git reset --hard ORIG_HEAD` snaps your branch back to where it started. But` ORIG_HEAD` is overwritten by the next reset, rebase, or merge, so if you’ve run another such command since, find the pre-rebase tip in` git reflog` instead.


```text
git   reflog
# HEAD@{0}: rebase (finish): returning to refs/heads/feature
# HEAD@{1}: rebase (pick): one more commit
# HEAD@{2}: rebase (start): checkout main
# HEAD@{3}: checkout: moving from main to feature  <-- pre-rebase tip


git   reset   --hard   HEAD@{  3  }
```


Look for the` rebase (start)` entry; the line just before it is your branch as it stood pre-rebase. If you only want *specific* commits the rebase dropped rather than undoing the whole thing, grab their hashes from the reflog and replay them:


```text
git   cherry-pick   <  has  h  >
```


## Recover a deleted branch


Deleting a branch removes the ref, not the commits. Its last commit still appears in the reflog of` HEAD` (from when you last checked it out), so find that hash and recreate the branch at it.


```text
git   reflog
# HEAD@{0}: checkout: moving from example-branch to main
# HEAD@{1}: commit: very important commit   <-- last commit on deleted branch


git   checkout   -b   example-branch   b5b3286      # or HEAD@{1}
```


` git checkout -b <branch> <hash>` recreates the branch pointing at the recovered commit, with its history intact. If you remember part of a commit message but not the hash,` git log --oneline -g --grep='<fragment>'` searches the reflog for it. (` git switch -c` is the modern equivalent of` checkout -b` ; both work, and` checkout` is not deprecated.)


## What can’t Git reflog recover?


Reflog can recover anything that moved` HEAD` or a branch tip (commits, resets, rebases, deleted branches), but it **cannot recover uncommitted working-directory changes** , because Git never recorded them in a ref in the first place. If a change was never committed, there is no ref update for the reflog to point back to, so searching the reflog for it will not turn anything up.


Reflog CAN recover Reflog CANNOT recover


Commits orphaned by` reset --hard` Uncommitted edits in the working tree


Commits dropped by a bad rebase Staged-but-uncommitted changes


Deleted local branches (recent) A teammate’s unpushed commits


Your local view of a force-pushed remote Entries already expired and gc’d


Three more constraints govern recovery:


- **It’s local and temporary.** The reflog lives only in your` .git` directory and is never pushed, so it rescues your own work but not a teammate’s unpushed commits.
- **Entries expire.** By default Git keeps reachable reflog entries for 90 days and unreachable ones for 30 days, via[gc.reflogExpire and gc.reflogExpireUnreachable](https://git-scm.com/docs/git-gc) . Orphaned commits survive until expiry plus a garbage-collection pass, so recover promptly and avoid running` git gc` until you’re done.
- **Recover onto a new branch first.** Make` git checkout -b recovered <hash>` your default move so you can verify before altering a shared branch. And write descriptive commit messages: reflog shows them, so “Fix login bug” is far easier to spot than a vague one.


**Bonus (after a force-push):** the remote server has no reflog you can read, but your local tracking ref does. To recover commits wiped from a remote by a force-push, check your local record with` git reflog show origin/<branch>` . The entry just before the force-push holds the old tip.


## Conclusion


The reflog is Git’s local safety net, and it turns nearly every “I lost my work” moment into a two-command fix: read the log, re-point to the hash. The one thing it can’t rescue is work you never committed, so the durable lesson is to commit early and often, which guarantees there’s always a reflog entry to find. Next time the terminal makes your stomach drop, run` git reflog` before anything else.


## FAQs


What is the difference between git reflog and git log?


git log shows the commit history reachable from a branch tip, following parent pointers, so orphaned commits never appear in it. git reflog shows every position HEAD or a ref has occupied in your local repository — commits, checkouts, resets, rebases, and merges — including commits that no branch points to anymore. That is why reflog can find a commit after a hard reset when git log cannot: the commit is orphaned but its ref update is still logged.


Does git reflog work after cloning a repository?


No. The reflog is stored only in your local .git directory and is never pushed or fetched, so a fresh clone starts with an empty reflog covering only actions taken since you cloned. It cannot show HEAD movements from before the clone or reveal a teammate's local history. Reflog rescues your own lost work in your own repository; it is not a shared or remote-backed recovery tool.


Can I recover a commit after git reflog entries have expired?


Not through the reflog itself. By default reachable entries expire after 90 days and unreachable ones after 30 days, and once a garbage-collection pass prunes the orphaned objects they are truly gone. Before that, you can sometimes still find the hash with git fsck --lost-found, which lists dangling commits in the object store. The reliable move is to recover promptly and avoid running git gc until your commits are back.


How do I find a lost commit if I don't know its hash?


Search the reflog directly. Run git log --oneline -g --grep='fragment' to filter reflog entries by part of a commit message, or git reflog --date=relative to scan entries by time and spot the operation you want to undo. For orphaned commits with no reflog entry, git fsck --lost-found lists dangling commits you can inspect with git show before recovering onto a new branch with git checkout -b recovered <hash>.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
