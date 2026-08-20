---
schema_version: "1.0.0"
document_id: "a2b16500f7f7cfb3a686e970a98c229d9ef0f14366daffcd5fdb81dae66987aa"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-99d40c54e3ad"
canonical_url: "https://deepnote.com/changelog/2026-02-09"
published_at: "2026-02-09T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:08.376400+00:00"
fetched_at: "2026-07-28T21:26:29.837264+00:00"
content_hash: "sha256:9028d636af85ff3256e96d8dd00494edd3cdd12b2225ddcb1ceae555c1efa174"
---

# GitLab support in Git integration

## [February 9, 2026](https://deepnote.com/changelog/2026-02-09)


###


[Git upgrades & GitLab support](https://deepnote.com/changelog/2026-02-09#git-upgrades--gitlab-support)


###


[Git integration updates](https://deepnote.com/changelog/2026-02-09#git-integration-updates)


On our Git integration front, we’ve reworked the three core actions: pull, commit, and push. Everything got a more polished UI, plus a set of improvements that make day-to-day collaboration smoother.


**Support for merge conflict resolution**


Previously, pull only supported fast-forward updates, which meant it worked only when branches weren’t divergent. Now you can pull even when branches diverge, and if Git can’t resolve conflicts automatically, Deepnote will show a conflict resolution modal where you can keep either your local changes or the incoming changes.


**Pull with stash**


No need to commit local changes just to pull anymore, even if they touch files updated on the remote. Deepnote can pull with stash, and if conflicts occur (between your local-only commits and remote, or between the stash and remote), you can resolve them in the same conflict resolution flow.


**Commit and push**


The commit and push modal got a big refresh:


- Preview your changes in a clean diff editor
- Exclude files from the commit
- Switch to a new branch directly from the modal
- Get prompted to open a PR after a successful push


We’ve also included a set of smaller improvements, including a clearer branch selector (with more intuitive new-branch creation) and a one-click shortcut to open your repository on GitHub directly from the integration menu.


###


[GitLab support in Git integration](https://deepnote.com/changelog/2026-02-09#gitlab-support-in-git-integration)


GitLab is now fully supported in Deepnote’s Git integration in the Files section - not just for exports.


You can now:


- Connect GitLab repositories to folders
- Pull changes from the remote
- Commit and push local changes
- Switch between branches


One key difference compared to GitHub: for GitLab, Deepnote performs Git actions on your behalf, whereas GitHub uses Deepnote’s GitHub App. As a result, the setup flow is slightly different. Full instructions are available in the new[GitLab integration documentation](https://deepnote.com/docs/gitlab) .
