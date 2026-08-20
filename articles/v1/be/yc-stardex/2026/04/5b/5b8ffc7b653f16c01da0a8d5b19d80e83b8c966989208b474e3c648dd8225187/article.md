---
schema_version: "1.0.0"
document_id: "5b8ffc7b653f16c01da0a8d5b19d80e83b8c966989208b474e3c648dd8225187"
company_key: "yc-stardex"
company: "Stardex"
source_id: "yc-stardex-news-import-5aa4df39d83f"
canonical_url: "https://www.stardex.com/blog/product-update-17"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-24T02:17:50.986437+00:00"
fetched_at: "2026-07-28T22:16:01.195245+00:00"
content_hash: "sha256:65208c3009b96f03509498f5ae025589ac0fdff8866b821780bcb2c5961606c6"
---

# Product Update #17

Last month we expanded our team with Nate who joined as a forward deployed engineer to work closely with our customers to add more automations. We[rebranded](https://links.heywharf.com/6441e4ef9b021d1b1ebdefc2/l/tHwsdXn6B3DbnSTW8?rn=&re=i02bj5CbpFWbnB0Mw4SZ5FmZsxWeklWZzlmblRmI&sc=false) , and we've loved the reception so far. We've also been running workshops on AI agents for recruiting workflows and planning more of those (next one is on Claude Cowork on Thursday April 9, doesn’t have to be a Stardex customer).


Here's what we shipped in March:


## Claude & ChatGPT Improvements


We're seeing a lot of firms start using Claude and Cowork in their day-to-day, and we want to be your Agentic ATS. So we invested heavily this month in improving our API and MCP server to work better with AI agents. If you are actively building workflows on top of your Stardex data, I'd love to hear and see how we can help out in the process.


## Placements & Fee Tracking


You can now track placements, retainer installments, commission splits, and fees end-to-end inside Stardex. If you've been managing these in spreadsheets alongside your ATS, this should replace that.


## AI Auto-Update in the background


When you log an activity, Stardex now automatically extracts tags and compensation data from it. No more need to click the AI update button, it'll just happen. You can configure which fields get auto-updated and review changes in a detail modal.


## Mobile responsiveness


Stardex now works as an app on your phone. We added a mobile-optimized people list, candidate detail view, and command menu. Add the Stardex website to your home screen if you're checking candidates between meetings.


## Send to Client


We overhauled the candidate submission flow when sending a single candidate for review. When you're ready to present candidates to a client, the new popup makes it faster to put together a clean submission with resumes.


##


## Custom Field Mapping for Imports


The bulk importer now supports custom field mapping, stage mapping, and tag behavior config. If you're migrating data or importing a large list, you have full control over how columns map to Stardex fields.


##
Permissions improvements


Want certain users or external sourcers to not be able to see previous candidate notes and send campaigns? Stardex permissions now supports that!


## Stage reached filter


You can now filter candidates by what stages they’ve reached in the past, not just what pipeline stage they’re in currently! Long overdue!


**Lists v2:** Lists now support templates, cloning, and linking records directly. If your team has a standard format for lists, save it as a template and reuse it. You can also now archive lists to keep your active lists view clean.


## Quality of Life:


-


Collapsible search bar and new filters: past company, list membership


-


Duplicate detection when viewing a person record


-


We now extract hyperlinks from resumes (portfolio links, GitHub profiles) that were previously lost in PDF conversion


-


Job vector search in Cmd-K so you can find jobs by description, not just title


-


Custom job names so you can use your own naming conventions (e.g. searches, roles, projects, whatever you want to call it)


-


Zapier triggers for job created, archived, and stage changes


-


Activity note drafts auto-save so you don't lose work


-


Task reminder emails now show linked people, jobs, and companies


-


Custom assessment creator for building your own candidate evaluation forms
