---
schema_version: "1.0.0"
document_id: "b2e64046bbed672ebd5be94d0a461ef9da224167f88aa0f59a62075da722d7d2"
company_key: "yc-inbuild"
company: "inBuild"
source_id: "yc-inbuild-news-import-7baf19c8db08"
canonical_url: "https://www.inbuild.ai/posts/retainage-separately-in-quickbooks"
published_at: "2026-01-23T00:00:00+00:00"
first_seen_at: "2026-07-25T09:24:38.269810+00:00"
fetched_at: "2026-07-28T22:23:13.149633+00:00"
content_hash: "sha256:f6e7d9e919514df0467223b12b22f96e6a43ae649a7de3ad43e1b939840ec4f9"
---

# Construction Companies Need to Track Retainage Separately in QuickBooks

Retainage is unavoidable in construction. What *is* avoidable is the accounting mess it creates when it’s mixed into job cost lines or handled manually. If you want clean AR/AP aging, accurate balance sheets, and an easy way to answer “how much retainage are we owed or owe,” you need to track retainage separately in QuickBooks.


Here’s the exact setup we recommend.


## **Step 1: Create the Retainage Accounts (Chart of Accounts)**


In **QuickBooks Online → Chart of Accounts** , create two accounts:


### **1) Retention Receivable**


- **Account Type:** Other Current Assets


- **Detail Type:** Other Current Assets


- **Name:** Retention Receivable (or AR Retainage)


This tracks retainage **customers are holding back from you** .


### **2) Retention Payable**


- **Account Type:** Other Current Liabilities


- **Detail Type:** Other Current Liabilities


- **Name:** Retention Payable (or AP Retainage)


This tracks retainage **you are holding back from subcontractors** .


These accounts should live on the balance sheet — retainage is earned or owed, just not paid yet.


## **Step 2: Create Retainage Products & Services (This Part Matters)**


Go to **Sales → Products & Services → New** and create **two items** .


### **AR Retainage (for customer invoices)**


- **Type:** Service


- **Name:** AR Retainage


- **Income Account:** *Retention Receivable*
- **Description (optional):** Retainage withheld per contract


Yes, this is intentionally mapped to a balance sheet account. That’s what makes retainage reportable.


### **AP Retainage (for vendor bills)**


- **Type:** Service


- **Name:** AP Retainage


- **Expense Account:** *Retention Payable*
- **Description (optional):** Retainage withheld per contract


This ensures retainage withheld from subs lands in a liability account instead of distorting job costs.


## **Step 3: How to Use Retainage on Invoices (AR)**


When billing a customer:


Example:


- Total work completed: **$100,000**
- Retainage (10%): **-$10,000**
- Invoice total: **$90,000**


**Invoice lines:**


1. Normal job cost / revenue line: $100,000


2. **AR Retainage:** -$10,000


When the customer pays $90,000, apply the payment to the invoice.
The remaining $10,000 stays in **Retention Receivable** until it’s released.


## **Step 4: How to Use Retainage on Bills (AP)**


When entering a subcontractor bill:


Example:


- Sub bill: **$40,000**
- Retainage (10%): **-$4,000**
- Amount paid now: **$36,000**


**Bill lines:**


1. Normal job cost line: $40,000


2. **AP Retainage:** -$4,000


Pay the $36,000.
The $4,000 remains in **Retention Payable** until it’s released.


## **Step 5: Releasing Retainage (Later)**


### **Collecting AR Retainage**


- Create a new invoice using **AR Retainage** as a **positive** amount.


- When paid, it clears **Retention Receivable** .


### **Paying AP Retainage**


- Create a new bill using **AP Retainage** as a **positive** amount.


- When paid, it clears **Retention Payable** .


No guessing. No backtracking. No spreadsheet cleanup.


## **Why This Works**


- AR and AP aging stay accurate (you’re invoicing what’s actually due now).


- Retainage is visible instantly on the balance sheet.


- It’s easy to reconcile with job costing tools like Procore.


- Months later, when retainage is released, nothing breaks.


## **Bottom Line**


If retainage isn’t separated in QuickBooks, it’s invisible — until it becomes a problem. This setup keeps retainage clean, auditable, and easy to manage across every project.
