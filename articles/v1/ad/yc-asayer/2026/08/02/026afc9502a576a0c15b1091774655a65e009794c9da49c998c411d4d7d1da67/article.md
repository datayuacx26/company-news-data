---
schema_version: "1.0.0"
document_id: "026afc9502a576a0c15b1091774655a65e009794c9da49c998c411d4d7d1da67"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/automatic-code-reviews-codeowners/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T10:22:48.360362+00:00"
fetched_at: "2026-08-06T10:22:50.657562+00:00"
content_hash: "sha256:36862bba4961881427a9ba54a7ad545732ffab1aded874df3934213312606bc8"
---

# Automatic Code Reviews with CODEOWNERS

A CODEOWNERS file is a plain-text file in your repository that maps path patterns to owners — GitHub users or teams — and automatically requests a review from those owners whenever a pull request touches a matching path. It does two jobs at once: it routes reviews to the right people without anyone pinging them manually, and it documents who owns what. The catch is that CODEOWNERS fails silently. A wrong pattern order, a team with no members, or an owner without write access produces no error dialog — the review request simply never fires, and the PR merges without the eyes you intended.


This guide gets the setup right in a few minutes, then spends most of its time on the traps: the last-match-wins rule that quietly overrides your specific rules, and the permission, empty-team, and default-branch failures that make CODEOWNERS look configured while doing nothing.


## Key Takeaways


- CODEOWNERS is **last-match-wins** : when several patterns match a file, only the last matching line assigns owners — put general rules at the top and specific overrides at the bottom.
- The file must live on the **base branch** of the PR, be under **3 MB** , use correct case, and contain valid syntax; any invalid line is silently skipped.
- CODEOWNERS alone does not block merges — you must also enable “Require a pull request before merging” and “Require review from Code Owners” in a ruleset or branch protection rule.
- An owner without **write access** is silently ignored, and a team owner must itself be visible and have write access even if every member already does.
- GitHub CODEOWNERS does **not** support` !` negation — patterns like` !README.md` are rejected as invalid.


## What does a CODEOWNERS file do?


CODEOWNERS designates individuals or teams responsible for specific paths in a repository. When someone opens a pull request that modifies a matched path,[GitHub automatically requests review from the listed owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners) . The same file format works on GitHub, GitLab, and Bitbucket; the examples here are GitHub-first.


With a growing share of PRs authored by AI agents, a CODEOWNERS rule on sensitive paths —` auth/` ,` **/migrations/` , CI config — guarantees a human owner still reviews agent changes before they land.


## How do you set up and enforce CODEOWNERS?


Put the file at` .github/CODEOWNERS` . GitHub looks in` .github/` , then the repository root, then` docs/` , and uses the first CODEOWNERS it finds, so a single canonical location prevents confusion. Write one rule per line as` pattern @owner` , then commit it to your **default branch** .


```text
# .github/CODEOWNERS
# Default owners for everything in the repo
*                   @my-org/core-team


# Frontend and backend by area
/src/frontend/      @my-org/frontend-team
/src/backend/       @my-org/backend-team


# Tests and docs
**/tests/           @my-org/qa-team
*.md                @my-org/docs-team


# Sensitive paths get a dedicated owner (put these last)
/src/auth/          @my-org/security-team
```


Ship it like any other file:


```text
git   add   .github/CODEOWNERS
git   commit   -m   "  Add CODEOWNERS  "
git   push   origin   main
```


Committing the file only requests reviewers — it does not block anything. To actually gate merges, enable two settings together: **Require a pull request before merging** and **Require review from Code Owners** . You can set these in the newer[Rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets) (Settings → Rules → Rulesets) or in a classic[branch protection rule](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches) (Settings → Branches). Both paths are live; rulesets is the newer surface.


## CODEOWNERS syntax patterns worth knowing


The pattern language follows most gitignore rules. These five cover almost everything:


```text
*                   @core-team      # global default
/api/               @backend-team   # a directory
*.ts                @frontend-team  # an extension, at any depth
**/tests/           @qa-team        # nested directory anywhere
/security/          @sec-team @compliance-team   # two owners, one line
```


An unanchored extension glob like` *.ts` matches files of that type anywhere in the repo — it behaves the same as` **/*.ts` . One rule matters when listing several owners: all owners must be on the same line. If you split them across lines, the pattern matches only the last owner mentioned. When review from code owners is required, an approval from *any one* listed owner satisfies the requirement.


One myth to kill: unlike` .gitignore` , GitHub CODEOWNERS does **not** support` !` negation. Patterns like` !README.md` are rejected as invalid — GitHub’s own docs list` !` ,` \[ \]` character ranges, and` \\#` escaping as gitignore features that do not work here. To exclude a path, assign it a different owner or arrange rules so none match it (leaving the owner column empty on a later, more specific line drops ownership for that path).


## The last-match-wins rule (the #1 mistake)


**CODEOWNERS is last-match-wins: when several patterns match a file, only the last matching line assigns owners.** Order is the single most common failure. Put general rules at the top and specific overrides at the bottom.


Here is the broken order — the catch-all lands last and silently claims everything:


```text
# WRONG — * is the last match, so @core-team owns /src/auth/ too
/src/auth/          @security-team
*                   @core-team
```


