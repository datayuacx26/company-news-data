---
schema_version: "1.0.0"
document_id: "e27c8a75b119371ad4fe33085d7cfab909d97304e2b5e1dd5bd8b426a028f040"
company_key: "yc-warpbuild"
company: "WarpBuild"
source_id: "yc-warpbuild-news-import-6421ae0a6624"
canonical_url: "https://warpbuild.com/blog/design-files-onboarding"
published_at: "2023-12-15T00:00:00+00:00"
first_seen_at: "2026-07-22T19:23:06.881853+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:c45af351de35b9ab537c736bcba06227ffc8878e0216258dd662c5fd626fb294"
---

# Design files: Onboarding

WarpBuild's value proposition is built around a fundamental invariant - everybody wants builds to be faster, and everybody wants things cheaper\[1\].


Now, that means our primary job shifts from convincing developers to use our product to making people aware of the fact that our product exists. This shortens the funnel significantly. Once the user signs up to try the product, the onus is on the onboarding flow to ensure there is no drop-off and that is a challenge we took up very seriously.


We laid out some principles to deliver on this promise:


- Onboarding must be trivially easy
- High degree of polish is required for the product, from day 1
- Speed is a feature and critical to developer experience


## The onboarding process


Switching a GitHub Actions workflow to WarpBuild is a one line change in the workflow file. Easy right? Not really. It's very common for repositories to have tens or even hundreds of GitHub Actions workflows.


Our single highest priority for a signed up user is that we want all the user's workflows to run on WarpBuild. Changing 100s of workflows manually is frustrating, obviously. Asking our users to do that would be conducive to exactly two things: having our users hate us and ensuring they don't switch workflows.


We set out to solve this while designing the UI to:


- Pull information from GitHub about the connected repositories. Then parse and display the workflows present.
- Raise a PR with the user selected runner configuration for all selected workflows in one click.
- Open the PR in a new tab so users can review and merge.


This is how our onboarding workflow looks like now and it worked brilliantly!


## The results


We had users move 50+ workflows in a few minutes and[got good HN karma](https://news.ycombinator.com/item?id=38572160) because of this. Our users love it! While the numbers are small to provide statistically significant analysis, we continue to see very high funnel progression and conversion to paid usage.


📚 Check out the[documentation here](https://www.warpbuild.com/docs/ci)


⚡️[Get started with WarpBuild in 2 minutes here](https://app.warpbuild.com/)


---


\[1\] Variant of[this quote by Jeff Bezos](https://www.goodreads.com/quotes/966699-i-very-frequently-get-the-question-what-s-going-to-change) .
