---
schema_version: "1.0.0"
document_id: "79e5e2db3bdfc673bc0538eaa5817401c1bdd813185f2d01218aa5e3c7909778"
company_key: "yc-mito"
company: "Mito"
source_id: "yc-mito-news-import-220d1fd2c6bc"
canonical_url: "https://www.trymito.io/blog/how-to-automate-reconciliations-in-python-a-complete-guide"
published_at: null
first_seen_at: "2026-07-24T04:41:15.374247+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:537ef8e68165286c3f2aceb220956a87d1b988bb95fc5b5369d4c7edeb56bce7"
---

# How to Automate Reconciliations in Python: A Complete Guide

Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)


Reconciliation is one of the most critical yet time-consuming tasks in accounting and finance. Whether you're matching bank transactions, comparing invoices with payments, or reconciling inventory records, the manual process of checking line-by-line entries is tedious, error-prone, and inefficient. For finance teams processing thousands of transactions monthly, reconciliation can consume entire workdays that could be better spent on strategic analysis.


Python automation transforms this painful process into an efficient, accurate workflow that runs in minutes rather than hours. In this comprehensive guide, we'll explore how to automate various types of reconciliations using Python, complete with practical code examples you can adapt to your specific needs.


## Why Automate Reconciliations with Python?


Before diving into implementation, let's understand the transformative benefits of reconciliation automation:


**Dramatic Time Savings** : Reduce reconciliation time from hours or days to just minutes, allowing your team to close books faster and focus on higher-value activities.


**Improved Accuracy** : Eliminate human error from manual data entry and comparison, ensuring every discrepancy is caught with mathematical precision.


**Complete Audit Trail** : Automatically document every matched transaction and exception, creating comprehensive records for compliance and audit purposes.


**Scalability** : Process reconciliations for multiple accounts, subsidiaries, or time periods simultaneously without increasing headcount.


**Consistency** : Apply the same matching logic uniformly across all transactions, removing subjective judgment calls that can vary between team members.


## Understanding Reconciliation Types


Reconciliations generally fall into several categories, each requiring slightly different approaches:


**Bank Reconciliation** : Matching company records against bank statements to identify outstanding checks, deposits in transit, and errors.


**Intercompany Reconciliation** : Ensuring transactions between related entities balance correctly in each company's books.


**Account Reconciliation** : Verifying that subsidiary ledger balances match general ledger control accounts.


**Three-Way Matching** : Confirming that purchase orders, receiving documents, and supplier invoices all agree before payment.


The good news? Python can handle all of these with similar core logic focused on intelligent data matching.


## Essential Python Libraries for Reconciliation


You'll need these key libraries to build robust reconciliation automation:


python


```text
pip install pandas numpy openpyxl fuzzywuzzy python-Levenshtein
```


**pandas** : The workhorse for data manipulation and comparison operations.


**numpy** : Enables efficient numerical calculations for amount matching with tolerances.


**openpyxl** : Reads and writes Excel files, crucial since most financial data arrives in spreadsheets.


**fuzzywuzzy** : Performs fuzzy string matching for descriptions that don't match exactly.


## Building Your First Automated Reconciliation


Let's start with a practical bank reconciliation example that matches company transactions against bank statements.


### Step 1: Load and Prepare Data


python


```text
import pandas as pd
import numpy as np
from fuzzywuzzy import fuzz


# Load company records and bank statement
company_records = pd.read_excel('company_transactions.xlsx')
bank_statement = pd.read_excel('bank_statement.xlsx')


# Standardize column names
company_records.columns = ['Date', 'Description', 'Reference', 'Amount']
bank_statement.columns = ['Date', 'Description', 'Reference', 'Amount']


# Convert dates to datetime
company_records['Date'] = pd.to_datetime(company_records['Date'])
bank_statement['Date'] = pd.to_datetime(bank_statement['Date'])


# Clean and standardize amounts
company_records['Amount'] = pd.to_numeric(company_records['Amount'], errors='coerce')
bank_statement['Amount'] = pd.to_numeric(bank_statement['Amount'], errors='coerce')


print(f"Company Records: {len(company_records)} transactions")
print(f"Bank Statement: {len(bank_statement)} transactions")
```


This foundational code loads your data sources and standardizes formats, ensuring consistent comparison logic throughout the reconciliation process.


### Step 2: Implement Exact Matching


python


