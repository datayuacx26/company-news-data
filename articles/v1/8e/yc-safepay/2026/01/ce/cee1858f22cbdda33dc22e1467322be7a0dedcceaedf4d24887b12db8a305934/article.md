---
schema_version: "1.0.0"
document_id: "cee1858f22cbdda33dc22e1467322be7a0dedcceaedf4d24887b12db8a305934"
company_key: "yc-safepay"
company: "Safepay"
source_id: "yc-safepay-news-import-bece91fff0a9"
canonical_url: "https://getsafepay.pk/blog/content/quicklinks-2-0"
published_at: "2026-01-26T00:00:00+00:00"
first_seen_at: "2026-07-22T12:31:50.940576+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:ba762a5b46b34bf59ecb2192ca76dde273c84991e7f69c1adaf595d947cd52e5"
---

# Quicklinks 2.0: A brand-first, accessible checkout experience

[All posts](https://getsafepay.pk/blog/)


Blog


# Quicklinks 2.0: A brand-first, accessible checkout experience


Quicklinks 2.0 brings a refreshed checkout with merchant-first branding, clearer hierarchy, and accessibility improvements powered by a more maintainable component system.


Hamza Rizvi


@hamzarizvi


# Quicklinks 2.0: A brand-first, accessible checkout experience


Quicklinks are the fastest way for customers to pay an invoice or a payment link. We recently overhauled the experience from top to bottom - design, accessibility, and code architecture - while keeping the flow familiar.


TL;DR: the new Quicklinks feel modern, merchant-branded, and clearer at every step, with a more maintainable component system under the hood.


## Where we started


Old Quicklinks were clean and functional, but they were also tightly tied to a single Safepay visual language. The layout worked, yet it left little room for merchant identity, stronger information hierarchy, and improved accessibility.


## The new Quicklinks experience


The redesign shifts to a split layout that separates the summary from the payment form. This creates a stronger visual rhythm, clearer hierarchy, and more room for inputs without overwhelming the user.


Key upgrades:


- Clearer hierarchy for amount, merchant, and description


- Cleaner form grouping and spacing for readability


- Better focus, contrast, and error affordances for accessibility


- Mobile-first layout that still looks premium on desktop


- Merchant-first branding that adapts to each logo and palette


- Deadline timers that surface urgency for expiring invoices or links


## Merchant-first branding


Every merchant now gets a tailored brand presence. The page identity adapts to the merchant logo and key colors, while Safepay trust signals remain consistent. The result is a payment page that feels like the merchant and still reads as Safepay-powered.


## Deadlines that are impossible to miss


If an invoice or payment link has an expiry, the new experience surfaces a live countdown timer. Customers get clarity on urgency, which reduces confusion and last-minute drop-offs.


## Built with Safepay Atoms


This revamp uses[Safepay Atoms](https://www.npmjs.com/package/@sfpy/atoms) and its exported components throughout the Quicklinks UI. This is the very first instance where Safepay Atoms is made to use, and it is in a production deployment.


## Smoother member invite and password reset flows


We also refined the invite and password reset journeys to reduce friction, speed up completion, and keep the experience consistent with the new Quicklinks UI.


## The result


Quicklinks 2.0 is a brighter, faster, and more trustworthy checkout that adapts to every merchant. It is quicker to complete and simpler for us to maintain.


If you are a merchant using Safepay Quicklinks, you now get a payment page that looks and feels like your brand, without losing the reliability of the platform.
