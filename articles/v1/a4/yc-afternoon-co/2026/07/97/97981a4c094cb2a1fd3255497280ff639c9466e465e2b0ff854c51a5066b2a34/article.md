---
schema_version: "1.0.0"
document_id: "97981a4c094cb2a1fd3255497280ff639c9466e465e2b0ff854c51a5066b2a34"
company_key: "yc-afternoon-co"
company: "Afternoon.co"
source_id: "yc-afternoon-co-news-import-d204747fe0e0"
canonical_url: "https://www.afternoon.co/blog/apple-app-store-sales-tax-guide"
published_at: null
first_seen_at: "2026-07-21T20:47:37.461730+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:eb26f6a349c993368d3a11b6c923b2ec10dad443774b994dd8fb921ef0af2a3a"
---

# Apple App Store Sales Tax: Do I Need To Collect It? (U.S. Developer Guide 2026)

If you sell apps or subscriptions through the Apple App Store, Apple App Store sales tax rules are confusing.


Apple collects some taxes for you. In other cases, you still have work to do. This guide explains how it works for U.S. developers in plain language.


## Quick answer: Do I need to collect sales tax on Apple App Store sales?


Short version for U.S. developers:


- For most App Store purchases, Apple is the merchant of record. Apple calculates, collects, and remits U.S. sales tax and many types of VAT and GST for you.
- Your main work is to give Apple correct tax info, track state economic nexus, and understand a few edge cases, like alternative payments and foreign entities.


U.S. sales tax:


- Apple collects and remits sales tax on App Store purchases where states require it.
- You may still need to register in some states and file zero dollar returns once you cross economic nexus thresholds.


International VAT and GST:


- Apple collects and remits VAT or GST in many countries as merchant of record.
- You still need to make sure your tax info and tax categories in App Store Connect are correct, and watch for countries where local developers are responsible instead.


Last updated: Jan 2026 based on official Apple documentation.


---


Talk to a sales tax expert


Get guidance tailored to your business


---


## U.S. sales tax: what Apple handles


### Apple as merchant of record


When Apple is merchant of record for App Store purchases, Apple:


- Decides when a sale is taxable
- Calculates the correct tax rate based on the buyer’s location
- Charges the tax to the customer at checkout
- Sends the tax to the right state agency


You see your proceeds after Apple’s commission. Sales tax amounts do not flow through you.