```text
def exact_match_reconciliation(company_df, bank_df, tolerance=0.01):
"""
Perform exact matching on amount and date with optional tolerance
"""
matched_pairs = []
unmatched_company = company_df.copy()
unmatched_bank = bank_df.copy()


# Add tracking columns
unmatched_company['Matched'] = False
unmatched_bank['Matched'] = False


for idx_c, company_row in company_df.iterrows():
if unmatched_company.loc[idx_c, 'Matched']:
continue


for idx_b, bank_row in bank_df.iterrows():
if unmatched_bank.loc[idx_b, 'Matched']:
continue


# Check if amounts match within tolerance
amount_match = abs(company_row['Amount'] - bank_row['Amount']) <= tolerance


# Check if dates match (within 3 days for processing delays)
date_diff = abs((company_row['Date'] - bank_row['Date']).days)
date_match = date_diff <= 3


if amount_match and date_match:
matched_pairs.append({
'Company_Index': idx_c,
'Bank_Index': idx_b,
'Date': company_row['Date'],
'Amount': company_row['Amount'],
'Company_Desc': company_row['Description'],
'Bank_Desc': bank_row['Description'],
'Match_Type': 'Exact'
})


unmatched_company.loc[idx_c, 'Matched'] = True
unmatched_bank.loc[idx_b, 'Matched'] = True
break


# Filter out matched transactions
unmatched_company = unmatched_company[~unmatched_company['Matched']].drop('Matched', axis=1)
unmatched_bank = unmatched_bank[~unmatched_bank['Matched']].drop('Matched', axis=1)


return pd.DataFrame(matched_pairs), unmatched_company, unmatched_bank


# Run exact matching
matches, unmatched_co, unmatched_bank = exact_match_reconciliation(
company_records, bank_statement
)


print(f"\nExact Matches Found: {len(matches)}")
print(f"Unmatched Company Transactions: {len(unmatched_co)}")
print(f"Unmatched Bank Transactions: {len(unmatched_bank)}")
```


This exact matching function handles the majority of straightforward reconciliations where transactions match perfectly on amount and date.


### Step 3: Implement Fuzzy Matching for Complex Cases


Many transactions won't match exactly due to description variations. Fuzzy matching solves this:


python


```text
def fuzzy_match_reconciliation(unmatched_company, unmatched_bank,
similarity_threshold=80):
"""
Use fuzzy string matching for description comparison
"""
fuzzy_matches = []
remaining_company = unmatched_company.copy()
remaining_bank = unmatched_bank.copy()


remaining_company['Matched'] = False
remaining_bank['Matched'] = False


for idx_c, company_row in unmatched_company.iterrows():
if remaining_company.loc[idx_c, 'Matched']:
continue


best_match_score = 0
best_match_idx = None


for idx_b, bank_row in unmatched_bank.iterrows():
if remaining_bank.loc[idx_b, 'Matched']:
continue


# Check amount match first
amount_match = abs(company_row['Amount'] - bank_row['Amount']) <= 0.01


if amount_match:
# Calculate description similarity
similarity = fuzz.ratio(
str(company_row['Description']).lower(),
str(bank_row['Description']).lower()
)


if similarity > best_match_score and similarity >= similarity_threshold:
best_match_score = similarity
best_match_idx = idx_b


if best_match_idx is not None:
fuzzy_matches.append({
'Company_Index': idx_c,
'Bank_Index': best_match_idx,
'Date': company_row['Date'],
'Amount': company_row['Amount'],
'Company_Desc': company_row['Description'],
'Bank_Desc': unmatched_bank.loc[best_match_idx, 'Description'],
'Match_Type': 'Fuzzy',
'Similarity_Score': best_match_score
})


remaining_company.loc[idx_c, 'Matched'] = True
remaining_bank.loc[best_match_idx, 'Matched'] = True


remaining_company = remaining_company[~remaining_company['Matched']].drop('Matched', axis=1)
remaining_bank = remaining_bank[~remaining_bank['Matched']].drop('Matched', axis=1)


return pd.DataFrame(fuzzy_matches), remaining_company, remaining_bank


# Run fuzzy matching on remaining transactions
fuzzy_matches, final_unmatched_co, final_unmatched_bank = fuzzy_match_reconciliation(
unmatched_co, unmatched_bank
)


print(f"\nFuzzy Matches Found: {len(fuzzy_matches)}")
print(f"Final Unmatched Company: {len(final_unmatched_co)}")
print(f"Final Unmatched Bank: {len(final_unmatched_bank)}")
```


Fuzzy matching catches transactions where descriptions vary slightly due to abbreviations, extra spaces, or different formatting.


### Step 4: Handle Many-to-One Matching


Sometimes multiple transactions reconcile to a single entry, such as several checks matching one bulk deposit:


python


```text
def group_matching(unmatched_company, unmatched_bank, tolerance=0.01):
"""
Match groups of transactions that sum to a single transaction
"""
group_matches = []


for idx_b, bank_row in unmatched_bank.iterrows():
bank_amount = bank_row['Amount']
bank_date = bank_row['Date']


# Find company transactions within date range
date_range = unmatched_company[
(unmatched_company['Date'] >= bank_date - pd.Timedelta(days=5)) &
(unmatched_company['Date'] <= bank_date + pd.Timedelta(days=5))
]


# Try to find combinations that sum to bank amount
for size in range(2, min(6, len(date_range) + 1)):
from itertools import combinations


for combo in combinations(date_range.index, size):
combo_sum = unmatched_company.loc[list(combo), 'Amount'].sum()


if abs(combo_sum - bank_amount) <= tolerance:
group_matches.append({
'Bank_Index': idx_b,
'Company_Indices': list(combo),
'Bank_Amount': bank_amount,
'Company_Total': combo_sum,
'Date': bank_date,
'Transaction_Count': len(combo),
'Match_Type': 'Group'
})
break


return pd.DataFrame(group_matches)


# Find group matches
group_matches = group_matching(final_unmatched_co, final_unmatched_bank)
print(f"\nGroup Matches Found: {len(group_matches)}")
```


