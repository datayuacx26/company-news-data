---
schema_version: "1.0.0"
document_id: "0f4152e55825aa8a0a9a2bbbc035383c48c2d1bbe4efa21fb48e1ef68e659aa9"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/combat-telecom-fraud/"
published_at: "2023-09-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:bd7d9acdca57359366aabe1fd19d26bc0437eb3de52bbe10f83833a55c1cc241"
---

# All about Telecom Fraud and How to Combat It

Telecommunications fraud costs billions of dollars each year — estimates put global losses from telecom fraud between $25-40 billion annually — and every business is a potential victim. That’s because every organization uses telecom services — voice calls, SMS, even website click-to-call — which means they’re targets for sophisticated criminals looking to exploit vulnerabilities and rack up unauthorized charges.


## Types of telecom fraud


What constitutes telecom fraud? There are many common types of telecom scams.


1. **SMS pumping:** This involves sending high volumes of SMS messages to premium rate numbers owned by the fraudster. They make money off the interconnect fees paid by the operator.
2. **International revenue share fraud (ISRF):** Fraudsters hack into business PBX systems or cloud services and make expensive international calls that terminate on high tariff destinations. The revenue is shared between fraudsters.
3. **SIM box or SIM farm fraud:** This uses SIM boxes with multiple prepaid SIM cards to terminate international calls as local calls, bypassing international rates.
4. **Subscription fraud:** Getting postpaid connections using fake identities to make calls and default on payments.
5. **PBX hacking:** Gaining access to enterprise phone systems to make unauthorized calls.
6. **Robocalls** : Illicit robocalls may use your numbers without your consent, harming brand reputation.


## Telecom fraud prevention


Fighting fraud takes the combined efforts of governments, carriers, cloud providers like[Plivo](https://www.plivo.com/) , and businesses. A number of tools and techniques are available from each of these entities to detect and mitigate telecom fraud.


### ✔ Government measures


At the government level, in the last couple of years we’ve seen a mandate for the use of the[STIR/SHAKEN framework](http://plivo-webflow.webflow.io/blog/voice-calls-stir-shaken) for caller ID authentication to verify originating numbers and identify spoofed calls in the US and Canada. Information from STIR/SHAKEN gives individuals more information about whether they should pick up an incoming call.


Governments can take other measures as well. In addition to making it harder to spoof phone numbers, the UK government[plans](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/1154660/Fraud_Strategy_2023.pdf) to ban cold calls on all financial products, and ban SIM farms, which criminals use as a way to bypass legitimate communications platforms to send thousands of scam texts at once. In India, officials are working on a bill to mandate that the identity of a person sending a message or calling be visible to the receiver irrespective of the platform used for communication.


### ✔ Carrier measures


At the carrier level, US carriers have mandated businesses register their brands and use 10-digit long codes ([10DLC](https://plivo-webflow.webflow.io/sms/10dlc) ) for application-to-person texting over long codes — regular 10-digit phone numbers. Similarly, toll-free numbers must be verified before being used for texting, and short codes have to meet carrier-mandated conditions when they’re set up. All of these measures are designed to mitigate unwanted robocalls.


Carriers have several other tools at their disposal.


- Real-time fraud management, in which carriers use AI and ML to analyze call patterns and identify fraud immediately through rule-based scoring.
- Fraud analytics, which uses historical data to identify fraud trends, high-risk periods, and fraud hotspots through geospatial analytics.
- Blockchain, in testing now by some operators, may provide immutable caller ID verification between networks.


‍


### ✔ Service provider measures


Cloud providers help combat fraud by offering[geo permissions](https://www.plivo.com/docs/voice/concepts/geo-permissions/) to disable call routes to countries and regions in which a business has no presence, thereby avoiding potential ISRF. Plivo, for instance, lets you filter calls to specific countries and continents, and block high-risk voice network groups.


Service providers also offer tools businesses can use to lower the risk of fraud. Two-factor authentication ([2FA](https://plivo-webflow.webflow.io/verify) ) is a critical tool for keeping unauthorized individuals out of company accounts and away from company resources.


### ✔ Customer measures


As a business you can adopt several practices on your own to counter telecom fraud:


- Work with telecom providers to implement STIR/SHAKEN caller authentication and trace back spoofed calls originating from your numbers.
- Use CAPTCHA as appropriate to deter bots from exploiting systems.
- Conduct regular dark web scans to check whether any numbers you use are being sold to scammers.
- Deploy tools to block high-risk traffic identified through telecom fraud analytics.
- Have your application server or content delivery network set rate limits by user, IP address, or device identifier, to prevent sending more than 1 message per n seconds to the same mobile number range or prefix. You can also limit call duration or the number of concurrent calls.
- Monitor customer complaints about receiving robocalls/spam from your numbers.
- Require 2FA for all account and data access. Confirm users’ email addresses and/or phone numbers before enrolling them in 2FA.
- Audit telecom invoices frequently for unusual spikes.


‍


### ✔ Individual awareness


All of those techniques can cut the risk of fraud for businesses and limit risks for their customers as well. However, individuals still have to stay alert and cautious to avoid being targeted by fraud that slips past all of the defenses. Number spoofing may be the most common scammer technique, in which fraudsters mimic legitimate numbers to socially engineer victims. Advanced[telecom analytics solutions](https://intellias.com/big-data-analytics-telecom/) can help detect unusual calling patterns associated with these scams, providing an additional layer of protection.Another is Wangiri fraud, where scammers call phone numbers and hang up after one ring to bait recipients into calling back premium rate numbers. The more cautious your customers are, the less likely they are to become victims of telecom fraud.


## Stay safe out there


Telecom fraud has been around for about as long as the telephone itself. As calling technology changes and evolves, so do scammers’ techniques. Businesses need to take advantage of layered fraud prevention provided by governments and carriers and continuously refine their own anti-fraud practices. With proactive participation from the business side, carriers can often trace, flag, and block fraudulent telecom activity being conducted over your platforms and numbers.


Plivo is committed to complying with all national and carrier regulations and best practices reducing telecom fraud. Our goal is to help you keep your customers safe from phone scams while controlling unauthorized telecom charges.
