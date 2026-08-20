---
schema_version: "1.0.0"
document_id: "31f1b7cd3926dff1a1ccc9ec62486ca9336dcbb8bf1a4d9bd5459102de8365ce"
company_key: "yc-onboard-io"
company: "Onboard.io"
source_id: "yc-onboard-io-news-import-03d553c3465d"
canonical_url: "https://onboard.io/blog/dependent-due-dates"
published_at: "2025-10-17T00:00:00+00:00"
first_seen_at: "2026-07-22T07:14:30.527567+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:3aa54677fdde95a1d0a536a3d9e6fc0ecd682cc4e1fa65c3d565b00b93ec2a56"
---

# Announcing Dependent Due Dates: Keep every task in lockstep

**TL;DR** Onboard now supports **Dependent Due Dates** . You can auto‑set a task’s due date based on another task’s due date—either the **previous task** or **any specific task** in the map. When a task’s due date changes, all dependent tasks reschedule automatically. Circular dependencies are blocked, and we send a **single digest email** to assignees when dates shift so nobody gets spammed.


## **Why this matters**


Launching customers on time depends on momentum. The second a key date moves, a dozen follow‑on tasks can go stale. Dependent Due Dates keeps every step in sequence so your team isn’t chasing spreadsheets or re‑calculating timelines. Result: fewer overdue tasks, faster time‑to‑value, and cleaner handoffs.


## **What you can do now**


-


**X day(s) after previous task due date):** chain a sequence so each step lands a set number of days after the prior task’s **due date** .


-


**X day(s) after specific task due date):** anchor multiple tasks to a milestone (e.g., “Kickoff Call,” “Contract Countersigned,” “Data Import Complete”).


-


**Mix and match:** use standard rules (e.g., 4 days after map creation) for some tasks and dependency rules for others.


-


**Edit safely:** Onboard prevents circular dependency creation.


-


**Notify smartly:** When upstream dates change, we send **one consolidated email** to each assignee summarizing their updated dates.


## **How it works (2 quick patterns)**


### **1) Waterfall from project start**


1.


Set Task 1 to **1 day after project creation** .


2.


Set Task 2 to **3 days after previous task due date** .


3.


Set Task 3 to **2 days after previous task due date** .


4.


Repeat as needed.


If the kickoff slips by two days, downstream tasks move automatically—no manual edits.


### **2) Milestone‑anchored plan**


1.


Choose a milestone task (e.g., **Contract Countersigned** ).


2.


For dependent tasks, select **X day(s) after specific task due date** and choose that milestone.


3.


Add offsets per task (e.g., Security Review = +2 days; SSO Setup = +5 days).


Change the milestone date once, and every linked task re‑flows.


## **Real‑world examples**


-


**SaaS implementation:**


-


*Kickoff* = 1 day after project creation


-


*Access Provisioning* = 1 day after **Kickoff due date**


-


*Data Import* = 3 days after **Kickoff due date**


-


*QA / UAT* = 2 days after **Data Import due date**


-


*Go‑Live* = 1 day after **QA / UAT due date**


-


**Services onboarding:**


-


*Contract Signed* (specific task)


-


*Billing Setup* = 0 days after **Contract Signed due date**


-


*Brand Intake* = 2 days after **Contract Signed due date**


-


*Creative Review* = 3 days after **Brand Intake due date**


## **What’s built‑in to prevent messes**


-


**Circular dependency tracking:** Onboard blocks invalid loops (task A can’t depend on task B if B already depends on A).


-


**Bulk date refresh:** Any upstream change triggers a recalculation of all affected tasks.


-


**Digest notifications:** One email per person covering all of their updated due dates for that change event.


## **Setup: enable in under a minute**


1.


Open a Global Task or task in a map.


2.


Toggle **Default due date → On** .


3.


Pick **Day(s) after previous task due date** or **Day(s) after specific task due date** from the dropdown.


4.


Enter the day offset.


5.


(Optional) Anchor to the specific task if chosen.


6.


Save. That’s it—new projects will inherit the rule, and existing projects update the moment the anchor date changes.


## **Who benefits**


-


**Customer Success & Onboarding:** Less manual timeline maintenance, fewer overdue tasks, clearer expectations for customers.


-


**RevOps:** Standardized timelines that adapt to reality without creating ticket noise.


-


**Leads & Managers:** Reliable forecasts for *Days to Launch* and workload planning.


## **FAQs**


**What happens if I edit a due date on a dependent task manually?** You can still make a one‑off change. If the upstream date changes later, Onboard will re‑apply the dependency rule and update the date.


**Can I create multi‑level chains (A → B → C → D)?** Yes. Each task can depend on the previous task’s due date, which rolls through the chain.


**Do business days and holidays apply?** Dependent Due Dates respect the same business day and holiday settings you already use in your workspace.


**Will this work alongside existing rules like X days after map creation?** Absolutely. You can use dependency‑based rules on some tasks and other due‑date rules on the rest.


**Can customers see updates?** If your customer has shared visibility, they’ll see dates update in real time and in their notifications (no extra logins required).
