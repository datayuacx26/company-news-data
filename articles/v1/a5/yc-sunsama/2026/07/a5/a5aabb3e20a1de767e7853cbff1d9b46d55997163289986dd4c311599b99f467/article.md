---
schema_version: "1.0.0"
document_id: "a5aabb3e20a1de767e7853cbff1d9b46d55997163289986dd4c311599b99f467"
company_key: "yc-sunsama"
company: "Sunsama"
source_id: "yc-sunsama-news-import-3d5f9384ecf1"
canonical_url: "https://roadmap.sunsama.com/changelog/weekly-product-changelog-june-19-2026"
published_at: null
first_seen_at: "2026-07-24T02:45:30.397865+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:df48e5ff525ca3e4e4bdc47808bd48819e810e47bcee5cc1c47ec0cb240e4506"
---

# Weekly Product Changelog: June 19, 2026

[Sunsama](https://roadmap.sunsama.com/)


[Home](https://roadmap.sunsama.com/)[Feedback](https://roadmap.sunsama.com/improvements)


Feedback


[Changelog](https://roadmap.sunsama.com/changelog)


[← Back to changelog](https://roadmap.sunsama.com/changelog)[Powered by Canny](https://canny.io/powered-by-canny?utm_source=changelog_subdomain&utm_medium=powered&utm_campaign=sunsama&company=Sunsama)


Added


:


- Notes & comments indicator


— Task cards now show a small icon when they have notes or comments attached, so you can see which tasks have additional context without opening each one. Visible on the kanban board, backlog, and mobile.


Improved


:


- AI channel recommendations for calendar events: Imported calendar events now get automatic channel assignments from AI channel recommendations, matching the behavior you'd expect from other integrations.
- Sunsama MCP: edit and append task notes: The Sunsama MCP server now supports editing and appending to task notes via edit_task_notes and append_task_notes tools, and task creation tools now accept Markdown for notes.


Fixed


:


- Sunny stripping legitimate URLs: Fixed an issue where Sunny was incorrectly removing valid links from AI responses. URLs in Sunny's replies now also render as clickable links.
- Duplicate tasks in Todoist filter views: Subtasks no longer appear twice in the Todoist Today and Next 7 Days filter views.
- Teams meeting-chat backlinks: The "Go to original Teams message" link on tasks created from Teams meeting chats now navigates correctly instead of briefly flashing and redirecting.
- Weekly review date picker for non-Monday week start: The date picker in Weekly Review and Weekly Planning now shows the correct week for users whose week starts on Sunday or Saturday.
- Outlook subtask sync errors: Fixed a data issue where corrupted Outlook subtask identifiers could cause the task list to fail to load.
