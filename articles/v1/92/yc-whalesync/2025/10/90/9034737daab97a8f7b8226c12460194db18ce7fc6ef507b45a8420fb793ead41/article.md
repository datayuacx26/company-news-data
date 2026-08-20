---
schema_version: "1.0.0"
document_id: "9034737daab97a8f7b8226c12460194db18ce7fc6ef507b45a8420fb793ead41"
company_key: "yc-whalesync"
company: "Whalesync"
source_id: "yc-whalesync-news-import-4fcd9b7a082a"
canonical_url: "https://www.whalesync.com/blog/how-to-export-shopify-inventory-as-a-csv"
published_at: "2025-10-20T00:00:00+00:00"
first_seen_at: "2026-07-22T19:45:33.835255+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:5fdc24bd471207a2447f7c53cba9f4b44e74b60152a291eab010a545c85a9659"
---

# How to Export Shopify Inventory as a CSV

Keeping an up-to-date record of your inventory is essential for managing stock, tracking changes, and planning reorders. Instead of checking each product manually, Shopify allows you to export your entire inventory as a CSV file, giving you a complete snapshot of product quantities across all your locations.


Here’s how to export your Shopify inventory in just a few clicks.


## Step 1: Go to your Inventory page


1. From your Shopify Admin, go to Products → Inventory.
2. Use filters or search to view inventory by product, vendor, or location (if you fulfil inventory from multiple lcoations)
3. Choose the location you want to export inventory from


## Step 2: Click “Export”


At the top right of the Inventory page, click Export.


A dialog box will appear asking what data you want to include in your export.


## Step 3: Choose your export range


You can export:


- Current page: only the inventory displayed on your screen.
- All products: every product and variant in your store.
- Selected products: only the ones you’ve checked.


Choose your preferred option, then click Export inventory.


## Step 4: Choose your file format


Shopify exports inventory data as a CSV file, which can be opened in Excel, Google Sheets, or any spreadsheet program.


You’ll see two file options:


- CSV for Excel, Numbers, or other spreadsheet programs, which is the most common choice.
- Plain CSV file, which is the best option if you plan to import the data into another system.


After selecting your format, click Export inventory again.


## Step 5: Download your inventory CSV


- For smaller inventories, the CSV file will download immediately.
- For larger datasets, Shopify will email you a download link once the export is ready.


You can then open your file in a spreadsheet editor to view, sort, and analyze your inventory quantities.


## What’s included in your Shopify inventory CSV


Your exported file includes the following columns:


- Handle: the unique identifier for each product.
- Title: product name.
- Option1/2/3 Name and Value: variant options (e.g., Size, Color).
- SKU: stock-keeping unit for each variant.
- Location name: the location tied to that inventory count.
- Available: number of units in stock.


If your store uses multiple locations, each product-variant-location combination appears on its own row.


## Limitations of Shopify’s inventory export


- Exports include stock levels but not incoming inventory or inventory history.
- You can’t customize the CSV columns or field order.
- For very large catalogs, the export may take a few minutes to generate.


## Tips for working with your inventory CSV


- Use exports as a backup before making bulk updates.
- Filter by location if you only need data for one warehouse or retail outlet.
- Save the CSV as UTF-8 to preserve special characters in product names or SKUs.
- Combine exports with your supplier or sales data to forecast restocks more accurately.


## FAQs


### Can I export inventory for only one location?


Yes. Use the Location filter at the top of the Inventory page to choose a specific warehouse or store before exporting.


### Will the CSV include product images or prices?


No. The inventory CSV only includes quantity-related data. For prices or descriptions, export your Products CSV instead.


### Can I schedule automatic inventory exports?


Not directly in Shopify. You’ll need to export manually or use a third-party app to automate recurring exports.


### Can I edit this CSV and re-upload it?


Yes. You can adjust stock levels in the file and re-import it using Shopify’s Import inventory feature, just be careful not to change the column headers or formatting.
