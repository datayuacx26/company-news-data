---
schema_version: "1.0.0"
document_id: "6e3ecdbc2b68c7a550f2272654599b561bd4e40d8e237c664267e9cad66623aa"
company_key: "yc-leadgenius"
company: "LeadGenius"
source_id: "yc-leadgenius-news-import-60e73975c0bf"
canonical_url: "https://www.leadgenius.com/resources/the-decision-maker-at-a-small-business-is-not-on-linkedin"
published_at: "2026-06-04T20:24:21.429+00:00"
first_seen_at: "2026-07-25T11:51:49.719746+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:e3818e49e12db7e05397341291dc0cef2fa85fc5681f7d75f1dbb86820018e97"
---

# The decision-maker at a small business is not on LinkedIn.

The Short Answer


Owner-operator data is the contact and identity information for the individual who both **owns and runs** a small business — pulled from Secretary of State officer filings and enriched with direct phone, direct email, and home address. It is the only reliable way to reach the buyer at an SMB, because small businesses don't have job-title-based decision-makers. The owner-operator is the procurement department, the IT department, and the buying committee — **all in one person.**


## Why owner-operator targeting is different from B2B title targeting


Enterprise B2B sales playbooks were built around job titles. You target the "VP of Procurement" or the "Head of Marketing Operations." Those titles exist because mid-market and enterprise companies have *functional specialization* — there's a person whose job is to evaluate vendors in your category.


At a small business, there isn't. According to SBA data, roughly **81% of US small businesses have no employees other than the owner** , and another 16% have between 1 and 19 employees. In both cases, the person who decides whether to buy your product is the owner. There is no VP of anything because the business doesn't *have* a vice presidency of anything.


81%


of US small businesses have no employees other than the owner


16%


have between 1 and 19 employees on payroll


≈ 1


person constitutes the entire buying committee


This means SMB outbound that imports the enterprise title-targeting playbook fails for a structural reason: **the buyer it's targeting doesn't exist.** The buyer is the owner. The data source that identifies the owner is the corporate registry, not LinkedIn.


## What owner-operator data contains


Owner-operator data is built from corporate registry officer filings, enriched with appended contact information. The standard field set falls into four groups:


Identity Fields


- ` name` ,` first_name` ,` last_name` — full name from the officer filing
- ` position` — typically "Member," "Managing Member," "President," "Owner," or "Sole Proprietor"
- ` occupation` — when reported, confirms whether the officer actually works in the business
- ` nationality` and` country_of_residence`
- ` partial_date_of_birth` — typically month and year


Tenure Fields


- ` start_date` — when the officer was filed; for owner-operators, often matches` incorporation_date`
- ` end_date` — when the officership ended (null for current owners)
- ` current_status` — active, resigned, removed


Contact Fields


- ` address.in_full` — the officer's contact address, frequently the home address for small business owners
- ` address.locality` ,` address.region` ,` address.postal_code` ,` address.country`
- ` officer_phone` — appended direct mobile
- ` officer_email_hem` — appended direct business or personal email


Linkage Fields


- ` company_number` +` jurisdiction_code` — joins the officer back to the company they own
- ` person_uid` — when available, deduplicates an individual across multiple companies they own


## How to identify the actual owner-operator (not the registered agent)


The most common mistake with officer data is treating every filed officer as an owner. The officer list at any Secretary of State includes *registered agents, compliance officers, and directors* who don't operate the business. For SMB outbound, you want only the operator.


### A clean identification rule, in five parts:


1. **Filter` position` to operator titles.** Strong signal: "Member," "Managing Member," "Sole Proprietor," "Owner," "President." Weak signal: "Director," "Treasurer," "Secretary." Reject outright: "Registered Agent," "Statutory Agent," "Service of Process."
2. **Cross-reference the officer address against the company's registered address.** When they match, the officer is operating from the business address — high operator likelihood. When both are residential and match, the signal is even stronger.
3. **Reject officers whose address matches a known registered-agent service provider** — CT Corporation, Cogency Global, Northwest Registered Agent, InCorp Services. These are professional registered agents, not owners.
4. **For multi-officer entities, take the officer with the earliest` start_date`** that matches the company's incorporation date — typically the founding owner.
5. **Use the` occupation` field when populated.** If it matches the business industry, treat that as confirmation of operator status.


Applied across a typical Secretary of State bulk delivery, this filter retains roughly **60–70% of filed officers as legitimate owner-operator candidates** and rejects the remainder as agents or passive officers.


