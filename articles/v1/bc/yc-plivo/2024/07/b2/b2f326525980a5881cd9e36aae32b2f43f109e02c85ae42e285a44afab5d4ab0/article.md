---
schema_version: "1.0.0"
document_id: "b2f326525980a5881cd9e36aae32b2f43f109e02c85ae42e285a44afab5d4ab0"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/10dlc-registration/"
published_at: "2024-07-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:dfe63cbe7fccd994ca1f35e070c52f17d81055bb76e71283b4a0d9020c95f1b9"
---

# Everything You Need to Know About 10DLC Registrations

Governments worldwide have started mandating 10DLC registrations in response to the rise in SMS fraud. The US government has declared that all SMS/MMS messages sent to US phone numbers from unregistered 10DLC numbers will be fully blocked from September 1, 2023. Elsewhere, the consequences of not registering your number include increased carrier fees and other penalties.


10DLC registration helps differentiate legitimate messages from spam. Here’s everything you need to know about registering your 10DLC phone number to make sure your messaging campaigns reach their intended customers and don’t get flagged as fraudulent.


## What is 10DLC registration?


10DLC registration is the process of registering your 10-digit long code (10DLC) phone number with a specific campaign (known as the messaging use case, or the reason for sending your message). Registration shows that your number and messages associated with it are approved by a mobile carrier. This approval helps verify your organization’s legitimacy and the purpose of your messages.


Some common campaign types for 10DLC phone numbers are:


- **Marketing** : Promotional offers, loyalty program updates, and more
- **Transactional** : Order confirmations, delivery notifications, or password reset codes.
- **Customer support** : Real-time support through SMS chat.


Registration is especially important for application-to-person (A2P) 10DLC phone numbers. Most phones filter and sort messages based on the campaign type. For example, transactional messages and OTPs will be prioritized, whereas other marketing messages might be delivered late.


Ultimately, registering your 10DLC phone number can build your brand’s credibility, prevent compliance issues, and ensure campaign success.


### How does the 10DLC process differ for direct customers vs. resellers?


The 10DLC registration process can vary depending on your business type. A brand that directly communicates with customers can register its business entity directly.


However, if you’re an independent software vendor (ISV) who sends SMS and MMS messages on your customer's behalf, you must register separately for each customer.


For example, consider a hypothetical messaging platform for businesses in the United States *Ovilp Corp* . It should register its own brand and each of its customers' brands as individual brands.


