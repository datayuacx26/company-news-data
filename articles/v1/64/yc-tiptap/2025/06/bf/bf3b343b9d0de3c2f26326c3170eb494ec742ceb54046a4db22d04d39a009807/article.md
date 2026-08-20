---
schema_version: "1.0.0"
document_id: "bf3b343b9d0de3c2f26326c3170eb494ec742ceb54046a4db22d04d39a009807"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-30112aa6d3bf"
canonical_url: "https://tiptap.dev/blog/release-notes/improved-docx-import-export-in-tiptap"
published_at: "2025-06-13T00:00:00+00:00"
first_seen_at: "2026-07-22T16:45:40.643427+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:a270ae09d3e2cb526c4dbcb3d39a791d3d9c79e7dd2aae687f67343f20c00c39"
---

# Improved DOCX Import & Export in Tiptap

## **What’s New**


### **New DOCX Import & Export**


We’ve added a dedicated extension to export DOCX files, as well as a new endpoint to our services to import DOCX files.


We’ve split the extension to provide a more specialized and efficient approach to document conversion. The existing` @tiptap-pro/extension-export` package remains unchanged, ensuring backward compatibility for users who don’t need to make adjustments.


At the same time, we’ve introduced a dedicated extension for DOCX export and dedicated service for the DOCX import, allowing you to include only the functionality you need. For example, if you’re only exporting to DOCX, you no longer have to carry over unnecessary code for PDF or Markdown conversion. This keeps your implementation lean and efficient.


```text
npm install @tiptap-pro/extension-  export  -docx
```


## **Run the extension in the frontend or backend**


The DOCX export functionality is flexible. You can run it in the frontend or backend, depending on your needs.


### **When to run it in the frontend**


If you’re using **custom nodes** , you’ll need to handle mapping them to standard DOCX elements. Running the conversion in the frontend ensures full control over this process.


## ‍ **When to Run it in the Backend**


**‍** If you **don’t need custom nodes or overriding DOCX styles** , and prefer a lighter frontend bundle, you can offload the conversion entirely to the backend. For users who don’t want to manage backend infrastructure or server code, our hosted REST API provides an easy solution for DOCX conversion without custom nodes or style overrides. You can switch to frontend or your own backend at any time if your needs change.


## **What Got Better**


### Smarter formatting conversion


Nested lists or tables? Mixed font styles inside a paragraph? We now handle those way more accurately, both when importing and exporting.


### Custom node support


You can now map your custom nodes (like callouts, embeds, or warnings) to standard DOCX elements like paragraphs or headings – no more losing structure on export.


### Flexible image handling


Images from imported DOCX files can now be uploaded to your own storage, we hand over control to you.


### Styling you can control


Define exactly how exported DOCX files should look: fonts, colors, spacing, it can all reflect your product’s UI instead of Word’s defaults.


## **Get started**


To start using the improved DOCX import and export, check out our updated[developer documentation](https://tiptap.dev/docs/conversion/import-export/docx) for setup instructions and examples.


## **We’d Love Your Feedback!**


Try out the new improvements and let us know how they work for you. If you have any feedback or suggestions, reach out athumans@tiptap.dev or join our[Discord Community](https://tiptap.dev/discord) .


Happy coding!