## Where owner-operator contact data comes from


Not all data sources are equivalent. The table below compares the major options for reaching SMB decision-makers — by coverage, by direct-dial yield, and by what each source structurally *can* and *cannot* provide.


Source What it provides SMB coverage Direct-dial rate


Secretary of State officer filings Owner name, address, position, tenure ~95% of registered businesses 0% (filings rarely include phone)


Officer data with appended contact Above + direct phone, direct email ~95% of registered businesses 40–65% by jurisdiction


LinkedIn-derived B2B providers Job-title contacts at companies with LinkedIn pages 30–40% of SMBs 50–70% on covered contacts


Web-scraped people-search sites Personal contact info from public records High coverage of individuals Mixed accuracy; compliance risk


Voter file overlays Demographic and household data tied to person High coverage of individuals No direct dial


The combination of Secretary of State officer filings *with* appended direct phone and email is the only data source that systematically covers the long tail of SMBs **without a digital footprint** while also producing usable contact information.


## How sales teams use owner-operator data


i.


### Direct-dial outbound


The fundamental use case is calling the owner directly. The` officer_phone` field, when matched against active SMB targets, produces connect rates 2–4× higher than calling main business lines — small business owners don't have receptionists screening their direct mobile.


ii.


### Personalized cold email


The` officer_email_hem` field provides a direct email to the owner that doesn't go through a generic *info@* inbox. Combined with first name and the business's industry codes, this enables genuinely personalized cold email at scale — without scraping LinkedIn.


iii.


### Owner profile enrichment


Once the owner's name and home address are known, consumer attribute files can be matched to build a profile of the owner *as a consumer.* For SMB sales, this matters because the owner's personal characteristics — household income, lifestyle segment, vehicle ownership — correlate with how they buy for the business.


iv.


### Recently-changed-ownership signals


A new officer filing on an established business is a strong buying signal: ownership change drives technology and vendor decisions. Filtering` start_date` for recent additions on companies with an` incorporation_date` more than three years prior surfaces these transitions.


v.


### Multi-business owners


The` person_uid` field, where available, identifies individuals who own multiple businesses. A single owner running three QSR franchise locations is a different sales conversation than a single-location owner — the multi-unit operator is a higher-value account with more complex needs.


## Frequently asked questions


What is owner-operator data?


Owner-operator data is contact and identity information for the individual who both owns and actively runs a small business. It is built from corporate registry officer filings and typically includes name, position, address, direct phone, and direct email. It is the SMB equivalent of executive contact data at enterprise companies.


How do I find the owner of a small business?


The most reliable source is the Secretary of State officer filing for the business in its state of incorporation. The filing lists the owner by name, position, and address. Direct contact information (phone and email) is typically appended by data providers using identity resolution against other sources.


Is owner-operator data the same as executive contact data?


No. Executive contact data targets job-title-based decision-makers (VPs, Directors, Heads of) at mid-market and enterprise companies. Owner-operator data targets the registered officer who runs an SMB and is the entire buying committee. The data sources, fields, and outreach playbooks are different.


Can I get owner-operator data for businesses that aren't on LinkedIn?


Yes — this is the primary advantage of owner-operator data over LinkedIn-derived B2B databases. Because the data comes from corporate registry filings rather than employee profile scraping, coverage is independent of the business's digital footprint. A solo HVAC contractor with no website and no LinkedIn presence is still filed with the Secretary of State.


Is officer data compliant for sales outreach?


Officer filings are public records, so the underlying data is unrestricted for business-purpose outreach. Appended contact data (phone, email) is subject to TCPA, CAN-SPAM, and state-level privacy regulations like CCPA and CPRA. Compliant outbound treats appended officer mobile numbers the same as any direct mobile — with proper opt-out handling and DNC screening.


How do I tell if the officer I'm looking at is actually the owner?


Five signals: (1) the position field shows an operator title like Member, Managing Member, or President rather than Registered Agent; (2) the officer's address doesn't match a known professional registered-agent service; (3) the officer's start date matches the company's incorporation date; (4) the occupation field matches the business industry when populated; (5) for very small businesses, the officer is often the only filed officer.


What about businesses where the owner uses a holding company?


Multi-layer ownership structures are visible through the relationships file, which links subsidiaries and control statements. For SMBs this is rare — fewer than 5% of US small businesses use holding company structures. When it occurs, the operator is still typically named as an officer at the operating entity, even if the equity flows through a parent.
