---
schema_version: "1.0.0"
document_id: "f43353c3926eefb375e519efdc77938d0f5454e6b2c841dfcecf206f6b2913a2"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-99d40c54e3ad"
canonical_url: "https://deepnote.com/changelog/2026-04-27"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:08.376400+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:410c2ddaeee4185710987ceb42e6d7861e2e17e007fe8d4408f57d906d6866e2"
---

# Run snapshots, Git sync, & AI usage visibility

## [April 27, 2026](https://deepnote.com/changelog/2026-04-27)


###


[Run snapshots, Git sync, & AI usage visibility](https://deepnote.com/changelog/2026-04-27#run-snapshots-git-sync--ai-usage-visibility)


####


[TL;DR](https://deepnote.com/changelog/2026-04-27#tldr)


- **Run snapshots** - every notebook run now saves an immutable record of blocks, outputs, and execution metadata, so you can jump back to any past run in a click.
- **Git sync** - connect a Deepnote project to a Git repo and keep them in sync automatically.
- **AI usage visibility** - workspace admins can now see every AI action taken, who triggered it, and the tokens it used.


##


[Run snapshots](https://deepnote.com/changelog/2026-04-27#run-snapshots)


Every notebook run now automatically saves an immutable snapshot — a frozen record of all blocks, outputs, and execution metadata from the moment the run finished. Open any past run from the **Runs sidebar** , your project logs, or the **Logs & Analytics** modal.


Scheduled notebook broke overnight? A teammate's run produced an unexpected number? Want to see what a chart looked like last Tuesday? Until now, you'd re-run and hope. Now you've got the full historical record one click away, so debugging, auditing, and reviewing someone else's work just gets easier.


Full details in[the docs](https://deepnote.com/docs/run-snapshots) .


##


[Git sync](https://deepnote.com/changelog/2026-04-27#git-sync)


You can now connect a Deepnote project to a Git repo and keep them in sync automatically. That means you can:


- **Edit locally:** take a project from Deepnote Cloud to your machine, work on it, and push changes back.
- **Put notebooks in your Git flow:** pull requests, code review, proper history.
- **Actually review notebook diffs:** the` .deepnote` format is human-readable, so changes are finally reviewable.


Full details in[the docs](https://deepnote.com/docs/deepnote-file-sync) .


##


[AI usage visibility](https://deepnote.com/changelog/2026-04-27#ai-usage-visibility)


Workspace admins can now see a full breakdown of AI activity in the **AI settings** page:


- Every AI action taken in the workspace.
- Which user triggered it, and from which notebook.
- Token usage per request, including cached tokens.
