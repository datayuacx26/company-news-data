---
schema_version: "1.0.0"
document_id: "fb9ceae87ed53fca1f3469ad9bcbd12d5f699e4db220355c09bf1ef2df2204e3"
company_key: "yc-axolo"
company: "Axolo"
source_id: "yc-axolo-news-import-c5f1135929e7"
canonical_url: "https://axolo.co/blog/p/how-legend-cut-pr-review-time-with-axolo"
published_at: "2025-10-08T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:48.922612+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:a055415d44f7136c2928b492fb112048e42e213a79840ece8b34743dec962ed8"
---

# How Legend cut PR review time (and kept sanity) with Axolo

John is a Senior Engineer at **Legend** . We sat down to discuss how Axolo changed their pull‑request workflow across five squads, cutting review delays and reducing Slack noise.


You can watch the recording here, or skim the highlights below.


## The context: squads, scale, and speed


Legend’s product team has ~30 engineers organized into five squads, collaborating with a wider group of ~200 engineers for cross‑cutting initiatives. Day‑to‑day work happens within squads; larger architecture and long‑term planning involve the broader group. Their goal each week: collaborate well, avoid incidents, and show real progress — ideally faster than planned.


## Before Axolo: fragmented and noisy


- PR notifications were inconsistent: some via email, some via ad‑hoc Slack links, others by manually checking GitHub.
- The official GitHub ↔ Slack app dumped everything into one channel — noisy, easy to ignore, and not targeted to the right reviewers.
- Result: developers waiting on reviews they didn’t know existed; PRs idled in Jira’s “waiting for review” column; context switching spiked.


## Why Axolo: focused channels, real-time collaboration


With Axolo, each PR opens a dedicated Slack channel scoped to the PR’s author and reviewers (often GitHub teams matching squads). This immediately reduced noise and made the “next action” obvious.


How the team actually uses it:


- **Triage & timing:** John waits for CI checks to pass in the PR channel, then reviews — no wasted effort on red builds.
- **Where to discuss:** Code-level comments stay on GitHub; quick back‑and‑forth and decisions happen in Slack for speed.
- **Channel hygiene:** Channels are grouped in Slack sections; Axolo archives them on merge/close, so the list stays manageable.


## ““Reviews used to take days — now we typically turn PRs around in a few hours.””


— John says review time dropped by about half, based on their original adoption period. The bigger win? Sanity — fewer places to check, less noise, and a clearer path to action.


## Measured impact (and what they remember)


Legend first adopted Axolo years ago, so some metrics aren’t readily available today. But two signals stood out:


- **Review time ≈ cut in half.** John recalls ~50% improvement during the initial rollout.
- **Cycle time: days → hours.** PRs that waited days now routinely move within hours, without sacrificing review quality.


> “We’ve always held a high bar for reviews. Axolo didn’t lower the bar — it helped us hit the same quality faster.”


## Onboarding & adoption


An engineer familiar with Slack tooling suggested trying Axolo. The product team piloted it, liked it, and later helped standardize it across the org. Most engineers prefer it; those with strong personal workflows can **pause** Axolo individually without disrupting others.


- **Setup was simple:** link GitHub, map squads/teams, go.
- **Cultural fit:** the team already lives in Slack — bringing PRs there removed friction.


## Tips from Legend: making Axolo sing


- **Use GitHub Teams & CODEOWNERS** to target the right reviewers and avoid “review everyone” spam.
- **Let Slack do the fast talk:** keep code-diff comments on GitHub; make decisions in the PR channel.
- **Organize Slack sections** so open PR channels float to the top.
- **Gate on green:** don’t start reviews until checks pass.


## Wishlist the team shared (we’re listening)


1. **Surface sync/auth errors** between GitHub and Slack so re‑auth is obvious when needed.
2. **Account sync improvements** (e.g., bulk email changes reflected more smoothly).
3. **Slim standup summaries** — a compact, bullet‑style digest option.
4. **More granular notifications** (e.g., tailor which PR events post back, per repo/team).
5. **Channel grouping** for PRs within Slack sections (limited by Slack API today).
6. **Auto‑leave after approval** and **per‑user/per‑repo overrides** for workflow preferences.


## How we’d pitch Axolo to a skeptic (John’s take)


If you’ve tried email or the default GitHub Slack app, you’ve felt the noise. Axolo creates **just‑enough** channels with **just‑the‑right** people, keeps updates in the tool you already use, and closes the channel when you’re done. It’s simply a cleaner mental model for modern PR flow.


## TL;DR


- **Problem:** Fragmented notifications, noisy channels, slow PR pickup.
- **Solution:** Per‑PR Slack channels with the right reviewers, fast back‑and‑forth, automated archiving.
- **Impact:** Reviews from **days → hours** and **~50% faster** overall, with **more sanity** and **less context switching** .


## Axolo is a Slack app to help tech


teams review pull request seamlessly


[Sign up](https://axolo.co/)
