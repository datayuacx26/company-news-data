---
schema_version: "1.0.0"
document_id: "a21207e19a25e047d16c2bd58af224cd59856b8edcc4c7e7c0682dc14106b95c"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-gha-log-search"
published_at: "2025-10-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:55:32.133421+00:00"
content_hash: "sha256:5ec5ad5d86f134bcb6a37ba25651c3bb61bdb0af190e93a28cfe3985719ca040"
---

# Now available: Github Actions log search

One of the biggest pain points with GitHub Actions? No CI log search across your various jobs. At Depot, we're obsessed with observability, so we decided to fix it ourselves. Today, we're excited to announce GitHub Actions log search: a feature that lets you search across all your historical CI logs (from this date forward) with powerful filtering capabilities. It's available right now to all Depot users on every plan.


**Key features** :


- Search by keyword across all repositories and workflows
- Apply smart filters to dynamically update results for only relevant repositories, workflows, and runner types by timeframe
- Adjust the context of your search results by configuring the number of lines to display before and after your result.
- See the complete build log in context with one click.


## How to search GitHub Actions logs


Head over to your organization's GitHub Actions page and click **Logs** .


If you've used the Job detail page before, this will feel familiar. By default, you'll see a day's worth of CI logs across all your repositories. To load additional logs, just scroll to the bottom.


### Filter logs


To filter by time range, select a **Timeframe** from the dropdown in the upper right.


Configure filters to help you focus your results by repository, workflow, action, and runner type. The filters update dynamically based on your timeframe and selections:


- **Repository** : Show only repositories that had CI builds during your selected timeframe.
- **Workflow** and **Actions** : Show only the workflows and actions that have run in your selected repositories.
- **Runner** : Show only runners that were used by your selected repositories.


### Search for keywords in logs


To search for specific keywords, use the search bar. The results show all matching log lines across your previous builds. You can scroll down to load more results.


To see more context around your matches, click the three-dot icon next to the search bar and adjust how many additional lines appear above and below each result.


To see a match in the context of the whole build, click on a specific log line to load the complete set of logs for that build.


## What’s next?


Our customers asked for GitHub Actions log search and we’re finding it incredibly useful for our own work. We're keeping a close eye on how people use this feature to see where we can improve log retrieval time and overall performance.


At Depot, we're all about building tools that boost developer productivity. GitHub Actions log search is just the beginning: it's laying the groundwork for some other cool features we have in the pipeline.


We hope you find the log search helpful for debugging and troubleshooting your GitHub Actions. If you have any questions, feedback, or ideas for how we can make this even better, please hop into our Community Discord and let us know!


## Related posts


- [Introducing GitHub Job Details: Observability for your CI/CD pipeline](https://depot.dev/blog/introducing-github-job-details-observability-for-your-cicd-pipeline)
- [A practical guide to debugging GitHub Actions](https://depot.dev/blog/guide-to-debugging-github-actions)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Introducing Depot managed GitHub Actions Runners](https://depot.dev/blog/depot-github-actions-runners)
- [Introducing Ultra Runners — Up to 3x faster GitHub Actions jobs](https://depot.dev/blog/introducing-github-actions-ultra-runners)


Peter Hasko-Nagy


Staff Engineer at Depot
