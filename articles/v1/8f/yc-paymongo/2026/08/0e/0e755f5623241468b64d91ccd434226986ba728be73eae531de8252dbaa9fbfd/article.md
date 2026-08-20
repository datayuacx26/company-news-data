---
schema_version: "1.0.0"
document_id: "0e755f5623241468b64d91ccd434226986ba728be73eae531de8252dbaa9fbfd"
company_key: "yc-paymongo"
company: "PayMongo"
source_id: "yc-paymongo-news-import-f1c07cbe72e4"
canonical_url: "https://www.paymongo.com/blog/how-to-get-qr-ph-code"
published_at: "2026-08-07T05:42:29+00:00"
first_seen_at: "2026-08-07T16:38:56.846155+00:00"
fetched_at: "2026-08-07T16:38:58.983547+00:00"
content_hash: "sha256:8de178ff33de2594389e81bf90827e4bc4f3d0f4cbb590682cf63a53cc52b62f"
---

# How to Get a QR Ph Code for Your Business in 2026 (Step-by-Step)

The first thing to know about how to get a QR Ph code is where it comes from. Not the Bangko Sentral ng Pilipinas. BSP wrote the standard; it does not issue codes. Your code comes from a participating bank, e-money issuer, or payment service provider, and that choice sets three things you will live with: your transaction fee, your settlement speed, and whether the same account can also take payments online.


This guide covers the requirements, the static-versus-dynamic decision, activation across each sales channel, real published fees, and the July 2026 rule changes. Configuration takes about 10 minutes once your account is verified.


## **What QR Ph is**


