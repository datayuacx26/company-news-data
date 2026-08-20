---
schema_version: "1.0.0"
document_id: "6875cd6667970816770757bb41dd007daa6fd86c21a80b8f5bff7af5ba881269"
company_key: "yc-tableflow"
company: "TableFlow"
source_id: "yc-tableflow-news-import-0d755cb9224b"
canonical_url: "https://tableflow.com/blog/guidance"
published_at: "2025-10-08T00:00:00+00:00"
first_seen_at: "2026-07-22T15:32:32.527484+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:1fd0619290193c29ce2e686cc1ef0fc3f09979ebd92aae3e9bbd6c6c95a8002b"
---

# Guidance: Your AI Data Editor

We're excited to share a new feature for TableFlow: Guidance. This makes data extraction more flexible by allowing you to provide plain-English instructions to refine, format, and transform data. It's like having an AI data editor working alongside you.


In this post, we'll explain what Guidance is, how it improves on templates, and how it can enhance your data workflows. You'll learn to adjust data, fix errors, and apply complex logic—all without code.


## What is Guidance?


Guidance allows you to give natural language commands to our models during data extraction. In TableFlow, you use[templates](https://tableflow.com/blog/tableflow-templates) to define the data you want to extract from your documents. Guidance expands on this, allowing you to handle unexpected formats or one-off changes.


You can map fields, format data, perform calculations, and merge rows using simple instructions. For example: "Capitalize all product names" or "Add a 10% markup to prices." It's like having an assistant to help clean your data.


### More Flexibility


Real-world data often has exceptions and edge cases. Extraction Guidance enables you to handle unique or one-off cases without modifying your template.


For example, if a supplier file lacks a brand name column, simply instruct the AI: "Set the brand to 'Acme' for all rows." The model applies the change instantly—no manual edits required.


## How to Use Guidance


When uploading a file to TableFlow, just set the` guidance` parameter with the instructions of what you want done. Here's some examples:


### 1. Advanced Field Mapping


Resolve ambiguous column headers.


Use 'Product Title' as the 'Name' field.


### 2. Dynamic Data Formatting


Ensure clean, consistent data without scripts.


Text Formatting:


Convert all product names to proper case.


Number Formatting:


Round all prices to the nearest dollar.


### 3. On-the-Fly Calculations


Apply calculations during extraction, like pricing or tax adjustments.


Add a 10% markup to all prices.


### 4. Merging and Combining Rows


Consolidate duplicate or related entries easily.


Merging Duplicates:


Sum quantities for duplicate rows.


Combining Info:


Combine rows with the same SKU, concatenating descriptions.


## Refine Data with Feedback


Guidance can be set on upload, before data is extracted from a file, or it can be set while rerunning an extraction. If you spot an issue after extraction, you can use guidance to tweak the data without starting over.


Convert all prices from EUR to USD using today's exchange rate.


TableFlow reprocesses the file with your feedback, letting you refine data until it's perfect.


## Try Guidance Today


Ready to see how Guidance with TableFlow can streamline your data workflows? Book a demo and we'll show you how to handle any file format with simple instructions.


[Book a Demo](https://tableflow.com/demo)


Guidance brings more flexibility to TableFlow, making data extraction faster than ever. Use plain-English instructions to tackle the messiest of files and get the data you need out of them.


## Key Takeaways


- • Guidance allows plain-English instructions for data transformation
- • Handle exceptions and edge cases without modifying templates
- • Apply formatting, calculations, and merging operations instantly
- • Refine data before or after extraction with flexible feedback
- • No coding required—accessible to all users


In Summary: Guidance transforms TableFlow into a flexible AI data editor, allowing you to refine, format, and transform data with simple instructions—making complex data workflows accessible to everyone.


## Frequently Asked Questions


###


###


###


###


###