Because` *` matches` /src/auth/app.ts` *and* appears later in the file,` @security-team` is never requested. Flip it:


```text
# RIGHT — general first, specific override last
*                   @core-team
/src/auth/          @security-team
```


Now a change under` /src/auth/` requests` @security-team` , and everything else falls through to` @core-team` . Review pattern order every time you add a rule.


## Why CODEOWNERS silently doesn’t fire


Most “it’s configured but nothing happens” reports trace to one of these. CODEOWNERS is read from the pull request’s base branch, is case-sensitive, must be under 3 MB, and skips any line with invalid syntax — so a file that looks correct can still fire zero reviews.


Symptom Cause Fix


No reviewer requested at all File not on the PR’s **base branch** Commit CODEOWNERS to the branch you merge into


A specific rule never fires **Last-match-wins** — a later pattern overrides it Move general rules up, specific rules down


One line ignored, rest work **Invalid syntax** on that line — it’s silently skipped Open the file on GitHub; a “Syntax errors” link flags bad lines


Owner listed but never asked Owner lacks **write access** , or user/team doesn’t exist Grant write access; verify the handle


Merge blocked, no one can approve **Empty team** owns the path Add at least one member to the team


Path matches nothing No rule covers it Add a rule, or accept any-writer approval


No request on a draft PR **Draft PRs** don’t trigger code-owner requests Mark the PR ready for review


Rule ignored on a huge file CODEOWNERS **over 3 MB** isn’t loaded Consolidate entries with wildcards


Two permission details cause most of the quiet failures. The people you choose as code owners must have write permissions — an owner who lacks write access is silently ignored. And when the owner is a team, that team must itself be visible and have write access, even if every member already has write access individually. If you name a user or team that doesn’t exist or lacks access, no code owner is assigned — with no warning on the PR. GitHub does surface bad lines: open the CODEOWNERS file in the repo UI to see highlighted errors, which are also available through the REST API.


## Beyond static assignment: team auto-assign and Actions


CODEOWNERS maps paths to owners statically. Two mechanisms extend it when that isn’t enough.


**Built-in team auto-assignment** stops you from pinging a whole team. Under Organization → Teams → *team* → Settings → Code review, enable auto assignment:[any time the team is requested, it’s removed and a subset of members is assigned in its place](https://docs.github.com/en/organizations/organizing-members-into-teams/managing-code-review-settings-for-your-team) . Pick **round robin** , which rotates by least-recent request, or **load balance** , which levels each member’s total recent requests. Note one interaction: when a code owner is required by branch protection, the team request cannot be removed, so the individual request appears in addition to the team.


**Reach for GitHub Actions** only when assignment must depend on the diff or a label — something CODEOWNERS can’t express. A minimal workflow using[actions/checkout](https://github.com/actions/checkout) (latest[v7.0.0](https://github.com/actions/checkout/releases/tag/v7.0.0) , released June 18, 2026) plus a reviewer-assignment action on the plain` pull_request` trigger:


```text
name  :   Assign Reviewers
on  :
pull_request  :
types  : [  opened  ,   ready_for_review  ]
permissions  :
pull-requests  :   write
jobs  :
assign  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v7
with  :
fetch-depth  :   0     # needed for git diff across branches
# ...map changed paths or labels to reviewers here
```


Use the escalation ladder as a decision, not a menu: **CODEOWNERS** for static path→owner rules, **team auto-assign** to spread load within a team, **Actions** for change- or label-based logic.


Start with a single` .github/CODEOWNERS` on your default branch, order it general-to-specific, enable “Require review from Code Owners,” then open a test PR and confirm the expected owner is requested — that one check catches the silent failures before they reach production.


## FAQs


What is the difference between CODEOWNERS and GitHub's team code review auto-assignment?


CODEOWNERS is a static file that maps path patterns to owners and requests review whenever a PR touches a matching path. Team auto-assignment is an organization setting that, once a team is requested, replaces the whole-team request with a subset of members chosen by round robin or load balance. They work together: CODEOWNERS decides which team owns a path, and auto-assignment decides which members of that team actually get pinged.


Can I exclude a specific file from a CODEOWNERS rule using negation like in gitignore?


No. GitHub CODEOWNERS does not support negation, so a pattern like '!README.md' is rejected as invalid and the line is silently skipped. GitHub's docs list '!' negation, '\[ \]' character ranges, and '#' escaping as gitignore features that do not work here. To exclude a path, add a later, more specific rule assigning it a different owner, or leave the owner column empty on that specific line to drop ownership for it.


Why aren't code owners being requested on my pull request even though the file looks correct?


The most common cause is that CODEOWNERS is read from the PR's base branch, so a file only present on your feature branch never fires. Other silent causes include an owner lacking write access, a team owner that isn't visible or lacks write access, an empty team, a draft PR (which never triggers code-owner requests), a file over 3 MB, wrong-case paths, or an invalid line that GitHub skips without warning.


Does CODEOWNERS block merges on its own, or do I need branch protection?


CODEOWNERS on its own only requests reviewers; it never blocks a merge. To gate merges you must also enable two settings together: 'Require a pull request before merging' and 'Require review from Code Owners.' Configure them in a ruleset under Settings, Rules, Rulesets, or in a classic branch protection rule under Settings, Branches. Both surfaces are live, with rulesets being the newer path.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
