---
schema_version: "1.0.0"
document_id: "c12daabf56ab0a1c19bb0a930ee020cd1b6c980951dbfa50e76c1f217eda4540"
company_key: "yc-cashfree-payments"
company: "Cashfree Payments"
source_id: "yc-cashfree-payments-rss-98daff448d11"
canonical_url: "https://blogrevamp.cashfree.com/types-of-payment-gateway/"
published_at: "2026-08-10T12:58:17+00:00"
first_seen_at: "2026-08-10T14:50:49.831718+00:00"
fetched_at: "2026-08-10T14:50:51.331333+00:00"
content_hash: "sha256:f6d4e1da21988cb09b4ffa6633c91c17c6815ac35232a84e7581b379dd71a88f"
---

# What Are the Different Types of Payment Gateways?

Table of Contents


Toggle


**TL;DR**


Payment gateways can be classified by their provider and how payments flow during checkout. The main types include hosted, API-hosted, self-hosted, and local bank integration gateways, along with off-website options such as payment links and QR codes. The right choice depends on your payment methods, checkout control, integration effort, pricing, security, settlement, and scalability needs.


Payment gateways have become an essential part of online commerce. As businesses accept more digital payments across websites, apps, and other channels, choosing the right payment gateway can directly affect checkout experience, payment success rates, and how easily a business manages its payments.


But payment gateways are not all the same. They can differ based on who provides them and how the payment flows during checkout.


In this guide, we’ll explain the different types of payment gateways, how they work, their key differences, and what businesses should consider when choosing one.


##


What is Payment Gateway?


