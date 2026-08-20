---
schema_version: "1.0.0"
document_id: "5ad445296adb95ddcb4488319d2e8a14ca717d4cef900d77cfefa0226fa95943"
company_key: "yc-loops"
company: "Loops"
source_id: "yc-loops-news-import-d3d77458967f"
canonical_url: "https://loops.so/changelog/goals-are-live"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-25T13:13:35.252230+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:96f035a10eb305d4a618aaa78ac91a9c2e9575efe09f138632281ab467f92698"
---

# Goals are live in Loops

[Goals](https://loops.so/docs/goals/overview) are live in Loops. Goals give every campaign a conversion target inside Loops.


For example, if you email free users about a paid plan, set a goal for contacts whose plan changes to paid within 14 days.


Create a goal in Loops, attach it to a draft campaign, send the campaign, and see whether contacts reached the conversion state you care about. The read-only Workflow API is now available for listing workflows and reading workflow node details.


## **Goals**


Move beyond opens and clicks. Use Goals to answer the question behind every campaign: did it work?


Create a goal in Loops, define who should be measured, choose the conversion state that counts as success, and set the attribution window: how long Loops should count conversions after the campaign sends.


Once the campaign sends, Loops tracks impressions, enrollments, and conversions, so you can measure activation, upgrades, demo bookings, purchases, retention, or any contact state that tells you the send worked.


[Read the Goals docs](https://loops.so/docs/goals/overview)


## **Workflow builder refresh**


Workflows now use the same three-panel structure as the rest of Loops: navigation on the left, the canvas in the center, and editing details on the right.


That brings workflows in line with the editor pages and makes the app feel more consistent when you move between building emails and building automations.


## **Themes and components API**


Themes and components are part of the API now too.


Components can now be listed and read through the API. Themes can be listed, read, created, and updated, so agents and internal tools can create email content that still uses the design system your team already has in Loops.


Read the[component](https://loops.so/docs/api-reference/list-components) and[theme](https://loops.so/docs/api-reference/list-themes) docs


## **Email message API updates**


The email message API now exposes more editor settings from code.


-


Fallback values for dynamic content


-


Styled or plain email format


-


Language code for translated variants


-


[Guardian checks](https://loops.so/docs/creating-emails/guardian) , so API-built emails can return errors and warnings before they ship


-


CC and BCC fields when enabled for your team


[Read the email message API docs](https://loops.so/docs/api-reference/update-email-message)


## **Campaign and transactional API updates**


Organize your dashboard from code: create[campaign groups](https://loops.so/docs/api-reference/create-campaign-group) and[transactional groups](https://loops.so/docs/api-reference/create-transactional-group) , then move campaigns and transactional emails into the right group.


Campaigns can also be[created](https://loops.so/docs/api-reference/create-campaign) with a mailing list, saved audience segment, inline audience filter, group, and schedule, then[updated](https://loops.so/docs/api-reference/update-campaign) while they are still drafts.


Transactional emails can now be renamed, moved into groups, previewed, drafted, and published through the API.


Email[previews](https://loops.so/docs/api-reference/preview-email-message) are available through the API for campaign, workflow, and transactional messages.


## **Everything else**


There are smaller product updates too: dates and times now use your local format across more of the app, campaign metrics surface bounce-rate warnings, and sections in the right-side style panel can now be collapsed.


Section editing also has better drag/drop, handles, border width, and border color support.


MJML variable editing moved into the side panel, and the CLI updates from the last product update are live too.
