---
schema_version: "1.0.0"
document_id: "830bb6ddf79447e1330849c900d6a500cad972d919c68f88a6f1dec501e1a9bd"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/github-copilot-extension-for-vs-code"
published_at: "2024-08-12T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:11d58c819f91f6a97d135480e805a4830647c53778ba0c38012b26c7f35ba31d"
---

# Official Supabase extension for VS Code and GitHub Copilot

Today we're launching a new[GitHub Copilot extension for VS Code](https://marketplace.visualstudio.com/items?itemName=Supabase.vscode-supabase-extension) to make your development with Supabase and VS Code even more delightful, starting with a Copilot-guided experience for[database migrations](https://supabase.com/docs/guides/deployment/database-migrations) .


The foundation for this extension was created by[Anas Araid](https://github.com/anas-araid) during a previous[Launch Week Hackathon](https://twitter.com/anas_araid/status/1736641409094988033) . Impressed with their work, we partnered with them to add a["Chat Participant"](https://code.visualstudio.com/api/extension-guides/chat) , an exciting[new feature recently launched](https://code.visualstudio.com/blogs/2024/06/24/extensions-are-all-you-need) by the GitHub and VS Code teams at Microsoft.


## Features#


The VS Code extension is quite feature rich:


### GitHub Copilot Chat Participant#


The extension provides a[Chat Participant](https://code.visualstudio.com/api/extension-guides/chat) for GitHub Copilot to help with your Supabase questions. Simply type` @supabase` in your Copilot Chat and the extension will include your database schema as context to Copilot.


### Copilot-guided database migrations#


The extension provides a guided experience to create and apply[database migrations](https://supabase.com/docs/guides/deployment/database-migrations) . Simply type` @supabase /migration <describe what you want to do>` in your Copilot Chat and the extension will generate a new SQL migration for you.


### Inspect tables & views#


Inspect your tables and views, including their columns, types, and data, directly from the editor:


### List database migrations#


See the migration history of your database:


### Inspect database functions#


Inspect your database functions and their SQL definitions:


### List Storage buckets#


List the Storage buckets in your Supabase project.


## What's Next?#


We're excited to continue adding more features that will make your development experience with Supabase even more delightful - and for this we need your help! If you have any feedback, feature requests, or bug reports, please[open an issue on GitHub](https://github.com/supabase-community/supabase-vscode-extension/issues) .


The extension requires you to have the Supabase CLI installed and have your project running locally. In a future release, we will integrate the[Supabase Management API](https://supabase.com/docs/reference/api/introduction) into the extension to make connecting to your hosted Supabase projects as seamless as possible.


## Contributing to Supabase#


The entire Supabase stack is[fully open source](https://supabase.com/open-source) , including[this extension](https://github.com/supabase-community/supabase-vscode-extension) . In fact, this extension was originally created by[Anas Araid](https://github.com/anas-araid) during a[previous Launch Week Hackathon](https://twitter.com/anas_araid/status/1736641409094988033) .


Your contributions, feedback, and engagement in the Supabase community are invaluable, and play a significant role in shaping our future. Thank you for your support!


## Resources#


- [Install the extension](https://marketplace.visualstudio.com/items?itemName=Supabase.vscode-supabase-extension)
- [Read the source code](https://github.com/supabase-community/supabase-vscode-extension)
- [Submit a Feature Request](https://github.com/supabase-community/supabase-vscode-extension/issues)
- [Get started with Supabase](https://database.new/)
