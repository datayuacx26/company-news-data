---
schema_version: "1.0.0"
document_id: "0ac1fb3b9fd57ef1185b13be2d57673f8432c2e486daa3cae16a2c8266f67b05"
company_key: "yc-aside"
company: "Aside"
source_id: "yc-aside-news-import-e4a720ca74ad"
canonical_url: "https://aside.com/blog/researchers"
published_at: "2026-06-22T07:00:00+00:00"
first_seen_at: "2026-07-21T08:01:04.221364+00:00"
fetched_at: "2026-07-28T21:23:18.688239+00:00"
content_hash: "sha256:676b057a395877bf1543c6e8ef5bff9adf9f60e6e24625059ea26bb64a717cd0"
---

# Aside for Researchers

A market brief from last month says the free plan includes 10 seats. Today the pricing page says 5. The source did not disappear, but the claim changed.


Give Aside the old brief and a schema. It can rerun the source pass in the background, compare current pages against old claims, and return changed text, screenshots, source URLs, downloaded files, and review status.


**Schema first:** Tell Aside the fields you need before it browses. Source URL, publisher, date, claim, screenshot, and review status are enough for a useful first pass.


## Evidence capture from live and private sources


Startup research often starts as a messy browser pass through pricing pages, security docs, technical blogs, paywalled databases, PDFs, review sites, and funding pages. Aside can capture the page state while it collects sources: URL, date accessed, claim, screenshot, downloaded file, changed or missing claim, and review status.


Evidence field Why it matters


Claim The sentence or data point the brief depends on


Source URL The page a reviewer can reopen


Date accessed The date the page was checked


Screenshot A record for pages that may change


Status Verified, changed, missing, or needs review


Old claim Current source Evidence Status


Free plan includes 10 seats Pricing page now lists 5 seats Screenshot, source URL, accessed date, old brief line Changed, needs reviewer note


Evidence capture


Build an evidence table for browser agents in enterprise SaaS workflows. Open company pages, security pages, technical blogs, paywalled databases, and research papers. For each source, capture title, publisher, URL, date accessed, claim, screenshot or downloaded file, changed-or-missing status, and reviewer status.


## Turn source monitoring into Routines


For questions you check every week, save the collection pass as a Routine. Every Friday, Aside can check the same pricing pages, keep the schema and exclusions from last time, compare the new table against last week's version, and flag changed limits or pages that moved behind sales contact. If a database export or long download is not ready, it can reopen the task later and try again.


Keep the extraction schema, last source set, known exclusions, and prior result counts so each new run compares against the same method.


Friday drift report Example


Changed claim "Free plan includes 10 seats" now says "5 seats"


Moved page Security whitepaper URL redirects to a gated form


Missing evidence Pricing page removed public API limits


Files handled 12 PDFs downloaded, renamed, and linked back to source rows


Reviewer action Update brief lines 14, 22, and 31 before sharing


**Routine candidates:** Weekly market scans, citation verification, pricing page captures, new-paper searches, and source pages that need screenshots because they change.


## PDF extraction without losing the method


PDFs get separated from the row, citation, and note they support. Aside can open the database results page, sign in if needed, download PDFs, rename them, extract fields, and flag uncertain values for review.


PDF extraction


Open this database results page, sign in if needed, download the PDFs, and rename each file as first-author-year-short-title.pdf. Create a spreadsheet with source URL, downloaded filename, sample size, method, outcomes, limitations, and review status.


## Pricing page capture


Pricing pages change. If you need a market snapshot, ask Aside to capture screenshots, extract plan names and limits, cite the URL, and flag pages that require a sales call.


Ask for a plan table, one screenshot per page, a sales-gated flag, and a last-checked date.


Pricing capture


Compare these five pricing pages. Capture screenshots, extract plan names and limits, cite the page URL, and flag anything that requires sales contact.


## Ask sources for missing information


Research often hits a wall because a page hides pricing, a database export takes time, or a support form is the only way to confirm a policy. Have Aside stage the question, wait for a reply, and reopen the task when the answer arrives.


Keep interpretation with the researcher. Aside can keep the waiting loop alive and attach the reply to the evidence table.


Ask and wait


For vendors that hide enterprise pricing, open the pricing page, support form, and source table. Stage one pricing-clarification question per vendor, wait for my approval before sending, then reopen this task when replies arrive and attach each answer to the evidence table.


## Claim verification for an existing brief


Old briefs decay. Links move, claims change, and screenshots go stale. Aside can open every source, capture the current page, compare it against the brief, and mark each line as verified, changed, missing, or needs human review.


Claim verification


Review the sources in this market brief. Open each link, capture a screenshot, compare the current page against the claim in the brief, and mark claims as verified, changed, missing, or needs human review. Include the changed text or missing-page evidence when a claim fails.


## Browsing history recall


Research often depends on the page you saw last month and never saved. Aside can search your browsing history, reopen likely sources, capture screenshots, and explain why each page probably belongs in the project.


History recall


Search my browsing history for pages I used in the procurement automation research last month. Reopen the likely sources, capture screenshots, explain why each page is likely relevant, and export grouped source rows with URL, title, date visited, topic, and review status.


## Use Ultrabrowse when the source pass is long


For a 30-source market map or source pack, one page is not enough. With Ultrabrowse, Aside can move through long result pages, PDFs, screenshots, forms, private databases, and structured exports before it stops with a table for review. Use a regular task for short verification. Use Routines when the same monitor should run again.


A web fetch stops at the public page. Aside can use the browser state you already have, including authenticated research tools and pages that only exist inside your company's account.


## Keep interpretation with the researcher


Use Aside for the collection pass: gather source URLs, download PDFs, capture screenshots for pages that may change, extract fields into a sheet, and mark claims that need human review. Missing pages, paywalls, changed claims, stale screenshots, and low-confidence extraction should stay visible.


The researcher still decides what counts as evidence.


## Questions researchers ask


###


###


###


###


###
