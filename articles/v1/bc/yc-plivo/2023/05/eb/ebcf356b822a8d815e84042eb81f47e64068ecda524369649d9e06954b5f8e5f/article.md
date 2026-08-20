---
schema_version: "1.0.0"
document_id: "ebcf356b822a8d815e84042eb81f47e64068ecda524369649d9e06954b5f8e5f"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/trai-dlt-regulations-for-sms/"
published_at: "2023-05-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:1f8a0d58749c21744de2a785b9376f597132e38f358e5221106255bcf36f28fa"
---

# Complying with TRAI Regulations for Sending SMS

The Telecom Regulatory Authority of India (TRAI)[mandates](https://trai.gov.in/advice-to-senders) that all organizations that want to send SMS to phones in India via domestic messaging register on distributed ledger technology (DLT) platforms. Sender registration and whitelisting is designed to curb unsolicited commercial communication (UCC), control promotional spam, and reduce SMS fraud.


The size of the commercial communication market in India makes spam a giant problem. The country has 1.2 billion mobile subscribers who send and receive more than 30 billion messages per month. Not all of those messages are welcome, however.


DLT is a blockchain-based registration system that, for TRAI, records details about organizations, headers, consent information, and message templates. By following the rules around DLT registration, businesses that engage in telemarketing can ensure that their messages reach their target recipients.


We’ve written a[post](https://support.plivo.com/hc/en-us/articles/360046769131-DLT-Registration-Process-for-Sending-SMS-to-India) in our support portal that walks Indian businesses through the process of registering using any of four telecom providers’ portals. The process involves


1. Obtaining an Entity ID
2. Registering headers
3. Registering consent templates and content templates


## Step 1 — Register an Entity ID


An Entity ID is a 19-digit number that uniquely identifies a business. Once you’ve obtained it, you can use your Entity ID for all future header and template registrations. You’ll need several documents to obtain an Entity ID:


- Business PAN, a government-issued permanent account number. You can[register for a PAN online](https://incometaxindia.gov.in/Pages/tax-services/apply-for-pan.aspx) .
- GST or TAN certificate. GST stands for Goods and Service Tax, and TAN is Tax Deduction and Collection Account Number. As you can for a PAN, you can register for a[GST](https://reg.gst.gov.in/registration/) or[TAN](https://incometaxindia.gov.in/Pages/tax-services/apply-for-tan-online.aspx) certificate online.
- Letter of Authorization allowing an individual to register on behalf of the business.
- Government-issued ID for the authorized individual.


## Step 2 — Register headers


Once you’ve obtained an Entity ID you should register your headers. Headers are simply sender IDs for the numbers you plan to use to send text messages. To avoid confusion among message recipients, you may not use look-alike headers that differ only in their use of uppercase and lowercase letters.


## Step 3 — Register templates


The next step is to register consent templates and content templates. A consent template is a standard message that’s sent to users to get their consent to receive communications from enterprises. There’s no limit on the number of consent templates you can create. After consent templates are approved by the registrar, you can link them to content templates.


A content template is the actual text of an SMS message. Content templates can include variables, so you can show where specific values are supposed to go in your messages; for example, “your verification code is {#var#}.”


There are four types of content templates:


- Transactional — messages that contain a one-time password (OTP) required to complete a banking or credit or debit card transaction initiated by a bank customer
- Service implicit — any message arising out of customers’ actions or their existing relationship with the enterprise, and that is not promotional
- Service explicit — messages that require explicit consent from customers. This category includes service messages that don’t fall under the service-implicit category.
- Promotional — messages intended to promote or sell products, goods, or services. Service content mixed with promotional content is treated as promotional.


Our support post shows examples of each, and presents a list of dos and don’ts for both consent templates and content templates. For consent templates, for instance, choose a logical short name for each consent template; that’ll help you choose the right consent template to associate with content templates. In your consent template, include the brand name that’s relevant to the scope of the consent, along with a complete description of what you’re asking consent for. For example, “We would like to send marketing offers and events to our registered customers” or “We need your consent to send you messages about your account as well as marketing offers.”


After consent templates are approved by the registrar, you can link them to content templates in promotional or service categories. As with consent templates, use relevant and recognizable names for content templates. You can insert a variable placeholder {#var#} to stand in for values such as dates, amounts, account numbers, names, and one-time passwords. Each content template may be associated with only a single header; don’t use the same content template with multiple headers.


## Making registration easy


We’ve done our best to make the multi-step[DLT registration process](http://plivo-webflow.webflow.io/blog/dlt-registration) as simple as possible. If you follow the instructions in our support post you should have no trouble registering your business, so you can send text messages to domestic Indian phone numbers in compliance with TRAI DLT regulations.


The good news is that DLT registration is working. Since the government imposed its mandate, more than 2.5 million principal entities have obtained IDs. They’ve registered more than 600,000 headers and generated more than 5,500,000 approved message templates. All of this activity has yielded a welcome reduction in the number of customer complaints about registered telemarketers.
