---
schema_version: "1.0.0"
document_id: "614660a691315354b58bb6cce07f526fbc7bb851f70941a3d36af11c2e3d42f5"
company_key: "yc-floweq"
company: "FlowEQ"
source_id: "yc-floweq-news-import-df453f5906cc"
canonical_url: "https://www.floweq.com/blog/export-and-import-your-flows"
published_at: null
first_seen_at: "2026-07-23T09:48:12.201585+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:02e7910a41f47a27c720ef46aab8447265061555942a2ed4817af161bad558ff"
---

# Export and Import your Flows

If you have BrightReps running in two instances, staging and production, you know how important it is to have your flows well duplicated to maintain integrity. Before, that used to be a more manual process, but now you can export flows from one instance, and reimport them in another.


Note: Some CRMs, like Zendesk for example, have global unique IDs for their ticket fields. If you had set up your BrightReps staging instance with your Zendesk staging instance, your ticket field IDs won't work from your BrightReps production instance. After import is complete, you will need to edit the flow to point any Zendesk Update Ticket Fields stepstypes towards your new Zendesk production instance field IDs.


**How to export a flow**


1. Open the flow you wish to export.
2. Click the dropdown on the **Publish Changes** button, then select **Export Flow as File**
3. Click on the download link in the next modal.
4. A text file will be downloaded to your computer - that's your flow!


‍


How to export your flows


‍ **How to import a flow**


1. From Flow Manager, click **** the **Create Flow** button at the top
2. **Drag the text file into the box at the bottom,** or click **Select File to Upload** then select the text file from there.
3. There is no step 3! Your flow will now appear in Flow Manager


How to import your flows


‍
