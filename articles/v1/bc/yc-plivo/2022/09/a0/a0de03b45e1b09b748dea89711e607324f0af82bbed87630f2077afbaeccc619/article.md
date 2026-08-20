---
schema_version: "1.0.0"
document_id: "a0de03b45e1b09b748dea89711e607324f0af82bbed87630f2077afbaeccc619"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/dlt-registration/"
published_at: "2022-09-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:60165dde3583d674406c8470d6c3e3394487e6332a46ca404cd1a829827a09a1"
---

# How to Complete DLT Registration for Sending Text Messages in India

To minimize spam texting in India, the Telecom Regulatory Authority of India (TRAI) mandates that all entities that want to send SMS messages must register certain information — their entity/organization, CTAs, and message templates — along with required consent information on distributed ledger technology (DLT) platforms. Many people find the process confusing; this post walks you through it.


The online DLT registration process necessary for sending bulk SMS messages involves four steps: registering your business entity, registering your calls to action (CTAs), registering your sender IDs (called headers in DLT parlance), registering consent templates, and registering content templates and associating them with headers and consent templates.


## What does DLT stand for?


DLT is an acronym for Distributed Ledger Technology. The most well-known application of DLT is the blockchain, but it is also relevant to telecommunications.


DLT replicates, shares, and synchronizes data across multiple locations. Imagine a digital ledger shared among multiple computers instead of stored in a single location. In telecoms, DLT registration is mandatory for all businesses sending SMS messages in India. The goal of registration is to protect customer privacy and deter spammers from sending unsolicited SMS messages.


## What is DLT registration?


DLT registration is the process of adding your business details, including sender IDs, message templates, and other required information, on a Distributed Ledger Technology (DLT) platform. As part of DLT registration, companies need to submit their business name, address, contact information, and the purpose of their SMS campaigns. DLT registration provides each business with a unique identifier to use when sending SMS messages.


## Why is DLT registration required?


