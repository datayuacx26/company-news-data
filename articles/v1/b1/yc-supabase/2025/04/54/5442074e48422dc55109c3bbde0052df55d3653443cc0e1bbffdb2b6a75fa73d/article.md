---
schema_version: "1.0.0"
document_id: "5442074e48422dc55109c3bbde0052df55d3653443cc0e1bbffdb2b6a75fa73d"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/tabs-dashboard-updates"
published_at: "2025-04-02T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:5559200a38a34d33baf8fb5111c205e21551bb42283369f25447560a47ebe37e"
---

# Keeping Tabs on What's New in Supabase Studio

We've had a busy few months working on Studio improvements and new features—big and small—to help you build, debug, and ship faster.


Here's a quick rundown of what's new:


-


Tabs! : The Table Editor and SQL Editor now have tabs!


-


Customizable Reports : create reports to show data exactly how you want to see it


-


SQL Blocks in Custom reports : use SQL Snippets in blocks inside your custom reports


-


Inline SQL Editor : use a mini SQL Editor anywhere in the Dashboard


-


Multiple AI Assistant chats : maintain history of your Assistant chats


-


Logs improvements : more detailed inspect panel, and stacked charts


### Tabs!#


This has been a common request for a long time, and should make working with data much easier. We've added Tabs to our two most-used tools: the Table Editor and the SQL Editor.


### Tabs in the Table Editor#


In the Table Editor, you can open multiple tables at a time and easily switch between them using tabs. This makes it easier to compare data, edit schemas, or reference related tables without losing your place. Enabled under Feature Previews (see below )


Tabs work the same as in VS Code, opening in preview mode. This is useful if you're quickly browsing files and don't want every visited file to have its own tab. A new tab will only be dedicated to that file when you start editing or simply click into it. Preview mode is indicated by *italics* in the tab heading.


The New Tab page also gives you quick access to create a new table or open a recently visited one.


### Tabs in the SQL Editor#


In the SQL Editor, you can now write and run multiple scripts at a time, without having to constantly change between snippets. The SQL Editor tabs also have preview mode, so you can quickly flip through snippets without leaving a bunch of tabs to clean up after. Enabled under Feature Previews (see below )


Multiple tabs will make it easier to work across datasets, debug, or compare different queries, all without losing your place.


## Customizable reports#


Reports in the Dashboard recently got a refresh. You can now resize and reorder chart blocks, giving you full control over the layout. It's perfect for crafting reports that look exactly how you want.


## SQL blocks in custom reports#


We've also added inline SQL execution within blocks, so you can run your own queries directly and build fully customized, data-driven reports. Just create a snippet in the SQL Editor and it will be available to use here.


You can show your data as a table of results:


Or display them as a chart:


The sky is the limit for these. You could query a Foreign Data Wrapper, join multiple tables, create a View to highlight key information, and much more.


## Inline SQL Editor#


You can now run SQL from anywhere in the Dashboard via the Inline SQL Editor. You can query and modify tables, add triggers, functions, RLS policies, and anything else you can do from the main SQL Editor, anywhere in the Dashboard. Enabled under Feature Previews (see below ).


## Multiple AI Assistant chats#


The AI Assistant now lets you create and store multiple chats. Create, rename, switch to and delete chats, all without losing your place. Chats are scoped to the current project, so switching your project also switches chat history. The chat history is stored in local storage.


## Logs improvements#


We've updated the[log detail panel](https://github.com/supabase/supabase/pull/33536) to show more info in the API log detail:


You can also quickly add a property or value from the detail panel to search and filter the results:


And we've added http status to available filters to help you narrow in on specific logs while debugging:


## Enabling Feature Previews#


Tabs and the Inline SQL Editor can be enabled via Feature Preview. Click your user avatar in the bottom right and click Feature previews.


## Get started#


You can see all these improvements in the Supabase Dashboard now.


- [Get started with Supabase](https://supabase.com/dashboard/sign-in?returnTo=%2Fprojects) and try it out today
