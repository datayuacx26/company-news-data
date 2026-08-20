---
schema_version: "1.0.0"
document_id: "90e25d0eda05801bfeccd75b76ffe0f925b6cad4c112c788aca486725d4910f0"
company_key: "yc-haystack"
company: "Haystack"
source_id: "yc-haystack-news-import-51ea83245eb5"
canonical_url: "https://www.usehaystack.io/blog/7-jira-delivery-metrics-for-software-dev-teams"
published_at: null
first_seen_at: "2026-07-21T22:30:19.779153+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:7ce6d6c260ece6e52990fc57e394f11dda5e02bb0df40b99aa48f1567534ab84"
---

# 7 Jira Delivery Metrics for Software Dev Teams

Haystack primarily focuses on getting software development data from version control systems (like GitHub, GitLab and Bitbucket). This way we collect data from where engineers actually work, so the data is as accurate as it can be. There are however some signals that are best to capture from a project management tool like Jira. Below we’ll outline 7 of the key Jira delivery metrics that Haystack surfaces to software engineering teams in our dashboard. Customers who need these metrics can access them by requesting our Jira integration from their account team.


*Jira Delivery add-on in Haystack.*


## Issue Cycle Time


Haystack uses Git data to calculate the *Change Lead Time* of software development work (time from the first commit to the work deployed into production). This is a part of the software development lifecycle that the engineering team fully own and can make improvements to optimise.


By contrast, when software developers are working together with product managers (or programme managers, scrum masters, etc), it is essential to ensure that work is broken down into small chunks so that business value can be delivered to customers in small increments so that the product managers can get quick feedback from real-world customers in order to improve the development roadmap in future. Shipping work in smaller chunks is safer, less stressful and makes more business sense.


This metric can be optimised by both the software engineering team and the product team working together to optimise Cycle Time.


Learn more about the difference between[Lead Time, Cycle Time & Change Lead Time](https://www.usehaystack.io/blog/lead-time-cycle-time-change-lead-time) .


## Full Resolution Time for Bugs (or Mean Time to Resolution)


DevOps teams will often consider MTTR to mean *Mean Time to Recovery* , the time to resolve an incident. This metric is often highly dependent on what is considered an incident, many ordinary customer bugs can be important to track for this metric (given the speed of fixing a bug can often be[inversely correlated to customer satisfaction](https://youtu.be/N3ipaOCKoYs) ).


By instead tracking[Full Resolution Time for bugs](https://www.engprod.guide/docs/full-resolution-time-for-bugs) , or Mean Time to Resolution, you are able to identify how quickly your team is able to resolve customer issues and respond to challenges when they come up.


## Issues in Multiple Sprints


Tracking issues that span multiple sprints is a great way of identifying work that is not properly broken down or that the team are stuck on. By drilling into these metrics, you can then identify the rogue Jira issues that need expediting.


## Issue Throughput


How much work is actually being completed in your sprints? Are you spreading your team too thin and causing burnout? Throughput allows you to visualise how many issues your team is able to get through week-over-week, and flag potential burnout as necessary.


## Sprint Scope Change


When issues are added to a sprint after it has already started, this can mean the team is working beyond its capacity and work that might not have been properly prioritised is being focussed on. By identifying issues that have been added mid-sprint, you can identify and mitigate these issues.


## Unlinked Pull Requests


Even if you require that Pull Requests contain a Jira label, they can sometimes be bypassed by invalid Jira IDs, like “ *PROJ-XXX* ” or “ *PROJ-0* ”. To ensure Pull Requests are correctly linked to Jira tickets, and match against the work that you’re trying to achieve, tracking Unliked Pull Requests allows you to manage this risk more effectively. This metric is calculated using both Git and Jira data in conjunction and allows you to ensure your Pull Requests are correctly tracked against Jira tickets.


## Pull Requests per Issue


Pull Requests per Issue is a fairly simple metric but can be a good proxy metric to understanding whether too much work is being grouped into one Jira story, but also whether there are too many places that developers need to make changes before they can push changes live. Whilst this metric shouldn’t be used as a goal in its own right, drilling down on this measure allows you to identify deeper issues.


## Conclusion


In this blog post, we’ve explored 7 Jira delivery metrics that are surfaced in Haystack. These metrics can be used alongside or in combination with Git data. These metrics provide powerful insights for an Engineering Manager to work with a Product Manager to improve processes collectively. If you’d like access to these metrics, reach out to your Haystack account manager.