[Source](https://docs.plivo.com/assets/posts/images/sms/10dlc/highlevel-outline.png)


## How does the 10DLC registration process work?


You can register your 10DLC phone number via campaign service providers (CSP) who liaise between your business and carrier. This CSP is usually the A2P provider or messaging service you use to send your messaging campaigns.


Here’s a general overview of the registration process.


### 1. Submit your details


The first step is sharing your business and campaign details with your CSP or A2P provider. This includes information like:


- Business type
- Business registration number
- Phone number
- Physical address
- Tax ID
- Website details
- Campaign type and purpose
- Opt-in and opt-out methodology
- Sample messages


You might also have to submit extra information depending on your A2P provider. Some A2P providers offer pre-defined campaign categories to choose from, while others might ask you to create custom campaigns.


### 2. Register your campaign


Once you share all the relevant details with the A2P provider, they will send your 10DLC application to The Campaign Registry (TCR). TCR is a central database that stores information about registered 10DLC phone numbers and their associated campaigns and is managed collaboratively by phone carriers in the United States.


### 3. Vet your campaign


Once the A2P provider submits your campaign to TCR, mobile carriers will thoroughly vet it. This process verifies the legitimacy of your business and ensures your intended use aligns with the chosen campaign category.


As all campaigns are manually verified by carriers, it can take anywhere from one to two weeks for your registration to be processed.


You can track the status of your 10DLC registration through your A2P provider. You can send SMS or MMS messages immediately if your registration is approved.


However, if your registration is rejected, you’ll have to share additional information depending on the feedback provided by the phone carrier and go through another round of vetting.


## **How much does it cost to register your 10DLC number?**


There are three main costs associated with registering your 10DLC number, as shown in the table below.


[Source](https://support.plivo.com/hc/en-us/articles/360054871572-A2P-10DLC)


Here’s what each of these fees goes toward.


### Brand registration fee


This is a one-time fee to register your brand with the registry. It costs $4 for sole proprietors and low volume standard brands (those that have less than 6,000 messages per day).


If your business sends over 6,000 messages, you must register as a standard brand, which costs $44 and includes extra vetting.


### Campaign vetting fee


This is another one-time payment, typically around $15 per campaign. It pays for the manual vetting process required to analyze each campaign's purpose and deliverability factors.


### Monthly campaign fees


These are recurring fees you pay based on the type of campaign you're running, ranging from $1.50 to $10 per month. The first payment will be for three months and must be paid when your campaign is approved. After that, you’ll be charged either monthly or quarterly, depending on your A2P provider’s protocols.


Here’s the fee breakdown for different campaign types.


- **Regular campaigns** : $10 per month
- **Low-volume mixed campaigns** : $2 per month
- **Political campaigns** : $10 per month
- **Charity campaigns** : $3 per month
- **Sole proprietor campaigns** : $0.75 per month


### Additional fees


In addition to the fees mentioned above, mobile carriers charge a fee for every message sent. This fee depends on the message type and your brand’s registration status.


Unregistered brands must pay a higher surcharge, varying from $0.01 (AT&T and Verizon) to $0.006 (T-Mobile).


Standard brands might also sometimes opt to pay a Trust Score Appeal fee. Every time a brand undergoes the vetting process, it’ll be given a Trust Score—this reflects your business’s legitimacy and potential for spam. You can apply for an appeal if you get a low trust score.


Here are the rates:


- $10 if the score is less than 45 days old
- $40 if the score is more than 45 days old


Also, Low-volume Standard and Standard brands pay an additional $40 when resubmitting a brand registration application.


***Note:*** *These are the rates when publishing this blog and may vary in the future. We recommend checking with your A2P provider or mobile carrier.*


## 10DLC vs. Toll-free phone numbers


While most organizations prefer using a 10DLC number for A2P messaging and a toll-free number for voice calls, that’s not always the case. You can register your toll-free number via your service provider and they’ll send it to the Toll-Free Number Administration (TFNA) for approval.


Unlike 10DLC registration, there’s no extra fee for registering your toll-free number. Plus, you can use it while your application is being processed.


However, toll-free numbers take longer to process (typically four to six weeks) and only support one-way broadcast messages.


Depending on your preferences, you can use 10DLC or toll-free numbers for SMS/MMS messaging campaigns. Make sure you register either number type with the relevant authorities and comply with the SMS/MMS messaging regulations.


## Registering your 10DLC number with Plivo


If you’re looking for an A2P provider to manage your 10DLC phone numbers and campaigns, then Plivo can help you streamline the entire process in four simple steps.


### 1. Create a profile


Start by creating a profile for your brand on Plivo. Your profile will include details like your legal name, authorized contact, physical address, and business registration details. This is also where you’ll specify the type of business you run—private, public, nonprofit, government, or sole proprietor.


If you’re a reseller, you can use Plivo’s *Profile API* keys to set up individual profiles for each customer.


### 2. Register your brand


The next step is registering your brand (or brands, in the case of resellers) with TCR. Standard brands (those who send more than 6000 messages per day) can opt for additional vetting at this point.


*Track the status of your brand registration application via the Plivo Console.*


‍


### 3. Register your campaigns


Once your brand is registered, you can register each of your campaigns. Here’s where you add details like campaign description, type, sample messages, and opt-in workflows for each campaign to the form shown below.


*Add your campaign details to the Plivo console and initiate the campaign registration process.*


‍


In the console, you can track the status of your campaign registrations—active, processing, or failed. If a campaign is rejected, you can review the feedback provided and reapply.


#### 4. Link campaigns to phone numbers


The final step is linking each active campaign to a phone number. Also, while there’s no limit on the number of campaigns a 10DLC phone number can be linked to, we recommend segmenting your numbers based on the campaign type—separating things like appointments, promotional offers, and customer support, for example.


This makes it easy to keep track of numbers and helps you avoid snowshoeing—sending the same message from different numbers, which is considered a violation by most mobile carriers.


## Why partner with Plivo?


Plivo offers enterprise-grade communication solutions that are secure, reliable, and scalable. We’re SOC 2 certified and compliant with major privacy and security regulations, such as GDPR, HIPAA/HITECH, PCI DSS, and ISO 27001:2022.


Plivo guarantees a 99.99% uptime owing to direct relationships with over 1,600 Tier 1 and local carriers. We’re backed by seven global POPs and two NOCs for continuous, round-the-clock coverage.


Curious to learn more about how Plivo can help you register your 10DLC phone numbers and manage your A2P messages?[Request a trial account](https://console.plivo.com/accounts/register/) and explore our platform commitment-free.
