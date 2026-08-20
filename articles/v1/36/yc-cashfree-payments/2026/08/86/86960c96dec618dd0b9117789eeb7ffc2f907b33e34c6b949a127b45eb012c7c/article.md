---
schema_version: "1.0.0"
document_id: "86960c96dec618dd0b9117789eeb7ffc2f907b33e34c6b949a127b45eb012c7c"
company_key: "yc-cashfree-payments"
company: "Cashfree Payments"
source_id: "yc-cashfree-payments-rss-98daff448d11"
canonical_url: "https://blogrevamp.cashfree.com/upi-integration/"
published_at: "2026-08-08T08:50:26+00:00"
first_seen_at: "2026-08-08T10:55:52.333309+00:00"
fetched_at: "2026-08-08T10:55:54.763171+00:00"
content_hash: "sha256:a1fa7402863095551e746b24e7e72d6571e2e6944ac01bb02a46f18f41da3e4a"
---

# How to Integrate UPI Payment Gateway in Website or Mobile App

Table of Contents


Toggle


UPI has become one of the most widely used ways to pay online in India. For businesses, integrating UPI payments into a website, mobile app, or checkout experience makes it easier for customers to pay directly from their bank accounts using apps such as Google Pay, PhonePe, Paytm, BHIM, and other UPI-enabled apps.


But UPI integration is more than simply adding a UPI ID or QR code to your checkout. Businesses can choose from different integration methods, including UPI Intent, dynamic QR codes, APIs, SDKs, and payment links, depending on their platform and payment requirements.


