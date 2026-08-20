---
schema_version: "1.0.0"
document_id: "de6ca70f49387b38c36ff13179b5dd97b3776dfa3cbd449b47b7a78a179fb954"
company_key: "yc-finta"
company: "Finta"
source_id: "yc-finta-news-import-8be56c12e8e6"
canonical_url: "https://www.finta.io/changelog/match-transactions-with-regex-patterns"
published_at: "2026-01-09T12:00:00+00:00"
first_seen_at: "2026-07-25T04:54:26.845641+00:00"
fetched_at: "2026-07-28T21:58:18.576112+00:00"
content_hash: "sha256:cfe00414f2329a798c6ea5380f4f97148a5dcb7c6eae13e4669d9007bdd83ea2"
---

# Match Transactions with Regex Patterns

## Powerful Pattern Matching for Your Rules


You can now use regular expressions (regex) in your Finta rules, giving you the flexibility to match complex patterns with a single rule instead of creating multiple rules for similar transactions.


## What's New


### New Rule Operators


Two new operators are available when creating rules:


- **matches regex** - Match transactions where the field value matches your pattern
- **does not match regex** - Match transactions where the field value does NOT match your pattern


These operators work with the Summary and Original Description fields.


### Why This Matters


Previously, if you wanted to categorize all your coffee purchases, you'd need separate rules for "Starbucks", "Coffee Bean", "Peet's Coffee", and every other coffee shop. Now you can create a single rule:


**Summary** matches regex` (Starbucks|Coffee Bean|Peet's|Dunkin)`


This one rule will match transactions from any of those merchants, keeping your rules list clean and manageable.


### More Examples


- Match any subscription:` (Netflix|Spotify|Hulu|Disney)`
- Match reference numbers:` REF-\\d{6}`
- Match date patterns:` \\d{2}/\\d{2}/\\d{4}`
- Match payment with optional reference:` payment(?:\\s*#\\d+)?`
- Match transaction IDs:` \[A-Z\]{2,3}-\\d{4,}`


All pattern matching is case-insensitive, so` starbucks` will match "STARBUCKS" and "Starbucks".


## Get Started


Regex pattern matching is available now. When creating or editing a rule, select "matches regex" or "does not match regex" from the operator dropdown to start using pattern matching in your rules.


[Control Pending Transactions in Your Coda Pack January 25, 2026](https://www.finta.io/changelog/control-pending-transactions-in-your-coda-pack)[Track Your Balance History Over Time November 29, 2025](https://www.finta.io/changelog/track-your-balance-history-over-time)