QR Ph is the Philippine national QR code standard, adopted by BSP under[Circular No. 1055](https://www.bsp.gov.ph/Regulations/Issuances/2023/M-2023-005.pdf?ref=paymongo-blog.ghost.io) in October 2019. Payments settle over InstaPay.


The standard exists to solve interoperability. Before QR Ph, a GCash QR worked only for GCash and a Maya QR only for Maya, so a single counter often carried four or five codes. BSP Memorandum M-2023-005 closed that off: from 1 July 2023, proprietary payment QR codes had to be disabled and every payment service provider moved to the QR Ph standard.


One code now covers every participating bank and wallet. For the full background on the standard, see[What is QRPH? Everything Filipino Merchants Need to Know](https://www.paymongo.com/blog/what-is-qrph?ref=paymongo-blog.ghost.io) .


## **What you need before you start**


Requirements vary by provider. Nearly all ask for:


- **Business registration** — DTI certificate for a sole proprietorship, SEC registration for a corporation or partnership
- **A valid government ID** for the owner or authorised signatory
- **A mobile number** that can receive SMS, for payment confirmations
- **Bank account or e-wallet details** for payouts
- **Mayor's or barangay business permit** — requested by some providers, most often banks


If the business is not registered yet, start there. Our guide on[how to register an online business in the Philippines](https://www.paymongo.com/blog/how-to-register-online-business-philippines?ref=paymongo-blog.ghost.io) covers the DTI, BIR, and LGU steps.


Not yet registered?


Some providers onboard sole proprietors and small sellers on lighter documentation. Ask before assuming you are disqualified, but expect a lower transaction limit until registration is complete.


[Get QR Ph Code with Minimal Requirements](https://dashboard.paymongo.com/signup?ref=paymongo-blog.ghost.io)


## **How to Generate QR Ph**


### **Step 1: Decide between a static and dynamic code**


This is the decision that matters most, and the one most guides skip. Getting it wrong means redoing the setup.


Static QR Ph


Dynamic QR Ph


Best for


Physical stores, market stalls, service providers, deliveries


Online checkout, invoices, payment links


Amount


Customer types it in


Pre-encoded in the code


Reusable


Yes, one code, unlimited transactions


No, generated per transaction


Expiry


Does not expire


Expires if unpaid


Format


Printed standee or sticker


Displayed on screen at checkout


Risk


Customer can mistype the amount


None; the amount is locked


Physical businesses generally want a static code. Print it once, mount it at the counter, done.


Online businesses generally want dynamic codes. They generate at checkout with the correct amount already encoded, which removes underpayments entirely.


Running both is normal: a standee at the counter, dynamic codes on the site. There is no reason to choose only one.


### **Step 2: Choose your QR Ph provider**


Providers fall into three categories.


1. **Payment service providers** — PayMongo, HitPay and others – issue QR Ph alongside cards, e-wallets, and online banking under one account and one dashboard. The right fit if you sell both in-store and online, or expect to.


[Generate a QR Ph Code in Minutes](https://dashboard.paymongo.com/signup?ref=paymongo-blog.ghost.io)


1. **Banks** — BDO, BPI, Metrobank, UnionBank, RCBC and other banks issue QR Ph codes to business account holders. Sensible if you already bank with them and sell mainly in person. Onboarding is slower and more document-heavy, and online payment tooling tends to be limited.
2. **E-wallets** — GCash and Maya offer merchant QR through their business products. Fast to set up and familiar to customers.


Compare on four things, in this order:


1. **Transaction fee (MDR)** — the percentage taken per payment
2. **Settlement speed** — how fast money reaches your account
3. **Static and dynamic support** — some providers only do one
4. **Other payment methods included** — cards and e-wallets on the same account, or a separate integration for each


### **Step 3: Sign up and get verified**


Create an account with your chosen provider and submit your documents.


With[PayMongo](https://www.paymongo.com/products/accept-payments/qr-ph?ref=paymongo-blog.ghost.io) , signing up is free: no setup fee, no monthly fee. Upload your business registration and ID, and verification is the gate on activation. Have clear photographs of your documents ready. Blurry uploads are the single most common cause of delay.


[Sign Up](https://dashboard.paymongo.com/signup?ref=paymongo-blog.ghost.io)


### **Step 4: Activate QR Ph on your account**


How you switch it depends on how you sell.


1. If you sell in person: Order a QR Ph standee for the counter and pair the code with a mobile number. Every completed payment triggers an SMS, so your cashier can confirm the money arrived without opening a dashboard or relying on the customer's screen. No POS terminal, no card reader, no hardware rental.
2. If you use payment links or hosted pages: Nothing to configure. On PayMongo, QR Ph appears automatically as an option on Links and Pages, and existing Pages are already set up to receive it.
3. If you run a Shopify store:


1. Open your Shopify admin and go to Settings
2. Select Payments, then Secure Payments via PayMongo
3. Click Manage and toggle QR Ph on
4. Save your payment methods


4. If you are integrating via API: Add qrph to payment_method_allowed when you create a Payment Intent, create a payment method of type qrph with the required billing details, then attach it. The response returns a Base64 QR image at next_action.code.image_url for you to render. Listen for three webhooks:


The minimum transaction is PHP 1.00. PayMongo sets no upper ceiling, though the customer's own bank or wallet limits still apply. Full details are in the[QR Ph developer documentation](https://docs.paymongo.com/docs/payment-acceptance-qr-ph?ref=paymongo-blog.ghost.io) .


### **Step 5: Test before you go live**


Send yourself a small payment – PHP 20 is enough – from a wallet or bank app you own, and confirm four things:


- The code scans on the first try, in the lighting where it will actually sit
- The confirmation SMS arrives, and reaches the right phone
- The transaction shows in your dashboard at the right amount
- For dynamic codes, the pre-filled amount is correct


Refund or withdraw the test payment and you are live.


### **Step 6: Train whoever handles the counter**


BSP requires payment service providers to train client-merchants, including cashiers and managers, under M-2023-005. In practice the training that protects your money is the training you run. Cover three rules:


- **Confirm before releasing goods.** Wait for the SMS or the dashboard entry. A screenshot on a customer's phone is not proof of payment; screenshots are trivially faked.
- **Watch for tampered codes.** "Quishing" is when someone sticks their own QR over yours and collects your sales. Check the standee at open and close, and laminate or frame it.
- **Know the amount rule.** On a static code the customer enters the amount. Cashiers should read it back before confirming.


## **What QR Ph costs**


Fees are a percentage of each transaction, with no fixed peso component on QR Ph. PayMongo's published rates, for reference:


Payment method


Fee


QR Ph (in-store and online)


1.34%


Maya


1.79%


GCash


2.23%


Domestic Visa / Mastercard


3.125% + PHP 13.39


Online banking (BDO, BPI, UnionBank, Metrobank, Landbank)


0.71% or PHP 13.39


On a PHP 1,000 sale, QR Ph costs PHP 13.40 against PHP 44.64 on a domestic card — roughly a third of the card fee. That gap is the argument for putting a QR standee *next to* your terminal rather than replacing it: steer walk-in customers to the cheaper rail, keep cards for those who insist.


Standard settlement runs on a 2–7 day cycle, with instant settlement available for 1.5% per transaction upon request. Payouts to your bank or wallet cost PHP 10 per transaction via InstaPay or PesoNet. Current rates are always on the[pricing page](https://www.paymongo.com/pricing?ref=paymongo-blog.ghost.io) .


## **What changed in 2026**


On 29 July 2026, BSP and the Philippine Payments Management Inc. announced four changes. Two affect merchants directly.


**QR Ph and InstaPay QR are now separate codes.** QR Ph handles person-to-merchant payments; InstaPay QR handles person-to-person transfers. If you have been collecting business payments through a personal P2P code, this is the point to move to a proper merchant QR Ph code. You get transaction records that reconcile, and business funds stop mixing with personal ones.


**InstaPay for Business raised the transfer ceiling tenfold** , from PHP 50,000 to PHP 500,000 for registered businesses. Relevant if you have been splitting larger payments across multiple transfers.


The other two – Direct Debit PH for scheduled biller collections, and InstaPay Cash-In for requesting funds – matter more for subscription and billing models than for counter sales.


## **Common problems, and how to fix them**


**The code will not scan.** Usually print quality or glare. Reprint at higher resolution and move it out of direct light or from under glass.


**Payment succeeded but no SMS.** Check that the paired number is correct and has cellular signal. Verify against the dashboard, which is the source of truth – the SMS is a convenience, not the record.


**Customer paid the wrong amount on a static code.** Refund and re-take it. If it happens often, switch that counter to dynamic codes so the amount is locked.


**Dynamic code expired before payment.** The customer took too long. Generate a fresh one; nothing is lost.


**Money not in the bank yet.** Check whether you are on standard settlement, which takes 2–7 days, then whether a payout has actually been initiated. Funds sit in your wallet until you move them.


## **Digital payments do not change your receipt obligations**


If your business is BIR-registered, you still need to issue a registered sales invoice or official receipt for the sale. The QR Ph transaction record is not a substitute for one.


Keep your payment dashboard exports. They make reconciliation at filing time considerably less painful than counting cash drawers. If you are unclear which document you are supposed to issue, settle that question early rather than at year-end.


****Start accepting QR Ph payments****
PayMongo gives you QR Ph plus cards, e-wallets, and online banking through a single account and dashboard. No setup fee, no monthly fee, and you only pay when you get paid.


[Create a Free account](https://dashboard.paymongo.com/signup?ref=paymongo-blog.ghost.io)


---


## **Frequently asked questions**


#### How long does it take to get a QR Ph code?


Configuration takes about 10 minutes. The wait is document verification, which ranges from same-day with a payment service provider to several business days with a bank. Having your DTI or SEC registration and a valid ID ready is what determines your actual timeline.


#### Can I get a QR Ph code without a registered business?


Some providers onboard unregistered sole proprietors and small sellers on lighter documentation, usually at a lower transaction limit. BSP-supervised institutions have KYC obligations, so full limits and features require registration. Registering also separates business and personal money, which matters once you are dealing with any volume.


#### Can one QR Ph code accept payments from any bank or e-wallet?


Yes — that is the point of the standard. A single QR Ph code accepts payment from any participating bank or e-money issuer, more than 30 institutions including GCash, Maya, BPI, BDO, Metrobank, UnionBank, GoTyme, ShopeePay, and GrabPay. Separate codes per provider are no longer needed.


#### What's the difference between QR Ph and InstaPay QR?


As of July 2026, BSP separated the two. QR Ph is for person-to-merchant business payments; InstaPay QR is for person-to-person transfers. Businesses should use QR Ph, which produces proper merchant transaction records and keeps business funds separate from personal ones.


#### Do I need a POS terminal or card reader for QR Ph?


No. QR Ph needs only a printed code and a phone to receive confirmations. No hardware purchase, no rental, no monthly terminal fee — which is why the effective cost sits well below card acceptance.


---


***Read more:***


- [What is QR Ph? Everything Filipino Merchants Need to Know](https://www.paymongo.com/blog/what-is-qrph?ref=paymongo-blog.ghost.io)
- [BSP Updates: What’s New for Philippine Merchants](https://www.paymongo.com/blog/bsp-updates-2026?ref=paymongo-blog.ghost.io)
- [How to Accept Payments Online? Payment Links and Payment Gateway Guide](https://www.paymongo.com/blog/accept-payments-online-philippines?ref=paymongo-blog.ghost.io)
