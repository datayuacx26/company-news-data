---
schema_version: "1.0.0"
document_id: "e2716367d1a24ac150e53cdc519222f8e39142f62af67660c0e929b466fc175f"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/form-w9"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T17:53:17.757440+00:00"
fetched_at: "2026-08-19T17:53:18.803914+00:00"
content_hash: "sha256:47fccc06ea92554a55ad3e665a8e703b98139e908644e7d7dbba2c63a05b5e45"
---

# Form W-9: Request for Taxpayer Identification Number (Filling It Out Correctly in 2026)

**Form W-9 is how you give a business your Taxpayer Identification Number so they can report payments to you on a 1099.** If you're a freelancer, independent contractor, LLC owner, or any vendor providing services to another US business, you'll be handed a W-9 before first check goes out. The form is short one page but entity classification checkboxes cause more misfilings than any other section.


This guide walks through Form W-9 line by line, explains March 2024 revisions (including new Line 3b for pass-through partnerships), covers when backup withholding kicks in, and clears up when a W-9 is right form vs. Form W-4 or a 1099.


## What Form W-9 is used for


Per[IRS Form W-9 page](https://www.irs.gov/forms-pubs/about-form-w-9) , you use Form W-9 to give a requester your correct Taxpayer Identification Number (TIN) so requester can file appropriate information return with IRS. The most common trigger is a business paying you for services they need your TIN to issue a 1099-NEC at year-end. Other triggers include:


- **1099-NEC:** Payments of $600+ for services from a non-employee
- **1099-MISC:** Rents, prizes, awards, other income
- **1099-INT / 1099-DIV:** Bank and brokerage account interest, dividends
- **1099-K:** Third-party network transactions (Stripe, PayPal, Venmo business)
- **1098:** Mortgage interest paid
- **1099-C:** Cancelled debt


The W-9 does not get sent to IRS. The requester keeps it on file and uses TIN on whatever information return they end up filing.


## Who fills out a W-9 and who doesn't


**You fill out a W-9 if you're:**


- An independent contractor or freelancer working with US businesses
- A single-member LLC (disregarded entity by default, reports on owner's return)
- A multi-member LLC, partnership, S-corp, or C-corp receiving payments
- A landlord receiving rental income from a property management company
- Anyone with a US bank or brokerage account that reports interest/dividends


**You don't fill out a W-9 if you're:**


- A W-2 employee you'd fill out[Form W-4](https://www.finlens.app/blogs/form-w4) instead so employer knows how much to withhold
- A foreign person or entity you'd fill out a W-8 series form (W-8BEN, W-8BEN-E) instead
- A payer collecting form requester is client / bank / broker


The single biggest source of confusion is between W-9 and W-4. If someone controls when, where, and how you work, and issues a paycheck, you're an employee → W-4. If you invoice, set your own hours, and use your own tools, you're a contractor → W-9. The[difference between W-2 and 1099](https://www.finlens.app/blogs/difference-between-w2-and-1099) guide covers where classification line actually falls.


## Line-by-line: Form W-9 in 2024/25


The current W-9 (rev. March 2024) has seven numbered lines plus signature block.


**Line 1 Name (as shown on your income tax return).** For individuals and sole proprietors, this is your legal name. For an LLC, this depends on Line 3a (see below).


**Line 2 Business name / disregarded entity name.** Only if it's different from Line 1. A single-member LLC operating as "Smith Consulting LLC" but reporting on owner's personal return would put "Jane Smith" on Line 1 and "Smith Consulting LLC" on Line 2.


**Line 3a Federal tax classification.** Check ONE box:


- Individual / sole proprietor
- C Corporation
- S Corporation
- Partnership
- Trust / estate
- LLC (with a required tax classification letter: C, S, or P)
- Other (with description)


For LLCs, letter matters:


- **Single-member LLC → do NOT check "LLC."** Check "Individual / sole proprietor" instead. This is #1 W-9 mistake a single-member LLC is a disregarded entity by default, so owner's Line 1 name and owner's SSN or EIN goes in Part I.
- **Multi-member LLC (default) → LLC + P** for partnership
- **LLC that elected S-corp taxation → LLC + S**
- **LLC that elected C-corp taxation → LLC + C**


**Line 3b NEW in March 2024.** Check this box if you're a partnership, trust, or estate providing form to a partnership, trust, or estate with foreign partners/owners/beneficiaries. This was added so pass-through entities can flag foreign-ownership situations that trigger additional withholding rules.


**Line 4 Exemptions.** Most filers leave both boxes blank. The two codes cover backup-withholding exemptions (mostly corporations, IRAs, government entities) and FATCA exemptions.


**Line 5 & 6 Address.** The address requester will use to mail your 1099.


**Line 7 Requester's information (optional).** Some businesses fill this in themselves before handing you form.


**Part I TIN.** Enter EITHER your SSN OR your EIN, not both. Sole proprietors can use either SSN is default; EIN is used if you have one. Single-member LLCs typically enter owner's SSN (or owner's EIN if they have one), NOT LLC's EIN.


**Part II Certification.** Sign and date. You're certifying (1) TIN is correct, (2) you're not subject to backup withholding, (3) you're a US person, and (4) any FATCA codes are correct.


## Backup withholding 24% penalty


If IRS notifies payer that your name and TIN don't match IRS record, or that you've under-reported interest/dividend income, payer must start withholding **24% of every payment** and remitting it to IRS. That's backup withholding, and it's reason certification on Line II matters falsely certifying "not subject to backup withholding" when you've been notified you are is a per-return penalty.


Getting off backup withholding requires resolving underlying issue with IRS (usually a TIN mismatch or an unreported-income notice), then providing a new W-9 to payer.


## When your entity classification changes


If you form an LLC mid-year, elect S-corp taxation, or change from single-member to multi-member, your W-9 information changes you must submit a NEW W-9 to every existing payer. Old 1099s issued to wrong TIN or wrong classification can trigger CP2100 notices to payer (name/TIN mismatch), who will then require you to submit a corrected W-9 or become subject to backup withholding.


For LLC owners weighing whether to make S-corp election, our[LLC bookkeeping software](https://www.finlens.app/blogs/llc-bookkeeping-software) guide covers accounting implications; tax election itself is a separate Form 2553 filing.


## Where W-9 sits in your business workflow


Every new US business relationship where you're being paid should start with:


1. **You send** a W-9 with your correct TIN and classification
2. **They pay** throughout year via check, ACH, wire, or platform
3. **They send** a 1099-NEC (for services $600+) or 1099-MISC (rents, other) by January 31 following year
4. **You reconcile** 1099s received against your own books; report income on Schedule C (sole prop) or your business return


If a 1099 doesn't arrive, you still report income 1099 is a reporting mechanism, not tax obligation itself.


For businesses on other side of this workflow collecting W-9s from vendors and issuing 1099s see our guide on[1099-K reporting rules](https://www.finlens.app/blogs/1099-k-reporting-cpa-guide) and how third-party network transactions interact with 1099-NEC / 1099-MISC categories.


## The most common W-9 mistakes


**Single-member LLC checking "LLC" box.** Should check "Individual / sole proprietor." A single-member LLC is disregarded by default, so owner is taxpayer.


**Using LLC's EIN when reporting on personal return.** If LLC is disregarded, SSN (or owner's EIN) goes in Part I, not LLC's EIN. Using LLC EIN triggers a TIN mismatch when IRS reconciles 1099s to owner's Schedule C.


**Signing without updating address.** The address on W-9 is where 1099 gets mailed. If you moved and never resubmitted, 1099 goes to old address and you may miss it.


**Multiple W-9s outstanding with mismatched entity classifications.** If one payer has you as "Individual" and another has you as "LLC," you'll get conflicting 1099s and a reconciliation headache. Pick one, and resubmit to everyone.


**Certifying not subject to backup withholding when you are.** If IRS notified you of a mismatch or unreported income, you must strike through item 2 in Part II certification before signing. Signing without strike-through is a false certification.


## Conclusion


**W-9 is a five-minute form that determines every 1099 you'll receive.** Get entity classification wrong and you'll spend months resolving mismatch notices; get it right once and payers use it for years.


## Frequently asked questions


### **Do I have to give my SSN on a W-9?**


**‍** If you're an individual or single-member LLC without an EIN, yes. You can apply for an EIN for free from IRS if you'd rather not give your SSN an EIN provides same TIN function without exposing your personal number.


### ‍ **How long is a W-9 valid?**


Indefinitely, until your information changes (name, TIN, entity classification, or address). The IRS suggests requesters ask for a new W-9 every few years to keep records current, but it's not a hard requirement.


### **What if a client doesn't give me a W-9 to fill out?**


Ask for one. If they don't have a form, you can download current version from IRS website. Some businesses use their own vendor-onboarding form that collects same information.


### **Can I refuse to give a W-9?**


Legally, no, if payment triggers information reporting. If you refuse, payer must apply 24% backup withholding to every payment. Practically, you should provide a W-9 to any legitimate US business that requests one.


### **Does a foreign contractor fill out a W-9?**


No foreign persons or entities complete a W-8 series form (W-8BEN for individuals, W-8BEN-E for entities). W-9 is for US persons only.


### **W-9 vs 1099 are they same?**


No. W-9 is input (you give payer your TIN). 1099 is output (payer reports payments to IRS and to you following January).


### **When does a W-9 trigger backup withholding automatically?**


If you refuse to provide form, provide an obviously incorrect TIN, or refuse to certify Part II. The 24% withholding starts immediately and continues until a valid W-9 is on file.


### **Do I need a W-9 from a corporation?**


For most 1099-NEC reporting, no payments to C-corps and S-corps are exempt from 1099-NEC (with exceptions for legal fees and medical payments). But collecting a W-9 up front confirms corporate status and protects payer if entity turns out to be a sole prop after all.
