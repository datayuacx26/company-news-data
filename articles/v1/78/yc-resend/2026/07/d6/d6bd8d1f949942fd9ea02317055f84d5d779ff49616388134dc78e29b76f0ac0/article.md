---
schema_version: "1.0.0"
document_id: "d6bd8d1f949942fd9ea02317055f84d5d779ff49616388134dc78e29b76f0ac0"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/the-new-dmarc-is-here"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:2de869303bb944dffa672f02fc0736e122d991c312e84235d490013d63de6342"
---

# The New DMARC is Here

After more than a decade of real-world use and several years of drafting, DMARC has a new specification. The effort known as "DMARCbis" is now finalized and published as three separate standards:


1. [RFC 9989](https://www.rfc-editor.org/rfc/rfc9989.html) : the core DMARC protocol
2. [RFC 9990](https://www.rfc-editor.org/rfc/rfc9990.html) : aggregate reporting
3. [RFC 9991](https://www.rfc-editor.org/rfc/rfc9991.html) : failure reporting


## What do you need to change?


*The short answer?* You likely **don't need to change anything today** , as existing records keep working exactly as before. You can, however,benefit from the new specification if you use the new features.


The updates mostly change how *receivers* evaluate your domain and clean up a few tags that were confusing or rarely honored.


## Why the new standard matters


The original DMARC ([RFC 7489](https://www.rfc-editor.org/rfc/rfc7489.html) ) was an *Informational* document, a description of something the industry had already agreed to do. The new RFCs are *Standards Track* , which reflects how central DMARC has become to email authentication.


Of the changes, there are at least three that you should know about.


## 1. New lookup pattern


The most significant update is how a receiving server figures out your **organizational domain** . Previously, DMARC relied on the[Public Suffix List (PSL)](https://publicsuffix.org/learn/) , a community-maintained mapping file. Every validator had to download and trust that list to know where a domain truly begins.


The new DMARC replaces this with an algorithmic **DNS Tree Walk** . A receiver walks up the DNS hierarchy from the` From:` address, querying for` _dmarc` records at each level until it finds the relevant policy.[The algorithm caps the work](https://www.rfc-editor.org/rfc/rfc9989.html) to protect against infinite lookups.


Results are now determined by DNS itself, which is more predictable and lets domain owners declare their own boundaries using the new` psd` tag.


**You don't need to take any action** , but your DNS provider may now define these boundaries.


## 2. DMARC tag updates


Here's a quick reference for which tags are new, gone, or changed.


Tag Status What it means


pct Removed Percentage-based enforcement is gone


t New Testing flag—replaces pct


rf Removed Report format; reports are XML only


ri Removed Report interval no longer configurable


psd New Declares whether a name is a Public Suffix Domain


np New (now standard) Policy for non-existent subdomains


sp Clarified Ignored on subdomain records; only meaningful on the organizational domain


Let's look at a few of these changes more closely.


### pct becomes t


In theory,` pct` let you apply your policy to a percentage of mail (` pct=50` meant "quarantine half of the failures"). In practice, the tag was almost entirely ignored. A value like` pct=100` was already the default behavior, so removing it changes nothing for most senders.


The new DMARC replaces it with a simpler boolean` t` (testing) tag:


- ` t=n` (default): Your stated policy is applied in full, the same as` pct=100` .
- ` t=y` : Receivers treat mail as if you're only monitoring, even if your` p` is` quarantine` or` reject` . This does the same job as the old` pct=0` .


**If you relied on` pct`**


If you were using a fractional` pct` to phase in enforcement, sit at` p=none` while you review reports, then move straight to` quarantine` or` reject` once your sources are aligned.


### np for non-existent subdomains


The` np` tag sets a policy specifically for **subdomains that don't exist** (i.e., names with no DNS records at all). Its syntax mirrors the` p` tag, suggesting to mail servers one of the following:


- ` none` : no recommended action
- ` quarantine` : place mail in spam
- ` reject` : reject mail (hard bounce)


The` np` tag closes a real spoofing gap. Attackers love to forge mail from plausible subdomains like` billing.example.com` that you never provisioned. With` np=reject` , you can tell receivers to reject mail from any non-existent subdomain without changing a policy for your own subdomains.


```text
example  .  com    TXT      "v=DMARC1; p=quarantine; np=reject;"
```


For the practical mechanics of organizational-domain discovery and how policy cascades down to subdomains, our companion guide walks through it in detail.


[How DMARC applies to subdomains Understanding how DMARC applies to subdomains is crucial for managing your email domain. resend.com/blog/how-dmarc-applies-to-subdomains](https://resend.com/blog/how-dmarc-applies-to-subdomains)


### psd for public suffix operators


The` psd` tag is the counterpart to thenew lookup pattern . It lets a name explicitly declare its role in the hierarchy:


- ` psd=y` — this name *is* a public suffix (used by registries and TLD operators).
- ` psd=n` — this name is an organizational domain, even if the Tree Walk might otherwise keep climbing.


Most senders will never set` psd` . It exists so that the parties who operate suffixes can participate in DMARC directly, instead of relying on their entries in an external list.


## 3. Reporting is now its own standard


Aggregate and failure reporting have been split out of the core protocol into[RFC 9990](https://www.rfc-editor.org/rfc/rfc9990.html) and[RFC 9991](https://www.rfc-editor.org/rfc/rfc9991.html) . Two knobs disappeared as part of the cleanup:


- **` rf` (report format)** is removed. Aggregate reports are always XML.
- **` ri` (report interval)** is removed, and receivers now send on their own schedule.


Your` rua` and` ruf` reporting addresses work exactly as before. If you're not yet reading those reports, they remain the single best way to see who is sending on your behalf before you tighten enforcement.


[How to read a DMARC report Learn how to decode the XML aggregate reports mailbox providers send back to you. resend.com/blog/how-to-read-a-dmarc-report](https://resend.com/blog/how-to-read-a-dmarc-report)


## What this means for you


While you likely don't *need* to make changes, the new specification makes it easier to enforce policy modes without breaking existing configurations.


Here's the short checklist of actions to take:


1. **Drop any` pct` tag** if you have one. If it was` pct=100` , behavior is unchanged. If it was fractional, move to a proper` none → quarantine → reject` rollout instead.
2. **Consider adding` np=reject`** to shut down spoofing of subdomains you never created.
3. **Keep reading your reports.** Reports remain key to understanding mail servers' behavior and identifying potential issues.
4. **Don't rush to` reject` .** Monitor first, confirm every legitimate source is authenticated and aligned, and then enforce.


For help understanding DMARC policy modes and what to do when enforcing them, start here:


[DMARC Policy Modes A practical guide to none, quarantine, and reject—and what to expect when enforcing them. resend.com/blog/dmarc-policy-modes](https://resend.com/blog/dmarc-policy-modes)


## Wrap-up


The new DMARC is the same protocol, formalized as an internet standard and sharpened in the places that caused the most confusion:


- DNS-native way to find organizational domains
- A dedicated policy for non-existent subdomains
- Cleaner enforcement signals


For more help, view our[DMARC docs](https://resend.com/docs/dashboard/domains/dmarc) or[reach out to our support team](https://resend.com/support) .
