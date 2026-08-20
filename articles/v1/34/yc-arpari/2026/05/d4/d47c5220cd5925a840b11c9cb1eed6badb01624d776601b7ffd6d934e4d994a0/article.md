---
schema_version: "1.0.0"
document_id: "d47c5220cd5925a840b11c9cb1eed6badb01624d776601b7ffd6d934e4d994a0"
company_key: "yc-arpari"
company: "Arpari"
source_id: "yc-arpari-news-import-d67ae8fe8438"
canonical_url: "https://arpari.com/post/check-positive-pay-vs-ach-positive-pay-why-one-without-the-other-leaves-a-gap"
published_at: "2026-05-27T00:00:00+00:00"
first_seen_at: "2026-07-24T17:09:57.576780+00:00"
fetched_at: "2026-07-28T22:36:37.412728+00:00"
content_hash: "sha256:e9168af2f62fae5112890f5b2388a681218d9e8fd682b457ead97fa44b80cafc"
---

# Check Positive Pay vs ACH Positive Pay: Why One Without the Other Leaves a Gap

Most controllers and treasury managers understand positive pay in concept: the bank matches outgoing payments against an authorized list before clearing them. What many teams underestimate is that check positive pay and ACH positive pay are two entirely separate controls protecting two entirely separate payment channels. Enabling one does not protect the other. A company with check positive pay in place but no ACH debit block or ACH positive pay is still exposed to unauthorized electronic debits hitting its accounts. For multi-bank real estate portfolios managing dozens of operating accounts, that gap multiplies with every bank relationship. Our team estimates that 40% to 60% of property management companies have check positive pay enabled but lack equivalent ACH controls on the same accounts.


## How Check Positive Pay Works


Check positive pay is straightforward. The company sends the bank a file of issued checks, including check number, amount, payee, and date. When a check is presented for payment, the bank compares it against that file. If the details do not match, the item is flagged as an exception for the company to review before the bank pays or returns it. This catches altered checks, forged checks, and checks written against closed or unauthorized accounts. It is a well-established check fraud control that most banks have offered for decades.


The limitation is scope. Check positive pay only covers paper checks. It says nothing about electronic transactions.


## How ACH Positive Pay Works


ACH positive pay applies the same principle to electronic debits. Instead of matching check numbers, the company defines rules for which originators are authorized to debit specific accounts, sometimes including amount limits and frequency parameters. When an ACH debit arrives that falls outside those rules, the bank flags it for review. An ACH debit block is the stricter version: it rejects all incoming ACH debits on an account unless the company has explicitly authorized them.


ACH fraud does not look like check fraud. It does not need a forged signature. It just needs an account number.


## Why Real Estate Portfolios Need Both


Property management companies are uniquely exposed. Operating accounts receive rent deposits and process vendor payments across dozens of properties. Each account is a potential target through both channels. A compromised vendor relationship can generate a fraudulent check. A leaked account number can trigger an unauthorized ACH debit. Controllers managing bank payment security across a multi-bank portfolio cannot assume that protecting one channel protects the other.


The operational reality makes this harder:


- Each bank may offer different positive pay configurations, requiring separate setup and file formats per relationship


- Some regional banks offer check positive pay but have limited or no ACH positive pay functionality


- Enrollment and activation timelines vary, meaning one account may be protected months before another


We often see portfolios where 70% to 90% of accounts have check positive pay active but fewer than half have ACH controls in place, simply because the enrollment process was never completed across all banks.


## Managing Both Controls at Scale


Enabling check positive pay and ACH positive pay across every account at every bank is the right goal. The challenge is operational. Each bank requires its own issue file format, its own exception review workflow, and its own enrollment process for ACH rules. Treasury managers at large portfolios end up managing positive pay as a per-bank, per-account administrative task rather than a centralized fraud program. A platform like Arpari helps by centralizing payment data and bank connectivity, making it easier to generate issue files, monitor exception activity, and maintain visibility across all accounts regardless of banking partner.


## Key Takeaways


Check positive pay and ACH positive pay protect different payment channels and are not interchangeable. Enabling one without the other leaves a clear exposure that grows with every account in the portfolio. Controllers and treasury managers should audit both controls across every bank relationship, not just the primary ones. For multi-bank real estate portfolios, the challenge is not understanding the need but completing enrollment and maintaining consistent coverage at scale. The safest assumption is that any account without both controls active is an account with an open door. Close both or accept the risk of leaving one unlocked.


## See it in action


Welcome to the next level of clarity from Arpari. Want to try it live? Book a 30-minute demo at[www.arpari.com/demo](https://www.arpari.com/demo) to see how Arpari helps you manage positive pay coverage across every account and banking partner from one place.


*Arpari is the modern treasury platform for real estate owners, operators, and finance teams. We aggregate bank data, automate cash reporting, and now let you move money securely, across every bank, in one workspace.*
