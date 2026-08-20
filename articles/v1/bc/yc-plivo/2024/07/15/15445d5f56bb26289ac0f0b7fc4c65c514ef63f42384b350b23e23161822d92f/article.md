---
schema_version: "1.0.0"
document_id: "15445d5f56bb26289ac0f0b7fc4c65c514ef63f42384b350b23e23161822d92f"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/toll-fraud-prevention/"
published_at: "2024-07-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:bd3aaba7016ee72ab42c0a8e33300190ad8a9ba9a665ae2ff7424400c6328ef8"
---

# Plivo’s Guide to Toll Fraud Prevention: How to Protect Your Business

The[Global Fraud Loss Survey 2023](https://cfca.org/wp-content/uploads/2023/11/Global-Fraud-Loss-Survey-2023.pdf) found that robocall scams in North America now account for more than 50% of global losses. Yet, many businesses are unprepared to deal with robocall scams and their implications for customers.


As businesses increasingly rely on automated systems and digital communication, the opportunities for fraudsters to exploit vulnerabilities also rise.


Toll fraud not only affects customer trust but can lead to significant financial losses and damage to a company's reputation. It is crucial for businesses to understand the mechanisms of such frauds and implement robust prevention strategies to safeguard their operations and maintain customer confidence.


In this guide, we’ll dive deeper into toll fraud and explain how to take steps to protect your business.


## What is toll fraud?


Toll fraud, otherwise called international revenue sharing fraud (IRSF), occurs when an unauthorized individual gains control of a company’s phone system to transfer long-distance, international, or even premium-rate calls at the owner's expense.


Usually, fraudsters gain access to voice mail systems or private branch exchanges (PBXs) that are not adequately secured. They use this access to make call-through calls, which rack up expenses by using unreasonable foreign or premium services.


## How does toll fraud work?


Toll fraud affects landline and mobile numbers in more than 200 countries. The profitable nature of toll fraud is reflected in the emergence and re-selling of the number range, as many as 10,000 new IRSF-related areas are promoted weekly.


Here’s a step-by-step example of how toll fraud works to the detriment of a business.


**Finding weaknesses:** Fraudsters scour telecommunication systems for vulnerabilities. These weaknesses could involve poorly secured account registration processes or weak security settings.


1. **Fake account factory:** Once a weakness is identified, the fraudster creates many fake accounts using automated bots or scripts. These fake accounts can appear quite legitimate at first glance.
2. **Premium number power:** Fraudsters leverage premium-rate or premium-service numbers provided by telecom carriers. These numbers charge significantly higher rates per call or text message compared to regular numbers.
3. **Exploiting the system:** Fraudsters use the fake accounts and premium numbers to steal money using two different methods:


- **The 2FA trap:** If a service uses SMS or voice verification codes for two-factor authentication (2FA), fraudsters can exploit this. They trigger a massive number of verification code requests to be sent to their fake accounts, all routed to the premium numbers. This results in a surge of expensive texts or calls being billed.
- **Account activity abuse:** Services with free trials or account creation are vulnerable. Fraudsters exploit weak registration processes to create a multitude of fake accounts. These accounts might then be used to trigger actions that generate SMS or voice traffic to premium numbers, racking up charges for the service provider.


4. **Profit sharing scheme:** In some cases, the fraudsters might collude with a complicit telecom carrier. The carrier might share a portion of the inflated revenue generated from the premium number usage.


By generating a massive amount of fake traffic to premium numbers, fraudsters steal money, often leaving the service provider or unsuspecting user with a hefty bill.


## Who is at risk of toll fraud?


Voice over internet protocol (VoIP) users, firms using premium rate numbers, and people dealing with international communications are most vulnerable to the threat of toll fraud.


### Industries and businesses at risk


#### 1. VoIP Users


**Volume** : High


Fraudsters frequently target VoIP users. Because of its digital nature, VoIP software is more prone to manipulation than other telecommunication methods.


#### 2. Users of premium-rate numbers


**Volume** : Very high


Businesses using premium-rate phone numbers are vulnerable to toll fraud. Fake helpline numbers are designed to charge callers more than they would pay if they directly called any other number. Fraudsters redirect callers to these helplines and collect the revenue.


Examples: adult chat lines, tech support lines, interactive voting systems


#### 3. **Companies with international call facilities**


**Volume** : Medium to high


Remote or international businesses, as well as those who are in the habit of making calls overseas, fall under the high-risk category as well. Fraudsters divert these calls to the high-cost destinations of their choice to pocket a maximum of the bogus billing payments.


For example, a U.S. tech company discovers that its phone system has been hacked, with all calls being redirected to premium-rate numbers in Latvia and the Maldives. Overnight, the sudden surge in international calls reveals significant financial misuse. This anomaly allowed the breach to be detected quickly.


#### 4. **High-risk geographic locations**


**Volume** : High


Fraudsters direct calls mainly to countries that are known for their comparatively expensive rates. Cuba, Estonia, Lithuania, Somalia, and Zimbabwe are a few popular options for toll fraud.


## How to prevent toll fraud


Plivo offers robust fraud control through[Verify API](https://www.plivo.com/verify/) for businesses to shield their operations. With built-in Fraud Shield, Plivo’s tools for limiting the risk of toll fraud can significantly help your business at no additional cost. Here’s how Fraud Shield works.


### Usage triggers


Statistically, large volumes of calls or SMS messages are strong indicators of fraudulent activity. Tracking these usage triggers is the first step to detecting and preventing toll fraud.


[Plivo’s](https://console.plivo.com/accounts/login/?next=/voice/overview/) console has tools for users to review voice and SMS usage, react to unusual patterns, and initiate investigations.


**Best Practices:**


- Define usage allocations based on your business needs. For instance, if your application sends a one-time password (OTP) to a user account for two-factor authentication, you might limit the number of OTPs to one within a specific period (such as one OTP per user per 15 minutes).
- Track outgoing and incoming calls and text messaging activities for any sudden or unusual increases or changes in patterns.


### Geographic Permissions


Toll fraud can be reduced by restricting call destinations. Through the Plivo console, geographic permissions can be managed and users can specify the countries that can process their outgoing calls.


**Best Practices:**


- To manage location permissions, click on Voice > Geo Permissions from the[Plivo console](https://console.plivo.com/accounts/login/) . Find the countries you’re looking for on this list. You can narrow the list of options by checking certain geo-spatial regions or countries as needed.
- Clarify permissions that align with your business and limit direct calls to risky countries unless only when necessary.


### Custom Prefix Blacklist


A blacklist of prefixes related to high percentages and fraud helps detect attempts at unrecognized use. Plivo follows an evolving list of risky prefixes that include the rate of calls, trends detected by third-party entities, and more. These characteristics can be used to make your phone system more secure.


**Best Practices:**


- From the Plivo high-risk screen (pictured below), follow the prompts to export the latest risky prefix list.
- Include these prefixes in the system’s blacklist feature to automatically stop the number of calls when it reaches a certain threshold.


### High-Risk Permissions


By tweaking permissions in the Plivo console, you can limit access to phone calls and messages that pose a high risk. These controls include banning outbound calls to numbers for revenue sharing.


**Best Practices:**


- Publish and regularly update a list of high-risk areas from **Plivo's Voice > Geo Permissions > High-Risk Permissions** screen. The SIP filter blacklist provided by Plivo details more than a thousand of these expensive rates and higher-risk prefixes.
- Formulate rate limits to check the volume of outgoing calls and messages and avoid traffic from formidable amounts to high-risk destinations.
- Create voice verification functions and two-factor authentication to better identify real customers and block undue service use.


## Prevent toll fraud with Plivo’s Fraud Shield


Plivo's[Fraud Shield](https://www.plivo.com/verify/) protects businesses against the negative impacts of toll fraud. Multiple security layers and continuous monitoring reduce the chances of unintended use of communication services that cause losses.


### **Key features of Plivo's Fraud Shield**


- **Real-time traffic monitoring:** Plivo sends notifications in real time, allowing the detection of anomalous patterns that could indicate fraud. This feature is a decisive parameter for detecting and handling possible threats.


- **Customizable thresholds:** Call management systems allow businesses to set definite durations, intervals, and destinations for calls. Users can specify the parameters of normal activity for their particular operations. If all of the thresholds are exceeded, Plivo sends an alert.


- **Geographic and prefix restrictions:** Users can restrict calls to high-risk countries or with high-cost prefixes and edit them individually from the Plivo console.


- **Automated blocks and alerts:** Plivo can generate real-time alerts on suspicious activities and automatically take appropriate action to block an identified threat. This method effectively blocks unauthorized use while preventing overall losses.


- **Detailed reporting:** Plivo generates rich reports for businesses to study call patterns and examine whether the existing strategies to curb fraudulent activity are effective. With this detailed analysis, it may be possible to adjust settings and develop more robust security measures for the future.


[Begin your free trial today](https://console.plivo.com/accounts/request-trial/) to experience how our range of tools can safeguard your business from toll fraud.
