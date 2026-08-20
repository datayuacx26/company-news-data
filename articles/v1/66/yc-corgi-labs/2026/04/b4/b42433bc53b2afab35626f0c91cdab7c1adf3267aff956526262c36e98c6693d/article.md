---
schema_version: "1.0.0"
document_id: "b42433bc53b2afab35626f0c91cdab7c1adf3267aff956526262c36e98c6693d"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/resources/sales-tax"
published_at: "2026-04-23T14:48:38.915+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-22T22:57:19.890747+00:00"
content_hash: "sha256:40653683b6222bbe2da44920584acb704b21afb59fc55222e09b361a825e59f8"
---

# Sales Tax

The Sales Tax page helps you track sales tax thresholds and estimated tax due by jurisdiction, covering US states and international markets. Use it to see where your revenue and transaction counts stand against each jurisdiction's economic nexus rules, and to understand when collection obligations are triggered.


## Jurisdiction Tax Calculation


The main table provides a comprehensive view of your sales tax obligations across all relevant jurisdictions. Each row corresponds to one jurisdiction, and the columns describe the data Corgi surfaces against it.


Column


What it shows


Jurisdiction


The state, territory, or country name.


Tax Type


The type of tax applicable (Sales Tax, Local Sales Tax, GST, GET, GRT, JCT, TPT, IVU, etc.).


Tax Rate


The applicable tax rate for the jurisdiction.


Revenue


Your total revenue in that jurisdiction during the selected period.


Transactions


Number of transactions in the jurisdiction.


Threshold


The economic nexus threshold that determines when you are required to collect and remit tax. Thresholds are typically based on revenue amount, transaction count, or both (for example, "$100,000 OR 200 transactions").


Threshold Status


Whether you have met the threshold. **Threshold Met** means you need to collect tax; **Not Met** means you are not yet obligated.


Read each row as a snapshot of your current exposure in that jurisdiction for the selected period. The Threshold Status column is the fastest signal for where action may be required.


## Covered Jurisdictions


Corgi tracks thresholds and obligations for:


-


**All US states** with sales tax obligations, including states with no sales tax (Delaware, Montana, New Hampshire, Oregon) which are shown with a "No nexus threshold" indicator.


-


**US territories** such as Puerto Rico and the District of Columbia.


-


**International markets** including Japan (JCT), Singapore (GST), and others depending on where your customers are located.


Coverage expands as your customer footprint grows, so jurisdictions appear in the table once you have qualifying activity in them.


## Understanding Thresholds


Economic nexus thresholds vary by jurisdiction. Common formats include:


-


**Revenue-only.** For example, $100,000 in annual sales.


-


**Revenue OR transaction count.** For example, $100,000 OR 200 transactions (meeting either triggers the obligation).


-


**Revenue AND transaction count.** For example, $100,000 AND 200 transactions (both must be met).


-


**No threshold.** Some jurisdictions have no economic nexus threshold, meaning any sale may trigger obligations.


Corgi automatically calculates your revenue and transaction counts against each jurisdiction's thresholds and indicates which ones you have reached. This lets you monitor progress toward nexus before you cross it, rather than discovering the obligation after the fact.


## Important Notes


-


Sales tax calculations are estimates based on your transaction data. Consult a tax professional for compliance decisions.


-


Corgi tracks thresholds based on approved transactions. Refunded or disputed amounts may affect your actual obligations.


-


Threshold rules change periodically. Corgi updates threshold information regularly, but you should verify current rules with the relevant tax authority.
