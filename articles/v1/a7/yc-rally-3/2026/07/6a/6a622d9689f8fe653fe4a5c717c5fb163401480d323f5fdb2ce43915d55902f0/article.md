---
schema_version: "1.0.0"
document_id: "6a622d9689f8fe653fe4a5c717c5fb163401480d323f5fdb2ce43915d55902f0"
company_key: "yc-rally-3"
company: "Rally"
source_id: "yc-rally-3-news-import-75685e7d2a3f"
canonical_url: "https://www.getrally.com/blog/reisekostenabrechnung-template"
published_at: "2026-07-12T00:00:00+00:00"
first_seen_at: "2026-07-22T10:49:14.710560+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:03613302f6e6d268082416ba5f78ba1b9a0102525040b84634757d714681ae8f"
---

# Reisekostenabrechnung Template 2026: Free Excel with Automatic Per Diems

Here is the download up front: our free **Reisekostenabrechnung template** for 2026 is an Excel form that does the annoying part — the per diem arithmetic — by itself. Pick the day type, tick which meals were provided, and the correct Verpflegungspauschale appears, meal reductions included. Mileage is calculated at €0.30 per kilometre, lodging and incidentals get their own receipted section, and the totals sum to one reimbursement figure.


**[Download the free Reisekostenabrechnung template (Excel)](https://www.getrally.com/assets/downloads/rally-reisekostenabrechnung-vorlage-2026.xlsx)** — no e-mail address required.


The form itself is in German — it is a German tax document — and works in Excel, LibreOffice, and Google Sheets. Below: what the template automates, the mandatory fields the Finanzamt expects, and the 2026 figures behind the formulas.


## What the Template Automates


Most free templates are a grid of empty cells. This one encodes the 2026 rules:


- **Per diems with automatic meal reductions.** For each travel day you select the day type — single-day trip over 8 hours, arrival/departure day, or full 24-hour day — and mark whether breakfast, lunch, or dinner was provided. The sheet applies the €14/€28 domestic rates and subtracts €5.60 per breakfast and €11.20 per lunch or dinner, never going below zero. Those reductions are the single most common per diem error, and they are exactly what a payroll auditor recalculates first.
- **Mileage at €0.30/km.** Enter the route and kilometres driven; the rate column is pre-filled with the 2026 business-trip rate for private cars and stays editable for motorcycles (€0.20) or a policy rate.
- **Lodging and incidentals with receipt references.** Hotel, train, parking, tolls, hospitality — each line with a category dropdown and a receipt number, because everything in this section needs a Beleg.
- **Header, totals, and signatures.** Traveller, cost centre, destination, business purpose, trip start and end with times, three subtotals, one grand total, and signature lines for employee and approver.


For foreign trips, enter the country rate from the BMF table manually in the per diem column — the domestic automation stays out of the way. The full 2026 country table, and every per diem rule in detail, is in our guide to the[Verpflegungsmehraufwand 2026](https://www.getrally.com/blog/verpflegungsmehraufwand-germany) .


## The Mandatory Fields (Pflichtangaben)


No statute prescribes a Reisekostenabrechnung form, but the tax rules dictate its content. To keep reimbursements tax-free under § 9 EStG, the record must show:


1. **Who travelled:** name, and in payroll practice the personnel number and cost centre.
2. **Why:** the business purpose (Reisezweck) — "customer visit, project X" beats "meeting".
3. **Where and exactly when:** destination plus start and end of the trip **with times** . Hours of absence decide between €0, €14, and €28, so times are not optional.
4. **The itemized costs:** per diems (no receipts needed — they are flat rates), transport costs, lodging, and incidentals with receipts attached.
5. **Provided meals:** flagged per day, because they reduce the allowance.
6. **Signatures or digital approval** of traveller and approver, with dates.


An incomplete record does not just annoy the bookkeeper: in a Lohnsteuer-Außenprüfung, reimbursements without proper documentation are reclassified as wages, and the employer owes the payroll tax plus interest — the audit window covers four years.


## The 2026 Figures Behind the Formulas


The template hard-codes only figures that are stable for 2026:


Item 2026 value


Per diem, absence over 8 hours / arrival & departure days €14


Per diem, full 24-hour day €28


Reduction: provided breakfast €5.60 (20% of €28)


Reduction: provided lunch or dinner €11.20 each (40% of €28)


Mileage, private car €0.30 per km driven


Mileage, motorcycle or scooter €0.20 per km driven


Truck-driver overnight flat rate (cab sleeping) €9 per calendar day


Two boundaries worth flagging to employees, because 2026 changed one of them:


- **€0.30/km is the business-trip rate, unchanged.** The *commuter* allowance (Pendlerpauschale) is a different animal — it rose to €0.38 per distance kilometre from the first kilometre in 2026, but it applies to the home-to-work route in the income tax return, never on an expense report. Mixing the two is a classic error in both directions.
- **Per diems stop after three months** at the same external location (Dreimonatsfrist), and days below 8 hours of absence pay nothing.


## Deadlines, Retention, and Digital Records


Three administrative rules complete the compliance picture:


- **Submission deadlines are contractual.** Tax law sets no deadline, but employment contracts and CBAs routinely contain Ausschlussfristen of three to six months — after that, the claim can lapse entirely. Encourage monthly submission; receipts do not age well.
- **Retention is eight years.** Expense reports and their receipts are Buchungsbelege. The Fourth Bureaucracy Relief Act cut their retention from ten to eight years (effective for records whose old period had not expired at the start of 2025). Annual financial statements remain at ten.
- **Digital is fine — GoBD applies.** Excel and PDF records are accepted if they are complete, unchangeable once filed, and legible for the full period. Scanned receipts can replace paper originals (ersetzendes Scannen) when the process is documented in a Verfahrensdokumentation.


## Common Mistakes on German Expense Reports


What auditors and payroll teams actually find:


- **Forgotten meal reductions** when the hotel rate included breakfast — the reduction applies even if the traveller skipped the buffet.
- **Missing times** on start and end days, making the per diem tier unverifiable.
- **The wrong country rate** on border-crossing days: the arrival day uses the country reached before midnight; the return day uses the last place of work abroad.
- **Per diems paid past the three-month limit** at the same site.
- **Pendlerpauschale logic on business trips** — one-way distance instead of kilometres driven, or 2026's €0.38 commuter rate instead of €0.30.
- **Lost receipts reimbursed without substitute documentation** — an Eigenbeleg with date, amount, purpose, and reason for the missing original is the accepted fallback, not a shrug.


## When the Spreadsheet Stops Scaling


A good template makes the per diem line painless — that part is genuinely just arithmetic on trip times. What it cannot fix is the paper economy around the trip: drivers fronting fuel and toll costs, receipts photographed weeks later, finance reconciling card statements against forms by hand, and VAT leaking from every non-compliant Beleg.


For fleets and field teams, the higher-leverage move is making the expense report shrink: route fuel, charging, tolls, and on-the-road purchases through[company cards](https://www.getrally.com/company-cards) with per-employee limits, so the receipt is captured at the point of payment and the accounting export is generated instead of typed. Fuel spend specifically consolidates onto[fuel cards](https://www.getrally.com/fuel-cards) with one invoice and per-transaction VAT. The Reisekostenabrechnung then carries what genuinely belongs on it — per diems and private-car mileage — and nothing else.


## Sources


- [§ 9 EStG — income-related expenses, per diems, and mileage (Abs. 1 Nr. 4a, Abs. 4a)](https://www.gesetze-im-internet.de/estg/__9.html)
- [BMF letter of 5 December 2025 — per diem and overnight rates for foreign business travel from 1 January 2026](https://www.bundesfinanzministerium.de/Content/DE/Downloads/BMF_Schreiben/Steuerarten/Lohnsteuer/2025-12-05-steuerliche-behandlung-reisekosten-2026.pdf)
- § 147 AO as amended by the Fourth Bureaucracy Relief Act (BEG IV) — eight-year retention for accounting records
- Steueränderungsgesetz 2025 — commuter allowance of €0.38 from the first kilometre as of 1 January 2026


*This article is general information, not tax advice. Rates are set by German law and the Federal Ministry of Finance and reviewed annually; verify specifics for your situation with your Steuerberater.*