The TRAI discovered that, in India,[85-90% of the total SMS senders](https://www.smscountry.com/blog/dlt-registration/) are spammers. The high number of fraudulent messages sent throughout the country damages customer relationships, compromises data security, and weighs down the profit margins of businesses that use SMS campaigns.


DLT registration aims to enhance customer privacy by deterring spammers. DLT registration involves using pre-approved message content, allowing for easy tracking and identification of spammy or misleading messages. Consumers can report unwanted messages, helping authorities crack down on fraud and spam.


Likewise, non-compliance with DLT regulations can lead to severe penalties, including suspension or termination of SMS services.


## How does DLT work?


DLT registration deters spam and other bad actors in a few different ways:


- **Identification:** Each message is linked to a registered entity and template.
- **Tracking:** Message content and sender information can be traced.
- **Penalties:** Non-compliance can lead to severe penalties, including blacklisting.
- **Consumer Control:** Consumers can report unwanted messages.


By implementing these measures, DLT registration helps create more reliable, trustworthy brand communication and protects consumers from fraud.


## How to apply for DLT registration to send SMS in India


The major India telecom providers all host DLT registration platforms and share their information with each other. You can use any of them to register:


- [Videocon](https://smartping.live/entity/register-with)
- [Vodafone Idea Limited](https://www.vilpower.in/) (VILPOWER)
- [Airtel](https://dltconnect.airtel.in/signup/)
- [Jio](https://trueconnect.jio.com/#/)


### 1. Get a unique Entity ID


The first step is to visit one of the DLT registration platforms and fill in the information required to register your business as an entity, including the organization type, KYC documentation, and the business’s postal and email addresses. There are two organization types: principal entity (a.k.a. enterprise) and telemarketer. You’ll get a temporary ID or reference number; please[share your ID or reference number with the Plivo support team](https://support.plivo.com/hc/en-us/requests/new?ticket_form_id=360000156292) . After the DLT platform verifies your documents, the operator will email you a unique Entity ID for the company registering, which you should share with us too.


### 2. Register headers


Once you get your unique Entity ID, register your headers on the DLT platform’s portal. Only entities with registered headers can send SMS messages.


Add your headers under the Headers tab in the dashboard.


You can choose between two types of Headers — Promotional and Others. To send campaigns of the former type, such as offers, discounts, and promotions, choose a six-digit numeric sender ID. The DLT platform will automatically add a prefix that represents your industry; for instance, for the communication industry, the prefix is “6.” If you plan to send transactional campaigns, such as alerts and notifications, you can choose a six-character alphanumeric header name. Headers are case-sensitive, so PLIVO and Plivo, for example, are two different headers and can be registered separately.


Enter each header name in the text box and state the reason for choosing it in the description box below. Use one header per use case: one for OTP, one for promotional, and one for notification messages, for instance. We recommend using as few headers as you can and linking multiple templates to a single header.


Your header name should correlate with your entity name. If the header name is different, explain the discrepancy in the description box. Include your mobile number in the description so the DLT platform’s support team can contact you if they have questions.


### 3. Register your CTAs


Recently TRAI updated its rules to require brands to whitelist any CTAs included in their messages. Whitelisting takes place on the DLT platform and is a crucial process for ensuring your messages are successfully delivered. The following types of CTAs must be registered.


#### 1. URLS


- Any link/URL used in an SMS.
- WhatsApp links
- APK links


#### 2. Numbers used for calls or messages


- Mobile
- Landline
- Toll-free


#### 3. Email addresses


**For example:**


- If you include a link for users to sign up for an account, that URL must be whitelisted.
- If your messages contain a callback number or an email address, those must also be whitelisted.


**Whitelisting Requirements at a Glance**


CTA Type CTA Sample CTA (If Required) Subtype Details


URL[https://abc.com/](https://abc.com/) N/A Static URL Only exact matches will be allowed


URL + Variable[https://abc.com/xyz?](https://abc.com/xyz?)[https://abc.com/xyz?product=1](https://abc.com/xyz?product=1) Dynamic URL You need to whitelist only the static part of the URL. For example, if you have two URLs, such as[https://abc.com/lmn](https://abc.com/lmn) and[https://abc.com/pqr](https://abc.com/pqr) , you only need to whitelist the static part of the URL, which is[https://abc.com/](https://abc.com/)


Number 91XXXXXXXXX
022XXXXXXXX
1800/1860XXXXXX 919414XXXXXX
02267676XXX
1800180XXXX Mobile / Landline / TFN Only exact matches will be allowed


Emailsupport@abc.com N/A Email Only exact matches will be allowed


### 4. Register consent templates


After you’ve registered your business entity and headers, the next step in the DLT registration process is to add consent templates. A consent template contains a standard message that the business must send to users to get opt-in consent to receive promotional communications from enterprises.


When users receive a consent message, they can respond directly or use a link in the message to allow the business to send them future messages. Promotional messages, which we’ll talk about in a moment, must be linked to consent templates. You can create multiple consent templates to cover as many brands and use cases as you have.


Give each template a descriptive name; you’ll use the name to associate content templates with the consent template later. For brand name, use a relevant product or trade name. The scope of consent should describe the content you plan to send to users, and should be related to the brand name provided. For example:


- We would like to send communication regarding marketing offers and events to you as one of our registered customers
- ABC Solutions needs your consent to send you messages about your account information and activity and our best offers
- We want to send updates, transactions, and recommendations of our services and products to registered customers like you


Be descriptive; don’t say something short like “consent” or “SMS to customers.” Also, don’t use variables in the scope of consent; variables are applicable only to content templates.


You can include opt-out instructions in the consent template; for example: “To opt out, send STOP to <your number>.”


After you fill in the details in the three fields, click **Submit** to submit the template for approval. The registrar may take three to seven business days to return an approval or rejection. If your template is rejected — if, for instance, the template isn’t a match for the header — you can update the template and submit the revised version.


### 5. Register content templates


After you’ve registered consent templates, the next step in the DLT registration process is to add content templates. A content template includes the actual text of an SMS message you want to send.


Choose the template type from among available choices: transactional, service implicit, service explicit, and promotional. We’ll explain what each means in the section below.


Give each content template a relevant and recognizable descriptive name; you’ll use the name to associate a consent template with your content templates later.


For the message type, choose “Text” for general messages and “Unicode” for regional messages.


For the template message, you can use letters, numbers, and special characters. Avoid using two or more spaces before, after, or between words.


Insert values such as dates, amounts, account numbers, names, and OTPs using variables. There’s no limit on the number of variables you can use per message; individual variable length may not exceed 30 characters. Transactional and service content should always have some variable content. Promotional content can have some variable or all fixed content. All messages should have some fixed content; no message should be only variables.


The variable format in text is {#var#}, which is case-sensitive.


Don’t use a single content template with multiple headers — tailor the content to a specific use case, and thus a single header. Also, make sure that you associate each content template with the relevant header; if you have a header for sending OTPs and another for promotional messages, pay attention to which header you map to each content template.


If your content template is for a promotional message, you must also associate the content template with an existing consent template, so users have a chance to opt in to receiving your promotional messages.


Click **Save** or **Submit** (depending on which portal you use) when you’ve filled out all the required fields. You should then see a screen that shows a list of your content templates.


One of the fields in the list shows the status for each template. New templates are marked Pending. The portal operator will review the template and return an approval or rejection in three to seven working days. Templates may be rejected if they’re irrelevant or not related to the business. The DLT platform will notify you if your template is rejected, and you can update it and reapply.


## Types of content templates


Here’s more information about the four types of content templates.


### Transactional


A message that contains a one-time password (OTP) required to complete a banking or credit or debit card transaction initiated by a bank customer is considered transactional. Here are some examples of this type of content, along with examples of the template formats that would create them.


Financial service enterprises should use the transactional category for OTP messages during fund transfers, online payments, and merchant transactions. Don’t select the transactional content type for nonbanking enterprises.


Actual Message Template Format


234567 is the OTP for txn of INR 57.75 at Izaak payment services PVT with your SBI card xx3931. OTP is valid for 10 mins. Pls do not share with anyone {#var#} is the OTP for txn of INR {#var#} at {#var#} with your SBI card {#var#}. OTP is valid for {#var#}. Pls do not share with anyone


234567 is your OTP for fund transfer for the amount Rs.6,000 to Raja. OTP valid for 8 minutes. Do not share this OTP with anyone {#var#} is your OTP for fund transfer for the amount {#var#} to {#var#}. OTP valid for {#var#}. Do not share this OTP with anyone


234567 is the OTP for your ecomm txn for the amount Rs.25,000. OTP valid for 8 minutes. Do not share this OTP with anyone {#var#} is OTP for your ecomm txn for the amount {#var#}. OTP valid for {#var#}. Do not share this OTP with anyone


### Service-implicit


Any message arising out of customers’ actions or their existing relationship with the enterprise, and that is not promotional, is considered a service-implicit message. These include:


- Confirmation messages of net banking and credit/debit card transactions
- Product purchase confirmation and delivery status from ecommerce websites
- Customer is making payments through a payment wallet for an ecommerce website or mobile app, and an OTP is sent to complete the transaction. These OTPs are not considered transactional because they’re not sent from a bank but from a third party.
- OTPs required for ecommerce websites, app logins, social media apps, authentication and verification links, securities trading, demat account operations, KYC, and ewallet registration
- Messages from telecom and internet service providers
- Periodic balance info, bill generation, bill dispatch, due date reminders, recharge confirmation (DTH, cable, prepaid electricity recharge, etc.)
- Delivery notifications, updates, and periodic upgrades
- Messages from retail stores related to invoices and warranties
- Messages from schools, such as attendance and transport alerts
- Messages from hospitals, clinics, pharmacies, radiologists, and pathologists about registration, appointments, discharge, and reports
- Confirmatory messages from app-based services
- Government/DOT/TRAI mandated messages
- Service updates from car workshops, repair shops, and gadgets service centers
- Directory services such as Justdial and Yellow Pages
- Day-end and month-end settlement alerts to securities and demat account holders


Actual Message Template Format


Thank you for using EMI Facility on your IDBI Bank Credit Card 4***1234. EMI request for Rs.89000.00 executed on 01/07/2022 Thank you for using EMI Facility on your IDBI Bank Credit Card {#var#}. EMI request for {#var#} executed on {#var#}


XXX BANK — Your new bill for BESCOM Bangalore account 0123456000 for Rs.4339.00 could not get scheduled because the auto-pay limit is less than the bill amount XXX BANK — Your new bill for {#var#} account {#var#} for Rs.{#var#} could not get scheduled because the auto-pay limit is less than the bill amount


Account: 123456 is your Pansoi account verification code Account: {#var#} is your Pansoi account verification code


### Service-explicit


Service-explicit messages require explicit consent from customers. Consent must be verified by the recipient in a robust and verifiable manner and recorded by the consent registrar. This category includes any service messages that don’t fall under the service-implicit category.


Service-explicit content templates must be linked to consent templates.


Examples include messages to the existing customers recommending or promoting products or services.


Actual Message Template Format


Your Rs.500 voucher is ready. Redeem it on purchase of Rs.1000 at M&M. Use code 654321001. Valid till 31 Mar 2023! T&C apply Your Rs.{#var#} voucher is ready. Redeem it on purchase of Rs.{#var#} at M&M. Use code {#var#}. Valid till {#var#}! T&C apply


Please click on https://mosl.co/ywq8FBJpAn to share your meeting experience with your Ojas Insurance representative Please click on {#var#} to share your meeting experience with {#var#}


Baba Finserv personal loans need minimal documentation. Meet your financial needs in one click: https://m.BabajFin.in/Iphr8tFE Baba Finserv personal loans need minimal documentation. Meet your financial needs in one click: {#var#}.


### Promotional


Promotional messages promote or sell products, goods, or services. Service content mixed with promotional content is treated as promotional.[Plivo](https://www.plivo.com/) will send these messages to customers after scrubbing them to ensure that messages are delivered only according to the consent criteria set by the customer in the national Do Not Disturb (DND) registry.


Promotional content templates must be linked to consent templates.


Promotional content must be sent only from numerical sender IDs.


## Benefits of DLT registration for customers


DLT registration reduces spam that, at best, is irritating for customers — and at worst, compromises personal data security. By verifying the identity of message senders, DLT significantly reduces the number of spam and fraudulent messages customers receive. Cutting back on spam increases trust, enhances privacy, and gives customers more control over their communication preferences. With DLT registration, customers know the messages they receive come from verified businesses. DLT prevents unauthorized access to personal information through unsolicited messages.


Plus, many DLT platforms offer customers the ability to opt out of specific message categories or from particular businesses, giving them power over the messages they receive.


## Benefits of DLT registration for businesses


The TRAI primarily designed DLT registration for customers, but it benefits businesses, too. Messages from registered senders are more likely to make it past spam filters and reach their intended recipients. In turn, reduced spam and higher message delivery rates contribute to better customer engagement and satisfaction. By adhering to regulatory standards, businesses build trust and credibility among customers.


There are also financial reasons to complete DLT registration. By complying with regulations, companies avoid TRAI penalties and legal issues. Businesses can optimize their SMS campaigns for better results, using pre-approved templates to boost conversion rates. And, DLT registration creates a more reliable communication channel.


## After successful registration, leave it to Plivo


Once you’ve registered everything on the DLT platform, please[create a ticket](https://support.plivo.com/hc/en-us/requests/new?ticket_form_id=360000156292) to share with us your


- Entity ID
- Header
- Header ID
- Whitelisted CTAs
- Template ID
- Message Template


Include screenshots for reference so Plivo can share the information with the carriers. Once the carriers have mapped the header and content, Plivo will map the sender ID to your account and let you know that you’re good to go. ***‍***
