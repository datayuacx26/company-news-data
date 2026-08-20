---
schema_version: "1.0.0"
document_id: "927a0fa2d8a8aa1ec5826d9090397a2e7c03510a869defa34e99a7ab51d6c819"
company_key: "yc-ashby"
company: "Ashby"
source_id: "yc-ashby-news-import-72f22fd66301"
canonical_url: "https://www.ashbyhq.com/blog/engineering/github-actions-deploy-reminder"
published_at: "2022-04-04T00:00:00+00:00"
first_seen_at: "2026-07-21T07:59:34.035471+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:03d8bb07190e73278fc76b1e71a2b548440fb76ea6c32cded3e5fe3a52bf0196"
---

# Continous Deployment via a GitHub Actions Reminder

At Ashby, we leverage GitHub Actions for lots of small automated tasks. We recently introduced a reminder to deploy changes to production when the number of commits in the deploy passes a specified threshold.


For some background, we deploy by merging` develop


` into` master


` (via a merge commit).` develop


` consists of commits squashed from pull requests. We default to smaller pull requests, so commits may pile up on` develop


` if nobody needs to deploy to production right away. Suppose a production issue happens after a deploy and the deploy has lots of commits. In that case, it's harder for our team to roll back (e.g., it has database migrations) and harder to identify the commit(s) that caused the issue.


To avoid letting deploys accumulate lots of commits, we wanted to figure out a continuous deployment strategy that would keep deploys small. We considered several strategies and, for now, landed on this automated reminder. The reminder has several benefits:


- Deploys are always attended. We wanted to avoid issues where an automated solution might deploy right when everyone signs off for the day in North America or Europe.
- No changes to our deploy process were needed. We're hiring[roles to dedicate more time to improving our deployments](https://jobs.ashbyhq.com/ashby/?departmentId=2c32d70f-d7cb-4a06-bd87-048084e3eb10) , but we wanted to reduce deploy size while minimizing immediate effort.


Here's our GitHub Action in its entirety:


```text
1  name:     Please     Deploy
2   on:
3      push:
4        branches:
5          -     'develop'
6
7   jobs:
8      please-deploy:
9        runs-on:     ubuntu-latest
10        steps:
11          -     name:     Check     out     repository     code
12            uses:     actions/checkout@v3
13            with:
14              fetch-depth:     0
15          -     name:     Maybe     Ask     for     Deploy
16            run:     |
17            if [ "$(git rev-list --count origin/develop ^origin/master)" -gt "$MAX_COMMITS_BEFORE_NOTIFY" ]; then
18              curl -X POST \
19                --data '{ "channel": "C02D678LP29", "text": ":rotating_light: The deploy is getting too big! Can someone (on-call?:pray:) please deploy? https://github.com/ashbyhq/Ashby/compare/master...develop :rotating_light:" }'\
20                -H 'Content-Type: application/json; charset=utf-8'\
21                -H "Authorization: Bearer $PROD_NOTIFY_SLACK_TOKEN"\
22                https://slack.com/api/chat.postMessage
23            fi
24             env:
25              PROD_NOTIFY_SLACK_TOKEN:     ${{     secrets.PROD_NOTIFY_SLACK_TOKEN     }}
26              MAX_COMMITS_BEFORE_NOTIFY:     4
```


The GitHub Action sends an automated message to our #ping-engineering channel when` develop


` is behind` master


` by five commits, ensuring that we keep the ship shipping throughout the day!


The automated message in Slack
