---
schema_version: "1.0.0"
document_id: "0241a11c63bbbcbc21ade7bb43f3e828729c71253d6f173e642ef836992c0c7e"
company_key: "yc-serif-health"
company: "Serif Health"
source_id: "yc-serif-health-news-import-98e88b19e297"
canonical_url: "https://www.serifhealth.com/blog/provider-directory-use-cases"
published_at: "2024-10-17T22:41:00+00:00"
first_seen_at: "2026-07-22T13:10:17.126339+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:d48efa20b091bcce7e46926baf8d9ff4d59a1a1f10bfdef1d847700b2ffd4599"
---

# Provider Directory Use Cases

Since its release, price transparency data has been a useful treasure trove of reimbursement rate information and intelligence. You can execute fine-grained searches to identify what a specific provider group or facility has contracted for a procedure. You can also aggregate data to understand the key terms in someone’s fee schedule or what market median reimbursement looks like in a given MSA for a common service like imaging.


Apart from analyzing rates though, price transparency data also contains extremely useful information related to providers themselves. In this blog, we’ll share some exciting use cases for price transparency data that actually have nothing to do with prices:


## In-network status lookups for **provider credentialing** and **eligibility verification:**


By law, payers must disclose data for all providers inside their given network down to the individual NPIs and EINs they have active contracts with.


For example, if you are a provider credentialing company and want clarity from the payer on which professionals they have on their roster, you can use[Serif Health’s provider directory API](https://app.serifhealth.com/docs/apis/provider-directory/production) and search by NPI-2, NPI-1, or EIN to look-up if they appear inside the payer’s machine readable file for a given network.


This data is available by network product so if you are curious if someone participates in the payer’s narrow high performance HMO vs. their standard PPO you can retrieve this information in a simple look-up using the data:


*For example,* *North Texas Village Health Partners (left - NPI 1396881884) shows up in Blue Choice PPO, PAR, Blue Essentials but is missing from the BCBS TX’ Blue Premier, Blue Advantage plans…*


Additionally, if you are an eligibility verification company and need to pass through a valid NPI or EIN to receive a member’s EOB from a payer, you can seamlessly execute cross-walks between an individual NPI to their group NPI to their TIN until you confirm the right identifier to receive a payer reply to your eligibility verification request.


Again, this is possible via our NPI and EIN API relationships endpoints.


*Searching an individual NPI (e.g., Keith Eppich - 161998526) gives affiliated NPI-2s (e.g., North Texas Village Health Partners - 1396881884) and EINs (e.g., Village Health Partners, PA - 752918050)*


## EIN analytics for **providers** , **payers,** and **provider directory companies** :


A key missing element for most provider directory companies is a given practice or facility’s EIN. These EINs function similarly to businesses’ unique social security numbers and are usually not available publicly (except for non-profits).


However, given they do appear inside payers’ machine readable files, now analytics can incorporate EINs into their models to provide a more complete picture of payers’ networks, practices, or facilities.


Additionally, Serif Health has taken an extra step to build our own proprietary EINs database to enrich the 9-digit EINs available in the price transparency data with their associated names, addresses.


With EIN, EIN name, EIN location(s), affiliated NPI(s) all consolidated into one place, Serif Health has helped unlock new insights into provider groups and networks – e.g.,


### How many EINs does a group operate?


*(Alma has 2 EINs associated with its group)*


### **For each EIN, how many unique NPIs fall underneath it and how does this trend?**


#### **Alma EINs**


Alma EINs January 2024 NPI-1s


Sept 2024 - NPI-1s


% Change


841856765 ~16K


~19K


~20%


871927518 ~3.7K


~4.0K


~8%


### **When looking at MSOs, what % overlap do they have with each other’s rosters?**


#### **Alma % NPIs Overlap with Peer MSOs**


**Alma % NPIs Overlap with Peer MSOs**
January 2024 NPI-1s


MSO A ~35%


MSO B ~15%


*Can further cut this by location (e.g., state, CBSA), level of licensure (e.g., Masters, NP, PhD, MD), etc.*


### **What EINs did a payer include in their high performance network vs. their standard PPO?**


*BCBS NC’s High Performance Atrium Network has ~50% of the EINs of the standard PPO. Can further cut this by specialty, facility type.*


If these in any way resemble questions your organization is trying to answer, Serif Health has the market-leading suite of APIs and underlying data assets to make use of the provider references included in payer’s price transparency data. Please reach out tosales@serifhealth.com if you ever want to chat.


‍
