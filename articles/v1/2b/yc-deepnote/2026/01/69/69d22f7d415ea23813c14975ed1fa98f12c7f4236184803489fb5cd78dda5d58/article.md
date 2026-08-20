---
schema_version: "1.0.0"
document_id: "69d22f7d415ea23813c14975ed1fa98f12c7f4236184803489fb5cd78dda5d58"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-99d40c54e3ad"
canonical_url: "https://deepnote.com/changelog/2026-01-09"
published_at: "2026-01-09T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:08.376400+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:f045111bef3fdd1728fe6830b3006ba4864ade27321f8f09b6b5193539c96ca9"
---

# Python & SQL language servers in VS Code, & bring your own environment

## [January 9, 2026](https://deepnote.com/changelog/2026-01-09)


###


[Python & SQL language servers in VS Code, & bring your own environment](https://deepnote.com/changelog/2026-01-09#python--sql-language-servers-in-vs-code--bring-your-own-environment)


###


[Python & SQL language servers in the VS Code extension](https://deepnote.com/changelog/2026-01-09#python--sql-language-servers-in-the-vs-code-extension)


The Deepnote VS Code extension now ships with Python and SQL language servers, which means your editor can actually understand your code and your schema.


What you get out of the box:


- Diagnostics
- Type hints and method signature info
- Autocomplete for functions and variables
- Autocomplete for table and column names, powered by your real database schema


This is included in[the newly released extension version 1.3.0](https://github.com/deepnote/vscode-deepnote/releases/tag/v1.3.0) , along with the other improvements shipped before the holidays.


###


[Bring your own virtual environment to the VS Code Extension](https://deepnote.com/changelog/2026-01-09#bring-your-own-virtual-environment-to-the-vs-code-extension)


You can now use the Deepnote VS Code extension with both **managed** and **existing (external)** Python environments. The environment list now clearly shows the type, and the extension can detect and reuse virtual environments you already have (poetry, uv, conda, native venv) instead of forcing you to create a new one. This comes in handy if, for instance, you have a **custom or opinionated way of installing dependencies** . In case the Deepnote toolkit is missing, the extension will guide you through installing it directly into the selected environment with clear progress and logs. We also improved safety and reliability around environment migration, cleanup, and toolkit installation to avoid conflicts or accidental deletion.
