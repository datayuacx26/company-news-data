---
schema_version: "1.0.0"
document_id: "706f2d1cc2aea14429c99c66c4f07df1b184f3bc72f9e8ccefecaa0c6111302e"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-news-import-53e8505a9d97"
canonical_url: "https://about.gitlab.com/blog/embedded-views-the-future-of-work-tracking-in-gitlab/"
published_at: "2025-08-21T00:00:00+00:00"
first_seen_at: "2026-07-25T06:37:24.143169+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:34befdc92d9260913c0820ecb54270f81e2c13ac51fb220ba74a3bf760dbe61d"
---

# Embedded views: The future of work tracking in GitLab

Ever find yourself switching between tabs in GitLab just to keep track of what’s happening in your project? Maybe you’re checking on an issue, then jumping to a merge request, then over to an epic to see how everything connects. Before you know it, you’ve got a browser full of tabs and you’ve lost your train of thought.


If that sounds familiar, you’re definitely not alone. So many teams waste time and energy flipping through various items in their project management software, just trying to get a handle on their work.


That's why we created[embedded views](https://docs.gitlab.com/user/glql/#embedded-views) , powered by[GitLab Query Language (GLQL)](https://docs.gitlab.com/user/glql/) . With embedded views,[available in 18.3](https://docs.gitlab.com/releases/18/gitlab-18-3-released/) , you get live, relevant information right where you’re already working in GitLab. No more endless context switching. No more outdated reports. Just the info you need, right when you need it.


## Why embedded views matter


Embedded views are more than just a new feature, they're a fundamental shift in how teams understand and track their work within GitLab. With embedded views, teams can maintain context while accessing real-time information, creating shared understanding, and improving collaboration without ever leaving their current workflow. It’s about making work tracking feel natural and effortless, so you can focus on what matters.


## How it works: Real-time data right where you need it the most


Embedded views let you insert live GLQL queries in Markdown code blocks throughout wiki pages, epics, issues, and merge requests. Here's what makes them so useful:


### Always up to date


GLQL queries are dynamic, pulling fresh data each time the page loads, so your embedded views always reflect the current state of your work, not the state when you embedded the view. When changes happen to issues, merge requests, or milestones, a page refresh will show those updates in your embedded view.


### Contextual awareness


Use functions like` currentUser()` and` today()` to make queries context-specific. Your embedded views automatically adapt to show relevant information for whoever is viewing them, creating personalized experiences without manual configuration.


### Powerful filtering


Filter by fields like assignee, author, label, milestone, health status, creation date, and more. Use logical expressions to get exactly the data you want. We support more than 30 fields as of 18.3.


### Customizable display


You can display your data as a table, a list, or a numbered list. Choose which fields to show, set a limit on the number of items, and specify the sort order to keep your view focused and actionable.


### Availability


You can use embedded views in group and project wikis, epic and issue descriptions, merge requests, and comments. GLQL is available across all GitLab tiers: Free, Premium, and Ultimate, on GitLab.com, GitLab Self-Managed, and GitLab Dedicated. Certain functionality, such as displaying epics, status, custom fields, iterations, and weights, is available in the Premium and Ultimate tiers. Displaying health status is available only in Ultimate.


## See embedded views in action


The syntax of an embedded view's source is a superset of YAML that consists of:


- The` query` parameter: Expressions joined together with a logical operator, such as` and` .
- Parameters related to the presentation layer, like` display` ,` limit` , or` fields` ,` title` , and` description` represented as YAML.


A view is defined in Markdown as a code block, similar to other code blocks like Mermaid. For example:


- Display a table of first 5 open issues assigned to the authenticated user in` gitlab-org/gitlab` .
- Display columns` title` ,` state` ,` health` ,` description` ,` epic` ,` milestone` ,` weight` , and` updated` .


```text
```glql
display: table
title: GLQL table 🎉
description: This view lists my open issues
fields: title, state, health, epic, milestone, weight, updated
limit: 5
query: project = "gitlab-org/gitlab" AND assignee = currentUser() AND state = opened
```


```


This source should render a table like the one below:


An easy way to create your first embedded view is to navigate to the **More options** dropdown in the rich text editor toolbar. Once in this toolbar, select **Embedded view** , which populates the following query in a Markdown code block:


```text
```glql
query: assignee = currentUser()
fields: title, createdAt, milestone, assignee
title: Issues assigned to current user
```


```


Save your changes to the comment or description where the code block appears, and you're done! You've successfully created your first embedded view!


## How GitLab uses embedded views


Whether tracking merge requests targeting security releases, triaging bugs to improve backlog hygiene, or managing team onboarding and milestone planning, we rely on embedded views for mission-critical processes every day. This isn't just a feature we built, it's a tool we depend on to run our business effectively. When you adopt embedded views, you're getting a tested solution that's already helping GitLab teams work more efficiently, make data-driven decisions, and maintain visibility across complex workflows. Simply stated, embedded views can transform how your team accesses and analyzes the work that matters most to your success.


To learn and see more about how GitLab is using embedded views internally, check out[How GitLab measures Red Team impact: The adoption rate metric](https://about.gitlab.com/blog/how-gitlab-measures-red-team-impact-the-adoption-rate-metric/) , and Global Search Release Planning issues for the[18.1](https://gitlab.com/gitlab-org/search-team/team-tasks/-/issues/239) ,[18.2](https://gitlab.com/gitlab-org/search-team/team-tasks/-/issues/241) , and[18.3](https://gitlab.com/gitlab-org/search-team/team-tasks/-/issues/245) milestones.


## What's next


[Embedded views](https://docs.gitlab.com/user/glql/) are just the start of Knowledge Group's vision for work tracking. Learn more about what we're focusing on next in the[embedded views post-GA epic](https://gitlab.com/groups/gitlab-org/-/epics/15249) . As embedded views evolve we're committed to making them even more powerful and[accessible](https://gitlab.com/gitlab-org/gitlab/-/issues/548722) .


## Share your experience


Share your feedback in the[embedded views GA feedback issue](https://gitlab.com/gitlab-org/gitlab/-/issues/509792) or via the[embedded views GA survey](https://gitlab.fra1.qualtrics.com/jfe/form/SV_6PFhgZMBA06kr7E) . Whether you've discovered innovative use cases, encountered challenges, or have ideas for improvements, we want to hear from you.
