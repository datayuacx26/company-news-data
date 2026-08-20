---
schema_version: "1.0.0"
document_id: "a840a7fbbd699ccb0ee31a32bb4f99971e67ecc99b3800a4e9ebf0870f1b2a57"
company_key: "shopify-inc-class-a-subordinate-voting-shares"
company: "Shopify Inc."
source_id: "shopify-inc-class-a-subordinate-voting-shares-news-import-b162817d780b"
canonical_url: "https://www.shopify.com/blog/shopify-retail-roundup-july-2026"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T01:53:31.660286+00:00"
fetched_at: "2026-08-06T01:53:33.927187+00:00"
content_hash: "sha256:128b31069a799269ba3d9e88f4fa383d949e73d4c3d529cf1d5f481f3ed468e6"
---

# Shopify Retail Roundup (July 2026 - v11.10 & 11.11)

Shopify is always evolving, with new features and updates rolling out regularly to help retailers stay ahead—but keeping up shouldn't be a chore. That's why this hub tracks every POS update and relevant Shopify changes in one place.


Every two weeks, Shopify releases an updated POS version and this post is refreshed to highlight what's new. If you have multiple retail locations, it's best practice to use an[MDM (Mobile Device Management)](https://help.shopify.com/en/manual/sell-in-person/getting-started/mobile-device-management/index) to manage app versions, and to test each new release on a few devices before rolling it out to all stores. To see previous months' updates, scroll to the bottom of this post. ****


**Scroll to see the July 2026 updates below, or click the[Retail Release Roundup](https://www.shopify.com/blog/retail-release-roundup) post for the latest news and most recent updates!**


**Updates in POS version 11.11**


- Access recent carts
- Nearby device authentication
- Configure cash rounding by market
- Print directly to receipt printers by POS UI extensions
- POS device two-step re-authentication


**Updates in POS version 11.10**


- Faster cash payments
- Faster split payment entry
- Faster email entry
- Receipt choices are streamlined
- Customize all POS receipts in one editor
- Know at a glance whether you're ready to sell


## Updates in POS version 11.11 (July 27, 2026 release date)


### **Build, share, and hand off carts**


****


**What changed:** Cart sharing lets any staff member build, access, and complete a cart from any device within a location. Carts save automatically, no manual save required.


**Why it matters:** Eliminates lost carts during shift changes and device switches, and lets merchants serve multiple customers at once. This enables true floor selling: associates build carts with customers and checkout from anywhere in the store without starting over.


Access: Enable the Managed recent carts permission in Shopify Admin. Tap the Cart to see a list of all active carts within a location. Tap and hold a cart to rename it. Carts auto-clear after 24 hours by default. Manage the auto-clear time frame in POS channel


[Learn more about recent cart](https://help.shopify.com/en/manual/sell-in-person/shopify-pos/order-management/manage-recent-carts)


### **Add a new device quickly by approving it with a nearby device**


****


**What changed:** You can now sign in a new POS device into your shop by approving it from a device that's already logged in. The logged-in device detects the new one over Bluetooth or by QR code, a manager approves it, and you're logged in.


**Why it matters:** Today, adding a device can be slow, especially in moments when you really need it like opening a register on a Saturday, replacing a device mid-shift, or during a busy event. These are moments where you can't afford any friction. Now with just a few taps, a nearby device you already trust can quickly approve a login.


**Access:** Bluetooth approval: The new device announces itself over Bluetooth. A logged-in POS device nearby picks up up, a staff member with the POS Device Setup permission taps approve and enters their PIN, and the new device joins your shop and location.


QR code when Bluetooth is off: Scan a code show on the new device with a logged-on device to activate it.


Session duration at approval: The approver picks how long the new device stays logged in. Email and password login are unchanged and still available.


**[Learn more about new device authentication](https://help.shopify.com/en/manual/sell-in-person/getting-started/securing-your-pos-setup#nearby-device-login)**


### **Configure cash rounding by market**


****


What changed: Cash rounding in Shopify POS is becoming more configurable across different markets. You can set country/currency-level rounding rules and create custom rules for specific retail locations, giving you more control over how cash totals are rounded at checkout. Settings are managed centrally in POS settings and synced when selling offline.


**Why it matters:** This helps merchants that accept cash configure their settings by market to round totals differently depending on local coin availability, country rules, or regional expectations.


**[Learn more about cash rounding settings](https://help.shopify.com/en/manual/sell-in-person/shopify-pos/cash-rounding-on-pos)**


### **Print directly to receipt printers from POS UI extensions**


**What changed:** POS UI extensions using API version 2026-07 can now discover paired hardware receipt printers and print to them directly with the new Printing API (shopify.printing). Developers can use getPrinters() to find available printers and print() to send HTML or image-based documents straight to a connected receipt printer without opening the system print dialog.


**Why it matters:** This removes an extra step in extension-based printing workflows, which helps staff move faster during in-store moments like order pickup, custom receipts, or back-of-house print tasks.


**[Learn more about the Printing API](https://shopify.dev/changelog/pos-ui-extensions-can-now-print-directly-to-hardware-receipt-printers)**


### **Some POS devices may require two-step authentication**


****


**What changed:** If you have two-step authentication enabled for all or some POS users of your store, some of your POS devices may need to be re-authenticated to match that security setting. If any of your devices are impacted, they will show a banner when staff PIN in. On each affected device, a staff member with one of the following roles needs to log out and log back in once, using two-step authentication with their email and password: store owner, POS device setup, POS administrator, organization owner, or organization POS administrator.


**Why it matters:** Taking this step ensures that all of your devices match your latest security settings.


**Access:** If you have affected devices, they must be re-authenticated prior to January 4th, 2027.


**[Learn more about two-step authentication](https://help.shopify.com/en/manual/sell-in-person/getting-started/securing-your-pos-setup#staff-permissions)**


## Updates in POS version 11.10 (July 6, 2026 release date)


### Faster cash payments


**What changed:** Cash payments in Shopify POS are now quicker to complete. Suggested cash amounts appear first for one-tap payment, custom amount entry is still available when needed, and change due now appears in the checkout flow instead of on a separate screen.


**Why it matters:** This helps staff move through common cash payments with fewer taps and less interruption. For example, if a customer hands over a common bill amount, staff can select the suggested amount and continue through checkout faster.


**Access:** Automatically available in POS v11.10. No action required; all staff will have access. This feature is rolling out gradually, so you may not see it right away.


[Learn more about cash payments](https://help.shopify.com/en/manual/sell-in-person/shopify-pos/payment-management/cash-payments)


### Faster split payment entry


**What changed:** Split payments in Shopify POS are now easier for staff to follow. Staff choose the payment type first, then enter the amount for that payment on a separate screen.


**Why it matters:** This helps when customers pay with more than one payment method, like cash and card or multiple cards. Each step is more focused, so staff can see what they’re doing and move through multi-payment checkouts with more confidence.


**Access:** Automatically available in POS v11.10. No action required; all staff will have access.


[Learn more about split payments](https://help.shopify.com/en/manual/sell-in-person/shopify-pos/payment-management/multiple-partial-payments)


### Faster email entry


**What changed:** Supported email fields in Shopify POS can now suggest common domains, like @gmail.com, while staff or customers enter an email address. This helps complete customer emails with fewer taps.


**Why it matters:** Email domain shortcuts are useful when staff are adding customer details, sending digital receipts, or entering contact information during checkout. They help reduce typing and make it easier to capture accurate customer emails.


[Learn more about managing customer details](https://help.shopify.com/en/manual/sell-in-person/shopify-pos/customer-management)


### Receipt choices are streamlined


**What changed:** We've made two improvements to the receipt delivery process:


-


When customer details are added before payment, if your checkout does not have a customer facing display connected, staff can now skip turning the POS tablet around for the customer-facing receipt step and finish checkout right away.


-


When a customer selects their receipt preference on a connected customer-facing display, that choice now shows up on the staff POS device so staff can see what's already been selected instead of asking again.


**Why it matters:** Every extra tap and screen-turn adds up during a busy rush. These changes cut out redundant steps so lines move faster, staff spend less time managing the receipt handoff, and customers aren't asked the same question twice. The result is a smoother, quicker finish to every sale.


[Learn more about receipt management](https://help.shopify.com/en/manual/sell-in-person/shopify-pos/receipt-management/customer-receipt-selection)


### Customize all POS receipts in one editor


**What changed:** Shopify POS now uses one unified printed receipt template across sale, return, exchange, gift, and gift card receipts. Merchants can manage receipt content in the POS channel content editor with bounded customization for header and footer content, smart defaults for branding and business identifiers, and updated gift receipts that remove line items. The new template also adds built-in fields like VAT/GST/PST ID, SIRET number for France, return policy, signature line, and custom text or Liquid blocks without requiring full-template Liquid edits.


**Why it matters:** Less per-location maintenance, more consistent receipts, and lower risk of missing a regional requirement during a sale or return. Common changes that used to need Liquid (return policies, signature capture, custom text) are now native fields with a real-time preview.


**Access:** We're rolling this out in phased waves across the POS channel between now and mid August. It will be available to all Shopify POS merchants except those located in Italy. Merchants in Italy rely on the Epson RT printer to fiscalize transactions today. All receipt content and presentation is handled via the Epson RT printer and cannot be altered within Shopify.


Merchants with simple customizations will be migrated automatically, while merchants who had customized their receipts with the Liquid editor have until January 1, 2027 to transition before Liquid editing is deprecated then.


Note: Devices must be operating POS v11.8 or greater to print the new receipt template and on v11.10 or greater to print the new fields. It’s recommended shops with devices under v11.8 upgrade devices to ensure a consistent receipt experience within your store.


### Know at a glance whether you're ready to sell


**What changed:** POS now shows the health of your selling environment in one place. An updated connectivity screen surfaces your internet connection, Shopify service status, and hardware connectivity health, and tells you what to do when something's degraded. ****


****


**Why it matters:** No more guessing whether your store is ready to sell. Instead of discovering a problem when a sale fails, you see connection and hardware status up front, get next-best actions like enabling offline checkout, and keep serving customers through a disruption.


**Access:** Automatically available in POS v11.10. Access the Connectivity screen by hitting the associated icon on the bottom left of the Shopify POS app.


****


Learn more about the


[Connectivity screen in POS](https://help.shopify.com/en/manual/sell-in-person/shopify-pos/troubleshooting#connectivity)
