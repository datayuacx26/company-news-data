---
schema_version: "1.0.0"
document_id: "5f4fbc71023a913974d16af9aca7fc5ef6e84ed8826cdb5046c57b8a5a24c7b3"
company_key: "yc-inbuild"
company: "inBuild"
source_id: "yc-inbuild-news-import-7baf19c8db08"
canonical_url: "https://www.inbuild.ai/posts/how-to-import-commitment-sovs-into-procore"
published_at: "2026-03-11T00:00:00+00:00"
first_seen_at: "2026-07-25T09:24:38.269810+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:16b1502d4281ee5c9c276d0f4f74b0049796dbcbadb11e0bc615f2f834d058a2"
---

# How to Import Commitment SOVs into Procore

If you've ever had to manually build a Schedule of Values inside Procore, you already know how tedious it can get. Copying line items from a subcontractor quote or estimate, reformatting everything, and then hoping nothing gets rejected on import. It's a lot! The good news: Procore supports CSV imports for SOV line items, and a tool called SOV2Procore.com can handle most of the heavy lifting for you.


Here's a complete breakdown of how the process works and how to make it faster.


## How Procore's SOV CSV Import Works


### First, Check Your Permissions


Before you can import SOV line items into the Commitments tool, make sure you have one of the following:


- **Admin permissions** on the Commitments tool, OR
- **Read Only or Standard permissions** with granular permissions enabled for *Update Purchase Order Contract* and *Update Work Order Contract*
- [Here's Procore's docs on permissions](https://v2.support.procore.com/faq-what-are-the-requirements-for-importing-sov-line-items-from-csv)


### Commitment Status Matters


Whether you can import also depends on your SOV editing settings:


- **Always Editable SOV is OFF (default):** The commitment must be in **Draft** status to import.
- **Always Editable SOV is ON:** You can import in any status, but you won't be able to replace line items that have already been invoiced.


### Always Use Procore's CSV Template


Don't try to build your import file from scratch. Procore requires its own template, which you can download directly from the commitment or their[support doc here](https://v2.support.procore.com/product-manuals/commitments-project/tutorials/import-commitment-sov-line-items-from-a-csv-file) :


**Commitments → Open Commitment → Schedule of Values → Edit → Import SOV from CSV**


You'll have the option to download either a blank template or one pre-filled with existing line items.


### CSV Formatting Rules


Procore's import is strict about formatting. A few rules to follow:


- Use a **comma** or **semicolon** as your delimiter
- **Do not add or reorder columns** : column names must match Procore exactly
- Each row represents one SOV line item


**Required fields for each line item include:**


- Cost Code Name (must exactly match the project cost code)
- Cost Code Number (must exactly match the project cost code)
- Line Item Type / Cost Type
- Amount, OR Quantity + Unit Price


One common import failure to watch for: the **Amount column must always contain a value** . A blank amount field will cause the row to fail, but a value of` 0` is perfectly fine. Completely empty rows are simply ignored.


### A Note on WBS and Budget Codes


CSV imports don't support budget codes directly. Instead, you'll map line items to Procore's Work Breakdown Structure (WBS) by providing:


- Cost Code
- Cost Type
- Sub Job (optional)


If your project uses Sub Jobs, the feature must be enabled and the Sub Job must already exist in Procore before you can import against it.


## Where[SOV2Procore.com](https://sov2procore.com/) Comes In


Even with a template in hand, the real bottleneck is transforming a PDF quote or estimate into a properly formatted CSV. That's exactly what[SOV2Procore](https://sov2procore.com/) is designed to solve.


### Step 1: Upload Your Estimate


Start by uploading your source document: a PDF quote, estimate, or subcontract proposal.


### Step 2: Configure Your Procore Settings


Tell the tool how your Procore project is set up:


- **Accounting method:** Amount-based or unit/quantity-based
- **Sub Job selection**
- **Tax code settings**


### Step 3: Automatic Line Item Extraction


[SOV2Procore](https://sov2procore.com/) extracts the line items from your document and structures them into a Procore-ready CSV, complete with:


- Description
- Quantity and Units
- Unit Price
- Amount


No manual copy-pasting required.


### Step 4: Add Your WBS Cost Codes


The one step that still requires your input: adding the project-specific WBS data. Before importing, you'll need to fill in:


- Cost Code Number
- Cost Code Name
- Cost Type


These need to match your Procore project's WBS configuration, so this step keeps you in control of the mapping.


### Step 5: Import into Procore


Once your file is ready:


1. Open the commitment in Procore
2. Go to **Schedule of Values**
3. Click **Edit**
4. Select **Import SOV from CSV**
5. Upload your file


Choose whether to **add additional line items** or **replace existing ones** , and Procore will confirm the import with a success or error message.


## The Bottom Line


Manually rebuilding an estimate as a Procore SOV line by line is a real time drain, especially when you're managing multiple commitments at once. Using Procore's CSV import alongside[SOV2Procore](https://sov2procore.com/) means you can:


- Convert PDF estimates into Procore SOVs in minutes
- Skip manual line item entry entirely
- Keep your formatting Procore-compliant from the start
- Focus your attention on WBS mapping, not data re-entry


If you're regularly creating commitment SOVs in Procore, this workflow is worth adding to your process.
