---
schema_version: "1.0.0"
document_id: "0540248fd084e41f3c8748bf4b4b10432af24a3de400e09ff826ae3c14c31f2a"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/review-scrutiny"
published_at: "2022-01-11T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T22:26:34.525443+00:00"
content_hash: "sha256:fb17e267eefb69bd2bed65add5b0df5bfbcfcb7b61a6c595af095e17aa43abd3"
---

# Review Scrutiny

Our team within Engineering Enablement serves our engineers so they can build the technology products that are used every day by our guests, team members, and partners. One way to do that successfully is to look for sources of friction that slow engineers down and find ways to reduce them. Case in point – our own GitHub pull request workflow for reviewing our colleagues’ changesets. Our


[monorepo](https://www.atlassian.com/git/tutorials/monorepos) contains 28 packages of varying scopes, from user-facing command-line tools, componentry, and configuration to documentation sites, internal modules, and tumbleweeds of dependencies.


This central hub of code deploys automatically after exhaustive continuous integration (CI) system test assertions. This process has allowed us traditionally to move a lot of code from development to production cleanly. As our team has grown, however, we noticed opportunities for improvement – not in the code, but in the process.


Requesting Clearance to Land


As nimble practitioners, we try to meet and converse live as a team to reduce the amount of written documentation we create, preferring to keep as much of it in or near the code as possible. This manifests in the pull request, which, for our team, is treated as a public forum where changes are discussed before we reach agreement.


We dedicate significant time to each pull request because we often go back to them months or years later to reference a system decision. The author is tasked with communicating information about the scope, trade-offs, validating changes, and any areas that may need additional eyes. Release notes are structured off blocks of text introduced in the pull request, and we version our packages semantically based on labels under the procedures of


[semver.](https://semver.org/)


While the upfront accounting of the change is beneficial, requiring the entire team to look at everything can also create bottlenecks, comparable to requiring every person to attend every meeting. That might make sense on a small team of two, but in a larger team like ours, sprint after sprint, we found there was always more code to review than time to write our own. What began as an earnest attempt to keep everyone in the loop was creating too much friction and slowing us down.


The tools were arrayed against us too:


- GitHub’s


[CODEOWNERS](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/about-code-owners) feature ensures everyone on the core team is alerted to new pull requests.


- A sprint-based


[project board](https://docs.github.com/en/issues/organizing-your-work-with-project-boards/managing-project-boards/about-project-boards) automatically tracks new things to review.


- Branch protection enforces that a


[defined number of approvals](https://docs.github.com/en/github/administering-a-repository/defining-the-mergeability-of-pull-requests/about-protected-branches#require-pull-request-reviews-before-merging) accompany every change.


However, what we found was that the review thresholds often didn’t match the scope. A single number couldn’t apply to the spectrum of work that 24 packages presented us with during the sprint, and our pull request process was low-trust. Well-known programmer Patricia Aas summarized the issue in a tweet:


Review Scrutiny


When making a pull request, Review Scrutiny is another upfront consideration our team now makes when writing a pull request. It’s a risk mitigation calculation that we think through before asking others to review.


Now on our team, each pull request comes pre-filled with this new Markdown section:


```text
```       ## Review Scrutiny   Because this change `has this impact`, this pull request requires `N` approval(s).       ```
```


Some examples of pull requests that would only require one person’s approval include a change limited to internal comments and tests or a change that affects unreleased code.


A change that affects a public API would require two approvals and a pull request for a change that impacts the team’s process would require three approvals.


To encourage engineers to use the new Markdown section, we added a new checklist item to our


[pull request template.](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository#adding-a-pull-request-template) An accompanying HTML comment visible only while editing markdown reminds the team and contributors how to determine Review Scrutiny. When a pull request reaches the number of approvals, Review Scrutiny defines it may be merged.


Sometimes this means it exceeds the branch protection minimum. Oftentimes, the Review Scrutiny threshold is lower, leading to an admin merge. We trust one another to make the right upfront risk assessment instead of depending automatically on what an external tool suggests is a releasable change. 👋 GitHub, if you are listening, please allow defining the number of approvers per pull request.


Impact and Results


We introduced Review Scrutiny to our team’s process at the end of May 2021. From our perspective, it’s felt like an intuitive part of our workflow and ultimately a win. As a writer and reviewer of pull requests, I now find myself wondering where Review Scrutiny is on our repositories outside of the monorepo.


But what really struck me was when I glanced at our pull request time-to-merge metrics.


In the four weeks prior to the introduction of Review Scrutiny, our time-to-merge averaged 208 hours. In the proceeding four weeks, and sustained even eighteen weeks out, our average is 129 hours. This is a


38% reduction


in time it takes our code to be merged. All from a simple process change added to our pull request template.


Pull request authors know best what sort of change they are introducing. We streamlined our process by placing more trust in each other. Try it out and see if it helps you and your team as well!
