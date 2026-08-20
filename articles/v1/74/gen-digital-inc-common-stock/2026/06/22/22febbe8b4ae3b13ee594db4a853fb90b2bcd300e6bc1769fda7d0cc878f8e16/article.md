---
schema_version: "1.0.0"
document_id: "22febbe8b4ae3b13ee594db4a853fb90b2bcd300e6bc1769fda7d0cc878f8e16"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-rss-b2c253ea0afd"
canonical_url: "https://www.gendigital.com/blog/insights/research/fake-invoices-shopping-apps"
published_at: "2026-06-24T16:54:21+00:00"
first_seen_at: "2026-07-20T03:32:39.128259+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:2edf606e03f229bd939492e1f5351e0b38360493b6a69228211601a2e740d91d"
---

# Fake invoices are moving from inboxes to shopping apps

By[Luis Corrons](https://www.gendigital.com/blog/authors/luis-corrons)


,[Jakub Vavra](https://www.gendigital.com/blog/authors/jakub-vavra)


A fake invoice in your email is easy to ignore. A fake invoice inside your order history feels different.


Norton customers have reported fake Norton invoices appearing inside the Shop app, the shopping and order-tracking app from Shopify. Public reports suggest the same technique is not limited to Norton. Similar suspicious Shop app notifications have used McAfee, Apple gift cards, iPhones, PayPal-style payment claims and other high-value purchases as bait. The impersonated brand may change, but the mechanics are familiar: make the user believe they have been charged, then give them a phone number to call.


That phone number is where the scam really begins.


#### Why the Shop app makes this more believable


Shop is a legitimate app. People use it to track packages, check receipts and receive order updates. According to Shop’s own documentation, the app can automatically track orders from several sources, including Gmail, Outlook, the email address linked to the user’s Shop account and orders paid through Shop Pay. If Gmail or Outlook is connected, Shop says it scans recent emails for keywords such as “tracking number” and “track your package” to populate deliveries in the Orders tab.


Shop also supports push notifications for delivery updates, payments and favorite products. So the first alert the victim sees may not be an email, an SMS or a calendar invite. It may be a notification from an app they already use for real purchases.


That context matters. The scam text is not especially polished. Some of the examples we reviewed contain awkward wording and obvious signs of fraud. But when the message appears inside an order-tracking app, surrounded by real receipts and shipping updates, users may give it more credibility than it deserves.


The screenshots reported to us show fake orders under generic seller names such as “My Store.” The receipts claim that a Norton or Norton LifeLock subscription has been activated or renewed, usually for several hundred dollars. The phone number appears in the product description, the receipt body or even the shipping address field.


That is not how a normal receipt should look.


#### The brand is interchangeable


Norton is the example we saw through customer reports. Norton support now warns that scammers may abuse Android and iOS shopping platforms or applications to generate fake order notifications that appear to be legitimate Norton purchases. Those notifications may include fake order confirmations, fake charges, trusted third-party app delivery and fake support phone numbers embedded in order details.


Public user reports point to a wider pattern. One Reddit user described receiving a Shop.app message about a transaction they did not recognize, with no follow-up email confirmation, and a message telling them to call support if the order was not placed by them. Another thread includes reports of unrecognized iPhone purchases under “My Store,” suspicious contact emails and users saying they saw no matching charges on their bank cards. Other reports mention fictitious Apple gift card purchases and iPhone orders using the same kind of wording: “If Order Not Placed By You,” followed by a phone number.


The choice of brand is part of the social engineering. A fake security subscription charge plays on two fears at once: being billed for something you did not buy, and being left unprotected if you ignore it. Apple products and gift cards create high-value purchase panic. PayPal-style wording makes users worry about payment account abuse. In each case, the fake order is there to push the victim toward a call.


#### What we know, and what we do not


We have not found any evidence that any of the impersonated brands have been compromised.


We also should not claim that Shop or Shopify was breached based on the evidence we have. The exact abuse path still needs confirmation. The fake order may be entering the app through a merchant/order workflow, through email parsing, through account association, or through another legitimate mechanism that scammers have found a way to misuse. Shop’s own documentation shows that orders can populate from multiple sources, so assuming one route would be premature.


What we can say is narrower and stronger: scammers are placing fake purchase claims inside shopping app order experiences, where users are used to seeing real receipts and order updates. Norton’s own support page now describes this class of shopping app scam, and public reports show similar lures using other brands and products.


That is enough to warn users without overstating the technical evidence.


#### How the scam works


The user receives a notification or sees a new order in the Shop app. The order looks like a receipt for something expensive: a security subscription, an Apple gift card, an iPhone, a MacBook or another high-value purchase.


The receipt says the charge has been processed, the subscription has renewed, or the order is being prepared. Somewhere in the order details, the scammer inserts a phone number, email address or support instruction.


The wording is often clumsy. We have seen phrases like “If Order Not Place By You” and “If Need Help.” Those errors may look obvious when reviewing a screenshot. They are easier to miss when a phone notification tells you that hundreds of dollars have just been charged.


If the victim calls, the scam moves off-platform. The person answering may claim to be billing support, Norton support, PayPal support or a cancellation department. The script can vary, but the request usually moves toward credentials, payment data, one-time codes or remote access software.


By that point, the fake receipt has done its job. It has created enough urgency for the victim to enter a conversation controlled by the scammer.


#### Why this is harder for users to judge


Most people understand that email can be spam. They may still fall for phishing, but at least the inbox is a place where scams are expected.


Order-tracking apps are different. Their purpose is to collect receipts, shipping updates and purchase information in one place. A notification from that environment can feel as if it has already passed through some layer of trust, even when the content inside the order is fraudulent.


We have seen the same logic with calendar invite scams. The message itself is not always convincing, but the delivery channel changes how people read it. A fake invoice in an inbox is one thing. A fake invoice inside a calendar reminder or shopping app receipt can feel closer to a real event.


For security products, this also creates a detection problem. The first visible alert may not be an email, SMS or malicious website. It may be a legitimate app notification carrying fraudulent text. The scam still depends on social engineering, but the delivery surface is harder to classify with the usual rules.


#### What to do if you see one


Do not call the number in the receipt.


Check the charge directly. Open your bank or credit card app. Open the official account for the brand being impersonated. If it claims to be Norton, go to your Norton account or official Norton support. If it claims to involve PayPal, open PayPal directly. Do not use the phone number, email address or link inside the suspicious order.


If there is no matching charge, treat the receipt as a scam attempt.


Report the suspicious store or message in the Shop app if the option is available. Shop’s help pages say suspicious Shop stores can be reported from the store page through the app, and Shopify also tells users to forward phishing messages to its phishing inbox.


Norton users can forward suspicious messages to Norton and should verify unexpected charges only through their Norton account or official Norton support channels.


Opening the receipt does not automatically mean your device was compromised. The risk increases if you call the number, share information, install software or follow instructions from the person on the phone.


#### What to do if you have already called


End the call.


If you shared payment details, contact your bank or card provider immediately using the number on your card or inside your banking app. If you gave away a password, change it from a different device if possible. Start with your email account, Apple ID or Google account, banking account and any account connected to the fake invoice.


If you installed remote access software, disconnect the device from the internet and remove the software. Run a security scan before using the device again for banking, email or shopping.


If you gave a one-time code to the caller, treat the related account as compromised. Change the password, review recovery options and check recent login activity.


#### The lesson


Scammers keep testing new places to put old tricks. For years, fake invoices were mostly an email problem. Then similar subscription notices appeared in calendar invites. Now we are seeing fake receipts inside shopping app order histories and push notification flows.


The safest response is to separate the alert from the action. If an app, email, text message or calendar invite says you have been charged, do not use the contact details inside that alert. Go directly to the account or payment provider yourself. Check whether the charge exists. Use official support channels.


The fake receipt, the fake support number and the fake emergency are all part of the same controlled environment. The moment you verify the charge somewhere else, the scam loses most of its power.


More on this topic


[Inside Vidar’s ABE Bypass: From Memory Scanning to APC Injections – From Research](https://www.gendigital.com/blog/insights/research/inside-vidar-abe-bypass)


[Your flight was cancelled. Is the refund message real? – From Research](https://www.gendigital.com/blog/insights/research/flight-cancelled-refund-message-scam)


[Fake hiring pages abuse FIFA and other major brands to steal work credentials – From Research](https://www.gendigital.com/blog/insights/research/fake-hiring-pages-abuse-fifa)


[Luis Corrons](https://www.gendigital.com/blog/authors/luis-corrons)


Security Evangelist at Gen


At Gen, Luis tracks evolving threats and trends, turning research into actionable safety advice. He has worked in cybersecurity since 1999. He chairs the AMTSO Board and serves on the Board of MUTE.


[Jakub Vavra](https://www.gendigital.com/blog/authors/jakub-vavra)


Threat Operations Analyst at Gen


Follow us for more