A **[payment gateway](https://cashfree.com/payment-gateway-india)** , often shortened to PG, is the technology that lets a business accept payments on its website or app. It securely encrypts or tokenizes payment information, communicates with the relevant payment networks and banks, and tells the customer whether the payment was successful or declined.


You can think of a payment gateway as the online equivalent of a card terminal at a shop counter. You may not see everything happening behind the scenes, but the gateway helps securely move payment information between the customer, merchant, and financial institutions whenever an online payment is made.


Payment gateways can support[multiple payment methods](https://www.cashfree.com/blog/payment-mode-types/) , including net banking, UPI, wallets, debit cards, credit cards, and prepaid cards. Depending on the provider and business requirements, they may also support different currencies and payment flows such as recurring payments, EMI, and other payment options.


Payment gateways can be classified in two primary ways:


1. **By the provider**
2. **By the payment flow during checkout**


Let’s look at both classifications in detail.


##


What does “PG” mean? – The meaning behind popular Payment Gateway terminology


PG stands for Payment Gateway. If you see “PG” on a bank statement, payment confirmation, or transaction notification, it generally means the payment was processed through a payment gateway.


Some common PG terms include:


- **PG transaction:** A transaction processed through a payment gateway.
- **PG settlement:** Transfer of collected funds from the payment gateway to the merchant’s bank account.
- **PG charges:**[Fees charged to a business for processing payments](https://www.cashfree.com/payment-gateway-charges/) , generally based on the transaction or payment method.
- **PG account:** The merchant setup or account through which payment gateway transactions are managed.


##


Types of Payment Gateways: On the Basis of Provider


ayment gateways can be classified into two types based on the provider:


1. **Third-Party Payment Gateway**
2. **Bank Payment Gateway**


### Third-Party Payment Gateway


[Third-party payment gateways](https://www.cashfree.com/blog/third-party-payment-processor/) are provided by specialised payment companies. They typically offer multiple payment methods, easier integrations, dashboards, reconciliation tools, and additional payment features.


**Key advantages include:**


- Multiple payment methods
- [APIs](https://www.cashfree.com/blog/payment-gateway-apis/) , SDKs, and plugins
- Faster integration
- [Payment reconciliation](https://www.cashfree.com/blog/reconciling-your-payment/)
- Flexible settlement options
- Customer and technical support
- Payment routing capabilities


They are generally suitable for startups, ecommerce businesses, SaaS companies, marketplaces, and growing enterprises.


### Bank Payment Gateway


Bank payment gateways are provided directly by banks. They can offer a familiar payment environment and may be suitable for businesses with specific banking relationships or domestic payment requirements.


However, payment methods, integration options, checkout experience, and features can vary by bank.


##


Differences Between Payment Gateway Types


Payment gateways can also be classified according to how the payment flows during checkout. The four core payment-flow types are Hosted, API-Hosted, Self-Hosted, and Local Bank Integration.


**Parameter** **Hosted Payment Gateway** **API-Hosted Payment Gateway** **Self-Hosted Payment Gateway** **Local Bank Integration Gateway**


Payment Flow Customer is redirected to a third-party payment page Customer stays on your page, while payment is processed through the API Customer payment-related details are collected on your site and sent to the gateway Customer is redirected to the bank’s own payment page


Integration Effort Low, quick setup Moderate, requires API integration knowledge High, requires full development expertise Moderate, depending on the bank’s API


Checkout Control Limited branding and control Medium to high, with a customisable UI Full control over UI and UX Low to medium, depending on the bank’s interface


Customer Experience Customer is redirected to another payment page Customer stays on the merchant’s page Fully controlled by the merchant Depends on the bank’s payment interface


Best Suited For Startups and small businesses Growing businesses, SaaS, marketplaces Large enterprises and fintechs Businesses focused on local or domestic payment methods


##


Types of Payment Gateway: On the Basis of Payment Flow


On the basis of payment flow, there are four core types of payment gateway:


1. **[Hosted Payment Gateway](https://www.cashfree.com/blog/hosted-payment-gateway/)**
2. **API-Hosted Payment Gateway**
3. **Self-Hosted Payment Gateway**
4. **Local Bank Integration Gateway**


Off-Website payment collection is also commonly used when businesses need to collect payments without a traditional website checkout.


Let’s look at each one in detail.


### 1. Hosted Payment Gateway


A hosted payment gateway redirects customers from the merchant’s website to a[payment page](https://www.cashfree.com/payment-pages/) managed by the payment provider. After completing the payment, the customer is redirected back to the merchant’s website.


#### Easy Integration


Hosted gateways are generally easy to integrate. The merchant sends the required order and customer information to the gateway, which handles the payment page and payment processing.


#### PCI Compliance


Since the payment page is maintained by the payment provider, merchants can reduce the burden of directly handling sensitive payment information.


#### Best Suited For


Hosted payment gateways are generally suitable for startups and small businesses looking for a quick and simple way to accept online payments.


### 2. API-Hosted Payment Gateway


An API-hosted payment gateway lets customers complete payments while staying on the merchant’s website or app. The payment flow is handled through APIs in the background. This gives businesses greater control over checkout design and branding compared with a traditional hosted gateway.


The trade-off is a more technical integration. Businesses need the appropriate development resources and must follow the provider’s security and compliance requirements.


#### Best Suited For


API-hosted gateways can be suitable for growing ecommerce businesses, SaaS companies, marketplaces, and businesses that need a customised checkout experience.


### 3. Self-Hosted Payment Gateway


In a self-hosted payment gateway, the merchant has greater control over how payment information is collected and how the checkout experience looks.


Self-hosted implementations can vary. In some setups, the payment form is provided by the gateway and embedded into the merchant’s page. In others, the merchant may have greater responsibility for handling sensitive payment information and meeting applicable[PCI DSS requirements](https://www.cashfree.com/blog/pci-dss-compliance-requirements-checklist/) .


#### Control Over User Experience


Self-hosted gateways give merchants greater control over checkout design, branding, and the overall customer journey.


#### Higher Technical Requirements


Greater control also means more development, security, testing, and compliance responsibilities compared with a hosted gateway.


#### Best Suited For


Self-hosted solutions are generally better suited to large enterprises, fintechs, and businesses with strong technical resources and customised payment requirements.


### 4. Local Bank Integration Gateway


A local bank integration gateway uses a bank’s payment infrastructure and typically redirects customers to the bank’s payment page. The integration effort and available payment methods depend on the bank.


Since the bank controls the payment interface, merchants generally have less control over the checkout experience compared with API-hosted or self-hosted solutions.


### 5. Off-Website Payment Gateway


Off-website payment collection allows businesses to accept payments without a traditional website checkout.


**Common methods include:**


#### Payment Links


Businesses can[create payment links](https://www.cashfree.com/payment-links/) and share them through email,[WhatsApp](https://www.cashfree.com/blog/indias-most-conversion-friendly-whatsapp-payment-links/) , or other channels. Customers open the link and complete the payment using an available payment method.


#### QR Codes


[QR codes](https://www.cashfree.com/upi-qr-code/) allow customers to scan and pay using supported payment methods such as UPI. A QR code can be static or dynamic. Dynamic QR codes can be generated for specific transactions or orders.


#### Tap and Pay


Tap and Pay allows customers to make eligible card payments by tapping their card or compatible device.


***Also read:[How to Accept Payments Online Without a Website or App in India](https://www.cashfree.com/blog/accept-payments-without-website-or-app/)***


##


**How to Choose the Right Payment Gateway**


For a business to succeed, having a payment gateway that processes digital payments reliably is essential. The payment gateway connects your business, customers, banks, and payment networks. It can influence the checkout experience, payment success rates, and how easily your business manages payments.


Because every business has different requirements based on its size, industry, customers, transaction volume, and growth plans, there is no single payment gateway that is best for everyone.


Here are the key factors to consider when choosing a payment gateway.


### **1. Understand Your Business Requirements**


Define your business needs to your payment provider based on your business model. Analyse the volume of transactions you expect your business to generate every day and every month, and how you deal with the high-volume payments. If your business is on a larger scale, then choosing a split payment or multi-payment settlement method might be a better option. On the other hand, if your business is a small startup, then you should ideally focus on low costs and ease of setup.


### **2. Suitable Payment Methods**


A 360-degree payment gateway provides businesses with a variety of choices to satisfy a wide range of client demands. Debit cards, credit cards, e-wallets,[UPI](https://www.cashfree.com/upi-payment-gateway/) , Scan Pay, net banking, and EMI are common payment methods in India. Selecting an international card support and multi-currency payment option is essential if you want to serve or target international clients.


Therefore, giving your consumers a variety of options makes them more accessible, and allowing them to make purchases according to their preferences results in successful transactions rather than cart abandonment.


### **3. Transaction Fees and Pricing Structure**


For any business, payment processing fees are a major factor when choosing a payment gateway. When choosing a payment partner, properly analyse the transaction fees, annual maintenance charges, setup charges, and any miscellaneous charges. So, choose a payment gateway that is true to you and disclose all pricing structures before the final agreement.


### **4. Ease of Integration and Developer Support**


The payment gateway you choose should integrate easily with your website and mobile app and fully align with your customers’ existing software. This ensures every customer’s experience is quick and effortless. Search for payment gateways that support APIs and[plugins](https://www.cashfree.com/docs/payments/plugins) , as this can save time and resources, allowing you to focus on expansion and new product launches.


### **5. Security and Compliance**


For any business, security is paramount and non-negotiable. Choose a payment gateway that ensures industry-standard security practices, such as PCI-DSS compliance, data encryption, tokenisation, and two- or three-factor authentication methods. A reliable payment gateway ensures to include assistance like fraud detection and risk monitoring that adds an extra layer of protection. In addition to shielding your company from monetary losses, a secure gateway increases consumer trust.


### **6. Payment Success Rate and Reliability**


For revenue growth, a high payment success rate is essential. Over time, even a small number of unsuccessful transactions can lead to significant losses. Analyse the transaction processing speed, server dependability, and gateway uptime. Success rates are often higher for gateways with solid bank partnerships and efficient routing. Another crucial sign of dependability is steady performance during times of high traffic or sales.


### **7. Cash Flow Management during settlement**


If the payment is reflected in the merchant’s bank account on time, it indicates the settlement is accurate, and the payment gateway is reliable. A faster settlement ratio helps maintain a healthy[cash flow for businesses](https://www.cashfree.com/blog/cash-flow-management-explained/) and indicates that a payment gateway is capable of handling high-value and high-volume transactions as well. When deciding on a payment gateway partner, compare the flexibility, time frame, and transparency offered.


### **8. Customer Experience and Checkout Flow**


Choosing a payment gateway that offers a smooth and straightforward checkout experience directly impacts conversion rates. A payment gateway should have a user-friendly interface and minimal load times. There should be features like auto-filled bank details, seamless mobile compatibility, and saved and multiple payment methods. All of these help improve customer checkout experience and also promote customer revisits.


### **9. Scalability and Future Readiness**


Think about whether the payment gateway can grow with your company. Your gateway should be able to manage increased transaction volumes, accept more payment methods, and provide sophisticated services like subscriptions, international payments, and analytics as you grow into new markets. Selecting a scalable gateway reduces the likelihood of frequent switching in the future.


### Looking for the right payment gateway?


Explore payment solutions built for different business models, payment methods, and checkout needs.


[Explore Payment Gateway](https://merchant.cashfree.com/merchants/signup?source-action=Blog&action=Sign%20Up&button-id=StartNow_BlogFooterCTA)


##


Simplify Your Payments with Cashfree


[Cashfree Payments](https://www.cashfree.com/) is built to make it simple for businesses to start accepting payments across different stages of growth.


Cashfree supports **100+ payment modes** , including UPI, credit cards, debit cards, net banking, wallets, and EMI, helping businesses give customers more ways to pay.


On pricing, Cashfree charges a **Merchant Discount Rate (MDR) per transaction** , with zero setup fees and zero annual maintenance charges. Exact rates can vary based on factors such as payment mode and transaction volume, so businesses should check the current pricing information for rates applicable to their requirements.


Beyond payment methods and pricing, Cashfree offers features designed to help businesses manage payment processing at scale.


These include:


- Multiple payment methods through a single integration
- Dynamic routing to help[improve payment success rates](https://www.cashfree.com/blog/what-is-payment-success-rate/)
- Settlement options, including instant settlement for eligible transactions
- Payment reconciliation and transaction management
- Payment links and QR-based payment collection
- APIs and integrations for websites, apps, and business systems


For businesses dealing with high transaction volumes or seasonal spikes, reliable payment infrastructure and efficient routing can be particularly important.


##


Conclusion


Payment gateways can differ based on their provider and how the payment flows during checkout.


Hosted gateways offer simple integration, while API-hosted and self-hosted gateways provide greater control over the checkout experience. Local bank integrations can suit businesses focused on domestic payments, while off-website solutions such as payment links and QR codes help businesses collect payments without a traditional checkout.


When choosing a payment gateway, compare payment methods, pricing, integration, security, payment success rates, settlement, customer experience, and scalability.


### Ready to choose a payment gateway for your business?


Accept 100+ payment modes and manage your payments with Cashfree Payments.


[Get Started with Cashfree](https://merchant.cashfree.com/merchants/signup?source-action=Blog&action=Sign%20Up&button-id=StartNow_BlogFooterCTA)


##


FAQs


#### What are the different types of payment gateways?


The mai n types of payment gateways based on payment flow are hosted, API-hosted, self-hosted, and local bank integration gateways. Off-website payment collection is another option for businesses that want to accept payments without a traditional checkout.


#### What is a hosted payment gateway?


A hosted payment gateway redirects customers to a payment page managed by the payment provider. It is generally easy to integrate and is suitable for businesses looking for a simple payment setup.


#### What is a self-hosted payment gateway?


A self-hosted payment gateway gives merchants greater control over the payment experience and checkout interface. It generally requires more technical resources and may involve additional security and compliance responsibilities.


#### What is an API-hosted payment gateway?


An API-hosted payment gateway processes payments through APIs while the customer stays on the merchant’s website or app. It provides greater checkout control than a traditional hosted gateway.


#### What is the difference between hosted and self-hosted payment gateways?


A hosted gateway redirects customers to the payment provider’s page, while a self-hosted gateway keeps the payment experience within the merchant’s environment and offers greater control over checkout.


#### Which payment gateway type is best for small businesses?


Hosted payment gateways are generally a good fit for small businesses because they offer simpler integration and require less technical effort.


#### Which payment gateway type is best for ecommerce?


The right option depends on the ecommerce business’s requirements. Hosted gateways are suitable for simpler setups, while API-hosted or self-hosted solutions can be better for businesses that need greater checkout control and customisation.


#### What is an off-website payment gateway?


Off-website payment collection allows businesses to accept payments without a traditional website checkout. Payment links and QR codes are common examples.
