---
schema_version: "1.0.0"
document_id: "8c8836b692de0b9029a26cfce14943d0f9782c509db71b8e4f043f6ec78a7b43"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-99d40c54e3ad"
canonical_url: "https://deepnote.com/changelog/2026-07-02"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:08.376400+00:00"
fetched_at: "2026-07-28T21:42:09.561502+00:00"
content_hash: "sha256:d8f5d3ddddff0fbd5429482c8a0530c24c4457df50364e0ab07fcaa8fb692faa"
---

# MCP tools & controls, Git directory sync, pivot tables, & more

## [July 2, 2026](https://deepnote.com/changelog/2026-07-02)


#


[MCP tools & controls, Git directory sync, pivot tables, & more](https://deepnote.com/changelog/2026-07-02#mcp-tools--controls-git-directory-sync-pivot-tables--more)


##


[MCP updates: better controls and more tools](https://deepnote.com/changelog/2026-07-02#mcp-updates-better-controls-and-more-tools)


###


[New MCP tools for integrations](https://deepnote.com/changelog/2026-07-02#new-mcp-tools-for-integrations)


MCP-connected agents can now manage integrations end-to-end with three new tools:` create_integration` ,` attach_integration` , and` detach_integration` . An agent can now create an integration and wire it into a project without a human step in between.


###


[Better MCP controls for workspace admins](https://deepnote.com/changelog/2026-07-02#better-mcp-controls-for-workspace-admins)


You can now see every MCP OAuth grant issued for your workspace and revoke any of them from **Settings > Security > MCP OAuth connections** .


A new workspace setting on the same page turns external MCP client access on or off for the whole workspace.


##


[New block type: pivot tables](https://deepnote.com/changelog/2026-07-02#new-block-type-pivot-tables)


You can now summarize and cross-tabulate data with a dedicated pivot table block. Just drop in rows, columns, values, and an aggregation; no code required. It's a focused first version, and we'll expand it based on how people use it.


##


[Git Directory Sync](https://deepnote.com/changelog/2026-07-02#git-directory-sync)


A new Git integration flow that syncs each notebook to its own` .deepnote` file, so a project's notebooks show up as separate files in your repo instead of being bundled into a single multi-notebook` .deepnote` file. Existing projects on the old bundled format get a built-in migration path. More improvements are on the way.


Watch this[walkthrough](https://www.loom.com/share/8392aaeab7fb49648f72e4f8c9a04604) for a quick look at how the migration and sync work.


##


[Collapsible sections in notebooks](https://deepnote.com/changelog/2026-07-02#collapsible-sections-in-notebooks)


Headings in notebooks are now collapsible, so you can fold away the parts you're not working on and move through a long notebook far more easily.


##


[Better agent context](https://deepnote.com/changelog/2026-07-02#better-agent-context)


The AI agent now has access to our documentation, so questions about setup, configuration, and product behavior come back grounded in the actual docs.


##


[Pick your AI model](https://deepnote.com/changelog/2026-07-02#pick-your-ai-model)


The AI model selector in your settings now offers more fine-grained selection of the latest publicly available models from OpenAI or Anthropic. You can also try Anthropic’s latest Sonnet 5 in Deepnote!


##


[SQL blocks in R environments](https://deepnote.com/changelog/2026-07-02#sql-blocks-in-r-environments)


SQL blocks now work in R-based environments (Deepnote iR with libs). Query any Deepnote integration from a SQL block, and results land in an R dataframe, the R equivalent of the pandas dataframe you'd get in a Python notebook. Downstream blocks (including DataFrame SQL) can consume it from there.


##


[BigQuery OAuth in the VS Code extension](https://deepnote.com/changelog/2026-07-02#bigquery-oauth-in-the-vs-code-extension)


The Deepnote VS Code extension (v1.5.0) now supports[OAuth-based BigQuery](https://deepnote.com/docs/bigquery-oauth) integrations, so each user signs in with their own Google account instead of sharing a service account. Short-lived tokens, MFA, and least-privilege access apply automatically.


Because the OAuth flow is proxied through Deepnote, the same integration config and redirect URLs work in both places: if you already use BigQuery OAuth in Deepnote Cloud, the integration carries over.


Install the latest version from the[VS Marketplace](https://marketplace.visualstudio.com/items?itemName=Deepnote.vscode-deepnote) or[Open VSX](https://open-vsx.org/extension/Deepnote/vscode-deepnote) .


Setup details in the[docs](https://deepnote.com/docs/bigquery-oauth) .