This advanced matching technique handles batch deposits and bulk payments that might otherwise appear as exceptions.


### Step 5: Generate Comprehensive Reconciliation Report


python


```text
def generate_reconciliation_report(exact_matches, fuzzy_matches,
group_matches, unmatched_co, unmatched_bank):
"""
Create comprehensive reconciliation report with all results
"""
# Combine all matches
all_matches = pd.concat([exact_matches, fuzzy_matches], ignore_index=True)


# Calculate summary statistics
total_company = len(company_records)
total_bank = len(bank_statement)
total_matched = len(all_matches) + len(group_matches)
match_rate = (total_matched / total_company) * 100


# Create Excel report with multiple sheets
with pd.ExcelWriter('reconciliation_report.xlsx', engine='openpyxl') as writer:
# Summary sheet
summary_data = {
'Metric': [
'Total Company Transactions',
'Total Bank Transactions',
'Exact Matches',
'Fuzzy Matches',
'Group Matches',
'Total Matched',
'Match Rate (%)',
'Unmatched Company',
'Unmatched Bank'
],
'Value': [
total_company,
total_bank,
len(exact_matches),
len(fuzzy_matches),
len(group_matches),
total_matched,
f"{match_rate:.2f}",
len(unmatched_co),
len(unmatched_bank)
]
}
pd.DataFrame(summary_data).to_excel(writer, sheet_name='Summary', index=False)


# Matched transactions
all_matches.to_excel(writer, sheet_name='Matched', index=False)


# Group matches
if len(group_matches) > 0:
group_matches.to_excel(writer, sheet_name='Group Matches', index=False)


# Exceptions requiring review
unmatched_co.to_excel(writer, sheet_name='Unmatched Company', index=False)
unmatched_bank.to_excel(writer, sheet_name='Unmatched Bank', index=False)


print("\n" + "="*50)
print("RECONCILIATION SUMMARY")
print("="*50)
print(f"Match Rate: {match_rate:.2f}%")
print(f"Total Matched: {total_matched}")
print(f"Exceptions Requiring Review: {len(unmatched_co) + len(unmatched_bank)}")
print("\nReport saved to 'reconciliation_report.xlsx'")


# Generate final report
generate_reconciliation_report(
matches, fuzzy_matches, group_matches,
final_unmatched_co, final_unmatched_bank
)
```


This creates a professional Excel report with multiple tabs organizing all reconciliation results for easy review.


## Advanced Reconciliation Techniques


For more complex scenarios, consider implementing these enhancements:


**Multi-Currency Handling** : Convert all amounts to a base currency using exchange rates before comparison.


**Automated Exception Classification** : Use machine learning to categorize unmatched items based on patterns in historical data.


**Scheduled Automation** : Deploy your reconciliation script to run automatically using task schedulers, processing new files as they arrive.


**Integration with Accounting Systems** : Connect directly to your ERP or accounting software APIs to eliminate manual file exports.


## Best Practices for Reconciliation Automation


**Data Quality Checks** : Always validate input data for completeness, correct formatting, and reasonable values before reconciliation begins.


**Version Control** : Track changes to matching rules and maintain audit logs showing exactly which logic version processed each reconciliation.


**Threshold Configuration** : Make matching tolerances and date ranges configurable parameters rather than hard-coded values.


**Exception Management** : Build workflows for efficiently reviewing and resolving exceptions identified by automation.


**Testing** : Always test reconciliation logic against known datasets with verified results before processing live data.


## Conclusion


Automating reconciliations with Python represents a fundamental shift from tedious manual checking to intelligent, scalable processing. The code examples in this guide provide a solid foundation you can customize for your specific reconciliation needs, whether matching bank transactions, reconciling intercompany accounts, or verifying invoice payments. Start by implementing exact matching for your highest-volume reconciliations, then gradually add fuzzy matching and group matching capabilities as you gain confidence. The time saved and accuracy gained will quickly prove the value of reconciliation automation.


## More Like This


[Automating Spreadsheets with Python 101 How to tell the difference between a good and bad Python automation target.](https://www.trymito.io/blog/automating-spreadsheets-with-python-101)[10 Mistakes To Look Out For When Transitioning from Excel To Python 10 Common Mistakes for new programmers transitioning from Excel to Python](https://www.trymito.io/blog/10-mistakes-to-look-out-for-when-transitioning-from-excel-to-python)[Research shows Mito speeds up by 400% We're always on the hunt for tools that improve our efficiency at work. Tools that let us accomplish more with less time, money, and resources.](https://www.trymito.io/blog/quantifying-mitos-impact-on-analyst-python-productivity)[3 Rules for Choosing Between SQL and Python Analysts at the world's top banks are automating their manual Excel work so they can spend less time creating baseline reports, and more time building new analyses that push the company forward.](https://www.trymito.io/blog/choosing-between-sql-and-python-best-practices-for-data-analytics-workflows)


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)
