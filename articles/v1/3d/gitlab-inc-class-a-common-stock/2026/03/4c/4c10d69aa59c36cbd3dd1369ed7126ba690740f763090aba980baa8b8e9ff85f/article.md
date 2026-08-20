---
schema_version: "1.0.0"
document_id: "4c10d69aa59c36cbd3dd1369ed7126ba690740f763090aba980baa8b8e9ff85f"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-news-import-53e8505a9d97"
canonical_url: "https://about.gitlab.com/blog/agile-planning-gets-a-boost-from-new-features-in-gitlab-18-10/"
published_at: "2026-03-23T00:00:00+00:00"
first_seen_at: "2026-07-25T06:37:24.143169+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:6131c1c0d9f62d46e907624b1baf547906ea6504bc90be7462e0233277ab08d8"
---

# Agile planning gets a boost from new features in GitLab 18.10

GitLab's Agile planning experience is getting a significant upgrade. Starting in GitLab 18.10, the new work items list and saved views bring together two long-requested capabilities: one list that displays all work item types together, and saved views that let you store and return to customized list configurations.


These capabilities help save time and effort by:


- Eliminating repetitive filter setup for common workflows
- Ensuring consistency in how teams view and assess work
- Facilitating standardized reporting and status checks


## What are work items?


Previously, epics and issues lived on separate list pages, requiring users to navigate between them. The work items list combines epics, issues, and other work items into a single, unified list experience, eliminating the need to switch between separate pages for different work item types.


This is also the foundation for deeper planning capabilities coming in the future. Bringing all work item types into one place paves the way for hierarchy views (like a Table view) that will make it easier to visualize relationships and structure across epics, issues, and other items at a glance.


Beyond list and hierarchy views, we also plan to consolidate other common workflows, like Boards, into this unified experience. The result: all of your essential planning views in one place, shareable with your team through saved views, without needing to navigate across different parts of the product.


You may be wondering why we call these "work items" rather than issues. The short answer is that "issue" doesn't scale to where we're going. Soon, you'll be able to fully configure your work item types, including their names, to match your organization's planning hierarchy. Locking the experience to legacy naming would work against that flexibility. "Work items" is the foundation for a model you can make your own.


## What led to the change to work items?


In 2024, we shared our vision for a[new Agile planning experience in GitLab](https://about.gitlab.com/blog/first-look-the-new-agile-planning-experience-in-gitlab/) , powered by the work items framework. That post outlined the core problem: Epics and issues existed as separate experiences, creating friction for teams who expected consistent functionality across planning objects. The work items framework was our answer — a unified architecture designed to deliver consistency and unlock new capabilities across GitLab's planning tools. Work items list and saved views are a step in that journey.


## What are saved views?


Saved views allow users to save and return to customized list configurations, including filters, sort order, and display options. The goal is to make routine checks more efficient and to support consistent, standardized ways of viewing work across a team.


## What's next


To understand why we are making the changes we are, it helps to picture where we're headed.


The goal isn't just a work items list; it's a planning experience that lets you move fluidly between different types of views (list, board, table, and more) while retaining your current filter scope.


Pair that with saved views, and you can create a dedicated view for each of your workflows: iteration planning, backlog refinement, portfolio-level planning with nested table views, and more.


Each view is ready to go, consistent in how it filters and displays work, and shareable with your team. This framework also sets the stage for more powerful capabilities down the road, including full swimlane support for any work item attribute in boards.


We know that changes to the tools you use every day can be disruptive. If you've built workflows around the existing epic and issue list pages, this will look and feel different. That's not something we take lightly.


This direction wasn't a decision we made quickly. It reflects years of feedback, a significant architectural investment in the work items framework, and a genuine belief that a unified experience will serve teams better in the long run. We expect the transition to take some adjustment, and we'll continue to iterate based on what we hear from you!


## Share your feedback


We encourage you try these new capabilities. Then, please reach out about your work items list and saved views experience in our[feedback issue](https://gitlab.com/gitlab-org/gitlab/-/work_items/590689) . Your comments will help us further improve these capabilities.