You can read more in Apple’s help article[Understanding taxes](https://developer.apple.com/help/app-store-connect/making-payments-to-apple/understanding-taxes/) .


### Your U.S. sales tax responsibilities


#### 1. Submit required tax forms in App Store Connect


As a U.S. developer you need to:


- Complete the W 9 in App Store Connect
- Make sure your legal name and tax ID match IRS records


You also choose tax related settings and categories during setup. If the W 9 or identity checks fail, Apple can hold or adjust payments.


For details, see[Provide tax information](https://developer.apple.com/help/app-store-connect/manage-tax-information/provide-tax-information/) .


#### 2. Track economic nexus in U.S. states


Even when Apple collects the tax, your App Store revenue still counts toward economic nexus thresholds.


Common patterns:


- Many states use a dollar threshold, a transaction count, or both
- Typical thresholds are around 100,000 dollars in sales or 200 transactions, but they vary
- Some states want marketplace sellers to register and file returns, even if platforms like Apple collect all tax


In those states, a typical return shows:


- Gross sales
- Marketplace sales that are already taxed by Apple
- Deductions that bring the taxable amount to zero


Other states are more relaxed and do not require registration if you only sell through marketplace facilitators.


You can check high level rules in[Streamlined Sales Tax marketplace facilitator guidance](https://www.streamlinedsalestax.org/for-businesses/marketplace-facilitator) and then review each state DOR site.


#### 3. Keep records from App Store Connect


From App Store Connect you can download:


- Transaction tax reports that show where Apple charged sales tax
- Financial reports for your accounting system


These reports help you:


- Prove that tax was collected and remitted by Apple
- Support nexus analysis by state
- Back up your own books during an audit


See[Transaction Tax Report fields](https://developer.apple.com/help/app-store-connect/reference/transaction-tax-report-fields/) for field level detail.


---


## International VAT and GST: where Apple collects


### Exhibit B: Apple’s official list


Apple publishes the current VAT and GST coverage list in[Exhibit B](https://developer.apple.com/support/terms/apple-developer-program-license-agreement/#S2a3-EXB) , which is part of the Apple Developer Program License Agreement.


This exhibit shows countries and regions where Apple:


- Is treated as merchant of record for App Store transactions
- Calculates and charges VAT or GST to customers
- Remits those amounts to local tax authorities


As of 2026, Apple collects VAT or GST in a long list of countries and regions, including much of Europe, Asia Pacific, the Americas, and parts of the Middle East and Africa.


Europe: Albania, Armenia, Austria, Belarus, Belgium, Bosnia and Herzegovina, Bulgaria, Croatia, Cyprus, Czech Republic, Denmark, Estonia, Finland, France, Georgia, Germany, Greece, Hungary, Iceland, Ireland, Italy, Kosovo, Latvia, Lithuania, Luxembourg, Malta, Moldova, Netherlands, Norway, Poland, Portugal, Romania, Russia, Serbia, Slovakia, Slovenia, Spain, Sweden, Switzerland, Ukraine, United Kingdom


Asia Pacific: Australia, Azerbaijan, Cambodia*, China*, India**, Indonesia*, Japan***, Kazakhstan*, Korea*, Kyrgyzstan*, Laos, Malaysia, Nepal*, New Zealand, Philippines*, Singapore*, Taiwan, Tajikistan*, Thailand*, Uzbekistan*, Vietnam††


Americas: Bahamas, Barbados, Canada, Chile, Colombia, Peru, United States, Uruguay†


Middle East and Africa: Bahrain, Benin, Cameroon, Egypt, Ghana, Ivory Coast, Kenya, Nigeria, Oman, Saudi Arabia, Senegal, South Africa, Suriname, Tanzania, Türkiye, Uganda, United Arab Emirates, Zambia, Zimbabwe


Notes for U.S. developers:


- Asterisked countries have special rules, but those rules usually affect local developers more than non resident U.S. developers
- Countries not listed in Exhibit B may expect the developer to handle VAT or similar taxes directly


Because Exhibit B changes over time, always check the current version here:


[Exhibit B](https://developer.apple.com/support/terms/apple-developer-program-license-agreement/#S2a3-EXB)


### Your international VAT responsibilities


#### 1. Submit complete tax information


For international tax handling to work correctly, you need to:


- Complete the W 9 if you are a U.S. entity
- Enter your legal name, address, and other requested details in App Store Connect


You can manage these settings in the same area covered in[Provide tax information](https://developer.apple.com/help/app-store-connect/manage-tax-information/provide-tax-information/) .


#### 2. Choose the right tax categories


Apple lets you set tax categories for your apps and in app purchases. These link to local VAT or GST rules.


For example, some countries offer reduced rates for:


- Books
- News or journalism
- Educational content


If you choose the wrong category, you might miss a reduced rate or trigger higher tax than needed.


You can learn how to set categories in:


- [Set a tax category for apps](https://developer.apple.com/help/app-store-connect/manage-app-information/set-a-tax-category/)
- [Set a tax category for in-app purchases](https://developer.apple.com/help/app-store-connect/configure-in-app-purchase-settings/set-a-tax-category-for-in-app-purchases/)


#### 3. Watch for tax and rate changes


Tax rates and coverage do not stay fixed. Apple updates rates and coverage and announces many changes in[Apple Developer News](https://developer.apple.com/news/) .


For example, Apple may adjust VAT rates for ebooks in a specific country, or add new regions where Apple will act as merchant of record.


A simple habit:


- Skim Apple Developer News every so often
- Look specifically for posts that mention tax, VAT, GST, or digital services


---


## When you must handle taxes yourself


### 1. You use alternative payment methods


If you use alternative payment methods that are allowed in some jurisdictions, Apple will not collect sales tax or VAT on those transactions.


In those cases, you are responsible for:


- Deciding when a sale is taxable
- Calculating and charging the correct tax rate
- Registering for tax in the relevant jurisdictions
- Filing and paying tax directly to tax authorities


Apple covers these cases in the article[Provide tax information for alternative payment options](https://developer.apple.com/help/app-store-connect/manage-tax-information/provide-tax-information-for-alternative-payment-options/) .


### 2. You have local entities outside the U.S.


If you open entities outside the U.S., the tax picture changes. Some countries treat local developers differently from foreign ones.


Based on[Exhibit B](https://developer.apple.com/support/terms/apple-developer-program-license-agreement/#S2a3-EXB) , Apple does not collect VAT on behalf of local developers in some countries. Examples include:


Non resident developers only, where Apple collects VAT:


- Cambodia
- Indonesia
- Kazakhstan
- Korea
- Kyrgyzstan
- Nepal
- Philippines
- Singapore
- Tajikistan
- Thailand
- Uzbekistan


Local developers must handle their own VAT:


- India
- Japan for local developers
- Vietnam for local corporate developers


Special situations:


- Mexico, where VAT registered developers must remit a portion of VAT themselves
- China, where Apple collects some but not all digital taxes
- Uruguay, where Apple collects certain digital transaction taxes only


Impact on a U.S. only developer:


- If you only have a U.S. entity, these local developer rules normally do not apply to you
- If you open a local entity in any of these countries, you should speak with a local tax advisor before routing App Store revenue there


### 3. Extra taxes on Apple’s service fee


Some countries may apply VAT or similar taxes to Apple’s service fee that you pay as a developer.


In that case:


- You may see VAT on Apple invoices for your fees
- Your accountant will need these invoices to decide if that tax is a cost or can be claimed as input tax


This VAT on Apple’s fee is separate from VAT on customer purchases. You do not pass it through to App Store users. It is handled in your own books.


---


## State by state sales tax considerations


### States that require registration even when Apple collects


Some states say that marketplace sellers must register or file returns even when a marketplace facilitator like Apple collects all the tax.


Common themes:


- You may need a sales tax account once you cross a threshold, even if returns show zero tax due
- Returns often ask you to report gross sales, then back out marketplace sales
- Filing frequency might be monthly, quarterly, or yearly, depending on the state


Action steps:


- Identify states where your App Store sales are highest
- Check each state’s marketplace facilitator page or guidance
- If rules are strict, consider registering and filing simple zero dollar returns


A good starting point is[Streamlined Sales Tax marketplace facilitator guidance](https://www.streamlinedsalestax.org/for-businesses/marketplace-facilitator) .


### States with lighter rules


Other states have more relaxed rules and do not require registration if:


- All your sales in the state are through marketplace facilitators
- The marketplace collects and remits all tax


You still want to:


- Confirm that the state treats Apple as a marketplace facilitator or similar role
- Keep your App Store Connect reports in case the state ever asks who collected tax


Because rules change, it is smart to review them once a year or ask a tax advisor to do a quick pass.


---


## Compliance checklist for U.S. developers


### Setup checklist


- Complete W 9 in App Store Connect
- Confirm your legal name and tax ID match IRS records
- Set tax categories for all apps and in app purchases using Apple’s guides
- Read the current version of[Exhibit B](https://developer.apple.com/support/terms/apple-developer-program-license-agreement/#S2a3-EXB) to see where Apple collects VAT or GST
- Make a note of U.S. states where you have meaningful App Store sales


### Ongoing checklist


- Download transaction tax reports and financial reports from App Store Connect each month or quarter
- Track U.S. sales by state to monitor economic nexus thresholds
- Review marketplace facilitator rules in your top states at least once a year, starting with[Streamlined Sales Tax guidance](https://www.streamlinedsalestax.org/for-businesses/marketplace-facilitator)
- Skim[Apple Developer News](https://developer.apple.com/news/) for tax and VAT updates
- Talk to a tax advisor if you open entities outside the U.S. or start using alternative payments


### Record keeping checklist


- Keep Apple’s tax reports, payout reports, and invoices in your accounting system
- Document your tax category choices and the reasoning for edge cases
- Keep copies of any state or foreign tax registrations and returns related to App Store sales
- Store any invoices that show VAT on Apple’s service fees


---


## Common questions from U.S. developers


### Does Apple collect U.S. sales tax for App Store purchases?


For App Store purchases where Apple is merchant of record and state rules apply, yes. Apple calculates the sales tax, charges it to the customer, and remits it to the state. Your payout is net of Apple’s commission, and the tax does not pass through you.


You can confirm the structure in[Understanding taxes](https://developer.apple.com/help/app-store-connect/making-payments-to-apple/understanding-taxes/) .


### Do I need to collect U.S. sales tax on top of what Apple collects?


Usually no, you do not collect extra sales tax on those App Store transactions. But your revenue still counts toward state economic nexus thresholds. Some states want marketplace sellers to register and file returns even when all sales tax is collected by platforms like Apple.


For those states, talk with a tax advisor or start with[Streamlined Sales Tax marketplace facilitator guidance](https://www.streamlinedsalestax.org/for-businesses/marketplace-facilitator) .


### Does Apple collect VAT or GST on international App Store sales?


In many countries, yes. When Apple is merchant of record and the country appears in[Exhibit B](https://developer.apple.com/support/terms/apple-developer-program-license-agreement/#S2a3-EXB) , Apple usually calculates and remits VAT or GST on App Store transactions.


You focus on:


- Keeping your tax info correct
- Choosing the right tax categories
- Tracking your reports for your own filings


### When am I fully responsible for tax on App Store sales?


You are fully responsible when:


- You use an alternative payment method that bypasses Apple’s billing
- You run a local entity in a country where local developers must handle VAT or similar taxes themselves
- You have other revenue streams outside the App Store, such as direct web sales, that are not covered by Apple’s merchant of record role


In these cases you decide where to register, how to charge tax, and how to file.


### How do I find current tax rates and coverage?


Use a mix of Apple and government sources:


- [Exhibit B](https://developer.apple.com/support/terms/apple-developer-program-license-agreement/#S2a3-EXB) for where Apple collects VAT or GST
- [Understanding taxes](https://developer.apple.com/help/app-store-connect/making-payments-to-apple/understanding-taxes/) for how Apple treats taxes in payments
- App Store Connect tax and payout reports for actual amounts charged
- [Apple Developer News](https://developer.apple.com/news/) for tax related updates
- State DOR sites and local tax authority sites for rules that apply directly to you


---


## Official documentation sources


Apple sources:


- [Exhibit B, official VAT and GST collection countries](https://developer.apple.com/support/terms/apple-developer-program-license-agreement/#S2a3-EXB)
- [Apple Developer News, tax updates](https://developer.apple.com/news/?id=2s2oe3qf)
- [Understanding taxes in App Store Connect](https://developer.apple.com/help/app-store-connect/making-payments-to-apple/understanding-taxes/)
- [Provide tax information in App Store Connect](https://developer.apple.com/help/app-store-connect/manage-tax-information/provide-tax-information/)
- [Tax report fields in App Store Connect](https://developer.apple.com/help/app-store-connect/reference/transaction-tax-report-fields/)
- [Apple Developer Program License Agreement](https://developer.apple.com/support/terms/apple-developer-program-license-agreement/)


Government and policy sources:


- [Streamlined Sales Tax marketplace facilitator guidance](https://www.streamlinedsalestax.org/for-businesses/marketplace-facilitator)
- State Department of Revenue sites for nexus and marketplace rules
- Local tax authority guidance in any country where you have a legal entity or significant sales
