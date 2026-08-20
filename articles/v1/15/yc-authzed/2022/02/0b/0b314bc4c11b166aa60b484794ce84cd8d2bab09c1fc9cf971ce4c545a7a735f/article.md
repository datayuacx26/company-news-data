---
schema_version: "1.0.0"
document_id: "0b314bc4c11b166aa60b484794ce84cd8d2bab09c1fc9cf971ce4c545a7a735f"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/our-github-merge-workflow"
published_at: "2022-02-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:cbb020c22854496838d526009910d1b20500e188dfa385f8e46dfe319ee72801"
---

# Our GitHub merge workflow

Here at Authzed we’ve become heavy users of GitHub. We have many public, open source repos (including[SpiceDB](https://github.com/authzed/spicedb) ), a private, internal monorepo, and the accompanying workflows for each repo built on top of GitHub actions. Our development workflow also revolves around GitHub issues and pull requests.


Managing GitHub settings and distributed software development should be as straightforward as the zipper merge but sadly it isn’t.


(This analogy might not apply if you live in *\[insert city with bad drivers here\]* .)


We thought it’d be useful to share our current pull request and merge workflow and how we arrived at it. And we have a request for the community at the end of this post.


## Development workflow


First, a bit about our development workflow. We use GitHub Actions to run tools for testing and code quality, with a continual push to automate as much of our workflow as possible. Looking at the workflows in the SpiceDB repo as an example, you can see a good representation of the variety of concerns handled by automation: building, linting, testing, compliance, and releasing. And we’re not just users of GitHub actions but authors of some custom actions as well.


[SpiceDB Validate](https://github.com/authzed/action-spicedb-validate) : this action validates SpiceDB schemas using test data.


[SpiceDB](https://github.com/authzed/action-spicedb) : this action runs a SpiceDB server to use during integration testing.


Second, we are firm believers in code reviews. We typically use one pull request per feature and view the interaction during the review process as the perfect opportunity to share knowledge: the reviewer gets to learn about the code changes as well as the author getting to express the intent of the change.


1. A pull request can start as a draft which allows the author to push intermediate changes to get feedback from the actions run on each commit.
2. The draft PR is marked as Ready for Review when the feature is complete and a reviewer is assigned.
3. The reviewer checks out the PR branch, runs it in their local workspace, and kicks the tires while reviewing the code.
4. We do our best to maintain a 24 hour turnaround time for reviews and there may be several back and forths based on the reviewer’s feedback.
5. Finally, to emphasize the expectation that a PR is a self-contained, complete feature, anyone (but usually the reviewer) can merge an approved PR.


## Merge workflow


Quality vs Velocity Quality + Velocity: quality and velocity do not have to be in tension but can be counterparts. The aim of our merge workflow is to maintain strict quality standards while enabling the team to consistently deliver code. Confidence in our quality enables us to deliver more quickly, which gives us the time and momentum to continue investing in quality improvements. (Who doesn’t like a[flywheel](https://omr.com/en/flywheel-marketing/) ?).


The following is our repo and branch protection settings we’ve landed on:


### Pull requests


*Allow merge commits, disable other options*


- ✅ Merge commits provide a clear record of development history: incremental commits are maintained and the introduction to the main branch is captured
- ❌ Squash merging loses feature branch development history
- ❌ Rebase merging doesn’t record the moment of feature integration


✅ *Enable auto-merge*


- Frees the developer/reviewer from attending to the status checks of PRs
- Note: this option must be used in tandem with requiring branches to be up to date before merging, otherwise a branch with stale passing checks can be merged onto the main branch


### Branch protection


✅ *Require a pull request before merging / Require approvals*


- See the above section on code review.


✅ *Dismiss stale pull request approvals*


- We want a PR to be a fully contained change. PRs should be mergeable and therefore complete as they are reviewed. Partial PRs don’t count


✅ *Require status checks to pass*


- This setting acts as **“require the selected status checks to pass”** , not **“require all status checks on the PR to pass”** . This may be significant if different sets of status checks apply to certain PRs. For example, PRs that update a particular subdirectory in your repo may run additional checks. These additional checks would not be checked by this setting.


✅ *Require branches to be up to date before merging*


- Prevents merging with stale passing checks


✅ *Include administrators*


- Admins are not immune from making mistakes


❗Disable *Allow force pushes* and *Allow deletions*


Our merge workflow works well…for now. It strikes a good balance between completeness of checks without requiring too much developer intervention. However, we already know we’ll need to update our workflow sooner than later. When multiple PRs are ready to merge, we have a race to see which PR can merge first. Losers of that race then have to rebase, run all checks again, and race to merge again. Thankfully for the developer, GitHub provides easy to use buttons to update the PR branch and automatically re-runs actions. Nonetheless, this is an unnecessary use of computing resources and time.


## On to the next


Short answer: we’ve gone shopping for a merge queue solution. The plethora of anecdotes from engineering teams indicates we’re not alone in concluding a merge queue addresses the aforementioned issues but we find ourselves with a paradox of choice.


- [https://github.com/kubernetes/test-infra/tree/master/prow/tide](https://github.com/kubernetes/test-infra/tree/master/prow/tide)
- [https://github.blog/changelog/2021-10-27-pull-request-merge-queue-limited-beta/](https://github.blog/changelog/2021-10-27-pull-request-merge-queue-limited-beta/)
- [https://shopify.engineering/introducing-the-merge-queue](https://shopify.engineering/introducing-the-merge-queue)
- [https://eng.uber.com/ios-monorepo/](https://eng.uber.com/ios-monorepo/)
- [https://medium.com/strava-engineering/butler-merge-queue-how-strava-merges-code-7095a3310930](https://medium.com/strava-engineering/butler-merge-queue-how-strava-merges-code-7095a3310930)
- [https://mergetastic.com/docs/whats-a-merge-queue/](https://mergetastic.com/docs/whats-a-merge-queue/)
- [https://bors.tech/devdocs/bors-ng/readme.html](https://bors.tech/devdocs/bors-ng/readme.html)
- [https://mergequeue.com/](https://mergequeue.com/)
- [https://blog.mergify.com/what-is-a-merge-queue/](https://blog.mergify.com/what-is-a-merge-queue/)


Do you have a recommendation for a merge queue service or solution? Any additional merge workflow anecdotes to add? We'd appreciate your input so drop into our[Discord channel](https://authzed.com/discord) !


If there is a lesson to takeaway for now: **it’s never too early to set up a good merge workflow.**


Hopefully with your input we can find the merge queue solution for us and keep our dev flywheel spinning quickly.


On this page


- Development workflow
- Merge workflow
- Pull requests
- Branch protection
- On to the next


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
