---
schema_version: "1.0.0"
document_id: "50dbeaa525cb4050a645ac258411b1c1b644adafda45c88c27321005dbc58886"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-rss-220861d4746e"
canonical_url: "https://embedded.gusto.com/blog/time-off-contractors/"
published_at: "2026-08-05T16:29:09+00:00"
first_seen_at: "2026-08-05T20:56:11.076330+00:00"
fetched_at: "2026-08-20T01:10:45.054767+00:00"
content_hash: "sha256:7a223b5237907e5ac19cf44b8fa48baa83eb843b23af44f7b3ea18f62ad3f31a"
---

# Midyear Product Update: Time Off Management, Contractor Lifecycle, and More

We’ve been shipping steadily this year, and we wanted to pause and share what’s new from the Gusto Embedded team. This update covers releases that expand what our partners can offer their payroll customers: a more robust time-off experience and an easier way to manage contractors, among many other improvements.


## **Time Off Requests and Approvals**


We’ve also added the full


[time-off request and approval](https://docs.gusto.com/embedded-payroll/docs/time-off-requests) lifecycle to Gusto Embedded. Employees can submit, track, and cancel time off requests via REST APIs. Admins get a dedicated Flow,


` time_off_request_management`


, to review pending requests, approve or decline them, and record time off on behalf of employees. Approved time off is automatically applied to payroll.


**New admin Flow:**


The


` time_off_request_management`


Flow surfaces a queue of pending requests, shows employee balance details, and lets admins approve or decline with notes. Admins can also edit hours or dates at the time of approval, and add time-off records directly. The


` approver_uuid`


can be specified when generating the Flow.


These are all additive, with no breaking changes. Partners building their own employee-facing UI for time off management UI can use the APIs directly. If you don’t have an employee-facing UI for time off management, admins can use the new pre-built Flow to manage employee time-off requests on their behalf.


## **Time Off Policy Management**


The


time_off_management


Flow gives partners a pre-built UI that lets admins create and manage time off policies directly within your platform, with no custom build required.


**What’s included in the Flow:**


- Create and update PTO (unlimited or accrual-based), sick, and holiday policies


- Manage employee enrollment and balances: add or remove employees, move employees between PTO policies with balance carryover, and edit balances manually


- Holiday policies start with 11 standard federal holidays, with per-holiday toggles


- Advanced policy settings based on accrual type, including waiting periods, maximum balances, carryover limits, and termination payout


**API updates alongside the Flow:**


- New


` policy_reset_date`


field in the time off policy object


- The existing


` complete`


field is now editable


For existing partners, implementing this Flow follows the same pattern you’re already familiar with, and


[documentation and testing guidance are available in the developer docs](https://docs.gusto.com/embedded-payroll/docs/manage-time-off-policies) .


## **Contractor Management Flow**


The new


[Contractor Management Flow](https://docs.gusto.com/embedded-payroll/docs/manage-contractors) gives partners a centralized interface for managing the full 1099 contractor lifecycle, including onboarding, active status, dismissals, rehires, and scheduled future actions. The Flow mirrors the structure of our existing Employee Management Flow.


Previously, partners had to stitch together multiple lower-level API calls to manage contractors. This new release consolidates this feature set into one Flow and a set of purpose-built endpoints.


**What the Flow supports:**


- View contractors by status: Active, Onboarding, or Dismissed


- Add new contractors (extends existing


` add_contractors`


Flow)


- Immediate or scheduled dismissals


- Cancel pending dismissals


- Contractor rehires


**A few things to know:**


The existing


` is_active`


parameter on the contractor update endpoint remains available for immediate activation or deactivation. The new termination endpoints are for scheduled operations — they go through the full state machine, create audit records, and support a 2-day cancellation window. When a scheduled dismissal is pending,


` is_active`


will return an error to prevent conflicting states.


## **Benefit Effective Dating**


You can now schedule effective-dated benefit deductions and contributions directly on external employee benefits, enabling accurate, auditable benefit changes over time and reducing manual work, payroll-time fixes, and partner-side scheduling logic. Multiple employee benefit records, each with its own window, can exist for the same company benefit, creating a log of scheduled changes.


This supports scenarios like new hires or terminations mid-cycle, leaves of absence with zero-dollar deduction periods, catch-up or corrective contributions, temporary increases or decreases to address over- or under-withholding, and switching benefit providers with changes scheduled to take effect in the future.


[Learn more about benefit effective dating in our docs.](https://docs.gusto.com/embedded-payroll/changelog/added-ability-to-specify-effective-dated-windows-for-employee-and-company-benefits)


## **Payroll Digests for Multi-Company Payroll Monitoring**


The new


[Payroll Digests API](https://docs.gusto.com/embedded-payroll/changelog/added-payroll-digests-api-for-multi-company-payroll-monitoring) lets partners retrieve payroll state summaries across many companies in a single asynchronous request, returning statuses, blockers, pay periods, and totals for up to 25 companies per batch. Partners building accountant dashboards, bookkeeper tools, or ops monitoring views no longer need to fan out per-company calls to assemble a cross-portfolio view of upcoming payrolls.


**Key functionality:**


- Submit up to 25 company UUIDs per batch and receive consolidated payroll state for each one


- Asynchronous request pattern: POST to submit, then GET to poll for results, mirroring the People Batch API


- Returns payroll statuses, blockers, pay period details, and payroll totals per company, over a fixed reporting window of roughly the last 7 days through the next 30 days


- Idempotent submissions via an` idempotency_key` field, so safe retries won’t create duplicate batches


- Partial success handling: companies that can’t be processed appear in exclusions with a reason, while successful companies appear in results


- Uses a system-level access token rather than per-company tokens, so a single call covers the whole portfolio


## **Batch Payroll Cancellation API**


The new


[Batch Payroll Cancellation API](https://docs.gusto.com/embedded-payroll/docs/bulk-payroll-cancellations) lets partners cancel multiple payrolls, across one or more client companies, in a single call. The most common use case is cancelling payrolls that couldn’t be funded, such as when a bank debit was rejected.


Like our other batch APIs, it’s asynchronous: you submit a batch, then poll for results once they’re ready. The pattern mirrors the People Batch API, so partners already familiar with that API can integrate quickly.


**Key functionality:**


- Cancel up to 100 payrolls across one or more companies in a single batch


- Asynchronous processing: submission and fulfillment are decoupled, so requests don’t time out regardless of how much work is processed


- Idempotent submissions: duplicate requests with the same` idempotency_key` return the original batch instead of re-running the cancellation


- Success and failure handling: payrolls that are cancelled successfully appear in results, while payrolls or companies that couldn’t be processed, such as an unmapped company, an unknown UUID, a duplicate UUID, or a payroll that isn’t in a cancellable state, appear in exclusions with a category and message, so one bad UUID never fails the whole batch


- System-level authentication: uses the same partner application access token already used for provisioning and bulk onboarding status


- New OAuth scopes:` payroll_batches:read, payroll_batches:write` . These scopes are gated — contact your Gusto Embedded representative to have them enabled on your application before integrating.


These releases reflect a consistent theme: giving partners more pre-built functionality to reduce engineering overhead, while keeping the API-first flexibility for teams who want to build their own experiences. Time off policy management, time off requests and approvals, and contractor lifecycle management are each additive: you can adopt them independently, in any order, as they fit your roadmap.


If you’re an existing Gusto Embedded partner and want to get started with any of these features, check the developer docs or reach out to your partner manager. If you’re interested in learning more about Gusto Embedded,


[visit our website](http://embedded.gusto.com/) .


The post[Midyear Product Update: Time Off Management, Contractor Lifecycle, and More](https://embedded.gusto.com/blog/time-off-contractors/) appeared first on[Gusto Embedded Blog](https://embedded.gusto.com/blog) .
