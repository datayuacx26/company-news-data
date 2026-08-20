---
schema_version: "1.0.0"
document_id: "5f0eac5291145a7f703fb58ec3d0f3c5cc8f7238814da4dbae2c728f1832b2c3"
company_key: "yc-datasaur"
company: "Datasaur"
source_id: "yc-datasaur-news-import-f7082d4871d2"
canonical_url: "https://datasaur.ai/blog-posts/july-2026-feature-updates-smarter-search-smoother-project-management-and-better-workflow-automation"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T09:18:46.233190+00:00"
fetched_at: "2026-08-05T09:18:47.484501+00:00"
content_hash: "sha256:4e270c441e7a94fe99abb401dd9e5728e3ea4ec032a981c0498f6fda48c8d0dd"
---

# July 2026 Feature Updates: Smarter Search, Smoother Project Management, and Better Workflow Automation

## Data Studio


### Advanced Search in Span Labeling: Find Comments and Label Attributes with More Precision


In Span Labeling projects, you can now search comments across documents in a project and use label attribute as a sub-condition under the label search target. This makes it easier not only to surface discussions during review, but also to narrow results based on label attribute values when you're working with more detailed labeling schemas.


[Learn more](https://docs.datasaur.ai/advanced/extensions/search#advanced-search)


### Custom Export Results: Choose Assignees and Fields for More Targeted Exports


You now have more control over exported annotation data by selecting specific assignees and choosing exactly which fields to include. This makes it easier to prepare cleaner exports for downstream review, reporting, or handoff.


[Learn more](https://docs.datasaur.ai/data-studio-projects/export-project#advanced-settings)


### In-Project File Management: Add Documents Faster and Remove Files More Flexibly


You can now add documents directly from inside a project, making it easier to manage files without breaking your flow. And when you need to clean things up, documents can be deleted either from the Projects page or from inside the project itself, giving teams more flexibility in how they manage active datasets.


[Learn more](https://docs.datasaur.ai/workspace-management/project-management/manage-documents-in-an-ongoing-project) ‍


### ‍
Real-time Assisted Labeling in Span Projects: Run Predictions Across the Entire Document on Demand


Real-time assisted labeling now lets you trigger predictions for the entire document immediately instead of waiting for batched auto-prediction.


### Create Project Action Flexibility: Keep Files in Place When Running Automations


Action: Create Project now includes a "Keep file in place" option, so you can run project-creation workflows without needing an output folder or extra file permission. This makes automation easier to adopt in more restrictive environments.


### Document Completion Webhooks: Trigger Downstream Work the Moment Labeling or Review Finishes


Webhook notifications can now fire when a document is marked complete by either a labeler or a reviewer, making it easier to connect Datasaur with downstream automation and status tracking workflows.


[Learn more](https://docs.datasaur.ai/integrations/webhook-notifications/events#document-completed)


### Smarter Retry Handling in ML-assisted Labeling: Reprocess Only the Failed Items


ML-assisted Labeling now retries only the failed items in a batch job instead of reprocessing everything. This makes large jobs more efficient and reduces wasted time when only a small portion needs another pass.


[Learn more](https://docs.datasaur.ai/compatibility-and-updates/release-notes/version-6/6.168.0)


### Enhanced Row-Based Labeling Agents: Support Multi-Question Flows with Conditional Logic


Row-Based Labeling Agents are now more flexible, with support for a single agent to handle multiple questions and follow conditional dependencies within the same setup. This reduces the need to deploy and manage multiple separate agents, making complex labeling workflows easier to configure and maintain.


These updates may be focused, but they make everyday workflows more flexible, from finding the right annotations faster to managing project files and automation with less overhead. More improvements are on the way next month.
