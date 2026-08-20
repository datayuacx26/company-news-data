---
schema_version: "1.0.0"
document_id: "5a8a485a915a366ae3fca737bd6d6c64f8c5f2ddf3b6ec5fd30651c9cd53522f"
company_key: "yc-zoko"
company: "ZOKO"
source_id: "yc-zoko-news-import-ab465cd49b6c"
canonical_url: "https://www.zoko.io/post/messagebird-whatsapp-business-api-pricing"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T23:04:26.118264+00:00"
fetched_at: "2026-08-19T23:04:27.924643+00:00"
content_hash: "sha256:4aa2d1bf9b915f1f788cdfd78ac84ff04d17254b385e3fea0c58c2d9b57f1519"
---

# MessageBird WhatsApp API Pricing in 2026: What Indian Shopify Sellers Actually Pay

An Indian Shopify seller shopping for a WhatsApp API provider usually runs into the same wall: global BSPs like MessageBird (now operating as Bird) publish pricing in USD, structured around markets like the US and Western Europe, while the actual cost that lands on an Indian business is set by[Meta's India-specific rate card](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing) , billed in rupees since the start of 2026.


Those are two different numbers, and conflating them is how a lot of merchants end up budgeting wrong.


This article breaks down exactly how Bird prices WhatsApp, what Meta specifically charges Indian businesses, and what an Indian D2C or[Shopify](https://www.zoko.io/whatsapp-plugin-for-shopify) brand should actually compare before choosing between a global BSP like Bird and a platform built for the Indian ecommerce market from the ground up.


‍


## **TL;DR**


- MessageBird (Bird)'s WhatsApp bill has two layers: Bird's own tiered processing fee based on monthly message volume, plus Meta's passthrough rate, which is set separately by category and by the recipient's country.
- For India specifically, Meta's 2026 rate card prices marketing template messages at roughly ₹0.86 per delivered message, with utility and authentication messages priced far lower at roughly ₹0.115 each — and these are billed in rupees directly, not converted from a USD base.
- User-initiated service replies inside the 24-hour customer service window are free under Meta's rules, regardless of which BSP an Indian merchant uses.
- On top of Meta's rate and the BSP's own fee, GST at 18% applies to both charges for any India-registered business, which is easy to miss when comparing a global provider's USD pricing page against a local one quoting rupees.
- For an Indian Shopify brand specifically, the more useful comparison isn't Bird's global rate card in isolation — it's the total cost once COD confirmation, abandoned cart recovery, and festive-season broadcast volume (all high-frequency use cases for Indian D2C) are factored in against a Shopify-native platform's automation.


‍


## **Is MessageBird's WhatsApp API Free for Indian Businesses? What Costs to Actually Expect**


Connecting a WhatsApp Business number to Bird doesn't cost anything by itself — there's no signup fee to get API access. What an Indian merchant actually pays for is the messages sent, which are split into two separate charges that appear together on the invoice.


This is standard across the WhatsApp Business API industry: the underlying platform access is free, and the cost sits entirely in per-message and per-conversation charges layered on top by Meta and by the Business Solution Provider (BSP) routing the messages.


‍


### **The Two-Part Cost Structure Behind Every Bird WhatsApp Bill**


‍


The first part is Bird's own processing fee, priced on a tiered basis by total monthly message volume. The second is Meta's passthrough fee, which is entirely outside Bird's control and depends on the message category (marketing, utility, or authentication) and the country of the person being messaged — which, for an Indian Shopify store, means the India-specific rate card, not the US or European rates that show up by default on most global BSP pricing pages.


Because these two fees come from two different companies, calculating the total cost requires checking both line items separately, in the correct currency, rather than assuming a single number covers everything.


‍


## **How Bird Prices WhatsApp Messages, and What Meta Charges Indian Businesses Specifically**


Bird's official WhatsApp pricing page lays out its own processing fee as a volume tier: businesses sending between 1 and 1,000 messages a month pay $0.0010 per 1,000 messages, that climbs to $0.0050 per 1,000 for the 1,001–100,000 range, then steps back down to $0.0045 per 1,000 for 100,001–500,000 messages, and $0.0040 per 1,000 beyond 500,000 a month, according to[Bird's own WhatsApp pricing page](https://bird.com/en-us/pricing/whatsapp) .


Monthly messages sent Fee per 1,000 messages


1 – 1,000 $0.00


1,001 – 100,000 0.005


100,001 – 500,000 $0.00


500,001+ $0.00


*This fee is denominated in USD by default, which matters for an Indian business budgeting in rupees, since it must be converted and added to a separately billed Meta rate.*


‍


### **Meta's India Rate Card Is the Number That Actually Matters**


As of the 2026 rate update, Meta prices marketing template messages to Indian numbers at roughly ₹0.8631 per delivered message. While the utility and authentication template messages sit at roughly ₹0.115 each, service replies remain free.


Since January 2026, Meta has billed Indian businesses directly in rupees rather than a converted USD figure, which simplifies budgeting somewhat compared to markets still billed in USD. This is a meaningfully different cost profile than the US rates most global BSP pricing pages lead with, which is exactly why an Indian merchant checking a provider's default rate card can end up looking at the wrong market's numbers entirely.


‍


Message Category Rate (Per Delivered Message)


Marketing ₹0.8631


Utility ₹0.115


Authentication ₹0.115


Service (user-initiated reply) Free


‍


### **‍** **User-Initiated Service Conversations Are Free**


When a customer messages an Indian business first, a common pattern is for WhatsApp-first shoppers to check on an order or ask a product question.


The reply is free under Meta's own rules, provided it's sent within the 24-hour customer service window and is either a non-template message or an approved utility template. This holds true regardless of BSP, since it's Meta's policy rather than something Bird or any other provider chooses to offer.


‍


## **Why Message Category Changes What an Indian Seller Pays**


The category assigned to a template — marketing, utility, or authentication — is the single biggest lever in total cost, more so than which BSP is used.


Meta shifted its entire pricing model from conversation-based charging to per-message charging on July 1, 2025.


This means businesses are billed for each delivered template message rather than an entire 24-hour conversation window. This change is confirmed in[Meta's official developer documentation](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing) on WhatsApp Business Platform pricing and corroborated by[Twilio's](https://www.twilio.com/en-us/changelog/meta-is-updating-whatsapp-pricing-on-july-1--2025) changelog announcement of the same update.


For an Indian D2C brand, this distinction is worth roughly 7–8x in per-message cost. A marketing broadcast (a festive sale announcement or a re-engagement campaign) costs significantly more per message than a utility notification, such as an order confirmation or a COD verification.


This is why brands running high broadcast volume around sales events like Diwali or end-of-season clearances should model marketing and utility spend separately rather than as one blended number.


‍


### **GST Adds a Layer to Global Pricing Pages Don't Show**


An 18% GST applies on top of both Meta's per-message charges and the BSP's own platform or processing fees for any India-registered business.


This is a straightforward compliance requirement, but it's also a cost that doesn't appear on Bird's USD-denominated global pricing page, which is another reason an Indian merchant needs to build their own cost model rather than reading a rate card at face value.


‍


## **How MessageBird's WhatsApp Pricing Compares to India-Focused Providers**


Global infrastructure platforms like Bird price WhatsApp as one product among many — email, SMS, voice, and WhatsApp all under one tiered processing-fee model, aimed at a broad, largely US- and Europe-facing customer base.


India-focused BSPs, by contrast, typically quote pricing directly in INR, price plans around Indian message volume patterns (which skew heavily toward utility and COD-related messages), and are built with GST already factored into their invoicing.


Neither structure is inherently cheaper; a business needs to model its actual expected message mix (how much marketing versus utility volume) against both a global provider's tiered USD fee and a local provider's rupee-denominated plan to see which comes out ahead at its specific scale.


‍


‍


### **What This Means for an Indian Shopify Brand Specifically**


The practical issue with comparing Bird against an India-first platform on rate card alone is that Bird's default pricing page shows US figures.


Converting those into an accurate Indian cost estimate requires manually swapping in Meta's India rate card and adding GST — extra steps a locally-built platform's pricing page has already done for you.


‍


## **WhatsApp Pricing by Use Case for Indian D2C and Shopify Brands**


‍


The use cases that generate the most WhatsApp volume for Indian ecommerce sellers specifically are worth costing out separately, since they sit at very different points on the marketing-versus-utility spectrum.


COD order confirmation and reconfirmation messages — a high-volume use case for Indian D2C given how much of the market still transacts cash-on-delivery — fall into the utility category and are priced at the lower ₹0.115 rate, making them relatively inexpensive to run even at high volume. Abandoned cart recovery messages typically also qualify as utility or service-conversation replies when timed correctly, keeping per-message cost low relative to broadcast marketing.


Festive-season and promotional broadcasts, by contrast, land squarely in the marketing category at the higher **₹0.8631 rate,** and this is where sending to a broad, unsegmented list gets expensive fast — a reason to prioritize targeting opted-in, engaged segments rather than blasting an entire customer list every time a sale goes live.


‍


### **Where Automation Changes the Math for Indian Sellers**


None of this changes what Meta charges per message, but it changes how many messages actually need to be sent and in which category.


A Shopify brand manually broadcasting to its full list for every sale burns through marketing-category sends fast. A brand using automation to trigger COD confirmations, cart recovery, and delivery updates as utility-category messages, while reserving marketing sends for genuinely targeted, opted-in segments, keeps its blended per-message cost meaningfully lower without changing Meta's underlying rate card at all.


‍


## **Choosing Between MessageBird and a Shopify-Native Platform Built for This Market**


For an Indian Shopify brand specifically, the decision usually comes down to whether WhatsApp needs to sit inside a broader, multi-channel infrastructure stack (which is what Bird is built for) or whether it's the primary sales and support channel for the store, which is a different starting point entirely.


Zoko prices WhatsApp specifically for Shopify brands, with plans built around conversation volume and no markup on Meta's own WhatsApp rates, as laid out on[Zoko's pricing page](https://www.zoko.io/pricing) — merchants can estimate their exact monthly cost using Zoko's own bill estimator before committing, in the same currency and rate structure they'll actually be billed in.


‍


### **Where Zoko Fits for Indian Shopify Sellers**


Beyond the pricing structure, the practical difference for an Indian D2C brand is what the subscription includes out of the box:[Zoko's Shopify-native platform](https://www.zoko.io/whatsapp-plugin-for-shopify) syncs your product catalog, automates COD confirmation and abandoned-cart recovery through Zoko Flows, and provides your team with a shared inbox.


All of these are built around the exact high-volume, utility-heavy use cases that define Indian ecommerce on WhatsApp, rather than requiring a separate Shopify connection, as a generalist global BSP typically does.


Merchants can see how this plays out for other Indian and Shopify-first brands in[Zoko's case studies](https://www.zoko.io/case-study) and get a full sense of the platform on[Zoko's homepage](https://www.zoko.io/) before comparing it against a broader infrastructure provider like Bird.


‍


## **Frequently Asked Questions**


‍


### **1. Does MessageBird bill Indian businesses in rupees or dollars?**


Bird's own processing fee is denominated in USD on its published pricing page. Meta's underlying per-message rate for India, however, is billed directly in rupees as of the 2026 update, so an Indian business using Bird ends up with a mixed-currency bill unless the provider explicitly converts and consolidates it.


‍


### **2. Is GST applicable on WhatsApp API charges through a global provider like Bird?**


Yes. Any India-registered business is subject to 18% GST on both Meta's message charges and the BSP's own fees, regardless of whether the BSP is based in India or operates globally like Bird.


‍


### **3. Why do COD confirmation messages cost less than marketing broadcasts?**


COD confirmations are classified as utility messages, which Meta prices significantly lower than marketing templates in every market, including India. Marketing messages carry the highest rate because they cover promotional and re-engagement content rather than functional order-related communication.


‍


### **4. Can I estimate my WhatsApp costs in rupees before signing up with a provider?**


Some BSPs, particularly India-focused ones, publish rate cards directly in INR. For global providers like Bird, whose default pricing page shows USD, you'll need to manually apply Meta's India-specific rate card and add GST to get an accurate rupee estimate.


‍


### **5. Does Bird charge extra fees beyond what Meta charges?**


Yes. Bird applies its own tiered processing fee on top of Meta's passthrough rate, with the exact fee depending on total monthly message volume. This is separate from, and in addition to, whatever Meta charges for the message category and country.


‍


### **6. Is WhatsApp API pricing the same for every provider in India?**


No. While Meta's underlying per-message rate is fixed by category and country, each BSP adds its own fee structure on top — global providers like Bird use tiered USD-denominated fees, while India-focused platforms often price and bill directly in rupees with GST built in.


‍


### **7. How much does a WhatsApp marketing broadcast cost during a festive sale in India?**


At Meta's 2026 India rate of roughly ₹0.8631 per marketing message, a broadcast to an unsegmented list of 100,000 contacts would cost roughly ₹86,310 in Meta charges alone, before any BSP fee or GST — which is why targeting opted-in, engaged segments rather than a full list matters more during high-volume periods.


‍


### **8. Are utility messages really free inside the 24-hour window?**


Yes, when sent in response to a customer who messaged first, utility templates and free-form replies inside that 24-hour window are free under Meta's current pricing rules, regardless of which BSP is used.


‍


### **9. Did WhatsApp pricing change again in 2026 for Indian businesses?**


Yes. Marketing template rates for India rose from roughly ₹0.7846 to ₹0.8631 per message, effective January 1, 2026, alongside Meta's move to bill Indian businesses directly in local currency rather than a converted USD rate.


‍


### **10. Should an Indian Shopify store use a global BSP like Bird or a Shopify-specific Indian platform?**


This depends on whether WhatsApp is one channel among several managed centrally across markets, or the store's primary commerce channel in India specifically. Shopify-native platforms built around Indian use cases like COD confirmation typically reduce both the integration work and the currency/GST guesswork involved in budgeting.