In this guide, we’ll explain what UPI integration is, how[UPI payment integration](https://www.cashfree.com/docs/payments/manage/payment-methods/upi) works, the different UPI payment flows, how to integrate UPI payments on a website or mobile app, and how to verify payment status after a transaction.


##


What is UPI Payment Gateway?


A UPI Payment Gateway is a digital service that allows businesses to accept and process UPI payments from UPI apps like PhonePe, Google Pay, Paytm, BHIM, etc.


Instead of building separate integrations with individual UPI apps or banks, businesses can use a payment gateway to connect UPI payments with their website or mobile application.


A[UPI payment gateway](https://www.cashfree.com/upi-payment-gateway/) typically handles:


- Payment initiation
- UPI Intent and QR-based payment flows
- Payment authentication
- Transaction status
- Payment notifications and webhooks
- Refunds
- Reconciliation and reporting


UPI, or Unified Payments Interface, is an instant payment system developed by the National Payments Corporation of India (NPCI). It allows customers to make payments directly from their bank accounts using participating UPI applications.


UPI continues to see significant adoption in India. NPCI’s latest statistics show that UPI processed more than[23.6 billion transactions in July 2026](https://www.npci.org.in/product/upi/product-statistics) , with a transaction value of approximately ₹29.9 lakh crore.


##


What is UPI Integration?


UPI integration is the process of connecting a website, mobile app, or business payment system with UPI so customers can make payments through UPI.


Businesses can integrate[UPI directly through APIs and SDKs](https://www.cashfree.com/docs/payments/online/mobile/android) or use a payment gateway that provides multiple UPI checkout options through a single integration.


##


**Why Do You Need UPI Payment Gateway Integration?**


UPI has clearly seen spectacular adoption as a P2P payment method. It


is designed as a convenient way to pay for online purchases and[B2B payments](https://www.cashfree.com/blog/what-businesses-need-to-know-about-b2b-payments/) .


**Parameters** **UPI** **NB** **Cards**


For Customers **Payment information needed for checkout** UPI ID-similar to email and 4-digit MPIN Bank Login id and password, OTP or PIN


16-19 digit card number, CVV, Card expiry details, Cardholder name, OTP or ATM PIN


**Mobile Friendly Design** High – Mobile first design Low


Low


For Business **Settlement to a business account** 1 Day


1-2 Days


1 Day


70-95%* 70-90% 70-90%


As compared to other payment modes, UPI checkout flows are reducing the friction for customers and merchants which means a higher transaction success rate and money hitting the bank accounts faster.


##


What are the Benefits of UPI Integration?


Well, many benefits.


But here are some advantages of UPI integration that really stand out.


1. Better customer experience. UPI is the most preferred payment mode for the Indian population. Integrating UPI as a payment mode on your website/app is of utmost importance.
2. The government has mandated 0%[payment gateway charges](https://www.cashfree.com/blog/payment-gateway-charges-india-free-payment-gateway/?utm_source=blog&utm_medium=organic&utm_campaign=upi_integration_blog) . This means that UPI as a payment mode only includes platform charges.
3. The[U](https://www.cashfree.com/blog/upi-transaction-limit/?utm_source=blog&utm_medium=organic&utm_campaign=upi_integration_blog)[PI transaction limit](https://www.cashfree.com/blog/upi-transaction-limit/?utm_source=blog&utm_medium=organic&utm_campaign=upi_integration_blog) is high. The P2P limit is set at 1 lakh. However, there is no such limit of peer to merchant transactions.
4. If you choose the right PSP, you will be able to get access to fast settlement cycles and easy reconciliation for UPI payments.


##


Choosing a Payment Service Provider for UPI Integration


With all the stakeholders of the payments ecosystem – banks, payment apps and NPCI trying to innovate using UPI, there have emerged many ways to offer UPI as a payment option that is suited for different kinds of payments and platforms.


Banks,[UPI](https://www.cashfree.com/upi-payment-gateway/?utm_source=blog&utm_medium=organic&utm_campaign=upi_integration_blog)[payment gateways (like Cashfree)](http://cashfree.com/upi-payment-gateway/?utm_source=blog&utm_medium=organic&utm_campaign=upi_integration_blog) and UPI apps (like PhonePe) – all provide integration kits for merchants to start collecting payments by using UPI payment method.


### **Banks**


**Pros:**


- Marginally better scope of price negotiation since aggregators and payment apps add their margin to the bank pricing. However, in practice, pricing is quite similar since aggregators are able to negotiate volume discounts with banks and pass them on to their merchants.


- Banks, being directly connected to NPCI, provide the UPI SDK that converts your app into a UPI app. The primary advantage is that there is no redirection to a third-party app. However, It is ideally used only by businesses having only UPI transactions and those who anticipate a very high volume of payments. A good example would be UPI payment apps themselves like Google Pay and PhonePe.


**Cons:**


- Setup fee


- Higher turnaround time for integration


- Limited integration options


- Success rates dependent on server uptimes of a single bank


- Pending or Failed transfers need manual reconciliation


- Dashboard, Reporting and MIS are less


- Operational Support and troubleshooting are slow


### **UPI Payment Gateway** Integration


**Pros:**


- [Simple and quick integration](https://docs.cashfree.com/docs/upi)


- No setup fees


- Multiple integration options for web and mobile*


- Higher success rates due to routing between multiple banks*


- Automatic reconciliation of pending and failed transactions*


- Better operational support, and q


uick troubleshooting over email and chat
- Detailed MIS, reporting and user-friendly dashboard


**Cons:**


- Common merchant UPI ID shared between multiple merchants. However Cashfree merchants can apply for unique, white-labelled UPI IDs which get registered at the banks.*


> How Cashfree offers higher than industry average success rates on UPI?
>
>
> Cashfree has direct UPI integrations with multiple banks & PSP apps such as Google Pay. Our in-house transaction routing algorithms and automated reconciliation process for pending payments ensure a higher success rate than the industry standard.


### **UPI Apps like Phonepe**


**Pros:**


- Occasional offers and cashback (can be offered via aggregators as well)


**Cons:**


- Cumbersome integration can be added via an aggregator as well


- Additional step at checkout if login is needed
- Only UPI supported, hence payment operations get split across multiple service providers


While banks and aggregators both offer UPI as an option, it is better to opt for an aggregator if you want to accept online payments through UPI as well as other payment modes.


UPI still accounts for a minor share of transactions in terms of value as compared to cards, and it is easier to integrate and operate multiple payment modes via the same payment provider such as Cashfree.


##


How to Integrate UPI Payment Gateway in Website


If you want to integrate UPI payments into a website, using a payment gateway API or hosted checkout is generally the simplest approach.


##


Types of UPI Payment Gateway Integrations


### **Webflow UPI Integration**


**Platform:** Web, Mobile web, Android, iOS


This is the most popular UPI integration mode.


Let’s take an example of a customer checkout experience on Mogra Designs, a Cashfree merchant having UPI Webflow integration.


- The customer selects items and fills in details like shipping address, delivery instructions etc


- Selects UPI as a payment mode, enters his mobile app UPI ID, also called VPA (Virtual Payment Address), verifies and submits it.


- Need to check his mobile phone and do a *two-way authentication* which involves:


a) Opening the UPI app (in this case BHIM app) using the app Passcode or thumb impression.


b) Authenticating the transfer by entering UPI PIN. (UPI-PIN is a 4-6 digit secret code you create/set when you link a bank account with your UPI mobile app.)


Once the customer authenticates the payment, the transaction will be marked as successful.


**Payment Flow:**


The customer has to first enter his[VPA (or UPI ID)](https://www.cashfree.com/blog/vpa-in-upi/) , open his UPI mobile app, do 2-factor authentication on his mobile phone and then come back to the website.


**Suitability:**


Any business that wants to provide UPI as a payment mode and doesn’t mind redirecting the customer to a third-party site (UPI app providers such as Google Pay, or PhonePe). It is a relatively economical UPI integration mode.


### **UPI Google Pay Integration**


**Platform:** Web, mobile web, Android, iOS (Payer needs to have Google Pay on their phone)


This is similar to the UPI web-flow, but the payer enters his phone number instead of the UPI ID.


UPI Google Pay integration is a very convenient checkout flow as the payer/customer need not remember the UPI ID/VPA


. Cashfree provides UPI Google Pay integration.


**Payment Flow:**


The customer enters a mobile number instead of UPI VPA/UPI ID, does a 2-factor authentication (as explained before) and completes the payment.


**Suitability:**


Any small and medium business that wants to provide UPI as a payment option. This mode should be used when you see that Google Pay is a preferred UPI payment mode among customers.


##


What is UPI Intent?


UPI intent is a payment flow wherein as soon as the user chooses the UPI payment app, the app installed on his mobile launches automatically, the user doesn’t need to enter UPI VPA or phone number as it is auto-filled along with other payment details including the amount to be paid.


### **Intent Flow UPI Integration**


**Platform:** Android


**Payment Flow:**


As soon as the user chooses the UPI payment app, the app installed on his mobile launches automatically


, the user doesn’t need to enter UPI VPA or phone number as it is auto-filled along with other payment details including the amount to be paid.


**Suitability:**


[UPI Intent flow](https://docs.cashfree.com/docs/33-initiate-payment-upi-intent-flow) is a frictionless checkout experience as it automatically launches a preferred UPI mobile app during payment. It is ideal when your customers are placing an order directly on an Android app. Intent flow is offered as a part of Cashfree’s Android SDK.


### **UPI SDK Flow Integration**


**Platform:** Android, iOS


Using the UPI SDK, the merchant can receive the payment without the customer having to open any third-party app. This form of integration works only on mobiles(Android & iPhone) and is provided by banks like RBL, ICICI, Yes Bank, and Axis Bank. For this, you need to contact the bank directly and request NPCI UPI Android SDK to receive payments.


**Payment Flow:**


- **In this case, the bank will create a VPA and then you can get paid by customers on the same VPA on your mobile app. Once you have the access to UPI SDK, you need to add it to your website/mobile application to enable transactions using UPI.**
- **Here no separate UPI app such as BHIM, Google Pay etc is required. Since there is no redirection to any third-party application, using UPI SDK integration, the conversion rate increases .**


**Suitability:**


Typically big businesses having a high volume of daily inward transactions on their mobile app opt for this type of integration.


\[What’s new\] Cashfree now offers UPI SDKs. Get in touch[here](https://www.cashfree.com/contact-sales/?utm_source=blog&utm_medium=organic&utm_campaign=upi_integration_blog) !


### **Payment by QR code on the UPI app**


**Platform:** Offline, Web


#### What is a QR Code?


**[QR code](https://www.cashfree.com/blog/qr-code-payments/)** or Quick Response Code is a unique graphics code.


- **Dynamic Universal QR Codes:**


**Payment Flow:**


- A unique QR code is created for each order during checkout.
- The customer can simply open any UPI mobile app, scan the dynamic QR code created for the order and pay.


- **This QR code works across all UPI apps.**


This is similar to the UPI web flow. However, the customer can scan the QR code instead of entering their UPI ID.


This feature is provided by Cashfree. On scanning, a payment request gets initiated to the customer’s UPI app which needs to be approved. Once the customer pays, Cashfree will automatically complete the transaction and mark it as paid.


**Suitability:**


Any business is willing to provide a seamless UPI payment flow. In this case, since a dynamic QR code is generated for each transaction, tagging the payment as successful for a transaction is automated.


- **Static QR Codes:**


**Payment Flow:**


- **The payee merchant, instead of (or in addition to) sharing their UPI ID as text, converts it to a QR Code which is easy to scan and pay.**


- The merchant can directly use app-specific QR codes (for eg. Paytm as shown above)


- Interoperable QR codes by **[UPI apps](https://www.cashfree.com/blog/whats-new-set-up-custom-recurring-payments-via-any-upi-app-with-on-demand-upi-autopay/)** can also be used ( for example if you use the BHIM UPI QR code, the same works for accepting payments from Google Pay, Phone Pe and Bhim app)


**Suitability:**


Used when the merchant wants to receive payments offline.


\[Want to offer a faster checkout experience for your store? Get in touch[here](https://www.cashfree.com/contact-sales/?utm_source=blog&utm_medium=organic&utm_campaign=upi_integration_blog) !


### **Auto-Collect and Reconcile UPI Collections Through Virtual UPI IDs**


The above-mentioned UPI payment flows involve the payer visiting the payee merchant’s app or website to initiate a transaction.


However, for many regular, day-to-day payments, the payer may not visit the payee merchant’s app. It is also possible the payee merchant may not have a website or app.


Examples can be phone bill payments, school fee payments, short-term loan repayments, apartment fee maintenance etc.


While the payee merchant can simply share their UPI ID with the payer to collect payments, it becomes difficult to reconcile received payments for the payer by looking at the bank statement.


Let’s take Airtel bill payments as an example — If 1 million people were to pay bills by sending money to airtel@icicibank, Airtel would probably take a very long time to mark those bills as paid.


**[Auto-Collect as a Solution](https://www.cashfree.com/auto-e-collect?utm_source=blog&utm_medium=organic&utm_campaign=upi_integration_blog/)**


- The payee merchant creates a unique virtual UPI Id for each payer. For example, Airtel.


*\[Phone No\]* @icicibank.


- Since a payer pays into a unique UPI ID instead of everyone sending it to airtel@icicbank — the bill payment is reconciled the moment the money is received.


- Better still, UPI being a platform that is available across all payment apps today, it is possible to make such **payments from any app like Google Pay, BHIM, Paytm and even WhatsApp.**


The payer just needs to know the correct UPI ID to send money which is shared by the merchant.


**Suitability:**


- AutoCollect by creating virtual UPI IDs


is a great solution for the offline UPI payment use case such as phone bill payments, school fee payments, short-term loan repayments, apartment fee maintenance etc.


****


##


**Conclusion:**


UPI is considered an incredibly well-designed and mobile-first payment solution. Integrating UPI into your website/application offers a faster and more preferred way to pay for an increasing number of customers.


Depending on the kind of checkout experience that suits your business use case, you can opt for the appropriate mode of UPI integration. If you have questions or want to talk to our payments experts,


contact us on[https://api.whatsapp.com/send?phone=919008134748&text=Hi](https://api.whatsapp.com/send?phone=919008134748&text=Hi)


### Ready to Accept UPI Payments?


Integrate UPI payments into your website or app with Cashfree Payments. Offer UPI Intent, QR payments, and other payment options through a single integration.


[Explore UPI Payment Gateway](https://merchant.cashfree.com/merchants/signup?source-action=Blog&action=Sign%20Up&button-id=StartNow_BlogFooterCTA)


##


FAQs


#### 1. How can I ensure a high transaction success rate of UPI transaction?


Once you have completed UPI integration, this is likely the next possible question.


UPI Payment processing involves a lot of players.


Therefore, a lot of things can go wrong.


So, here are some tips that can help you ensure that your UPI transactions have a high success rate.


1. Choose a[payment aggregator](https://cashfree.com/blog/payment-aggregator/?utm_source=blog&utm_medium=organic&utm_campaign=upi_integration_blog) that offers dynamic routing based on availability and performance.
2. Inform the customer in advance if any UPI app or any bank is facing downtime
3. Ensure that your PSP validates the VPA in UPI before any UPI transactions
4. Offer dynamic QR codes as a method of payment
5. Save customer’s UPI VPA so they do not have to enter it repeatedly for future purchases
6. Include a timer on payment pages so that the customer makes the payment faster
7. Blacklist the UPI apps with extremely low success rates
8. Consider implementing an intent flow as it has a low churn rate and a higher success rate


#### 2. Is UPI payment accepted in other countries apart from India?


Yes, apart from India, **UPI is live or being integrated** in the following countries:


Country UPI Use Case


Singapore UPI linked with PayNow for real-time remittances


UAE Indians can pay via UPI at select merchants


Nepal First international country to adopt UPI system


Mauritius Cross-border UPI payments & remittances


France UPI to be accepted at Eiffel Tower and more


UK Plans in progress with local payment networks


Thailand Discussions ongoing for UPI-RTP link


#### 3. How can I integrate UPI payments into my website?


You can integrate UPI payments into a website using a UPI-enabled payment gateway or API. The typical process involves creating a merchant account, integrating the provider’s API or checkout, creating payment orders, offering a UPI flow such as Intent or QR, and verifying the final transaction status through the API or webhook.


#### 4. How do I integrate UPI payments into a mobile app?


You can integrate UPI into a mobile application using a payment gateway’s APIs, SDKs, or supported UPI Intent flow. The customer selects UPI, chooses a supported UPI app, completes authentication, and the merchant verifies the resulting transaction status before completing the order.


#### 5. What is the best way to integrate UPI payments for an online business?


For many online businesses, a UPI-enabled payment gateway is the simplest option because it can provide multiple UPI flows through a single integration. The right choice depends on your platform, transaction volume, technical requirements, and need for features such as APIs, webhooks, reconciliation, refunds, and reporting.


#### 6. Can I accept UPI payments without a website or app?


Yes. Businesses can use solutions such as UPI payment links or QR codes to collect payments without building a complete website or mobile application. The appropriate option depends on whether you need order-level tracking, fixed payment amounts, or recurring collections.


#### 7. Is UPI integration suitable for ecommerce businesses?


Yes. UPI is widely used for online payments in India and can be integrated into ecommerce checkout through flows such as UPI Intent and dynamic QR. A payment gateway can also provide transaction tracking, refunds, webhooks, and reconciliation.
