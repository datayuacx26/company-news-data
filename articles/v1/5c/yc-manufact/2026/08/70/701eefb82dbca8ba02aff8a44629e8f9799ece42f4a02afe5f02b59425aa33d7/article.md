---
schema_version: "1.0.0"
document_id: "701eefb82dbca8ba02aff8a44629e8f9799ece42f4a02afe5f02b59425aa33d7"
company_key: "yc-manufact"
company: "Manufact (formerly mcp-use)"
source_id: "yc-manufact-news-import-914a697bc101"
canonical_url: "https://manufact.com/blog/submit-mcp-server-to-anthropic-directory"
published_at: "2026-08-05T07:00:00+00:00"
first_seen_at: "2026-08-05T23:51:05.616510+00:00"
fetched_at: "2026-08-05T23:51:06.880178+00:00"
content_hash: "sha256:df37095cc2d5ecdc843ea063477da87f60066c2d54b52c874c1019ecff283b95"
---

# How to Submit an MCP Server to Anthropic's Connectors Directory

Anthropic's Connectors Directory is the catalog of MCP integrations available in Claude. Remote MCP server submissions now happen in the Claude.ai organization admin portal.


This guide focuses on remote MCP servers. Anthropic's[official submission documentation](https://claude.com/docs/connectors/building/submission) is the source of truth for current requirements and portal behavior.


## Before You Start


Before submitting, have the following ready:


-


**A Team or Enterprise Claude organization with directory-management access.**


-


**A remote MCP server available through a public HTTPS endpoint.**


-


**Your listing details, public documentation, privacy policy, icon, and authentication instructions.**


-


**A test account if reviewers need to authenticate and use your tools.**


By submitting, you agree to Anthropic's[Software Directory Policy](https://support.claude.com/en/articles/12131204-anthropic-software-directory-policy) and[Software Directory Terms](https://support.claude.com/en/articles/12131218-anthropic-software-directory-terms) . Submission is a review request, not a guarantee of inclusion.


## Submit Your MCP Server


Open the[Claude Connectors Directory submission portal](https://claude.ai/admin-settings/directory/submissions) while signed into the organization that will own the listing. Add the MCP endpoint, listing information, authentication details, policy answers, and any requested review access. Review the submission and send it when the portal reports that all required fields are complete.


## Common Blockers


Run[Manufact Publishing Checks](https://manufact.com/platform/publishing-checks) against your live MCP URL before submitting. The audit checks protocol behavior, tools, MCP App requirements, metadata, and client compatibility. Each failed check includes the issue to fix before resubmitting.


## Review, Status, and Updates


Automated reviews run after submission. Passing these checks publishes the connector in the **Community** state. Its Claude listing displays a banner identifying it as a community connector.


Resubmissions also run through automated reviews. Small changes to tool names, descriptions, or arguments, as well as additional tools, update automatically after passing. Only changes that substantially alter the MCP server may require additional review.


Review time varies depending on the connector's use case. If you have questions about timing or what to expect, reach out to us.


Use the[submissions portal](https://claude.ai/admin-settings/directory/submissions) to track the listing and submit updates.


For the broader distribution strategy, see[How to Distribute Your MCP Server Across ChatGPT, Claude, and Cursor](https://manufact.com/blog/distribute-mcp-server-across-directories) .


### Get your MCP server submission-ready


We'll review your server against the current connector requirements, test its connection and tool metadata, and help you prepare the listing and reviewer account before submission.


[Book a call](https://manufact.com/book-call)
